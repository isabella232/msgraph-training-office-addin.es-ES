---
ms.openlocfilehash: facdbb5c42e60e5bb0ee98b06ef68939a6010a67
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274320"
---
<!-- markdownlint-disable MD002 MD041 -->

En esta sección agregará la capacidad de crear eventos en el calendario del usuario.

## <a name="implement-the-api"></a>Implementar la API

1. Abra **./src/api/graph.ts** y agregue el siguiente código para implementar una nueva API de eventos ( `POST /graph/newevent` ).

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="CreateEventSnippet":::

1. Abra **./src/addin/taskpane.js** y agregue la siguiente función para llamar a la nueva API de eventos.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="CreateEventSnippet":::

1. Guarde todos los cambios, reinicie el servidor y actualice el panel de tareas en Excel (cierre los paneles de tareas abiertos y vuelva a abrir).

    ![Captura de pantalla del formulario de creación de eventos](images/create-event-ui.png)

1. Rellene el formulario y elija **Crear**. Compruebe que el evento se agrega al calendario del usuario.
