---
title: "Azure számlázás vállalati API - használat részleteiről |} Microsoft Docs"
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
ms.openlocfilehash: 5b49220e6eb27544dba54255ee88c56ad79c3141
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="reporting-apis-for-enterprise-customers---usage-details"></a>A vállalati ügyfelek - használat részleteiről jelentéskészítési API-k

A használati részletes API kínál a felhasznált és a beléptetési díjak becsült napi bontásban. Az eredmény a példányok, a mérőszámok és a szervezeti adatokat is tartalmaz. Az API-t számlázási időszak, vagy a megadott kezdő és záró dátum kérdezhetők le. 
## <a name="consumption-apis"></a>Felhasználás API-k


##<a name="request"></a>Kérés 
Hozzá kell adni közös fejléc tulajdonságokhoz megadott [Itt](billing-enterprise-api.md). Ha a számlázott időszak nincs megadva, majd az aktuális elszámolási időszak adat. Egyéni időtartományhoz megadhatók a kezdő és befejező dátum paramétereket, amelyek szerepelnek a formátum éééé-hh-nn. A maximális támogatott időtartomány 36 hónap.  

|Módszer | Kérelem URI-azonosítója|
|-|-|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / usagedetails 
|GET|{billingPeriod} {enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ /billingPeriods/ / usagedetails|
|GET|{enrollmentNumber} https://consumption.Azure.com/v2/enrollments/ / usagedetailsbycustomdate? startTime = 2017-01-01 & endTime = 2017-01-10|

> [!Note]
> API előzetes verzióját használja, a fenti URL-címben v1 v2 cserélje.
>

## <a name="response"></a>Válasz

> A potenciálisan nagy mennyiségű adat az eredmény miatt lapozható memóriakészlet beállítása. A nextLink tulajdonság, ha jelen van, adja meg az adatok a következő lapra mutató hivatkozást. A hivatkozás nem üres, ha az azt jelzi, hogy az utolsó lap. 
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
|id| Karakterlánc| Az API-hívás egyedi azonosítója. |
|Adatok| JSON-tömb| A tömb minden instance\meter napi használati adatait.|
|nextLink| Karakterlánc| Ha az adatok több lapot a nextLink URL-CÍMÉT adja vissza az adatok a következő lapra mutat. |
|accountId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|Termékazonosító| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|resourceLocationId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|consumedServiceID| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|departmentId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|accountOwnerEmail| Karakterlánc| A fiók tulajdonosának e-mail-fiókot. |
|Fióknév| Karakterlánc| A megadott felhasználói fiók neve. |
|serviceAdministratorId| Karakterlánc| E-mail cím a szolgáltatás-rendszergazdát. |
|subscriptionId| int| A mező elavult. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|subscriptionGuid| Karakterlánc| Az előfizetés globális egyedi azonosítója. |
|SubscriptionName| Karakterlánc| Az előfizetés nevét. |
|Dátum| Karakterlánc| A felhasználási létrejöttének dátumát. |
|A termék| Karakterlánc| További részletek a mérő. Példa: A1 (VM) Windows - hozzáférési pont keleti régiója|
|meterId| Karakterlánc| A mérő, amely a kibocsátott használati azonosítója. |
|meterCategory| Karakterlánc| Az Azure platform szolgáltatás használt. |
|meterSubCategory| Karakterlánc| Az Azure szolgáltatás típusa, amelyek hatással lehetnek a sebesség határozza meg. Példa: A1 méretű VM (nem Windows|
|meterRegion| Karakterlánc| Az igénybe vett vagy üzemeltető adatközpont elhelyezkedése, ha a szolgáltatás díjszabása az adatközpontok elhelyezkedésétől is függ. |
|meterName| Karakterlánc| A mérőműszer neve. |
|consumedQuantity| Dupla| A mérő, amely felhasználta mennyisége. |
|resourceRate| Dupla| Az alkalmazandó számlázható egységenként aránya. |
|Költség| Dupla| Amely a mérő vállalt kell fizetni. |
|resourceLocation| Karakterlánc| Az Adatközpont, amelyen fut a mérő azonosítja. |
|consumedService| Karakterlánc| Az Azure platform szolgáltatás használt. |
|instanceId| Karakterlánc| Ez az azonosító, az erőforrás vagy a teljes erőforrás-azonosító. További információkért lásd: [Azure Resource Manager API](https://docs.microsoft.com/rest/api/resources/resources) |
|serviceInfo1| Karakterlánc| Belső Azure szolgáltatás metaadatai. |
|serviceInfo2| Karakterlánc| Például egy kép típusa egy virtuális gép és az ExpressRoute Internetszolgáltató nevét. |
|additionalinfo részben| Karakterlánc| Szolgáltatással kapcsolatos metaadatok. Például egy kép típusa egy virtuális géphez. |
|tags| Karakterlánc| Ügyfél hozzáadott címkéket. További információkért lásd: [címke az Azure-erőforrások rendszerezése](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags). |
|storeServiceIdentifier| Karakterlánc| Az oszlopok nem használatos. Tegye elérhetővé a visszamenőleges kompatibilitás érdekében. |
|departmentname nevű| Karakterlánc| A részleg neve. |
|CostCenter| Karakterlánc| A költségközpont, amely a használati társítva van. |
|unitOfMeasure| Karakterlánc| A szolgáltatás fel van töltve a időegység azonosítja. Példa: GB, óra, 10 000 s. |
|Erőforráscsoport| Karakterlánc| Az erőforráscsoport, amelyben a telepített mérő futtatja. További információk: [Azure Resource Manager overview](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) (Az Azure Resource Manager áttekintése). |
<br/>
## <a name="see-also"></a>Lásd még:

* [Elszámolási időszakok API](billing-enterprise-api-billing-periods.md)

* [Egyenleg és összegzés API](billing-enterprise-api-balance-summary.md)

* [Piactér-tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) 

* [Price Sheet API](billing-enterprise-api-pricesheet.md)
