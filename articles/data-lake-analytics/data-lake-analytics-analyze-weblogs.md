---
title: "Azure Data Lake Analytics használatával aaaAnalyze webhelyek naplóinak |} Microsoft Docs"
description: "Ismerje meg, hogy miként naplózza tooanalyze webhely Data Lake Analytics használatával. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 3a196735-d0d9-4deb-ba68-c4b3f3be8403
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: saveenr
ms.openlocfilehash: d27aaca95ed2b643cfed7a17b0066bf7fa4a1bf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a>Webhelyek naplóinak elemzése az Azure Data Lake Analytics használatával
Ismerje meg, hogyan tooanalyze webhelyek naplóinak használatával a Data Lake Analytics, különösen a tudni, melyik hivatkozó kérelmei hibába ütközött, amikor megpróbáltak toovisit hello webhelyet.

## <a name="prerequisites"></a>Előfeltételek
* **Visual Studio 2015-öt vagy a Visual Studio 2013**.
* **[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.

    A Data Lake Tools for Visual Studio telepítése után megjelenik egy **Data Lake** hello elemére **eszközök** elemét a Visual Studióban:

    ![U-SQL Visual Studio menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* **A Data Lake Analytics és a Data Lake Tools for Visual Studio hello vonatkozó általános ismeretekre**. tooget lépések, olvassa el:

  * [Fejlesztése a Data Lake tools for Visual Studio használatával U-SQL parancsfájl](data-lake-analytics-data-lake-tools-get-started.md).
* **Data Lake Analytics-fiók.**  Lásd: [Azure Data Lake Analytics-fiók létrehozása](data-lake-analytics-get-started-portal.md).
* **Töltse fel a hello minta adatok toohello Data Lake Analytics-fiók.** Lásd: [toocopy mintaadatfájlok](data-lake-analytics-get-started-portal.md).

    egy Data Lake Analytics-feladat toorun, szüksége lesz a bizonyos adatokat. Annak ellenére, hogy hello Data Lake Tools támogatja az adatok feltöltését, az oktatóprogram könnyebb toofollow hello portál tooupload hello minta adatok toomake fogja használni.

## <a name="connect-tooazure"></a>Csatlakozás tooAzure
Mielőtt build és a U-SQL-parancsfájlok tesztelése, először kapcsolódnia tooAzure.

**tooconnect tooData Lake Analytics**

1. Nyissa meg a Visual Studiót.
2. Kattintson a **a Data Lake > lehetőségek és beállítások**.
3. Kattintson a **bejelentkezés**, vagy **felhasználói módosítása** Ha valaki bejelentkezett-e, és hello utasítások szerint.
4. Kattintson a **OK** tooclose hello lehetőségek és beállítások párbeszédpanel.

**toobrowse a Data Lake Analytics-fiókok**

1. Nyissa meg a Visual Studio eszközből **Server Explorer** press által **CTRL + ALT + S**.
2. A **Server Explorer** eszközben bontsa ki az **Azure** elemet, majd a **Data Lake Analytics** elemet. Ekkor megjelenik a Data Lake Analytics-fiókok listája, ha vannak ilyenek. Hello Studio Data Lake Analytics-fiókok nem hozható létre. egy olyan fiók, toocreate lásd [Ismerkedés az Azure Data Lake Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md) vagy [Ismerkedés az Azure Data Lake Analytics az Azure PowerShell](data-lake-analytics-get-started-powershell.md).

## <a name="develop-u-sql-application"></a>U-SQL-alkalmazások fejlesztése
A U-SQL-alkalmazások többnyire U-SQL parancsfájl. További információk a U-SQL, toolearn lásd [Ismerkedés a U-SQL](data-lake-analytics-u-sql-get-started.md).

Továbbá operátorok felhasználói toohello alkalmazást adhat hozzá.  További információkért lásd: [fejlesztése U-SQL-felhasználó által definiált Data Lake Analytics-feladatok operátorok](data-lake-analytics-u-sql-develop-user-defined-operators.md).

**toocreate, és küldje el egy Data Lake Analytics-feladat**

1. Kattintson a hello **fájl > Új > projekt**.
2. Válassza ki a hello U-SQL projekt típusát.

    ![új U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Kattintson az **OK** gombra. A Visual studio létrehoz egy megoldást Script.usql fájllal.
4. Adja meg a következő parancsfájl hello Script.usql fájlba hello:

        // Create a database for easy reuse, so you don't need tooread from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from hello weblog file with hello correct schema.
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string,
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string,
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string,
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int,
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN

            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;

        // Create a table for storing referrers and status
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            DISTRIBUTED BY HASH(Year)
        ) AS

        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer,
                sc_status;

    toounderstand hello U-SQL, lásd: [Ismerkedés a Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).    
5. Új U-SQL parancsfájl tooyour projektet, és írja be a következő hello:

        // Query hello referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        too@"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. Váltson vissza toohello első U-SQL parancsfájlt és a következő toohello **Submit** gombra, adja meg az Analytics-fiókja.
7. A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Build Script** (Parancsfájl létrehozása) elemre. Ellenőrizze a hello eredményez hello Tesztkimenet ablaktáblán.
8. A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Submit Script** (Parancsfájl elküldése) lehetőségre.
9. Ellenőrizze a hello **Analytics-fiók** hello egyet, ha szeretné, hogy toorun hello feladatot, és kattintson az **Submit**. Után az eredmények és a feladatra mutató hivatkozás esetén érhetők el a Visual Studio eredmények ablakában a Data Lake Tools hello hello elküldés.
10. Várjon, amíg hello feladat sikeresen befejeződött.  Hello feladat nem sikerült, valószínűleg hiányzik hello forrásfájl.  Lásd: Ez az oktatóanyag hello Előfeltételek szakaszában. További hibaelhárítási információért lásd: [figyelése és hibaelhárítása az Azure Data Lake Analytics-feladatok](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).

    Hello feladat befejezése után megjelenik a következő képernyő hello:

    ![a Data lake analytics webes naplók webhelyek naplóinak elemzése](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. Most ismételje meg a 7 – 10 a **Script1.usql**.

**toosee hello feladatkiemenetét**

1. A **Server Explorer**, bontsa ki a **Azure**, bontsa ki a **Data Lake Analytics**, bontsa ki a Data Lake Analytics-fiókjait, bontsa ki a **Storage-fiókok**, kattintson a jobb gombbal a hello alapértelmezett Data Lake-tárfiókra, és kattintson a **Explorer**.
2. Kattintson duplán a **minták** tooopen hello mappa, és kattintson duplán **kimenetek**.
3. Kattintson duplán a **UnsuccessfulResponsees.log**.
4. Hello kimeneti fájl belül hello diagramnézetnek hello feladat rendelés toonavigate a dupla közvetlen toohello kimeneti is.

## <a name="see-also"></a>Lásd még:
a Data Lake Analytics használatának különböző eszközök használatába tooget lásd:

* [A Data Lake Analytics használatának első lépései az Azure Portallal](data-lake-analytics-get-started-portal.md)
* [A Data Lake Analytics használatának első lépései az Azure PowerShell-lel](data-lake-analytics-get-started-powershell.md)
* [A Data Lake Analytics használatának első lépései a .NET SDK-val](data-lake-analytics-get-started-net-sdk.md)
