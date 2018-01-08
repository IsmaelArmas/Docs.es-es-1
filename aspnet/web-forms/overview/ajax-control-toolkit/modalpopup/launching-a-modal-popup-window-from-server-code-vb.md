---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "Inicie una ventana emergente Modal del código del servidor (VB) | Documentos de Microsoft"
author: wenz
description: El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente. Sin embargo, algunos escenarios requieren que t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="100ce-104">Inicie una ventana emergente Modal del código del servidor (VB)</span><span class="sxs-lookup"><span data-stu-id="100ce-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="100ce-105">por [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="100ce-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="100ce-106">[Descargar código](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) o [descarga de PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="100ce-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="100ce-107">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="100ce-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="100ce-108">Sin embargo algunos escenarios requieren que se desencadene la apertura de la ventana emergente modal en el lado del servidor.</span><span class="sxs-lookup"><span data-stu-id="100ce-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="100ce-109">Información general</span><span class="sxs-lookup"><span data-stu-id="100ce-109">Overview</span></span>

<span data-ttu-id="100ce-110">El control ModalPopup en el Kit de herramientas de Control de AJAX ofrece una manera sencilla de crear un elemento emergente modal mediante medios de lado cliente.</span><span class="sxs-lookup"><span data-stu-id="100ce-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="100ce-111">Sin embargo algunos escenarios requieren que se desencadene la apertura de la ventana emergente modal en el lado del servidor.</span><span class="sxs-lookup"><span data-stu-id="100ce-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="100ce-112">Pasos</span><span class="sxs-lookup"><span data-stu-id="100ce-112">Steps</span></span>

<span data-ttu-id="100ce-113">En primer lugar, se requiere un control de botón de ASP.NET web para demostrar cómo funciona el control ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="100ce-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="100ce-114">Agregar un botón de este tipo dentro de la &lt;formulario&gt; elemento en una página nueva:</span><span class="sxs-lookup"><span data-stu-id="100ce-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="100ce-115">A continuación, necesitará el marcado para la ventana emergente que desea crear.</span><span class="sxs-lookup"><span data-stu-id="100ce-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="100ce-116">Definir como un `<asp:Panel>` de control y asegúrese de que incluye un control de botón.</span><span class="sxs-lookup"><span data-stu-id="100ce-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="100ce-117">El control ModalPopup ofrece la funcionalidad necesaria para realizar este tipo un botón para cerrar la ventana emergente; en caso contrario, no hay ninguna manera fácil de dejar que desaparecer.</span><span class="sxs-lookup"><span data-stu-id="100ce-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="100ce-118">A continuación, agregue el control de ModalPopup desde el Kit de herramientas de AJAX de ASP.NET a la página.</span><span class="sxs-lookup"><span data-stu-id="100ce-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="100ce-119">Establecer propiedades para el botón que cargue el control, el botón que facilita el desaparecen y el identificador de la ventana emergente real.</span><span class="sxs-lookup"><span data-stu-id="100ce-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="100ce-120">Al igual que con todas las páginas web en función de AJAX de ASP.NET; el Administrador de scripts es necesario para cargar las bibliotecas de JavaScript necesarias para los exploradores de destino diferente:</span><span class="sxs-lookup"><span data-stu-id="100ce-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="100ce-121">Ejecutar el ejemplo en el explorador.</span><span class="sxs-lookup"><span data-stu-id="100ce-121">Run the example in the browser.</span></span> <span data-ttu-id="100ce-122">Al hacer clic en el botón, aparece el cuadro emergente modal.</span><span class="sxs-lookup"><span data-stu-id="100ce-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="100ce-123">Para lograr el mismo efecto mediante código del lado servidor, se requiere un nuevo botón:</span><span class="sxs-lookup"><span data-stu-id="100ce-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="100ce-124">Como puede ver, al hacer clic en el botón genera una devolución de datos y ejecuta el `ServerButton_Click()` método en el servidor.</span><span class="sxs-lookup"><span data-stu-id="100ce-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="100ce-125">En este método, llama a una función de JavaScript `launchModal()` se ejecuta para ser exactos, la función de JavaScript se ejecutará una vez que se ha cargado la página:</span><span class="sxs-lookup"><span data-stu-id="100ce-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="100ce-126">El trabajo de `launchModal()` es mostrar la ModalPopup.</span><span class="sxs-lookup"><span data-stu-id="100ce-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="100ce-127">El `launchModal()` se ejecuta la función una vez que se ha cargado la página HTML completa.</span><span class="sxs-lookup"><span data-stu-id="100ce-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="100ce-128">En ese momento, sin embargo, el marco de AJAX de ASP.NET no ha sido totalmente cargado aún.</span><span class="sxs-lookup"><span data-stu-id="100ce-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="100ce-129">Por lo tanto, la `launchModal()` función simplemente establece una variable que se debe mostrar el control ModalPopup más adelante:</span><span class="sxs-lookup"><span data-stu-id="100ce-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="100ce-130">El `pageLoad()` función de JavaScript es una función especial que se ejecuta una vez que se ha cargado completamente AJAX de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="100ce-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="100ce-131">Por lo tanto, se agregue código a esta función para mostrar el control ModalPopup, pero solo si `launchModal()` se ha llamado antes de:</span><span class="sxs-lookup"><span data-stu-id="100ce-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="100ce-132">El `$find()` función busca un elemento con nombre en la página y espera el identificador de servidor como un parámetro.</span><span class="sxs-lookup"><span data-stu-id="100ce-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="100ce-133">Por lo tanto, `$find("mpe")` devuelve la representación del cliente del control ModalPopup; su `show()` método permite que aparezca la ventana emergente.</span><span class="sxs-lookup"><span data-stu-id="100ce-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="100ce-134">[![Aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="100ce-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="100ce-135">Aparece la ventana emergente modal cuando se hace clic en cualquiera de los botones ([haga clic aquí para ver la imagen a tamaño completo](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="100ce-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="100ce-136">[Anterior](positioning-a-modalpopup-cs.md)
[Siguiente](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="100ce-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>