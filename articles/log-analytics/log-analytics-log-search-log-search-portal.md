---
title: "aaaUsing hello napló keresése portálra az Azure Naplóelemzés |} Microsoft Docs"
description: "A cikk leírja, hogyan toocreate keresések naplózása és elemzése a Naplóelemzési munkaterület hello napló keresése portálon tárolt adatok webalkalmazással tartalmaz.  hello oktatóanyag egyszerű lekérdezések futtatásának tooreturn különféle típusú adatokat, és az eredmények elemzése tartalmazza."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: 2e6633d548bb508edc0c650d11d2c32fc6ee536c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-log-searches-in-azure-log-analytics-using-hello-log-search-portal"></a>Napló keresések létrehozása az Azure Naplóelemzés hello napló keresése portál használatával

> [!NOTE]
> Ez a cikk ismerteti a hello napló keresése portálra az Azure Naplóelemzés hello új lekérdezési nyelv használatával.  További tudnivalók hello új nyelv, és hello eljárás tooupgrade lekérése a munkaterületen a [az Azure Naplóelemzés munkaterület toonew napló keresés frissítése](log-analytics-log-search-upgrade.md).  
>
> Ha a munkaterületet még nem frissített toohello új lekérdező nyelv, tekintse át túl[található adatokat, és napló kereséseket a Naplóelemzési](log-analytics-log-searches.md) hello napló keresése portál jelenlegi verziója hello olvashat.

