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
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a><span data-ttu-id="9c3fa-103">Webhelyek naplóinak elemzése az Azure Data Lake Analytics használatával</span><span class="sxs-lookup"><span data-stu-id="9c3fa-103">Analyze Website logs using Azure Data Lake Analytics</span></span>
<span data-ttu-id="9c3fa-104">Ismerje meg, hogyan tooanalyze webhelyek naplóinak használatával a Data Lake Analytics, különösen a tudni, melyik hivatkozó kérelmei hibába ütközött, amikor megpróbáltak toovisit hello webhelyet.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-104">Learn how tooanalyze website logs using Data Lake Analytics, especially on finding out which referrers ran into errors when they tried toovisit hello website.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c3fa-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9c3fa-105">Prerequisites</span></span>
* <span data-ttu-id="9c3fa-106">**Visual Studio 2015-öt vagy a Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-106">**Visual Studio 2015 or Visual Studio 2013**.</span></span>
* <span data-ttu-id="9c3fa-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span></span>

    <span data-ttu-id="9c3fa-108">A Data Lake Tools for Visual Studio telepítése után megjelenik egy **Data Lake** hello elemére **eszközök** elemét a Visual Studióban:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-108">Once Data Lake Tools for Visual Studio is installed, you will see a **Data Lake** item in hello **Tools** menu in Visual Studio:</span></span>

    ![U-SQL Visual Studio menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* <span data-ttu-id="9c3fa-110">**A Data Lake Analytics és a Data Lake Tools for Visual Studio hello vonatkozó általános ismeretekre**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-110">**Basic knowledge of Data Lake Analytics and hello Data Lake Tools for Visual Studio**.</span></span> <span data-ttu-id="9c3fa-111">tooget lépések, olvassa el:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-111">tooget started, see:</span></span>

  * <span data-ttu-id="9c3fa-112">[Fejlesztése a Data Lake tools for Visual Studio használatával U-SQL parancsfájl](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-112">[Develop U-SQL script using Data Lake tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="9c3fa-113">**Data Lake Analytics-fiók.**</span><span class="sxs-lookup"><span data-stu-id="9c3fa-113">**A Data Lake Analytics account.**</span></span>  <span data-ttu-id="9c3fa-114">Lásd: [Azure Data Lake Analytics-fiók létrehozása](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-114">See [Create an Azure Data Lake Analytics account](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="9c3fa-115">**Töltse fel a hello minta adatok toohello Data Lake Analytics-fiók.**</span><span class="sxs-lookup"><span data-stu-id="9c3fa-115">**Upload hello sample data toohello Data Lake Analytics account.**</span></span> <span data-ttu-id="9c3fa-116">Lásd: [toocopy mintaadatfájlok](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-116">See [toocopy sample data files](data-lake-analytics-get-started-portal.md).</span></span>

    <span data-ttu-id="9c3fa-117">egy Data Lake Analytics-feladat toorun, szüksége lesz a bizonyos adatokat.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-117">toorun a Data Lake Analytics job, you will need some data.</span></span> <span data-ttu-id="9c3fa-118">Annak ellenére, hogy hello Data Lake Tools támogatja az adatok feltöltését, az oktatóprogram könnyebb toofollow hello portál tooupload hello minta adatok toomake fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-118">Even though hello Data Lake Tools supports uploading data, you will use hello portal tooupload hello sample data toomake this tutorial easier toofollow.</span></span>

## <a name="connect-tooazure"></a><span data-ttu-id="9c3fa-119">Csatlakozás tooAzure</span><span class="sxs-lookup"><span data-stu-id="9c3fa-119">Connect tooAzure</span></span>
<span data-ttu-id="9c3fa-120">Mielőtt build és a U-SQL-parancsfájlok tesztelése, először kapcsolódnia tooAzure.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-120">Before you can build and test any U-SQL scripts, you must first connect tooAzure.</span></span>

<span data-ttu-id="9c3fa-121">**tooconnect tooData Lake Analytics**</span><span class="sxs-lookup"><span data-stu-id="9c3fa-121">**tooconnect tooData Lake Analytics**</span></span>

1. <span data-ttu-id="9c3fa-122">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-122">Open Visual Studio.</span></span>
2. <span data-ttu-id="9c3fa-123">Kattintson a **a Data Lake > lehetőségek és beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-123">Click **Data Lake > Options and Settings**.</span></span>
3. <span data-ttu-id="9c3fa-124">Kattintson a **bejelentkezés**, vagy **felhasználói módosítása** Ha valaki bejelentkezett-e, és hello utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-124">Click **Sign In**, or **Change User** if someone has signed in, and follow hello instructions.</span></span>
4. <span data-ttu-id="9c3fa-125">Kattintson a **OK** tooclose hello lehetőségek és beállítások párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-125">Click **OK** tooclose hello Options and Settings dialog.</span></span>

<span data-ttu-id="9c3fa-126">**toobrowse a Data Lake Analytics-fiókok**</span><span class="sxs-lookup"><span data-stu-id="9c3fa-126">**toobrowse your Data Lake Analytics accounts**</span></span>

1. <span data-ttu-id="9c3fa-127">Nyissa meg a Visual Studio eszközből **Server Explorer** press által **CTRL + ALT + S**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-127">From Visual Studio, open **Server Explorer** by press **CTRL+ALT+S**.</span></span>
2. <span data-ttu-id="9c3fa-128">A **Server Explorer** eszközben bontsa ki az **Azure** elemet, majd a **Data Lake Analytics** elemet.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-128">From **Server Explorer**, expand **Azure**, and then expand **Data Lake Analytics**.</span></span> <span data-ttu-id="9c3fa-129">Ekkor megjelenik a Data Lake Analytics-fiókok listája, ha vannak ilyenek.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-129">You shall see a list of your Data Lake Analytics accounts if there are any.</span></span> <span data-ttu-id="9c3fa-130">Hello Studio Data Lake Analytics-fiókok nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-130">You cannot create Data Lake Analytics accounts from hello studio.</span></span> <span data-ttu-id="9c3fa-131">egy olyan fiók, toocreate lásd [Ismerkedés az Azure Data Lake Analytics Azure portál használatával](data-lake-analytics-get-started-portal.md) vagy [Ismerkedés az Azure Data Lake Analytics az Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-131">toocreate an account, see [Get Started with Azure Data Lake Analytics using Azure Portal](data-lake-analytics-get-started-portal.md) or [Get Started with Azure Data Lake Analytics using Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span></span>

## <a name="develop-u-sql-application"></a><span data-ttu-id="9c3fa-132">U-SQL-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="9c3fa-132">Develop U-SQL application</span></span>
<span data-ttu-id="9c3fa-133">A U-SQL-alkalmazások többnyire U-SQL parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-133">A U-SQL application is mostly a U-SQL script.</span></span> <span data-ttu-id="9c3fa-134">További információk a U-SQL, toolearn lásd [Ismerkedés a U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-134">toolearn more about U-SQL, see [Get started with U-SQL](data-lake-analytics-u-sql-get-started.md).</span></span>

<span data-ttu-id="9c3fa-135">Továbbá operátorok felhasználói toohello alkalmazást adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-135">You can add addition user-defined operators toohello application.</span></span>  <span data-ttu-id="9c3fa-136">További információkért lásd: [fejlesztése U-SQL-felhasználó által definiált Data Lake Analytics-feladatok operátorok](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-136">For more information, see [Develop U-SQL user defined operators for Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span></span>

<span data-ttu-id="9c3fa-137">**toocreate, és küldje el egy Data Lake Analytics-feladat**</span><span class="sxs-lookup"><span data-stu-id="9c3fa-137">**toocreate and submit a Data Lake Analytics job**</span></span>

1. <span data-ttu-id="9c3fa-138">Kattintson a hello **fájl > Új > projekt**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-138">Click hello **File > New > Project**.</span></span>
2. <span data-ttu-id="9c3fa-139">Válassza ki a hello U-SQL projekt típusát.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-139">Select hello U-SQL Project type.</span></span>

    ![új U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. <span data-ttu-id="9c3fa-141">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-141">Click **OK**.</span></span> <span data-ttu-id="9c3fa-142">A Visual studio létrehoz egy megoldást Script.usql fájllal.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-142">Visual studio creates a solution with a Script.usql file.</span></span>
4. <span data-ttu-id="9c3fa-143">Adja meg a következő parancsfájl hello Script.usql fájlba hello:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-143">Enter hello following script into hello Script.usql file:</span></span>

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

    <span data-ttu-id="9c3fa-144">toounderstand hello U-SQL, lásd: [Ismerkedés a Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-144">toounderstand hello U-SQL, see [Get started with Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>    
5. <span data-ttu-id="9c3fa-145">Új U-SQL parancsfájl tooyour projektet, és írja be a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-145">Add a new U-SQL script tooyour project and enter hello following:</span></span>

        // Query hello referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        too@"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. <span data-ttu-id="9c3fa-146">Váltson vissza toohello első U-SQL parancsfájlt és a következő toohello **Submit** gombra, adja meg az Analytics-fiókja.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-146">Switch back toohello first U-SQL script and next toohello **Submit** button, specify your Analytics account.</span></span>
7. <span data-ttu-id="9c3fa-147">A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Build Script** (Parancsfájl létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-147">From **Solution Explorer**, right click **Script.usql**, and then click **Build Script**.</span></span> <span data-ttu-id="9c3fa-148">Ellenőrizze a hello eredményez hello Tesztkimenet ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-148">Verify hello results in hello Output pane.</span></span>
8. <span data-ttu-id="9c3fa-149">A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Submit Script** (Parancsfájl elküldése) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-149">From **Solution Explorer**, right click **Script.usql**, and then click **Submit Script**.</span></span>
9. <span data-ttu-id="9c3fa-150">Ellenőrizze a hello **Analytics-fiók** hello egyet, ha szeretné, hogy toorun hello feladatot, és kattintson az **Submit**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-150">Verify hello **Analytics Account** is hello one where you want toorun hello job, and then click **Submit**.</span></span> <span data-ttu-id="9c3fa-151">Után az eredmények és a feladatra mutató hivatkozás esetén érhetők el a Visual Studio eredmények ablakában a Data Lake Tools hello hello elküldés.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-151">Submission results and job link are available in hello Data Lake Tools for Visual Studio Results window when hello submission is completed.</span></span>
10. <span data-ttu-id="9c3fa-152">Várjon, amíg hello feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-152">Wait until hello job is completed successfully.</span></span>  <span data-ttu-id="9c3fa-153">Hello feladat nem sikerült, valószínűleg hiányzik hello forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-153">If hello job failed, it is most likely missing hello source file.</span></span>  <span data-ttu-id="9c3fa-154">Lásd: Ez az oktatóanyag hello Előfeltételek szakaszában.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-154">Please see hello Prerequisite section of this tutorial.</span></span> <span data-ttu-id="9c3fa-155">További hibaelhárítási információért lásd: [figyelése és hibaelhárítása az Azure Data Lake Analytics-feladatok](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="9c3fa-155">For additional troubleshooting information, see [Monitor and troubleshoot Azure Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>

    <span data-ttu-id="9c3fa-156">Hello feladat befejezése után megjelenik a következő képernyő hello:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-156">When hello job is completed, you shall see hello following screen:</span></span>

    ![a Data lake analytics webes naplók webhelyek naplóinak elemzése](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. <span data-ttu-id="9c3fa-158">Most ismételje meg a 7 – 10 a **Script1.usql**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-158">Now repeat steps 7- 10 for **Script1.usql**.</span></span>

<span data-ttu-id="9c3fa-159">**toosee hello feladatkiemenetét**</span><span class="sxs-lookup"><span data-stu-id="9c3fa-159">**toosee hello job output**</span></span>

1. <span data-ttu-id="9c3fa-160">A **Server Explorer**, bontsa ki a **Azure**, bontsa ki a **Data Lake Analytics**, bontsa ki a Data Lake Analytics-fiókjait, bontsa ki a **Storage-fiókok**, kattintson a jobb gombbal a hello alapértelmezett Data Lake-tárfiókra, és kattintson a **Explorer**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-160">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click hello default Data Lake Storage account, and then click **Explorer**.</span></span>
2. <span data-ttu-id="9c3fa-161">Kattintson duplán a **minták** tooopen hello mappa, és kattintson duplán **kimenetek**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-161">Double-click **Samples** tooopen hello folder, and then double-click **Outputs**.</span></span>
3. <span data-ttu-id="9c3fa-162">Kattintson duplán a **UnsuccessfulResponsees.log**.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-162">Double-click **UnsuccessfulResponsees.log**.</span></span>
4. <span data-ttu-id="9c3fa-163">Hello kimeneti fájl belül hello diagramnézetnek hello feladat rendelés toonavigate a dupla közvetlen toohello kimeneti is.</span><span class="sxs-lookup"><span data-stu-id="9c3fa-163">You can also double-click hello output file inside hello graph view of hello job in order toonavigate directly toohello output.</span></span>

## <a name="see-also"></a><span data-ttu-id="9c3fa-164">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-164">See also</span></span>
<span data-ttu-id="9c3fa-165">a Data Lake Analytics használatának különböző eszközök használatába tooget lásd:</span><span class="sxs-lookup"><span data-stu-id="9c3fa-165">tooget started with Data Lake Analytics using different tools, see:</span></span>

* [<span data-ttu-id="9c3fa-166">A Data Lake Analytics használatának első lépései az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="9c3fa-166">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="9c3fa-167">A Data Lake Analytics használatának első lépései az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="9c3fa-167">Get started with Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="9c3fa-168">A Data Lake Analytics használatának első lépései a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="9c3fa-168">Get started with Data Lake Analytics using .NET SDK</span></span>](data-lake-analytics-get-started-net-sdk.md)
