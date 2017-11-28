---
title: "API-k aaaSecure ügyféltanúsítvány-alapú hitelesítés használata az API Management - Azure API Management |} Microsoft Docs"
description: "Ismerje meg, hogyan férhetnek hozzá a toosecure az ügyféltanúsítványok tooAPIs"
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: apimpm
ms.openlocfilehash: 6ff78bda3d429829da79d0dc4d652f19669cc919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosecure-apis-using-client-certificate-authentication-in-api-management"></a><span data-ttu-id="5365d-103">Hogyan toosecure ügyfél API-k Tanúsítványalapú hitelesítés az API Management</span><span class="sxs-lookup"><span data-stu-id="5365d-103">How toosecure APIs using client certificate authentication in API Management</span></span>

<span data-ttu-id="5365d-104">API-kezelési hello funkció toosecure hozzáférési tooAPIs (azaz ügyfél tooAPI felügyeleti) biztosítja az ügyféltanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="5365d-104">API Management provides hello capability toosecure access tooAPIs (i.e., client tooAPI Management) using client certificates.</span></span> <span data-ttu-id="5365d-105">Jelenleg a kívánt értékkel ügyféltanúsítvány ujjlenyomata hello ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="5365d-105">Currently, you can check hello thumbprint of a client certificate against a desired value.</span></span> <span data-ttu-id="5365d-106">Meglévő tanúsítványokkal szemben hello ujjlenyomat feltöltött tooAPI felügyeleti is ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="5365d-106">You can also check hello thumbprint against existing certificates uploaded tooAPI Management.</span></span>  

<span data-ttu-id="5365d-107">További információ a hozzáférési toohello háttér-szolgáltatás, az API-k az ügyféltanúsítványok (azaz az API Management tooback közötti) biztonságossá tétele: [hogyan toosecure háttér-szolgáltatásaihoz ügyfélprogrammal Tanúsítványalapú hitelesítés](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span><span class="sxs-lookup"><span data-stu-id="5365d-107">For information about securing access toohello back-end service of an API using client certificates (i.e., API Management tooback-end), see [How toosecure back-end services using client certificate authentication](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span></span>

## <a name="checking-hello-expiration-date"></a><span data-ttu-id="5365d-108">Ellenőrzési hello lejárati dátuma</span><span class="sxs-lookup"><span data-stu-id="5365d-108">Checking hello expiration date</span></span>

<span data-ttu-id="5365d-109">Alább házirendeket lehet konfigurált toocheck Ha hello tanúsítvány lejárt:</span><span class="sxs-lookup"><span data-stu-id="5365d-109">Below policies can be configured toocheck if hello certificate is expired:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-issuer-and-subject"></a><span data-ttu-id="5365d-110">Hello kibocsátó és tulajdonos ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5365d-110">Checking hello issuer and subject</span></span>

<span data-ttu-id="5365d-111">Alább házirendeket lehet konfigurált toocheck hello kibocsátó és egy ügyfél-tanúsítvány tulajdonosának:</span><span class="sxs-lookup"><span data-stu-id="5365d-111">Below policies can be configured toocheck hello issuer and subject of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-thumbprint"></a><span data-ttu-id="5365d-112">Hello ujjlenyomat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="5365d-112">Checking hello thumbprint</span></span>

<span data-ttu-id="5365d-113">Alább házirendeket lehet konfigurált toocheck hello ügyféltanúsítvány ujjlenyomata:</span><span class="sxs-lookup"><span data-stu-id="5365d-113">Below policies can be configured toocheck hello thumbprint of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-tooapi-management"></a><span data-ttu-id="5365d-114">A tanúsítványokkal szemben ujjlenyomat ellenőrzése feltöltött tooAPI felügyeleti</span><span class="sxs-lookup"><span data-stu-id="5365d-114">Checking a thumbprint against certificates uploaded tooAPI Management</span></span>

<span data-ttu-id="5365d-115">hello következő példa bemutatja, hogyan toocheck hello tanúsítványokkal szemben ügyféltanúsítvány ujjlenyomata feltöltött tooAPI felügyeleti:</span><span class="sxs-lookup"><span data-stu-id="5365d-115">hello following example shows how toocheck hello thumbprint of a client certificate against certificates uploaded tooAPI Management:</span></span> 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a><span data-ttu-id="5365d-116">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="5365d-116">Next step</span></span>

*  [<span data-ttu-id="5365d-117">Hogyan toosecure háttér-szolgáltatásaihoz ügyfélprogrammal Tanúsítványalapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5365d-117">How toosecure back-end services using client certificate authentication</span></span>](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [<span data-ttu-id="5365d-118">Hogyan tooupload tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="5365d-118">How tooupload certificates</span></span>](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

