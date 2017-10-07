---
title: "egy Azure Storage-fiók aaaHow toomonitor |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor használatával az Azure storage-fiók hello Azure-portálon."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: b83cba7b-4627-4ba7-b5d0-f1039fe30e78
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: marsma
ms.openlocfilehash: 782873648fdbccc0191a074cd4044f97af8b7c19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-a-storage-account-in-hello-azure-portal"></a>A figyelő a tárfiókokat hello Azure-portálon

[Az Azure Storage Analytics](storage-analytics.md) metrikákat biztosít minden a tárolási szolgáltatások és a naplókat a BLOB, üzenetsorok, és a táblázatot. Használhatja a hello [Azure-portálon](https://portal.azure.com) tooconfigure mely metrikák és a naplók tárolja, a fiókjához, diagramok és konfigurálása, amelyek a metrikai adatok vizuális ábrázolásai biztosítanak.

> [!NOTE]
> Nincsenek hello Azure-portálon a figyelési adatok vizsgálata kapcsolódó költségeket. További információkért lásd: [tárolási analitika és számlázási](/rest/api/storageservices/Storage-Analytics-and-Billing).
>
> Az Azure File storage jelenleg tárolási analitika metrikák támogatja, de még nem támogatja a naplózást.
>
> Tárfiókok replikációs típussal a Zónaredundáns tárolás (ZRS) jelenleg nem rendelkezik hello metrikák vagy engedélyezve a naplózási szolgáltatás.
> 
> A részletes útmutatót a tárolási analitika és egyéb eszközök tooidentify, diagnosztizálása, és az Azure tárolással kapcsolatos problémák elhárításához, lásd: [figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).
>

## <a name="configure-monitoring-for-a-storage-account"></a>A tárfiók figyelésének konfigurálása

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **tárfiókok**, majd hello tárolási fiók neve tooopen hello fiók irányítópult.
1. Válassza ki **diagnosztika** a hello **figyelés** hello menü panel szakasza.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)

