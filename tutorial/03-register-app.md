---
ms.openlocfilehash: a336095a238eefeae22dac86d29e3140e8d94bf1
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274338"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio, creará un nuevo registro de aplicación web de Azure AD mediante el Centro de administración de Azure Active Directory.

1. Abra un explorador y vaya al [centro de administración de Azure Active Directory](https://aad.portal.azure.com). Inicie sesión con una **cuenta personal** (también conocida como: cuenta Microsoft) o una **cuenta profesional o educativa**.

1. Seleccione **Azure Active Directory** en el panel de navegación izquierdo y, a continuación, seleccione **Registros de aplicaciones** en **Administrar**.

    ![Captura de pantalla de los registros de aplicaciones ](images/app-registrations.png)

1. Seleccione **Nuevo registro**. En la página **Registrar una aplicación**, establezca los valores siguientes.

    - Establezca **Nombre** como `Office Add-in Graph Tutorial`.
    - Establezca **Tipos de cuenta admitidos** en **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft**.
    - En **URI de redirección**, establezca la primera lista desplegable en `Single-page application (SPA)` y establezca el valor `https://localhost:3000/consent.html`.

    ![Captura de pantalla de la página Registrar una aplicación](images/register-an-app.png)

1. Seleccione **Registrar**. En la **página Tutorial de Office Add-in Graph,** copie el valor del id. de aplicación **(cliente)** y guárdelo, lo necesitará en el paso siguiente.

    ![Captura de pantalla del identificador de aplicación del nuevo registro de la aplicación](images/application-id.png)

1. Seleccione **Certificados y secretos** en **Administrar**. Seleccione el botón **Nuevo secreto de cliente**. Escriba un valor en **Descripción,** seleccione una de las opciones para **Expira y** seleccione **Agregar**.

1. Copie el valor del secreto de cliente antes de salir de esta página. Lo necesitará en el siguiente paso.

    > [!IMPORTANT]
    > El secreto de cliente no se vuelve a mostrar, así que asegúrese de copiarlo en este momento.

1. Seleccione **permisos de API en** **Administrar** y, a continuación, seleccione Agregar **un permiso.**

1. Seleccione **Microsoft Graph** y, a continuación, permisos **delegados.**

1. Seleccione los permisos siguientes y, a **continuación, seleccione Agregar permisos.**

    - **offline_access:** esto permitirá que la aplicación actualice los tokens de acceso cuando expiren.
    - **Calendars.ReadWrite:** esto permitirá que la aplicación lea y escriba en el calendario del usuario.
    - **MailboxSettings.Read:** esto permitirá que la aplicación obtenga la zona horaria del usuario desde su configuración de buzón.

    ![Captura de pantalla de los permisos configurados](images/configured-permissions.png)

## <a name="configure-office-add-in-single-sign-on"></a>Configurar el inicio de sesión único del complemento de Office

En esta sección, actualizará el registro de la aplicación para admitir el inicio de sesión único [(SSO)](https://docs.microsoft.com/office/dev/add-ins/develop/sso-in-office-add-ins)del complemento de Office.

1. Seleccione **Exponer una API.** En la **sección Ámbitos definidos por esta API,** seleccione **Agregar un ámbito.** Cuando se le pida que establezca un **URI de id.** de aplicación, establezca el valor en `api://localhost:3000/YOUR_APP_ID_HERE` , reemplazando con el identificador de `YOUR_APP_ID_HERE` aplicación. Elija **Guardar y continuar.**

1. Rellene los campos como se muestra a continuación y seleccione **Agregar ámbito.**

    - **Nombre del ámbito:**`access_as_user`
    - **¿Quién puede dar su consentimiento?: Administradores y usuarios**
    - **Nombre para mostrar del consentimiento del administrador:**`Access the app as the user`
    - **Descripción del consentimiento del administrador:**`Allows Office Add-ins to call the app's web APIs as the current user.`
    - **Nombre para mostrar del consentimiento del usuario:**`Access the app as you`
    - **Descripción del consentimiento del usuario:**`Allows Office Add-ins to call the app's web APIs as you.`
    - **Estado: habilitado**

    ![Captura de pantalla del formulario Agregar un ámbito](images/add-scope.png)

1. En la **sección Aplicaciones cliente autorizadas,** seleccione Agregar una **aplicación cliente.** Escriba un identificador de cliente en la siguiente lista, habilite el ámbito en **Ámbitos autorizados** y seleccione **Agregar aplicación.** Repita este proceso para cada uno de los IDs de cliente de la lista.

    - `d3590ed6-52b3-4102-aeff-aad2292ab01c` (Microsoft Office)
    - `ea5a67f6-b6f3-4338-b240-c655ddc3cc8e` (Microsoft Office)
    - `57fb890c-0dab-4253-a5e0-7188c88b2bb4` (Office en la Web)
    - `08e18876-6177-487e-b8b5-cf950c1e598c` (Office en la Web)
