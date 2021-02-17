---
ms.openlocfilehash: ade142f1518d9bd56fa1472889721a4c8995bc3c
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274321"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="de4dc-101">En este ejercicio incorporará Microsoft Graph a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="de4dc-101">In this exercise you will incorporate Microsoft Graph into the application.</span></span> <span data-ttu-id="de4dc-102">Para esta aplicación, usará la biblioteca [de microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="de4dc-102">For this application, you will use the [microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) library to make calls to Microsoft Graph.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="de4dc-103">Obtener eventos de calendario de Outlook</span><span class="sxs-lookup"><span data-stu-id="de4dc-103">Get calendar events from Outlook</span></span>

<span data-ttu-id="de4dc-104">Empiece por agregar una API para obtener una [vista de calendario](https://docs.microsoft.com/graph/api/user-list-calendarview) del calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="de4dc-104">Start by adding an API to get a [calendar view](https://docs.microsoft.com/graph/api/user-list-calendarview) from the user's calendar.</span></span>

1. <span data-ttu-id="de4dc-105">Abra **./src/api/graph.ts** y agregue las siguientes instrucciones en `import` la parte superior del archivo.</span><span class="sxs-lookup"><span data-stu-id="de4dc-105">Open **./src/api/graph.ts** and add the following `import` statements to the top of the file.</span></span>

    ```typescript
    import { zonedTimeToUtc } from 'date-fns-tz';
    import { findOneIana } from 'windows-iana';
    import * as graph from '@microsoft/microsoft-graph-client';
    import { Event, MailboxSettings } from 'microsoft-graph';
    import 'isomorphic-fetch';
    import { getTokenOnBehalfOf } from './auth';
    ```

1. <span data-ttu-id="de4dc-106">Agregue la siguiente función para inicializar el SDK de Microsoft Graph y devolver un **cliente**.</span><span class="sxs-lookup"><span data-stu-id="de4dc-106">Add the following function to initialize the Microsoft Graph SDK and return a **Client**.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetClientSnippet":::

1. <span data-ttu-id="de4dc-107">Agregue la siguiente función para obtener la zona horaria del usuario de su configuración de buzón de correo y para convertir ese valor en un identificador de zona horaria IANA.</span><span class="sxs-lookup"><span data-stu-id="de4dc-107">Add the following function to get the user's time zone from their mailbox settings, and to convert that value to an IANA time zone identifier.</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetTimeZonesSnippet":::

1. <span data-ttu-id="de4dc-108">Agregue la siguiente función (debajo de la `const graphRouter = Router();` línea) para implementar un punto de conexión de API ( `GET /graph/calendarview` ).</span><span class="sxs-lookup"><span data-stu-id="de4dc-108">Add the following function (below the `const graphRouter = Router();` line) to implement an API endpoint (`GET /graph/calendarview`).</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetCalendarViewSnippet":::

    <span data-ttu-id="de4dc-109">Ten en cuenta lo que hace este código.</span><span class="sxs-lookup"><span data-stu-id="de4dc-109">Consider what this code does.</span></span>

    - <span data-ttu-id="de4dc-110">Obtiene la zona horaria del usuario y la usa para convertir el inicio y el final de la vista de calendario solicitada en valores UTC.</span><span class="sxs-lookup"><span data-stu-id="de4dc-110">It gets the user's time zone and uses that to convert the start and end of the requested calendar view into UTC values.</span></span>
    - <span data-ttu-id="de4dc-111">Realiza una llamada al `GET` punto de conexión de la API de `/me/calendarview` Graph.</span><span class="sxs-lookup"><span data-stu-id="de4dc-111">It does a `GET` to the `/me/calendarview` Graph API endpoint.</span></span>
        - <span data-ttu-id="de4dc-112">Usa la función para establecer el encabezado, lo que hace que las horas de inicio y finalización de los eventos devueltos se ajusten a la zona horaria `header` `Prefer: outlook.timezone` del usuario.</span><span class="sxs-lookup"><span data-stu-id="de4dc-112">It uses the `header` function to set the `Prefer: outlook.timezone` header, causing the start and end times of the returned events to be adjusted to the user's time zone.</span></span>
        - <span data-ttu-id="de4dc-113">Usa la función `query` para agregar los parámetros `startDateTime` `endDateTime` and, estableciendo el inicio y el final de la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="de4dc-113">It uses the `query` function to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
        - <span data-ttu-id="de4dc-114">Usa la función `select` para solicitar solo los campos usados por el complemento.</span><span class="sxs-lookup"><span data-stu-id="de4dc-114">It uses the `select` function to request only the fields used by the add-in.</span></span>
        - <span data-ttu-id="de4dc-115">Usa la función `orderby` para ordenar los resultados por hora de inicio.</span><span class="sxs-lookup"><span data-stu-id="de4dc-115">It uses the `orderby` function to sort the results by the start time.</span></span>
        - <span data-ttu-id="de4dc-116">Usa la función `top` para limitar los resultados en una sola solicitud a 25.</span><span class="sxs-lookup"><span data-stu-id="de4dc-116">It uses the `top` function to limit the results in a single request to 25.</span></span>
    - <span data-ttu-id="de4dc-117">Usa un objeto **PageIteratorCallback** para recorrer en [iteración](https://docs.microsoft.com/graph/sdks/paging) los resultados y realizar solicitudes adicionales si hay más páginas de resultados disponibles.</span><span class="sxs-lookup"><span data-stu-id="de4dc-117">It uses a **PageIteratorCallback** object to [iterate through the results](https://docs.microsoft.com/graph/sdks/paging) and to make additional requests if more pages of results are available.</span></span>

## <a name="update-the-ui"></a><span data-ttu-id="de4dc-118">Actualizar la interfaz de usuario</span><span class="sxs-lookup"><span data-stu-id="de4dc-118">Update the UI</span></span>

<span data-ttu-id="de4dc-119">Ahora vamos a actualizar el panel de tareas para permitir al usuario especificar una fecha de inicio y finalización para la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="de4dc-119">Now let's update the task pane to allow the user to specify a start and end date for the calendar view.</span></span>

1. <span data-ttu-id="de4dc-120">Abra **./src/addin/taskpane.js** y reemplace la función `showMainUi` existente por lo siguiente.</span><span class="sxs-lookup"><span data-stu-id="de4dc-120">Open **./src/addin/taskpane.js** and replace the existing `showMainUi` function with the following.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="MainUiSnippet":::

    <span data-ttu-id="de4dc-121">Este código agrega un formulario simple para que el usuario pueda especificar una fecha de inicio y finalización.</span><span class="sxs-lookup"><span data-stu-id="de4dc-121">This code adds a simple form so the user can specify a start and end date.</span></span> <span data-ttu-id="de4dc-122">También implementa un segundo formulario para crear un nuevo evento.</span><span class="sxs-lookup"><span data-stu-id="de4dc-122">It also implements a second form for creating a new event.</span></span> <span data-ttu-id="de4dc-123">Ese formulario no hace nada por ahora, implementará esa característica en la siguiente sección.</span><span class="sxs-lookup"><span data-stu-id="de4dc-123">That form doesn't do anything for now, you'll implement that feature in the next section.</span></span>

1. <span data-ttu-id="de4dc-124">Agregue el siguiente código al archivo para crear una tabla en la hoja de cálculo activa que contenga los eventos recuperados de la vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="de4dc-124">Add the following code to the file to create a table in the active worksheet containing the events retrieved from the calendar view.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="WriteToSheetSnippet":::

1. <span data-ttu-id="de4dc-125">Agregue la siguiente función para llamar a la API de vista de calendario.</span><span class="sxs-lookup"><span data-stu-id="de4dc-125">Add the following function to call the calendar view API.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="GetCalendarSnippet":::

1. <span data-ttu-id="de4dc-126">Guarde todos los cambios, reinicie el servidor y actualice el panel de tareas en Excel (cierre los paneles de tareas abiertos y vuelva a abrir).</span><span class="sxs-lookup"><span data-stu-id="de4dc-126">Save all of your changes, restart the server, and refresh the task pane in Excel (close any open task panes and re-open).</span></span>

    ![Captura de pantalla del formulario de importación](images/get-calendar-view-ui.png)

1. <span data-ttu-id="de4dc-128">Elija fechas de inicio y finalización y elija **Importar**.</span><span class="sxs-lookup"><span data-stu-id="de4dc-128">Choose start and end dates and choose **Import**.</span></span>

    ![Captura de pantalla de la tabla de eventos](images/calendar-view-table.png)
