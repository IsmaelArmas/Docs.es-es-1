---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding de ASP.NET en Visual Studio 2013 | Documentos de Microsoft
author: tfitzmac
description: "Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="eabe2-103">Scaffolding de ASP.NET en Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="eabe2-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="eabe2-104">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="eabe2-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="eabe2-105">Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="eabe2-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="eabe2-106">Información general</span><span class="sxs-lookup"><span data-stu-id="eabe2-106">Overview</span></span>

<span data-ttu-id="eabe2-107">La técnica de Scaffolding de ASP.NET es un marco de trabajo de generación de código para aplicaciones Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="eabe2-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="eabe2-108">Visual Studio 2013 incluye generadores de código previamente instalados para los proyectos de MVC y API Web.</span><span class="sxs-lookup"><span data-stu-id="eabe2-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="eabe2-109">Agregar scaffolding al proyecto cuando desee agregar rápidamente código que interactúe con modelos de datos.</span><span class="sxs-lookup"><span data-stu-id="eabe2-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="eabe2-110">Mediante scaffolding puede reducir la cantidad de tiempo para desarrollar las operaciones de datos estándar en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="eabe2-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="eabe2-111">De forma predeterminada, Visual Studio 2013 no admite la generación de código para un proyecto de formularios Web Forms, pero puede usar scaffolding con formularios Web Forms agregando las dependencias de MVC al proyecto o instalar una extensión.</span><span class="sxs-lookup"><span data-stu-id="eabe2-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="eabe2-112">A continuación se muestran ambos enfoques.</span><span class="sxs-lookup"><span data-stu-id="eabe2-112">Both approaches are shown below.</span></span>

<span data-ttu-id="eabe2-113">Visual Studio 2013 Update 2 (actualmente RC) proporciona la capacidad para extender el Scaffolding de ASP.NET para cumplir los requisitos de su escenario.</span><span class="sxs-lookup"><span data-stu-id="eabe2-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="eabe2-114">Con esta funcionalidad, puede crear una plantilla personalizada de scaffolding y agregarlo al cuadro de diálogo Agregar nuevo scaffolding.</span><span class="sxs-lookup"><span data-stu-id="eabe2-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="eabe2-115">Dentro de la plantilla personalizada, especifique el código que se genera cuando se agrega un elemento con scaffolding.</span><span class="sxs-lookup"><span data-stu-id="eabe2-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="eabe2-116">Para obtener más información, consulte [crear un Scaffolder personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="eabe2-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eabe2-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="eabe2-117">Prerequisites</span></span>

