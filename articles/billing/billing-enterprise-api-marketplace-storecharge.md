---
title: "Azure számlázási vállalati API - piactér díjak |} Microsoft Docs"
description: "További tudnivalók a Reporting API-k, amelyek lehetővé teszik a vállalati Azure ügyfelek való lekérésére programozott módon fogyasztási adatokhoz."
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
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a>A vállalati ügyfelek - piactér tároló kell fizetni jelentéskészítési API-k

A piactér tároló ingyenesen elérhető API piactér használat alapú költségek bontásban tartalmazza a megadott számlázási időszak vagy a kezdő és befejező dátumok (egyszer díjak nincsenek) napi adja vissza.

##<a name="request"></a>Kérés 
Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md). Ha a számlázott időszak nincs megadva, majd az aktuális elszámolási időszak adat. Egyéni időtartományhoz megadhatók a kezdő és befejező dátum paraméterek éééé-hh-nn formátum a, a maximális támogatott időtartomány 36 hónap.  

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacecharges|
|GET|{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / marketplacecharges|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.
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
|id|Karakterlánc|A piactér kell fizetni elem egyedi azonosítója|
|subscriptionGuid|GUID|Az előfizetés Guid|
|SubscriptionName|Karakterlánc|Az előfizetés neve|
|meterId|Karakterlánc|A kibocsátott mérő azonosítója|
|usageStartDate|Dátum és idő|Indítsa el a használati rekord|
|usageEndDate|Dátum és idő|A használati rekord befejezési időpontja|
|offerName|Karakterlánc|Az ajánlat neve|
|Erőforráscsoport|Karakterlánc|Az erőforrás csoport|
|instanceId|Karakterlánc|Példányazonosító|
|additionalinfo részben|Karakterlánc|További információ a JSON-karakterláncban|
|tags|Karakterlánc|Címke JSON-karakterláncban|
|orderNumber|Karakterlánc|A sorszám|
|unitOfMeasure|Karakterlánc|A mérő mértékegysége|
|CostCenter|Karakterlánc|A költségközpont|
|accountId|int|A fiók azonosítója|
|Fióknév|Karakterlánc |A fiók neve|
|accountOwnerId|Karakterlánc|A fiók tulajdonosa azonosítója|
|departmentId|int|A szervezeti egység azonosítója|
|departmentname nevű|Karakterlánc|Az osztály neve|
|publisherName|Karakterlánc|A közzétevő neve|
|planName|Karakterlánc|A csomag neve|
|consumedQuantity|Decimális|Ezen időszak alatt felhasznált mennyiség|
|resourceRate|Decimális|A mérő Egységár|
|extendedCost|Decimális|Becsült költség felhasznált mennyiség és kiterjesztett alapján|
<br/>
## <a name="see-also"></a>Lásd még:

* [Elszámolási időszakok API](billing-enterprise-api-billing-periods.md)

* [Használat részletei API](billing-enterprise-api-usage-detail.md) 

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Price Sheet API](billing-enterprise-api-pricesheet.md)