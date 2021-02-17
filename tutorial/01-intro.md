---
ms.openlocfilehash: 2c323d61632c62c82af0561536656f1d68fe8938
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274375"
---
<!-- markdownlint-disable MD002 MD041 -->

En este tutorial se explica cómo crear un complemento de Office para Excel que use la API de Microsoft Graph para recuperar información de calendario de un usuario.

> [!TIP]
> Si prefiere descargar el tutorial completado, puede descargar o clonar el [repositorio de GitHub.](https://github.com/microsoftgraph/msgraph-training-office-addin)

## <a name="prerequisites"></a>Requisitos previos

Antes de iniciar esta demostración, debes tener [ instaladosNode.js](https://nodejs.org) [y Desastados](https://yarnpkg.com/) en el equipo de desarrollo. Si no tiene opciones de Node.js o Desvase, visite el vínculo anterior para ver las opciones de descarga.

> [!NOTE]
> Es posible que los usuarios de Windows deban instalar Python y Visual Studio Build Tools para admitir módulos NPM que deben compilarse desde C/C++. El Node.js en Windows ofrece una opción para instalar automáticamente estas herramientas. Como alternativa, puede seguir las instrucciones en [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) .

También debe tener una cuenta personal de Microsoft con un buzón en Outlook.com o una cuenta de Microsoft para el trabajo o la escuela. Si no tienes una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:

- Puede registrarse [para obtener una nueva cuenta personal de Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)
- Puede registrarse en el Programa de desarrolladores de [Office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.

> [!NOTE]
> Este tutorial se escribió con Node versión 14.15.0 y La versión 1.22.0 de Desambre. Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.

## <a name="feedback"></a>Comentarios

Envíe sus comentarios sobre este tutorial en el [repositorio de GitHub.](https://github.com/microsoftgraph/msgraph-training-office-addin)
