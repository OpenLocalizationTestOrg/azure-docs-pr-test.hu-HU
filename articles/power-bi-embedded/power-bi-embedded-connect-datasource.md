---
title: "Microsoft Power BI Embedded – egy adatforráshoz való kapcsolódáshoz"
description: "A Power BI Embedded, kapcsolódás az adatforrásokhoz"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2a4caeb3-255d-4215-9554-0ca8e3568c13
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: 9f614bbc63eae788aa52132c8f0e42ad8963559a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-a-data-source"></a>Kapcsolódás adatforráshoz
A **Power BI Embedded**, jelentés beágyazása az alkalmazásba. A Power BI-jelentés beágyazása az alkalmazásba, a jelentés csatlakozik-e az alapul szolgáló adatokat által **importálása** egy másolatot, az adatok vagy a **közvetlenül csatlakozó** a forrás-történő **DirectQuery** .

Az alábbiakban a két módszer (**importálás** és **DirectQuery**) közti különbségeket láthatja.

| Importálás | DirectQuery |
| --- | --- |
| A táblákat, oszlopokat *és az adatok* importálhatja vagy másolhatja be a jelentésben adatkészlet. A mögöttes adatok módosulásainak megtekintéséhez frissítse, vagy importáljon egy teljes aktuális adatkészletet újra. |Csak *táblákat és oszlopokat* importálhatja vagy másolhatja be a jelentésben adatkészlet. Mindig a legfrissebb adatok megtekintéséhez. |

Power BI Embedded akkor felhő adatforrások DirectQuery használható, de nem a helyi adatforrások jelenleg.

> [!NOTE]
> Az On-Premises átjáró jelenleg nem támogatott a Power BI Embedded. Ez azt jelenti, hogy a helyszíni adatforrások DirectQuery nem használhat.

## <a name="supported-data-sources"></a>Támogatott adatforrások

**DirectQuery**
* Az Azure SQL-adatbázis
* Azure SQL Data Warehouse

**Importálás**

Az összes az elérhető adatforrásokhoz Power BI Desktop segítségével importálhatja. Lehetővé teszi a **nem** kell lehet csatlakozni a Power BI Embedded belül. A PBIX-fájl a Power BI Embedded feltölteni a módosításokat fog. Rendelkezésre álló átjáró okozza. 

## <a name="benefits-of-using-directquery"></a>DirectQuery használatának előnyei
Nincsenek két elsődleges előnnyel jár, használatakor **DirectQuery**:

* **DirectQuery** is létrehozhat képi megjelenítések alatt a nagy adatkészleteknél, ahol egyébként lenne, amelyeknek első importálandó összes adatot.
* Az alapul szolgáló adatváltozásokat lehet szükség, adatok frissítését, és az egyes jelentések aktuális adatok megjelenítéséhez szükség lehet szükség nagy adatátvitelt, és amelyeknek újraimportálásának adatok. Ezzel szemben **DirectQuery** jelentések mindig aktuális adatokat használják.

## <a name="limitations-of-directquery"></a>DirectQuery korlátozásai
   Néhány korlátozást érdemes használatával **DirectQuery**:

* Minden táblának egyetlen adatbázisra kell származnia.
* Ha a lekérdezés túl összetett, hiba történik. A hiba elhárításához a lekérdezést, hogy kevésbé összetett kell refactor. A lekérdezés összetett kell lennie, ha szüksége lesz az adatimportálás használata helyett **DirectQuery**.
* Kapcsolat szűrés csak egy irányban ahelyett, hogy mindkét irányban korlátozva.
* Egy oszlop adattípusa nem módosítható.
* Alapértelmezés szerint engedélyezett a mértékek DAX-kifejezések korlátozások kerülnek. Lásd: [DirectQuery és intézkedések](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery és intézkedések
Az alapul szolgáló adatforrásban küldött lekérdezések rendelkezik a megfelelő teljesítmény érdekében az korlátozások intézkedések a küszöbértéket. Használata esetén **Power BI Desktop**, speciális felhasználók engedélyezze a Ez a korlátozás kiválasztásával **fájl > lehetőségek és beállítások > Beállítások**. Az a **beállítások** párbeszédpanelen válasszon **DirectQuery**, válassza ki, **nem korlátozott mértékek engedélyezése DirectQuery módban**. Ez a lehetőség van kiválasztva, ha bármely mérték érvényes DAX-kifejezés használható. Lehet, hogy a felhasználók tisztában; azonban, hogy néhány kifejezés, amely jól végre, ha az adatok importálása azt eredményezheti, nagyon lassú lekérdezi a háttérrendszer forrás, amikor a **DirectQuery** mód. 

## <a name="see-also"></a>Lásd még:
* [Ismerkedés a Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

További kérdései vannak? [Tegye próbára a Power BI közösségét](http://community.powerbi.com/)

