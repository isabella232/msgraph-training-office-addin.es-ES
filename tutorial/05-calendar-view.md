---
ms.openlocfilehash: bd5901525561f7e169f2b7c71a280883dac8c762
ms.sourcegitcommit: 8a65c826f6b229c287a782d784b6d9629aa5a3d0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/20/2021
ms.locfileid: "51899418"
---
<!-- markdownlint-disable MD002 MD041 -->

En este ejercicio incorporará Microsoft Graph a la aplicación. Para esta aplicación, usará la biblioteca [de microsoft-graph-client](https://github.com/microsoftgraph/msgraph-sdk-javascript) para realizar llamadas a Microsoft Graph.

## <a name="get-calendar-events-from-outlook"></a>Obtener eventos del calendario desde Outlook

Empiece agregando una API para obtener una [vista de calendario](https://docs.microsoft.com/graph/api/user-list-calendarview) del calendario del usuario.

1. Abra **./src/api/graph.ts** y agregue las siguientes `import` instrucciones a la parte superior del archivo.

    ```typescript
    import { zonedTimeToUtc } from 'date-fns-tz';
    import { findIana } from 'windows-iana';
    import * as graph from '@microsoft/microsoft-graph-client';
    import { Event, MailboxSettings } from 'microsoft-graph';
    import 'isomorphic-fetch';
    import { getTokenOnBehalfOf } from './auth';
    ```

1. Agregue la siguiente función para inicializar el SDK de Microsoft Graph y devolver un **objeto Client**.

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetClientSnippet":::

1. Agregue la siguiente función para obtener la zona horaria del usuario de su configuración de buzón de correo y para convertir ese valor en un identificador de zona horaria IANA.

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetTimeZonesSnippet":::

1. Agregue la siguiente función (debajo de la `const graphRouter = Router();` línea) para implementar un extremo de API ( `GET /graph/calendarview` ).

    :::code language="typescript" source="../demo/graph-tutorial/src/api/graph.ts" id="GetCalendarViewSnippet":::

    Tenga en cuenta lo que hace este código.

    - Obtiene la zona horaria del usuario y la usa para convertir el inicio y el final de la vista de calendario solicitada en valores UTC.
    - Realiza una aplicación `GET` al punto de conexión de la API de `/me/calendarview` Graph.
        - Usa la función para establecer el encabezado, lo que hace que las horas de inicio y finalización de los eventos devueltos se ajusten a la zona `header` `Prefer: outlook.timezone` horaria del usuario.
        - Usa la función `query` para agregar los parámetros `startDateTime` `endDateTime` and, estableciendo el inicio y el final de la vista de calendario.
        - Usa la `select` función para solicitar solo los campos usados por el complemento.
        - Usa la función `orderby` para ordenar los resultados por hora de inicio.
        - Usa la `top` función para limitar los resultados de una sola solicitud a 25.
    - Usa un **objeto PageIteratorCallback** para [iterar](https://docs.microsoft.com/graph/sdks/paging) los resultados y realizar solicitudes adicionales si hay más páginas de resultados disponibles.

## <a name="update-the-ui"></a>Actualizar la interfaz de usuario

Ahora vamos a actualizar el panel de tareas para permitir al usuario especificar una fecha de inicio y finalización para la vista de calendario.

1. Abra **./src/addin/taskpane.js** y reemplace la función `showMainUi` existente por la siguiente.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="MainUiSnippet":::

    Este código agrega un formulario simple para que el usuario pueda especificar una fecha de inicio y finalización. También implementa un segundo formulario para crear un nuevo evento. Ese formulario no hace nada por ahora, implementará esa característica en la siguiente sección.

1. Agregue el siguiente código al archivo para crear una tabla en la hoja de cálculo activa que contenga los eventos recuperados de la vista de calendario.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="WriteToSheetSnippet":::

1. Agregue la siguiente función para llamar a la API de vista de calendario.

    :::code language="javascript" source="../demo/graph-tutorial/src/addin/taskpane.js" id="GetCalendarSnippet":::

1. Guarde todos los cambios, reinicie el servidor y actualice el panel de tareas en Excel (cierre los paneles de tareas abiertos y vuelva a abrir).

    ![Captura de pantalla del formulario de importación](images/get-calendar-view-ui.png)

1. Elija fechas de inicio y finalización y elija **Importar**.

    ![Una captura de pantalla de la tabla de eventos](images/calendar-view-table.png)
