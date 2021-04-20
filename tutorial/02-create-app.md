---
ms.openlocfilehash: 09ef719985094d87c438b6f98b931865dbd59c75
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899425"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="ffb91-101">En este ejercicio, creará una solución de complemento de Office con [Express](http://expressjs.com/).</span><span class="sxs-lookup"><span data-stu-id="ffb91-101">In this exercise you will create an Office Add-in solution using [Express](http://expressjs.com/).</span></span> <span data-ttu-id="ffb91-102">La solución constará de dos partes.</span><span class="sxs-lookup"><span data-stu-id="ffb91-102">The solution will consist of two parts.</span></span>

- <span data-ttu-id="ffb91-103">El complemento, implementado como archivos HTML estáticos y JavaScript.</span><span class="sxs-lookup"><span data-stu-id="ffb91-103">The add-in, implemented as static HTML and JavaScript files.</span></span>
- <span data-ttu-id="ffb91-104">Un Node.js/Express que sirve el complemento e implementa una API web para recuperar datos del complemento.</span><span class="sxs-lookup"><span data-stu-id="ffb91-104">A Node.js/Express server that serves the add-in and implements a web API to retrieve data for the add-in.</span></span>

## <a name="create-the-server"></a><span data-ttu-id="ffb91-105">Crear el servidor</span><span class="sxs-lookup"><span data-stu-id="ffb91-105">Create the server</span></span>

1. <span data-ttu-id="ffb91-106">Abra la interfaz de línea de comandos (CLI), vaya a un directorio donde desee crear el proyecto y ejecute el siguiente comando para generar una package.jsarchivo.</span><span class="sxs-lookup"><span data-stu-id="ffb91-106">Open your command-line interface (CLI), navigate to a directory where you want to create your project, and run the following command to generate a package.json file.</span></span>

    ```Shell
    yarn init
    ```

    <span data-ttu-id="ffb91-107">Escriba valores para los mensajes según corresponda.</span><span class="sxs-lookup"><span data-stu-id="ffb91-107">Enter values for the prompts as appropriate.</span></span> <span data-ttu-id="ffb91-108">Si no está seguro, los valores predeterminados están bien.</span><span class="sxs-lookup"><span data-stu-id="ffb91-108">If you're unsure, the default values are fine.</span></span>

1. <span data-ttu-id="ffb91-109">Ejecute los siguientes comandos para instalar dependencias.</span><span class="sxs-lookup"><span data-stu-id="ffb91-109">Run the following commands to install dependencies.</span></span>

    ```Shell
    yarn add express@4.17.1 express-promise-router@4.1.0 dotenv@8.2.0 node-fetch@2.6.1 jsonwebtoken@8.5.1@
    yarn add jwks-rsa@2.0.2 @azure/msal-node@1.0.2 @microsoft/microsoft-graph-client@2.2.1
    yarn add date-fns@2.21.1 date-fns-tz@1.1.4 isomorphic-fetch@3.0.0 windows-iana@5.0.1
    yarn add -D typescript@4.2.4 ts-node@9.1.1 nodemon@2.0.7 @types/node@14.14.41 @types/express@4.17.11
    yarn add -D @types/node-fetch@2.5.10 @types/jsonwebtoken@8.5.1 @types/microsoft-graph@1.35.0
    yarn add -D @types/office-js@1.0.174 @types/jquery@3.5.5 @types/isomorphic-fetch@0.0.35
    ```

1. <span data-ttu-id="ffb91-110">Ejecute el siguiente comando para generar una tsconfig.jsen el archivo.</span><span class="sxs-lookup"><span data-stu-id="ffb91-110">Run the following command to generate a tsconfig.json file.</span></span>

    ```Shell
    tsc --init
    ```

