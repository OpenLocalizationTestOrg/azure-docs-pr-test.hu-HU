---
title: "Azure számlázás vállalati API-k |} Microsoft Docs"
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
ms.openlocfilehash: e3a5f9bcd6b54a51c29df649f1ae8ac185b153a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>A vállalati ügyfelek a Reporting API-k – áttekintés
A Reporting API-k engedélyezése a vállalati Azure-ügyfelek számára használati és elszámolási adatok programozott módon le az elsődleges adatok elemzésére szolgáló eszközöket. 

## <a name="enabling-data-access-to-the-api"></a>Az API adataihoz hozzáférés engedélyezése
* **Hozza létre, vagy az API-kulcs beolvasása** - a vállalati portál és kövesse az oktatóanyag a Súgó - Reporting API-k. Az első szakasza a súgócikk ismerteti, hogyan létrehozásához vagy a megadott beléptetési API-kulcs beolvasása.
* **Kulcsok átadja a API** -az API-kulcsot kell átadni minden hívás a hitelesítéshez és engedélyezéshez. A következő tulajdonság kell lennie, hogy a HTTP-fejlécek

|Fejléc kulcs kérése | Érték|
|-|-|
|Engedélyezés| Adja meg az értéket a következő formátumban: **tulajdonosi {API_KEY}** <br/> Példa: tulajdonosi eyr... 09|

## <a name="consumption-apis"></a>Felhasználás API-k
A Swagger-végpont esetében elérhető [Itt](https://consumption.azure.com/swagger/ui/index) az API-k leírt, amely alatt API könnyen introspection és képességét ügyfél SDK-k használatával engedélyezze a [AutoRest](https://github.com/Azure/AutoRest) vagy [Swagger CodeGen](http://swagger.io/swagger-codegen/). 2014. Előfordulhat, hogy 1 verziótól adatok az API-n keresztül érhető el. 

* **Egyenleg és összegzése** – a [egyenleg és összefoglaló API](billing-enterprise-api-balance-summary.md) kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosításának és keretét díjak adatainak havi összegzése kínál.

* **Használat részleteiről** – a [használatának részletességgel API](billing-enterprise-api-usage-detail.md) kínál a felhasznált és a beléptetési díjak becsült napi bontásban. Az eredmény a példányok, a mérőszámok és a szervezeti adatokat is tartalmaz. Az API-t számlázási időszak, vagy a megadott kezdő és záró dátum kérdezhetők le. 

* **Piactér-tároló kell fizetni** – a [piactér tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) ad vissza a piactér használat alapú költségek bontásban tartalmazza a megadott számlázási időszak vagy a kezdő és befejező dátumok (egyszer díjak nincsenek) nap.

* **Árlista** – a [Price Sheet API](billing-enterprise-api-pricesheet.md) alkalmazható sebessége biztosít minden a megadott regisztrációs és számlázási időszak. 

## <a name="helper-apis"></a>Segéd API-k
 **Számlázási időszakok listában** – a [számlázási időszakok API](billing-enterprise-api-billing-periods.md) időszakokat, amelyek a megadott regisztrációs fordított időrendben adatokkal rendelkezik számlázási listáját adja vissza. Minden időszak mutat az API útvonalat az adat - BalanceSummary, UsageDetails, piactér díjakat és árlista négy csoportjai tulajdonságot tartalmaz.


## <a name="api-response-codes"></a>API-válaszkódok  
|Válasz állapotkódja|Üzenet|Leírás|
|-|-|-|
|200| OKÉ|Nem történt hiba|
|401| Nem engedélyezett| Az API-kulcs nem található, érvénytelen, lejárt stb.|
|404| Nem érhető el| A jelentés a végpont nem található|
|400| Helytelen kérelem| Érvénytelen paraméterek – dátumtartományok, EA számok stb.|
|500| Kiszolgálóhiba| Unexoected hiba a kérelem feldolgozása| 








