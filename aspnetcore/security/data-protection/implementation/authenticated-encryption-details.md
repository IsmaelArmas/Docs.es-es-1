---
title: Detalles de cifrado autenticado.
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 826e6d5d-9620-44e6-ad93-3b1d9969b70b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 4d0e63d7722071ab8806a217e96ee7ad7bf10286
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="authenticated-encryption-details"></a><span data-ttu-id="2567c-103">Detalles de cifrado autenticado.</span><span class="sxs-lookup"><span data-stu-id="2567c-103">Authenticated encryption details.</span></span>

<a name=data-protection-implementation-authenticated-encryption-details></a>

<span data-ttu-id="2567c-104">Llamadas a IDataProtector.Protect son operaciones de cifrado autenticado.</span><span class="sxs-lookup"><span data-stu-id="2567c-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="2567c-105">El método Protect ofrece confidencialidad y la autenticidad y está asociado a la cadena de propósito que se usó para esta instancia concreta de IDataProtector se deriva su raíz IDataProtectionProvider.</span><span class="sxs-lookup"><span data-stu-id="2567c-105">The Protect method offers both confidentiality and authenticity, and it is tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="2567c-106">IDataProtector.Protect toma un parámetro de texto simple de byte [] y genera una byte [] protegido carga, cuyo formato se describe a continuación.</span><span class="sxs-lookup"><span data-stu-id="2567c-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="2567c-107">(También hay una sobrecarga del método de extensión que toma un parámetro de cadena de texto simple y devuelve una carga protegido de cadena.</span><span class="sxs-lookup"><span data-stu-id="2567c-107">(There is also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="2567c-108">Si se usa esta API seguirá teniendo el formato de carga protegido el por debajo de la estructura, pero será [codificado en base64url](https://tools.ietf.org/html/rfc4648#section-5).)</span><span class="sxs-lookup"><span data-stu-id="2567c-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="2567c-109">Formato de carga protegido</span><span class="sxs-lookup"><span data-stu-id="2567c-109">Protected payload format</span></span>

<span data-ttu-id="2567c-110">El formato de carga protegido consta de tres componentes principales:</span><span class="sxs-lookup"><span data-stu-id="2567c-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="2567c-111">Encabezado mágico de 32 bits que identifica la versión del sistema de protección de datos.</span><span class="sxs-lookup"><span data-stu-id="2567c-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="2567c-112">Identificador de clave de 128 bits que identifica la clave utilizada para proteger esta carga determinada.</span><span class="sxs-lookup"><span data-stu-id="2567c-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="2567c-113">El resto de la carga protegido es [específico para el sistema de cifrado encapsulada por esta clave](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span><span class="sxs-lookup"><span data-stu-id="2567c-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](subkeyderivation.md#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="2567c-114">En el ejemplo siguiente representa la clave de un cifrado AES-256-CBC + HMACSHA256 cifrado y la carga se subdivide como sigue: * el modificador de tecla A 128 bits.</span><span class="sxs-lookup"><span data-stu-id="2567c-114">In the example below the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows: * A 128-bit key modifier.</span></span> <span data-ttu-id="2567c-115">* Un vector de inicialización de 128 bits.</span><span class="sxs-lookup"><span data-stu-id="2567c-115">* A 128-bit initialization vector.</span></span> <span data-ttu-id="2567c-116">* 48 bytes de salida de AES-256-CBC.</span><span class="sxs-lookup"><span data-stu-id="2567c-116">* 48 bytes of AES-256-CBC output.</span></span> <span data-ttu-id="2567c-117">* Una etiqueta de autenticación HMACSHA256.</span><span class="sxs-lookup"><span data-stu-id="2567c-117">* An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="2567c-118">Una carga protegido de ejemplo se ilustra a continuación.</span><span class="sxs-lookup"><span data-stu-id="2567c-118">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="2567c-119">Desde el formato de carga por encima de los primeros 32 bits o 4 bytes son el encabezado mágico identifica la versión (09 F0 C9 F0)</span><span class="sxs-lookup"><span data-stu-id="2567c-119">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="2567c-120">Los siguientes 128 bits o 16 bytes es el identificador de clave (80 9 81 C 0c 19 66 19 40 95 36 53 F8 AA FF EE 57)</span><span class="sxs-lookup"><span data-stu-id="2567c-120">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="2567c-121">El resto contiene la carga y es específico para el formato utilizado.</span><span class="sxs-lookup"><span data-stu-id="2567c-121">The remainder contains the payload and is specific to the format used.</span></span>

>[!WARNING]
> <span data-ttu-id="2567c-122">Todas las cargas protegidas para una clave determinada se iniciará con el mismo encabezado de 20 bytes (valor mágica, Id. de clave).</span><span class="sxs-lookup"><span data-stu-id="2567c-122">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="2567c-123">Los administradores pueden usar este hecho con fines de diagnóstico para la aproximación cuando se genera una carga.</span><span class="sxs-lookup"><span data-stu-id="2567c-123">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="2567c-124">Por ejemplo, la carga anterior corresponde a la clave {0c819c80-6619-4019-9536-53f8aaffee57}.</span><span class="sxs-lookup"><span data-stu-id="2567c-124">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="2567c-125">Si después de comprobar el repositorio clave encuentra que la fecha de activación de esta clave específica fue 2015-01-01 y su fecha de expiración era 2015-03-01, entonces es razonable suponer la carga (si no ha sido manipulado con) se ha generado dentro de esa ventana, conceda a o tomar una pequeña factor de aglutinante a cada lado.</span><span class="sxs-lookup"><span data-stu-id="2567c-125">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it is reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>