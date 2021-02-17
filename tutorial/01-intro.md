---
ms.openlocfilehash: 2c323d61632c62c82af0561536656f1d68fe8938
ms.sourcegitcommit: 24bb4b3df6a035806a58b609e1d8078ac5505fa1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/17/2021
ms.locfileid: "50274375"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="0559b-101">En este tutorial se explica cómo crear un complemento de Office para Excel que use la API de Microsoft Graph para recuperar información de calendario de un usuario.</span><span class="sxs-lookup"><span data-stu-id="0559b-101">This tutorial teaches you how to build an Office Add-in for Excel that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="0559b-102">Si prefiere descargar el tutorial completado, puede descargar o clonar el [repositorio de GitHub.](https://github.com/microsoftgraph/msgraph-training-office-addin)</span><span class="sxs-lookup"><span data-stu-id="0559b-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-office-addin).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0559b-103">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="0559b-103">Prerequisites</span></span>

<span data-ttu-id="0559b-104">Antes de iniciar esta demostración, debes tener [ instaladosNode.js](https://nodejs.org) [y Desastados](https://yarnpkg.com/) en el equipo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="0559b-104">Before you start this demo, you should have [Node.js](https://nodejs.org) and [Yarn](https://yarnpkg.com/) installed on your development machine.</span></span> <span data-ttu-id="0559b-105">Si no tiene opciones de Node.js o Desvase, visite el vínculo anterior para ver las opciones de descarga.</span><span class="sxs-lookup"><span data-stu-id="0559b-105">If you do not have Node.js or Yarn, visit the previous link for download options.</span></span>

> [!NOTE]
> <span data-ttu-id="0559b-106">Es posible que los usuarios de Windows deban instalar Python y Visual Studio Build Tools para admitir módulos NPM que deben compilarse desde C/C++.</span><span class="sxs-lookup"><span data-stu-id="0559b-106">Windows users may need to install Python and Visual Studio Build Tools to support NPM modules that need to be compiled from C/C++.</span></span> <span data-ttu-id="0559b-107">El Node.js en Windows ofrece una opción para instalar automáticamente estas herramientas.</span><span class="sxs-lookup"><span data-stu-id="0559b-107">The Node.js installer on Windows gives an option to automatically install these tools.</span></span> <span data-ttu-id="0559b-108">Como alternativa, puede seguir las instrucciones en [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows) .</span><span class="sxs-lookup"><span data-stu-id="0559b-108">Alternatively, you can follow instructions at [https://github.com/nodejs/node-gyp#on-windows](https://github.com/nodejs/node-gyp#on-windows).</span></span>

<span data-ttu-id="0559b-109">También debe tener una cuenta personal de Microsoft con un buzón en Outlook.com o una cuenta de Microsoft para el trabajo o la escuela.</span><span class="sxs-lookup"><span data-stu-id="0559b-109">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="0559b-110">Si no tienes una cuenta de Microsoft, hay un par de opciones para obtener una cuenta gratuita:</span><span class="sxs-lookup"><span data-stu-id="0559b-110">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="0559b-111">Puede registrarse [para obtener una nueva cuenta personal de Microsoft.](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1)</span><span class="sxs-lookup"><span data-stu-id="0559b-111">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="0559b-112">Puede registrarse en el Programa de desarrolladores de [Office 365](https://developer.microsoft.com/office/dev-program) para obtener una suscripción gratuita a Office 365.</span><span class="sxs-lookup"><span data-stu-id="0559b-112">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="0559b-113">Este tutorial se escribió con Node versión 14.15.0 y La versión 1.22.0 de Desambre.</span><span class="sxs-lookup"><span data-stu-id="0559b-113">This tutorial was written with Node version 14.15.0 and Yarn version 1.22.0.</span></span> <span data-ttu-id="0559b-114">Los pasos de esta guía pueden funcionar con otras versiones, pero no se han probado.</span><span class="sxs-lookup"><span data-stu-id="0559b-114">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="0559b-115">Comentarios</span><span class="sxs-lookup"><span data-stu-id="0559b-115">Feedback</span></span>

<span data-ttu-id="0559b-116">Envíe sus comentarios sobre este tutorial en el [repositorio de GitHub.](https://github.com/microsoftgraph/msgraph-training-office-addin)</span><span class="sxs-lookup"><span data-stu-id="0559b-116">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-office-addin).</span></span>
