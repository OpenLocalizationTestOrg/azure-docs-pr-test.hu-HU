---
title: "hello JavaScript API használatával jelentéseket aaaInteract |} Microsoft Docs"
description: "A Power BI Embedded, kezelheti a jelentéseket hello JavaScript API használatával"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: bdd885d3-1b00-4dcf-bdff-531eb1f97bfb
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 657e4d5cee031bdda173ab3f451cc19b93ddb17b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="interact-with-power-bi-reports-using-hello-javascript-api"></a>Kezelheti a Power BI-jelentéseket hello JavaScript API használatával
hello Power BI JavaScript API lehetővé teszi, hogy Ön tooeasily Power BI-jelentéseket beágyazása az alkalmazások. A hello API az alkalmazások programozott módon működhetnek együtt a különböző jelentési elemek, például a lapok és a szűrők. Ez az interaktivitás a Power BI-jelentéseket az alkalmazás még szervesebb részévé teszi.

Az alkalmazás Power BI-jelentés beágyazása iframe üzemeltetett hello alkalmazás részeként használatával. hello iframe között az alkalmazás- és hello jelentés határaként szolgálnak, mint a kép a következő hello látható. 

![A Power BI beágyazott iframe eleme JavaScript API nélkül](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-1.png)

hello JavaScript API hello nélkül jelentés és az alkalmazás nem kommunikál egymással, de hello iframe hello beágyazás folyamat sokkal könnyebb lesz. Interakció hiánya teheti érzi hello jelentés valóban nem hello alkalmazás részét. hello jelentés és az alkalmazás valóban szükség van a toocommunicate oda-vissza, úgy, ahogy a következő kép hello.

![A Power BI beágyazott iframe eleme JavaScript API-val](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-2.png)

hello Power BI JavaScript API lehetővé teszi toowrite kódot, amely képes biztonságosan továbbítása hello iframe-határ. Ez lehetővé teszi, hogy az alkalmazás tooprogrammatically valamilyen műveletet hajt végre egy jelentést, és toolisten azon eseményeit, hello jelentésen belül felhasználók hajthatnak végre műveleteket.

## <a name="what-can-you-do-with-hello-power-bi-javascript-api"></a>Mire szolgál a Power BI JavaScript API hello?
A JavaScript API hello készült jelentések kezelése, keresse meg a jelentésekben toopages, a jelentések szűréséhez és beágyazás események kezelésére. hello alábbi ábrán látható hello hello API struktúráját.

![A Power BI JavaScript API-t bemutató ábra](media/powerbi-embedded-interact-with-reports/powerbi-embedded-interact-report-3.png)

### <a name="manage-reports"></a>Jelentések kezelése
hello Javascript API lehetővé teszi hello jelentés és a lap szintjén toomanage viselkedést:

* Egy adott Power BI-jelentés biztonságos beágyazása az alkalmazás - hello próbálja [bemutató alkalmazás beágyazása](http://azure-samples.github.io/powerbi-angular-client/#/scenario1)
  * Hozzáférési jogkivonat beállítása
* Hello jelentés konfigurálása
  * Engedélyezése és letiltása hello szűrő és a lap navigációs ablaktábla – hello próbálja [beállításait bemutató alkalmazás frissítése](http://azure-samples.github.io/powerbi-angular-client/#/scenario6)
  * Lapok és a szűrők alapértelmezett értékeinek beállítása – hello próbálja [készlet alapértelmezett bemutató](http://azure-samples.github.io/powerbi-angular-client/#/scenario5)
* Teljes képernyős mód vagy kilépés a teljes képernyős módból

[További információ jelentések beágyazásáról](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Embedding-Basics)

### <a name="navigate-toopages-in-a-report"></a>Keresse meg a jelentésekben toopages
hello JavaScript API enbales meg toodiscover összes lapok jelentés és tooset hello aktuális lapon. Próbálja meg hello [navigációs bemutató alkalmazás](http://azure-samples.github.io/powerbi-angular-client/#/scenario3).

[További információ a lapok közötti navigálásról](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Page-Navigation)

### <a name="filter-a-report"></a>Jelentés szűrése
hello JavaScript API biztosít az alapszintű és speciális szűrési lehetőségek beágyazott jelentések és jelentés oldalán. Próbálja hello [bemutató alkalmazás szűrés](http://azure-samples.github.io/powerbi-angular-client/#/scenario4), és tekintse át az egyes bevezető kódot.  

#### <a name="basic-filters"></a>Alapszintű szűrők
Alapszintű szűrő egy oszlop vagy a hierarchia szint helyezkedik el, és értékek tooinclude vagy kizárási listáját tartalmazza.

```
const basicFilter: pbi.models.IBasicFilter = {
  $schema: "http://powerbi.com/product/schema#basic",
  target: {
    table: "Store",
    column: "Count"
  },
  operator: "In",
  values: [1,2,3,4]
}
```


#### <a name="advanced-filters"></a>Speciális szűrők
Speciális szűrők hello logikai operátor használata és vagy vagy, és el kell fogadnia egy vagy két feltételt, egyenként a saját operátor és értéke. Támogatott feltételek:

* None
* LessThan
* LessThanOrEqual
* GreaterThan
* GreaterThanOrEqual
* Contains
* DoesNotContain
* StartsWith
* DoesNotStartWith
* Is
* IsNot
* IsBlank
* IsNotBlank

```
const advancedFilter: pbi.models.IAdvancedFilter = {
  $schema: "http://powerbi.com/product/schema#advanced",
  target: {
    table: "Store",
    column: "Name"
  },
  logicalOperator: "Or",
  conditions: [
    {
      operator: "Contains",
      value: "Wash"
    },
    {
      operator: "Contains",
      value: "Park"
    }
  ]
}
```
[További információk a szűrésről](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Filters)

### <a name="handling-events"></a>Események kezelése
Ezenkívül hello iframe toosending adatait, az alkalmazás akkor is jelentkezhet hello hello iframe érkező események a következő információkat:

* Beágyazás
  * loaded
  * error
* Jelentések
  * pageChanged
  * dataSelected (hamarosan)

[További tudnivalók az események kezeléséről](https://github.com/Microsoft/PowerBI-JavaScript/wiki/Handling-Events)

## <a name="next-steps"></a>Következő lépések
A Power BI JavaScript API hello kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [JavaScript API wiki](https://github.com/Microsoft/PowerBI-JavaScript/wiki)
* [Objektummodell-hivatkozás](https://microsoft.github.io/powerbi-models/modules/_models_.html)
* Példák
  * [Angular](http://azure-samples.github.io/powerbi-angular-client)
  * [Ember](https://github.com/Microsoft/powerbi-ember)
* [Élő bemutató](https://microsoft.github.io/PowerBI-JavaScript/demo/)

