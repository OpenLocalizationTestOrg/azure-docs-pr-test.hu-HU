---
title: "Az Azure Application Insights tooSQL exportálása |} Microsoft Docs"
description: "Az Application Insights adatok tooSQL használja a Stream Analytics folyamatosan exportálja."
services: application-insights
documentationcenter: 
author: noamben
manager: carmonm
ms.assetid: 48903032-2c99-4987-9948-d6e4559b4a63
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/06/2015
ms.author: bwren
ms.openlocfilehash: 58b579499113751a088dc7e66cbec71529773322
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-export-toosql-from-application-insights-using-stream-analytics"></a>Forgatókönyv: A Stream Analytics használ az Application Insights tooSQL exportálása
Ez a cikk bemutatja, hogyan toomove a telemetriai adatokat a [Azure Application Insights] [ start] használatával az Azure SQL adatbázishoz [a folyamatos exportálás] [ export] és [az Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/). 

A folyamatos exportálás a telemetriai adatok áthelyezése az Azure Storage JSON formátumban. Azt fogja hello JSON-objektumok használatával az Azure Stream Analytics elemzése és sorok az adatbázistábla létrehozása.

(Általában a folyamatos exportálás hello módon toodo saját hello telemetriai adatok elemzését az alkalmazások küldése tooApplication Insights. Akkor lehetett igazítja a kód a minta toodo más exportált hello telemetriai adatok, például az adatok összesítése a műveleteket.)

Először foglalkozunk hello azt feltételezi, hogy már rendelkezik hello-alkalmazáshoz toomonitor.

Ebben a példában a fogjuk hello Lapmegtekintések adatainak használni, de hello ugyanilyen mintájú egyszerűen bővíthető tooother adattípusok, például egyéni események és a kivételeket. 

## <a name="add-application-insights-tooyour-application"></a>Az Application Insights tooyour alkalmazás hozzáadása
tooget lépések:

1. [Az Application Insights beállítása a weblapok](app-insights-javascript.md). 
   
    (A példában azt összpontosítson Lapmegtekintések adatainak hello ügyfél böngészőkkel történő feldolgozásáról, de a kiszolgálói oldalon hello is beállíthat az Application Insights a [Java](app-insights-java-get-started.md) vagy [ASP.NET](app-insights-asp-net.md) alkalmazás és a folyamat függőség és más kiszolgáló telemetriai adat.)
2. Tegye közzé az alkalmazást, és tekintse meg a "telemetrikus" adatok jelennek meg az Application Insights-erőforrást.

## <a name="create-storage-in-azure"></a>Tároló létrehozása az Azure-ban
A folyamatos exportálás adatok tooan Azure Storage-fiók esetén mindig kimenete, ezért először a toocreate hello tárolási kell.

1. Hozzon létre egy tárfiókot az előfizetésében szereplő hello [Azure-portálon][portal].
   
    ![Azure-portálon válassza az új, az adatok, a tároló. Válassza ki a klasszikus, válassza a létrehozása. Adja meg a tároló nevét.](./media/app-insights-code-sample-export-sql-stream-analytics/040-store.png)
2. Tároló létrehozása
   
    ![Az új tárolási hello tárolók kiválasztása, hello tárolók csempére, majd a Hozzáadás gombra](./media/app-insights-code-sample-export-sql-stream-analytics/050-container.png)
