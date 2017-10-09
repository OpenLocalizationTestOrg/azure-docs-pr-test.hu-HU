---
title: "aaaAzure számlázási vállalati API - használati részletek |} Microsoft Docs"
description: "Tudnivalók Azure számlázási használati és RateCard API-k, amelyek az Azure erőforrás-felhasználás és trendek használt tooprovide betekintést."
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
ms.openlocfilehash: def0805008261df5872f015db3d2b26e47d25569
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a>A vállalati ügyfelek - használat részleteiről jelentéskészítési API-k

Használat részletei API hello kínál felhasznált és a beléptetési díjak becsült napi bontásban. hello eredmény példányok, mérőszámok és szervezeti egységek adatokat is tartalmaz. hello API számlázási időszak szerint, vagy a megadott kezdési és befejezési dátum kérdezhetők le. 
## <a name="consumption-apis"></a>Felhasználás API-k


##<a name="request"></a>Kérés 
Közös fejléc tulajdonságok hozzáadott toobe igénylő megadott [Itt](billing-enterprise-api.md). Ha nincs megadva a számlázott időszak, majd a hello aktuális elszámolási időszak adat. Egyéni időtartományhoz hello megkezdve adható meg, és a befejező dátum paraméterek lévő hello formátum éééé-hh-nn. hello támogatott maximális idő tartománya 36 hónap.  

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / usagedetails 
|GET|{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / usagedetails|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / usagedetailsbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> toouse hello előzetes verzióját API, v2 v1 a fenti URL-cím hello cserélje le.
>

## <a name="response"></a>Válasz

> Lejáró toohello potenciálisan nagy mennyiségű adatok hello eredmény lapozható memóriakészlet beállítása. hello nextLink tulajdonság, ha jelen van, a következő lap hello adatok hello hivatkozás határozza meg. Ha hello hivatkozás üres, akkor azt jelzi, amely hello utolsóként megjelenő oldala. 
<br/>

    {
        "id": "string",
        "data": [
            {                       
            "accountId": 0,
            "productId": 0,
            "resourceLocationId": 0,
            "consumedServiceId": 0,
            "departmentId": 0,
            "accountOwnerEmail": "string",
            "accountName": "string",
            "serviceAdministratorId": "string",
            "subscriptionId": 0,
            "subscriptionGuid": "string",
            "subscriptionName": "string",
            "date": "2017-04-27T23:01:43.799Z",
            "product": "string",
            "meterId": "string",
            "meterCategory": "string",
            "meterSubCategory": "string",
            "meterRegion": "string",
            "meterName": "string",
            "consumedQuantity": 0,
            "resourceRate": 0,
            "Cost": 0,
            "resourceLocation": "string",
            "consumedService": "string",
            "instanceId": "string",
            "serviceInfo1": "string",
            "serviceInfo2": "string",
            "additionalInfo": "string",
            "tags": "string",
            "storeServiceIdentifier": "string",
            "departmentName": "string",
            "costCenter": "string",
            "unitOfMeasure": "string",
            "resourceGroup": "string"
            }
        ],
        "nextLink": "string"
    }

<br/>
**Válasz erőforrástulajdonság-meghatározások**

|Tulajdonság neve| Típus| Leírás
|-|-|-|
|id| Karakterlánc| hello hello API-hívás egyedi azonosítója. |
|Adatok| JSON-tömb| hello tömb minden instance\meter napi használati adatait.|
|nextLink| Karakterlánc| Ha az hello nextLink pontok toohello URL-cím tooreturn hello következő lap az adatok több lapot. |
|accountId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|Termékazonosító| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|resourceLocationId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|consumedServiceID| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|departmentId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|accountOwnerEmail| Karakterlánc| Hello fiók tulajdonosának e-mail-fiókot. |
|Fióknév| Karakterlánc| A megadott felhasználói hello fiók nevét. |
|serviceAdministratorId| Karakterlánc| E-mail cím a szolgáltatás-rendszergazdát. |
|subscriptionId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|subscriptionGuid| Karakterlánc| Hello előfizetés globális egyedi azonosítója. |
|SubscriptionName| Karakterlánc| Hello előfizetés nevét. |
|Dátum| Karakterlánc| hello fogyasztás létrejöttének dátumát. |
|A termék| Karakterlánc| További részletek a hello mérni. Példa: A1 (VM) Windows - hozzáférési pont keleti régiója|
|meterId| Karakterlánc| hello mérni, amely a kibocsátott használati hello azonosítója. |
|meterCategory| Karakterlánc| hello Azure platformszolgáltatás, amely lett megadva. |
|meterSubCategory| Karakterlánc| Határozza meg, amelyek hatással lehetnek a hello arány hello Azure szolgáltatás típusa. Példa: A1 méretű VM (nem Windows|
|meterRegion| Karakterlánc| Bizonyos szolgáltatások árú datacenter helye alapján hello datacenter hello helyét azonosítja. |
|meterName| Karakterlánc| Hello mérő neve. |
|consumedQuantity| Dupla| hello a mennyisége hello mérni, hogy fel lett használva. |
|resourceRate| Dupla| hello arány számlázható egységenként alkalmazható. |
|Költség| Dupla| hello kell fizetni, amely hello mérő vállalt. |
|resourceLocation| Karakterlánc| Hello mérő futtató hello datacenter azonosítja. |
|consumedService| Karakterlánc| hello Azure platformszolgáltatás, amely lett megadva. |
|instanceId| Karakterlánc| Ez az azonosító hello hello erőforrás nevét, vagy hello teljesen minősített erőforrás-azonosító. További információkért lásd: [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources) |
|serviceInfo1| Karakterlánc| Belső Azure szolgáltatás metaadatai. |
|serviceInfo2| Karakterlánc| Például egy kép típusa egy virtuális gép és az ExpressRoute Internetszolgáltató nevét. |
|additionalinfo részben| Karakterlánc| Szolgáltatással kapcsolatos metaadatok. Például egy kép típusa egy virtuális géphez. |
|tags| Karakterlánc| Ügyfél hozzáadott címkéket. További információkért lásd: [címke az Azure-erőforrások rendszerezése](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags). |
|storeServiceIdentifier| Karakterlánc| Az oszlopok nem használatos. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|departmentname nevű| Karakterlánc| Hello részleg neve. |
|CostCenter| Karakterlánc| hello hello használati kapcsolódó erőforrás. |
|unitOfMeasure| Karakterlánc| Hello szolgáltatás fel van töltve a hello egység azonosítja. Példa: GB, óra, 10 000 s. |
|Erőforráscsoport| Karakterlánc| hello erőforráscsoport mely hello a telepített mérő futtatja. További információk: [Azure Resource Manager overview](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) (Az Azure Resource Manager áttekintése). |
<br/>
## <a name="see-also"></a>Lásd még:

* [Elszámolási időszakok API](billing-enterprise-api-billing-periods.md)

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Piactér-tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)
