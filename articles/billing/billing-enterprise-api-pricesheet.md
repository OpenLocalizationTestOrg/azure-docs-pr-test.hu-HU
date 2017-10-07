---
title: "Árlista számlázási vállalati API - aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 4cfe694c63fba266d117054b046d9caacb3b7197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a>A vállalati ügyfelek - árlista jelentéskészítési API-k

hello Price Sheet API biztosít minden mérni a regisztráció és a számlázási időszak hello hello alkalmazható sebessége.

##<a name="request"></a>Kérés
Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md). Ha nincs megadva a számlázott időszak, majd a hello aktuális elszámolási időszak adat.

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / árlista|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ {billingPeriod} / árlista|

> [!Note]
> toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.
>

## <a name="response"></a>Válasz

    
        [
            {
                "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                "billingPeriodId": "201704",
                "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
                "meterName": "A1 VM",
                "unitOfMeasure": "100 Hours",
                "includedQuantity": 0,
                "partNumber": "N7H-00015",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            {
                "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                "billingPeriodId": "201404",
                "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
                "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                "unitOfMeasure": "100 GB",
                "includedQuantity": 0,
                "partNumber": "N9H-00402",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            ...
        ]
    

> [!Note]
>Ha hello előnézeti API-t használ, a meterId mező nem érhető el.
>

**Válasz erőforrástulajdonság-meghatározások**

|Tulajdonság neve| Típus| Leírás
|-|-|-|
|id| Karakterlánc| hello egy adott árlista elemet (mérő számlázási időszak szerint) jelölő elem egyedi azonosítója|
|billingPeriodId| Karakterlánc| hello egy adott számlázási időszak jelölő egyedi azonosítója|
|meterId| Karakterlánc| hello mérő hello azonosítója. Ez lehet a csatlakoztatott toohello használati meterId.|
|meterName| Karakterlánc| hello mérő neve|
|unitOfMeasure| Karakterlánc| hello mértékegység méri hello szolgáltatás|
|includedQuantity| Decimális| Részét képező mennyiség |
|partNumber| Karakterlánc| hello mérő társított hello cikkszám|
|Egységár| Decimális| hello mérő hello Egységár|
|currencyCode| Karakterlánc| hello pénznemkódot a hello Egységár|
<br/>
## <a name="see-also"></a>Lásd még:

* [Elszámolási időszakok API](billing-enterprise-api-billing-periods.md)

* [Használat részletei API](billing-enterprise-api-usage-detail.md)

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Piactér-tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md)
