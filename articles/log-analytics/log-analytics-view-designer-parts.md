---
title: "az OMS szolgáltatáshoz Nézettervező aaaPart referencia |} Microsoft Docs"
description: "A Naplóelemzési Nézettervező lehetővé teszi a toocreate egyéni nézetek hello OMS hello OMS-tárházban található adatok különböző képi tartalmazó. Ez a cikk ismerteti a hello-beállítások az egyes hello képi megjelenítés részek elérhető toouse az egyéni nézetekben hivatkozást."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 6a19a451cf4cefd2fa5c94e6f61d812c4f820f73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Napló Analytics Nézettervező képi megjelenítés rész referencia
hello Naplóelemzési Az adatforrásnézet-tervezőből toocreate egyéni nézetek hello OMS adattárból hello OMS adatok különböző képi tartalmazó lehetővé teszi. Ez a cikk ismerteti a hello-beállítások az egyes hello képi megjelenítés részek elérhető toouse az egyéni nézetekben hivatkozást.

Az adatforrásnézet-tervezőből elérhető további cikkeit a következők:

* [Megtekintheti a tervező](log-analytics-view-designer.md) -hello adatforrásnézet-tervezőből és eljárások létrehozásának és szerkesztésének egyéni nézetek áttekintése.
* [Hivatkozás csempe](log-analytics-view-designer-tiles.md) -hivatkozás hello-beállítások az egyes hello tartalmazó csempék éppen úgy az egyéni nézetekben elérhető toouse.

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor minden nézetben lekérdezések úgy kell megírni, a hello [új lekérdezési nyelv](https://go.microsoft.com/fwlink/?linkid=856078).  Bármely hello munkaterület verziófrissítése előtt létrehozott nézetek lesz konvertálva automtically.

hello következő táblázat bemutatja hello különféle csempék hello adatforrásnézet-tervezőből érhető el.  hello az alábbi részekben csempe típusonként részletek és azok tulajdonságait.

| A nézettípus | Leírás |
|:--- |:--- |
| [A lekérdezések listája](#list-of-queries-part) |A naplófájl-keresési lekérdezések listáját jeleníti meg.  hello felhasználó minden egyes lekérdezés toodisplay rákattinthat az eredményeket. |
| [Lista és száma](#number-amp-list-part) |Fejléce egyetlen számú ábrázoló bejegyzések száma a napló keresési lekérdezés rendelkezik.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg. |
| [Két lista és számok](#two-numbers-amp-list-part) |Fejléce rendelkezik két szám megjelenítő érkező saját naplófájlt keresési lekérdezések száma.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg. |
| [Fánk & lista](#donut-amp-list-part) |Fejléc egy egyetlen napló lekérdezésben érték oszlop összesített számát jeleníti meg.  hello fánk grafikusan hello felső három rekordok eredményeit jeleníti meg. |
| [Két lista és ütemtervek](#two-timelines-amp-list-part) |Egy érték oszlopból napló lekérdezésben fejléc megjelenítése hello egy megjelenítése egyetlen számnak kihívás az oszlopdiagramok idővel két naplófájl-lekérdezések eredményének foglalja össze.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg. |
| [Információ](#information-part) |Fejléc statikus szöveget és egy nem kötelező hivatkozást jeleníti meg.  Lista egy vagy több elem statikus szöveget és címét jeleníti meg. |
| [Lista, és az vonaldiagram, a kihívás](#line-chart-callout-amp-list-part) |Fejléc egy grafikonon napló lekérdezésből több adatsorozattal időt és az összesített érték egy kihívás keresztül.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg. |
| [Lista és a vonaldiagram](#line-chart-amp-list-part) |Fejléc egy grafikonon napló lekérdezésből több adatsorozattal adott idő alatt.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg. |
| [Sor diagramok rész verem](#stack-of-line-charts-part) |Megjeleníti a napló lekérdezésből több sorozat három különálló vonaldiagramok adott idő alatt. |

## <a name="list-of-queries-part"></a>Lekérdezések rész listája
A naplófájl-keresési lekérdezések listáját jeleníti meg.  hello felhasználó minden egyes lekérdezés toodisplay rákattinthat az eredményeket.  hello nézet alapértelmezés szerint egyetlen lekérdezést tartalmazza, és kattintson **+ lekérdezés** tooadd további lekérdezések.

![Lekérdezések nézet listája](media/log-analytics-view-designer/view-list-queries.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Cím |Szöveg toodisplay hello nézet hello tetején. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Az előre kijelölt szűrők |Vesszővel elválasztott felsorolása tulajdonságok tooinclude hello szűrő bal oldali ablaktáblán, amikor hello felhasználó kiválaszt egy lekérdezést. |
| Leképezési módban |Kezdeti nézet jelenik meg, ha a lekérdezés hello van kiválasztva.  hello felhasználó kiválaszthatja a rendelkezésre álló nézetek hello lekérdezés megnyitása után. |
| **Lekérdezések** | |
| Keresési lekérdezés |Lekérdezés toorun. |
| Rövid név |Hello lekérdezés toodisplay toohello felhasználói leíró neve. |

## <a name="number--list-part"></a>Lista és a szám része
Fejléce egyetlen számú ábrázoló bejegyzések száma a napló keresési lekérdezés rendelkezik.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg.

![Lekérdezések nézet listája](media/log-analytics-view-designer/view-number-list.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello nézet hello tetején. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| Ikon használata |Válassza ki a toohave hello ikon megjelenítése. |
| **Cím** | |
| Jelmagyarázat |Szöveg toodisplay hello fejléc hello tetején. |
| Lekérdezés |Lekérdezés toorun hello fejléc.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| **Lista** | |
| Lekérdezés |Lekérdezés toorun hello listáját.  első két tulajdonságainak hello hello első tíz eredmények jelennek hello rögzíti.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Sávok automatikusan hello relatív hello numerikus oszlop értéke alapján hozzák létre.<br><br>Paranccsal hello rendezési rekordokban hello lekérdezés toosort hello hello listában.  hello felhasználói kattintva tekintse meg az összes toorun hello lekérdezéséhez és az összes rekordot ad vissza. |
| Graph elrejtése |Válassza ki a toodisable hello graph toohello jobb oldalán hello numerikus oszlopot. |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Szín |Hello sávok vagy értékgörbéket színét. |
| Név és érték elválasztó |Ha a kívánt tooparse hello text tulajdonságához több érték egyetlen karakter elválasztó.  Lásd: [általános beállítások](#name-value-separator) részleteiről. |
| Navigációs lekérdezés |Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Lásd: [általános beállítások](#navigation-query) részleteiről. |
| **Lista** |**> Oszlopfejlécek** |
| Név |Szöveg toodisplay hello hello listát első oszlopa hello tetején. |
| Érték |Szöveg toodisplay hello lista hello második oszlopában hello tetején. |
| **Lista** |**> Küszöbértékek** |
| Küszöbértékek engedélyezése |Válassza ki a tooenable küszöbértékeket.  Lásd: [általános beállítások](#thresholds) részleteiről. |

## <a name="two-numbers--list-part"></a>Két szám & a lista része
Fejléce rendelkezik két szám megjelenítő érkező saját naplófájlt keresési lekérdezések száma.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg.

![Két szám & a listanézet](media/log-analytics-view-designer/view-two-numbers-list.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello nézet hello tetején. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| Ikon használata |Válassza ki a toohave hello ikon megjelenítése. |
| **Cím** | |
| Jelmagyarázat |Szöveg toodisplay hello fejléc hello tetején. |
| Lekérdezés |Lekérdezés toorun hello fejléc.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| **Lista** | |
| Lekérdezés |Lekérdezés toorun hello listáját.  első két tulajdonságainak hello hello első tíz eredmények jelennek hello rögzíti.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Sávok automatikusan hello relatív hello numerikus oszlop értéke alapján hozzák létre.<br><br>Paranccsal hello rendezési rekordokban hello lekérdezés toosort hello hello listában.  hello felhasználói kattintva tekintse meg az összes toorun hello lekérdezéséhez és az összes rekordot ad vissza. |
| Graph elrejtése |Válassza ki a toodisable hello graph toohello jobb oldalán hello numerikus oszlopot. |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Szín |Hello sávok vagy értékgörbéket színét. |
| Művelet |A hello értékgörbe tooperform műveletet.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Név és érték elválasztó |Ha a kívánt tooparse hello text tulajdonságához több érték egyetlen karakter elválasztó.  Lásd: [általános beállítások](#name-value-separator) részleteiről. |
| Navigációs lekérdezés |Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Lásd: [általános beállítások](#navigation-query) részleteiről. |
| **Lista** |**> Oszlopfejlécek** |
| Név |Szöveg toodisplay hello hello listát első oszlopa hello tetején. |
| Érték |Szöveg toodisplay hello lista hello második oszlopában hello tetején. |
| **Lista** |**> Küszöbértékek** |
| Küszöbértékek engedélyezése |Válassza ki a tooenable küszöbértékeket.  Lásd: [általános beállítások](#thresholds) részleteiről. |

## <a name="donut--list-part"></a>Lista és fánk része
Fejléc egy egyetlen napló lekérdezésben érték oszlop összesített számát jeleníti meg.  hello fánk grafikusan hello felső három rekordok eredményeit jeleníti meg.

![Fánk & lista megtekintése](media/log-analytics-view-designer/view-donut-list.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello hello tetején csempére. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| Ikon használata |Válassza ki a toohave hello ikon megjelenítése. |
| **Fejléc** | |
| Cím |Szöveg toodisplay hello fejléc hello tetején. |
| Felirat |Szöveg toodisplay hello cím hello felső hello fejléc alatt. |
| **Fánk** | |
| Lekérdezés |Lekérdezés toorun hello fánk a.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie. |
| **Fánk** |**> Center** |
| Szöveg |Szöveg toodisplay alatt hello fánk hello értéket. |
| Művelet |hello művelet tooperform hello érték tulajdonság toosummarize tooa egyetlen értékét.<br><br>-Összeg: Hello értékek összes rekordok hozzáadásához.<br>-Százalékos: Hello értékek által visszaadott hello rekordok százaléka **center művelet során használt értékek eredménye** toohello összes rekord száma hello lekérdezésben. |
| Center művelet során használt eredményt értékek |Opcionálisan kattintson hello pluszjel tooadd egy vagy több értéket.  hello lekérdezési eredmények hello lesz korlátozott toorecords értékekkel hello tulajdonság.  Ha nincs érték ad hozzá, majd az összes rekord szerepelnek hello lekérdezés. |
| **További beállítások** |**> Színek** |
| Szín 1<br>Szín 2<br>Szín 3 |Válassza ki a hello minden hello fánk megjelenő hello értékek hello színét. |
| **További beállítások** |**> Speciális szín leképezése** |
| Mezőérték |A mező toodisplay hello nevét azt egy másik színét, ha hello fánk szerepel. |
| Szín |Válassza ki a hello egyedi mező hello színét. |
| **Lista** | |
| Lekérdezés |Lekérdezés toorun hello listáját.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| Graph elrejtése |Válassza ki a toodisable hello graph toohello jobb oldalán hello numerikus oszlopot. |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Szín |Hello sávok vagy értékgörbéket színét. |
| Művelet |A hello értékgörbe tooperform műveletet.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Név és érték elválasztó |Ha a kívánt tooparse hello text tulajdonságához több érték egyetlen karakter elválasztó.  Lásd: [általános beállítások](#name-value-separator) részleteiről. |
| Navigációs lekérdezés |Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Lásd: [általános beállítások](#navigation-query) részleteiről. |
| **Lista** |**> Oszlopfejlécek** |
| Név |Szöveg toodisplay hello hello listát első oszlopa hello tetején. |
| Érték |Szöveg toodisplay hello lista hello második oszlopában hello tetején. |
| **Lista** |**> Küszöbértékek** |
| Küszöbértékek engedélyezése |Válassza ki a tooenable küszöbértékeket.  Lásd: [általános beállítások](#thresholds) részleteiről. |

## <a name="two-timelines--list-part"></a>Két lista és ütemtervek része
Egy érték oszlopból napló lekérdezésben fejléc megjelenítése hello egy megjelenítése egyetlen számnak kihívás az oszlopdiagramok idővel két naplófájl-lekérdezések eredményének foglalja össze.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg.

![Két ütemtervek & lista megtekintése](media/log-analytics-view-designer/view-two-timelines-list.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello hello tetején csempére. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| Ikon használata |Válassza ki a toohave hello ikon megjelenítése. |
| **Először diagram<br>második diagram** | |
| Jelmagyarázat |Szöveg toodisplay hello kihívás hello első sorozata alapján. |
| Szín |Hello oszlopokhoz hello sorozat toouse színét. |
| Lekérdezés |Lekérdezés toorun hello első adatsorozathoz.  hello számát hello rekordok időbeli minden időközhöz hello diagram oszlop szerint fog megjelenni. |
| Művelet |hello művelet tooperform hello érték toosummarize tooa egyetlen tulajdonságértéket hello kihívás a.<br><br>– Összeg: Az összes rekord hello értéket összege.<br>-Átlagos: Az összes rekord hello értéket átlaga.<br>-Utolsó minta: Hello utolsó időköze hello diagram szereplő értéket.<br>-Első minta: Hello első intervallum hello diagram szereplő értéket.<br>-Számláló: Hello lekérdezés által visszaadott összes rekord száma. |
| **Lista** | |
| Lekérdezés |Lekérdezés toorun hello listáját.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| Graph elrejtése |Válassza ki a toodisable hello graph toohello jobb oldalán hello numerikus oszlopot. |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Szín |Hello sávok vagy értékgörbéket színét. |
| Művelet |A hello értékgörbe tooperform műveletet.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Navigációs lekérdezés |Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Lásd: [általános beállítások](#navigation-query) részleteiről. |
| **Lista** |**> Oszlopfejlécek** |
| Név |Szöveg toodisplay hello hello listát első oszlopa hello tetején. |
| Érték |Szöveg toodisplay hello lista hello második oszlopában hello tetején. |
| **Lista** |**> Küszöbértékek** |
| Küszöbértékek engedélyezése |Válassza ki a tooenable küszöbértékeket.  Lásd: [általános beállítások](#thresholds) részleteiről. |

## <a name="information-part"></a>Információkat
Fejléc statikus szöveget és egy nem kötelező hivatkozást jeleníti meg.  Lista egy vagy több elem statikus szöveget és címét jeleníti meg.

![Információk megtekintése](media/log-analytics-view-designer/view-information.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello hello tetején csempére. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Szín |Hello fejléc háttérszínét. |
| **Fejléc** | |
| Kép |Kép fájl toodisplay hello fejlécben. |
| Címke |Szöveg toodisplay hello fejlécben. |
| **Fejléc** |**> Hivatkozás** |
| Címke |Hivatkozás szövege. |
| URL-cím |Hivatkozás URL-címe. |
| **Elemek** | |
| Cím |Az egyes elemek cím toodisplay szöveg. |
| Tartalom |Szöveg toodisplay minden elemhez. |

## <a name="line-chart-callout--list-part"></a>Vonaldiagram, kihívás & lista része
Fejléc egy grafikonon napló lekérdezésből több adatsorozattal időt és az összesített érték egy kihívás keresztül.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg.

![Vonaldiagram, kihívás & listanézet](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello hello tetején csempére. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| Ikon használata |Válassza ki a toohave hello ikon megjelenítése. |
| **Fejléc** | |
| Cím |Szöveg toodisplay hello fejléc hello tetején. |
| Felirat |Szöveg toodisplay hello cím hello felső hello fejléc alatt. |
| **Vonaldiagram** | |
| Lekérdezés |Lekérdezés toorun hello sor a diagramon.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Ez általában az hello használó lekérdezések **mérték** kulcsszó toosummarize eredmények.  Ha hello lekérdezés használ hello **időköz** kulcsszót, majd hello hello diagram x tengelyéhez fogja használni az adott időszakban.  Ha hello lekérdezés nem tartalmazza a hello **időköz** kulcsszót, majd óránként intervallumok használt x tengely hello. |
| **Vonaldiagram** |**> Kihívás** |
| Kihívás cím |Szöveg toodisplay hello kihívás érték felett. |
| Az adatsorozat neve |Hello adatsorozat toouse hello kihívás értékhez tulajdonság értéke.  Adatsorozatok valósul meg, ha minden hello lekérdezésből rekordjai. |
| Művelet |hello művelet tooperform hello érték toosummarize tooa egyetlen tulajdonságértéket hello kihívás a.<br><br>-Átlagos: Az összes rekord hello értéket átlaga.<br>-Hello lekérdezés által visszaadott összes rekord száma száma.<br>-Utolsó minta: Hello utolsó időköze hello diagram szereplő értéket.<br>-Maximális: Hello diagram szereplő hello időközök maximális értéket.<br>-Min: Hello diagram szereplő hello időszakok közül a minimális érték.<br>– Összeg: Az összes rekord hello értéket összege. |
| **Vonaldiagram** |**> Y tengely** |
| A Logaritmikus skála használata |Válassza ki a toouse hello y tengely a Logaritmikus skála. |
| egység |Hello lekérdezés által visszaadott hello értékek hello egységeket adjon meg.  Ez az információ használt toodisplay címkék hello értéktípusok jelző hello diagram és opcionálisan hello értékek alakításának.  hello egységtípus hello egység hello kategória határozza meg, és elérhető aktuális egységtípus értékek hello határozza meg.  Ha kiválaszthat egy értéket a Convert toothen hello numerikus értékek hello aktuális egység típus toohello Convert tootype konvertálja. |
| Egyéni felirat |Szöveg toodisplay hello Y tengely következő toohello címke hello egység típushoz.  Ha nincs címke van megadva, akkor csak a hello egységtípus jelenik meg. |
| **Lista** | |
| Lekérdezés |Lekérdezés toorun hello listáját.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| Graph elrejtése |Válassza ki a toodisable hello graph toohello jobb oldalán hello numerikus oszlopot. |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Szín |Hello sávok vagy értékgörbéket színét. |
| Művelet |A hello értékgörbe tooperform műveletet.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Név és érték elválasztó |Ha a kívánt tooparse hello text tulajdonságához több érték egyetlen karakter elválasztó.  Lásd: [általános beállítások](#name-value-separator) részleteiről. |
| Navigációs lekérdezés |Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Lásd: [általános beállítások](#navigation-query) részleteiről. |
| **Lista** |**> Oszlopfejlécek** |
| Név |Szöveg toodisplay hello hello listát első oszlopa hello tetején. |
| Érték |Szöveg toodisplay hello lista hello második oszlopában hello tetején. |
| **Lista** |**> Küszöbértékek** |
| Küszöbértékek engedélyezése |Válassza ki a tooenable küszöbértékeket.  Lásd: [általános beállítások](#thresholds) részleteiről. |

## <a name="line-chart--list-part"></a>Sor lista és a diagram része
Fejléc egy grafikonon napló lekérdezésből több adatsorozattal adott idő alatt.  Lista hello felső tíz egy lekérdezés eredményeként előálló az egy numerikus oszlopot vagy adott idő alatt a módosítás relatív értékének hello jelző grafikon jeleníti meg.

![Sor lista és a diagram nézet](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello hello tetején csempére. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| Ikon használata |Válassza ki a toohave hello ikon megjelenítése. |
| **Fejléc** | |
| Cím |Szöveg toodisplay hello fejléc hello tetején. |
| Felirat |Szöveg toodisplay hello cím hello felső hello fejléc alatt. |
| **Vonaldiagram** | |
| Lekérdezés |Lekérdezés toorun hello sor a diagramon.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Ez általában az hello használó lekérdezések **mérték** kulcsszó toosummarize eredmények.  Ha hello lekérdezés használ hello **időköz** kulcsszót, majd hello hello diagram x tengelyéhez fogja használni az adott időszakban.  Ha hello lekérdezés nem tartalmazza a hello **időköz** kulcsszót, majd óránként intervallumok használt x tengely hello. |
| **Vonaldiagram** |**> Y tengely** |
| A Logaritmikus skála használata |Válassza ki a toouse hello y tengely a Logaritmikus skála. |
| egység |Hello lekérdezés által visszaadott hello értékek hello egységeket adjon meg.  Ez az információ használt toodisplay címkék hello értéktípusok jelző hello diagram és opcionálisan hello értékek alakításának.  hello egységtípus hello egység hello kategória határozza meg, és elérhető aktuális egységtípus értékek hello határozza meg.  Ha kiválaszthat egy értéket a Convert toothen hello numerikus értékek hello aktuális egység típus toohello Convert tootype konvertálja. |
| Egyéni felirat |Szöveg toodisplay hello Y tengely következő toohello címke hello egység típushoz.  Ha nincs címke van megadva, akkor csak a hello egységtípus jelenik meg. |
| **Lista** | |
| Lekérdezés |Lekérdezés toorun hello listáját.  hello lekérdezés által visszaadott rekordok számát hello hello száma jelenik meg. |
| Graph elrejtése |Válassza ki a toodisable hello graph toohello jobb oldalán hello numerikus oszlopot. |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Szín |Hello sávok vagy értékgörbéket színét. |
| Művelet |A hello értékgörbe tooperform műveletet.  Lásd: [általános beállítások](#sparklines) részleteiről. |
| Név és érték elválasztó |Ha a kívánt tooparse hello text tulajdonságához több érték egyetlen karakter elválasztó.  Lásd: [általános beállítások](#name-value-separator) részleteiről. |
| Navigációs lekérdezés |Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Lásd: [általános beállítások](#navigation-query) részleteiről. |
| **Lista** |**> Oszlopfejlécek** |
| Név |Szöveg toodisplay hello hello listát első oszlopa hello tetején. |
| Érték |Szöveg toodisplay hello lista hello második oszlopában hello tetején. |
| **Lista** |**> Küszöbértékek** |
| Küszöbértékek engedélyezése |Válassza ki a tooenable küszöbértékeket.  Lásd: [általános beállítások](#thresholds) részleteiről. |

## <a name="stack-of-line-charts-part"></a>Sor diagramok rész verem
Megjeleníti a napló lekérdezésből több sorozat három különálló vonaldiagramok adott idő alatt.

![Navigációs veremben vonaldiagramok](media/log-analytics-view-designer/view-stack-line-charts.png)

| Beállítás | Leírás |
|:--- |:--- |
| **Általános** | |
| Csoport cím |Szöveg toodisplay hello hello tetején csempére. |
| Új csoport |Válassza ki a toocreate egy új csoportot az hello aktuális nézet kezdődő hello nézetben. |
| Ikon |Kép fájl toodisplay következő toohello eredmény hello fejlécben. |
| **Diagram – 1<br>2 diagram<br>3 diagram** |**> Fejléc** |
| Cím |Szöveg toodisplay hello diagram hello tetején. |
| Felirat |Szöveg toodisplay hello hello diagram hello tetején cím alatt. |
| **Diagram – 1<br>2 diagram<br>3 diagram** |**Vonaldiagram** |
| Lekérdezés |Lekérdezés toorun hello sor a diagramon.  hello első tulajdonság szöveges érték, és hello második tulajdonság numerikus értéknek kell lennie.  Ez általában az hello használó lekérdezések **mérték** kulcsszó toosummarize eredmények.  Ha hello lekérdezés használ hello **időköz** kulcsszót, majd hello hello diagram x tengelyéhez fogja használni az adott időszakban.  Ha hello lekérdezés nem tartalmazza a hello **időköz** kulcsszót, majd óránként intervallumok használt x tengely hello. |
| **A diagram** |**> Y tengely** |
| A Logaritmikus skála használata |Válassza ki a toouse hello y tengely a Logaritmikus skála. |
| egység |Hello lekérdezés által visszaadott hello értékek hello egységeket adjon meg.  Ez az információ használt toodisplay címkék hello értéktípusok jelző hello diagram és opcionálisan hello értékek alakításának.  hello egységtípus hello egység hello kategória határozza meg, és elérhető aktuális egységtípus értékek hello határozza meg.  Ha kiválaszthat egy értéket a Convert toothen hello numerikus értékek hello aktuális egység típus toohello Convert tootype konvertálja. |
| Egyéni felirat |Szöveg toodisplay hello Y tengely következő toohello címke hello egység típushoz.  Ha nincs címke van megadva, akkor csak a hello egységtípus jelenik meg. |

## <a name="common-settings"></a>Általános beállítások
a következő szakaszok hello beállítások közös tooseveral képi megjelenítés részek ismertetik.

### <a name="name-value-separator">Név és érték elválasztó</a>
Egyetlen határolójel Ha tooparse hello text tulajdonságához lista lekérdezésből a kívánt több érték.  Ha megad egy elválasztó, biztosíthat nevét az egyes mezők elválasztott hello azonos elválasztó hello név mezőben.

Vegyük példaként a tulajdonságot, *hely* például része, amely értékek *Redmond-összeállító 41* és *Bellevue-Building12*.  Megadhatja – hello név és érték elválasztó és *város-összeállító* a hello nevét.  Ez lenne az egyes értékek értelmezhető nevű két tulajdonságok *Város* és *épület*.

### <a name="navigation-query">Navigációs lekérdezés</a>
Toorun lekérdezik hello felhasználó kiválaszt egy elemet a hello listában.  Használjon *{kijelölt elem}* tooinclude hello szintaxis elem, amely a kiválasztott felhasználóhoz hello.

Például akkor, ha hello lekérdezés tartalmaz egy nevű oszlopot *számítógép* és hello navigációs lekérdezés *{kijelölt elem}*, többek között a lekérdezés *számítógép = "Sajátgép"* akkor futtatható, ha hello felhasználó kiválasztott számítógép.  Ha hello navigációs lekérdezés *típus = {kijelölt elem} esemény* majd hello lekérdezés *típus = esemény számítógép = "Sajátgép"* lesz futtatható.

### <a name="sparklines">Értékgörbéket</a>
Értékgörbe kis vonaldiagramról, amely bemutatja a lista bejegyzés hello értékének adott idő alatt.  A képi megjelenítés részei a listájával kiválaszthatja, hogy toodisplay sáv jelző vízszintes hello egy numerikus oszlopot a relatív értéket vagy jelző értéke időbeli értékgörbe.

a következő táblázat hello értékgörbéket hello beállításait ismerteti.

| Beállítás | Leírás |
|:--- |:--- |
| Értékgörbéket engedélyezése |Válassza ki a toodisplay értékgörbe helyett sávot. |
| Művelet |Ha értékgörbéket engedélyezve vannak, akkor hello művelet tooperform a hello toocalculate hello listaértékek hello értékgörbe az egyes tulajdonságai.<br><br>-Utolsó minta: Utolsó érték hello adatsorozathoz hello időtartam alatt.<br>-Maximális: Maximális érték hello adatsorozathoz hello időtartam alatt.<br>-Min: Minimális érték hello adatsorozathoz hello időtartam alatt.<br>-Összeg: Sum hello adatsorok hello időtartam alatt.<br>-Összegzés: Használ hello hello lekérdezés hello fejlécben azonos mérték parancsot. |

### <a name="thresholds">Küszöbértékek</a>
Küszöbértékek lehetővé teszik a színes ikon következő tooeach elem toodisplay felkínálva egy gyors visual jelzi, hogy lehet egy adott értéket, vagy egy adott tartományba esnek elemek listájában.  Például egy elfogadható érték, a hello értéke egy tartományba, amely figyelmeztet, ha sárga, és a piros elemek zöld ikonnal lehetett megjeleníteni, ha az érték meghaladja a hibaérték.

Ha engedélyezi a küszöbértékeket egy részére, meg kell adnia egy vagy több küszöbértékeket.  Ha hello elem értéke nagyobb, mint a küszöbérték és kisebb hello következő küszöbértéknél, szín szolgál.  Ha hello elem nagyobb, mint majd legmagasabb küszöbértéket, szín van beállítva.   

Minden egyes küszöbérték beállítása rendelkezik egy küszöbértéket értékkel rendelkező **alapértelmezett**.  Ez a hello színét, ha nincs más értékek számát.  Adja hozzá, vagy távolítsa el a küszöbértékek hello kattintva  **+**  vagy **x** gombra.

hello a következő táblázat bemutatja a küszöbértékek hello beállításait.

| Beállítás | Leírás |
|:--- |:--- |
| Küszöbértékek engedélyezése |Válassza ki a szín ikon toohello balra minden egyes érték, amely jelzi, az egészségügyi relatív toospecified küszöbértékek toodisplay. |
| Név |Név tooidentify hello küszöbértéket. |
| Küszöbérték |Hello küszöb értékét.  hello állapotfigyelő mindegyik listaelemhez beállítás hello legmagasabb küszöbérték; hello elem értéke túllépte toohello színét.  Van egy alapértelmezett küszöböt, akkor hello színét, ha nincs küszöbértéket. |
| Szín |Hello küszöbérték színét. |

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toosupport hello lekérdezések a képi megjelenítés részeit.
