---
ms.openlocfilehash: a336095a238eefeae22dac86d29e3140e8d94bf1
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274338"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="4be53-101">En este ejercicio, creará un nuevo registro de aplicación web de Azure AD mediante el Centro de administración de Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="4be53-101">In this exercise, you will create a new Azure AD web application registration using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="4be53-102">Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4be53-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="4be53-103">Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.</span><span class="sxs-lookup"><span data-stu-id="4be53-103">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="4be53-104">Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="4be53-104">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="4be53-105">Captura de pantalla de los registros de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="4be53-105">A screenshot of the App registrations</span></span> ](images/app-registrations.png)

1. <span data-ttu-id="4be53-106">Seleccione **Nuevo registro**.</span><span class="sxs-lookup"><span data-stu-id="4be53-106">Select **New registration**.</span></span> <span data-ttu-id="4be53-107">En la página **Registrar una aplicación**, establezca los valores siguientes.</span><span class="sxs-lookup"><span data-stu-id="4be53-107">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="4be53-108">Establezca **Nombre** como `Office Add-in Graph Tutorial`.</span><span class="sxs-lookup"><span data-stu-id="4be53-108">Set **Name** to `Office Add-in Graph Tutorial`.</span></span>
    - <span data-ttu-id="4be53-109">Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="4be53-109">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="4be53-110">En **URI de redirección**, establezca la primera lista desplegable en `Single-page application (SPA)` y establezca el valor `https://localhost:3000/consent.html`.</span><span class="sxs-lookup"><span data-stu-id="4be53-110">Under **Redirect URI**, set the first drop-down to `Single-page application (SPA)` and set the value to `https://localhost:3000/consent.html`.</span></span>

    ![Captura de pantalla de la página Registrar una aplicación](images/register-an-app.png)

1. <span data-ttu-id="4be53-112">Seleccione **Registrar**.</span><span class="sxs-lookup"><span data-stu-id="4be53-112">Select **Register**.</span></span> <span data-ttu-id="4be53-113">En la **página Tutorial de Office Add-in Graph,** copie el valor del id. de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.</span><span class="sxs-lookup"><span data-stu-id="4be53-113">On the **Office Add-in Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Captura de pantalla del identificador de aplicación del nuevo registro de la aplicación](images/application-id.png)

1. <span data-ttu-id="4be53-115">Seleccione **Certificados y secretos** en **Administrar**.</span><span class="sxs-lookup"><span data-stu-id="4be53-115">Select **Certificates & secrets** under **Manage**.</span></span> <span data-ttu-id="4be53-116">Seleccione el botón **Nuevo secreto de cliente**.</span><span class="sxs-lookup"><span data-stu-id="4be53-116">Select the **New client secret** button.</span></span> <span data-ttu-id="4be53-117">Escriba un valor en **Descripción,** seleccione una de las opciones para **Expira y** seleccione **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="4be53-117">Enter a value in **Description** and select one of the options for **Expires** and select **Add**.</span></span>

1. <span data-ttu-id="4be53-118">Copie el valor del secreto de cliente antes de salir de esta página.</span><span class="sxs-lookup"><span data-stu-id="4be53-118">Copy the client secret value before you leave this page.</span></span> <span data-ttu-id="4be53-119">Lo necesitará en el siguiente paso.</span><span class="sxs-lookup"><span data-stu-id="4be53-119">You will need it in the next step.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="4be53-120">El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.</span><span class="sxs-lookup"><span data-stu-id="4be53-120">This client secret is never shown again, so make sure you copy it now.</span></span>

1. <span data-ttu-id="4be53-121">Seleccione **permisos de API en** **Administrar** y, a continuación, seleccione Agregar **un permiso.**</span><span class="sxs-lookup"><span data-stu-id="4be53-121">Select **API permissions** under **Manage**, then select **Add a permission**.</span></span>

1. <span data-ttu-id="4be53-122">Seleccione **Microsoft Graph** y, a continuación, permisos **delegados.**</span><span class="sxs-lookup"><span data-stu-id="4be53-122">Select **Microsoft Graph**, then **Delegated permissions**.</span></span>

