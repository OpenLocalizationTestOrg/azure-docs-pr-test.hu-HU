---
title: "Azure számlázás vállalati API - elosztás és összegzése |} Microsoft Docs"
description: "Tudnivalók Azure számlázási használati és RateCard API-k, amely biztosítja a trendeket és az Azure erőforrás-felhasználás."
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
ms.openlocfilehash: f6b149f0e656d2263705048aa5b644f5bb4a5712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a>Jelentéskészítési API-k, a vállalati ügyfelek - elosztás és összegzése

Az egyenleg és összefoglaló API kínál kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosítását, és keretét díjak adatainak havi összegzése.


##<a name="request"></a>Kérés 
Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md). Ha a számlázott időszak nincs megadva, majd az aktuális elszámolási időszak adat.

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET| {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / balancesummary|
|GET| {billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / balancesummary|

> [!Note]
> API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.
>

## <a name="response"></a>Válasz

        {
            "id": "enrollments/100/billingperiods/201507/balancesummaries",
            "billingPeriodId": 201507,
            "currencyCode": "USD",
            "beginningBalance": 0,
            "endingBalance": 1.1,
            "newPurchases": 1,
            "adjustments": 1.1,
            "utilized": 1.1,
            "serviceOverage": 1,
            "chargesBilledSeparately": 1,
            "totalOverage": 1,
            "totalUsage": 1.1,
            "azureMarketplaceServiceCharges": 1,
            "newPurchasesDetails": [
                {
                "name": "",
                "value": 1
                }
            ],
            "adjustmentDetails": [
                {
                "name": "Promo Credit",
                "value": 1.1
                },
                {
                "name": "SIE Credit",
                "value": 1.0
                }
            ]
        }


**Válasz erőforrástulajdonság-meghatározások**

|Tulajdonság neve| Típus| Leírás
|-|-|-|
|id|Karakterlánc|Egy adott számlázási időszakban és a beléptetési egyedi azonosítója|
|billingPeriodId|Karakterlánc |A számlázott időszak azonosítója|
|currencyCode|Karakterlánc |A pénznemkódot|
|beginningBalance|Decimális| A számlázott időszak kezdete egyenlege|
|endingBalance|Decimális| A befejezési elosztás a számlázott időszak (időszakokra nyissa meg ezt a rendszer naponta frissíti)|
|newPurchases|Decimális| Új beszerzési végösszeg|
|beállításai|Decimális| Teljes módosítás összeg|
|használata|Decimális| Teljes kötelezettségvállalás használata|
|serviceOverage|Decimális| Az Azure szolgáltatások túlhasználati|
|chargesBilledSeparately|Decimális| Külön számlázva díjak|
|totalOverage|Decimális| serviceOverage + chargesBilledSeparately|
|totalUsage|Decimális| Azure-szolgáltatások kötelezettségvállalás + többlete|
|azureMarketplaceServiceCharges|Decimális| Teljes költségek az Azure piactéren|
|newPurchasesDetails|A név-érték pár karakterlánc-tömbben JSON|Új vásárlás listája|
|adjustmentDetails|A név-érték pár karakterlánc-tömbben JSON|(Az előléptetés jóváírás, SIE jóváírás stb.) beállításainak listája |


<br/>
## <a name="see-also"></a>Lásd még:

* [Elszámolási időszakok API](billing-enterprise-api-billing-periods.md)

* [Használat részletei API](billing-enterprise-api-usage-detail.md) 

* [Piactér-tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)