<span data-ttu-id="eabe2-118">Para usar la técnica Scaffolding de ASP.NET, debe tener:</span><span class="sxs-lookup"><span data-stu-id="eabe2-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="eabe2-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="eabe2-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="eabe2-120">Herramientas de desarrollador Web (parte de la instalación predeterminada de Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="eabe2-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="eabe2-121">Marcos de trabajo ASP.NET Web y herramientas de 2013 (parte de la instalación predeterminada de Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="eabe2-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="eabe2-122">Agregar un elemento con scaffolding para MVC o Web API</span><span class="sxs-lookup"><span data-stu-id="eabe2-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="eabe2-123">Para agregar un scaffold, haga clic en el proyecto o una carpeta dentro del proyecto y seleccione **agregar** : **nuevo elemento de scaffolding**, tal y como se muestra en la siguiente imagen.</span><span class="sxs-lookup"><span data-stu-id="eabe2-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Agregar elemento de scaffolding](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="eabe2-125">Desde el **agregar scaffolding** ventana, seleccione el tipo de scaffolding para agregar.</span><span class="sxs-lookup"><span data-stu-id="eabe2-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Seleccione el tipo de scaffolding](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="eabe2-127">El **Agregar controlador** ventana le ofrece la oportunidad de seleccionar opciones para generar el controlador, incluido si desea usar las nuevas características asincrónicas de Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="eabe2-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Agregar controlador](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="eabe2-129">Las clases relevantes y las páginas se crean para su escenario.</span><span class="sxs-lookup"><span data-stu-id="eabe2-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="eabe2-130">Por ejemplo, la siguiente imagen muestra el controlador MVC y vistas que se crearon a través de la técnica scaffolding para una clase de modelo denominada películas.</span><span class="sxs-lookup"><span data-stu-id="eabe2-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Los archivos creados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="eabe2-132">Agregar un elemento con scaffolding a formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="eabe2-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="eabe2-133">Para agregar scaffolding que genera el código de formularios Web Forms, debe instalar una extensión de Visual Studio o agregar las dependencias de MVC.</span><span class="sxs-lookup"><span data-stu-id="eabe2-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="eabe2-134">A continuación se muestran ambos enfoques, pero sólo necesite realizar uno de estos métodos.</span><span class="sxs-lookup"><span data-stu-id="eabe2-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="eabe2-135">Formularios Web Forms Scaffolding extensión</span><span class="sxs-lookup"><span data-stu-id="eabe2-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="eabe2-136">Puede instalar una extensión de Visual Studio que le permiten usar la técnica scaffolding con un proyecto de formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="eabe2-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="eabe2-137">En Visual Studio, seleccione **herramientas** y, a continuación, **extensiones y actualizaciones**.</span><span class="sxs-lookup"><span data-stu-id="eabe2-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="eabe2-138">En este cuadro de diálogo Buscar en la Galería de Visual Studio para **Scaffolding de Web Forms**.</span><span class="sxs-lookup"><span data-stu-id="eabe2-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![instalar formularios web forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="eabe2-140">Para obtener más información, consulte [Scaffolding de Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="eabe2-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="eabe2-141">Dependencias de MVC</span><span class="sxs-lookup"><span data-stu-id="eabe2-141">MVC Dependencies</span></span>

<span data-ttu-id="eabe2-142">Para agregar las dependencias de MVC, seleccione **agregar** - **nuevo elemento de scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="eabe2-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="eabe2-143">En la ventana Agregar scaffolding, seleccione **las dependencias de MVC**, tal y como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="eabe2-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![agregar las dependencias de MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="eabe2-145">Hay dos opciones de scaffolding MVC; Mínimo y completa.</span><span class="sxs-lookup"><span data-stu-id="eabe2-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="eabe2-146">Si selecciona mínimo, solo los paquetes de NuGet y referencias para ASP.NET MVC se agregan al proyecto.</span><span class="sxs-lookup"><span data-stu-id="eabe2-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="eabe2-147">Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.</span><span class="sxs-lookup"><span data-stu-id="eabe2-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="eabe2-148">Para utilizar fácilmente la técnica scaffolding, seleccione dependencias completas.</span><span class="sxs-lookup"><span data-stu-id="eabe2-148">To easily use scaffolding, select Full dependencies.</span></span>

![Seleccione dependencias completas](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="eabe2-150">Después de agregar las dependencias, verá un **readme.txt** archivo.</span><span class="sxs-lookup"><span data-stu-id="eabe2-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="eabe2-151">Siga cuidadosamente las instrucciones de este archivo para asegurarse de que el proyecto funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="eabe2-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="eabe2-152">Cuando se hayan completado los pasos descritos en el archivo readme.txt, puede agregar un nuevo elemento con scaffolding tal y como se muestra en la sección anterior sobre MVC y API Web.</span><span class="sxs-lookup"><span data-stu-id="eabe2-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="eabe2-153">Las vistas de generados automáticamente y el controlador funcionará correctamente dentro del proyecto.</span><span class="sxs-lookup"><span data-stu-id="eabe2-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="eabe2-154">Tutoriales</span><span class="sxs-lookup"><span data-stu-id="eabe2-154">Tutorials</span></span>

<span data-ttu-id="eabe2-155">Para crear un scaffolder personalizado, consulte [crear un Scaffolder personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="eabe2-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="eabe2-156">Para personalizar los archivos generados, vea [cómo personalizar los archivos generados desde el cuadro de diálogo nuevo elemento de scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="eabe2-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="eabe2-157">Para obtener un ejemplo del uso de la técnica scaffolding con **desarrollo Database First**, consulte [EF Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="eabe2-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="eabe2-158">Para obtener un ejemplo del uso de la técnica scaffolding en una **MVC** proyecto, vea [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="eabe2-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="eabe2-159">Para obtener un ejemplo del uso de la técnica scaffolding en una **API Web** proyecto, vea [crear una API de REST con el atributo de enrutamiento de Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="eabe2-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>