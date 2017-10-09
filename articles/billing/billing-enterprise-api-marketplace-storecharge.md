---
title: "aaaAzure számlázási vállalati API - piactér díjak |} Microsoft Docs"
description: "További információk a hello Reporting API-k, amelyek lehetővé teszik a vállalati Azure ügyfelek toopull adatokkal programozott módon."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: cdf2836b52df06a4bf5ed71a476fe33662c5363c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>A vállalati ügyfelek - piactér tároló kell fizetni jelentéskészítési API-k

hello piactér tároló ingyenesen elérhető API értéket ad vissza hello piactér használat alapú költségek bontás hello nap megadott számlázási időszak vagy kezdő és befejező dátumok (egyszer díjak nem szerepelnek).

##<a name="request"></a>Kérés 
Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md). Ha nincs megadva a számlázott időszak, majd a hello aktuális elszámolási időszak adat. Egyéni időtartományhoz hello megkezdve adható meg, és dátumparaméterekkel, amelyek hello formátum éééé-hh-nn hello maximális támogatott időben tartománya 36 hónap végén.  

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacecharges|
|GET|{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / marketplacecharges|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.
>

## <a name="response"></a>Válasz
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

**Válasz erőforrástulajdonság-meghatározások**

|Tulajdonság neve| Típus| Leírás
|-|-|-|
|id|Karakterlánc|Hello piactér kell fizetni elem egyedi azonosítója|
|subscriptionGuid|GUID|hello előfizetés Guid|
|SubscriptionName|Karakterlánc|hello előfizetés neve|
|meterId|Karakterlánc|A kibocsátott mérő hello azonosítója|
|usageStartDate|Dátum és idő|Hello használati rekord kezdési időpontja|
|usageEndDate|Dátum és idő|Hello használati rekord befejezési időpontja|
|offerName|Karakterlánc|hello ajánlat neve|
|Erőforráscsoport|Karakterlánc|hello erőforrás csoport|
|instanceId|Karakterlánc|Példányazonosító|
|additionalinfo részben|Karakterlánc|További információ a JSON-karakterláncban|
|tags|Karakterlánc|Címke JSON-karakterláncban|
|orderNumber|Karakterlánc|hello sorszámú|
|unitOfMeasure|Karakterlánc|Hello mérő mértékegységét|
|CostCenter|Karakterlánc|költség hello center|
|accountId|int|hello fiók azonosítója|
|Fióknév|Karakterlánc |hello fiók neve|
|accountOwnerId|Karakterlánc|hello fiókazonosító tulajdonosa|
|departmentId|int|hello részleg azonosítója|
|departmentname nevű|Karakterlánc|hello osztály neve|
|publisherName|Karakterlánc|hello közzétevő neve|
|planName|Karakterlánc|hello csomag neve|
|consumedQuantity|Decimális|Ezen időszak alatt felhasznált mennyiség|
|resourceRate|Decimális|Hello mérő Egységár|
|extendedCost|Decimális|Becsült költség felhasznált mennyiség és kiterjesztett alapján|
<br/>
## <a name="see-also"></a>Lásd még:

* [Elszámolási időszakok API](billing-enterprise-api-billing-periods.md)

* [Használat részletei API](billing-enterprise-api-usage-detail.md) 

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Price Sheet API](billing-enterprise-api-pricesheet.md)