1. <span data-ttu-id="ffb91-111">Abra **./tsconfig.jsen** un editor de texto y realice los siguientes cambios.</span><span class="sxs-lookup"><span data-stu-id="ffb91-111">Open **./tsconfig.json** in a text editor and make the following changes.</span></span>

    - <span data-ttu-id="ffb91-112">Cambie el `target` valor a `es6` .</span><span class="sxs-lookup"><span data-stu-id="ffb91-112">Change the `target` value to `es6`.</span></span>
    - <span data-ttu-id="ffb91-113">Descomprima `outDir` el valor y estadúdelo en `./dist` .</span><span class="sxs-lookup"><span data-stu-id="ffb91-113">Uncomment the `outDir` value and set it to `./dist`.</span></span>
    - <span data-ttu-id="ffb91-114">Descomprima `rootDir` el valor y estadúdelo en `./src` .</span><span class="sxs-lookup"><span data-stu-id="ffb91-114">Uncomment the `rootDir` value and set it to `./src`.</span></span>

1. <span data-ttu-id="ffb91-115">Abra **./package.jsy** agregue la siguiente propiedad al JSON.</span><span class="sxs-lookup"><span data-stu-id="ffb91-115">Open **./package.json** and add the following property to the JSON.</span></span>

    ```json
    "scripts": {
      "start": "nodemon ./src/server.ts",
      "build": "tsc --project ./"
    },
    ```

1. <span data-ttu-id="ffb91-116">Ejecute el siguiente comando para generar e instalar certificados de desarrollo para el complemento.</span><span class="sxs-lookup"><span data-stu-id="ffb91-116">Run the following command to generate and install development certificates for your add-in.</span></span>

    ```Shell
    npx office-addin-dev-certs install
    ```

    <span data-ttu-id="ffb91-117">Si se le pide confirmación, confirme las acciones.</span><span class="sxs-lookup"><span data-stu-id="ffb91-117">If prompted for confirmation, confirm the actions.</span></span> <span data-ttu-id="ffb91-118">Una vez completado el comando, verá un resultado similar al siguiente.</span><span class="sxs-lookup"><span data-stu-id="ffb91-118">Once the command completes, you will see output similar to the following.</span></span>

    ```Shell
    You now have trusted access to https://localhost.
    Certificate: <path>\localhost.crt
    Key: <path>\localhost.key
    ```

1. <span data-ttu-id="ffb91-119">Cree un nuevo archivo denominado **.env** en la raíz del proyecto y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-119">Create a new file named **.env** in the root of your project and add the following code.</span></span>

    :::code language="ini" source="../demo/graph-tutorial/example.env":::

    <span data-ttu-id="ffb91-120">Reemplace por la ruta de acceso a localhost.crt y por la ruta de acceso a la salida `PATH_TO_LOCALHOST.CRT` `PATH_TO_LOCALHOST.KEY` localhost.key por el comando anterior.</span><span class="sxs-lookup"><span data-stu-id="ffb91-120">Replace `PATH_TO_LOCALHOST.CRT` with the path to localhost.crt and `PATH_TO_LOCALHOST.KEY` with the path to localhost.key output by the previous command.</span></span>

1. <span data-ttu-id="ffb91-121">Cree un nuevo directorio en la raíz del proyecto denominado **src**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-121">Create a new directory in the root of your project named **src**.</span></span>

1. <span data-ttu-id="ffb91-122">Cree dos directorios en el directorio **./src:** **addin** y **api**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-122">Create two directories in the **./src** directory: **addin** and **api**.</span></span>

1. <span data-ttu-id="ffb91-123">Cree un nuevo archivo denominado **auth.ts** en el directorio **./src/api** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-123">Create a new file named **auth.ts** in the **./src/api** directory and add the following code.</span></span>

    ```typescript
    import Router from 'express-promise-router';

    const authRouter = Router();

    // TODO: Implement this router

    export default authRouter;
    ```

1. <span data-ttu-id="ffb91-124">Cree un nuevo archivo denominado **graph.ts** en el directorio **./src/api** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-124">Create a new file named **graph.ts** in the **./src/api** directory and add the following code.</span></span>

    ```typescript
    import Router from 'express-promise-router';

    const graphRouter = Router();

    // TODO: Implement this router

    export default graphRouter;
    ```

