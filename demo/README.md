---
ms.openlocfilehash: fed56663591ac36e4defcae3a8d0a222f86e3026
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274369"
---
# <a name="how-to-run-the-completed-project"></a>Cómo ejecutar el proyecto completado

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:

- [Node.js](https://nodejs.org) [y Descuido](https://yarnpkg.com/) instalados en el equipo de desarrollo. (**Nota: Este** tutorial se escribió con Node versión 14.15.0 y La versión 1.22.0 de Lana. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado).
- Ya sea una cuenta personal de Microsoft con un buzón en Outlook.com o una cuenta de Microsoft para trabajo o escuela.

Si no tienes una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede registrarse [para obtener una nueva cuenta personal de Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Puede registrarse en el Programa de desarrolladores de [Office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a>Registrar una aplicación web con el Centro de administración de Azure Active Directory

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Captura de pantalla de los registros de aplicaciones ](/tutorial/images/app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación**, establezca los valores siguientes.

    - Establezca **Nombre** como `Office Add-in Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - En **URI de redirección**, establezca la primera lista desplegable en `Single-page application (SPA)` y establezca el valor `https://localhost:3000/consent.html`.

    ![Captura de pantalla de la página Registrar una aplicación](/tutorial/images/register-an-app.png)

1. Seleccione **Registrar**. En la **página Tutorial de Office Add-in Graph,** copie el valor del id. de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de aplicación del nuevo registro de la aplicación](/tutorial/images/application-id.png)

1. Seleccione **Certificados y secretos** en **Administrar**. Seleccione el botón **Nuevo secreto de cliente**. Escriba un valor en **Descripción,** seleccione una de las opciones para **Expira y** seleccione **Agregar**.

1. Copie el valor del secreto de cliente antes de salir de esta página. Lo necesitará en el siguiente paso.

    > [!IMPORTANT]
    > El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.

1. Seleccione **permisos de API en** **Administrar** y, a continuación, seleccione Agregar **un permiso.**

1. Seleccione **Microsoft Graph** y, a continuación, permisos **delegados.**

1. Seleccione los permisos siguientes y, a **continuación, seleccione Agregar permisos.**

    - **Calendars.ReadWrite:** esto permitirá que la aplicación lea y escriba en el calendario del usuario.
    - **MailboxSettings.Read:** esto permitirá que la aplicación obtenga la zona horaria del usuario desde su configuración de buzón.

    ![Captura de pantalla de los permisos configurados](/tutorial/images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a>Configurar el inicio de sesión único del complemento de Office

Actualice el registro de la aplicación para admitir el inicio de sesión [único (SSO) del complemento de Office.](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)

1. Seleccione **Exponer una API.** En la **sección Ámbitos definidos por esta API,** seleccione **Agregar un ámbito.** Cuando se le pida que establezca un **URI de id.** de aplicación, establezca el valor en `api://localhost:3000/YOUR_APP_ID_HERE` , reemplazando con el identificador de `YOUR_APP_ID_HERE` aplicación. Elija **Guardar y continuar.**

1. Rellene los campos como se muestra a continuación y seleccione **Agregar ámbito.**

    - **Nombre del ámbito:**`access_as_user`
    - **¿Quién puede dar su consentimiento?: Administradores y usuarios**
    - **Nombre para mostrar del consentimiento del administrador:**`Access the app as the user`
    - **Descripción del consentimiento del administrador:**`Allows Office Add-ins to call the app's web APIs as the current user.`
    - **Nombre para mostrar del consentimiento del usuario:**`Access the app as you`
    - **Descripción del consentimiento del usuario:**`Allows Office Add-ins to call the app's web APIs as you.`
    - **Estado: habilitado**

    ![Captura de pantalla del formulario Agregar un ámbito](/tutorial/images/add-scope.png)

1. En la **sección Aplicaciones cliente autorizadas,** seleccione Agregar una **aplicación cliente.** Escriba un identificador de cliente en la siguiente lista, habilite el ámbito en **Ámbitos autorizados** y seleccione **Agregar aplicación.** Repita este proceso para cada uno de los IDs de cliente de la lista.

    - `d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)
    - `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)
    - `57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office en la Web)
    - `08e18876-6177-487e-b8b5-cf950c1e598c` (Office en la Web)

## <a name="install-development-certificates"></a>Instalar certificados de desarrollo

1. Ejecute el siguiente comando para generar e instalar certificados de desarrollo para el complemento.

    ```Shell
    npx office-addin-dev-certs install
    ```

    Si se le solicita confirmación, confirme las acciones. Una vez completado el comando, verá un resultado similar al siguiente.

    ```Shell
    You now have trusted access to https://localhost.
    Certificate: <path>\localhost.crt
    Key: <path>\localhost.key
    ```

1. Copie las rutas de acceso a localhost.crt y localhost.key, las necesitará en el paso siguiente.

## <a name="update-the-manifest"></a>Actualizar el manifiesto

1. Abra el **manifest.xml** archivo y realice los siguientes cambios.
    1. Reemplazar `NEW_GUID_HERE` con un nuevo GUID, como `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` .
    1. Reemplaza todas las instancias por `YOUR_APP_ID_HERE` el identificador de aplicación del registro de la aplicación.

## <a name="configure-the-sample"></a>Configuración del ejemplo

1. Cambie el nombre `example.env` del archivo a `.env` .
1. Edite `.env` el archivo y realice los siguientes cambios.
    1. Reemplace `YOUR_APP_ID_HERE` por el **id. de** aplicación que obtuvo del Portal de registro de aplicaciones.
    1. Reemplace `YOUR_CLIENT_SECRET_HERE` por el secreto de cliente que obtuvo del Portal de registro de aplicaciones.
    1. Reemplace `PATH_TO_LOCALHOST.CRT` por la ruta de acceso al archivo localhost.crt desde el resultado del `npx office-addin-dev-certs install` comando.
    1. Reemplace `PATH_TO_LOCALHOST.KEY` por la ruta de acceso al archivo localhost.key desde el resultado del `npx office-addin-dev-certs install` comando.

1. Cambie el nombre `config.example.js` del archivo a `config.js` .
1. Edite `config.js` el archivo y realice los siguientes cambios.
    1. Reemplace `YOUR_APP_ID_HERE` por el **id. de** aplicación que obtuvo del Portal de registro de aplicaciones.
1. En la interfaz de línea de comandos (CLI), vaya a este directorio y ejecute el siguiente comando para instalar los requisitos.

    ```Shell
    yarn install
    ```

## <a name="run-the-sample"></a>Ejecutar el ejemplo

1. Ejecute el siguiente comando en la CLI para iniciar la aplicación.

    ```Shell
    yarn start
    ```

1. En el explorador, vaya a [Office.com](https://www.office.com/) e inicie sesión. Seleccione **Crear en la** barra de herramientas izquierda y, a continuación, seleccione Hoja de **cálculo.**

    ![Captura de pantalla del menú Crear en Office.com](/tutorial/images/office-select-excel.png)

1. Seleccione la **pestaña** Insertar y, a continuación, **seleccione Complementos de Office.**

1. Seleccione **Cargar mi complemento y,** a continuación, seleccione **Examinar.** Cargue el **manifest.xml** archivo.

1. Seleccione el **botón Importar calendario** en la pestaña **Inicio** para abrir el panel de tareas.

    ![Captura de pantalla del botón Importar calendario en la pestaña Inicio](/tutorial/images/get-started.png)
