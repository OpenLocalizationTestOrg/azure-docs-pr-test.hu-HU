---
title: "az Azure Application Insights Stream Analytics segítségével aaaExport |} Microsoft Docs"
description: "A Stream Analytics folyamatosan alakíthatja át, és a útvonal hello adatok exportálása az Application Insights."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 31594221-17bd-4e5e-9534-950f3b022209
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: bwren
ms.openlocfilehash: fda9b64f588c520833b2669eafdf650efc3de6be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-stream-analytics-tooprocess-exported-data-from-application-insights"></a>Használja a Stream Analytics tooprocess az exportált adatok az Application Insights
[Az Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) hello ideális eszköz adatfeldolgozás [Application Insights-ból exportált](app-insights-export-telemetry.md). A Stream Analytics segítségével olvasnak be adatokat különböző forrásokból. Az átalakítási és hello adatok szűrése és tooa különböző nyelő irányításához.

Ebben a példában mi létrehozunk egy adaptert, amely beolvassa az Application Insights, átnevezése és dolgozza fel néhány hello mező, és kiszolgálókészletéhez azt a Power BI-bA.

> [!WARNING]
> Nincsenek sokkal hatékonyabb és könnyebben [javasolt módját toodisplay Application Insights adatokat a Power BI](app-insights-export-power-bi.md). hello bemutatott elérési út csak egy példa tooillustrate hogyan tooprocess exportált adatok.
> 
> 

