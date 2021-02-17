---
ms.openlocfilehash: facdbb5c42e60e5bb0ee98b06ef68939a6010a67
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274320"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="b9613-101">En esta sección agregará la capacidad de crear eventos en el calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="b9613-101">In this section you will add the ability to create events on the user's calendar.</span></span>

## <a name="implement-the-api"></a><span data-ttu-id="b9613-102">Implementar la API</span><span class="sxs-lookup"><span data-stu-id="b9613-102">Implement the API</span></span>

1. <span data-ttu-id="b9613-103">Abra **./src/api/graph.ts** y agregue el siguiente código para implementar una nueva API de eventos ( `POST /graph/newevent` ).</span><span class="sxs-lookup"><span data-stu-id="b9613-103">Open **./src/api/graph.ts** and add the following code to implement a new event API (`POST /graph/newevent`).</span></span>

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="CreateEventSnippet":::

1. <span data-ttu-id="b9613-104">Abra **./src/addin/taskpane.js** y agregue la siguiente función para llamar a la nueva API de eventos.</span><span class="sxs-lookup"><span data-stu-id="b9613-104">Open **./src/addin/taskpane.js** and add the following function to call the new event API.</span></span>

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="CreateEventSnippet":::

1. <span data-ttu-id="b9613-105">Guarde todos los cambios, reinicie el servidor y actualice el panel de tareas en Excel (cierre los paneles de tareas abiertos y vuelva a abrir).</span><span class="sxs-lookup"><span data-stu-id="b9613-105">Save all of your changes, restart the server, and refresh the task pane in Excel (close any open task panes and re-open).</span></span>

    ![Captura de pantalla del formulario de creación de eventos](images/create-event-ui.png)

1. <span data-ttu-id="b9613-107">Rellene el formulario y elija **Crear**.</span><span class="sxs-lookup"><span data-stu-id="b9613-107">Fill in the form and choose **Create**.</span></span> <span data-ttu-id="b9613-108">Compruebe que el evento se agrega al calendario del usuario.</span><span class="sxs-lookup"><span data-stu-id="b9613-108">Verify that the event is added to the user's calendar.</span></span>