1. Jelölje be hello **típus** metrikák adatok az egyes **szolgáltatás** toomonitor kívánja, és hello **adatmegőrzési** hello adatok. Letilthatja a figyelés beállítás **állapot** túl**ki**.

    ![MonitoringOptions](./media/storage-monitor-storage-account/stg-enable-metrics-01.png)

   A mérőszámok engedélyezheti az egyes szolgáltatásokhoz, amelyek mindegyikét új storage-fiókok alapértelmezés szerint engedélyezve vannak két típusa van:

   * **Összesített**: például be-és kilépési, a rendelkezésre állási, a késés és a sikeres százalékos metrikákat gyűjtő. A metrikák összesítése hello blob, a várólista, a tábla és a Fájlszolgáltatások.
   * **/ API**: toohello összesített metrikák hozzáadása, a gyűjti hello ugyanazokat a metrikák hello Azure Storage szolgáltatás API lévő egyes tárolási műveletek.

   tooset hello adatmegőrzési házirend, áthelyezés hello **megőrzés (nap)** csúszkát, vagy adja meg a hello ennyi napra visszamenőleg adatok tooretain, az 1 too365. hello új tárfiókok alapértelmezés szerint hét nap. Ha nem szeretné, hogy tooset adatmegőrzési, adja meg a nulla. Ha nincs megőrzési házirend, tooyou toodelete hello figyelési adatok működik.

   > [!WARNING]
   > Metrikai adatok manuális törlésekor van szó. Elavult analytics (régebbi, mint a adatmegőrzési) adatokat hello rendszer ingyenesen törlése. Azt javasoljuk, hogy mennyi ideig szeretné tooretain storage analytics adatokat a fiók alapján adatmegőrzési beállítás. Lásd: [mi díja tegye Ön tudomásával storage mérőszámainak engedélyezésével?](storage-enable-and-view-metrics.md#what-charges-do-you-incur-when-you-enable-storage-metrics) további információt.
   >

1. Hello figyelési konfiguráció befejezése után válassza ki a **mentése**.

Metrikák alapértelmezett készletét diagramok hello storage-fiók panelen, valamint hello szolgáltatás paneleken (blob, várólista, a táblának és fájl) jelenik meg. Metrikák szolgáltatás engedélyezése után a diagramokat az adatok tooappear tooan órát igénybe vehet. Kiválaszthatja **szerkesztése** bármely metrikát a diagram túl[mely metrikáinak megadása](#how-to-customize-metrics-charts) hello diagram jelennek meg.

Értékre állításával letilthatja metrikák adatgyűjtési és -naplózás **állapot** túl**ki**.

> [!NOTE]
> Használja az Azure Storage [table storage](storage-introduction.md#table-storage) toostore hello metrikákat a tárfiók, és a tárolók hello metrikák fiókja táblájában. További információkért lásd:. [Metrikák tárolási módját](storage-analytics.md#how-metrics-are-stored).
>

## <a name="customize-metrics-charts"></a>Metrikák diagramok testreszabása

Használja a következő eljárás toochoose hello mely tárolási metrikák tooview metrikák diagramon. 

1. Indítsa el a tárolási metrika diagram hello Azure-portálon megjelenő. A hello diagramok található **storage-fiók panelen** és hello **metrikák** egy adott szolgáltatás (blob, várólista, tábla, fájl) panelen.

   Ebben a példában a következő diagram hello megjelenő hello dolgozunk **storage-fiók panelen**:

   ![A diagram kijelölés Azure-portálon](./media/storage-monitor-storage-account/stg-customize-chart-00.png)

1. Ezután kattintson bárhová belül hello diagram tooopen hello **metrika** panelen. Válassza ki **diagram szerkesztése** tooopen hello **diagram szerkesztése lehetőséget** panelen.

   ![A diagram gomb diagram panelt a szerkesztése](./media/storage-monitor-storage-account/stg-customize-chart-01.png)

1. A hello **diagram szerkesztése lehetőséget** panelen, jelölje be hello **időtartomány** a hello metrikák toodisplay hello diagram és hello **szolgáltatás** (blob, várólista, table, a fájl) amelynek kívánja metrikák toodisplay. Itt kiválasztottuk, hogy toodisplay hello elmúlt hét metrikák hello blob szolgáltatás:

   ![Tartomány- és időbeállítást hello diagram szerkesztése lehetőséget a panelen](./media/storage-monitor-storage-account/stg-customize-chart-02.png)

1. Jelölje be hello egyedi **metrikák** hello diagram például kellett megjelenő, majd kattintson a **OK**. Például Itt azt a kiválasztott toodisplay hello *ContainerCount* és *ObjectCount* metrikák:

   ![Egyes kiválasztott diagram szerkesztése lehetőséget a panelen](./media/storage-monitor-storage-account/stg-customize-chart-03.png)

A diagram beállításai hatással hello gyűjtemény, összesítési és a figyelés hello tárfiókban lévő adatokat tároló, nem csak hello az metrikák adatainak megjelenítése.

### <a name="metrics-availability-in-charts"></a>Metrikák diagramok rendelkezésre állás érdekében

elérhető módosítások hello listája alapján a szolgáltatást választott ki hello legördülő listán, és hello egységtípus hello diagram szerkesztése. Például kiválaszthatja például százalékos metrikák *PercentNetworkError* és *PercentThrottlingError* csak akkor, ha százalékos egységek megjelenítő diagram szerkesztése:

![Kérelem hiba százalékos diagram hello Azure-portálon](./media/storage-monitor-storage-account/stg-customize-chart-04.png)

### <a name="metrics-resolution"></a>Metrikák felbontás

Diagnosztika kiválasztott hello metrikák érhető el a fiókjához hello metrikák hello feloldása határozza meg:

* **Összesített** figyelés nyújt metrikák például be-és kilépési, a rendelkezésre állási, a késés és a sikeres százalékos aránya. A metrikák összesítése hello blob, table, fájlt és várólista-szolgáltatás.
* **/ API** biztosít egyeztetését feloldási metrikák elérhető egyes tárhelyműveletek, továbbá toohello szolgáltatásiszint-összesítések.

## <a name="configure-metrics-alerts"></a>Metrikák riasztások konfigurálása

Riasztások toonotify hozhat létre, amikor a küszöbértékeket a rendszer elérte a storage erőforrás metrikáit.

1. tooopen hello **riasztási szabályok panel**, toohello görgetve **figyelés** hello szakasza **menü panel** válassza **riasztási szabályok**.
1. Válassza ki **riasztás hozzáadása** tooopen hello **riasztási szabály felvétele** panel
1. Válassza ki a **erőforrás** (blob-, fájl, várólista, tábla) a legördülő lista hello, és adja meg egy **neve** és **leírás** az Új riasztási szabály.
1. Jelölje be hello **metrika** , amelynek szeretné tooadd riasztást, egy riasztás **feltétel**, és egy **küszöbérték**. hello küszöbérték egység típusa attól függően változik, hello metrika választotta. Például "count" típus hello egység *ContainerCount*, közben hello egységet a hello *PercentNetworkError* mérőszáma százalékában.
1. Jelölje be hello **időszak**. Metrika eléri vagy meghaladja a küszöbértéket hello hello időszak amely figyelmeztetést belül.
1. (Választható) Konfigurálása **E-mail** és **Webhook** értesítések. A webhook további információkért lásd: [olyan webhook konfigurálása Azure metrika riasztást](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Ha nem adja meg e-mailben vagy webhook értesítések, riasztásokat csak hello Azure-portálon megjelenik.

!["A riasztási szabály felvétele" panel az Azure-portálon hello](./media/storage-monitor-storage-account/stg-alert-rules-01.png)

## <a name="add-metrics-charts-toohello-portal-dashboard"></a>Adja hozzá a metrikák diagramok toohello portál irányítópultján

A tárolási fiók tooyour portál Irányítópultjára bármely Azure Storage metrikák diagramok is hozzáadhat.

1. Jelölje be kattintson **Szerkesztés irányítópult** közben a webkonzolban megjeleníti az irányítópultot hello [Azure-portálon](https://portal.azure.com).
1. A hello **csempe gyűjtemény**, jelölje be **keresés tartalmazó csempék éppen úgy által** > **típus**.
1. Válassza ki **típus** > **tárfiókok**.
1. A **erőforrások**, válasszon hello tárfiókot, amelynek tooadd toohello irányítópult kívánja metrikákat.
1. Válassza ki **kategóriák** > **figyelési**.
1. Fogd és vidd hello diagram csempe alakzatot hello mérőszám azt szeretné, megjelenik az irányítópulton. Ismételje meg a hello irányítópulton megjelenített szeretné összes metrikát. A kép a következő hello hello "Blobok – összes lekérdezés" diagram például ki van jelölve, de minden hello diagramok érhetők el elhelyezésre az irányítópulton.

   ![Azure-portálon csempe gyűjteménye](./media/storage-monitor-storage-account/stg-customize-dashboard-01.png)
1. Válassza ki **végzett Testreszabás** hello irányítópult amikor elkészült hozzáadását diagramok hello tetején.

Diagramok tooyour irányítópult hozzáadása után tetszés azokat a [metrikák diagramok testreszabása](#how-to-customize-metrics-charts).

## <a name="configure-logging"></a>Naplózás konfigurálása

Azure Storage toosave diagnosztikai naplókat a további olvasási, írási és törlési kérelmek hello blob, table és queue szolgáltatások találhatók. hello adatmegőrzési házirend beállított toothese naplókat is vonatkozik.

> [!NOTE]
> Az Azure File storage jelenleg tárolási analitika metrikák támogatja, de még nem támogatja a naplózást.
>

1. A hello [Azure-portálon](https://portal.azure.com), jelölje be **tárfiókok**, hello tárolási fiók tooopen hello storage-fiók panelen hello nevét.
1. Válassza ki **diagnosztika** a hello **figyelés** hello menü panel szakasza.

    ![Diagnosztika menüpontja alatt figyelés hello Azure-portálon.](./media/storage-monitor-storage-account/stg-enable-metrics-00.png)
    
1. Győződjön meg arról **állapot** értéke túl**a**, és jelölje be hello **szolgáltatások** , amelynek szeretné tooenable naplózás.

    ![Naplózás konfigurálása az Azure-portálon hello.](./media/storage-monitor-storage-account/stg-enable-logging-01.png)
1. Kattintson a **Save** (Mentés) gombra.

hello diagnosztikai naplók tárfiókba $logs nevű blob tárolóban lesznek mentve. Megtekintheti például hello Tártallózóval hello naplóadatok [Microsoft Tártallózó](http://storageexplorer.com), vagy programozott módon a hello a storage ügyféloldali kódtára vagy a PowerShell használatával.

Hello $logs tároló elérésével kapcsolatos információkért lásd: [tárolási naplózás engedélyezése és használata naplóadatok](/rest/api/storageservices/enabling-storage-logging-and-accessing-log-data).

## <a name="next-steps"></a>Következő lépések

* Kapcsolatos további részletekért található [metrika, naplózási és számlázási](storage-analytics.md) tárolási elemzéséhez.
* [Lehetővé teszik az Azure Storage-metrikák és a nézet metrikák adatok](storage-enable-and-view-metrics.md) PowerShell használatával és a C# programozott módon.
