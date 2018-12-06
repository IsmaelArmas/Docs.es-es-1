---
title: Prácticas recomendadas de rendimiento de ASP.NET Core
author: mjrousos
description: Sugerencias para aumentar el rendimiento en aplicaciones ASP.NET Core y evitar problemas de rendimiento comunes.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 11/29/2018
uid: performance/performance-best-practices
ms.openlocfilehash: 9f3ed97bf4d4eb371ff5ae3874234b44745cc4ca
ms.sourcegitcommit: 0fc89b80bb1952852ecbcf3c5c156459b02a6ceb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52618121"
---
# <a name="aspnet-core-performance-best-practices"></a><span data-ttu-id="5b3db-103">Prácticas recomendadas de rendimiento de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b3db-103">ASP.NET Core Performance Best Practices</span></span>

<span data-ttu-id="5b3db-104">Por [Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="5b3db-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="5b3db-105">Este tema proporciona instrucciones para el rendimiento de los procedimientos recomendados con ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b3db-105">This topic provides guidelines for performance best practices with ASP.NET Core.</span></span>

<a name="hot"></a>
<span data-ttu-id="5b3db-106"><!-- TODO review hot code paths is jargon that won't MT (machine translate) and is not well defined for native speakers. --> En este documento, una ruta de acceso de código activo se define como una ruta de acceso del código que se llama con frecuencia y donde ocurre gran parte del tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="5b3db-106"><!-- TODO review hot code paths is jargon that won't MT (machine translate) and is not well defined for native speakers. --> In this document, a hot code path is defined as a code path that is frequently called and where much of the execution time occurs.</span></span> <span data-ttu-id="5b3db-107">Las rutas de acceso frecuente de código normalmente limitan la escalabilidad horizontal de la aplicación y el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-107">Hot code paths typically limit app scale-out and performance.</span></span>

## <a name="cache-aggressively"></a><span data-ttu-id="5b3db-108">Caché de forma agresiva</span><span class="sxs-lookup"><span data-stu-id="5b3db-108">Cache aggressively</span></span>

<span data-ttu-id="5b3db-109">Almacenamiento en caché se analiza en varias partes de este documento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-109">Caching is discussed in several parts of this document.</span></span> <span data-ttu-id="5b3db-110">Para obtener más información, consulta <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="5b3db-110">For more information, see <xref:performance/caching/response>.</span></span>

## <a name="avoid-blocking-calls"></a><span data-ttu-id="5b3db-111">Evitar llamadas de bloqueo</span><span class="sxs-lookup"><span data-stu-id="5b3db-111">Avoid blocking calls</span></span>

<span data-ttu-id="5b3db-112">Aplicaciones de ASP.NET Core deben diseñarse para procesar muchas solicitudes simultáneamente.</span><span class="sxs-lookup"><span data-stu-id="5b3db-112">ASP.NET Core apps should be designed to process many requests simultaneously.</span></span> <span data-ttu-id="5b3db-113">Las API asincrónicas permiten un pequeño grupo de subprocesos para controlar miles de solicitudes simultáneas por no Esperando llamadas de bloqueo.</span><span class="sxs-lookup"><span data-stu-id="5b3db-113">Asynchronous APIs allow a small pool of threads to handle thousands of concurrent requests by not waiting on blocking calls.</span></span> <span data-ttu-id="5b3db-114">En lugar de esperar en una tarea de larga ejecución sincrónica para completar, el subproceso puede trabajar en otra solicitud.</span><span class="sxs-lookup"><span data-stu-id="5b3db-114">Rather than waiting on a long-running synchronous task to complete, the thread can work on another request.</span></span>

<span data-ttu-id="5b3db-115">Un problema de rendimiento comunes en aplicaciones ASP.NET Core está bloqueando las llamadas que podrían ser asincrónicas.</span><span class="sxs-lookup"><span data-stu-id="5b3db-115">A common performance problem in ASP.NET Core apps is blocking calls that could be asynchronous.</span></span> <span data-ttu-id="5b3db-116">Muchas llamadas bloqueos sincrónicas conduce a [privación de grupos de subprocesos](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) y degradar los tiempos de respuesta.</span><span class="sxs-lookup"><span data-stu-id="5b3db-116">Many synchronous blocking calls leads to [Thread Pool starvation](https://blogs.msdn.microsoft.com/vancem/2018/10/16/diagnosing-net-core-threadpool-starvation-with-perfview-why-my-service-is-not-saturating-all-cores-or-seems-to-stall/) and degrading response times.</span></span>

<span data-ttu-id="5b3db-117">**No**:</span><span class="sxs-lookup"><span data-stu-id="5b3db-117">**Do not**:</span></span>

* <span data-ttu-id="5b3db-118">Bloquear la ejecución asincrónica mediante una llamada a [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) o [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).</span><span class="sxs-lookup"><span data-stu-id="5b3db-118">Block asynchronous execution by calling [Task.Wait](/dotnet/api/system.threading.tasks.task.wait) or [Task.Result](/dotnet/api/system.threading.tasks.task-1.result).</span></span>
* <span data-ttu-id="5b3db-119">Adquirir bloqueos en las rutas de código común.</span><span class="sxs-lookup"><span data-stu-id="5b3db-119">Acquire locks in common code paths.</span></span> <span data-ttu-id="5b3db-120">Las aplicaciones de ASP.NET Core son de mayor rendimiento cuando se ha diseñado como para ejecutar código en paralelo.</span><span class="sxs-lookup"><span data-stu-id="5b3db-120">ASP.NET Core apps are most performant when architected to run code in parallel.</span></span>

<span data-ttu-id="5b3db-121">**Hacer**:</span><span class="sxs-lookup"><span data-stu-id="5b3db-121">**Do**:</span></span>

* <span data-ttu-id="5b3db-122">Asegúrese de ["hot" las rutas de código](#hot) asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5b3db-122">Make [hot code paths](#hot) asynchronous.</span></span>
* <span data-ttu-id="5b3db-123">Llamar de forma asincrónica el acceso a datos y las API de operaciones de larga ejecución.</span><span class="sxs-lookup"><span data-stu-id="5b3db-123">Call data access and long-running operations APIs asynchronously.</span></span>
* <span data-ttu-id="5b3db-124">Asegúrese de controlador/Razor acciones de la página asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5b3db-124">Make controller/Razor Page actions asynchronous.</span></span> <span data-ttu-id="5b3db-125">Debe ser asincrónica con el fin de beneficiarse de la pila de llamadas completa [async y await](/dotnet/csharp/programming-guide/concepts/async/) patrones.</span><span class="sxs-lookup"><span data-stu-id="5b3db-125">The entire call stack needs to be asynchronous in order to benefit from [async/await](/dotnet/csharp/programming-guide/concepts/async/) patterns.</span></span>

<span data-ttu-id="5b3db-126">Al igual que un generador de perfiles [PerfView](https://github.com/Microsoft/perfview) puede usarse para buscar subprocesos con frecuencia que se va a agregar a la [grupos de subprocesos de](/windows/desktop/procthread/thread-pool).</span><span class="sxs-lookup"><span data-stu-id="5b3db-126">A profiler like [PerfView](https://github.com/Microsoft/perfview) can be used to look for threads frequently being added to the [Thread Pool](/windows/desktop/procthread/thread-pool).</span></span> <span data-ttu-id="5b3db-127">El `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` evento indica que un subproceso que se agrega al grupo de subprocesos.</span><span class="sxs-lookup"><span data-stu-id="5b3db-127">The `Microsoft-Windows-DotNETRuntime/ThreadPoolWorkerThread/Start` event indicates a thread being added to the thread pool.</span></span> <!--  For more information, see [async guidance docs](TBD-Link_To_Davifowl_Doc  -->

## <a name="minimize-large-object-allocations"></a><span data-ttu-id="5b3db-128">Minimizar las asignaciones de objetos grandes</span><span class="sxs-lookup"><span data-stu-id="5b3db-128">Minimize large object allocations</span></span>

<span data-ttu-id="5b3db-129"><!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core --> El [recolector de elementos no utilizados de .NET Core](/dotnet/standard/garbage-collection/) administra la asignación y liberación de memoria automáticamente en aplicaciones ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b3db-129"><!-- TODO review Bill - replaced original .NET language below with .NET Core since this targets .NET Core --> The [.NET Core garbage collector](/dotnet/standard/garbage-collection/) manages allocation and release of memory automatically in ASP.NET Core apps.</span></span> <span data-ttu-id="5b3db-130">Colección de elementos no utilizados automática generalmente significa que los desarrolladores no tienen que preocuparse por cómo y cuándo se libera memoria.</span><span class="sxs-lookup"><span data-stu-id="5b3db-130">Automatic garbage collection generally means that developers don't need to worry about how or when memory is freed.</span></span> <span data-ttu-id="5b3db-131">Sin embargo, limpiar los objetos sin referencia necesita tiempo de CPU, por lo que los desarrolladores deben minimizar la asignación de objetos en ["hot" las rutas de código](#hot).</span><span class="sxs-lookup"><span data-stu-id="5b3db-131">However, cleaning up unreferenced objects takes CPU time, so developers should minimize allocating objects in [hot code paths](#hot).</span></span> <span data-ttu-id="5b3db-132">Colección de elementos no utilizados es especialmente costosa en objetos grandes (bytes > 85 KB).</span><span class="sxs-lookup"><span data-stu-id="5b3db-132">Garbage collection is especially expensive on large objects (> 85 K bytes).</span></span> <span data-ttu-id="5b3db-133">Los objetos grandes se almacenan en el [montón de objetos grandes](/dotnet/standard/garbage-collection/large-object-heap) y requieren un completo (generación 2) recolección de elementos para limpiar.</span><span class="sxs-lookup"><span data-stu-id="5b3db-133">Large objects are stored on the [large object heap](/dotnet/standard/garbage-collection/large-object-heap) and require a full (generation 2) garbage collection to clean up.</span></span> <span data-ttu-id="5b3db-134">A diferencia de las colecciones de generación 1 y generación 0, una recolección de generación 2 requiere la ejecución de la aplicación va a suspender temporalmente.</span><span class="sxs-lookup"><span data-stu-id="5b3db-134">Unlike generation 0 and generation 1 collections, a generation 2 collection requires app execution to be temporarily suspended.</span></span> <span data-ttu-id="5b3db-135">Frecuente asignación y desasignación de objetos grandes pueden provocar un rendimiento incoherente.</span><span class="sxs-lookup"><span data-stu-id="5b3db-135">Frequent allocation and de-allocation of large objects can cause inconsistent performance.</span></span>

<span data-ttu-id="5b3db-136">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-136">Recommendations:</span></span>

* <span data-ttu-id="5b3db-137">**Hacer** considere la posibilidad de almacenamiento en caché de objetos grandes que se utilizan con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="5b3db-137">**Do** consider caching large objects that are frequently used.</span></span> <span data-ttu-id="5b3db-138">Almacenamiento en caché de objetos grandes evita que las asignaciones de costosas.</span><span class="sxs-lookup"><span data-stu-id="5b3db-138">Caching large objects prevents expensive allocations.</span></span>
* <span data-ttu-id="5b3db-139">**Hacer** grupo de búferes mediante el uso de un [ `ArrayPool<T>` ](/dotnet/api/system.buffers.arraypool-1) para almacenar matrices de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="5b3db-139">**Do** pool buffers by using an [`ArrayPool<T>`](/dotnet/api/system.buffers.arraypool-1) to store large arrays.</span></span>
* <span data-ttu-id="5b3db-140">**No** asignar muchos y de corta duración de objetos grandes en ["hot" las rutas de código](#hot).</span><span class="sxs-lookup"><span data-stu-id="5b3db-140">**Do not** allocate many, short-lived large objects on [hot code paths](#hot).</span></span>

<span data-ttu-id="5b3db-141">Problemas de memoria, como los anteriores se pueden diagnosticar revisando las estadísticas de elementos no utilizados (GC) de la colección en [PerfView](https://github.com/Microsoft/perfview) y examinar:</span><span class="sxs-lookup"><span data-stu-id="5b3db-141">Memory issues like the preceding can be diagnosed by reviewing garbage collection (GC) stats in [PerfView](https://github.com/Microsoft/perfview) and examining:</span></span>

* <span data-ttu-id="5b3db-142">Tiempo de pausa de la colección de elementos no utilizados.</span><span class="sxs-lookup"><span data-stu-id="5b3db-142">Garbage collection pause time.</span></span>
* <span data-ttu-id="5b3db-143">¿Qué porcentaje de tiempo del procesador se invierte en la recolección de elementos.</span><span class="sxs-lookup"><span data-stu-id="5b3db-143">What percentage of the processor time is spent in garbage collection.</span></span>
* <span data-ttu-id="5b3db-144">¿Cuántas colecciones de elementos no utilizados son la generación 0, 1 y 2.</span><span class="sxs-lookup"><span data-stu-id="5b3db-144">How many garbage collections are generation 0, 1, and 2.</span></span>

<span data-ttu-id="5b3db-145">Para obtener más información, consulte [colección de elementos no utilizados y rendimiento](/dotnet/standard/garbage-collection/performance).</span><span class="sxs-lookup"><span data-stu-id="5b3db-145">For more information, see [Garbage Collection and Performance](/dotnet/standard/garbage-collection/performance).</span></span>

## <a name="optimize-data-access"></a><span data-ttu-id="5b3db-146">Optimizar el acceso a datos</span><span class="sxs-lookup"><span data-stu-id="5b3db-146">Optimize Data Access</span></span>

<span data-ttu-id="5b3db-147">Las interacciones con otros servicios remotos o un almacén de datos suelen ser la parte más lenta de una aplicación ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b3db-147">Interactions with a data store or other remote services are often the slowest part of an ASP.NET Core app.</span></span> <span data-ttu-id="5b3db-148">Leer y escribir datos de forma eficaz son fundamental para un buen rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-148">Reading and writing data efficiently is critical for good performance.</span></span>

<span data-ttu-id="5b3db-149">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-149">Recommendations:</span></span>

* <span data-ttu-id="5b3db-150">**Hacer** llamar a todas las API de acceso de datos de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5b3db-150">**Do** call all data access APIs asynchronously.</span></span>
* <span data-ttu-id="5b3db-151">**No** recuperar más datos que es necesario.</span><span class="sxs-lookup"><span data-stu-id="5b3db-151">**Do not** retrieve more data than is necessary.</span></span> <span data-ttu-id="5b3db-152">Escribir consultas para devolver solamente los datos que es necesarios para la solicitud HTTP actual.</span><span class="sxs-lookup"><span data-stu-id="5b3db-152">Write queries to return just the data that is necessary for the current HTTP request.</span></span>
* <span data-ttu-id="5b3db-153">**Hacer** considere la posibilidad de almacenamiento en caché con frecuencia tiene acceso a los datos recuperados de una base de datos o servicio remoto, si es aceptable para los datos que está un poco anticuado.</span><span class="sxs-lookup"><span data-stu-id="5b3db-153">**Do** consider caching frequently accessed data retrieved from a database or remote service if it is acceptable for the data to be slightly out-of-date.</span></span> <span data-ttu-id="5b3db-154">Según el escenario, puede usar un [MemoryCache](xref:performance/caching/memory) o un [DistributedCache](xref:performance/caching/distributed).</span><span class="sxs-lookup"><span data-stu-id="5b3db-154">Depending on the scenario, you might use a [MemoryCache](xref:performance/caching/memory) or a [DistributedCache](xref:performance/caching/distributed).</span></span> <span data-ttu-id="5b3db-155">Para obtener más información, consulta <xref:performance/caching/response>.</span><span class="sxs-lookup"><span data-stu-id="5b3db-155">For more information, see <xref:performance/caching/response>.</span></span>
* <span data-ttu-id="5b3db-156">Minimizar la ida y vuelta de red.</span><span class="sxs-lookup"><span data-stu-id="5b3db-156">Minimize network round trips.</span></span> <span data-ttu-id="5b3db-157">El objetivo es recuperar todos los datos que se necesitará en una sola llamada en lugar de varias llamadas.</span><span class="sxs-lookup"><span data-stu-id="5b3db-157">The goal is to retrieve all the data that will be needed in a single call rather than several calls.</span></span>
* <span data-ttu-id="5b3db-158">**Hacer** usar [consultas de no seguimiento](/ef/core/querying/tracking#no-tracking-queries) en Entity Framework Core al obtener acceso a datos de solo lectura.</span><span class="sxs-lookup"><span data-stu-id="5b3db-158">**Do** use [no-tracking queries](/ef/core/querying/tracking#no-tracking-queries) in Entity Framework Core when accessing data for read-only purposes.</span></span> <span data-ttu-id="5b3db-159">EF Core puede devolver los resultados de consultas no realizar un seguimiento de forma más eficaz.</span><span class="sxs-lookup"><span data-stu-id="5b3db-159">EF Core can return the results of no-tracking queries more efficiently.</span></span>
* <span data-ttu-id="5b3db-160">**Hacer** agregadas consultas LINQ y filtro (con `.Where`, `.Select`, o `.Sum` las instrucciones, por ejemplo) para que el filtrado se realiza mediante la base de datos.</span><span class="sxs-lookup"><span data-stu-id="5b3db-160">**Do** filter and aggregate LINQ queries (with `.Where`, `.Select`, or `.Sum` statements, for example) so that the filtering is done by the database.</span></span>
* <span data-ttu-id="5b3db-161">**Hacer** considere la posibilidad de que EF Core resuelve algunos operadores de consulta en el cliente, lo que puede provocar la ejecución de consultas ineficaces.</span><span class="sxs-lookup"><span data-stu-id="5b3db-161">**Do** consider that EF Core resolves some query operators on the client, which may lead to inefficient query execution.</span></span> <span data-ttu-id="5b3db-162">Para obtener más información, consulte [problemas de rendimiento de evaluación de cliente](/ef/core/querying/client-eval#client-evaluation-performance-issues)</span><span class="sxs-lookup"><span data-stu-id="5b3db-162">For more information, see [Client evaluation performance issues](/ef/core/querying/client-eval#client-evaluation-performance-issues)</span></span>
* <span data-ttu-id="5b3db-163">**No** utilizar consultas de proyección en colecciones, que pueden dar lugar a ejecución de "N + 1" consultas SQL.</span><span class="sxs-lookup"><span data-stu-id="5b3db-163">**Do not** use projection queries on collections, which can result in executing "N + 1" SQL queries.</span></span> <span data-ttu-id="5b3db-164">Para obtener más información, consulte [optimización de subconsultas correlacionadas](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).</span><span class="sxs-lookup"><span data-stu-id="5b3db-164">For more information, see [Optimization of correlated subqueries](/ef/core/what-is-new/ef-core-2.1#optimization-of-correlated-subqueries).</span></span>

<span data-ttu-id="5b3db-165">Consulte [EF de alto rendimiento](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) enfoques que puede mejorar el rendimiento de aplicaciones a gran escala:</span><span class="sxs-lookup"><span data-stu-id="5b3db-165">See [EF High Performance](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries) for approaches that may improve performance in high-scale apps:</span></span>

* [<span data-ttu-id="5b3db-166">Agrupación de DbContext</span><span class="sxs-lookup"><span data-stu-id="5b3db-166">DbContext pooling</span></span>](/ef/core/what-is-new/ef-core-2.0#dbcontext-pooling)
* [<span data-ttu-id="5b3db-167">Consultas compiladas de manera explícita</span><span class="sxs-lookup"><span data-stu-id="5b3db-167">Explicitly compiled queries</span></span>](/ef/core/what-is-new/ef-core-2.0#explicitly-compiled-queries)

<span data-ttu-id="5b3db-168">Se recomienda que medir el impacto de los enfoques de alto rendimiento anteriores antes de confirmar la base de código.</span><span class="sxs-lookup"><span data-stu-id="5b3db-168">We recommend you measure the impact of the preceding high-performance approaches before committing to your code base.</span></span> <span data-ttu-id="5b3db-169">Es posible que la complejidad adicional de las consultas compiladas no justifiquen la mejora del rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-169">The additional complexity of compiled queries may not justify the performance improvement.</span></span>

<span data-ttu-id="5b3db-170">Consulta que se pueden detectar problemas mediante la revisión de tiempo dedicado a obtener acceso a datos con [Application Insights](/azure/application-insights/app-insights-overview) o con herramientas de generación de perfiles.</span><span class="sxs-lookup"><span data-stu-id="5b3db-170">Query issues can be detected by reviewing time spent accessing data with [Application Insights](/azure/application-insights/app-insights-overview) or with profiling tools.</span></span> <span data-ttu-id="5b3db-171">La mayoría de las bases de datos también disponen de las estadísticas relativas a las consultas ejecutadas con frecuencia.</span><span class="sxs-lookup"><span data-stu-id="5b3db-171">Most databases also make statistics available concerning frequently executed queries.</span></span>

## <a name="pool-http-connections-with-httpclientfactory"></a><span data-ttu-id="5b3db-172">Conexiones de grupo HTTP con HttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="5b3db-172">Pool HTTP connections with HttpClientFactory</span></span>

<span data-ttu-id="5b3db-173">Aunque [HttpClient](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) implementa el `IDisposable` interfaz, está pensado para volver a utilizarse.</span><span class="sxs-lookup"><span data-stu-id="5b3db-173">Although [HttpClient](/dotnet/api/system.net.http.httpclient?view=netstandard-2.0) implements the `IDisposable` interface, it's meant to be reused.</span></span> <span data-ttu-id="5b3db-174">Cerrado `HttpClient` instancias deje sockets abiertos en el `TIME_WAIT` estado durante un breve período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="5b3db-174">Closed `HttpClient` instances leave sockets open in the `TIME_WAIT` state for a short period of time.</span></span> <span data-ttu-id="5b3db-175">Por lo tanto, si una ruta de acceso del código que crea y se desecha `HttpClient` objetos se usa con frecuencia, la aplicación puede agotar sockets disponibles.</span><span class="sxs-lookup"><span data-stu-id="5b3db-175">Consequently, if a code path that creates and disposes of `HttpClient` objects is frequently used, the app may exhaust available sockets.</span></span> <span data-ttu-id="5b3db-176">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) se introdujo en ASP.NET Core 2.1 como una solución a este problema.</span><span class="sxs-lookup"><span data-stu-id="5b3db-176">[HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) was introduced in ASP.NET Core 2.1 as a solution to this problem.</span></span> <span data-ttu-id="5b3db-177">Controla la agrupación de conexiones de HTTP para optimizar el rendimiento y confiabilidad.</span><span class="sxs-lookup"><span data-stu-id="5b3db-177">It handles pooling HTTP connections to optimize performance and reliability.</span></span>

<span data-ttu-id="5b3db-178">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-178">Recommendations:</span></span>

* <span data-ttu-id="5b3db-179">**No** crear y eliminar `HttpClient` instancias directamente.</span><span class="sxs-lookup"><span data-stu-id="5b3db-179">**Do not** create and dispose of `HttpClient` instances directly.</span></span>
* <span data-ttu-id="5b3db-180">**Hacer** usar [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) para recuperar `HttpClient` instancias.</span><span class="sxs-lookup"><span data-stu-id="5b3db-180">**Do** use [HttpClientFactory](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests) to retrieve `HttpClient` instances.</span></span> <span data-ttu-id="5b3db-181">Para obtener más información, consulte [HttpClientFactory de uso para implementar las solicitudes HTTP resistentes](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).</span><span class="sxs-lookup"><span data-stu-id="5b3db-181">For more information, see [Use HttpClientFactory to implement resilient HTTP requests](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests).</span></span>

## <a name="keep-common-code-paths-fast"></a><span data-ttu-id="5b3db-182">Mantener las rutas de código comunes rápidas</span><span class="sxs-lookup"><span data-stu-id="5b3db-182">Keep common code paths fast</span></span>

<span data-ttu-id="5b3db-183">Desea que todo el código sea rápido, pero las rutas de código se llama con frecuencia son los más importantes para optimizar:</span><span class="sxs-lookup"><span data-stu-id="5b3db-183">You want all of your code to be fast, but frequently called code paths are the most critical to optimize:</span></span>

* <span data-ttu-id="5b3db-184">Componentes de middleware en la canalización de procesamiento de solicitudes de la aplicación, especialmente middleware que se ejecute al principio de la canalización.</span><span class="sxs-lookup"><span data-stu-id="5b3db-184">Middleware components in the app's request processing pipeline, especially middleware run early in the pipeline.</span></span> <span data-ttu-id="5b3db-185">Estos componentes tienen un gran impacto en el rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-185">These components have a large impact on performance.</span></span>
* <span data-ttu-id="5b3db-186">Código que se ejecuta para cada solicitud o varias veces por solicitud.</span><span class="sxs-lookup"><span data-stu-id="5b3db-186">Code that is executed for every request or multiple times per request.</span></span> <span data-ttu-id="5b3db-187">Por ejemplo, el registro personalizado, los controladores de autorización o inicialización de los servicios transitorios.</span><span class="sxs-lookup"><span data-stu-id="5b3db-187">For example, custom logging, authorization handlers, or initialization of transient services.</span></span>

<span data-ttu-id="5b3db-188">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-188">Recommendations:</span></span>

* <span data-ttu-id="5b3db-189">**No** usar componentes de middleware personalizado con tareas de ejecución prolongada.</span><span class="sxs-lookup"><span data-stu-id="5b3db-189">**Do not** use custom middleware components with long-running tasks.</span></span>
* <span data-ttu-id="5b3db-190">**Hacer** usar herramientas de generación de perfiles de rendimiento (como [herramientas de diagnóstico de Visual Studio](/visualstudio/profiling/profiling-feature-tour) o [PerfView](https://github.com/Microsoft/perfview)) para identificar ["hot" las rutas de código](#hot).</span><span class="sxs-lookup"><span data-stu-id="5b3db-190">**Do** use performance profiling tools (like [Visual Studio Diagnostic Tools](/visualstudio/profiling/profiling-feature-tour) or [PerfView](https://github.com/Microsoft/perfview)) to identify [hot code paths](#hot).</span></span>

## <a name="complete-long-running-tasks-outside-of-http-requests"></a><span data-ttu-id="5b3db-191">Completar larga tareas fuera de las solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="5b3db-191">Complete long-running Tasks outside of HTTP requests</span></span>

<span data-ttu-id="5b3db-192">La mayoría de las solicitudes a una aplicación ASP.NET Core pueden controlarse mediante un controlador o un modelo de página de una llamada a los servicios necesarios y devolver una respuesta HTTP.</span><span class="sxs-lookup"><span data-stu-id="5b3db-192">Most requests to an ASP.NET Core app can be handled by a controller or page model calling necessary services and returning an HTTP response.</span></span> <span data-ttu-id="5b3db-193">Para algunas solicitudes que implican las tareas de larga ejecución, es mejor facilitar el proceso de respuesta de solicitud completa asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5b3db-193">For some requests that involve long-running tasks, it's better to make the entire request-response process asynchronous.</span></span>

<span data-ttu-id="5b3db-194">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-194">Recommendations:</span></span>

* <span data-ttu-id="5b3db-195">**No** esperar a las tareas de ejecución prolongada llevar a cabo como parte del procesamiento normal de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="5b3db-195">**Do not** wait for long-running tasks to complete as part of ordinary HTTP request processing.</span></span>
* <span data-ttu-id="5b3db-196">**Hacer** considere la posibilidad de controlar las solicitudes de ejecución prolongada con [servicios en segundo plano](/aspnet/core/fundamentals/host/hosted-services) o fuera de proceso con un [Azure Function](/azure/azure-functions/).</span><span class="sxs-lookup"><span data-stu-id="5b3db-196">**Do** consider handling long-running requests with [background services](/aspnet/core/fundamentals/host/hosted-services) or out of process with an [Azure Function](/azure/azure-functions/).</span></span> <span data-ttu-id="5b3db-197">Completar fuera de proceso de trabajo es especialmente útil para las tareas que consumen más CPU.</span><span class="sxs-lookup"><span data-stu-id="5b3db-197">Completing work out-of-process is especially beneficial for CPU-intensive tasks.</span></span>
* <span data-ttu-id="5b3db-198">**Hacer** usar opciones de comunicación en tiempo real como [SignalR](xref:signalr/introduction) para comunicarse con clientes de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="5b3db-198">**Do** use real-time communication options like [SignalR](xref:signalr/introduction) to communicate with clients asynchronously.</span></span>

## <a name="minify-client-assets"></a><span data-ttu-id="5b3db-199">Minimizar los recursos del cliente</span><span class="sxs-lookup"><span data-stu-id="5b3db-199">Minify client assets</span></span>

<span data-ttu-id="5b3db-200">Aplicaciones ASP.NET Core con servidores front-end complejo con frecuencia servir muchos archivos de imagen, CSS o JavaScript.</span><span class="sxs-lookup"><span data-stu-id="5b3db-200">ASP.NET Core apps with complex front-ends frequently serve many JavaScript, CSS, or image files.</span></span> <span data-ttu-id="5b3db-201">Puede mejorar el rendimiento de las solicitudes de carga inicial por:</span><span class="sxs-lookup"><span data-stu-id="5b3db-201">Performance of initial load requests can be improved by:</span></span>

* <span data-ttu-id="5b3db-202">La unión, que combina varios archivos en uno.</span><span class="sxs-lookup"><span data-stu-id="5b3db-202">Bundling, which combines multiple files into one.</span></span>
* <span data-ttu-id="5b3db-203">Minificar, lo que reduce el tamaño de los archivos mediante.</span><span class="sxs-lookup"><span data-stu-id="5b3db-203">Minifying, which reduces the size of files by.</span></span>

<span data-ttu-id="5b3db-204">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-204">Recommendations:</span></span>

* <span data-ttu-id="5b3db-205">**Hacer** usar ASP.NET Core [compatibilidad integrada con](xref:client-side/bundling-and-minification) para agrupar y minificar recursos del cliente.</span><span class="sxs-lookup"><span data-stu-id="5b3db-205">**Do** use ASP.NET Core's [built-in support](xref:client-side/bundling-and-minification) for bundling and minifying client assets.</span></span>
* <span data-ttu-id="5b3db-206">**Hacer** considere la posibilidad de otras herramientas de terceros como [Gulp](uid:client-side/bundling-and-minification#consume-bundleconfigjson-from-gulp) o [Webpack](https://webpack.js.org/) más compleja administración de clientes activos.</span><span class="sxs-lookup"><span data-stu-id="5b3db-206">**Do** consider other third-party tools like [Gulp](uid:client-side/bundling-and-minification#consume-bundleconfigjson-from-gulp) or [Webpack](https://webpack.js.org/) for more complex client asset management.</span></span>

## <a name="use-the-latest-aspnet-core-release"></a><span data-ttu-id="5b3db-207">Use la versión más reciente de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b3db-207">Use the latest ASP.NET Core release</span></span>

<span data-ttu-id="5b3db-208">Cada nueva versión de ASP.NET incluye mejoras de rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-208">Each new release of ASP.NET includes performance improvements.</span></span> <span data-ttu-id="5b3db-209">Las optimizaciones en .NET Core y ASP.NET Core significan que las versiones más recientes se superan el rendimiento de las versiones anteriores.</span><span class="sxs-lookup"><span data-stu-id="5b3db-209">Optimizations in .NET Core and ASP.NET Core mean that newer versions will outperform older versions.</span></span> <span data-ttu-id="5b3db-210">Por ejemplo, .NET Core 2.1 agrega compatibilidad para expresiones regulares compiladas y saca partido de [ `Span<T>` ](https://msdn.microsoft.com/magazine/mt814808.aspx).</span><span class="sxs-lookup"><span data-stu-id="5b3db-210">For example, .NET Core 2.1 added support for compiled regular expressions and benefitted from [`Span<T>`](https://msdn.microsoft.com/magazine/mt814808.aspx).</span></span> <span data-ttu-id="5b3db-211">Compatibilidad de ASP.NET Core 2.2 agregado para HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="5b3db-211">ASP.NET Core 2.2 added support for HTTP/2.</span></span> <span data-ttu-id="5b3db-212">Si el rendimiento es una prioridad, considere la posibilidad de actualizar a la versión más reciente de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5b3db-212">If performance is a priority, consider upgrading to the most current version of ASP.NET Core.</span></span>

<!-- TODO review link and taking advantage of new [performance features](#TBD)
Maybe skip this TBD link as each version will have perf improvements -->

## <a name="minimize-exceptions"></a><span data-ttu-id="5b3db-213">Minimizar las excepciones</span><span class="sxs-lookup"><span data-stu-id="5b3db-213">Minimize exceptions</span></span>

<span data-ttu-id="5b3db-214">Las excepciones deben ser poco frecuentes.</span><span class="sxs-lookup"><span data-stu-id="5b3db-214">Exceptions should be rare.</span></span> <span data-ttu-id="5b3db-215">Iniciar y detectar excepciones son lento en relación con otros patrones de flujo de código.</span><span class="sxs-lookup"><span data-stu-id="5b3db-215">Throwing and catching exceptions is slow relative to other code flow patterns.</span></span> <span data-ttu-id="5b3db-216">Por este motivo, las excepciones no deben usarse para controlar el flujo normal del programa.</span><span class="sxs-lookup"><span data-stu-id="5b3db-216">Because of this, exceptions should not be used to control normal program flow.</span></span>

<span data-ttu-id="5b3db-217">Recomendaciones:</span><span class="sxs-lookup"><span data-stu-id="5b3db-217">Recommendations:</span></span>

* <span data-ttu-id="5b3db-218">**No** uso lanzar o capturar excepciones como medio de flujo del programa normal, especialmente en las rutas de código activo.</span><span class="sxs-lookup"><span data-stu-id="5b3db-218">**Do not** use throwing or catching exceptions as a means of normal program flow, especially in hot code paths.</span></span>
* <span data-ttu-id="5b3db-219">**Hacer** incluir lógica en la aplicación para detectar y controlar las condiciones que podrían provocar una excepción.</span><span class="sxs-lookup"><span data-stu-id="5b3db-219">**Do** include logic in the app to detect and handle conditions that would cause an exception.</span></span>
* <span data-ttu-id="5b3db-220">**Hacer** producir o detectar las excepciones para las condiciones inusuales o inesperadas.</span><span class="sxs-lookup"><span data-stu-id="5b3db-220">**Do** throw or catch exceptions for unusual or unexpected conditions.</span></span>

<span data-ttu-id="5b3db-221">Herramientas de diagnóstico de aplicaciones (por ejemplo, Application Insights) pueden ayudar a identificar las excepciones comunes en una aplicación que puede afectar al rendimiento.</span><span class="sxs-lookup"><span data-stu-id="5b3db-221">App diagnostic tools (like Application Insights) can help to identify common exceptions in an application which may affect performance.</span></span>