1. <span data-ttu-id="ffb91-125">Cree un nuevo archivo denominado **server.ts en** el directorio **./src** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-125">Create a new file named **server.ts** in the **./src** directory and add the following code.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/server.ts" id="ServerSnippet":::

## <a name="create-the-add-in"></a><span data-ttu-id="ffb91-126">Crear el complemento</span><span class="sxs-lookup"><span data-stu-id="ffb91-126">Create the add-in</span></span>

1. <span data-ttu-id="ffb91-127">Cree un nuevo archivo denominado **taskpane.html** en el directorio **./src/addin** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-127">Create a new file named **taskpane.html** in the **./src/addin** directory and add the following code.</span></span>

    :::code language="html" source="../demo/graph-tutorial/src/addin/taskpane.html" id="TaskPaneHtmlSnippet":::

1. <span data-ttu-id="ffb91-128">Cree un nuevo archivo denominado **taskpane.css** en el directorio **./src/addin** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-128">Create a new file named **taskpane.css** in the **./src/addin** directory and add the following code.</span></span>

    :::code language="css" source="../demo/graph-tutorial/src/addin/taskpane.css":::

1. <span data-ttu-id="ffb91-129">Cree un nuevo archivo denominado **taskpane.js** en el directorio **./src/addin** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-129">Create a new file named **taskpane.js** in the **./src/addin** directory and add the following code.</span></span>

    ```javascript
    // TEMPORARY CODE TO VERIFY ADD-IN LOADS
    'use strict';

    Office.onReady(info => {
      if (info.host === Office.HostType.Excel) {
        $(function() {
          $('p').text('Hello World!!');
        });
      }
    });
    ```

1. <span data-ttu-id="ffb91-130">Cree un nuevo directorio en el directorio **.src/addin** denominado **assets**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-130">Create a new directory in the **.src/addin** directory named **assets**.</span></span>