3. Hello tárelérési kulcs másolása
   
    Szüksége lehet rájuk hamarosan tooset hello bemeneti toohello stream analytics szolgáltatás.
   
    ![Hello tárolóban nyissa meg a beállításokat, a kulcsokat, és másolatot hello elsődleges elérési kulcsot](./media/app-insights-code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-tooazure-storage"></a>Indítsa el a folyamatos exportálás tooAzure tároló
1. Hello Azure-portálon keresse meg az alkalmazáshoz létrehozott toohello Application Insights-erőforrást.
   
    ![A Tallózás, az Application Insights az alkalmazáshoz](./media/app-insights-code-sample-export-sql-stream-analytics/060-browse.png)
2. A folyamatos exportálás létrehozása.
   
    ![Válassza a beállítások, folyamatos exportálási hozzáadása](./media/app-insights-code-sample-export-sql-stream-analytics/070-export.png)

    Válassza ki a korábban létrehozott hello tárfiókot:

    ![Hello exportálási cél beállítása](./media/app-insights-code-sample-export-sql-stream-analytics/080-add.png)

    Állítsa be a kívánt hello eseménytípusok toosee:

    ![Válassza ki az esemény típusa](./media/app-insights-code-sample-export-sql-stream-analytics/085-types.png)


1. Néhány adat gyűlik össze legyen. Elhelyezkedik vissza, és a felhasználók használhassa az alkalmazást egy ideig. Telemetria határozza meg, és látni fogja, statisztikai diagramok [metrika explorer](app-insights-metrics-explorer.md) és az egyes események [diagnosztikai keresési](app-insights-diagnostic-search.md). 
   
    És is, hello adatok tooyour tárolási exportálja. 
2. Vizsgálja meg az exportált hello adatok, vagy hello portal - kiválasztása **Tallózás**, válassza ki a tárfiókot, majd **tárolók** - vagy a Visual Studio. A Visual Studio felületén válassza **megtekintése / Cloud Explorer**, és nyissa meg az Azure / tárolási. (Ha még nem rendelkezik a menüpont, tooinstall hello Azure SDK-t kell: hello új projekt párbeszédpanel megnyitásához, és nyissa meg a Visual C# / felhő / Microsoft Azure SDK beolvasása a .NET-hez.)
   
    ![Nyissa meg a Visual Studio Server Browser bővítményt, az Azure, a tároló](./media/app-insights-code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Jegyezze fel a hello közös hello alkalmazás nevét és instrumentation kulcsból származtatott hello elérési út része. 

hello események tooblob fájlok írt JSON formátumban. Minden fájl tartalmazhat egy vagy több esemény. Ezért szeretnénk tooread hello eseményadatok és szűrő ki szeretnénk hello mezőket. Azt sikerült műveletekből hello adatokkal az összes típusú léteznek, de a terv ma toouse Stream Analytics toomove hello az tooa SQL-adatbázist. Amely megkönnyítő könnyen toorun nagy mennyiségű érdekes lekérdezések.

## <a name="create-an-azure-sql-database"></a>Egy Azure SQL-adatbázis létrehozása
Ismét az előfizetés-től kezdődő [Azure-portálon][portal], hello adatbázis létrehozása (és egy új kiszolgálót, kivéve, ha már van egy) toowhich fog írni hello adatok.

![Új adatok, SQL](./media/app-insights-code-sample-export-sql-stream-analytics/090-sql.png)

Győződjön meg arról, hogy hello adatbázis-kiszolgáló hozzáférést tooAzure szolgáltatások:

![Tallózással keresse meg, kiszolgálók, a kiszolgáló beállításait, a tűzfal, tooAzure hozzáférés engedélyezése](./media/app-insights-code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Létrehoz egy táblát az Azure SQL-adatbázis
Csatlakoztassa az előnyben részesített felügyeleti eszközzel hello előző szakaszban létrehozott toohello adatbázis. Ebben a bemutatóban fogjuk használni [az SQL Server felügyeleti eszközei](https://msdn.microsoft.com/ms174173.aspx) (SSMS).

![](./media/app-insights-code-sample-export-sql-stream-analytics/31-sql-table.png)

Hozzon létre egy új lekérdezést, és hajtsa végre a következő T-SQL hello:

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/app-insights-code-sample-export-sql-stream-analytics/34-create-table.png)

Ez a példa Lapmegtekintések adatainak használunk. toosee hello található egyéb adatok, vizsgálhatja meg a JSON-kimenetét, és tekintse meg a hello [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).

## <a name="create-an-azure-stream-analytics-instance"></a>Hozzon létre egy Azure Stream Analytics-példányt
A hello [klasszikus Azure portálon](https://manage.windowsazure.com/), válassza ki a hello Azure Stream Analytics szolgáltatás, és hozzon létre egy új Stream Analytics-feladatot:

![](./media/app-insights-code-sample-export-sql-stream-analytics/37-create-stream-analytics.png)

![](./media/app-insights-code-sample-export-sql-stream-analytics/38-create-stream-analytics-form.png)

Amikor hello új feladatot hoz létre, bontsa ki a hozzá tartozó részletek:

![](./media/app-insights-code-sample-export-sql-stream-analytics/41-sa-job.png)

#### <a name="set-blob-location"></a>A blob hely beállítása
A folyamatos exportálás blob tootake bemenetének állítsa be:

![](./media/app-insights-code-sample-export-sql-stream-analytics/42-sa-wizard1.png)

Most importáljon a Tárfiókba, amelyet korábban feljegyzett hello elsődleges elérési kulcsot lesz szüksége. Állítsa be a hello Tárfiók kulcsa.

![](./media/app-insights-code-sample-export-sql-stream-analytics/46-sa-wizard2.png)

#### <a name="set-path-prefix-pattern"></a>Set elérési út előtag mintája
![](./media/app-insights-code-sample-export-sql-stream-analytics/47-sa-wizard3.png)

Túl lehet, hogy tooset hello dátumformátum**éééé-hh-nn** (a **kötőjelek**).

elérési út előtag mintája hello határozza meg, hogyan Stream Analytics keresse meg a bemeneti fájlok hello hello tárolóban. Tooset kell azt a folyamatos exportálás toocorrespond toohow hello adatokat tárolja. Állítsa be ehhez hasonló:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Ebben a példában:

* `webapplication27`hello Application Insights-erőforrás neve hello **minden a nagybetűs**. 
* `1234...`az Application Insights-erőforrás hello hello instrumentation kulcsa **az eltávolított kötőjelek**. 
* `PageViews`hello típusú adatok tooanalyze szeretnénk. rendelkezésre álló típusok hello hello szűrő, beállíthatja a folyamatos exportálás függ. Vizsgálja meg a hello exportált adatok toosee hello más elérhető típusok, és tekintse meg a hello [exportálja az adatokat az adatmodellbe](app-insights-export-data-model.md).
* `/{date}/{time}`a minta írt szó.

tooget hello nevét és az Application Insights-erőforrás iKey Essentials az Áttekintés lap megnyitásához, vagy nyissa meg a beállításokat.

#### <a name="finish-initial-setup"></a>Kezdeti telepítés befejezése
Erősítse meg a hello szerializálási formátum:

![Erősítse meg, és zárja be a varázsló](./media/app-insights-code-sample-export-sql-stream-analytics/48-sa-wizard4.png)

Hello bezárása, és várjon, amíg a telepítő toocomplete hello.

> [!TIP]
> Hello minta függvény toocheck, hogy helyesen van beállítva hello bemeneti elérési utat használja. Ha a hiba: Ellenőrizze, hogy van-e adatok hello tárolási hello úgy döntött, hogy a minta az időtartomány. Szerkessze a hello bemeneti definícióját, és ellenőrizze a hello tárfiókot, elérési út előtag beállítása és a dátum helyesen formátumban.
> 
> 

## <a name="set-query"></a>Set-lekérdezés
Nyissa meg a hello lekérdezésszakaszt:

![A stream analytics válassza ki a lekérdezés](./media/app-insights-code-sample-export-sql-stream-analytics/51-query.png)

Cserélje le a hello alapértelmezett lekérdezés:

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

Figyelje meg, hogy először hello néhány tulajdonságainak adott toopage adatok megtekintéséhez. Más telemetriai típusú kivitel más tulajdonságokkal fog rendelkezni. Lásd: hello [részletes adatok modell hivatkozást hello Tulajdonságtípusok és értékeket.](app-insights-export-data-model.md)

## <a name="set-up-output-toodatabase"></a>Kimeneti toodatabase beállítása
Válassza ki az SQL hello output típusúként.

![A stream analytics válassza ki a kimenetek](./media/app-insights-code-sample-export-sql-stream-analytics/53-store.png)

Adja meg a hello SQL-adatbázis.

![Adja meg az adatbázis hello részletei](./media/app-insights-code-sample-export-sql-stream-analytics/55-output.png)

Hello bezárása, és várja meg, egy értesítés, hogy hello kimeneti be van állítva.

## <a name="start-processing"></a>Indítsa el a feldolgozási
Hello feladat indításának hello műveletsávon:

![A stream analytics kattintson a Start](./media/app-insights-code-sample-export-sql-stream-analytics/61-start.png)

Kiválaszthatja, hogy toostart feldolgozási hello most vagy a korábbi adatok toostart kiindulva adatokat. Ez utóbbi hello akkor hasznos, ha korábban a folyamatos exportálás már fut egy ideig.

![A stream analytics kattintson a Start](./media/app-insights-code-sample-export-sql-stream-analytics/63-start.png)

Néhány perc elteltével tooSQL kiszolgálófelügyeleti visszaléphet, és tekintse meg a áramló adatok hello. Például lekérdezéssel ehhez hasonló:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>Kapcsolódó cikkek
* [Használja a Stream Analytics tooPowerBI exportálása](app-insights-export-power-bi.md)
* [Részletes adatok modellhez tartozó referencia hello Tulajdonságtípusok és értékeket.](app-insights-export-data-model.md)
* [A folyamatos Exportálás az Application Insightsban](app-insights-export-telemetry.md)
* [Application Insights](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[export]: app-insights-export-telemetry.md
[metrics]: app-insights-metrics-explorer.md
[portal]: http://portal.azure.com/
[start]: app-insights-overview.md

