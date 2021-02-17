---
ms.openlocfilehash: 69d77c19cb2c589086df2b4bf19eeadf41aad58a
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274394"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, habilitará el inicio de sesión único [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins) del complemento de Office en el complemento y ampliará la API web para que admita el flujo en nombre [de.](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow) Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph.

## <a name="overview"></a>Información general

El SSO del complemento de Office proporciona un token de acceso, pero ese token solo permite al complemento llamar a su propia API web. No habilita el acceso directo a Microsoft Graph. El proceso funciona de la siguiente manera.

1. El complemento obtiene un token llamando a [getAccessToken](https://docs.microsoft.com/javascript/api/office-runtime/officeruntime.auth?view=common-js#getaccesstoken-options-). La audiencia de este token (la notificación) es el identificador de aplicación del registro de la aplicación `aud` del complemento.
1. El complemento envía este token en el `Authorization` encabezado cuando realiza una llamada a la API web.
1. La API web valida el token y, a continuación, usa el flujo en nombre de para intercambiar este token por un token de Microsoft Graph. La audiencia de este nuevo token es `https://graph.microsoft.com` .
1. La API web usa el nuevo token para realizar llamadas a Microsoft Graph y devuelve los resultados al complemento.

## <a name="configure-the-solution"></a>Configurar la solución

1. Abra **./.env y** actualice el id. de aplicación y el secreto de cliente `AZURE_APP_ID` del registro de la `AZURE_CLIENT_SECRET` aplicación.

    > [!IMPORTANT]
    > Si usas el control de código fuente como Git, ahora sería un buen momento para excluir el archivo **.env** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación y el secreto de cliente.

1. Abra **./manifest/manifest.xml** y reemplace todas las instancias con el `YOUR_APP_ID_HERE` identificador de aplicación del registro de la aplicación.

1. Cree un nuevo archivo en el directorio **./src/addin** denominado **config.js** y agregue el siguiente código, reemplazando con el identificador de aplicación del registro `YOUR_APP_ID_HERE` de la aplicación.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/config.example.js":::

## <a name="implement-sign-in"></a>Implementar el inicio de sesión

1. Abra **./src/api/auth.ts** y agregue las siguientes `import` instrucciones en la parte superior del archivo.

    ```typescript
    import jwt, { SigningKeyCallback, JwtHeader } from 'jsonwebtoken';
    import jwksClient from 'jwks-rsa';
    import * as msal from '@azure/msal-node';
    ```

1. Agregue el siguiente código después de `import` las instrucciones.

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="TokenExchangeSnippet":::

    Este código inicializa un cliente confidencial de [MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md)y exporta una función para obtener un token de Graph del token enviado por el complemento.

1. Agregue el siguiente código antes de la `export default authRouter;` línea.

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="GetAuthStatusSnippet":::

    Este código implementa una API ( ) que comprueba si el token de complemento se puede intercambiar silenciosamente `GET /auth/status` por un token de Graph. El complemento usará esta API para determinar si necesita presentar un inicio de sesión interactivo al usuario.

1. Abra **./src/addin/taskpane.js** y agregue el siguiente código al archivo.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="AuthUiSnippet":::

    Este código agrega funciones para actualizar la interfaz de usuario y para usar la API de cuadros de diálogo de [Office](https://docs.microsoft.com/office/dev/add-ins/develop/dialog-api-in-office-add-ins) para iniciar un flujo de autenticación interactivo.

1. Agregue la siguiente función para implementar una interfaz de usuario principal temporal.

    ```javascript
    function showMainUi() {
      $('.container').empty();
      $('<p/>', {
        class: 'ms-fontSize-24 ms-fontWeight-bold',
        text: 'Authenticated!'
      }).appendTo('.container');
    }
    ```

1. Reemplace la llamada `Office.onReady` existente por lo siguiente.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="OfficeReadySnippet":::

    Ten en cuenta lo que hace este código.

    - Cuando el panel de tareas se carga por primera vez, llama para obtener un token con ámbito para la `getAccessToken` API web del complemento.
    - Usa ese token para llamar a la API para comprobar si el usuario ha dado su consentimiento todavía a `/auth/status` los ámbitos de Microsoft Graph.
        - Si el usuario no ha dado su consentimiento, usa una ventana emergente para obtener el consentimiento del usuario a través de un inicio de sesión interactivo.
        - Si el usuario ha dado su consentimiento, carga la interfaz de usuario principal.

### <a name="getting-user-consent"></a>Obtener el consentimiento del usuario

Aunque el complemento usa SSO, el usuario todavía tiene que dar su consentimiento para que el complemento acceda a sus datos a través de Microsoft Graph. Obtener el consentimiento es un proceso único. Una vez que el usuario ha concedido el consentimiento, el token SSO se puede intercambiar por un token de Graph sin ninguna interacción del usuario. En esta sección, implementará la experiencia de consentimiento en el complemento con [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser).

1. Cree un nuevo archivo en el directorio **./src/addin** denominado **consent.js** y agregue el siguiente código.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/consent.js" id="ConsentJsSnippet":::

    Este código inicia sesión para el usuario y solicita el conjunto de permisos de Microsoft Graph configurados en el registro de la aplicación.

1. Cree un nuevo archivo en el directorio **./src/addin** denominado **consent.html** y agregue el siguiente código.

    :::code language="html" source="../demo/graph-tutorial/src/addin/consent.html" id="ConsentHtmlSnippet":::

    Este código implementa una página HTML básica para cargar el **consent.js** archivo. Esta página se cargará en un cuadro de diálogo emergente.

1. Guarde todos los cambios y reinicie el servidor.

1. Vuelva a cargar **elmanifest.xml** archivo con los mismos pasos de carga lateral del complemento [en Excel.](02-create-app.md#side-load-the-add-in-in-excel)

1. Seleccione el **botón Importar calendario** en la pestaña **Inicio** para abrir el panel de tareas.

1. Seleccione el **botón Conceder permiso** en el panel de tareas para iniciar el cuadro de diálogo de consentimiento en una ventana emergente. Inicie sesión y conceda su consentimiento.

1. El panel de tareas se actualiza con un "Autenticado". Mensaje. Puede comprobar los tokens de la siguiente manera.

    - En las herramientas para desarrolladores del brower, el token de API se muestra en la consola.
    - En la CLI donde ejecuta el servidor Node.js, se imprime el token de Graph.

    Puede comparar estos tokens en [https://jwt.ms](https://jwt.ms) . Observe que la audiencia del token de API ( ) se establece en el identificador de aplicación del registro de la aplicación y el ámbito `aud` ( `scp` ) es `access_as_user` .
