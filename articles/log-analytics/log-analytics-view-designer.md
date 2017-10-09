---
title: "aaaCreate tooanalyze adatok megtekinti az OMS szolgáltatáshoz |} Microsoft Docs"
description: "A Naplóelemzési Nézettervező lehetővé teszi a toocreate egyéni nézeteket, és tartalmazhat különböző képi adatok hello OMS-tárházban hello OMS és az Azure portálon jelenik meg. Ez a cikk adatforrásnézet-tervezőből és létrehozásának és szerkesztésének egyéni nézetek eljárások áttekintését tartalmazza."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40b4bfef50d70e4479b6cae16abfa8ec33d1a2f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-view-designer-toocreate-custom-views-in-log-analytics"></a>A Naplóelemzési adatforrásnézet-tervezőből toocreate egyéni nézeteket használ
hello adatforrásnézet-tervezőből a [Naplóelemzési](log-analytics-overview.md) toocreate egyéni nézetek hello OMS hello OMS-tárházban található adatok különböző képi tartalmazó lehetővé teszi. Ez a cikk adatforrásnézet-tervezőből és létrehozásának és szerkesztésének egyéni nézetek eljárások áttekintését tartalmazza.

Az adatforrásnézet-tervezőből elérhető további cikkeit a következők:

* [Hivatkozás csempe](log-analytics-view-designer-tiles.md) -hivatkozás hello-beállítások az egyes hello tartalmazó csempék éppen úgy az egyéni nézetekben elérhető toouse.
* [A képi megjelenítés része hivatkozás](log-analytics-view-designer-parts.md) -hivatkozás hello-beállítások az egyes hello tartalmazó csempék éppen úgy az egyéni nézetekben elérhető toouse.

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor minden nézetben lekérdezések úgy kell megírni, a hello [új lekérdezési nyelv](https://go.microsoft.com/fwlink/?linkid=856078).  Bármely hello munkaterület verziófrissítése előtt létrehozott nézetek lesz konvertálva automtically.

## <a name="concepts"></a>Alapelvek
Az adatforrásnézet-tervezőből hello létrehozott nézetek hello elemek a következő táblázat hello tartalmaz.

| Rész | Leírás |
|:--- |:--- |
| Mozaik elrendezés |Megjelenik a hello fő napló Analytics áttekintő irányítópulthoz.  Tartalmazza a hello lévő hello információt visual összefoglalójához egyéni nézetet.  Különböző csempe adjon meg másik képi megjelenítések hello OMS-tárház a rekordok.  Kattintson a hello csempe tooopen hello egyéni nézet. |
| Egyéni nézet |Hello kattintva jeleníthető meg hello csempe.  Egy vagy több képi megjelenítés részeket tartalmazza. |
| A képi megjelenítés részei |Egy vagy több alapuló képi megjelenítés adatainak hello OMS-tárházban [keresések jelentkezzen](log-analytics-log-searches.md).  A legtöbb részei magas szintű képi megjelenítés biztosító fejléc és a legfelső szintű eredmények hello listáját tartalmazza.  Másik része típusok különböző képi hello OMS-tárház a rekordok adja meg.  Kattintson a hello rész tooperform elemeinek biztosít a részletes rekordok napló keresést. |

![Tervező áttekintése](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-tooyour-workspace"></a>Adja hozzá az adatforrásnézet-tervezőből tooyour munkaterület
Míg adatforrásnézet-tervezőből Preview, hozzá kell adnia az tooyour munkaterület kiválasztásával **előzetes verziójú funkciók** a hello **beállítások** hello OMS-portálon szakasza.

![Minta engedélyezése](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Létrehozásának és szerkesztésének nézetek
### <a name="create-a-new-view"></a>Új nézet létrehozása
Nyissa meg egy új nézet hello **adatforrásnézet-tervezőből** hello fő OMS irányítópulton csempére kattintva hello adatforrásnézet-tervezőből.

![Tervező csempéje](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Egy meglévő nézet szerkesztése
Az adatforrásnézet-tervezőből, a fő hello OMS-irányítópulton a csempére kattintva nyissa meg hello nézet hello meglévő nézet tooedit.  Kattintson a hello **szerkesztése** gombra kattint, az adatforrásnézet-tervezőből hello tooopen hello nézetében.

![Nézet szerkesztése](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Klónozza a meglévő nézet
Klónozni egy nézet, ha létrehoz egy új nézetet, és megnyitja azt a hello adatforrásnézet-tervezőből.  Új nézet hello azonos nevet, amint a "Másolás" hozzáfűzött toohello végén az eredeti hello hello fog rendelkezni.  tooclone nézet, nyissa meg a hello fő OMS-irányítópulton a csempére kattintva meglévő nézet hello.  Kattintson a hello **Klónozás** gombra kattint, az adatforrásnézet-tervezőből hello tooopen hello nézetében.

![Egy nézet klónozása](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Meglévő nézet törlése
toodelete meglévő nézet, a fő hello OMS-irányítópulton a csempére kattintva nyissa meg hello nézet.  Kattintson a hello **szerkesztése** tooopen hello nézet hello adatforrásnézet-tervezőből gombra, és kattintson a **nézet törlése**.

![Nézet törlése](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Egy meglévő nézethez exportálása
Egy nézet tooa JSON-fájl, amely egy másik munkaterület importálja, illetve használja exportálhatja egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-authoring-templates.md).  tooexport meglévő nézet, a fő hello OMS-irányítópulton a csempére kattintva nyissa meg hello nézet.  Kattintson a hello **exportálása** gomb toocreate hello böngésző letöltési mappában található a fájl.  hello hello fájl lesz hello nézet hello kiterjesztésű hello neve *omsview*.

![Nézet exportálása](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Egy meglévő nézethez importálása
Importálhatja egy *omsview* exportált egy másik felügyeleti csoportból.  egy meglévő nézethez tooimport először létre kell hoznia egy új nézetet.  Kattintson a hello **importálási** gombra, jelölje be hello *omsview* fájlt.  hello konfigurációs hello fájlban hello meglévő nézet másolódnak.

![Nézet exportálása](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Az adatforrásnézet-tervezőből használata
hello adatforrásnézet-tervezőből három ablaktáblák rendelkezik.  Hello **tervezési** ablaktábla hello egyéni nézet jelöli.  Hozzáadásakor csempék és részei a hello **vezérlő** ablaktábla toohello **tervezési** ablaktáblában, vannak hozzáadva toohello nézet.  Hello **tulajdonságok** ablaktábla hello csempe vagy kijelölt hello tulajdonságait.

![Nézettervező](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Konfigurálja a csempéje
Egyéni nézet csak egyetlen csempe rendelkezhet.  Jelölje be hello **csempe** hello lapján **vezérlő** ablaktábla tooview hello aktuális csempére, vagy válasszon másik kódot.  Hello **tulajdonságok** ablaktábla hello aktuális csempe hello tulajdonságait.  Hello csempe tulajdonságainak konfigurálása toohello megfelelően részletes információkat a hello [csempe hivatkozás](log-analytics-view-designer-tiles.md) kattintson **alkalmaz** toosave módosításokat.

### <a name="configure-visualization-parts"></a>A képi megjelenítés részek konfigurálása
A nézet a képi megjelenítés részek tetszőleges számú tartalmazhatnak.  Jelölje be hello **nézet** fülre, majd a képi megjelenítés része tooadd toohello nézet.  Hello **tulajdonságok** ablaktábla a kijelölt hello rész hello tulajdonságok.  Hello nézet konfigurálása tulajdonságok szerint toohello részletes információkat a hello [képi megjelenítés része hivatkozás](log-analytics-view-designer-parts.md) kattintson **alkalmaz** toosave módosításokat.

### <a name="delete-a-visualization-part"></a>Törölje a képi megjelenítés részt
Eltávolíthatja a képi megjelenítés része hello nézet hello kattintva **X** hello jobb felső sarkában hello rész gombjára.

### <a name="rearrange-visualization-parts"></a>A képi megjelenítés részek átrendezése
Nézetek csak a képi megjelenítés részek több sorban is rendelkezik.  Átrendezését meglévő alkatrész nézetben kattintással és húzással őket tooa új helyre.

## <a name="next-steps"></a>Következő lépések
* Adja hozzá [Csempék](log-analytics-view-designer-tiles.md) tooyour egyéni nézetet.
* Adja hozzá [képi megjelenítés részek](log-analytics-view-designer-parts.md) tooyour egyéni nézetet.
