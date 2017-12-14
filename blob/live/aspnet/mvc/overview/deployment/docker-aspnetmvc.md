---
uid: mvc/overview/deployment/docker
title: Migrar aplicaciones de ASP.NET MVC a contenedores de Windows
description: "Aprenda a ejecutar una aplicación existente de ASP.NET MVC en un contenedor de Docker de Windows"
keywords: Windows Containers,Docker,ASP.NET MVC
author: BillWagner
ms.author: wiwagn
ms.date: 02/01/2017
ms.topic: article
ms.prod: .net-framework
ms.technology: dotnet-mvc
ms.devlang: dotnet
ms.assetid: c9f1d52c-b4bd-4b5d-b7f9-8f9ceaf778c4
ms.openlocfilehash: 336d0e9d2247dd2d63c779fd446f9a50be6dbc50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-aspnet-mvc-applications-to-windows-containers"></a><span data-ttu-id="29adf-104">Migrar aplicaciones de ASP.NET MVC a contenedores de Windows</span><span class="sxs-lookup"><span data-stu-id="29adf-104">Migrating ASP.NET MVC Applications to Windows Containers</span></span>

<span data-ttu-id="29adf-105">Ejecutar una aplicación existente basada en .NET Framework en un contenedor de Windows no requiere cambios en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29adf-105">Running an existing .NET Framework-based application in a Windows container doesn't require any changes to your app.</span></span> <span data-ttu-id="29adf-106">Para ejecutar la aplicación en un contenedor de Windows, crea una imagen de Docker que contenga la aplicación e inicie el contenedor.</span><span class="sxs-lookup"><span data-stu-id="29adf-106">To run your app in a Windows container you create a Docker image containing your app and start the container.</span></span> <span data-ttu-id="29adf-107">En este tema, se explican cómo realizar una [aplicación ASP.NET MVC](http://www.asp.net/mvc) existente e implementarla en un contenedor de Windows.</span><span class="sxs-lookup"><span data-stu-id="29adf-107">This topic explains how to take an existing [ASP.NET MVC application](http://www.asp.net/mvc) and deploy it in a Windows container.</span></span>

<span data-ttu-id="29adf-108">Comience con una aplicación existente de ASP.NET MVC y luego compile los recursos publicados mediante Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="29adf-108">You start with an existing ASP.NET MVC app, then build the published assets using Visual Studio.</span></span> <span data-ttu-id="29adf-109">Puede usar Docker para crear la imagen que contiene y ejecuta la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29adf-109">You use Docker to create the image that contains and runs your app.</span></span> <span data-ttu-id="29adf-110">Podrá ir al sitio que se ejecuta en un contenedor de Windows y comprobar que la aplicación funciona.</span><span class="sxs-lookup"><span data-stu-id="29adf-110">You'll browse to the site running in a Windows container and verify the app is working.</span></span>

