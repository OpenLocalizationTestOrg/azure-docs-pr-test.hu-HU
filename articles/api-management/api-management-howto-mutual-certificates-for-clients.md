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
# <a name="how-toosecure-apis-using-client-certificate-authentication-in-api-management"></a>Hogyan toosecure ügyfél API-k Tanúsítványalapú hitelesítés az API Management

API-kezelési hello funkció toosecure hozzáférési tooAPIs (azaz ügyfél tooAPI felügyeleti) biztosítja az ügyféltanúsítványok. Jelenleg a kívánt értékkel ügyféltanúsítvány ujjlenyomata hello ellenőrizheti. Meglévő tanúsítványokkal szemben hello ujjlenyomat feltöltött tooAPI felügyeleti is ellenőrizheti.  

További információ a hozzáférési toohello háttér-szolgáltatás, az API-k az ügyféltanúsítványok (azaz az API Management tooback közötti) biztonságossá tétele: [hogyan toosecure háttér-szolgáltatásaihoz ügyfélprogrammal Tanúsítványalapú hitelesítés](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)

## <a name="checking-hello-expiration-date"></a>Ellenőrzési hello lejárati dátuma

Alább házirendeket lehet konfigurált toocheck Ha hello tanúsítvány lejárt:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.NotAfter < DateTime.Now)" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-issuer-and-subject"></a>Hello kibocsátó és tulajdonos ellenőrzése

Alább házirendeket lehet konfigurált toocheck hello kibocsátó és egy ügyfél-tanúsítvány tulajdonosának:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-hello-thumbprint"></a>Hello ujjlenyomat ellenőrzése

Alább házirendeket lehet konfigurált toocheck hello ügyféltanúsítvány ujjlenyomata:

```
<choose>
    <when condition="@(context.Request.Certificate == null || context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

## <a name="checking-a-thumbprint-against-certificates-uploaded-tooapi-management"></a>A tanúsítványokkal szemben ujjlenyomat ellenőrzése feltöltött tooAPI felügyeleti

hello következő példa bemutatja, hogyan toocheck hello tanúsítványokkal szemben ügyféltanúsítvány ujjlenyomata feltöltött tooAPI felügyeleti: 

```
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

## <a name="next-step"></a>Következő lépés

*  [Hogyan toosecure háttér-szolgáltatásaihoz ügyfélprogrammal Tanúsítványalapú hitelesítés](https://docs.microsoft.com/en-us/azure/api-management/api-management-howto-mutual-certificates)
*  [Hogyan tooupload tanúsítványok](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates#a-namestep1-aupload-a-client-certificate)

