---
title: Aplicar formato a los datos de respuesta en MVC de ASP.NET Core
author: ardalis
description: "Obtenga información acerca de cómo dar formato a los datos de respuesta en MVC de ASP.NET Core."
keywords: "Núcleo de ASP.NET, datos de respuesta, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 91ba2456178fe806b90f27bbd2940773da950423
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a><span data-ttu-id="e1624-104">Introducción a los datos de respuesta de formato en MVC de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e1624-104">Introduction to formatting response data in ASP.NET Core MVC</span></span>

<span data-ttu-id="e1624-105">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e1624-105">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e1624-106">Núcleo de ASP.NET MVC tiene compatibilidad integrada para dar formato a los datos de respuesta, con formatos fijos o en respuesta a las especificaciones del cliente.</span><span class="sxs-lookup"><span data-stu-id="e1624-106">ASP.NET Core MVC has built-in support for formatting response data, using fixed formats or in response to client specifications.</span></span>

<span data-ttu-id="e1624-107">[Ver o descargar el ejemplo desde GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span><span class="sxs-lookup"><span data-stu-id="e1624-107">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="e1624-108">Resultados de acción específica de formato</span><span class="sxs-lookup"><span data-stu-id="e1624-108">Format-Specific Action Results</span></span>

<span data-ttu-id="e1624-109">Algunos tipos de resultado de acción son específicos de un formato determinado, como `JsonResult` y `ContentResult`.</span><span class="sxs-lookup"><span data-stu-id="e1624-109">Some action result types are specific to a particular format, such as `JsonResult` and `ContentResult`.</span></span> <span data-ttu-id="e1624-110">Las acciones pueden devolver resultados específicos que siempre se da formato de una manera determinada.</span><span class="sxs-lookup"><span data-stu-id="e1624-110">Actions can return specific results that are always formatted in a particular manner.</span></span> <span data-ttu-id="e1624-111">Por ejemplo, devolver un `JsonResult` devolverá datos con formato JSON, independientemente de las preferencias del cliente.</span><span class="sxs-lookup"><span data-stu-id="e1624-111">For example, returning a `JsonResult` will return JSON-formatted data, regardless of client preferences.</span></span> <span data-ttu-id="e1624-112">Del mismo modo, devolver un `ContentResult` devolverá datos de cadena en formato de texto sin formato (así como devolver simplemente una cadena).</span><span class="sxs-lookup"><span data-stu-id="e1624-112">Likewise, returning a `ContentResult` will return plain-text-formatted string data (as will simply returning a string).</span></span>

> [!NOTE]
> <span data-ttu-id="e1624-113">No es necesario una acción para devolver ningún tipo en particular; MVC es compatible con cualquier valor de objeto del valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="e1624-113">An action isn't required to return any particular type; MVC supports any object return value.</span></span> <span data-ttu-id="e1624-114">Si una acción devuelve un `IActionResult` hereda de implementación y el controlador de `Controller`, los desarrolladores tienen muchos métodos auxiliares correspondientes a muchas de las opciones.</span><span class="sxs-lookup"><span data-stu-id="e1624-114">If an action returns an `IActionResult` implementation and the controller inherits from `Controller`, developers have many helper methods corresponding to many of the choices.</span></span> <span data-ttu-id="e1624-115">Resultados de las acciones que devuelven objetos que no son `IActionResult` tipos se serializará con la correspondiente `IOutputFormatter` implementación.</span><span class="sxs-lookup"><span data-stu-id="e1624-115">Results from actions that return objects that are not `IActionResult` types will be serialized using the appropriate `IOutputFormatter` implementation.</span></span>

<span data-ttu-id="e1624-116">Se devuelven los datos en un formato específico de un controlador que hereda de la `Controller` clase base, utilice el método de aplicación auxiliar integradas `Json` para devolver JSON y `Content` de texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="e1624-116">To return data in a specific format from a controller that inherits from the `Controller` base class, use the built-in helper method `Json` to return JSON and `Content` for plain text.</span></span> <span data-ttu-id="e1624-117">El método de acción debe devolver el tipo de resultado específico (por ejemplo, `JsonResult`) o `IActionResult`.</span><span class="sxs-lookup"><span data-stu-id="e1624-117">Your action method should return either the specific result type (for instance, `JsonResult`) or `IActionResult`.</span></span>

<span data-ttu-id="e1624-118">Devolver datos con formato JSON:</span><span class="sxs-lookup"><span data-stu-id="e1624-118">Returning JSON-formatted data:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

<span data-ttu-id="e1624-119">Respuesta de ejemplo de esta acción:</span><span class="sxs-lookup"><span data-stu-id="e1624-119">Sample response from this action:</span></span>

![Pestaña red de las herramientas de desarrollo con Microsoft Edge, que muestra el tipo de contenido de la respuesta es application/json](formatting/_static/json-response.png)

<span data-ttu-id="e1624-121">Tenga en cuenta que el tipo de contenido de la respuesta es `application/json`, como se muestra en la lista de las solicitudes de red y en la sección de encabezados de respuesta.</span><span class="sxs-lookup"><span data-stu-id="e1624-121">Note that the content type of the response is `application/json`, shown both in the list of network requests and in the Response Headers section.</span></span> <span data-ttu-id="e1624-122">Tenga en cuenta también la lista de opciones que se presentan por el explorador (en este caso, Microsoft Edge) en el encabezado Accept en la sección de encabezados de solicitud.</span><span class="sxs-lookup"><span data-stu-id="e1624-122">Also note the list of options presented by the browser (in this case, Microsoft Edge) in the Accept header in the Request Headers section.</span></span> <span data-ttu-id="e1624-123">La técnica actual está omitiendo este encabezado; seguir se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="e1624-123">The current technique is ignoring this header; obeying it is discussed below.</span></span>

<span data-ttu-id="e1624-124">Para devolver datos de texto con formato, use `ContentResult` y `Content` auxiliar:</span><span class="sxs-lookup"><span data-stu-id="e1624-124">To return plain text formatted data, use `ContentResult` and the `Content` helper:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

<span data-ttu-id="e1624-125">Una respuesta de esta acción:</span><span class="sxs-lookup"><span data-stu-id="e1624-125">A response from this action:</span></span>

![Pestaña red de las herramientas de desarrollo con Microsoft Edge, que muestra el tipo de contenido de la respuesta es texto/sin formato](formatting/_static/text-response.png)

<span data-ttu-id="e1624-127">Tenga en cuenta en este caso el `Content-Type` devuelto es `text/plain`.</span><span class="sxs-lookup"><span data-stu-id="e1624-127">Note in this case the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="e1624-128">También se puede lograr este mismo comportamiento usando simplemente un tipo de respuesta de cadena:</span><span class="sxs-lookup"><span data-stu-id="e1624-128">You can also achieve this same behavior using just a string response type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> <span data-ttu-id="e1624-129">Para acciones no trivial con varios, opciones (por ejemplo, códigos de estado HTTP distintos en función del resultado de las operaciones realizadas) o tipos de retorno, prefiera `IActionResult` como el tipo de valor devuelto.</span><span class="sxs-lookup"><span data-stu-id="e1624-129">For non-trivial actions with multiple return types or options (for example, different HTTP status codes based on the result of operations performed), prefer `IActionResult` as the return type.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="e1624-130">Negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="e1624-130">Content Negotiation</span></span>

<span data-ttu-id="e1624-131">Negociación de contenido (*conneg* abreviado) se produce cuando el cliente especifica un [encabezado Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span><span class="sxs-lookup"><span data-stu-id="e1624-131">Content negotiation (*conneg* for short) occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="e1624-132">El formato predeterminado utilizado por núcleo de ASP.NET MVC es JSON.</span><span class="sxs-lookup"><span data-stu-id="e1624-132">The default format used by ASP.NET Core MVC is JSON.</span></span> <span data-ttu-id="e1624-133">Negociación de contenido se implementa mediante `ObjectResult`.</span><span class="sxs-lookup"><span data-stu-id="e1624-133">Content negotiation is implemented by `ObjectResult`.</span></span> <span data-ttu-id="e1624-134">También se integra en el código de estado que devuelven resultados de acción específica de los métodos auxiliares (que se basan en `ObjectResult`).</span><span class="sxs-lookup"><span data-stu-id="e1624-134">It is also built into the status code specific action results returned from the helper methods (which are all based on `ObjectResult`).</span></span> <span data-ttu-id="e1624-135">También puede devolver un tipo de modelo (es decir, una clase que haya definido como el tipo de transferencia de datos) y el marco de trabajo ajustará automáticamente en un `ObjectResult` automáticamente.</span><span class="sxs-lookup"><span data-stu-id="e1624-135">You can also return a model type (a class you've defined as your data transfer type) and the framework will automatically wrap it in an `ObjectResult` for you.</span></span>

<span data-ttu-id="e1624-136">El siguiente método de acción utiliza el `Ok` y `NotFound` métodos auxiliares:</span><span class="sxs-lookup"><span data-stu-id="e1624-136">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

<span data-ttu-id="e1624-137">Se devolverá una respuesta con formato JSON a menos que se solicitó otro formato y el servidor puede devolver el formato solicitado.</span><span class="sxs-lookup"><span data-stu-id="e1624-137">A JSON-formatted response will be returned unless another format was requested and the server can return the requested format.</span></span> <span data-ttu-id="e1624-138">Puede usar una herramienta como [Fiddler](http://www.telerik.com/fiddler) para crear una solicitud que incluye un encabezado de aceptación y especifique otro formato.</span><span class="sxs-lookup"><span data-stu-id="e1624-138">You can use a tool like [Fiddler](http://www.telerik.com/fiddler) to create a request that includes an Accept header and specify another format.</span></span> <span data-ttu-id="e1624-139">En ese caso, si el servidor tiene un *formateador* que puede producir una respuesta en el formato solicitado, se devolverá el resultado en el formato preferido por el cliente.</span><span class="sxs-lookup"><span data-stu-id="e1624-139">In that case, if the server has a *formatter* that can produce a response in the requested format, the result will be returned in the client-preferred format.</span></span>

![Consola de Fiddler que muestra un creados manualmente obtener la solicitud con un valor de encabezado de aceptación de aplicación/xml](formatting/_static/fiddler-composer.png)

<span data-ttu-id="e1624-141">En la captura de pantalla anterior, se ha utilizado el Compositor de Fiddler para generar una solicitud, especificar `Accept: application/xml`.</span><span class="sxs-lookup"><span data-stu-id="e1624-141">In the above screenshot, the Fiddler Composer has been used to generate a request, specifying `Accept: application/xml`.</span></span> <span data-ttu-id="e1624-142">De forma predeterminada, MVC de ASP.NET Core solo admite JSON, incluso cuando se especifica otro formato, el resultado devuelto es todavía con formato JSON.</span><span class="sxs-lookup"><span data-stu-id="e1624-142">By default, ASP.NET Core MVC only supports JSON, so even when another format is specified, the result returned is still JSON-formatted.</span></span> <span data-ttu-id="e1624-143">Verá cómo agregar los formateadores adicionales en la sección siguiente.</span><span class="sxs-lookup"><span data-stu-id="e1624-143">You'll see how to add additional formatters in the next section.</span></span>

<span data-ttu-id="e1624-144">Acciones del controlador pueden devolver POCOs (sin formato antiguo objetos CLR), en cuyo caso ASP.NET MVC creará automáticamente un `ObjectResult` que ajusta el objeto.</span><span class="sxs-lookup"><span data-stu-id="e1624-144">Controller actions can return POCOs (Plain Old CLR Objects), in which case ASP.NET MVC will automatically create an `ObjectResult` for you that wraps the object.</span></span> <span data-ttu-id="e1624-145">El cliente reciba el objeto serializado con formato (formato JSON es el valor predeterminado; puede configurar otros formatos o XML).</span><span class="sxs-lookup"><span data-stu-id="e1624-145">The client will get the formatted serialized object (JSON format is the default; you can configure XML or other formats).</span></span> <span data-ttu-id="e1624-146">Si el objeto que se va a devolver es `null`, el marco de trabajo se devolverá un `204 No Content` respuesta.</span><span class="sxs-lookup"><span data-stu-id="e1624-146">If the object being returned is `null`, then the framework will return a `204 No Content` response.</span></span>

<span data-ttu-id="e1624-147">Devuelve un tipo de objeto:</span><span class="sxs-lookup"><span data-stu-id="e1624-147">Returning an object type:</span></span>

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

<span data-ttu-id="e1624-148">En el ejemplo, una solicitud de un alias válido autor recibirá una respuesta 200 OK con datos del autor.</span><span class="sxs-lookup"><span data-stu-id="e1624-148">In the sample, a request for a valid author alias will receive a 200 OK response with the author's data.</span></span> <span data-ttu-id="e1624-149">Una solicitud de un alias válido recibirá una respuesta 204 Sin contenido.</span><span class="sxs-lookup"><span data-stu-id="e1624-149">A request for an invalid alias will receive a 204 No Content response.</span></span> <span data-ttu-id="e1624-150">A continuación se muestran capturas de pantalla que muestra la respuesta en formatos XML y JSON.</span><span class="sxs-lookup"><span data-stu-id="e1624-150">Screenshots showing the response in XML and JSON formats are shown below.</span></span>

### <a name="content-negotiation-process"></a><span data-ttu-id="e1624-151">Proceso de negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="e1624-151">Content Negotiation Process</span></span>

<span data-ttu-id="e1624-152">Contenido *negociación* solo se lleva a cabo si una `Accept` encabezado aparece en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="e1624-152">Content *negotiation* only takes place if an `Accept` header appears in the request.</span></span> <span data-ttu-id="e1624-153">Cuando una solicitud contiene un encabezado de aceptación, el marco de trabajo enumerará los tipos de medios en el encabezado accept en orden de preferencia e intentará encontrar a un formateador que puede generar una respuesta en uno de los formatos especificados por el encabezado accept.</span><span class="sxs-lookup"><span data-stu-id="e1624-153">When a request contains an accept header, the framework will enumerate the media types in the accept header in preference order and will try to find a formatter that can produce a response in one of the formats specified by the accept header.</span></span> <span data-ttu-id="e1624-154">En caso de que no se encuentra ningún formateador que puede satisfacer la solicitud del cliente, el marco de trabajo intentará encontrar el primer formateador que puede generar una respuesta (a menos que el desarrollador ha configurado la opción de `MvcOptions` para devolver 406 No aceptable en su lugar).</span><span class="sxs-lookup"><span data-stu-id="e1624-154">In case no formatter is found that can satisfy the client's request, the framework will try to find the first formatter that can produce a response (unless the developer has configured the option on `MvcOptions` to return 406 Not Acceptable instead).</span></span> <span data-ttu-id="e1624-155">Si la solicitud especifica XML, pero no se ha configurado el formateador de XML, se usará el formateador JSON.</span><span class="sxs-lookup"><span data-stu-id="e1624-155">If the request specifies XML, but the XML formatter has not been configured, then the JSON formatter will be used.</span></span> <span data-ttu-id="e1624-156">Por lo general, si no se ha configurado ningún formateador que puede proporcionar el formato solicitado, a continuación, se utiliza el primer formateador que puede dar formato al objeto.</span><span class="sxs-lookup"><span data-stu-id="e1624-156">More generally, if no formatter is configured that can provide the requested format, then the first formatter than can format the object is used.</span></span> <span data-ttu-id="e1624-157">Si no se especifica ningún encabezado, se utilizará el primer formateador que puede controlar el objeto que se va a devolver para serializar la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e1624-157">If no header is given, the first formatter that can handle the object to be returned will be used to serialize the response.</span></span> <span data-ttu-id="e1624-158">En este caso, no hay ninguna negociación que tienen lugar: el servidor es determinar qué formato va a usar.</span><span class="sxs-lookup"><span data-stu-id="e1624-158">In this case, there isn't any negotiation taking place - the server is determining what format it will use.</span></span>

> [!NOTE]
> <span data-ttu-id="e1624-159">Si el encabezado Accept contiene `*/*`, el encabezado se omitirá a menos que `RespectBrowserAcceptHeader` está establecida en true en `MvcOptions`.</span><span class="sxs-lookup"><span data-stu-id="e1624-159">If the Accept header contains `*/*`, the Header will be ignored unless `RespectBrowserAcceptHeader` is set to true on `MvcOptions`.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="e1624-160">Exploradores y negociación de contenido</span><span class="sxs-lookup"><span data-stu-id="e1624-160">Browsers and Content Negotiation</span></span>

<span data-ttu-id="e1624-161">A diferencia de los clientes de API típicos, exploradores web tienden a proporcionar `Accept` encabezados que incluyen una amplia gama de formatos, incluidos los caracteres comodín.</span><span class="sxs-lookup"><span data-stu-id="e1624-161">Unlike typical API clients, web browsers tend to supply `Accept` headers that include a wide array of formats, including wildcards.</span></span> <span data-ttu-id="e1624-162">De forma predeterminada, cuando el marco de trabajo detecta que la solicitud procede de un explorador, omitirá el `Accept` encabezado y valor devuelto en su lugar, el contenido de la aplicación configurar formato predeterminado (JSON, a menos que se configure de otra).</span><span class="sxs-lookup"><span data-stu-id="e1624-162">By default, when the framework detects that the request is coming from a browser, it will ignore the `Accept` header and instead return the content in the application's configured default format (JSON unless otherwise configured).</span></span> <span data-ttu-id="e1624-163">Esto proporciona una experiencia más coherente al utilizar otros exploradores para utilizar las API.</span><span class="sxs-lookup"><span data-stu-id="e1624-163">This provides a more consistent experience when using different browsers to consume APIs.</span></span>

<span data-ttu-id="e1624-164">Si prefiere el Explorador de honor aplicación encabezados de aceptación, puede configurar esto como parte de la configuración de MVC estableciendo `RespectBrowserAcceptHeader` a `true` en el `ConfigureServices` método *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="e1624-164">If you would prefer your application honor browser accept headers, you can configure this as part of MVC's configuration by setting `RespectBrowserAcceptHeader` to `true` in the `ConfigureServices` method in *Startup.cs*.</span></span>

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
}
```

## <a name="configuring-formatters"></a><span data-ttu-id="e1624-165">Configuración de formateadores</span><span class="sxs-lookup"><span data-stu-id="e1624-165">Configuring Formatters</span></span>

<span data-ttu-id="e1624-166">Si la aplicación necesita admitir formatos adicionales más allá del valor predeterminado es JSON, puede agregar paquetes de NuGet y configurar MVC para admitirlas.</span><span class="sxs-lookup"><span data-stu-id="e1624-166">If your application needs to support additional formats beyond the default of JSON, you can add NuGet packages and configure MVC to support them.</span></span> <span data-ttu-id="e1624-167">Hay formateadores independientes para la entrada y salida.</span><span class="sxs-lookup"><span data-stu-id="e1624-167">There are separate formatters for input and output.</span></span> <span data-ttu-id="e1624-168">Formateadores de entrada se utilizan por [enlace de modelos](model-binding.md); formateadores de salida se utilizan para dar formato a las respuestas.</span><span class="sxs-lookup"><span data-stu-id="e1624-168">Input formatters are used by [Model Binding](model-binding.md); output formatters are used to format responses.</span></span> <span data-ttu-id="e1624-169">También puede configurar [personalizado formateadores](../advanced/custom-formatters.md).</span><span class="sxs-lookup"><span data-stu-id="e1624-169">You can also configure [Custom Formatters](../advanced/custom-formatters.md).</span></span>

### <a name="adding-xml-format-support"></a><span data-ttu-id="e1624-170">Agregar compatibilidad con el formato XML</span><span class="sxs-lookup"><span data-stu-id="e1624-170">Adding XML Format Support</span></span>

<span data-ttu-id="e1624-171">Para agregar compatibilidad para dar formato a XML, instale el `Microsoft.AspNetCore.Mvc.Formatters.Xml` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="e1624-171">To add support for XML formatting, install the `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet package.</span></span>

<span data-ttu-id="e1624-172">Agregue el XmlSerializerFormatters a configuración de MVC en *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e1624-172">Add the XmlSerializerFormatters to MVC's configuration in *Startup.cs*:</span></span>

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

<span data-ttu-id="e1624-173">Como alternativa, puede agregar al formateador de salida:</span><span class="sxs-lookup"><span data-stu-id="e1624-173">Alternately, you can add just the output formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

<span data-ttu-id="e1624-174">Estos dos enfoques serializará resultados con `System.Xml.Serialization.XmlSerializer`.</span><span class="sxs-lookup"><span data-stu-id="e1624-174">These two approaches will serialize results using `System.Xml.Serialization.XmlSerializer`.</span></span> <span data-ttu-id="e1624-175">Si lo prefiere, puede usar el `System.Runtime.Serialization.DataContractSerializer` agregando su formateador asociado:</span><span class="sxs-lookup"><span data-stu-id="e1624-175">If you prefer, you can use the `System.Runtime.Serialization.DataContractSerializer` by adding its associated formatter:</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

<span data-ttu-id="e1624-176">Una vez que se ha agregado compatibilidad con formato XML, los métodos de controlador deben devolver el formato adecuado en función de la solicitud `Accept` encabezado, como en este ejemplo se muestra de Fiddler:</span><span class="sxs-lookup"><span data-stu-id="e1624-176">Once you've added support for XML formatting, your controller methods should return the appropriate format based on the request's `Accept` header, as this Fiddler example demonstrates:</span></span>

![Consola de Fiddler: pestaña sin el formato de la solicitud muestra el valor del encabezado Accept es application/xml.](formatting/_static/xml-response.png)

<span data-ttu-id="e1624-179">Puede ver en la ficha de inspectores que se realizó la solicitud GET sin formato con un `Accept: application/xml` conjunto de encabezados.</span><span class="sxs-lookup"><span data-stu-id="e1624-179">You can see in the Inspectors tab that the Raw GET request was made with an `Accept: application/xml` header set.</span></span> <span data-ttu-id="e1624-180">Se muestra en el panel respuesta el `Content-Type: application/xml` encabezado y el `Author` objeto se ha serializado en XML.</span><span class="sxs-lookup"><span data-stu-id="e1624-180">The response pane shows the `Content-Type: application/xml` header, and the `Author` object has been serialized to XML.</span></span>

<span data-ttu-id="e1624-181">Utilice la ficha compositor para modificar la solicitud para especificar `application/json` en el `Accept` encabezado.</span><span class="sxs-lookup"><span data-stu-id="e1624-181">Use the Composer tab to modify the request to specify `application/json` in the `Accept` header.</span></span> <span data-ttu-id="e1624-182">Ejecutar la solicitud y la respuesta se formatearán como JSON:</span><span class="sxs-lookup"><span data-stu-id="e1624-182">Execute the request, and the response will be formatted as JSON:</span></span>

![Consola de Fiddler: pestaña sin el formato de la solicitud muestra el valor del encabezado Accept es application/json.](formatting/_static/json-response-fiddler.png)

<span data-ttu-id="e1624-185">En esta captura de pantalla, puede ver la solicitud establece un encabezado de `Accept: application/json` y especifica la respuesta de la misma que su `Content-Type`.</span><span class="sxs-lookup"><span data-stu-id="e1624-185">In this screenshot, you can see the request sets a header of `Accept: application/json` and the response specifies the same as its `Content-Type`.</span></span> <span data-ttu-id="e1624-186">La `Author` objeto se muestra en el cuerpo de la respuesta, en formato JSON.</span><span class="sxs-lookup"><span data-stu-id="e1624-186">The `Author` object is shown in the body of the response, in JSON format.</span></span>

### <a name="forcing-a-particular-format"></a><span data-ttu-id="e1624-187">Si fuerza un formato determinado</span><span class="sxs-lookup"><span data-stu-id="e1624-187">Forcing a Particular Format</span></span>

<span data-ttu-id="e1624-188">Si desea restringir los formatos de respuesta para una acción concreta puede, puede aplicar el `[Produces]` filtro.</span><span class="sxs-lookup"><span data-stu-id="e1624-188">If you would like to restrict the response formats for a specific action you can, you can apply the `[Produces]` filter.</span></span> <span data-ttu-id="e1624-189">El `[Produces]` filtro especifica los formatos de respuesta para una acción específica (o controlador).</span><span class="sxs-lookup"><span data-stu-id="e1624-189">The `[Produces]` filter specifies the response formats for a specific action (or controller).</span></span> <span data-ttu-id="e1624-190">Al igual que la mayoría [filtros](../controllers/filters.md), esto puede aplicarse a la acción, el controlador o el ámbito global.</span><span class="sxs-lookup"><span data-stu-id="e1624-190">Like most [Filters](../controllers/filters.md), this can be applied at the action, controller, or global scope.</span></span>

```csharp
[Produces("application/json")]
public class AuthorsController
```

<span data-ttu-id="e1624-191">El `[Produces]` filtro forzará todas las acciones dentro de la `AuthorsController` para devolver las respuestas con formato JSON, incluso si otros formateadores estaban configurados para la aplicación y el cliente proporciona un `Accept` encabezado solicita diferentes, disponibles formato.</span><span class="sxs-lookup"><span data-stu-id="e1624-191">The `[Produces]` filter will force all actions within the `AuthorsController` to return JSON-formatted responses, even if other formatters were configured for the application and the client provided an `Accept` header requesting a different, available format.</span></span> <span data-ttu-id="e1624-192">Vea [filtros](../controllers/filters.md) para obtener más información, incluida la forma de aplicar filtros globales.</span><span class="sxs-lookup"><span data-stu-id="e1624-192">See [Filters](../controllers/filters.md) to learn more, including how to apply filters globally.</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="e1624-193">Formateadores de casos especiales</span><span class="sxs-lookup"><span data-stu-id="e1624-193">Special Case Formatters</span></span>

<span data-ttu-id="e1624-194">Algunos casos especiales se implementan mediante formateadores integrados.</span><span class="sxs-lookup"><span data-stu-id="e1624-194">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="e1624-195">De forma predeterminada, `string` tipos de valor devuelto se formatearán como *texto/sin formato* (*texto/html* si se solicita a través de `Accept` encabezado).</span><span class="sxs-lookup"><span data-stu-id="e1624-195">By default, `string` return types will be formatted as *text/plain* (*text/html* if requested via `Accept` header).</span></span> <span data-ttu-id="e1624-196">Este comportamiento se puede quitar mediante la eliminación de la `TextOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="e1624-196">This behavior can be removed by removing the `TextOutputFormatter`.</span></span> <span data-ttu-id="e1624-197">Quitar formateadores en el `Configure` método *Startup.cs* (se muestra a continuación).</span><span class="sxs-lookup"><span data-stu-id="e1624-197">You remove formatters in the `Configure` method in *Startup.cs* (shown below).</span></span> <span data-ttu-id="e1624-198">Las acciones que tienen un objeto de modelo devuelven tipo no devolverá un 204 contenido respuesta al devolver `null`.</span><span class="sxs-lookup"><span data-stu-id="e1624-198">Actions that have a model object return type will return a 204 No Content response when returning `null`.</span></span> <span data-ttu-id="e1624-199">Este comportamiento se puede quitar mediante la eliminación de la `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="e1624-199">This behavior can be removed by removing the `HttpNoContentOutputFormatter`.</span></span> <span data-ttu-id="e1624-200">El siguiente código quita la `TextOutputFormatter` y `HttpNoContentOutputFormatter`.</span><span class="sxs-lookup"><span data-stu-id="e1624-200">The following code removes the `TextOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

<span data-ttu-id="e1624-201">Sin el `TextOutputFormatter`, `string` devuelven tipos devuelven 406 No aceptable, por ejemplo.</span><span class="sxs-lookup"><span data-stu-id="e1624-201">Without the `TextOutputFormatter`, `string` return types return 406 Not Acceptable, for example.</span></span> <span data-ttu-id="e1624-202">Tenga en cuenta que si existe un formateador XML, dará formato `string` tipos de valor devuelto si el `TextOutputFormatter` se quita.</span><span class="sxs-lookup"><span data-stu-id="e1624-202">Note that if an XML formatter exists, it will format `string` return types if the `TextOutputFormatter` is removed.</span></span>

<span data-ttu-id="e1624-203">Sin el `HttpNoContentOutputFormatter`, se da formato a objetos nulos utilizando el formateador configurado.</span><span class="sxs-lookup"><span data-stu-id="e1624-203">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="e1624-204">Por ejemplo, el formateador JSON simplemente devuelven una respuesta con un cuerpo de `null`, mientras que el formateador XML devolverá un elemento XML vacío con el atributo `xsi:nil="true"` establecido.</span><span class="sxs-lookup"><span data-stu-id="e1624-204">For example, the JSON formatter will simply return a response with a body of `null`, while the XML formatter will return an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="e1624-205">Asignaciones de direcciones URL de formato de respuesta</span><span class="sxs-lookup"><span data-stu-id="e1624-205">Response Format URL Mappings</span></span>

<span data-ttu-id="e1624-206">Los clientes pueden solicitar un formato determinado como parte de la dirección URL, como en la cadena de consulta o una parte de la ruta de acceso, o mediante el uso de una extensión de archivo de formato específico, como .xml o .json.</span><span class="sxs-lookup"><span data-stu-id="e1624-206">Clients can request a particular format as part of the URL, such as in the query string or part of the path, or by using a format-specific file extension such as .xml or .json.</span></span> <span data-ttu-id="e1624-207">La asignación de ruta de acceso de solicitud debe especificarse en la ruta que es utilizando la API.</span><span class="sxs-lookup"><span data-stu-id="e1624-207">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="e1624-208">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="e1624-208">For example:</span></span>

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

<span data-ttu-id="e1624-209">Esta ruta permitiría que el formato solicitado estar especificado como una extensión de archivo opcional.</span><span class="sxs-lookup"><span data-stu-id="e1624-209">This route would allow the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="e1624-210">El `[FormatFilter]` atributo comprueba la existencia del valor de formato en el `RouteData` y asignará el formato de respuesta al formateador adecuado cuando se crea la respuesta.</span><span class="sxs-lookup"><span data-stu-id="e1624-210">The `[FormatFilter]` attribute checks for the existence of the format value in the `RouteData` and will map the response format to the appropriate formatter when the response is created.</span></span>

| <span data-ttu-id="e1624-211">Ruta</span><span class="sxs-lookup"><span data-stu-id="e1624-211">Route</span></span>                      | <span data-ttu-id="e1624-212">Formateador</span><span class="sxs-lookup"><span data-stu-id="e1624-212">Formatter</span></span>                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | <span data-ttu-id="e1624-213">El formateador de salida predeterminado</span><span class="sxs-lookup"><span data-stu-id="e1624-213">The default output formatter</span></span>       |
| `/products/GetById/5.json` | <span data-ttu-id="e1624-214">El formateador JSON (si está configurado)</span><span class="sxs-lookup"><span data-stu-id="e1624-214">The JSON formatter (if configured)</span></span> |
| `/products/GetById/5.xml`  | <span data-ttu-id="e1624-215">El formateador XML (si está configurado)</span><span class="sxs-lookup"><span data-stu-id="e1624-215">The XML formatter (if configured)</span></span>  |