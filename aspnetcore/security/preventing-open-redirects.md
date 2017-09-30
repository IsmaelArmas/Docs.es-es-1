---
title: "Prevención de ataques de redirección abierta en una aplicación de ASP.NET Core | Documentos de Microsoft"
author: ardalis
description: "Muestra cómo evitar los ataques de redirección abierta en una aplicación de ASP.NET Core"
keywords: "Ataque de núcleo de ASP.NET, seguridad, redirección abierta"
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: d5419aa149b3201ecbc93f4f17ae5928f1d39b1d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a><span data-ttu-id="cc38f-104">Prevención de ataques de redirección abierta en una aplicación ASP.NET básica</span><span class="sxs-lookup"><span data-stu-id="cc38f-104">Preventing Open Redirect Attacks in an ASP.NET Core app</span></span>

<span data-ttu-id="cc38f-105">Una aplicación web que redirija a una dirección URL que se especifica a través de la solicitud como la cadena de consulta o un formulario de datos potencialmente puede alterada para redirigir a los usuarios a una dirección URL externa, malintencionada.</span><span class="sxs-lookup"><span data-stu-id="cc38f-105">A web app that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="cc38f-106">Esta modificación se llama a un ataque de redirección abierta.</span><span class="sxs-lookup"><span data-stu-id="cc38f-106">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="cc38f-107">Cada vez que la lógica de aplicación se redirige a una dirección URL especificada, debe comprobar que la dirección URL de redireccionamiento no se ha manipulado.</span><span class="sxs-lookup"><span data-stu-id="cc38f-107">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="cc38f-108">ASP.NET Core tiene funcionalidad integrada para ayudar a proteger las aplicaciones frente a ataques de redirección abierta (también conocido como abrir redirección).</span><span class="sxs-lookup"><span data-stu-id="cc38f-108">ASP.NET Core has built-in functionality to help protect apps from open redirect (also known as open redirection) attacks.</span></span>

## <a name="what-is-an-open-redirect-attack"></a><span data-ttu-id="cc38f-109">¿Qué es un ataque de redirección abierta?</span><span class="sxs-lookup"><span data-stu-id="cc38f-109">What is an open redirect attack?</span></span>

<span data-ttu-id="cc38f-110">Las aplicaciones Web con frecuencia redirección a los usuarios a una página de inicio de sesión cuando accedan a los recursos que requieren autenticación.</span><span class="sxs-lookup"><span data-stu-id="cc38f-110">Web applications frequently redirect users to a login page when they access resources that require authentication.</span></span> <span data-ttu-id="cc38f-111">La redirección typlically incluye un `returnUrl` parámetro de cadena de consulta para que el usuario puede devolverse a la dirección URL solicitada originalmente después de que han iniciado sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="cc38f-111">The redirection typlically includes a `returnUrl` querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span> <span data-ttu-id="cc38f-112">Después de que el usuario se autentica, se le redirige a la dirección URL que tenían originalmente solicitada.</span><span class="sxs-lookup"><span data-stu-id="cc38f-112">After the user authenticates, they are redirected to the URL they had originally requested.</span></span>

<span data-ttu-id="cc38f-113">Dado que la dirección URL de destino se especifica en la cadena de consulta de la solicitud, un usuario malintencionado podría manipular la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="cc38f-113">Because the destination URL is specified in the querystring of the request, a malicious user could tamper with the querystring.</span></span> <span data-ttu-id="cc38f-114">Una cadena de consulta modificada podría permitir al sitio redirigir al usuario a un sitio externo, malintencionado.</span><span class="sxs-lookup"><span data-stu-id="cc38f-114">A tampered querystring could allow the site to redirect the user to an external, malicious site.</span></span> <span data-ttu-id="cc38f-115">Esta técnica se denomina un ataque de redirección (o redirección) abierto.</span><span class="sxs-lookup"><span data-stu-id="cc38f-115">This technique is called an open redirect (or redirection) attack.</span></span>

### <a name="an-example-attack"></a><span data-ttu-id="cc38f-116">Un ataque de ejemplo</span><span class="sxs-lookup"><span data-stu-id="cc38f-116">An example attack</span></span>