<span data-ttu-id="29adf-111">En este artículo, se supone un conocimiento básico de Docker.</span><span class="sxs-lookup"><span data-stu-id="29adf-111">This article assumes a basic understanding of Docker.</span></span> <span data-ttu-id="29adf-112">Para más información sobre Docker, consulte [Docker Overview](https://docs.docker.com/engine/understanding-docker/) (Introducción a Docker).</span><span class="sxs-lookup"><span data-stu-id="29adf-112">You can learn about Docker by reading the [Docker Overview](https://docs.docker.com/engine/understanding-docker/).</span></span>

<span data-ttu-id="29adf-113">La aplicación que ejecutará en un contenedor es un sitio web sencillo que responde a preguntas de forma aleatoria.</span><span class="sxs-lookup"><span data-stu-id="29adf-113">The app you'll run in a container is a simple website that answers questions randomly.</span></span> <span data-ttu-id="29adf-114">Esta aplicación es una aplicación MVC básica sin autenticación ni almacenamiento de base de datos; esto le permite centrarse en mover la capa web a un contenedor.</span><span class="sxs-lookup"><span data-stu-id="29adf-114">This app is a basic MVC application with no authentication or database storage; it lets you focus on moving the web tier to a container.</span></span> <span data-ttu-id="29adf-115">En temas futuros, se le mostrará cómo mover y administrar el almacenamiento persistente en aplicaciones en contenedor.</span><span class="sxs-lookup"><span data-stu-id="29adf-115">Future topics will show how to move and manage persistent storage in containerized applications.</span></span>

<span data-ttu-id="29adf-116">Al mover la aplicación, se incluyen los siguientes pasos:</span><span class="sxs-lookup"><span data-stu-id="29adf-116">Moving your application involves these steps:</span></span>

1. [<span data-ttu-id="29adf-117">Crear una tarea de publicación para crear los recursos de una imagen.</span><span class="sxs-lookup"><span data-stu-id="29adf-117">Creating a publish task to build the assets for an image.</span></span>](#publish-script)
1. [<span data-ttu-id="29adf-118">Crear una imagen de Docker que ejecutará la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29adf-118">Building a Docker image that will run your application.</span></span>](#build-the-image)
1. [<span data-ttu-id="29adf-119">Iniciar un contenedor de Docker que ejecuta la imagen.</span><span class="sxs-lookup"><span data-stu-id="29adf-119">Starting a Docker container that runs your image.</span></span>](#start-a-container)
1. [<span data-ttu-id="29adf-120">Comprobar la aplicación mediante el explorador.</span><span class="sxs-lookup"><span data-stu-id="29adf-120">Verifying the application using your browser.</span></span>](#verify-in-the-browser)

<span data-ttu-id="29adf-121">La [aplicación finalizada](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) se encuentra en GitHub.</span><span class="sxs-lookup"><span data-stu-id="29adf-121">The [finished application](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator) is on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="29adf-122">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="29adf-122">Prerequisites</span></span>

<span data-ttu-id="29adf-123">La máquina de desarrollo debe de estar ejecutando</span><span class="sxs-lookup"><span data-stu-id="29adf-123">The development machine must be running</span></span>

- <span data-ttu-id="29adf-124">[Actualización de aniversario de Windows 10](https://www.microsoft.com/en-us/software-download/windows10/) (o superior) o [Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server) (o posterior).</span><span class="sxs-lookup"><span data-stu-id="29adf-124">[Windows 10 Anniversary Update](https://www.microsoft.com/en-us/software-download/windows10/) (or higher) or [Windows Server 2016](https://www.microsoft.com/en-us/cloud-platform/windows-server) (or higher).</span></span>
- <span data-ttu-id="29adf-125">[Docker para Windows](https://docs.docker.com/docker-for-windows/): versión 1.13.0 estable o 1.12 Beta 26 (o versiones más recientes)</span><span class="sxs-lookup"><span data-stu-id="29adf-125">[Docker for Windows](https://docs.docker.com/docker-for-windows/) - version Stable 1.13.0 or 1.12 Beta 26 (or newer versions)</span></span>
- <span data-ttu-id="29adf-126">[Visual Studio de 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="29adf-126">[Visual Studio 2017](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="29adf-127">Si usa Windows Server 2016, siga las instrucciones de [Implementación de host de contenedor - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span><span class="sxs-lookup"><span data-stu-id="29adf-127">If you are using Windows Server 2016, follow the instructions for [Container Host Deployment - Windows Server](https://msdn.microsoft.com/virtualization/windowscontainers/deployment/deployment).</span></span>

<span data-ttu-id="29adf-128">Después de instalar e iniciar Docker, haga clic con el botón derecho en el icono de bandeja y seleccione **Cambiar a contenedores de Windows**.</span><span class="sxs-lookup"><span data-stu-id="29adf-128">After installing and starting Docker,  right-click on the tray icon and select **Switch to Windows containers**.</span></span> <span data-ttu-id="29adf-129">Esto es necesario para ejecutar imágenes de Docker basadas en Windows.</span><span class="sxs-lookup"><span data-stu-id="29adf-129">This is required to run Docker images based on Windows.</span></span> <span data-ttu-id="29adf-130">Este comando tarda algunos segundos en ejecutarse:</span><span class="sxs-lookup"><span data-stu-id="29adf-130">This command takes a few seconds to execute:</span></span>

<span data-ttu-id="29adf-131">![Contenedor de Windows][windows-container]</span><span class="sxs-lookup"><span data-stu-id="29adf-131">![Windows Container][windows-container]</span></span>

## <a name="publish-script"></a><span data-ttu-id="29adf-132">Publicar script</span><span class="sxs-lookup"><span data-stu-id="29adf-132">Publish script</span></span>

<span data-ttu-id="29adf-133">Reúna en un mismo lugar todos los recursos que necesita cargar en una imagen de Docker.</span><span class="sxs-lookup"><span data-stu-id="29adf-133">Collect all the assets that you need to load into a Docker image in one place.</span></span> <span data-ttu-id="29adf-134">Puede usar el comando **Publicar** de Visual Studio para crear un perfil de publicación para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29adf-134">You can use the Visual Studio **Publish** command to create a publish profile for your app.</span></span> <span data-ttu-id="29adf-135">Este perfil incluirá todos los recursos en un árbol de directorio que copia en la imagen de destino más adelante en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="29adf-135">This profile will put all the assets in one directory tree that you copy to your target image later in this tutorial.</span></span>

<span data-ttu-id="29adf-136">**Pasos de publicación**</span><span class="sxs-lookup"><span data-stu-id="29adf-136">**Publish Steps**</span></span>

1. <span data-ttu-id="29adf-137">Haga clic con el botón derecho en el proyecto web en Visual Studio y seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="29adf-137">Right click on the web project in Visual Studio, and select **Publish**.</span></span>
1. <span data-ttu-id="29adf-138">Haga clic en el **botón Perfil personalizado** y después seleccione **Sistema de archivos** como método.</span><span class="sxs-lookup"><span data-stu-id="29adf-138">Click the **Custom profile button**, and then select **File System** as the method.</span></span>
1. <span data-ttu-id="29adf-139">Elija el directorio.</span><span class="sxs-lookup"><span data-stu-id="29adf-139">Choose the directory.</span></span> <span data-ttu-id="29adf-140">Por convención, el ejemplo descargado usa `bin\Release\PublishOutput`.</span><span class="sxs-lookup"><span data-stu-id="29adf-140">By convention, the downloaded sample uses `bin\Release\PublishOutput`.</span></span>

<span data-ttu-id="29adf-141">![Conexión de publicación][publish-connection]</span><span class="sxs-lookup"><span data-stu-id="29adf-141">![Publish Connection][publish-connection]</span></span>

<span data-ttu-id="29adf-142">Abra la sección **Opciones de publicación de archivos** de la pestaña **Configuración**. Seleccione **Precompilar durante la publicación**.</span><span class="sxs-lookup"><span data-stu-id="29adf-142">Open the **File Publish Options** section of the **Settings** tab. Select **Precompile during publishing**.</span></span> <span data-ttu-id="29adf-143">Esta optimización significa que compilará vistas en el contenedor de Docker, está copiando las vistas precompiladas.</span><span class="sxs-lookup"><span data-stu-id="29adf-143">This optimization means that you'll be compiling views in the Docker container, you are copying the precompiled views.</span></span>

<span data-ttu-id="29adf-144">![Configuración de publicación][publish-settings]</span><span class="sxs-lookup"><span data-stu-id="29adf-144">![Publish Settings][publish-settings]</span></span>

<span data-ttu-id="29adf-145">Haga clic en **Publicar** y Visual Studio copiará todos los recursos necesarios en la carpeta de destino.</span><span class="sxs-lookup"><span data-stu-id="29adf-145">Click **Publish**, and Visual Studio will copy all the needed assets to the destination folder.</span></span>

## <a name="build-the-image"></a><span data-ttu-id="29adf-146">Compilar la imagen</span><span class="sxs-lookup"><span data-stu-id="29adf-146">Build the image</span></span>

<span data-ttu-id="29adf-147">Defina la imagen de Docker en un Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="29adf-147">Define your Docker image in a Dockerfile.</span></span> <span data-ttu-id="29adf-148">El Dockerfile que contenga instrucciones para la imagen base, componentes adicionales, la aplicación que quiere ejecutar y otras imágenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="29adf-148">The Dockerfile contains instructions for the base image, additional components, the app you want to run, and other configuration images.</span></span>  <span data-ttu-id="29adf-149">El Dockerfile es la entrada al comando `docker build`, que crea la imagen.</span><span class="sxs-lookup"><span data-stu-id="29adf-149">The Dockerfile is the input to the `docker build` command, which creates the image.</span></span>

<span data-ttu-id="29adf-150">Creará una imagen basada en la imagen `microsft/aspnet` que se encuentra en [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span><span class="sxs-lookup"><span data-stu-id="29adf-150">You will build an image based on the `microsft/aspnet` image located on [Docker Hub](https://hub.docker.com/r/microsoft/aspnet/).</span></span>
<span data-ttu-id="29adf-151">La imagen base, `microsoft/aspnet`, es una imagen de Windows Server.</span><span class="sxs-lookup"><span data-stu-id="29adf-151">The base image, `microsoft/aspnet`, is a Windows Server image.</span></span> <span data-ttu-id="29adf-152">Contiene Windows Server Core, IIS y ASP.NET 4.6.2.</span><span class="sxs-lookup"><span data-stu-id="29adf-152">It contains Windows Server Core, IIS and ASP.NET 4.6.2.</span></span> <span data-ttu-id="29adf-153">Al ejecutar esta imagen en el contenedor, iniciará de forma automática IIS y los sitios web instalados.</span><span class="sxs-lookup"><span data-stu-id="29adf-153">When you run this image in your container, it will automatically start IIS and installed websites.</span></span>

<span data-ttu-id="29adf-154">El Dockerfile que crea la imagen tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="29adf-154">The Dockerfile that creates your image looks like this:</span></span>

```console
# The `FROM` instruction specifies the base image. You are
# extending the `microsoft/aspnet` image.

FROM microsoft/aspnet

# The final instruction copies the site you published earlier into the container.
COPY ./bin/Release/PublishOutput/ /inetpub/wwwroot
```

<span data-ttu-id="29adf-155">No hay ningún comando `ENTRYPOINT` en este Dockerfile.</span><span class="sxs-lookup"><span data-stu-id="29adf-155">There is no `ENTRYPOINT` command in this Dockerfile.</span></span> <span data-ttu-id="29adf-156">No se necesita ninguno.</span><span class="sxs-lookup"><span data-stu-id="29adf-156">You don't need one.</span></span> <span data-ttu-id="29adf-157">Cuando se ejecuta Windows Server con IIS, el proceso IIS es el punto de entrada, que está configurado para iniciarse en la imagen base de aspnet.</span><span class="sxs-lookup"><span data-stu-id="29adf-157">When running Windows Server with IIS, the IIS process is the entrypoint, which is configured to start in the aspnet base image.</span></span>

<span data-ttu-id="29adf-158">Ejecute el comando de compilación Docker para crear la imagen que se ejecuta en la aplicación ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="29adf-158">Run the Docker build command to create the image that runs your ASP.NET app.</span></span> <span data-ttu-id="29adf-159">Para ello, abra una ventana de PowerShell en el directorio del proyecto y escriba el siguiente comando en el directorio de la solución:</span><span class="sxs-lookup"><span data-stu-id="29adf-159">To do this, open a PowerShell window in the directory of your project and type the following command in the solution directory:</span></span>

```console
docker build -t mvcrandomanswers .
```

<span data-ttu-id="29adf-160">Este comando creará la nueva imagen con las instrucciones en el Dockerfile, nomenclatura (-t etiquetado) la imagen como mvcrandomanswers.</span><span class="sxs-lookup"><span data-stu-id="29adf-160">This command will build the new image using the instructions in your Dockerfile, naming (-t tagging) the image as mvcrandomanswers.</span></span> <span data-ttu-id="29adf-161">Esto puede incluir la extracción de la imagen base de [Docker Hub](http://hub.docker.com) y después agrega la aplicación a esa imagen.</span><span class="sxs-lookup"><span data-stu-id="29adf-161">This may include pulling the base image from [Docker Hub](http://hub.docker.com), and then adding your app to that image.</span></span>

<span data-ttu-id="29adf-162">Una vez que el comando finaliza, puede ejecutar el comando `docker images` para ver información sobre la nueva imagen:</span><span class="sxs-lookup"><span data-stu-id="29adf-162">Once that command completes, you can run the `docker images` command to see information on the new image:</span></span>

```console
REPOSITORY                    TAG                 IMAGE ID            CREATED             SIZE
mvcrandomanswers              latest              86838648aab6        2 minutes ago       10.1 GB
```

<span data-ttu-id="29adf-163">El IDENTIFICADOR DE LA IMAGEN será diferente en su equipo.</span><span class="sxs-lookup"><span data-stu-id="29adf-163">The IMAGE ID will be different on your machine.</span></span> <span data-ttu-id="29adf-164">Ahora, ejecutará la aplicación.</span><span class="sxs-lookup"><span data-stu-id="29adf-164">Now, let's run the app.</span></span>

## <a name="start-a-container"></a><span data-ttu-id="29adf-165">Iniciar un contenedor</span><span class="sxs-lookup"><span data-stu-id="29adf-165">Start a container</span></span>

<span data-ttu-id="29adf-166">Inicie un contenedor mediante la ejecución del siguiente comando `docker run`:</span><span class="sxs-lookup"><span data-stu-id="29adf-166">Start a container by executing the following `docker run` command:</span></span>

```console
docker run -d --name randomanswers mvcrandomanswers
```

<span data-ttu-id="29adf-167">El argumento `-d` indica a Docker que inicie la imagen en modo desasociado.</span><span class="sxs-lookup"><span data-stu-id="29adf-167">The `-d` argument tells Docker to start the image in detached mode.</span></span> <span data-ttu-id="29adf-168">Esto significa que la imagen de Docker se ejecuta desconectada del shell actual.</span><span class="sxs-lookup"><span data-stu-id="29adf-168">That means the Docker image runs disconnected from the current shell.</span></span>

<span data-ttu-id="29adf-169">En muchos ejemplos de docker, pueden sufrir -p para asignar los puertos de contenedor y el host.</span><span class="sxs-lookup"><span data-stu-id="29adf-169">In many docker examples, you may see -p to map the container and host ports.</span></span> <span data-ttu-id="29adf-170">La imagen de aspnet predeterminada ya configuró el contenedor para que escuche en el puerto 80 y exponerlo.</span><span class="sxs-lookup"><span data-stu-id="29adf-170">The default aspnet image has already configured the container to listen on port 80 and expose it.</span></span> 

<span data-ttu-id="29adf-171">El argumento `--name randomanswers` da un nombre al contenedor en ejecución.</span><span class="sxs-lookup"><span data-stu-id="29adf-171">The `--name randomanswers` gives a name to the running container.</span></span> <span data-ttu-id="29adf-172">Puede usar este nombre en lugar del identificador del contenedor en la mayoría de los comandos.</span><span class="sxs-lookup"><span data-stu-id="29adf-172">You can use this name instead of the container ID in most commands.</span></span>

<span data-ttu-id="29adf-173">El argumento `mvcrandomanswers` es el nombre de la imagen que se iniciará.</span><span class="sxs-lookup"><span data-stu-id="29adf-173">The `mvcrandomanswers` is the name of the image to start.</span></span>

## <a name="verify-in-the-browser"></a><span data-ttu-id="29adf-174">Comprobar en el explorador</span><span class="sxs-lookup"><span data-stu-id="29adf-174">Verify in the browser</span></span>

> [!NOTE]
> <span data-ttu-id="29adf-175">Con la versión actual del contenedor de Windows, no puede ir a `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="29adf-175">With the current Windows Container release, you can't browse to `http://localhost`.</span></span>
> <span data-ttu-id="29adf-176">Esto se debe a un comportamiento conocido en WinNAT y se resolverá en el futuro.</span><span class="sxs-lookup"><span data-stu-id="29adf-176">This is a known behavior in WinNAT, and it will be resolved in the future.</span></span> <span data-ttu-id="29adf-177">Hasta que se solucione, debe usar la dirección IP del contenedor.</span><span class="sxs-lookup"><span data-stu-id="29adf-177">Until that is addressed, you need to use the IP address of the container.</span></span>

<span data-ttu-id="29adf-178">Una vez iniciado el contenedor, encontrará su dirección IP para poder conectarse al contenedor en ejecución desde un explorador:</span><span class="sxs-lookup"><span data-stu-id="29adf-178">Once the container starts, find its IP address so that you can connect to your running container from a browser:</span></span>

```console
docker inspect -f "{{ .NetworkSettings.Networks.nat.IPAddress }}" randomanswers
172.31.194.61
```

<span data-ttu-id="29adf-179">Conectar con el contenedor en ejecución con la dirección IPv4, `http://172.31.194.61` en el ejemplo mostrado.</span><span class="sxs-lookup"><span data-stu-id="29adf-179">Connect to the running container using the IPv4 address, `http://172.31.194.61` in the example shown.</span></span> <span data-ttu-id="29adf-180">Escriba esa dirección URL en el explorador y debería ver el sitio en ejecución.</span><span class="sxs-lookup"><span data-stu-id="29adf-180">Type that URL into your browser, and you should see the running site.</span></span>

> [!NOTE]
> <span data-ttu-id="29adf-181">Algún software de proxy o VPN puede impedir que explore su sitio.</span><span class="sxs-lookup"><span data-stu-id="29adf-181">Some VPN or proxy software may prevent you from navigating to your site.</span></span>
> <span data-ttu-id="29adf-182">Puede deshabilitarlo de forma temporal para asegurarse de que el contenedor funciona.</span><span class="sxs-lookup"><span data-stu-id="29adf-182">You can temporarily disable it to make sure your container is working.</span></span>

<span data-ttu-id="29adf-183">El directorio de ejemplo en GitHub contiene un [script de PowerShell](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) que ejecuta estos comandos.</span><span class="sxs-lookup"><span data-stu-id="29adf-183">The sample directory on GitHub contains a [PowerShell script](https://github.com/dotnet/docs/tree/master/samples/framework/docker/MVCRandomAnswerGenerator/run.ps1) that executes these commands for you.</span></span> <span data-ttu-id="29adf-184">Abra una ventana de PowerShell, cambie el directorio por el directorio de la solución y escriba:</span><span class="sxs-lookup"><span data-stu-id="29adf-184">Open a PowerShell window, change directory to your solution directory, and type:</span></span>

```console
./run.ps1
```

<span data-ttu-id="29adf-185">El comando anterior crea la imagen, muestra la lista de imágenes del equipo, inicia un contenedor y muestra la dirección IP de ese contenedor.</span><span class="sxs-lookup"><span data-stu-id="29adf-185">The command above builds the image, displays the list of images on your machine, starts a container, and displays the IP address for that container.</span></span>

<span data-ttu-id="29adf-186">Para detener el contenedor, envíe un comando `docker
stop`:</span><span class="sxs-lookup"><span data-stu-id="29adf-186">To stop your container, issue a `docker
stop` command:</span></span>

```console
docker stop randomanswers
```

<span data-ttu-id="29adf-187">Para quitar el contenedor, envíe un comando `docker rm`:</span><span class="sxs-lookup"><span data-stu-id="29adf-187">To remove the container, issue a `docker rm` command:</span></span>

```console
docker rm randomanswers
```

[windows-container]: media/aspnetmvc/SwitchContainer.png "Cambiar al contenedor de Windows"
[publish-connection]: media/aspnetmvc/PublishConnection.png "Publicar en el sistema de archivos"
[publish-settings]: media/aspnetmvc/PublishSettings.png "Configuración de publicación"