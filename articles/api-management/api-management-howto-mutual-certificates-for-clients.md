---
title: "Biztonságos ügyfél API-k az API Management - Azure API Management tanúsítványhitelesítés |} Microsoft Docs"
description: "Megtudhatja, hogyan biztosíthat biztonságos hozzáférést a az ügyféltanúsítványok API-k"
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
ms.openlocfilehash: d3d51d0575a6d2dacced931601d48eb1e51a4051
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a><span data-ttu-id="103e1-103">Az API Management tanúsítványhitelesítés biztonságossá tétele a ügyfél API-k</span><span class="sxs-lookup"><span data-stu-id="103e1-103">How to secure APIs using client certificate authentication in API Management</span></span>

<span data-ttu-id="103e1-104">API-kezelés lehetővé teszi a biztonságos hozzáférés a API-k (azaz ügyfél API-kezelés) ügyfél-tanúsítványok használatával.</span><span class="sxs-lookup"><span data-stu-id="103e1-104">API Management provides the capability to secure access to APIs (i.e., client to API Management) using client certificates.</span></span> <span data-ttu-id="103e1-105">Jelenleg ellenőrizheti a kívánt értékkel ügyféltanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="103e1-105">Currently, you can check the thumbprint of a client certificate against a desired value.</span></span> <span data-ttu-id="103e1-106">Ellenőrizheti az ujjlenyomatot, az API Management feltöltött meglévő tanúsítványokkal szemben.</span><span class="sxs-lookup"><span data-stu-id="103e1-106">You can also check the thumbprint against existing certificates uploaded to API Management.</span></span>  

<span data-ttu-id="103e1-107">További információ a háttér-szolgáltatás, az API-k (azaz API Management háttér-) ügyfél-tanúsítványok használatával biztonságossá tétele: [Tanúsítványalapú hitelesítés biztonságossá tétele a háttér-szolgáltatásaihoz ügyfél használatával](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span><span class="sxs-lookup"><span data-stu-id="103e1-107">For information about securing access to the back-end service of an API using client certificates (i.e., API Management to back-end), see [How to secure back-end services using client certificate authentication](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)</span></span>

## <a name="checking-the-expiration-date"></a><span data-ttu-id="103e1-108">A lejárati dátum ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="103e1-108">Checking the expiration date</span></span>

<span data-ttu-id="103e1-109">Alább házirendek beállítható úgy, hogy ellenőrizze, hogy ha a tanúsítvány lejárt-e:</span><span class="sxs-lookup"><span data-stu-id="103e1-109">Below policies can be configured to check if the certificate is expired:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-issuer-and-subject"></a><span data-ttu-id="103e1-110">A kibocsátó és tulajdonos ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="103e1-110">Checking the issuer and subject</span></span>

<span data-ttu-id="103e1-111">Alább házirendek beállítható úgy, hogy ellenőrizze a kibocsátó és egy ügyfél-tanúsítvány tulajdonosának:</span><span class="sxs-lookup"><span data-stu-id="103e1-111">Below policies can be configured to check the issuer and subject of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-the-thumbprint"></a><span data-ttu-id="103e1-112">Az ujjlenyomat ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="103e1-112">Checking the thumbprint</span></span>

<span data-ttu-id="103e1-113">Alább házirendek beállítható úgy, hogy ellenőrizze az ügyféltanúsítvány ujjlenyomata:</span><span class="sxs-lookup"><span data-stu-id="103e1-113">Below policies can be configured to check the thumbprint of a client certificate:</span></span>

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a><span data-ttu-id="103e1-114">A tanúsítványokkal szemben ujjlenyomat ellenőrzése fel van töltve az API Management</span><span class="sxs-lookup"><span data-stu-id="103e1-114">Checking a thumbprint against certificates uploaded to API Management</span></span>

<span data-ttu-id="103e1-115">A következő példa bemutatja, hogyan API Management feltöltött tanúsítványokkal szemben ügyféltanúsítvány ujjlenyomata kereséséhez:</span><span class="sxs-lookup"><span data-stu-id="103e1-115">The following example shows how to check the thumbprint of a client certificate against certificates uploaded to API Management:</span></span> 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a><span data-ttu-id="103e1-116">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="103e1-116">Next step</span></span>

*  [<span data-ttu-id="103e1-117">Tanúsítványalapú hitelesítés biztonságossá tétele a háttér-szolgáltatásaihoz ügyfél használatával</span><span class="sxs-lookup"><span data-stu-id="103e1-117">How to secure back-end services using client certificate authentication</span></span>](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [<span data-ttu-id="103e1-118">Tanúsítványok feltöltéséről</span><span class="sxs-lookup"><span data-stu-id="103e1-118">How to upload certificates</span></span>](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