A cikk leírja, hogyan toocreate keresések naplózása és elemzése a Naplóelemzési munkaterület hello napló keresése portálon tárolt adatok webalkalmazással tartalmaz.  hello oktatóanyag egyszerű lekérdezések futtatásának tooreturn különféle típusú adatokat, és az eredmények elemzése tartalmazza.  Hello napló keresése portálon hello lekérdezés módosításával, nem pedig közvetlenül módosítsák a szolgáltatásokra összpontosít.  További részletek a közvetlenül a hello lekérdezés szerkesztése: hello [lekérdezés nyelvi referencia](https://go.microsoft.com/fwlink/?linkid=856079).

toocreate keresések hello Advanced Analytics portál helyett hello napló keresése portal, lásd: [hello Analytics portál első lépések](https://go.microsoft.com/fwlink/?linkid=856587).  Mindkét portálok hello használata ugyanabban a lekérdezésben nyelvi tooaccess hello hello Naplóelemzési munkaterület ugyanazokat az adatokat.

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag feltételezi, hogy már rendelkezik a Naplóelemzési munkaterület legalább egy csatlakoztatott forrás, amely a hello lekérdezések tooanalyze adatokat állít elő.  

- Ha a munkaterületet nincs, létrehozhat egy ingyenes egy hello az eljárást használja [Ismerkedjen meg a Naplóelemzési munkaterület](log-analytics-get-started.md).
- Csatlakozás legalább egy [Windows-ügynök](log-analytics-windows-agents.md) vagy egy [Linux-ügynök](log-analytics-linux-agents.md) toohello munkaterületen.  

## <a name="open-hello-log-search-portal"></a>Nyissa meg hello napló keresése portál
Első lépésként hello napló keresése portál megnyitásával.  Hello Azure-portálon vagy az OMS-portálon hello hozzá tud férni.

1. Nyissa meg hello Azure-portálon.
2. Nyissa meg a tooLog Analytics, és válassza ki a munkaterületen.
3. Válasszon ki **naplófájl-keresési** az Azure portál vagy indítási hello OMS-portálon kiválasztásával hello toostay **OMS-portálon** , majd kattintson a hello napló keresése gombra.

![Napló Keresés gomb](media/log-analytics-log-search-log-search-portal/log-search-button.png)

## <a name="create-a-simple-search"></a>Hozzon létre egy egyszerű keresés
hello leggyorsabb módon tooretrieve az egyes adatok toowork egy egyszerű lekérdezést, amely visszaadja az összes rekord táblában.  Ha a Windows vagy Linux rendszerű ügyfelek csatlakoztatott tooyour munkaterület, ezzel meglesz az adatokat, vagy hello esemény (Windows) vagy a Syslog (Linux) tábla.

Írja be a következő lekérdezések hello keresőmezőbe egy hello és hello Keresés gombra.  

```
Event
```
```
Syslog
```

Adat hello alapértelmezett listanézet, és láthatja, hogy hány teljes rekord lett visszaadva.

![Egyszerű lekérdezés](media/log-analytics-log-search-log-search-portal/log-search-portal-01.png)

Csak hello első néhány tulajdonságok mindegyik rekorddal jelennek meg.  Kattintson a **megjelenítése további** toodisplay az egy adott bejegyzés minden tulajdonságát.

![Rekord részletei](media/log-analytics-log-search-log-search-portal/log-search-portal-02.png)

## <a name="set-hello-time-scope"></a>Hello idő hatókör megadása
Minden Naplóelemzési által gyűjtött rekord tartalmazza-e egy **TimeGenerated** hello bejegyzéshez hello dátumot és időpontot tartalmazó tulajdonság lett létrehozva.  Hello napló keresése portálon lekérdezés csak rekordokat adja vissza egy **TimeGenerated** bal oldalán található üdvözlő képernyőt hello megjelenő hello idő hatókörén belül.  

Hello időszűrője hello legördülő kiválasztásával vagy hello csúszkát módosításával módosíthatja.  hello csúszkát sáv diagramját, amelyek hello relatív hello tartományon belüli idő szegmensekhez rekordok számát jeleníti meg.  Ebbe a szegmensbe hello tartománytól függően változnak.

hello alapértelmezett idő hatókör **1 nap**.  Módosítsa ezt az értéket túl**7 nap**, és növelje a hello rekordok teljes száma.

![Dátum idő hatókör](media/log-analytics-log-search-log-search-portal/log-search-portal-03.png)

## <a name="filter-results-of-hello-query"></a>Hello lekérdezés eredményeit szűrni
Hello hello képernyő bal oldalán található hello keresőablak, amely lehetővé teszi tooadd szűrési toohello lekérdezést közvetlenül a módosítása nélkül.  Az a rekordok száma az első tíz értékükön visszaadott hello rekordok több tulajdonságainak jelennek meg.

Ha dolgozunk **esemény**, válassza ki hello jelölőnégyzet mellett túl**hiba** alatt **EVENTLEVELNAME**.   Ha dolgozunk **Syslog**, válassza ki hello jelölőnégyzet mellett túl**err** alatt **súlyossági szint**.  A következő toolimit hello hello tooone eredmények tooerror események hello lekérdezés értékre változik.

```
Event | where (EventLevelName == "Error")
```
```
Syslog | where (SeverityLevel == "err")
```

![Szűrés](media/log-analytics-log-search-log-search-portal/log-search-portal-04.png)

Hozzáadás tulajdonságok toohello keresőablak kiválasztásával **toofilters hozzáadása** hello rekordok egyik hello tulajdonság menüből.

![Adja hozzá a toofilter menü](media/log-analytics-log-search-log-search-portal/log-search-portal-02a.png)

Beállíthatja, hogy azonos szűrése kiválasztásával hello **szűrő** hello tulajdonság menüből hello érték egy rekord toofilter kívánt.  

Csak akkor kell hello **szűrő** kék a nevükkel tulajdonságok beállítása.  Ezek a *kereshető* mezők, amely az indexelt keresési feltételeket.  A szürke színnel mezők *szabad kereshető szöveg* mezők, amelyek csak hello **hivatkozások megjelenítése** lehetőséget.  Ez a beállítás azt jelzi, hogy az egyik tulajdonságnak sem lehet ezt az értéket adja vissza.

![Szűrő menü](media/log-analytics-log-search-log-search-portal/log-search-portal-01a.png)

Hello eredmények egyetlen tulajdonság alapján csoportosíthatja hello kiválasztásával **szerint kell csoportosítani a** beállítást hello rekord menüből.  Ez hozzáadja egy [összefoglalója](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) operátor tooyour lekérdezés, amely a diagram hello eredményeit jeleníti meg.  Egynél több tulajdonság a csoportosíthatja, de tooedit hello lekérdezés közvetlenül kell.  Jelölje be hello rekord menü következő hello hello **számítógép** tulajdonság, és válassza **"Számítógép" csoport**.  

![Számítógép szerint kell csoportosítani](media/log-analytics-log-search-log-search-portal/log-search-portal-10.png)

## <a name="work-with-results"></a>Eredmények használata
hello napló keresése portal számos a lekérdezés eredményeinek hello való munkához funkcióval rendelkezik.  Rendezheti, és a csoport eredmények tooanalyze hello adatok hello tényleges lekérdezés módosítása nélkül.  A lekérdezés nem alapértelmezés szerint vannak rendezve.

tooview hello adatok táblázatos formában, amely további lehetőségeket nyújt a szűrési és rendezési, kattintson a **tábla**.  

![Tábla megtekintése](media/log-analytics-log-search-log-search-portal/log-search-portal-05.png)

Hello nyílra által egy rekord tooview hello részletek rekord.

![Rendezés eredmények](media/log-analytics-log-search-log-search-portal/log-search-portal-06.png)

Az oszlop fejlécére kattintva a mező rendezése.

![Rendezés eredmények](media/log-analytics-log-search-log-search-portal/log-search-portal-07.png)

Szűrő hello eredmények a hello a szűrő gombra kattint, és egy szűrési feltételt megadásával hello oszlop megadott értéket.

![Szűrés eredménye](media/log-analytics-log-search-log-search-portal/log-search-portal-08.png)

Egy oszlop csoportosítás az oszlop fejlécének toohello felső hello eredmények húzásával.  Több oszlop toohello felső húzásával csoportosíthatja több mező.

![Az eredmények csoportosításához](media/log-analytics-log-search-log-search-portal/log-search-portal-09.png)



## <a name="work-with-performance-data"></a>Teljesítményadatok használata
Windows- és Linux-ügynökök teljesítményadatait tárolódik hello Naplóelemzési munkaterület hello **telj** tábla.  Teljesítmény rekordok csakúgy, mint bármilyen más rekordéval. Keresse meg, és azt írhat, amely visszaadja az összes teljesítmény rekord ugyanúgy, mint az események egyszerű lekérdezést.

```
Perf
```

![Teljesítményadatok](media/log-analytics-log-search-log-search-portal/log-search-portal-11.png)

Ad vissza, ha több millió rekordot a teljesítményobjektumok és a teljesítményszámlálók nem nagyon hasznosak.  Hello napló keresőmezőbe közvetlenül a fenti toofilter hello adatok vagy a használt, csak akkor írja be a hello következő ugyanazokat a módszereket lekérdezése hello is használhatja.  Ez adja vissza csak a processzor kihasználtsága rögzíti a Windows és Linux számítógépek.

```
Perf | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time")
```

![Processzorhasználat](media/log-analytics-log-search-log-search-portal/log-search-portal-12.png)

Ez korlátozza hello adatok tooa számláló, de az még nem elhelyezi egy űrlapon, amelyek különösen hasznosak.  Hello adatok megjelenítése egy vonaldiagramot, de először a toogroup kell azt a számítógépet és TimeGenerated.  több mező toogroup, toomodify hello lekérdezés szüksége, így módosítható közvetlenül hello lekérdezés toohello következő.  Ez használ hello [avg](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/avg()) hello funkció **ellenértéknek** toocalculate hello átlagos tulajdonságérték keresztül minden órában.

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated
```

![Adatok teljesítménydiagramban](media/log-analytics-log-search-log-search-portal/log-search-portal-13.png)

Most, hogy hello adatok megfelelően vannak csoportosítva, meg lehet jeleníteni visual diagram hello hozzáadásával [leképezési](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator) operátor.  

```
Perf  | where (ObjectName == "Processor")  | where (CounterName == "% Processor Time") | summarize avg(CounterValue) by Computer, TimeGenerated | render timechart
```

![Vonaldiagram](media/log-analytics-log-search-log-search-portal/log-search-portal-14.png)

## <a name="next-steps"></a>Következő lépések

- További tudnivalók hello Log Analytics lekérdezési nyelv [hello Analytics portál első lépések](https://go.microsoft.com/fwlink/?linkid=856079).
- Végezze el a oktatóanyagot hello használatával [Advanced Analytics portál](https://go.microsoft.com/fwlink/?linkid=856587) toorun hello lehetővé teszi azonos lekérdezések és hozzáférési hello hello napló keresése portál ugyanazokat az adatokat.
