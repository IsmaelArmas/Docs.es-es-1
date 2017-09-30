---
title: "Usar JavaScriptServices para crear aplicaciones de una sola página"
author: scottaddie
description: "Obtenga información acerca de las ventajas de usar JavaScriptServices para crear una aplicación de página única (SPA) respaldado por ASP.NET Core."
keywords: "Núcleo de ASP.NET, Angular, SPA, JavaScriptServices, SpaServices"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/spa-services
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0dbe5e3567bb4feea7e56cff3dcf75be5df88310
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="using-javascriptservices-for-creating-single-page-applications-with-aspnet-core"></a><span data-ttu-id="cdea7-104">Usar JavaScriptServices para crear aplicaciones de una página con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cdea7-104">Using JavaScriptServices for Creating Single Page Applications with ASP.NET Core</span></span>

<span data-ttu-id="cdea7-105">Por [Scott Addie](https://github.com/scottaddie) y [Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="cdea7-105">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="cdea7-106">Una aplicación de página única (SPA) es un tipo conocido de aplicación web debido a su experiencia de usuario enriquecida inherente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-106">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="cdea7-107">Integración de cliente marcos SPA o bibliotecas, como [Angular](https://angular.io/) o [reaccionar](https://facebook.github.io/react/), con marcos de trabajo de servidor como ASP.NET Core puede ser difícil.</span><span class="sxs-lookup"><span data-stu-id="cdea7-107">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="cdea7-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) se desarrolló para reducir la fricción en el proceso de integración.</span><span class="sxs-lookup"><span data-stu-id="cdea7-108">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="cdea7-109">Permite la operación sin problemas entre los distintos clientes y pilas de tecnología de servidor.</span><span class="sxs-lookup"><span data-stu-id="cdea7-109">It enables seamless operation between the different client and server technology stacks.</span></span>

[<span data-ttu-id="cdea7-110">Ver o descargar el código de ejemplo</span><span class="sxs-lookup"><span data-stu-id="cdea7-110">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample)

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="cdea7-111">¿Qué es JavaScriptServices?</span><span class="sxs-lookup"><span data-stu-id="cdea7-111">What is JavaScriptServices?</span></span>

<span data-ttu-id="cdea7-112">JavaScriptServices es un conjunto de tecnologías de cliente para ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cdea7-112">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="cdea7-113">Su objetivo consiste en colocar ASP.NET Core como plataforma de servidor preferido de los desarrolladores para compilar SPAs.</span><span class="sxs-lookup"><span data-stu-id="cdea7-113">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="cdea7-114">JavaScriptServices consta de tres paquetes de NuGet distintos:</span><span class="sxs-lookup"><span data-stu-id="cdea7-114">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="cdea7-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="cdea7-115">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="cdea7-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="cdea7-116">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="cdea7-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="cdea7-117">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="cdea7-118">Estos paquetes son útiles si se:</span><span class="sxs-lookup"><span data-stu-id="cdea7-118">These packages are useful if you:</span></span>
* <span data-ttu-id="cdea7-119">Ejecutar JavaScript en el servidor</span><span class="sxs-lookup"><span data-stu-id="cdea7-119">Run JavaScript on the server</span></span>
* <span data-ttu-id="cdea7-120">Usar un marco de SPA o biblioteca</span><span class="sxs-lookup"><span data-stu-id="cdea7-120">Use a SPA framework or library</span></span>
* <span data-ttu-id="cdea7-121">Compilar recursos de lado cliente con Webpack</span><span class="sxs-lookup"><span data-stu-id="cdea7-121">Build client-side assets with Webpack</span></span>

<span data-ttu-id="cdea7-122">Gran parte de la atención en este artículo se coloca sobre cómo usar el paquete SpaServices.</span><span class="sxs-lookup"><span data-stu-id="cdea7-122">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="cdea7-123">¿Qué es SpaServices?</span><span class="sxs-lookup"><span data-stu-id="cdea7-123">What is SpaServices?</span></span>

<span data-ttu-id="cdea7-124">Se creó para colocar ASP.NET Core como plataforma de servidor preferido de los desarrolladores para compilar SPAs SpaServices.</span><span class="sxs-lookup"><span data-stu-id="cdea7-124">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="cdea7-125">No es necesario desarrollar SPAs con ASP.NET Core SpaServices y no bloquea en un marco de trabajo de cliente en particular.</span><span class="sxs-lookup"><span data-stu-id="cdea7-125">SpaServices is not required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="cdea7-126">SpaServices ofrece una infraestructura útil como:</span><span class="sxs-lookup"><span data-stu-id="cdea7-126">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="cdea7-127">Procesamiento previo de servidor</span><span class="sxs-lookup"><span data-stu-id="cdea7-127">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="cdea7-128">Middleware de desarrollo Webpack</span><span class="sxs-lookup"><span data-stu-id="cdea7-128">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="cdea7-129">Sustitución del módulo de acceso rápido</span><span class="sxs-lookup"><span data-stu-id="cdea7-129">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="cdea7-130">Aplicaciones auxiliares de enrutamientos</span><span class="sxs-lookup"><span data-stu-id="cdea7-130">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="cdea7-131">Colectivamente, estos componentes de infraestructura mejoran el flujo de trabajo de desarrollo y la experiencia en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="cdea7-131">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="cdea7-132">Los componentes pueden adoptar individualmente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-132">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="cdea7-133">Requisitos previos para usar SpaServices</span><span class="sxs-lookup"><span data-stu-id="cdea7-133">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="cdea7-134">Para trabajar con SpaServices, instalar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-134">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="cdea7-135">[Node.js](https://nodejs.org/) (versión 6 o posterior) con npm</span><span class="sxs-lookup"><span data-stu-id="cdea7-135">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
    * <span data-ttu-id="cdea7-136">Para comprobar que estos componentes se instalan y se encuentra, ejecute lo siguiente desde la línea de comandos:</span><span class="sxs-lookup"><span data-stu-id="cdea7-136">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="cdea7-137">Nota: Si va a implementar en un sitio web de Azure, no es necesario hacer nada aquí &mdash; Node.js está instalada y disponible en los entornos de servidor.</span><span class="sxs-lookup"><span data-stu-id="cdea7-137">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* <span data-ttu-id="cdea7-138">[.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (o posterior)</span><span class="sxs-lookup"><span data-stu-id="cdea7-138">[.NET Core SDK](https://www.microsoft.com/net/download/core) 1.0 (or later)</span></span>
    * <span data-ttu-id="cdea7-139">Si está en Windows, esto se puede instalar mediante la selección de Visual Studio 2017 **.NET Core el desarrollo multiplataforma** carga de trabajo.</span><span class="sxs-lookup"><span data-stu-id="cdea7-139">If you're on Windows, this can be installed by selecting Visual Studio 2017's **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="cdea7-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) paquete de NuGet</span><span class="sxs-lookup"><span data-stu-id="cdea7-140">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="cdea7-141">Procesamiento previo de servidor</span><span class="sxs-lookup"><span data-stu-id="cdea7-141">Server-side prerendering</span></span>

<span data-ttu-id="cdea7-142">Una aplicación universal (también conocido como mismo formato) es una aplicación de JavaScript capaz de ejecutar tanto en el servidor y el cliente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-142">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="cdea7-143">Angular, reaccionar y otros entornos populares proporciona una plataforma universal de este estilo de desarrollo de aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="cdea7-143">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="cdea7-144">La idea es representar primero los componentes de framework en el servidor a través de Node.js y, a continuación, más delegar la ejecución al cliente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-144">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="cdea7-145">ASP.NET Core [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) proporcionada por SpaServices simplificar la implementación de procesamiento previo de servidor mediante la invocación de las funciones de JavaScript en el servidor.</span><span class="sxs-lookup"><span data-stu-id="cdea7-145">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cdea7-146">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="cdea7-146">Prerequisites</span></span>

<span data-ttu-id="cdea7-147">Instalar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-147">Install the following:</span></span>
* <span data-ttu-id="cdea7-148">[ASPNET-procesamiento previo](https://www.npmjs.com/package/aspnet-prerendering) npm paquete:</span><span class="sxs-lookup"><span data-stu-id="cdea7-148">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="cdea7-149">Configuración</span><span class="sxs-lookup"><span data-stu-id="cdea7-149">Configuration</span></span>

<span data-ttu-id="cdea7-150">Las aplicaciones auxiliares de etiquetas se hacen reconocibles a través de registro del espacio de nombres en el proyecto *_ViewImports.cshtml* archivo:</span><span class="sxs-lookup"><span data-stu-id="cdea7-150">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

<span data-ttu-id="cdea7-151">[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-151">[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]</span></span>

<span data-ttu-id="cdea7-152">Estas aplicaciones auxiliares de etiquetas abstraer los detalles de comunicarse directamente con las API de bajo nivel mediante el aprovechamiento de una sintaxis similar a HTML dentro de la vista Razor:</span><span class="sxs-lookup"><span data-stu-id="cdea7-152">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

<span data-ttu-id="cdea7-153">[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-153">[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]</span></span>

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="cdea7-154">El `asp-prerender-module` auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="cdea7-154">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="cdea7-155">El `asp-prerender-module` se ejecuta la aplicación auxiliar de etiquetas, usado en el ejemplo de código anterior, *ClientApp/dist/main-server.js* en el servidor a través de Node.js.</span><span class="sxs-lookup"><span data-stu-id="cdea7-155">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="cdea7-156">Para no complicarlo, *main server.js* archivo es un artefacto de la tarea de transpilation TypeScript y JavaScript en el [Webpack](http://webpack.github.io/) proceso de compilación.</span><span class="sxs-lookup"><span data-stu-id="cdea7-156">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="cdea7-157">Webpack define un alias de punto de entrada de `main-server`; y cruce seguro del gráfico de dependencia para este alias comienza en el *ClientApp/arranque-server.ts* archivo:</span><span class="sxs-lookup"><span data-stu-id="cdea7-157">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

<span data-ttu-id="cdea7-158">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-158">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]</span></span>

<span data-ttu-id="cdea7-159">En el siguiente ejemplo Angular, el *ClientApp/arranque-server.ts* archivo utiliza el `createServerRenderer` función y `RenderResult` el tipo de la `aspnet-prerendering` paquete de npm para configurar la representación del servidor a través de Node.js.</span><span class="sxs-lookup"><span data-stu-id="cdea7-159">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="cdea7-160">El marcado HTML destinado para la representación del lado servidor se pasa a una llamada de función de la resolución, que se incluye en JavaScript fuertemente tipado `Promise` objeto.</span><span class="sxs-lookup"><span data-stu-id="cdea7-160">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="cdea7-161">El `Promise` es de importancia del objeto que proporciona asincrónicamente el marcado HTML a la página de inyección de código en el elemento del DOM al marcador de posición.</span><span class="sxs-lookup"><span data-stu-id="cdea7-161">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

<span data-ttu-id="cdea7-162">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-162">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]</span></span>

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="cdea7-163">El `asp-prerender-data` auxiliar de etiquetas</span><span class="sxs-lookup"><span data-stu-id="cdea7-163">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="cdea7-164">Cuando se acopla con el `asp-prerender-module` aplicación auxiliar de etiqueta, la `asp-prerender-data` etiqueta auxiliar puede utilizarse para pasar información contextual de la vista de Razor en el código de JavaScript del lado servidor.</span><span class="sxs-lookup"><span data-stu-id="cdea7-164">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="cdea7-165">Por ejemplo, el siguiente marcado pasa los datos de usuario en el `main-server` módulo:</span><span class="sxs-lookup"><span data-stu-id="cdea7-165">For example, the following markup passes user data to the `main-server` module:</span></span>

<span data-ttu-id="cdea7-166">[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-166">[!code-html[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]</span></span>

<span data-ttu-id="cdea7-167">Los datos recibidos `UserName` argumento se serializa utilizando el serializador JSON integrado y se almacena en la `params.data` objeto.</span><span class="sxs-lookup"><span data-stu-id="cdea7-167">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="cdea7-168">En el siguiente ejemplo Angular, los datos se utilizan para construir un saludo personalizado dentro de un `h1` elemento:</span><span class="sxs-lookup"><span data-stu-id="cdea7-168">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

<span data-ttu-id="cdea7-169">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-169">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]</span></span>

<span data-ttu-id="cdea7-170">Nota: Los nombres de propiedad pasados en aplicaciones auxiliares de etiquetas se representan con **PascalCase** notación.</span><span class="sxs-lookup"><span data-stu-id="cdea7-170">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="cdea7-171">Esto contrasta con JavaScript, donde los mismos nombres de propiedad se representan con **camelCase**.</span><span class="sxs-lookup"><span data-stu-id="cdea7-171">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="cdea7-172">La configuración predeterminada de la serialización de JSON es responsable de esta diferencia.</span><span class="sxs-lookup"><span data-stu-id="cdea7-172">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="cdea7-173">Para ampliar el ejemplo de código anterior, datos pueden pasarse desde el servidor a la vista por hidratación el `globals` propiedad proporcionada para el `resolve` función:</span><span class="sxs-lookup"><span data-stu-id="cdea7-173">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

<span data-ttu-id="cdea7-174">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-174">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]</span></span>

<span data-ttu-id="cdea7-175">El `postList` matriz definida dentro de la `globals` objeto se adjunta en el explorador global `window` objeto.</span><span class="sxs-lookup"><span data-stu-id="cdea7-175">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="cdea7-176">Esta activación variables en ámbito global elimina la duplicación de esfuerzos, especialmente ya que pertenece a la carga de los mismos datos una vez en el servidor y de nuevo en el cliente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-176">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![variable postList global adjuntado al objeto de ventana](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="cdea7-178">Middleware de desarrollo Webpack</span><span class="sxs-lookup"><span data-stu-id="cdea7-178">Webpack Dev Middleware</span></span>

<span data-ttu-id="cdea7-179">[Middleware de desarrollo Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html) incorpora un flujo de trabajo de desarrollo mejorado mediante el cual Webpack crea recursos a petición.</span><span class="sxs-lookup"><span data-stu-id="cdea7-179">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="cdea7-180">El middleware automáticamente compila y sirve de recursos del cliente cuando se vuelve a cargar una página en el explorador.</span><span class="sxs-lookup"><span data-stu-id="cdea7-180">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="cdea7-181">El método alternativo consiste en invocar manualmente Webpack a través de script de compilación del proyecto npm cuando cambia una dependencia de otro fabricante o el código personalizado.</span><span class="sxs-lookup"><span data-stu-id="cdea7-181">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="cdea7-182">Un npm Generar script en el *package.json* archivo se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-182">An npm build script in the *package.json* file is shown in the following example:</span></span>

<span data-ttu-id="cdea7-183">[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-183">[!code-json[Main](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cdea7-184">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="cdea7-184">Prerequisites</span></span>

<span data-ttu-id="cdea7-185">Instalar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-185">Install the following:</span></span>
* <span data-ttu-id="cdea7-186">[ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) npm paquete:</span><span class="sxs-lookup"><span data-stu-id="cdea7-186">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="cdea7-187">Configuración</span><span class="sxs-lookup"><span data-stu-id="cdea7-187">Configuration</span></span>

<span data-ttu-id="cdea7-188">Middleware de desarrollo Webpack está registrado en la canalización de solicitudes HTTP mediante el siguiente código en el *Startup.cs* del archivo `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="cdea7-188">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

<span data-ttu-id="cdea7-189">[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-189">[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]</span></span>

<span data-ttu-id="cdea7-190">El `UseWebpackDevMiddleware` método de extensión se debe llamar antes [registrar archivos estáticos hospedaje](xref:fundamentals/static-files) a través de la `UseStaticFiles` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="cdea7-190">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="cdea7-191">Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="cdea7-191">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="cdea7-192">El *webpack.config.js* del archivo `output.publicPath` propiedad indica el middleware para observar el `dist` carpeta cambios:</span><span class="sxs-lookup"><span data-stu-id="cdea7-192">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

<span data-ttu-id="cdea7-193">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-193">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]</span></span>

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="cdea7-194">Sustitución del módulo de acceso rápido</span><span class="sxs-lookup"><span data-stu-id="cdea7-194">Hot Module Replacement</span></span>

<span data-ttu-id="cdea7-195">Piense del Webpack [activa sustitución del módulo](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) característica (HMR) como una evolución del [Webpack Dev Middleware](#webpack-dev-middleware).</span><span class="sxs-lookup"><span data-stu-id="cdea7-195">Think of Webpack's [Hot Module Replacement](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="cdea7-196">HMR incorpora las mismas ventajas, pero optimiza aún más el flujo de trabajo de desarrollo al actualizar automáticamente el contenido de página después de compilar los cambios.</span><span class="sxs-lookup"><span data-stu-id="cdea7-196">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="cdea7-197">No se debe confundir con una actualización del explorador, lo que interferiría con el estado actual de en memoria y la sesión de depuración de la aplicación SPA.</span><span class="sxs-lookup"><span data-stu-id="cdea7-197">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="cdea7-198">Hay un vínculo directo entre el servicio de Middleware de desarrollo de Webpack y el explorador, lo que significa que los cambios son ~ simplemente otra palabra prohibida ~ insertados en el explorador.</span><span class="sxs-lookup"><span data-stu-id="cdea7-198">There is a live link between the Webpack Dev Middleware service and the browser, which means changes are ~simply another banned word~ pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cdea7-199">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="cdea7-199">Prerequisites</span></span>

<span data-ttu-id="cdea7-200">Instalar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-200">Install the following:</span></span>
* <span data-ttu-id="cdea7-201">[middleware caliente webpack](https://www.npmjs.com/package/webpack-hot-middleware) npm paquete:</span><span class="sxs-lookup"><span data-stu-id="cdea7-201">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="cdea7-202">Configuración</span><span class="sxs-lookup"><span data-stu-id="cdea7-202">Configuration</span></span>

<span data-ttu-id="cdea7-203">El componente HMR debe estar registrado en la canalización de solicitudes HTTP de MVC en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="cdea7-203">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="cdea7-204">Al igual que ocurría con [Webpack Dev Middleware](#webpack-dev-middleware), `UseWebpackDevMiddleware` método de extensión se debe llamar antes el `UseStaticFiles` método de extensión.</span><span class="sxs-lookup"><span data-stu-id="cdea7-204">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="cdea7-205">Por motivos de seguridad, registre el middleware solo cuando la aplicación se ejecuta en modo de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="cdea7-205">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="cdea7-206">El *webpack.config.js* archivo debe definir una `plugins` de las matrices, incluso si se deja vacía:</span><span class="sxs-lookup"><span data-stu-id="cdea7-206">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

<span data-ttu-id="cdea7-207">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-207">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]</span></span>

<span data-ttu-id="cdea7-208">Después de cargar la aplicación en el explorador, pestaña de la consola de las herramientas de desarrollo proporciona confirmación de activación de HMR:</span><span class="sxs-lookup"><span data-stu-id="cdea7-208">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![Mensaje de conectado activa de sustitución del módulo](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="cdea7-210">Aplicaciones auxiliares de enrutamientos</span><span class="sxs-lookup"><span data-stu-id="cdea7-210">Routing helpers</span></span>

<span data-ttu-id="cdea7-211">En la mayoría SPAs basada en núcleo de ASP.NET, es conveniente enrutamiento de cliente además del enrutamiento del servidor.</span><span class="sxs-lookup"><span data-stu-id="cdea7-211">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="cdea7-212">Los sistemas de enrutamiento SPA y MVC pueden trabajar independientemente sin interferencias.</span><span class="sxs-lookup"><span data-stu-id="cdea7-212">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="cdea7-213">Sin embargo, hay un caso extremo supone desafíos: identificación de respuestas HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="cdea7-213">There is, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="cdea7-214">Considere el escenario en el que una ruta sin extensión de `/some/page` se utiliza.</span><span class="sxs-lookup"><span data-stu-id="cdea7-214">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="cdea7-215">Se supone la solicitud no hacer coincidir el patrón una ruta de servidor, pero su patrón coincide con una ruta de cliente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-215">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="cdea7-216">Ahora considere una solicitud entrante para `/images/user-512.png`, por lo general que espera encontrar un archivo de imagen en el servidor.</span><span class="sxs-lookup"><span data-stu-id="cdea7-216">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="cdea7-217">Si esa ruta de acceso del recurso solicitado no coincide con ninguna ruta de servidor o un archivo estático, no es probable que la aplicación de cliente podría controlarla, por lo general desea devolver un código de estado HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="cdea7-217">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="cdea7-218">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="cdea7-218">Prerequisites</span></span>

<span data-ttu-id="cdea7-219">Instalar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-219">Install the following:</span></span>
* <span data-ttu-id="cdea7-220">El paquete de cliente de enrutamiento npm.</span><span class="sxs-lookup"><span data-stu-id="cdea7-220">The client-side routing npm package.</span></span> <span data-ttu-id="cdea7-221">Utilizando Angular como ejemplo:</span><span class="sxs-lookup"><span data-stu-id="cdea7-221">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="cdea7-222">Configuración</span><span class="sxs-lookup"><span data-stu-id="cdea7-222">Configuration</span></span>

<span data-ttu-id="cdea7-223">Un método de extensión denominado `MapSpaFallbackRoute` se utiliza en el `Configure` método:</span><span class="sxs-lookup"><span data-stu-id="cdea7-223">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

<span data-ttu-id="cdea7-224">[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-224">[!code-csharp[Main](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]</span></span>

<span data-ttu-id="cdea7-225">Sugerencia: Las rutas se evalúan en el orden en el que está configurados.</span><span class="sxs-lookup"><span data-stu-id="cdea7-225">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="cdea7-226">Por lo tanto, la `default` ruta en el ejemplo de código anterior se utiliza en primer lugar para la coincidencia.</span><span class="sxs-lookup"><span data-stu-id="cdea7-226">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="cdea7-227">Crear un nuevo proyecto</span><span class="sxs-lookup"><span data-stu-id="cdea7-227">Creating a new project</span></span>

<span data-ttu-id="cdea7-228">JavaScriptServices proporciona plantillas de aplicaciones configuradas previamente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-228">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="cdea7-229">SpaServices se utiliza en estas plantillas, junto con una diversidad de marcos y bibliotecas como Angular, Aurelia, Knockout, reaccionar y objeto.</span><span class="sxs-lookup"><span data-stu-id="cdea7-229">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, Aurelia, Knockout, React, and Vue.</span></span>

<span data-ttu-id="cdea7-230">Estas plantillas se pueden instalar a través de la CLI de núcleo de .NET con el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="cdea7-230">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="cdea7-231">Se muestra una lista de plantillas SPA disponibles:</span><span class="sxs-lookup"><span data-stu-id="cdea7-231">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="cdea7-232">Plantillas</span><span class="sxs-lookup"><span data-stu-id="cdea7-232">Templates</span></span>                                 | <span data-ttu-id="cdea7-233">Nombre corto</span><span class="sxs-lookup"><span data-stu-id="cdea7-233">Short Name</span></span> | <span data-ttu-id="cdea7-234">Lenguaje</span><span class="sxs-lookup"><span data-stu-id="cdea7-234">Language</span></span> | <span data-ttu-id="cdea7-235">Etiquetas</span><span class="sxs-lookup"><span data-stu-id="cdea7-235">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="cdea7-236">Núcleo de ASP.NET de MVC con Angular</span><span class="sxs-lookup"><span data-stu-id="cdea7-236">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="cdea7-237">angular</span><span class="sxs-lookup"><span data-stu-id="cdea7-237">angular</span></span>    | <span data-ttu-id="cdea7-238">[C#]</span><span class="sxs-lookup"><span data-stu-id="cdea7-238">[C#]</span></span>     | <span data-ttu-id="cdea7-239">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cdea7-239">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cdea7-240">Núcleo de ASP.NET de MVC con Aurelia</span><span class="sxs-lookup"><span data-stu-id="cdea7-240">MVC ASP.NET Core with Aurelia</span></span>             | <span data-ttu-id="cdea7-241">aurelia</span><span class="sxs-lookup"><span data-stu-id="cdea7-241">aurelia</span></span>    | <span data-ttu-id="cdea7-242">[C#]</span><span class="sxs-lookup"><span data-stu-id="cdea7-242">[C#]</span></span>     | <span data-ttu-id="cdea7-243">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cdea7-243">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cdea7-244">Núcleo de ASP.NET de MVC con Knockout.js</span><span class="sxs-lookup"><span data-stu-id="cdea7-244">MVC ASP.NET Core with Knockout.js</span></span>         | <span data-ttu-id="cdea7-245">knockout</span><span class="sxs-lookup"><span data-stu-id="cdea7-245">knockout</span></span>   | <span data-ttu-id="cdea7-246">[C#]</span><span class="sxs-lookup"><span data-stu-id="cdea7-246">[C#]</span></span>     | <span data-ttu-id="cdea7-247">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cdea7-247">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cdea7-248">Núcleo de ASP.NET de MVC con React.js</span><span class="sxs-lookup"><span data-stu-id="cdea7-248">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="cdea7-249">react</span><span class="sxs-lookup"><span data-stu-id="cdea7-249">react</span></span>      | <span data-ttu-id="cdea7-250">[C#]</span><span class="sxs-lookup"><span data-stu-id="cdea7-250">[C#]</span></span>     | <span data-ttu-id="cdea7-251">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cdea7-251">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cdea7-252">Núcleo de ASP.NET MVC con React.js y reducción</span><span class="sxs-lookup"><span data-stu-id="cdea7-252">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="cdea7-253">reactredux</span><span class="sxs-lookup"><span data-stu-id="cdea7-253">reactredux</span></span> | <span data-ttu-id="cdea7-254">[C#]</span><span class="sxs-lookup"><span data-stu-id="cdea7-254">[C#]</span></span>     | <span data-ttu-id="cdea7-255">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cdea7-255">Web/MVC/SPA</span></span> |
| <span data-ttu-id="cdea7-256">Núcleo de ASP.NET de MVC con Vue.js</span><span class="sxs-lookup"><span data-stu-id="cdea7-256">MVC ASP.NET Core with Vue.js</span></span>              | <span data-ttu-id="cdea7-257">objeto</span><span class="sxs-lookup"><span data-stu-id="cdea7-257">vue</span></span>        | <span data-ttu-id="cdea7-258">[C#]</span><span class="sxs-lookup"><span data-stu-id="cdea7-258">[C#]</span></span>     | <span data-ttu-id="cdea7-259">MVC/Web/SPA</span><span class="sxs-lookup"><span data-stu-id="cdea7-259">Web/MVC/SPA</span></span> | 

<span data-ttu-id="cdea7-260">Para crear un nuevo proyecto con una de las plantillas SPA, incluya el **nombre corto** de la plantilla en el `dotnet new` comando.</span><span class="sxs-lookup"><span data-stu-id="cdea7-260">To create a new project using one of the SPA templates, include the **Short Name** of the template in the `dotnet new` command.</span></span> <span data-ttu-id="cdea7-261">El siguiente comando crea una aplicación Angular con ASP.NET MVC de núcleo configurado para el lado del servidor:</span><span class="sxs-lookup"><span data-stu-id="cdea7-261">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="cdea7-262">Establecer el modo de configuración en tiempo de ejecución</span><span class="sxs-lookup"><span data-stu-id="cdea7-262">Set the runtime configuration mode</span></span>

<span data-ttu-id="cdea7-263">Existen dos modos de configuración en tiempo de ejecución principal:</span><span class="sxs-lookup"><span data-stu-id="cdea7-263">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="cdea7-264">**Desarrollo de**:</span><span class="sxs-lookup"><span data-stu-id="cdea7-264">**Development**:</span></span>
    * <span data-ttu-id="cdea7-265">Incluye asignaciones de origen para facilitar la depuración.</span><span class="sxs-lookup"><span data-stu-id="cdea7-265">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="cdea7-266">No optimice el código del lado cliente para el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="cdea7-266">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="cdea7-267">**Producción**:</span><span class="sxs-lookup"><span data-stu-id="cdea7-267">**Production**:</span></span>
    * <span data-ttu-id="cdea7-268">Excluye los mapas de código fuente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-268">Excludes source maps.</span></span>
    * <span data-ttu-id="cdea7-269">Optimiza el código de cliente a través de agrupación y minificación.</span><span class="sxs-lookup"><span data-stu-id="cdea7-269">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="cdea7-270">ASP.NET Core utiliza una variable de entorno denominada `ASPNETCORE_ENVIRONMENT` para almacenar el modo de configuración.</span><span class="sxs-lookup"><span data-stu-id="cdea7-270">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="cdea7-271">Vea ** [establecer el entorno de](xref:fundamentals/environments#setting-the-environment) ** para obtener más información.</span><span class="sxs-lookup"><span data-stu-id="cdea7-271">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="cdea7-272">Ejecutar con .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cdea7-272">Running with .NET Core CLI</span></span>

<span data-ttu-id="cdea7-273">Restaurar el NuGet necesario y los paquetes de npm ejecutando el siguiente comando en la raíz del proyecto:</span><span class="sxs-lookup"><span data-stu-id="cdea7-273">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="cdea7-274">Compilar y ejecutar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="cdea7-274">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="cdea7-275">Inicia la aplicación en localhost según la [el modo de configuración en tiempo de ejecución](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="cdea7-275">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="cdea7-276">Vaya a `http://localhost:5000` en el explorador muestra la página de aterrizaje.</span><span class="sxs-lookup"><span data-stu-id="cdea7-276">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="cdea7-277">Ejecutar con Visual Studio de 2017</span><span class="sxs-lookup"><span data-stu-id="cdea7-277">Running with Visual Studio 2017</span></span>

<span data-ttu-id="cdea7-278">Abra la *.csproj* archivo generado por el `dotnet new` comando.</span><span class="sxs-lookup"><span data-stu-id="cdea7-278">Open the *.csproj* file generated by the `dotnet new` command.</span></span> <span data-ttu-id="cdea7-279">Los paquetes de NuGet y npm necesarios se restauran automáticamente al proyecto abierto.</span><span class="sxs-lookup"><span data-stu-id="cdea7-279">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="cdea7-280">Este proceso de restauración puede tardar unos minutos y está lista para ejecutarse cuando finaliza la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cdea7-280">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="cdea7-281">Haga clic en el botón verde de ejecución o presione `Ctrl + F5`, y el explorador se abre en la página de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cdea7-281">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="cdea7-282">La aplicación se ejecuta en localhost según la [el modo de configuración en tiempo de ejecución](#runtime-config-mode).</span><span class="sxs-lookup"><span data-stu-id="cdea7-282">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="cdea7-283">Probar la aplicación</span><span class="sxs-lookup"><span data-stu-id="cdea7-283">Testing the app</span></span>

<span data-ttu-id="cdea7-284">Plantillas de SpaServices están preconfiguradas para ejecutar pruebas de cliente mediante [Karma](https://karma-runner.github.io/1.0/index.html) y [Jasmine](https://jasmine.github.io/).</span><span class="sxs-lookup"><span data-stu-id="cdea7-284">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="cdea7-285">Jasmine es una unidad popular marco de pruebas para JavaScript, mientras que Karma es un ejecutor de pruebas para las pruebas.</span><span class="sxs-lookup"><span data-stu-id="cdea7-285">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="cdea7-286">Karma está configurado para trabajar con la [Webpack Dev Middleware](#webpack-dev-middleware) tal que no tiene que detener y ejecutar la prueba cada vez que se realizan cambios.</span><span class="sxs-lookup"><span data-stu-id="cdea7-286">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that you don’t have to stop and run the test every time changes are made.</span></span> <span data-ttu-id="cdea7-287">Si es el código que se ejecuta en el caso de prueba o el propio caso de prueba, la prueba se ejecuta automáticamente.</span><span class="sxs-lookup"><span data-stu-id="cdea7-287">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="cdea7-288">Usar la aplicación Angular como ejemplo, dos casos de prueba criterio Comidos ya se proporcionan para que la `CounterComponent` en el *counter.component.spec.ts* archivo:</span><span class="sxs-lookup"><span data-stu-id="cdea7-288">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

<span data-ttu-id="cdea7-289">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-289">[!code-typescript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]</span></span>

<span data-ttu-id="cdea7-290">Abra el símbolo del sistema en la raíz del proyecto y ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="cdea7-290">Open the command prompt at the project root, and run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="cdea7-291">La secuencia de comandos inicia el ejecutor de pruebas Karma, que lee la configuración definida en el *karma.conf.js* archivo.</span><span class="sxs-lookup"><span data-stu-id="cdea7-291">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="cdea7-292">Entre otras opciones, el *karma.conf.js* identifica los archivos de prueba para ejecutarse a través de su `files` matriz:</span><span class="sxs-lookup"><span data-stu-id="cdea7-292">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

<span data-ttu-id="cdea7-293">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-293">[!code-javascript[Main](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]</span></span>

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="cdea7-294">Publicar la aplicación</span><span class="sxs-lookup"><span data-stu-id="cdea7-294">Publishing the application</span></span>

<span data-ttu-id="cdea7-295">Combinación de los activos de cliente generados y los artefactos de ASP.NET Core publicados en un paquete listo para implementar puede resultar complicada.</span><span class="sxs-lookup"><span data-stu-id="cdea7-295">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="cdea7-296">Por suerte, SpaServices orquesta ese proceso de publicación completa con un destino de MSBuild personalizado denominado `RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="cdea7-296">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

<span data-ttu-id="cdea7-297">[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]</span><span class="sxs-lookup"><span data-stu-id="cdea7-297">[!code-xml[Main](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]</span></span>

<span data-ttu-id="cdea7-298">El destino de MSBuild tiene las siguientes responsabilidades:</span><span class="sxs-lookup"><span data-stu-id="cdea7-298">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="cdea7-299">Restaure los paquetes npm</span><span class="sxs-lookup"><span data-stu-id="cdea7-299">Restore the npm packages</span></span>
1. <span data-ttu-id="cdea7-300">Cree una versión de producción automatizado de los activos de terceros, de cliente</span><span class="sxs-lookup"><span data-stu-id="cdea7-300">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="cdea7-301">Cree una versión de producción automatizado de los activos de cliente personalizados</span><span class="sxs-lookup"><span data-stu-id="cdea7-301">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="cdea7-302">Copiar los activos Webpack generados en la carpeta de publicación</span><span class="sxs-lookup"><span data-stu-id="cdea7-302">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="cdea7-303">El destino de MSBuild se invoca cuando se ejecuta:</span><span class="sxs-lookup"><span data-stu-id="cdea7-303">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="cdea7-304">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="cdea7-304">Additional resources</span></span>

* [<span data-ttu-id="cdea7-305">Documentos angulares</span><span class="sxs-lookup"><span data-stu-id="cdea7-305">Angular Docs</span></span>](https://angular.io/docs)