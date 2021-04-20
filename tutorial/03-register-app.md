---
ms.openlocfilehash: 89bc4baff47ae895c7c0dfd34a8f8d4d0da091f5
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899438"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="eb1c6-101">En este ejercicio, creará un nuevo registro de aplicaciones web de Azure AD mediante el Centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="eb1c6-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="eb1c6-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="eb1c6-103">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="eb1c6-104">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="eb1c6-105">Captura de pantalla de los registros de la aplicación</span><span class="sxs-lookup"><span data-stu-id="eb1c6-105">A screenshot of the App registrations</span></span> ](images/app-registrations.png)

1. <span data-ttu-id="eb1c6-106">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-106">Select **New registration**.</span></span> <span data-ttu-id="eb1c6-107">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="eb1c6-108">Establezca **Nombre** como `Office Add-in Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-108">Set **Name** to `Office Add-in Graph Tutorial`.</span></span>
    - <span data-ttu-id="eb1c6-109">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="eb1c6-110">En **URI de redirección**, establezca la primera lista desplegable en `Single-page application (SPA)` y establezca el valor `https://localhost:3000/consent.html`.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `https://localhost:3000/consent.html`.</span></span>

    ![Captura de pantalla de la página Registrar una aplicación](images/register-an-app.png)

1. <span data-ttu-id="eb1c6-112">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-112">Select **Register**.</span></span> <span data-ttu-id="eb1c6-113">En la página Tutorial de Gráfico de complementos de **Office,** copie el valor del identificador de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-113">On the **Office Add-in Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Una captura de pantalla del Id. de aplicación del nuevo registro de la aplicación](images/application-id.png)

1. <span data-ttu-id="eb1c6-115">Seleccione **Autenticación** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-115">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="eb1c6-116">Busque la **sección Concesión** implícita y habilite **los tokens de Access** y los tokens de **id.**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-116">Locate the **Implicit grant** section and enable **Access tokens** and **ID tokens**.</span></span> <span data-ttu-id="eb1c6-117">Seleccione **Guardar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-117">Select **Save**.</span></span>

    ![Una captura de pantalla de la sección concesión implícita](./images/aad-implicit-grant.png)

1. <span data-ttu-id="eb1c6-119">Seleccione **Certificados y secretos** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-119">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="eb1c6-120">Seleccione el botón **Nuevo secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-120">Select the **New client secret** button.</span></span> <span data-ttu-id="eb1c6-121">Escriba un valor en **Descripción**, y seleccione una de las opciones para **Expira**, y después, **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-121">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="eb1c6-122">Copie el valor del secreto de cliente antes de salir de esta página.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-122">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="eb1c6-123">Lo necesitará en el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-123">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="eb1c6-124">El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-124">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="eb1c6-125">Seleccione **Permisos de API en** **Administrar** y, a continuación, seleccione Agregar **un permiso**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-125">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="eb1c6-126">Seleccione **Microsoft Graph** y, a continuación, Permisos **delegados**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-126">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="eb1c6-127">Seleccione los siguientes permisos y, a continuación, **seleccione Agregar permisos**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-127">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="eb1c6-128">**offline_access:** esto permitirá a la aplicación actualizar los tokens de acceso cuando expiren.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-128">**offline_access** - this will allow the app to refresh access tokens when they expire.</span></span>
    - <span data-ttu-id="eb1c6-129">**Calendars.ReadWrite:** esto permitirá que la aplicación lea y escriba en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-129">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="eb1c6-130">**MailboxSettings.Read:** esto permitirá a la aplicación obtener la zona horaria del usuario desde la configuración de su buzón.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-130">**MailboxSettings.Read** - this will allow the app to get the user's time zone from their mailbox settings.</span></span>

    ![Captura de pantalla de los permisos configurados](images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a><span data-ttu-id="eb1c6-132">Configurar el inicio de sesión único del complemento de Office</span><span class="sxs-lookup"><span data-stu-id="eb1c6-132">Configure Office Add-in single sign-on</span></span>

<span data-ttu-id="eb1c6-133">En esta sección, actualizará el registro de la aplicación para admitir el inicio de sesión único [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)del complemento de Office.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-133">In this section you'll update the app registration to support [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins).</span></span>

1. <span data-ttu-id="eb1c6-134">Seleccione **Exponer una API**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-134">Select **Expose an API**.</span></span> <span data-ttu-id="eb1c6-135">En la **sección Ámbitos definidos por esta API,** seleccione **Agregar un ámbito**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-135">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="eb1c6-136">Cuando se le pida que establezca un **URI de id.** de aplicación, establezca el valor en `api://localhost:3000/YOUR_APP_ID_HERE` , reemplazando por el identificador de `YOUR_APP_ID_HERE` aplicación.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-136">When prompted to set an **Application ID URI**, set the value to `api://localhost:3000/YOUR_APP_ID_HERE`, replacing `YOUR_APP_ID_HERE` with the application ID.</span></span> <span data-ttu-id="eb1c6-137">Elija **Guardar y continuar**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-137">Choose **Save and continue**.</span></span>

1. <span data-ttu-id="eb1c6-138">Rellene los campos de la siguiente manera y seleccione **Agregar ámbito**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-138">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="eb1c6-139">**Nombre del ámbito:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="eb1c6-139">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="eb1c6-140">**¿Quién puede dar su consentimiento?: Administradores y usuarios**</span><span class="sxs-lookup"><span data-stu-id="eb1c6-140">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="eb1c6-141">**Nombre para mostrar del consentimiento de administrador:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="eb1c6-141">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="eb1c6-142">**Descripción del consentimiento de administrador:**`Allows Office Add-ins to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="eb1c6-142">**Admin consent description:** `Allows Office Add-ins to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="eb1c6-143">**Nombre para mostrar del consentimiento del usuario:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="eb1c6-143">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="eb1c6-144">**Descripción del consentimiento del usuario:**`Allows Office Add-ins to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="eb1c6-144">**User consent description:** `Allows Office Add-ins to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="eb1c6-145">**Estado: habilitado**</span><span class="sxs-lookup"><span data-stu-id="eb1c6-145">**State: Enabled**</span></span>

    ![Captura de pantalla del formulario Agregar un ámbito](images/add-scope.png)

1. <span data-ttu-id="eb1c6-147">En la **sección Aplicaciones cliente autorizadas,** seleccione Agregar una **aplicación cliente**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-147">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="eb1c6-148">Escriba un identificador de cliente en la siguiente lista, habilite el ámbito en **Ámbitos autorizados** y seleccione **Agregar aplicación**.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-148">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="eb1c6-149">Repita este proceso para cada uno de los IDs de cliente de la lista.</span><span class="sxs-lookup"><span data-stu-id="eb1c6-149">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="eb1c6-150">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="eb1c6-150">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span></span>
    - <span data-ttu-id="eb1c6-151">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="eb1c6-151">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span></span>
    - <span data-ttu-id="eb1c6-152">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office en la Web)</span><span class="sxs-lookup"><span data-stu-id="eb1c6-152">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span></span>
    - <span data-ttu-id="eb1c6-153">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office en la Web)</span><span class="sxs-lookup"><span data-stu-id="eb1c6-153">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span></span>