1. <span data-ttu-id="4be53-123">Seleccione los permisos siguientes y, a **continuación, seleccione Agregar permisos.**</span><span class="sxs-lookup"><span data-stu-id="4be53-123">Select the following permissions, then select **Add permissions**.</span></span>

    - <span data-ttu-id="4be53-124">**offline_access:** esto permitirá que la aplicación actualice los tokens de acceso cuando expiren.</span><span class="sxs-lookup"><span data-stu-id="4be53-124">**offline_access** - this will allow the app to refresh access tokens when they expire.</span></span>
    - <span data-ttu-id="4be53-125">**Calendars.ReadWrite:** esto permitirá que la aplicación lea y escriba en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="4be53-125">**Calendars.ReadWrite** - this will allow the app to read and write to the user's calendar.</span></span>
    - <span data-ttu-id="4be53-126">**MailboxSettings.Read:** esto permitirá que la aplicación obtenga la zona horaria del usuario desde su configuración de buzón.</span><span class="sxs-lookup"><span data-stu-id="4be53-126">**MailboxSettings.Read** - this will allow the app to get the user's time zone from their mailbox settings.</span></span>

    ![Captura de pantalla de los permisos configurados](images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a><span data-ttu-id="4be53-128">Configurar el inicio de sesión único del complemento de Office</span><span class="sxs-lookup"><span data-stu-id="4be53-128">Configure Office Add-in single sign-on</span></span>

<span data-ttu-id="4be53-129">En esta sección, actualizará el registro de la aplicación para admitir el inicio de sesión único [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)del complemento de Office.</span><span class="sxs-lookup"><span data-stu-id="4be53-129">In this section you'll update the app registration to support [Office Add-in single sign-on (SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins).</span></span>

1. <span data-ttu-id="4be53-130">Seleccione **Exponer una API.**</span><span class="sxs-lookup"><span data-stu-id="4be53-130">Select **Expose an API**.</span></span> <span data-ttu-id="4be53-131">En la **sección Ámbitos definidos por esta API,** seleccione **Agregar un ámbito.**</span><span class="sxs-lookup"><span data-stu-id="4be53-131">In the **Scopes defined by this API** section, select **Add a scope**.</span></span> <span data-ttu-id="4be53-132">Cuando se le pida que establezca un **URI de id.** de aplicación, establezca el valor en `api://localhost:3000/YOUR_APP_ID_HERE` , reemplazando con el identificador de `YOUR_APP_ID_HERE` aplicación.</span><span class="sxs-lookup"><span data-stu-id="4be53-132">When prompted to set an **Application ID URI**, set the value to `api://localhost:3000/YOUR_APP_ID_HERE`, replacing `YOUR_APP_ID_HERE` with the application ID.</span></span> <span data-ttu-id="4be53-133">Elija **Guardar y continuar.**</span><span class="sxs-lookup"><span data-stu-id="4be53-133">Choose **Save and continue**.</span></span>

1. <span data-ttu-id="4be53-134">Rellene los campos como se muestra a continuación y seleccione **Agregar ámbito.**</span><span class="sxs-lookup"><span data-stu-id="4be53-134">Fill in the fields as follows and select **Add scope**.</span></span>

    - <span data-ttu-id="4be53-135">**Nombre del ámbito:**`access_as_user`</span><span class="sxs-lookup"><span data-stu-id="4be53-135">**Scope name:** `access_as_user`</span></span>
    - <span data-ttu-id="4be53-136">**¿Quién puede dar su consentimiento?: Administradores y usuarios**</span><span class="sxs-lookup"><span data-stu-id="4be53-136">**Who can consent?: Admins and users**</span></span>
    - <span data-ttu-id="4be53-137">**Nombre para mostrar del consentimiento del administrador:**`Access the app as the user`</span><span class="sxs-lookup"><span data-stu-id="4be53-137">**Admin consent display name:** `Access the app as the user`</span></span>
    - <span data-ttu-id="4be53-138">**Descripción del consentimiento del administrador:**`Allows Office Add-ins to call the app's web APIs as the current user.`</span><span class="sxs-lookup"><span data-stu-id="4be53-138">**Admin consent description:** `Allows Office Add-ins to call the app's web APIs as the current user.`</span></span>
    - <span data-ttu-id="4be53-139">**Nombre para mostrar del consentimiento del usuario:**`Access the app as you`</span><span class="sxs-lookup"><span data-stu-id="4be53-139">**User consent display name:** `Access the app as you`</span></span>
    - <span data-ttu-id="4be53-140">**Descripción del consentimiento del usuario:**`Allows Office Add-ins to call the app's web APIs as you.`</span><span class="sxs-lookup"><span data-stu-id="4be53-140">**User consent description:** `Allows Office Add-ins to call the app's web APIs as you.`</span></span>
    - <span data-ttu-id="4be53-141">**Estado: habilitado**</span><span class="sxs-lookup"><span data-stu-id="4be53-141">**State: Enabled**</span></span>

    ![Captura de pantalla del formulario Agregar un ámbito](images/add-scope.png)

1. <span data-ttu-id="4be53-143">En la **sección Aplicaciones cliente autorizadas,** seleccione Agregar una **aplicación cliente.**</span><span class="sxs-lookup"><span data-stu-id="4be53-143">In the **Authorized client applications** section, select **Add a client application**.</span></span> <span data-ttu-id="4be53-144">Escriba un identificador de cliente en la siguiente lista, habilite el ámbito en **Ámbitos autorizados** y seleccione **Agregar aplicación.**</span><span class="sxs-lookup"><span data-stu-id="4be53-144">Enter a client ID from the following list, enable the scope under **Authorized scopes**, and select **Add application**.</span></span> <span data-ttu-id="4be53-145">Repita este proceso para cada uno de los IDs de cliente de la lista.</span><span class="sxs-lookup"><span data-stu-id="4be53-145">Repeat this process for each of the client IDs in the list.</span></span>

    - <span data-ttu-id="4be53-146">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="4be53-146">`d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)</span></span>
    - <span data-ttu-id="4be53-147">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span><span class="sxs-lookup"><span data-stu-id="4be53-147">`ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)</span></span>
    - <span data-ttu-id="4be53-148">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office en la Web)</span><span class="sxs-lookup"><span data-stu-id="4be53-148">`57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office on the web)</span></span>
    - <span data-ttu-id="4be53-149">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office en la Web)</span><span class="sxs-lookup"><span data-stu-id="4be53-149">`08e18876-6177-487e-b8b5-cf950c1e598c` (Office on the web)</span></span>
