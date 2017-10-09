---
title: "Vállalati API-k számlázási aaaAzure |} Microsoft Docs"
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
ms.openlocfilehash: 017cecc57ad6bdeb402b5d9d57fc95df9b033a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a>A vállalati ügyfelek a Reporting API-k – áttekintés
hello Reporting API-k engedélyezése vállalati Azure ügyfelek tooprogrammatically lekéréses használati és elszámolási adatok az elsődleges adatok elemzésére szolgáló eszközöket. 

## <a name="enabling-data-access-toohello-api"></a>Adatok hozzáférési toohello API engedélyezése
* **Hozza létre vagy hello API-kulcs beolvasása** - toohello vállalati portálon, és hajtsa végre hello az oktatóanyagban a Súgó - Reporting API-k. hello első szakasza a Súgó cikk azt ismerteti, hogyan toogenerate vagy olvashatók be hello API key hello a megadott regisztrációs.
* **Kulcsok benyújtása hello API** -hello API-kulcsot kell toobe átadott minden hívás a hitelesítéshez és engedélyezéshez. hello következő kell tulajdonságot toobe toohello HTTP-fejlécek

|Fejléc kulcs kérése | Érték|
|-|-|
|Engedélyezés| Hello értéket adjon meg a következő formátumban: **tulajdonosi {API_KEY}** <br/> Példa: tulajdonosi eyr... 09|

## <a name="consumption-apis"></a>Felhasználás API-k
A Swagger-végpont esetében elérhető [Itt](https://consumption.azure.com/swagger/ui/index) hello az API-k leírt, amely alatt kell engedélyezése könnyen introspection hello API és hello képességét toogenerate ügyfél SDK-k használatával [AutoRest](https://github.com/Azure/AutoRest) vagy [ Swagger CodeGen](http://swagger.io/swagger-codegen/). 2014. Előfordulhat, hogy 1 verziótól adatok az API-n keresztül érhető el. 

* **Egyenleg és összegzése** – hello [egyenleg és összefoglaló API](billing-enterprise-api-balance-summary.md) kiegyensúlyozza, új, Azure piactér szolgáltatási díjak, módosításának és keretét díjak adatainak havi összegzése kínál.

* **Használat részletei** – hello [használatának részletességgel API](billing-enterprise-api-usage-detail.md) kínál a felhasznált és a beléptetési díjak becsült napi bontásban. hello eredmény példányok, mérőszámok és szervezeti egységek adatokat is tartalmaz. hello API számlázási időszak szerint, vagy a megadott kezdési és befejezési dátum kérdezhetők le. 

* **Piactér tároló kell fizetni** – hello [piactér tároló ingyenesen elérhető API](billing-enterprise-api-marketplace-storecharge.md) adja vissza a hello piactér használat alapú költségek lebontása napi hello megadott számlázási időszak vagy kezdő és befejező dátumok (egyszer díjak kimaradnak) .

* **Árlista** – hello [Price Sheet API](billing-enterprise-api-pricesheet.md) hello alkalmazható arány biztosít minden mérni a regisztráció és a számlázási időszak hello. 

## <a name="helper-apis"></a>Segéd API-k
 **Számlázási időszakok listában** – hello [számlázási időszakok API](billing-enterprise-api-billing-periods.md) időszakokat, amelyek hello beléptetési megadott fordított időrendben adatokkal rendelkeznie számlázási listáját adja vissza. Minden időszak toohello API útvonalat az adat - BalanceSummary, UsageDetails, piactér díjakat és árlista hello négy csoportjai mutató tulajdonságot tartalmaz.


## <a name="api-response-codes"></a>API-válaszkódok  
|Válasz állapotkódja|Üzenet|Leírás|
|-|-|-|
|200| OKÉ|Nem történt hiba|
|401| Nem engedélyezett| Az API-kulcs nem található, érvénytelen, lejárt stb.|
|404| Nem érhető el| A jelentés a végpont nem található|
|400| Helytelen kérelem| Érvénytelen paraméterek – dátumtartományok, EA számok stb.|
|500| Kiszolgálóhiba| Unexoected hiba a kérelem feldolgozása| 









