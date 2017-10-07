---
title: "Számlázási vállalati API - időszakok számlázási aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: d4e17f25b22729a7f213306fb019ee0dbeca87ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a>A vállalati ügyfelek - számlázási időszakok jelentéskészítési API-k

hello számlázási időszakok API adja vissza a hello adatokkal rendelkező időszakok számlázási listája beléptetési meg fordított időrendi sorrendben. Minden időszak toohello API útvonalat az adat - BalanceSummary, UsageDetails, Marktplace díjakat és árlista hello négy csoportjai mutató tulajdonságot tartalmaz. Hello időszak nincs adatokat, ha hello megfelelő tulajdonsága null értékű. 


##<a name="request"></a>Kérés 
Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md). 

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET| {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / billingperiods|

> [!Note]
> toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.
>

## <a name="response"></a>Válasz
 
    
    
      [
            {
                "billingPeriodId": "201704",
                "billingStart": "2017-04-01T00:00:00Z",
                "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },          
            ....
      ]
    

**Válasz erőforrástulajdonság-meghatározások**

|Tulajdonság neve| Típus| Leírás
|-|-|-|
|billingPeriodId| Karakterlánc| hello egy adott számlázási időszak jelölő egyedi azonosítója|
|billingStart| Dátum és idő| ISO 8601 karakterlánc, amely hello időszak kezdő dátuma|
|billingEnd| Dátum és idő| ISO 8601 karakterlánc, amely hello záró dátuma|
|balanceSummary| Karakterlánc| hello URL-címet, amely toohello egyenleg összegző adatokat az ebben az időszakban|
|usageDetails| Karakterlánc| hello URL-címet, amely az adott időszakra vonatkozó toohello használati adatainak|
|marketplaceCharges| Karakterlánc| hello URL-címet, amely az adott időszakra vonatkozó toohello piactér díjtételekre vonatkozó adatot|
|Árlista| Karakterlánc| hello URL-címet, amely az adott időszakra vonatkozó toohello árlista adatok|

<br/>
## <a name="see-also"></a>Lásd még:

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Használat részletei API](billing-enterprise-api-usage-detail.md) 

* [Piactér-tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)