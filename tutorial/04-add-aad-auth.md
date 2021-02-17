---
ms.openlocfilehash: 69d77c19cb2c589086df2b4bf19eeadf41aad58a
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274394"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="28a8c-101">En este ejercicio, habilitará el inicio de sesión único [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins) del complemento de Office en el complemento y ampliará la API web para que admita el flujo en nombre [de.](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow)</span><span class="sxs-lookup"><span data-stu-id="28a8c-101">In this exercise you will enable [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins) in the add-in, and extend the web API to support [on-behalf-of flow](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow).</span></span> <span data-ttu-id="28a8c-102">Esto es necesario para obtener el token de acceso OAuth necesario para llamar a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span>

## <a name="overview"></a><span data-ttu-id="28a8c-103">Información general</span><span class="sxs-lookup"><span data-stu-id="28a8c-103">Overview</span></span>

<span data-ttu-id="28a8c-104">El SSO del complemento de Office proporciona un token de acceso, pero ese token solo permite al complemento llamar a su propia API web.</span><span class="sxs-lookup"><span data-stu-id="28a8c-104">Office Add-in SSO provides an access token, but that token is only enables the add-in to call it's own web API.</span></span> <span data-ttu-id="28a8c-105">No habilita el acceso directo a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-105">It does not enable direct access to the Microsoft Graph.</span></span> <span data-ttu-id="28a8c-106">El proceso funciona de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="28a8c-106">The process works as follows.</span></span>

