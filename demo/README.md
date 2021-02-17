---
ms.openlocfilehash: fed56663591ac36e4defcae3a8d0a222f86e3026
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274369"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="6b3fa-101">Cómo ejecutar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="6b3fa-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b3fa-102">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="6b3fa-102">Prerequisites</span></span>

<span data-ttu-id="6b3fa-103">Para ejecutar el proyecto completado en esta carpeta, necesita lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="6b3fa-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="6b3fa-104">[Node.js](https://nodejs.org) [y Descuido](https://yarnpkg.com/) instalados en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-104">[Node.js](https://nodejs.org) and [Yarn](https://yarnpkg.com/) installed on your development machine.</span></span> <span data-ttu-id="6b3fa-105">(**Nota: Este** tutorial se escribió con Node versión 14.15.0 y La versión 1.22.0 de Lana.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-105">(**Note:** This tutorial was written with Node version 14.15.0 and Yarn version 1.22.0.</span></span> <span data-ttu-id="6b3fa-106">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado).</span><span class="sxs-lookup"><span data-stu-id="6b3fa-106">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="6b3fa-107">Ya sea una cuenta personal de Microsoft con un buzón en Outlook.com o una cuenta de Microsoft para trabajo o escuela.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-107">Either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span>

<span data-ttu-id="6b3fa-108">Si no tienes una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="6b3fa-108">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="6b3fa-109">Puede registrarse [para obtener una nueva cuenta personal de Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="6b3fa-109">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="6b3fa-110">Puede registrarse en el Programa de desarrolladores de [Office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-110">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="6b3fa-111">Registrar una aplicación web con el Centro de administración de Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="6b3fa-111">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="6b3fa-112">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6b3fa-112">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="6b3fa-113">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-113">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="6b3fa-114">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-114">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="6b3fa-115">Captura de pantalla de los registros de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="6b3fa-115">A screenshot of the App registrations</span></span> ](/tutorial/images/app-registrations.png)

1. <span data-ttu-id="6b3fa-116">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-116">Select **New registration**.</span></span> <span data-ttu-id="6b3fa-117">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-117">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="6b3fa-118">Establezca **Nombre** como `Office Add-in Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-118">Set **Name** to `Office Add-in Graph Tutorial`.</span></span>
    - <span data-ttu-id="6b3fa-119">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-119">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="6b3fa-120">En **URI de redirección**, establezca la primera lista desplegable en `Single-page application (SPA)` y establezca el valor `https://localhost:3000/consent.html`.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-120">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `https://localhost:3000/consent.html`.</span></span>

    ![Captura de pantalla de la página Registrar una aplicación](/tutorial/images/register-an-app.png)

1. <span data-ttu-id="6b3fa-122">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-122">Select **Register**.</span></span> <span data-ttu-id="6b3fa-123">En la **página Tutorial de Office Add-in Graph,** copie el valor del id. de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-123">On the **Office Add-in Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de aplicación del nuevo registro de la aplicación](/tutorial/images/application-id.png)

1. <span data-ttu-id="6b3fa-125">Seleccione **Certificados y secretos** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-125">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="6b3fa-126">Seleccione el botón **Nuevo secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-126">Select the **New client secret** button.</span></span> <span data-ttu-id="6b3fa-127">Escriba un valor en **Descripción,** seleccione una de las opciones para **Expira y** seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-127">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="6b3fa-128">Copie el valor del secreto de cliente antes de salir de esta página.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-128">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="6b3fa-129">Lo necesitará en el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-129">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6b3fa-130">El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-130">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="6b3fa-131">Seleccione **permisos de API en** **Administrar** y, a continuación, seleccione Agregar **un permiso.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-131">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="6b3fa-132">Seleccione **Microsoft Graph** y, a continuación, permisos **delegados.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-132">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="6b3fa-133">Seleccione los permisos siguientes y, a **continuación, seleccione Agregar permisos.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-133">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="6b3fa-134">**Calendars.ReadWrite:** esto permitirá que la aplicación lea y escriba en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-134">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="6b3fa-135">**MailboxSettings.Read:** esto permitirá que la aplicación obtenga la zona horaria del usuario desde su configuración de buzón.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-135">**MailboxSettings.Read** - this will allow the app to get the user's time zone from their mailbox settings.</span></span>

    ![Captura de pantalla de los permisos configurados](/tutorial/images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a><span data-ttu-id="6b3fa-137">Configurar el inicio de sesión único del complemento de Office</span><span class="sxs-lookup"><span data-stu-id="6b3fa-137">Configure Office Add-in single sign-on</span></span>

<span data-ttu-id="6b3fa-138">Actualice el registro de la aplicación para admitir el inicio de sesión [único (SSO) del complemento de Office.](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)</span><span class="sxs-lookup"><span data-stu-id="6b3fa-138">Update the app registration to support [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins).</span></span>

1. <span data-ttu-id="6b3fa-139">Seleccione **Exponer una API.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-139">Select **Expose an API**.</span></span> <span data-ttu-id="6b3fa-140">En la **sección Ámbitos definidos por esta API,** seleccione **Agregar un ámbito.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-140">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="6b3fa-141">Cuando se le pida que establezca un **URI de id.** de aplicación, establezca el valor en `api://localhost:3000/YOUR_APP_ID_HERE` , reemplazando con el identificador de `YOUR_APP_ID_HERE` aplicación.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-141">When prompted to set an **Application ID URI**, set the value to `api://localhost:3000/YOUR_APP_ID_HERE`, replacing `YOUR_APP_ID_HERE` with the application ID.</span></span> <span data-ttu-id="6b3fa-142">Elija **Guardar y continuar.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-142">Choose **Save and continue**.</span></span>

1. <span data-ttu-id="6b3fa-143">Rellene los campos como se muestra a continuación y seleccione **Agregar ámbito.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-143">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="6b3fa-144">**Nombre del ámbito:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="6b3fa-144">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="6b3fa-145">**¿Quién puede dar su consentimiento?: Administradores y usuarios**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-145">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="6b3fa-146">**Nombre para mostrar del consentimiento del administrador:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="6b3fa-146">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="6b3fa-147">**Descripción del consentimiento del administrador:**`Allows Office Add-ins to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="6b3fa-147">**Admin consent description:** `Allows Office Add-ins to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="6b3fa-148">**Nombre para mostrar del consentimiento del usuario:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="6b3fa-148">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="6b3fa-149">**Descripción del consentimiento del usuario:**`Allows Office Add-ins to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="6b3fa-149">**User consent description:** `Allows Office Add-ins to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="6b3fa-150">**Estado: habilitado**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-150">**State: Enabled**</span></span>

    ![Captura de pantalla del formulario Agregar un ámbito](/tutorial/images/add-scope.png)

1. <span data-ttu-id="6b3fa-152">En la **sección Aplicaciones cliente autorizadas,** seleccione Agregar una **aplicación cliente.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-152">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="6b3fa-153">Escriba un identificador de cliente en la siguiente lista, habilite el ámbito en **Ámbitos autorizados** y seleccione **Agregar aplicación.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-153">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="6b3fa-154">Repita este proceso para cada uno de los IDs de cliente de la lista.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-154">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="6b3fa-155">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="6b3fa-155">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span></span>
    - <span data-ttu-id="6b3fa-156">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="6b3fa-156">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span></span>
    - <span data-ttu-id="6b3fa-157">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office en la Web)</span><span class="sxs-lookup"><span data-stu-id="6b3fa-157">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span></span>
    - <span data-ttu-id="6b3fa-158">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office en la Web)</span><span class="sxs-lookup"><span data-stu-id="6b3fa-158">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span></span>