![SA tooPBI keresztül exportálás letiltása diagramja](./media/app-insights-export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Tároló létrehozása az Azure-ban
A folyamatos exportálás adatok tooan Azure Storage-fiók esetén mindig kimenete, ezért először a toocreate hello tárolási kell.

1. Hozzon létre egy "klasszikus" tárfiókot az előfizetésében szereplő hello [Azure-portálon](https://portal.azure.com).
   
   ![Azure-portálon válassza ki az új, az adatok, a tárolás](./media/app-insights-export-stream-analytics/030.png)
2. Tároló létrehozása
   
    ![Az új tárolási hello tárolók kiválasztása, hello tárolók csempére, majd a Hozzáadás gombra](./media/app-insights-export-stream-analytics/040.png)
3. Hello tárelérési kulcs másolása
   
    Szüksége lehet rájuk hamarosan tooset hello bemeneti toohello stream analytics szolgáltatás.
   
    ![Hello tárolóban nyissa meg a beállításokat, a kulcsokat, és másolatot hello elsődleges elérési kulcsot](./media/app-insights-export-stream-analytics/045.png)

## <a name="start-continuous-export-tooazure-storage"></a>Indítsa el a folyamatos exportálás tooAzure tároló
[A folyamatos exportálás](app-insights-export-telemetry.md) helyezi át az adatokat az Application Insights az Azure storage-be.

1. Hello Azure-portálon keresse meg az alkalmazáshoz létrehozott toohello Application Insights-erőforrást.
   
    ![A Tallózás, az Application Insights az alkalmazáshoz](./media/app-insights-export-stream-analytics/050.png)
2. A folyamatos exportálás létrehozása.
   
    ![Válassza a beállítások, folyamatos exportálási hozzáadása](./media/app-insights-export-stream-analytics/060.png)

    Válassza ki a korábban létrehozott hello tárfiókot:

    ![Hello exportálási cél beállítása](./media/app-insights-export-stream-analytics/070.png)

    Állítsa be a kívánt hello eseménytípusok toosee:

    ![Válassza ki az esemény típusa](./media/app-insights-export-stream-analytics/080.png)

1. Néhány adat gyűlik össze legyen. Elhelyezkedik vissza, és a felhasználók használhassa az alkalmazást egy ideig. Telemetria határozza meg, és látni fogja, statisztikai diagramok [metrika explorer](app-insights-metrics-explorer.md) és az egyes események [diagnosztikai keresési](app-insights-diagnostic-search.md). 
   
    És is, hello adatok tooyour tárolási exportálja. 
2. Vizsgálja meg az exportált hello adatokat. A Visual Studio felületén válassza **megtekintése / Cloud Explorer**, és nyissa meg az Azure / tárolási. (Ha még nem rendelkezik a menüpont, tooinstall hello Azure SDK-t kell: hello új projekt párbeszédpanel megnyitásához, és nyissa meg a Visual C# / felhő / Microsoft Azure SDK beolvasása a .NET-hez.)
   
    ![](./media/app-insights-export-stream-analytics/04-data.png)
   
    Jegyezze fel a hello közös hello alkalmazás nevét és instrumentation kulcsból származtatott hello elérési út része. 

hello események tooblob fájlok írt JSON formátumban. Minden fájl tartalmazhat egy vagy több esemény. Ezért szeretnénk tooread hello eseményadatok és szűrő ki szeretnénk hello mezőket. Azt sikerült műveletekből hello adatokkal az összes típusú léteznek, de a terv ma toouse Stream Analytics toopipe hello adatok tooPower BI.

## <a name="create-an-azure-stream-analytics-instance"></a>Hozzon létre egy Azure Stream Analytics-példányt
A hello [klasszikus Azure portálon](https://manage.windowsazure.com/), válassza ki a hello Azure Stream Analytics szolgáltatás, és hozzon létre egy új Stream Analytics-feladatot:

![](./media/app-insights-export-stream-analytics/090.png)

![](./media/app-insights-export-stream-analytics/100.png)

Amikor hello új feladatot hoz létre, bontsa ki a hozzá tartozó részletek:

![](./media/app-insights-export-stream-analytics/110.png)

### <a name="set-blob-location"></a>A blob hely beállítása
A folyamatos exportálás blob tootake bemenetének állítsa be:

![](./media/app-insights-export-stream-analytics/120.png)

Most importáljon a Tárfiókba, amelyet korábban feljegyzett hello elsődleges elérési kulcsot lesz szüksége. Állítsa be a hello Tárfiók kulcsa.

![](./media/app-insights-export-stream-analytics/130.png)

### <a name="set-path-prefix-pattern"></a>Set elérési út előtag mintája
![](./media/app-insights-export-stream-analytics/140.png)

**Lehet, hogy tooset hello dátumformátum tooYYYY-hh-nn (a szaggatott vonal).**

elérési út előtag mintája hello határozza meg, ahol a Stream Analytics hello tárolási hello bemeneti fájlok talál. Tooset kell azt a folyamatos exportálás toocorrespond toohow hello adatokat tárolja. Állítsa be ehhez hasonló:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Ebben a példában:

* `webapplication27`hello Application Insights-erőforrás neve hello **összes kisbetű**.
* `1234...`az Application Insights-erőforrást, hello hello instrumentation kulcsa **kötőjelek kihagyásával**. 
* `PageViews`hello típusú adatok tooanalyze szeretné. rendelkezésre álló típusok hello hello szűrő, beállíthatja a folyamatos exportálás függ. Vizsgálja meg a hello exportált adatok toosee hello más elérhető típusok, és tekintse meg a hello [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).
* `/{date}/{time}`a minta írt szó.

> [!NOTE]
> Vizsgálja meg a hello tárolási toomake meg arról, hogy a megfelelő beolvasni hello elérési útját.
> 
> 

### <a name="finish-initial-setup"></a>Kezdeti telepítés befejezése
Erősítse meg a hello szerializálási formátum:

![Erősítse meg, és zárja be a varázsló](./media/app-insights-export-stream-analytics/150.png)

Hello bezárása, és várjon, amíg a telepítő toocomplete hello.

> [!TIP]
> Hello minta parancs toodownload bizonyos adatok felhasználásával. Legyen, a teszt minta toodebug a lekérdezést.
> 
> 

## <a name="set-hello-output"></a>Set hello kimeneti
Most jelölje ki a feladatot, és állítsa be a hello kimeneti.

![Válassza ki a hello új csatorna, kattintson a kimenetek, a Hozzáadás, a Power bi-ban](./media/app-insights-export-stream-analytics/160.png)

Adja meg a **munkahelyi vagy iskolai fiók** tooauthorize Stream Analytics tooaccess a Power BI-erőforrás. Majd készlet hello kimeneti, és hello cél Power BI DataSet adatkészlet és a tábla nevét.

![A neveket készlet](./media/app-insights-export-stream-analytics/170.png)

## <a name="set-hello-query"></a>Set hello lekérdezés
hello lekérdezés hello fordítási a bemeneti toooutput szabályozza.

![Jelölje ki a hello feladat, majd kattintson a lekérdezést. Illessze be az alábbi hello minta.](./media/app-insights-export-stream-analytics/180.png)

Hello teszt függvény toocheck, hogy elérhetővé hello jobb oldali kimeneti használja. Adjon meg hozzá hello bemenetek oldalról lejegyezte hello mintaadatok. 

### <a name="query-toodisplay-counts-of-events"></a>Lekérdezés toodisplay számát is események
Illessze be a lekérdezést:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* export-bemeneti érték azt a nevet adott toohello adatfolyam-bemenet hello alias
* pbi-kimeneti rendszer hello kimeneti alias meghatározott
* Használjuk [külső alkalmazása GetElements](https://msdn.microsoft.com/library/azure/dn706229.aspx) mert egy beágyazott JSON arrray hello esemény neve. Majd hello válassza kivételezések hello esemény-névvel, hello több példányban található ilyen nevű hello időszak számát. Hello [Group By](https://msdn.microsoft.com/library/azure/dn835023.aspx) záradék hello elemek csoportok 1 perces időszakokra.

### <a name="query-toodisplay-metric-values"></a>Lekérdezés toodisplay metrika értékek
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Ez a lekérdezés eseményrögzítési hello metrikák telemetriai tooget hello esemény időpontja és hello Átjárómetrika értékeként. hello metrika értékei belül tömb, így hello külső alkalmazása GetElements mintát tooextract hello sorok használjuk. "myMetric" Ebben az esetben az hello metrika hello név. 

### <a name="query-tooinclude-values-of-dimension-properties"></a>Lekérdezés tooinclude értékek dimenzió tulajdonságai
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Ez a lekérdezés hello dimenziótulajdonságok nélkül attól függően, hogy egy adott dimenzió hello dimenzió tömb rögzített indexnél alatt az értékeket tartalmaz.

## <a name="run-hello-job"></a>Hello feladat futtatása
A múltbeli toostart hello feladatot hello kiválaszthatja a dátumot. 

![Jelölje ki a hello feladat, majd kattintson a lekérdezést. Illessze be az alábbi hello minta.](./media/app-insights-export-stream-analytics/190.png)

Várjon, amíg hello feladat fut-e.

## <a name="see-results-in-power-bi"></a>A Power BI eredmények megtekintése
> [!WARNING]
> Nincsenek sokkal hatékonyabb és könnyebben [javasolt módját toodisplay Application Insights adatokat a Power BI](app-insights-export-power-bi.md). hello bemutatott elérési út csak egy példa tooillustrate hogyan tooprocess exportált adatok.
> 
> 

Nyissa meg a munkahelyi vagy iskolai fiókját, és jelölje be hello dataset és tábla, amelyet hello Stream Analytics-feladat eredményének hello Power bi-ban.

![A Power bi-ban válassza ki a DataSet adatkészlet és a mezőket.](./media/app-insights-export-stream-analytics/200.png)

Most már használhat ez az adatkészlet a jelentések és irányítópultok a [Power BI](https://powerbi.microsoft.com).

![A Power bi-ban válassza ki a DataSet adatkészlet és a mezőket.](./media/app-insights-export-stream-analytics/210.png)

## <a name="no-data"></a>Nincs adat?
* Ellenőrizze, hogy [set hello dátumformátum](#set-path-prefix-pattern) megfelelően tooYYYY-hh-nn (a szaggatott vonal).

## <a name="video"></a>Videó
Noam Ben Zeev bemutatja, hogyan tooprocess exportált adatok használatával a Stream Analytics.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Következő lépések
* [Folyamatos exportálás](app-insights-export-telemetry.md)
* [Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.](app-insights-export-data-model.md)
* [Application Insights](app-insights-overview.md)