1. <span data-ttu-id="28a8c-107">El complemento obtiene un token llamando a [getAccessToken](https://docs.microsoft.com/javascript/api/office-runtime/officeruntime.auth?view=common-js#getaccesstoken-options-).</span><span class="sxs-lookup"><span data-stu-id="28a8c-107">The add-in gets a token by calling [getAccessToken](https://docs.microsoft.com/javascript/api/office-runtime/officeruntime.auth?view=common-js#getaccesstoken-options-).</span></span> <span data-ttu-id="28a8c-108">La audiencia de este token (la notificación) es el identificador de aplicación del registro de la aplicación `aud` del complemento.</span><span class="sxs-lookup"><span data-stu-id="28a8c-108">This token's audience (the `aud` claim) is the application ID of the add-in's app registration.</span></span>
1. <span data-ttu-id="28a8c-109">El complemento envía este token en el `Authorization` encabezado cuando realiza una llamada a la API web.</span><span class="sxs-lookup"><span data-stu-id="28a8c-109">The add-in sends this token in the `Authorization` header when it makes a call to the web API.</span></span>
1. <span data-ttu-id="28a8c-110">La API web valida el token y, a continuación, usa el flujo en nombre de para intercambiar este token por un token de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-110">The web API validates the token, then uses the on-behalf-of flow to exchange this token for a Microsoft Graph token.</span></span> <span data-ttu-id="28a8c-111">La audiencia de este nuevo token es `https://graph.microsoft.com` .</span><span class="sxs-lookup"><span data-stu-id="28a8c-111">This new token's audience is `https://graph.microsoft.com`.</span></span>
1. <span data-ttu-id="28a8c-112">La API web usa el nuevo token para realizar llamadas a Microsoft Graph y devuelve los resultados al complemento.</span><span class="sxs-lookup"><span data-stu-id="28a8c-112">The web API uses the new token to make calls to the Microsoft Graph, and returns the results back to the add-in.</span></span>

## <a name="configure-the-solution"></a><span data-ttu-id="28a8c-113">Configurar la solución</span><span class="sxs-lookup"><span data-stu-id="28a8c-113">Configure the solution</span></span>

1. <span data-ttu-id="28a8c-114">Abra **./.env y** actualice el id. de aplicación y el secreto de cliente `AZURE_APP_ID` del registro de la `AZURE_CLIENT_SECRET` aplicación.</span><span class="sxs-lookup"><span data-stu-id="28a8c-114">Open **./.env** and update the `AZURE_APP_ID` and `AZURE_CLIENT_SECRET` with the application ID and client secret from your app registration.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="28a8c-115">Si usas el control de código fuente como Git, ahora sería un buen momento para excluir el archivo **.env** del control de código fuente para evitar la pérdida involuntaria del identificador de la aplicación y el secreto de cliente.</span><span class="sxs-lookup"><span data-stu-id="28a8c-115">If you're using source control such as git, now would be a good time to exclude the **.env** file from source control to avoid inadvertently leaking your app ID and client secret.</span></span>

1. <span data-ttu-id="28a8c-116">Abra **./manifest/manifest.xml** y reemplace todas las instancias con el `YOUR_APP_ID_HERE` identificador de aplicación del registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28a8c-116">Open **./manifest/manifest.xml** and replace all instances of `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

1. <span data-ttu-id="28a8c-117">Cree un nuevo archivo en el directorio **./src/addin** denominado **config.js** y agregue el siguiente código, reemplazando con el identificador de aplicación del registro `YOUR_APP_ID_HERE` de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28a8c-117">Create a new file in the **./src/addin** directory named **config.js** and add the following code, replacing `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/config.example.js":::

## <a name="implement-sign-in"></a><span data-ttu-id="28a8c-118">Implementar el inicio de sesión</span><span class="sxs-lookup"><span data-stu-id="28a8c-118">Implement sign-in</span></span>

1. <span data-ttu-id="28a8c-119">Abra **./src/api/auth.ts** y agregue las siguientes `import` instrucciones en la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="28a8c-119">Open **./src/api/auth.ts** and add the following `import` statements at the top of the file.</span></span>

    ```typescript
    import jwt, { SigningKeyCallback, JwtHeader } from 'jsonwebtoken';
    import jwksClient from 'jwks-rsa';
    import * as msal from '@azure/msal-node';
    ```

1. <span data-ttu-id="28a8c-120">Agregue el siguiente código después de `import` las instrucciones.</span><span class="sxs-lookup"><span data-stu-id="28a8c-120">Add the following code after the `import` statements.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="TokenExchangeSnippet":::

    <span data-ttu-id="28a8c-121">Este código inicializa un cliente confidencial de [MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md)y exporta una función para obtener un token de Graph del token enviado por el complemento.</span><span class="sxs-lookup"><span data-stu-id="28a8c-121">This code [initializes an MSAL confidential client](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-node/docs/initialize-confidential-client-application.md), and exports a function to get a Graph token from the token sent by the add-in.</span></span>

1. <span data-ttu-id="28a8c-122">Agregue el siguiente código antes de la `export default authRouter;` línea.</span><span class="sxs-lookup"><span data-stu-id="28a8c-122">Add the following code before the `export default authRouter;` line.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/auth.ts" id="GetAuthStatusSnippet":::

    <span data-ttu-id="28a8c-123">Este código implementa una API ( ) que comprueba si el token de complemento se puede intercambiar silenciosamente `GET /auth/status` por un token de Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-123">This code implements an API (`GET /auth/status`) that checks if the add-in token can be silently exchanged for a Graph token.</span></span> <span data-ttu-id="28a8c-124">El complemento usará esta API para determinar si necesita presentar un inicio de sesión interactivo al usuario.</span><span class="sxs-lookup"><span data-stu-id="28a8c-124">The add-in will use this API to determine if it needs to present an interactive login to the user.</span></span>

1. <span data-ttu-id="28a8c-125">Abra **./src/addin/taskpane.js** y agregue el siguiente código al archivo.</span><span class="sxs-lookup"><span data-stu-id="28a8c-125">Open **./src/addin/taskpane.js** and add the following code to the file.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="AuthUiSnippet":::

    <span data-ttu-id="28a8c-126">Este código agrega funciones para actualizar la interfaz de usuario y para usar la API de cuadros de diálogo de [Office](https://docs.microsoft.com/office/dev/add-ins/develop/dialog-api-in-office-add-ins) para iniciar un flujo de autenticación interactivo.</span><span class="sxs-lookup"><span data-stu-id="28a8c-126">This code adds functions to update the UI, and to use the [Office Dialog API](https://docs.microsoft.com/office/dev/add-ins/develop/dialog-api-in-office-add-ins) to initiate an interactive authentication flow.</span></span>

1. <span data-ttu-id="28a8c-127">Agregue la siguiente función para implementar una interfaz de usuario principal temporal.</span><span class="sxs-lookup"><span data-stu-id="28a8c-127">Add the following function to implement a temporary main UI.</span></span>

    ```javascript
    function showMainUi() {
      $('.container').empty();
      $('<p/>', {
        class: 'ms-fontSize-24 ms-fontWeight-bold',
        text: 'Authenticated!'
      }).appendTo('.container');
    }
    ```

1. <span data-ttu-id="28a8c-128">Reemplace la llamada `Office.onReady` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="28a8c-128">Replace the existing `Office.onReady` call with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="OfficeReadySnippet":::

    <span data-ttu-id="28a8c-129">Ten en cuenta lo que hace este código.</span><span class="sxs-lookup"><span data-stu-id="28a8c-129">Consider what this code does.</span></span>

    - <span data-ttu-id="28a8c-130">Cuando el panel de tareas se carga por primera vez, llama para obtener un token con ámbito para la `getAccessToken` API web del complemento.</span><span class="sxs-lookup"><span data-stu-id="28a8c-130">When the task pane first loads, it calls `getAccessToken` to get a token scoped for the add-in's web API.</span></span>
    - <span data-ttu-id="28a8c-131">Usa ese token para llamar a la API para comprobar si el usuario ha dado su consentimiento todavía a `/auth/status` los ámbitos de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-131">It uses that token to call the `/auth/status` API to check if the user has given consent to the Microsoft Graph scopes yet.</span></span>
        - <span data-ttu-id="28a8c-132">Si el usuario no ha dado su consentimiento, usa una ventana emergente para obtener el consentimiento del usuario a través de un inicio de sesión interactivo.</span><span class="sxs-lookup"><span data-stu-id="28a8c-132">If the user has not consented, it uses a pop-up window to get the user's consent through an interactive login.</span></span>
        - <span data-ttu-id="28a8c-133">Si el usuario ha dado su consentimiento, carga la interfaz de usuario principal.</span><span class="sxs-lookup"><span data-stu-id="28a8c-133">If the user has consented, it loads the main UI.</span></span>

### <a name="getting-user-consent"></a><span data-ttu-id="28a8c-134">Obtener el consentimiento del usuario</span><span class="sxs-lookup"><span data-stu-id="28a8c-134">Getting user consent</span></span>

<span data-ttu-id="28a8c-135">Aunque el complemento usa SSO, el usuario todavía tiene que dar su consentimiento para que el complemento acceda a sus datos a través de Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-135">Even though the add-in is using SSO, the user still has to consent to the add-in accessing their data via Microsoft Graph.</span></span> <span data-ttu-id="28a8c-136">Obtener el consentimiento es un proceso único.</span><span class="sxs-lookup"><span data-stu-id="28a8c-136">Getting consent is a one-time process.</span></span> <span data-ttu-id="28a8c-137">Una vez que el usuario ha concedido el consentimiento, el token SSO se puede intercambiar por un token de Graph sin ninguna interacción del usuario.</span><span class="sxs-lookup"><span data-stu-id="28a8c-137">Once the user has granted consent, the SSO token can be exchanged for a Graph token without any user interaction.</span></span> <span data-ttu-id="28a8c-138">En esta sección, implementará la experiencia de consentimiento en el complemento con [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser).</span><span class="sxs-lookup"><span data-stu-id="28a8c-138">In this section you'll implement the consent experience in the add-in using [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser).</span></span>

1. <span data-ttu-id="28a8c-139">Cree un nuevo archivo en el directorio **./src/addin** denominado **consent.js** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="28a8c-139">Create a new file in the **./src/addin** directory named **consent.js** and add the following code.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/consent.js" id="ConsentJsSnippet":::

    <span data-ttu-id="28a8c-140">Este código inicia sesión para el usuario y solicita el conjunto de permisos de Microsoft Graph configurados en el registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="28a8c-140">This code does login for the user, requesting the set of Microsoft Graph permissions that are configured on the app registration.</span></span>

1. <span data-ttu-id="28a8c-141">Cree un nuevo archivo en el directorio **./src/addin** denominado **consent.html** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="28a8c-141">Create a new file in the **./src/addin** directory named **consent.html** and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/addin/consent.html" id="ConsentHtmlSnippet":::

    <span data-ttu-id="28a8c-142">Este código implementa una página HTML básica para cargar el **consent.js** archivo.</span><span class="sxs-lookup"><span data-stu-id="28a8c-142">This code implements a basic HTML page to load the **consent.js** file.</span></span> <span data-ttu-id="28a8c-143">Esta página se cargará en un cuadro de diálogo emergente.</span><span class="sxs-lookup"><span data-stu-id="28a8c-143">This page will be loaded in a pop-up dialog.</span></span>

1. <span data-ttu-id="28a8c-144">Guarde todos los cambios y reinicie el servidor.</span><span class="sxs-lookup"><span data-stu-id="28a8c-144">Save all of your changes and restart the server.</span></span>

1. <span data-ttu-id="28a8c-145">Vuelva a cargar **elmanifest.xml** archivo con los mismos pasos de carga lateral del complemento [en Excel.](02-create-app.md#side-load-the-add-in-in-excel)</span><span class="sxs-lookup"><span data-stu-id="28a8c-145">Re-upload your **manifest.xml** file using the same steps in [Side-load the add-in in Excel](02-create-app.md#side-load-the-add-in-in-excel).</span></span>

1. <span data-ttu-id="28a8c-146">Seleccione el **botón Importar calendario** en la pestaña **Inicio** para abrir el panel de tareas.</span><span class="sxs-lookup"><span data-stu-id="28a8c-146">Select the **Import Calendar** button on the **Home** tab to open the task pane.</span></span>

1. <span data-ttu-id="28a8c-147">Seleccione el **botón Conceder permiso** en el panel de tareas para iniciar el cuadro de diálogo de consentimiento en una ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="28a8c-147">Select the **Give permission** button in the task pane to launch the consent dialog in a pop-up window.</span></span> <span data-ttu-id="28a8c-148">Inicie sesión y conceda su consentimiento.</span><span class="sxs-lookup"><span data-stu-id="28a8c-148">Sign in and grant consent.</span></span>

1. <span data-ttu-id="28a8c-149">El panel de tareas se actualiza con un "Autenticado".</span><span class="sxs-lookup"><span data-stu-id="28a8c-149">The task pane updates with an "Authenticated!"</span></span> <span data-ttu-id="28a8c-150">Mensaje.</span><span class="sxs-lookup"><span data-stu-id="28a8c-150">message.</span></span> <span data-ttu-id="28a8c-151">Puede comprobar los tokens de la siguiente manera.</span><span class="sxs-lookup"><span data-stu-id="28a8c-151">You can check the tokens as follows.</span></span>

    - <span data-ttu-id="28a8c-152">En las herramientas para desarrolladores del brower, el token de API se muestra en la consola.</span><span class="sxs-lookup"><span data-stu-id="28a8c-152">In your brower's developer tools, the API token is shown in the Console.</span></span>
    - <span data-ttu-id="28a8c-153">En la CLI donde ejecuta el servidor Node.js, se imprime el token de Graph.</span><span class="sxs-lookup"><span data-stu-id="28a8c-153">In your CLI where you are running the Node.js server, the Graph token is printed.</span></span>

    <span data-ttu-id="28a8c-154">Puede comparar estos tokens en [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="28a8c-154">You can compare these token at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="28a8c-155">Observe que la audiencia del token de API ( ) se establece en el identificador de aplicación del registro de la aplicación y el ámbito `aud` ( `scp` ) es `access_as_user` .</span><span class="sxs-lookup"><span data-stu-id="28a8c-155">Notice that the API token's audience (`aud`) is set to the application ID of your app registration, and the scope (`scp`) is `access_as_user`.</span></span>
