---
title: "Azure számlázás vállalati API - időszakok számlázási |} Microsoft Docs"
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
ms.openlocfilehash: c6880b79189e0683387a7aafbd6fa4805b3b42ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a>A vállalati ügyfelek - számlázási időszakok jelentéskészítési API-k

A számlázási időszak API adatokkal a megadott beléptetési fordított időrendben elszámolási időszakok listáját adja vissza. Minden időszak mutat az API útvonalat az adat - BalanceSummary, UsageDetails, Marktplace díjakat és árlista négy csoportjai tulajdonságot tartalmaz. Ha az időszakban nem rendelkezik adatokat, a megfelelő tulajdonság értéke null. 


##<a name="request"></a>Kérés 
Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md). 

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET| {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / billingperiods|

> [!Note]
> API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.
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
|billingPeriodId| Karakterlánc| Egy adott számlázási időszak jelölő egyedi azonosítója|
|billingStart| Dátum és idő| ISO 8601 karakterlánc, amely az időszak kezdő dátuma|
|billingEnd| Dátum és idő| ISO 8601 karakterlánc, amely a záró dátuma|
|balanceSummary| Karakterlánc| Az URL-címe, amely továbbítja az egyenleg összegző adatokat az ebben az időszakban|
|usageDetails| Karakterlánc| Az URL-címet, amely a használat részleteiről adatoknak az ebben az időszakban|
|marketplaceCharges| Karakterlánc| Az URL-címe, amely továbbítja a piactér díjtételekre vonatkozó adatot az ebben az időszakban|
|Árlista| Karakterlánc| Az URL-címet, amely a árlista adatoknak az ebben az időszakban|

<br/>
## <a name="see-also"></a>Lásd még:

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Használat részletei API](billing-enterprise-api-usage-detail.md) 

* [Piactér-tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)