## <a name="install-development-certificates"></a><span data-ttu-id="6b3fa-159">Instalar certificados de desarrollo</span><span class="sxs-lookup"><span data-stu-id="6b3fa-159">Install development certificates</span></span>

1. <span data-ttu-id="6b3fa-160">Ejecute el siguiente comando para generar e instalar certificados de desarrollo para el complemento.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-160">Run the following command to generate and install development certificates for your add-in.</span></span>

    ```Shell
    npx office-addin-dev-certs install
    ```

    <span data-ttu-id="6b3fa-161">Si se le solicita confirmación, confirme las acciones.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-161">If prompted for confirmation, confirm the actions.</span></span> <span data-ttu-id="6b3fa-162">Una vez completado el comando, verá un resultado similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-162">Once the command completes, you will see output similar to the following.</span></span>

    ```Shell
    You now have trusted access to https://localhost.
    Certificate: <path>\localhost.crt
    Key: <path>\localhost.key
    ```

1. <span data-ttu-id="6b3fa-163">Copie las rutas de acceso a localhost.crt y localhost.key, las necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-163">Copy the paths to localhost.crt and localhost.key, you'll need them in the next step.</span></span>

## <a name="update-the-manifest"></a><span data-ttu-id="6b3fa-164">Actualizar el manifiesto</span><span class="sxs-lookup"><span data-stu-id="6b3fa-164">Update the manifest</span></span>

1. <span data-ttu-id="6b3fa-165">Abra el **manifest.xml** archivo y realice los siguientes cambios.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-165">Open the **manifest.xml** file and make the following changes.</span></span>
    1. <span data-ttu-id="6b3fa-166">Reemplazar `NEW_GUID_HERE` con un nuevo GUID, como `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` .</span><span class="sxs-lookup"><span data-stu-id="6b3fa-166">Replace `NEW_GUID_HERE` with a new GUID, like `b4fa03b8-1eb6-4e8b-a380-e0476be9e019`.</span></span>
    1. <span data-ttu-id="6b3fa-167">Reemplaza todas las instancias por `YOUR_APP_ID_HERE` el identificador de aplicación del registro de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-167">Replace all instances of `YOUR_APP_ID_HERE` with the application ID from your app registration.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="6b3fa-168">Configuración del ejemplo</span><span class="sxs-lookup"><span data-stu-id="6b3fa-168">Configure the sample</span></span>

