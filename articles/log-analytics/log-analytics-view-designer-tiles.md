---
title: "az OMS szolgáltatáshoz Nézettervező aaaTile referencia |} Microsoft Docs"
description: "A Naplóelemzési Nézettervező lehetővé teszi a toocreate egyéni nézetek hello OMS hello OMS-tárházban található adatok különböző képi tartalmazó. Ez a cikk ismerteti a hello-beállítások az egyes hello csempék toouse érhető el az egyéni nézetekben hivatkozást."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 4706abb16b8a3719f5dbe8c89cd61739391ab8f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-view-designer-tile-reference"></a>Napló Analytics Nézettervező csempe leírása
a Log Analyticshez Nézettervező hello lehetővé teszi a toocreate egyéni nézetek hello OMS hello OMS-tárházban található adatok különböző képi tartalmazó. Ez a cikk ismerteti a hello-beállítások az egyes hello csempék toouse érhető el az egyéni nézetekben hivatkozást.

Az adatforrásnézet-tervezőből elérhető további cikkeit a következők:

* [Megtekintheti a tervező](log-analytics-view-designer.md) -hello adatforrásnézet-tervezőből és eljárások létrehozásának és szerkesztésének egyéni nézetek áttekintése.
* [A képi megjelenítés része hivatkozás](log-analytics-view-designer-parts.md) -hivatkozás hello-beállítások az egyes hello tartalmazó csempék éppen úgy az egyéni nézetekben elérhető toouse.

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor minden nézetben lekérdezések úgy kell megírni, a hello [új lekérdezési nyelv](https://go.microsoft.com/fwlink/?linkid=856078).  Bármely hello munkaterület verziófrissítése előtt létrehozott nézetek lesz konvertálva automtically.

hello következő táblázatban hello különböző típusú csempék hello adatforrásnézet-tervezőből érhető el.  hello az alábbi részekben csempe típusonként részletek és azok tulajdonságait.

| Mozaik elrendezés | Leírás |
|:--- |:--- |
| [Szám](#number-tile) |Egyetlen szám megjelenítő lekérdezésből rekordok száma. |
| [Két szám](#two-numbers-tile) |Két egyetlen szám megjelenítő számát is két különböző lekérdezéseket bejegyzéseit. |
| [Fánk](#donut-tile) |Fánk diagram hello center összefoglaló érték lekérdezés alapján. |
| [Vonaldiagram & kihívás](#line-chart-amp-callout-tile) |A lekérdezés és összefoglaló értékkel felirat alapján vonaldiagram. |
| [Vonaldiagram](#line-chart-tile) |A vonaldiagram lekérdezés alapján. |
| [Két ütemtervek](#two-timelines-tile) |Oszlopdiagram két adatsorozattal egyes külön lekérdezés alapján. |

## <a name="number-tile"></a>Szám csempe
Hello **szám** csempe érkező napló lekérdezés és a címkék száma hello megjelenítő egyetlen számot jeleníti meg.

![Szám csempe](media/log-analytics-view-designer/tile-number.png)

| Beállítás | Leírás |
|:--- |:--- |
| Név |Szöveg toodisplay hello hello tetején csempére. |
| Leírás |Szöveg toodisplay hello csempe neve alatt. |
| **Mozaik elrendezés** | |
| Jelmagyarázat |Szöveg toodisplay hello érték alatti. |
| Lekérdezés |Lekérdezés toorun.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| **Speciális** |**> Adatfolyamot ellenőrzése** |
| Engedélyezve |Válassza ki, ha adatfolyam ellenőrzési hello csempe engedélyezni kell.  Ez egy másik üzenetet biztosít, ha adatokat hello csempe nem érhető el.  Ez az üzenet általánosan használt tooprovide hello ideiglenes időszak, amikor hello nézet van telepítve, és a rendelkezésre álló adatok származnak. |
| Lekérdezés |Lekérdezési toorun toocheck, ha az adatok hello nézet érhető el.  Ha hello lekérdezés eredménytelen, majd egy üzenet jelenik meg hello fő lekérdezésből hello érték helyett. |
| Üzenet |Üzenet toodisplay Ha hello adatfolyam ellenőrzési lekérdezés adatot nem ad vissza.  Ha megadja, hogy nincs üzenet *végrehajtása Assessment* jelenik meg. |


## <a name="two-numbers-tile"></a>Két szám csempéje
Hello **két szám** csempe megjeleníti a két szám megjelenítő hello az egyes bejegyzések két különböző naplófájl-lekérdezések és a címkék száma.

![Két szám csempéje](media/log-analytics-view-designer/tile-two-numbers.png)

| Beállítás | Leírás |
|:--- |:--- |
| Név |Szöveg toodisplay hello hello tetején csempére. |
| Leírás |Szöveg toodisplay hello csempe neve alatt. |
| **Első csempe** | |
| Jelmagyarázat |Szöveg toodisplay hello érték alatti. |
| Lekérdezés |Lekérdezés toorun.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| **Második csempe** | |
| Jelmagyarázat |Szöveg toodisplay hello érték alatti. |
| Lekérdezés |Lekérdezés toorun.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| **Speciális** |**> Adatfolyamot ellenőrzése** |
| Engedélyezve |Válassza ki, ha adatfolyam ellenőrzési hello csempe engedélyezni kell.  Ez egy másik üzenetet biztosít, ha adatokat hello csempe nem érhető el.  Ez az üzenet általánosan használt tooprovide hello ideiglenes időszak, amikor hello nézet van telepítve, és a rendelkezésre álló adatok származnak. |
| Lekérdezés |Lekérdezési toorun toocheck, ha az adatok hello nézet érhető el.  Ha hello lekérdezés eredménytelen, majd egy üzenet jelenik meg hello fő lekérdezésből hello érték helyett. |
| Üzenet |Üzenet toodisplay Ha hello adatfolyam ellenőrzési lekérdezés adatot nem ad vissza.  Ha megadja, hogy nincs üzenet *végrehajtása Assessment* jelenik meg. |


## <a name="donut-tile"></a>Fánk csempe
Hello **fánk** csempe egy egyetlen napló lekérdezésben érték oszlop összesített számát jeleníti meg.  hello fánk grafikusan hello felső három rekordok eredményeit jeleníti meg.

![Fánk csempe](media/log-analytics-view-designer/tile-donut.png)

| Beállítás | Leírás |
|:--- |:--- |
| Név |Szöveg toodisplay hello hello tetején csempére. |
| Leírás |Szöveg toodisplay hello csempe neve alatt. |
| **Fánk** | |
| Lekérdezés |Lekérdezés toorun hello fánk a.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Ez általában az hello használó lekérdezések **mérték** kulcsszó toosummarize eredmények. |
| **Fánk** |**> Center** |
| Szöveg |Szöveg toodisplay alatt hello fánk hello értéket. |
| Művelet |hello művelet tooperform hello érték tulajdonság toosummarize tooa egyetlen értékét.<br><br>-Összeg: Hello értékek hello tulajdonság értéke az összes rekord hozzáadása.<br>-Százalékos: Hello tulajdonság értéke képest toohello rekordokból összesítve hello értékek százalékos összegzi az összes rekord értékeit. |
| Center művelet során használt eredményt értékek |Opcionálisan kattintson hello pluszjel tooadd egy vagy több értéket.  hello lekérdezési eredmények hello lesz korlátozott toorecords értékekkel hello tulajdonság.  Ha nincs érték ad hozzá, mint minden rekordot szerepelnek hello lekérdezés. |
| **Fánk** |**> További beállítások** |
| Színek |hello szín toodisplay az egyes hello három felső tulajdonságot.  Toospecify alternatív színek adott tulajdonságértékek, majd használja leképezési speciális színét. |
| Speciális szín leképezése |Megjeleníti egy adott tulajdonságértékeket színét.  Ha hello megadott érték a hello felső három, majd hello másodlagos színt helyett hello szabványos szín jelenik meg.  Ha hello tulajdonság nem szerepel a hello felső három, akkor a hello szín nem jelenik meg. |
| **Speciális** |**> Adatfolyamot ellenőrzése** |
| Engedélyezve |Válassza ki, ha adatfolyam ellenőrzési hello csempe engedélyezni kell.  Ez egy másik üzenetet biztosít, ha adatokat hello csempe nem érhető el.  Ez az üzenet általánosan használt tooprovide hello ideiglenes időszak, amikor hello nézet van telepítve, és a rendelkezésre álló adatok származnak. |
| Lekérdezés |Lekérdezési toorun toocheck, ha az adatok hello nézet érhető el.  Ha hello lekérdezés eredménytelen, majd egy üzenet jelenik meg hello fő lekérdezésből hello érték helyett. |
| Üzenet |Üzenet toodisplay Ha hello adatfolyam ellenőrzési lekérdezés adatot nem ad vissza.  Ha megadja, hogy nincs üzenet *végrehajtása Assessment* jelenik meg. |


## <a name="line-chart-tile"></a>Sor diagram csempe
Hello **vonaldiagram** csempe egy grafikonon napló lekérdezésből több adatsorozattal adott idő alatt.  

![Vonaldiagram & kihívás csempe](media/log-analytics-view-designer/tile-line-chart.png)

| Beállítás | Leírás |
|:--- |:--- |
| Név |Szöveg toodisplay hello hello tetején csempére. |
| Leírás |Szöveg toodisplay hello csempe neve alatt. |
| **Vonaldiagram** | |
| Lekérdezés |Lekérdezés toorun hello sor a diagramon.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Ez általában az hello használó lekérdezések **mérték** kulcsszó toosummarize eredmények.  Ha hello lekérdezés használ hello **időköz** kulcsszót, majd hello hello diagram x tengelyéhez fogja használni az adott időszakban.  Ha hello lekérdezés nem tartalmazza a hello **időköz** kulcsszót, majd óránként intervallumok használt x tengely hello. |
| **Vonaldiagram** |**> Y tengely** |
| A Logaritmikus skála használata |Válassza ki a toouse hello y tengely a Logaritmikus skála. |
| egység |Hello lekérdezés által visszaadott hello értékek hello egységeket adjon meg.  Ez az információ használt toodisplay címkék hello értéktípusok jelző hello diagram és opcionálisan hello értékek alakításának.  Hello **egységtípus** hello egység hello kategória határozza meg, és határozza meg a hello **aktuális egységtípus** elérhető értékek.  Ha válasszon ki egy értéket **átalakítása** hello numerikus értékek hello konvertálja, majd **aktuális egység** írja be a toohello **átalakítása** típusa. |
| Egyéni felirat |Szöveg toodisplay hello Y tengely következő toohello címke hello egység típushoz.  Ha nincs címke van megadva, akkor csak a hello egységtípus jelenik meg. |
| **Speciális** |**> Adatfolyamot ellenőrzése** |
| Engedélyezve |Válassza ki, ha adatfolyam ellenőrzési hello csempe engedélyezni kell.  Ez egy másik üzenetet biztosít, ha adatokat hello csempe nem érhető el.  Ez az üzenet általánosan használt tooprovide hello ideiglenes időszak, amikor hello nézet van telepítve, és a rendelkezésre álló adatok származnak. |
| Lekérdezés |Lekérdezési toorun toocheck, ha az adatok hello nézet érhető el.  Ha hello lekérdezés eredménytelen, majd egy üzenet jelenik meg hello fő lekérdezésből hello érték helyett. |
| Üzenet |Üzenet toodisplay Ha hello adatfolyam ellenőrzési lekérdezés adatot nem ad vissza.  Ha megadja, hogy nincs üzenet *végrehajtása Assessment* jelenik meg. |


## <a name="line-chart--callout-tile"></a>Sor diagram & kihívás csempe
Hello **diagram & kihívás sor** csempe egy grafikonon napló lekérdezésből több adatsorozattal időt és az összesített érték egy kihívás keresztül.  

![Vonaldiagram & kihívás csempe](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Beállítás | Leírás |
|:--- |:--- |
| Név |Szöveg toodisplay hello hello tetején csempére. |
| Leírás |Szöveg toodisplay hello csempe neve alatt. |
| **Vonaldiagram** | |
| Lekérdezés |Lekérdezés toorun hello sor a diagramon.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Ez általában az hello használó lekérdezések **mérték** kulcsszó toosummarize eredmények.  Ha hello lekérdezés használ hello **időköz** kulcsszót, majd hello hello diagram x tengelyéhez fogja használni az adott időszakban.  Ha hello lekérdezés nem tartalmazza a hello **időköz** kulcsszót, majd óránként intervallumok használt x tengely hello. |
| **Vonaldiagram** |**> Kihívás** |
| Kihívás |Cím szöveg toodisplay hello kihívás érték felett. |
| Az adatsorozat neve |Hello adatsorozat toouse hello kihívás értékhez tulajdonság értéke.  Adatsorozatok valósul meg, ha minden hello lekérdezésből rekordjai. |
| Művelet |hello művelet tooperform hello érték toosummarize tooa egyetlen tulajdonságértéket hello kihívás a.<br>-Átlagos: Az összes rekord hello értéket átlaga.<br><br>-Számláló: Hello lekérdezés által visszaadott összes rekord száma.<br>-Utolsó minta: Hello utolsó időköze hello diagram szereplő értéket.<br>-Maximális: Hello diagram szereplő hello időközök maximális értéket.<br>-Min: Hello diagram szereplő hello időszakok közül a minimális érték.<br>– Összeg: Az összes rekord hello értéket összege. |
| **Vonaldiagram** |**> Y tengely** |
| A Logaritmikus skála használata |Válassza ki a toouse hello y tengely a Logaritmikus skála. |
| egység |Hello lekérdezés által visszaadott hello értékek hello egységeket adjon meg.  Ez az információ használt toodisplay címkék hello értéktípusok jelző hello diagram és opcionálisan hello értékek alakításának.  Hello **egységtípus** hello egység hello kategória határozza meg, és határozza meg a hello **aktuális egységtípus** elérhető értékek.  Ha válasszon ki egy értéket **átalakítása** hello numerikus értékek hello konvertálja, majd **aktuális egység** írja be a toohello **átalakítása** típusa. |
| Egyéni felirat |Szöveg toodisplay hello Y tengely következő toohello címke hello egység típushoz.  Ha nincs címke van megadva, akkor csak a hello egységtípus jelenik meg. |
| **Speciális** |**> Adatfolyamot ellenőrzése** |
| Engedélyezve |Válassza ki, ha adatfolyam ellenőrzési hello csempe engedélyezni kell.  Ez egy másik üzenetet biztosít, ha adatokat hello csempe nem érhető el.  Ez az üzenet általánosan használt tooprovide hello ideiglenes időszak, amikor hello nézet van telepítve, és a rendelkezésre álló adatok származnak. |
| Lekérdezés |Lekérdezési toorun toocheck, ha az adatok hello nézet érhető el.  Ha hello lekérdezés eredménytelen, majd egy üzenet jelenik meg hello fő lekérdezésből hello érték helyett. |
| Üzenet |Üzenet toodisplay Ha hello adatfolyam ellenőrzési lekérdezés adatot nem ad vissza.  Ha megadja, hogy nincs üzenet *végrehajtása Assessment* jelenik meg. |


## <a name="two-timelines-tile"></a>Két ütemtervek csempe
Hello **két ütemtervek** csempe megjeleníti a két naplófájl-lekérdezések eredményének hello oszlopdiagramok idővel.  Valamennyi adatsorozatnál felirat jelenik meg.  

![Két ütemtervek csempe](media/log-analytics-view-designer/tile-two-timelines.png)

| Beállítás | Leírás |
|:--- |:--- |
| Név |Szöveg toodisplay hello hello tetején csempére. |
| Leírás |Szöveg toodisplay hello csempe neve alatt. |
| Első diagram | |
| Jelmagyarázat |Szöveg toodisplay hello kihívás hello első sorozata alapján. |
| Szín |Hello oszlopokhoz hello első sorozat toouse színét. |
| A diagram lekérdezés |Lekérdezés toorun hello első adatsorozathoz.  hello számát hello rekordok időbeli minden időközhöz hello diagram oszlop szerint fog megjelenni. |
| Művelet |hello művelet tooperform hello érték toosummarize tooa egyetlen tulajdonságértéket hello kihívás a.<br><br>-Átlagos: Az összes rekord hello értéket átlaga.<br>-Számláló: Hello lekérdezés által visszaadott összes rekord száma.<br>-Utolsó minta: Hello utolsó időköze hello diagram szereplő értéket.<br>-Maximális: Hello diagram szereplő hello időközök maximális értéket. |
| **Második diagram** | |
| Jelmagyarázat |Szöveg toodisplay hello kihívás hello második sorozata alapján. |
| Szín |Hello oszlopokhoz hello második sorozat toouse színét. |
| A diagram lekérdezés |Lekérdezés toorun hello második adatsorozathoz.  hello számát hello rekordok időbeli minden időközhöz hello diagram oszlop szerint fog megjelenni. |
| Művelet |hello művelet tooperform hello érték toosummarize tooa egyetlen tulajdonságértéket hello kihívás a.<br><br>-Átlagos: Az összes rekord hello értéket átlaga.<br>-Számláló: Hello lekérdezés által visszaadott összes rekord száma.<br>-Utolsó minta: Hello utolsó időköze hello diagram szereplő értéket.<br>-Maximális: Hello diagram szereplő hello időközök maximális értéket. |
| **Speciális** |**> Adatfolyamot ellenőrzése** |
| Engedélyezve |Válassza ki, ha adatfolyam ellenőrzési hello csempe engedélyezni kell.  Ez egy másik üzenetet biztosít, ha adatokat hello csempe nem érhető el.  Ez az üzenet általánosan használt tooprovide hello ideiglenes időszak, amikor hello nézet van telepítve, és a rendelkezésre álló adatok származnak. |
| Lekérdezés |Lekérdezési toorun toocheck, ha az adatok hello nézet érhető el.  Ha hello lekérdezés eredménytelen, majd egy üzenet jelenik meg hello fő lekérdezésből hello érték helyett. |
| Üzenet |Üzenet toodisplay Ha hello adatfolyam ellenőrzési lekérdezés adatot nem ad vissza.  Ha megadja, hogy nincs üzenet *végrehajtása Assessment* jelenik meg. |


## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toosupport hello lekérdezések a csempéket.
* Adja hozzá [képi megjelenítés részek](log-analytics-view-designer-parts.md) tooyour egyéni nézetet.