1. <span data-ttu-id="ffb91-131">Agregue tres archivos PNG en este directorio de acuerdo con la tabla siguiente.</span><span class="sxs-lookup"><span data-stu-id="ffb91-131">Add three PNG files in this directory according to the following table.</span></span>

    | <span data-ttu-id="ffb91-132">Nombre de archivo</span><span class="sxs-lookup"><span data-stu-id="ffb91-132">File name</span></span>   | <span data-ttu-id="ffb91-133">Tamaño en píxeles</span><span class="sxs-lookup"><span data-stu-id="ffb91-133">Size in pixels</span></span> |
    |-------------|----------------|
    | <span data-ttu-id="ffb91-134">icon-80.png</span><span class="sxs-lookup"><span data-stu-id="ffb91-134">icon-80.png</span></span> | <span data-ttu-id="ffb91-135">80x80</span><span class="sxs-lookup"><span data-stu-id="ffb91-135">80x80</span></span>          |
    | <span data-ttu-id="ffb91-136">icon-32.png</span><span class="sxs-lookup"><span data-stu-id="ffb91-136">icon-32.png</span></span> | <span data-ttu-id="ffb91-137">32x32</span><span class="sxs-lookup"><span data-stu-id="ffb91-137">32x32</span></span>          |
    | <span data-ttu-id="ffb91-138">icon-16.png</span><span class="sxs-lookup"><span data-stu-id="ffb91-138">icon-16.png</span></span> | <span data-ttu-id="ffb91-139">16x16</span><span class="sxs-lookup"><span data-stu-id="ffb91-139">16x16</span></span>          |

    > [!NOTE]
    > <span data-ttu-id="ffb91-140">Puede usar cualquier imagen que desee para este paso.</span><span class="sxs-lookup"><span data-stu-id="ffb91-140">You can use any image you want for this step.</span></span> <span data-ttu-id="ffb91-141">También puede descargar las imágenes usadas en este ejemplo directamente desde [GitHub](https://github.com/microsoftgraph/msgraph-training-office-addin/demo/graph-tutorial/src/addin/assets).</span><span class="sxs-lookup"><span data-stu-id="ffb91-141">You can also download the images used in this sample directly from [GitHub](https://github.com/microsoftgraph/msgraph-training-office-addin/demo/graph-tutorial/src/addin/assets).</span></span>

1. <span data-ttu-id="ffb91-142">Cree un nuevo directorio en la raíz del proyecto denominado **manifiesto**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-142">Create a new directory in the root of the project named **manifest**.</span></span>

1. <span data-ttu-id="ffb91-143">Cree un nuevo archivo denominado **manifest.xml** en la **carpeta ./manifest** y agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ffb91-143">Create a new file named **manifest.xml** in the **./manifest** folder and add the following code.</span></span> <span data-ttu-id="ffb91-144">Reemplace `NEW_GUID_HERE` por un NUEVO GUID, como `b4fa03b8-1eb6-4e8b-a380-e0476be9e019` .</span><span class="sxs-lookup"><span data-stu-id="ffb91-144">Replace `NEW_GUID_HERE` with a new GUID, like `b4fa03b8-1eb6-4e8b-a380-e0476be9e019`.</span></span>

    :::code language="xml" source="../demo/graph-tutorial/manifest/manifest.xml":::

## <a name="side-load-the-add-in-in-excel"></a><span data-ttu-id="ffb91-145">Carga lateral del complemento en Excel</span><span class="sxs-lookup"><span data-stu-id="ffb91-145">Side-load the add-in in Excel</span></span>

1. <span data-ttu-id="ffb91-146">Inicie el servidor ejecutando el siguiente comando.</span><span class="sxs-lookup"><span data-stu-id="ffb91-146">Start the server by running the following command.</span></span>

    ```Shell
    yarn start
    ```

1. <span data-ttu-id="ffb91-147">Abra el explorador y vaya a `https://localhost:3000/taskpane.html` .</span><span class="sxs-lookup"><span data-stu-id="ffb91-147">Open your browser and browse to `https://localhost:3000/taskpane.html`.</span></span> <span data-ttu-id="ffb91-148">Debería ver un `Not loaded` mensaje.</span><span class="sxs-lookup"><span data-stu-id="ffb91-148">You should see a `Not loaded` message.</span></span>

1. <span data-ttu-id="ffb91-149">En el explorador, vaya a [Office.com](https://www.office.com/) e inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="ffb91-149">In your browser, go to [Office.com](https://www.office.com/) and sign in.</span></span> <span data-ttu-id="ffb91-150">Seleccione **Crear en** la barra de herramientas de la izquierda y, a continuación, seleccione Hoja de **cálculo**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-150">Select **Create** in the left-hand toolbar, then select **Spreadsheet**.</span></span>

    ![Captura de pantalla del menú Crear en Office.com](images/office-select-excel.png)

1. <span data-ttu-id="ffb91-152">Seleccione la **pestaña** Insertar y, a continuación, seleccione **Complementos de Office**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-152">Select the **Insert** tab, then select **Office Add-ins**.</span></span>

1. <span data-ttu-id="ffb91-153">Seleccione **Cargar mi complemento y,** a continuación, seleccione **Examinar**.</span><span class="sxs-lookup"><span data-stu-id="ffb91-153">Select **Upload My Add-in**, then select **Browse**.</span></span> <span data-ttu-id="ffb91-154">Cargue el **archivo ./manifest/manifest.xml.**</span><span class="sxs-lookup"><span data-stu-id="ffb91-154">Upload your **./manifest/manifest.xml** file.</span></span>

1. <span data-ttu-id="ffb91-155">Seleccione el **botón Importar calendario** de la ficha **Inicio** para abrir el panel de tareas.</span><span class="sxs-lookup"><span data-stu-id="ffb91-155">Select the **Import Calendar** button on the **Home** tab to open the taskpane.</span></span>

    ![Captura de pantalla del botón Importar calendario en la pestaña Inicio](images/get-started.png)

1. <span data-ttu-id="ffb91-157">Después de que se abra el panel de tareas, debería ver un `Hello World!` mensaje.</span><span class="sxs-lookup"><span data-stu-id="ffb91-157">After the taskpane opens, you should see a `Hello World!` message.</span></span>

    ![Una captura de pantalla del mensaje Hello World](images/hello-world.png)