1. <span data-ttu-id="6b3fa-169">Cambie el nombre `example.env` del archivo a `.env` .</span><span class="sxs-lookup"><span data-stu-id="6b3fa-169">Rename the `example.env` file to `.env`.</span></span>
1. <span data-ttu-id="6b3fa-170">Edite `.env` el archivo y realice los siguientes cambios.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-170">Edit the `.env` file and make the following changes.</span></span>
    1. <span data-ttu-id="6b3fa-171">Reemplace `YOUR_APP_ID_HERE` por el **id. de** aplicación que obtuvo del Portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-171">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="6b3fa-172">Reemplace `YOUR_CLIENT_SECRET_HERE` por el secreto de cliente que obtuvo del Portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-172">Replace `YOUR_CLIENT_SECRET_HERE` with the client secret you got from the App Registration Portal.</span></span>
    1. <span data-ttu-id="6b3fa-173">Reemplace `PATH_TO_LOCALHOST.CRT` por la ruta de acceso al archivo localhost.crt desde el resultado del `npx office-addin-dev-certs install` comando.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-173">Replace `PATH_TO_LOCALHOST.CRT` with the path to your localhost.crt file from the output of the `npx office-addin-dev-certs install` command.</span></span>
    1. <span data-ttu-id="6b3fa-174">Reemplace `PATH_TO_LOCALHOST.KEY` por la ruta de acceso al archivo localhost.key desde el resultado del `npx office-addin-dev-certs install` comando.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-174">Replace `PATH_TO_LOCALHOST.KEY` with the path to your localhost.key file from the output of the `npx office-addin-dev-certs install` command.</span></span>

1. <span data-ttu-id="6b3fa-175">Cambie el nombre `config.example.js` del archivo a `config.js` .</span><span class="sxs-lookup"><span data-stu-id="6b3fa-175">Rename the `config.example.js` file to `config.js`.</span></span>
1. <span data-ttu-id="6b3fa-176">Edite `config.js` el archivo y realice los siguientes cambios.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-176">Edit the `config.js` file and make the following changes.</span></span>
    1. <span data-ttu-id="6b3fa-177">Reemplace `YOUR_APP_ID_HERE` por el **id. de** aplicación que obtuvo del Portal de registro de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-177">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>
1. <span data-ttu-id="6b3fa-178">En la interfaz de línea de comandos (CLI), vaya a este directorio y ejecute el siguiente comando para instalar los requisitos.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-178">In your command-line interface (CLI), navigate to this directory and run the following command to install requirements.</span></span>

    ```Shell
    yarn install
    ```

## <a name="run-the-sample"></a><span data-ttu-id="6b3fa-179">Ejecutar el ejemplo</span><span class="sxs-lookup"><span data-stu-id="6b3fa-179">Run the sample</span></span>

1. <span data-ttu-id="6b3fa-180">Ejecute el siguiente comando en la CLI para iniciar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-180">Run the following command in your CLI to start the application.</span></span>

    ```Shell
    yarn start
    ```

1. <span data-ttu-id="6b3fa-181">En el explorador, vaya a [Office.com](https://www.office.com/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-181">In your browser, go to [Office.com](https://www.office.com/) and sign in.</span></span> <span data-ttu-id="6b3fa-182">Seleccione **Crear en la** barra de herramientas izquierda y, a continuación, seleccione Hoja de **cálculo.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-182">Select **Create** in the left-hand toolbar, then select **Spreadsheet**.</span></span>

    ![Captura de pantalla del menú Crear en Office.com](/tutorial/images/office-select-excel.png)

1. <span data-ttu-id="6b3fa-184">Seleccione la **pestaña** Insertar y, a continuación, **seleccione Complementos de Office.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-184">Select the **Insert** tab, then select **Office Add-ins**.</span></span>

1. <span data-ttu-id="6b3fa-185">Seleccione **Cargar mi complemento y,** a continuación, seleccione **Examinar.**</span><span class="sxs-lookup"><span data-stu-id="6b3fa-185">Select **Upload My Add-in**, then select **Browse**.</span></span> <span data-ttu-id="6b3fa-186">Cargue el **manifest.xml** archivo.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-186">Upload your **manifest.xml** file.</span></span>

1. <span data-ttu-id="6b3fa-187">Seleccione el **botón Importar calendario** en la pestaña **Inicio** para abrir el panel de tareas.</span><span class="sxs-lookup"><span data-stu-id="6b3fa-187">Select the **Import Calendar** button on the **Home** tab to open the taskpane.</span></span>

    ![Captura de pantalla del botón Importar calendario en la pestaña Inicio](/tutorial/images/get-started.png)
