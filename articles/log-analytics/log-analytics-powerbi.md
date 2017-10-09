---
title: "aaaExport Naplóelemzési adatok tooPower BI |} Microsoft Docs"
description: "A Power BI-alapú üzleti analytics felhőszolgáltatás, amely gazdag képi megjelenítések és jelentéseket biztosít a különböző adatok elemzése a Microsoft.  A Naplóelemzési is folyamatosan adatok exportálása adattárból hello OMS Power BI-ba így kihasználhatja a képi megjelenítések és elemzésére szolgáló eszközöket.  Ez a cikk ismerteti, hogyan tooconfigure a Naplóelemzési, amely automatikusan exportálja a tooPower BI rendszeres időközönként lekérdezi."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 83edc411-6886-4de1-aadd-33982147b9c3
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: bwren
ms.openlocfilehash: 4822f99677e5d1080c72e95cda410da81615bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="export-log-analytics-data-toopower-bi"></a>A Naplóelemzési adatok tooPower BI exportálása

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor a Log Analyticshez adatok tooPower BI exportálási folyamat nem fog többé működni.  A frissítés előtt létrehozott meglévő ütemezések a program letiltja. 
>
> Frissítés után használja az Azure Naplóelemzés hello ugyanahhoz a platformhoz, az Application Insights, és használja ugyanezt a folyamatot tooexport Naplóelemzési lekérdezések tooPower BI mint hello [hello folyamat tooexport Application Insights lekérdezi tooPower BI](../application-insights/app-insights-export-power-bi.md#export-analytics-queries).  Vagy exportálhatja hello lekérdezés hello Analytics konzol használatával, ha a cikkben leírtak szerint, vagy választhat hello **Power BI** üdvözlő képernyőt hello napló keresése portálon hello tetején gombra.



[A Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) , amely gazdag képi megjelenítések és jelentéseket biztosít a különböző adatok elemzése a Microsoft felhőalapú üzleti analytics szolgáltatás.  A Naplóelemzési is automatikusan adatok exportálása adattárból hello OMS Power BI-ba így kihasználhatja a képi megjelenítések és elemzésére szolgáló eszközöket.

A Power BI Log Analyticshez való konfigurálásakor, a Power bi-ban, az eredmények toocorresponding adatkészletek exportálása napló lekérdezéseket hozhat létre.  hello lekérdezés és exportálási továbbra is az Ön által meghatározott tookeep hello dataset mentése toodate hello legújabb Naplóelemzési gyűjtött adatok ütemezhető tooautomatically.

![Napló Analytics tooPower BI](media/log-analytics-powerbi/overview.png)

## <a name="power-bi-schedules"></a>A Power BI ütemezése
A *Power BI ütemezés* adatok exportálhatók adatkészletből hello OMS tárház tooa megfelelő a Power bi-ban, és egy ütemezést, amely meghatározza a keresés milyen gyakran fut tookeep hello adatkészlet aktuális napló keresés tartalmazza.

hello mezők hello adatkészlet hello napló keresés által visszaadott hello rekordok hello tulajdonságainak fog egyezni.  Ha hello keresési különböző típusú rekordot ad vissza, hello adatkészlet összes tartalmazza, majd az egyes hello hello tulajdonságok tartalmazza rekordtípusokhoz.  

> [!NOTE]
> A bevált gyakorlat toouse napló keresési lekérdezés, amely a nyers adatokat adja vissza megakadályozását tooperforming parancsokkal, mint bármely összevonási [mérték](log-analytics-search-reference.md#measure).  Is elvégezheti bármely összevonásának és a számítások a Power BI hello nyers adatok.
>
>

## <a name="connecting-oms-workspace-toopower-bi"></a>Kapcsolódás az OMS-munkaterület tooPower BI
A Naplóelemzési tooPower BI exportálása, előtt mindenképpen kapcsolódnia kell a OMS munkaterület tooyour Power BI-fiókjába hello a következő eljárás használatával.  

1. Hello OMS-konzolon kattintson a hello **beállítások** csempére.
2. Válassza ki **fiókok**.
3. A hello **munkaterület információk** szakaszban kattintson **tooPower BI-fiók csatlakozás**.
4. Adja meg a Power BI-fiókjába hello hitelesítő adatait.

## <a name="create-a-power-bi-schedule"></a>A Power BI ütemezés létrehozása
Minden adatkészlet hello a következő eljárás használatával a Power BI ütemezést létrehozni.

1. Hello OMS-konzolon kattintson a hello **naplófájl-keresési** csempére.
2. Adja meg egy új lekérdezést, vagy válasszon ki egy mentett keresést, hogy a értéket ad vissza, amelyet tooexport túl adatok hello**Power BI**.  
3. Hello kattintson **Power BI** hello lap tooopen hello hello tetején gomb **Power BI** párbeszédpanel.
4. Adja meg a következő táblázatban, majd kattintson a hello hello információkat **mentése**.

| Tulajdonság | Leírás |
|:--- |:--- |
| Név |Név tooidentify hello ütemezés hello listája a Power BI ütemezések megtekintésekor. |
| Mentett keresés |hello napló keresése toorun.  Válassza ki az aktuális lekérdezés hello, vagy hello legördülő listából válassza ki a korábban mentett keresési. |
| Ütemezés |Milyen gyakran toorun hello mentett keresés és toohello Power BI dataset exportálása.  hello érték 15 perc és 24 óra között kell lennie. |
| A DataSet neve |a Power BI hello dataset hello neve.  Rendszer hozható létre, ha még nem létezik és frissítése, ha már létezik. |

## <a name="viewing-and-removing-power-bi-schedules"></a>A Power BI ütemezések megtekintése és eltávolítása
Hello listájának megtekintése a Power BI meglévő ütemezések közül az eljárást követő hello.

1. Hello OMS-konzolon kattintson a hello **beállítások** csempére.
2. Válassza ki **a Power BI**.

Továbbá hello toohello részleteit ütemezése, hello ennyiszer ütemezés hello hello a múlt hét, ezért nem hello legutóbbi szinkronizálás állapotának hello jelennek meg.  Ha hello szinkronizálási hibát észlelt, kattintson a hello hivatkozás toorun bejegyzések hello hiba részleteit a napló keresése.

Ütemezés eltávolításához kattintson a hello **X** a hello **eltávolítása oszlop**.  Ütemezés szerint letilthatja kiválasztásával **ki**.  toomodify ütemezés szerint távolítsa el azt, majd hozza létre újra az új beállítások hello.

![A Power BI ütemezése](media/log-analytics-powerbi/schedules.png)

## <a name="sample-walkthrough"></a>A minta forgatókönyv
hello következő szakasz végigvezeti a Power BI ütemezett létrehozása és használata a dataset toocreate például egy egyszerű jelentést.  Ebben a példában állítja be a számítógépek minden teljesítményadat exportált tooPower BI, majd egy vonaldiagramot toodisplay processzorhasználat jön létre.

### <a name="create-log-search"></a>Naplófájl-keresési létrehozása
Először hozzon létre egy napló keressen rá, hogy szeretnénk toosend toohello dataset hello adatokat.  A jelen példában használjuk a számítógépek névvel rendelkező összes teljesítményadatokat visszaadó *srv*.  

![A Power BI ütemezése](media/log-analytics-powerbi/walkthrough-query.png)

### <a name="create-power-bi-search"></a>A Power BI keresési létrehozása
Hello elemre kattint **Power BI** tooopen hello Power BI párbeszédpanel gombra, és adja meg a hello szükséges információkat.  Azt szeretné, hogy a keresés toorun óránként egyszer, és hozzon létre egy nevű adatkészlet *Contoso telj*.  Mivel azt már hello keresési megnyitott, amely azt szeretnénk, ha hello adatokat hoz létre, hogy tartsa meg hello alapértelmezett, *használata aktuális keresési lekérdezés* a **mentett keresési**.

![A Power BI-keresés](media/log-analytics-powerbi/walkthrough-schedule.png)

### <a name="verify-power-bi-search"></a>Ellenőrizze a Power BI-keresés
hello ütemezés megfelelően létrehozott tooverify, azt a hello hello Power BI keresések lista megtekintése **beállítások** hello OMS irányítópult csempére.  A Microsoft Várjon egy pár percet, és frissítse a nézetet, amíg a rendszer jelzi, hogy hello történt szinkronizálás.

![A Power BI-keresés](media/log-analytics-powerbi/walkthrough-schedules.png)

### <a name="verify-hello-dataset-in-power-bi"></a>A Power BI hello dataset ellenőrzése
Azt a fiók be tudjon jelentkezni [powerbi.microsoft.com](http://powerbi.microsoft.com/) túl görgessen**adatkészletek** hello aljához hello bal oldali ablaktáblán.  Láthatja, hogy hello *Contoso telj* dataset szerepel, amely azt jelzi, hogy az exportálás sikeresen lefutott.

![A Power BI-adatkészlet](media/log-analytics-powerbi/walkthrough-datasets.png)

### <a name="create-report-based-on-dataset"></a>Az adatkészlet-alapú jelentés létrehozása
Hello válassza ki, hogy **Contoso telj** adatkészlet majd kattintson a **eredmények** a hello **mezők** hello jobb tooview hello mezőket az adatkészlethez tartozó ablaktábláján.  toocreate egy sor graph ábrázoló processzorhasználat számítógépenként, az hello a következő műveleteket végezzük.

1. Válassza ki a hello sor diagram képi megjelenítés.
2. A csomóponthúzási **ObjectName** túl**jelentés szintű szűrő** , és ellenőrizze, **processzor**.
3. A csomóponthúzási **CounterName** túl**jelentés szintű szűrő** , és ellenőrizze, **processzoridő %**.
4. A csomóponthúzási **ellenértéknek** túl**értékek**.
5. A csomóponthúzási **számítógép** túl**jelmagyarázat**.
6. A csomóponthúzási **TimeGenerated** túl**tengely**.

Láthatja, hogy hello eredményül kapott vonaldiagram jelenik meg, amely az adathalmaz hello adatait.

![A Power BI vonaldiagram](media/log-analytics-powerbi/walkthrough-linegraph.png)

### <a name="save-hello-report"></a>Hello jelentés mentése
A Microsoft hello jelentés hello kattintva mentse mentés gombja üdvözlő képernyőt hello tetején, és ellenőrizze, hogy most már szerepel hello jelentések szakaszban hello bal oldali ablaktáblán.

![A Power BI-jelentések](media/log-analytics-powerbi/walkthrough-report.png)

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toobuild lekérdezések, amelyek lehetnek exportált tooPower BI.
* További információ [Power BI](http://powerbi.microsoft.com) toobuild képi megjelenítések Naplóelemzési kivitel alapján.
