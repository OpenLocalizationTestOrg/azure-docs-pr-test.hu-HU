---
title: "a Power BI Embedded - csatlakozás tooa adatforrás aaaMicrosoft"
description: "A Power BI Embedded, csatlakozás toodata források"
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
ms.openlocfilehash: b1aad6e638104716d90f7e1d060eefcbc9daedbc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooa-data-source"></a>Tooa adatforrás csatlakoztatása
A **Power BI Embedded**, jelentés beágyazása az alkalmazásba. Power BI-jelentés beágyazása az alkalmazásba, hello jelentés csatlakozik-e az alapul szolgáló adatokat toohello **importálása** hello adatok vagy az **közvetlenül csatlakozó** toohello használhatja  **DirectQuery**.

Az alábbiakban közti különbségeket hello **importálási** és **DirectQuery**.

| Importálás | DirectQuery |
| --- | --- |
| A táblákat, oszlopokat *és az adatok* importálhatja vagy másolhatja a hello jelentés adatkészlet. toosee történt toohello alapjául szolgáló adatok megváltozik, frissítse, vagy importálja, a teljes aktuális adatkészletet újra. |Csak *táblákat és oszlopokat* importálhatja vagy másolhatja a hello jelentés adatkészlet. Bármikor megtekintheti a legfrissebb adatok hello. |

Power BI Embedded akkor felhő adatforrások DirectQuery használható, de nem a helyi adatforrások jelenleg.

> [!NOTE]
> hello helyszíni adatátjáró nem támogatott a Power BI Embedded most. Ez azt jelenti, hogy a helyszíni adatforrások DirectQuery nem használhat.

## <a name="supported-data-sources"></a>Támogatott adatforrások

**DirectQuery**
* Az Azure SQL-adatbázis
* Azure SQL Data Warehouse

**Importálás**

Az összes elérhető adatforrások hello Power BI Desktop segítségével importálhatja. Lehetővé teszi a **nem** kell tudni toorefresh belül a Power BI Embedded adatokat. Tooupload módosításokat követően tooyour PBIX-fájl BI Embedded tooPower. Ez a rendelkezésre álló átjáró toono miatt. 

## <a name="benefits-of-using-directquery"></a>DirectQuery használatának előnyei
Nincsenek két elsődleges előnnyel jár, használatakor **DirectQuery**:

* **DirectQuery** segítségével készít képi megjelenítések alatt a nagy adatkészleteknél, ahol egyébként lenne, amelyeknek toofirst importálása összes hello adatokat.
* Az alapul szolgáló adatváltozásokat is van szükség adatok frissítését, és az egyes jelentések hello kell toodisplay aktuális adatokat is nagy adatátvitelt, és amelyeknek újraimportálásának adatokat. Ezzel szemben **DirectQuery** jelentések mindig aktuális adatokat használják.

## <a name="limitations-of-directquery"></a>DirectQuery korlátozásai
   Van néhány korlátozások toousing **DirectQuery**:

* Minden táblának egyetlen adatbázisra kell származnia.
* Ha hello lekérdezés túl összetett, hiba történik. tooremedy hello hiba, hogy kevésbé összetett, meg kell refactor hello lekérdezés. Hello lekérdezés összetett kell lennie, ha szüksége lesz a tooimport hello adatok használata helyett **DirectQuery**.
* Szűrés kapcsolat, korlátozott tooa irányra ahelyett, hogy mindkét irányban.
* Egy oszlop hello adatok típusa nem módosítható.
* Alapértelmezés szerint engedélyezett a mértékek DAX-kifejezések korlátozások kerülnek. Lásd: [DirectQuery és intézkedések](#measures).

<a name="measures"/>

## <a name="directquery-and-measures"></a>DirectQuery és intézkedések
küldött toohello alapul szolgáló adatforrásban tooensure lekérdezések elfogadható teljesítmény, a mértékek a korlátozást. Használata esetén **Power BI Desktop**, speciális felhasználók maguk dönthetik toobypass Ez a korlátozás kiválasztásával **fájl > lehetőségek és beállítások > Beállítások**. A hello **beállítások** párbeszédpanelen válasszon **DirectQuery**, és hello beállítást **nem korlátozott mértékek engedélyezése DirectQuery módban**. Ez a lehetőség van kiválasztva, ha bármely mérték érvényes DAX-kifejezés használható. Lehet, hogy a felhasználók tisztában; azonban, hogy egyes kifejezések, amelyek végrehajtása jól hello adatok importálása azt eredményezheti, nagyon lelassul lekérdezések toohello háttér forrás, amikor a **DirectQuery** mód. 

## <a name="see-also"></a>Lásd még:
* [Ismerkedés a Microsoft Power BI Embedded](power-bi-embedded-get-started.md)
* [Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)

További kérdései vannak? [Próbálja meg a Power BI-Közösség hello](http://community.powerbi.com/)

