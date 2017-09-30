---
title: "Introducción a las páginas de Razor en ASP.NET Core"
author: rick-anderson
description: "Introducción a las páginas de Razor en ASP.NET Core"
keywords: "ASP.NET Core, páginas de Razor, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 8c6e281e761e69908fc742d1f19c14a00de4bd46
ms.sourcegitcommit: 67f54fabbfa4e3942f5bfe1f8a7fdfe4a7a75358
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="86a10-104">Introducción a las páginas de Razor en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="86a10-104">Getting started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="86a10-105">Por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86a10-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="86a10-106">En este tutorial se enseñan los conceptos básicos de la compilación de una aplicación web de páginas de Razor de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86a10-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="86a10-107">Se recomienda leer [Introduction to Razor Pages](xref:mvc/razor-pages/index) (Introducción a las páginas de Razor) antes de empezar este tutorial.</span><span class="sxs-lookup"><span data-stu-id="86a10-107">We recommend you complete [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="86a10-108">Las páginas de Razor son el método recomendado para crear la interfaz de usuario de aplicaciones web en ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86a10-108">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="86a10-109">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="86a10-109">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="86a10-110">Creación de una aplicación web de Razor</span><span class="sxs-lookup"><span data-stu-id="86a10-110">Create a Razor web app</span></span>

* <span data-ttu-id="86a10-111">En el menú **Archivo** de Visual Studio, seleccione **Nuevo > Proyecto**.</span><span class="sxs-lookup"><span data-stu-id="86a10-111">From the Visual Studio **File** menu, select **New > Project**.</span></span>
* <span data-ttu-id="86a10-112">Cree una aplicación web de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="86a10-112">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="86a10-113">Asigne al proyecto el nombre **RazorPagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="86a10-113">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="86a10-114">Es importante asignarle el nombre *RazorPagesMovie* para que los espacios de nombres coincidan al copiar y pegar el código.</span><span class="sxs-lookup"><span data-stu-id="86a10-114">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
 <span data-ttu-id="86a10-115">![Nueva aplicación web de ASP.NET Core](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="86a10-115">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="86a10-116">Seleccione **ASP.NET Core 2.0** en la lista desplegable y, luego, seleccione **Aplicación web**.</span><span class="sxs-lookup"><span data-stu-id="86a10-116">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="86a10-117">![Aplicación web (páginas de Razor)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="86a10-117">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="86a10-118">La plantilla de Visual Studio crea un proyecto de inicio:</span><span class="sxs-lookup"><span data-stu-id="86a10-118">The Visual Studio template creates a starter project:</span></span>

![Explorador de soluciones](razor-pages-start/_static/se.png)

<span data-ttu-id="86a10-120">Presione **F5** para ejecutar la aplicación en modo de depuración o **Ctrl-F5** para que se ejecute sin adjuntar el depurador.</span><span class="sxs-lookup"><span data-stu-id="86a10-120">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Página Inicio o Índice](razor-pages-start/_static/home.png)

* <span data-ttu-id="86a10-122">Visual Studio inicia [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="86a10-122">Visual Studio starts [IIS Express](https://docs.microsoft.com/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="86a10-123">En la barra de direcciones aparece `localhost:port#` (y no algo como `example.com`).</span><span class="sxs-lookup"><span data-stu-id="86a10-123">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="86a10-124">Esto es así porque `localhost` es el nombre de host estándar del equipo local.</span><span class="sxs-lookup"><span data-stu-id="86a10-124">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="86a10-125">Localhost solo sirve las solicitudes web del equipo local.</span><span class="sxs-lookup"><span data-stu-id="86a10-125">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="86a10-126">Cuando Visual Studio crea un proyecto web, se usa un puerto aleatorio para el servidor web.</span><span class="sxs-lookup"><span data-stu-id="86a10-126">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="86a10-127">En la imagen anterior, el número de puerto es 5000.</span><span class="sxs-lookup"><span data-stu-id="86a10-127">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="86a10-128">Al ejecutar la aplicación verá otro número de puerto.</span><span class="sxs-lookup"><span data-stu-id="86a10-128">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="86a10-129">Iniciar la aplicación con **CTRL+F5** (modo de no depuración) le permite efectuar cambios en el código, guardar el archivo, actualizar el explorador y ver los cambios de código.</span><span class="sxs-lookup"><span data-stu-id="86a10-129">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="86a10-130">Muchos desarrolladores prefieren usar el modo de no depuración para iniciar la aplicación rápidamente y ver los cambios.</span><span class="sxs-lookup"><span data-stu-id="86a10-130">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

>[!div class="step-by-step"]
[<span data-ttu-id="86a10-131">Siguiente: Adición de un modelo</span><span class="sxs-lookup"><span data-stu-id="86a10-131">Next: Adding a model</span></span>](xref:tutorials/razor-pages/modelz)  