<span data-ttu-id="cc38f-117">Un usuario malintencionado podría desarrollar un ataque diseñado para permitir el acceso de usuario malintencionado para las credenciales de un usuario o información confidencial en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="cc38f-117">A malicious user could develop an attack intended to allow the malicious user access to a user's credentials or sensitive information on your app.</span></span> <span data-ttu-id="cc38f-118">Para iniciar el ataque, convencer a los usuarios hacer clic en un vínculo a la página de inicio de sesión de su sitio, con un `returnUrl` valor cadena de consulta que se agrega a la dirección URL.</span><span class="sxs-lookup"><span data-stu-id="cc38f-118">To begin the attack, they convince the user to click a link to your site's login page, with a `returnUrl` querystring value added to the URL.</span></span> <span data-ttu-id="cc38f-119">Por ejemplo, el [NerdDinner.com](http://nerddinner.com) aplicación de ejemplo (escrito para ASP.NET MVC) incluye aquí tal una página de inicio de sesión: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span><span class="sxs-lookup"><span data-stu-id="cc38f-119">For example, the [NerdDinner.com](http://nerddinner.com) sample application (written for ASP.NET MVC) includes such a login page here: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``.</span></span> <span data-ttu-id="cc38f-120">El ataque, a continuación, sigue estos pasos:</span><span class="sxs-lookup"><span data-stu-id="cc38f-120">The attack then follows these steps:</span></span>

1. <span data-ttu-id="cc38f-121">Usuario hace clic en un vínculo a ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (tenga en cuenta, segunda dirección URL es nerddi**n**er, no nerddi**nn**er).</span><span class="sxs-lookup"><span data-stu-id="cc38f-121">User clicks a link to ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (note, second URL is nerddi**n**er, not nerddi**nn**er).</span></span>
2. <span data-ttu-id="cc38f-122">El usuario inicia sesión correctamente.</span><span class="sxs-lookup"><span data-stu-id="cc38f-122">The user logs in successfully.</span></span>
3. <span data-ttu-id="cc38f-123">Se redirige al usuario (en el sitio) a ``http://nerddiner.com/Account/LogOn`` (sitio malintencionado que parece sitio real).</span><span class="sxs-lookup"><span data-stu-id="cc38f-123">The user is redirected (by the site) to ``http://nerddiner.com/Account/LogOn`` (malicious site that looks like real site).</span></span>
4. <span data-ttu-id="cc38f-124">El usuario inicia sesión de nuevo (dando malintencionado sus credenciales de sitio) y se le redirige al sitio real.</span><span class="sxs-lookup"><span data-stu-id="cc38f-124">The user logs in again (giving malicious site their credentials) and is redirected back to the real site.</span></span>

<span data-ttu-id="cc38f-125">El usuario es probable que cree su primer intento de iniciar sesión no se pudo y la otra se realizó correctamente.</span><span class="sxs-lookup"><span data-stu-id="cc38f-125">The user will likely believe their first attempt to log in failed, and their second one was successful.</span></span> <span data-ttu-id="cc38f-126">Probablemente le permanecen sin tener en cuenta sus credenciales se han visto comprometidas.</span><span class="sxs-lookup"><span data-stu-id="cc38f-126">They'll most likely remain unaware their credentials have been compromised.</span></span>

![Proceso de ataques de redirección abierta](preventing-open-redirects/_static/open-redirection-attack-process.png)

<span data-ttu-id="cc38f-128">Además de las páginas de inicio de sesión, algunos sitios proporcionan páginas de redireccionamiento o puntos de conexión.</span><span class="sxs-lookup"><span data-stu-id="cc38f-128">In addition to login pages, some sites provide redirect pages or endpoints.</span></span> <span data-ttu-id="cc38f-129">Imagine que la aplicación tiene una página con una redirección abierta, ``/Home/Redirect``.</span><span class="sxs-lookup"><span data-stu-id="cc38f-129">Imagine your app has a page with an open redirect, ``/Home/Redirect``.</span></span> <span data-ttu-id="cc38f-130">Un atacante podría crear, por ejemplo, un vínculo en un correo electrónico que se va a ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span><span class="sxs-lookup"><span data-stu-id="cc38f-130">An attacker could create, for example, a link in an email that goes to ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``.</span></span> <span data-ttu-id="cc38f-131">Un usuario típico en la dirección URL y saber que comienza con el nombre del sitio.</span><span class="sxs-lookup"><span data-stu-id="cc38f-131">A typical user will look at the URL and see it begins with your site name.</span></span> <span data-ttu-id="cc38f-132">Confiar en, hará clic en el vínculo.</span><span class="sxs-lookup"><span data-stu-id="cc38f-132">Trusting that, they will click the link.</span></span> <span data-ttu-id="cc38f-133">La redirección abierta enviaría a continuación, el usuario para el sitio de suplantación de identidad, cuya apariencia es idéntico a la suya, y es probable que lo haría el usuario inicie sesión en lo creen que es su sitio.</span><span class="sxs-lookup"><span data-stu-id="cc38f-133">The open redirect would then send the user to the phishing site, which looks identical to yours, and the user would likely login to what they believe is your site.</span></span>

## <a name="protecting-against-open-redirect-attacks"></a><span data-ttu-id="cc38f-134">Protegerse contra los ataques de redirección abierta</span><span class="sxs-lookup"><span data-stu-id="cc38f-134">Protecting against open redirect attacks</span></span>

<span data-ttu-id="cc38f-135">Al desarrollar aplicaciones web, trate todos los datos proporcionados por el usuario que no es de confianza.</span><span class="sxs-lookup"><span data-stu-id="cc38f-135">When developing web applications, treat all user-provided data as untrustworthy.</span></span> <span data-ttu-id="cc38f-136">Si la aplicación dispone de funcionalidad que redirige al usuario según el contenido de la dirección URL, asegúrese de que estas redirecciones solo se realizan localmente dentro de la aplicación (o a una dirección URL conocida, no cualquier dirección URL que puede especificarse en la cadena de consulta).</span><span class="sxs-lookup"><span data-stu-id="cc38f-136">If your application has functionality that redirects the user based on the contents of the URL,  ensure that such redirects are only done locally within your app (or to a known URL, not any URL that may be supplied in the querystring).</span></span>

### <a name="localredirect"></a><span data-ttu-id="cc38f-137">LocalRedirect</span><span class="sxs-lookup"><span data-stu-id="cc38f-137">LocalRedirect</span></span>

<span data-ttu-id="cc38f-138">Use la ``LocalRedirect`` método auxiliar de la base de `Controller` clase:</span><span class="sxs-lookup"><span data-stu-id="cc38f-138">Use the ``LocalRedirect`` helper method from the base `Controller` class:</span></span>

```
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

<span data-ttu-id="cc38f-139">``LocalRedirect``se iniciará una excepción si se especifica una dirección URL no locales.</span><span class="sxs-lookup"><span data-stu-id="cc38f-139">``LocalRedirect`` will throw an exception if a non-local URL is specified.</span></span> <span data-ttu-id="cc38f-140">En caso contrario, se comporta igual que el ``Redirect`` método.</span><span class="sxs-lookup"><span data-stu-id="cc38f-140">Otherwise, it behaves just like the ``Redirect`` method.</span></span>

### <a name="islocalurl"></a><span data-ttu-id="cc38f-141">IsLocalUrl</span><span class="sxs-lookup"><span data-stu-id="cc38f-141">IsLocalUrl</span></span>

<span data-ttu-id="cc38f-142">Use la [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) método para probar las direcciones URL antes de redirigir:</span><span class="sxs-lookup"><span data-stu-id="cc38f-142">Use the [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) method to test URLs before redirecting:</span></span>

<span data-ttu-id="cc38f-143">En el ejemplo siguiente se muestra cómo comprobar si una dirección URL es local antes de redirigir.</span><span class="sxs-lookup"><span data-stu-id="cc38f-143">The following example shows how to check whether a URL is local before redirecting.</span></span>

```
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

<span data-ttu-id="cc38f-144">El `IsLocalUrl` método protege a los usuarios sin darse cuenta sea redirigido a un sitio malintencionado.</span><span class="sxs-lookup"><span data-stu-id="cc38f-144">The `IsLocalUrl` method protects users from being inadvertently redirected to a malicious site.</span></span> <span data-ttu-id="cc38f-145">Puede registrar los detalles de la dirección URL que se proporcionó cuando se proporciona una dirección URL no es local en una situación donde se espera una dirección URL local.</span><span class="sxs-lookup"><span data-stu-id="cc38f-145">You can log the details of the URL that was provided when a non-local URL is supplied in a situation where you expected a local URL.</span></span> <span data-ttu-id="cc38f-146">Registro de redirección de direcciones URL pueden ayudar a diagnosticar los ataques de redirección.</span><span class="sxs-lookup"><span data-stu-id="cc38f-146">Logging redirect URLs may help in diagnosing redirection attacks.</span></span>