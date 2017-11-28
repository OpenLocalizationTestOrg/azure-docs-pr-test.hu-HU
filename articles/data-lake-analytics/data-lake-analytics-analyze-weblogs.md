---
title: "Azure Data Lake Analytics használatával webhelyek naplóinak elemzése |} Microsoft Docs"
description: "Megtudhatja, hogyan használja a Data Lake Analytics webhelyek naplóinak elemzése. "
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
ms.openlocfilehash: 25fbbe97d26491fc421f4821315761c18e523ec8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="analyze-website-logs-using-azure-data-lake-analytics"></a><span data-ttu-id="beb2b-103">Webhelyek naplóinak elemzése az Azure Data Lake Analytics használatával</span><span class="sxs-lookup"><span data-stu-id="beb2b-103">Analyze Website logs using Azure Data Lake Analytics</span></span>
<span data-ttu-id="beb2b-104">Megtudhatja, hogyan használja a Data Lake Analytics, különösen akkor tudni, melyik hivatkozó kérelmei hibába ütközött a webhelyen való webhelyek naplóinak elemzése.</span><span class="sxs-lookup"><span data-stu-id="beb2b-104">Learn how to analyze website logs using Data Lake Analytics, especially on finding out which referrers ran into errors when they tried to visit the website.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="beb2b-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="beb2b-105">Prerequisites</span></span>
* <span data-ttu-id="beb2b-106">**Visual Studio 2015-öt vagy a Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-106">**Visual Studio 2015 or Visual Studio 2013**.</span></span>
* <span data-ttu-id="beb2b-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-107">**[Data Lake Tools for Visual Studio](http://aka.ms/adltoolsvs)**.</span></span>

    <span data-ttu-id="beb2b-108">A Data Lake Tools for Visual Studio telepítése után megjelenik egy **Data Lake** elemnek a **eszközök** elemét a Visual Studióban:</span><span class="sxs-lookup"><span data-stu-id="beb2b-108">Once Data Lake Tools for Visual Studio is installed, you will see a **Data Lake** item in the **Tools** menu in Visual Studio:</span></span>

    ![U-SQL Visual Studio menü](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)
* <span data-ttu-id="beb2b-110">**A Data Lake Analytics és a Data Lake Tools for Visual Studio vonatkozó általános ismeretekre**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-110">**Basic knowledge of Data Lake Analytics and the Data Lake Tools for Visual Studio**.</span></span> <span data-ttu-id="beb2b-111">Első lépések, olvassa el:</span><span class="sxs-lookup"><span data-stu-id="beb2b-111">To get started, see:</span></span>

  * <span data-ttu-id="beb2b-112">[Fejlesztése a Data Lake tools for Visual Studio használatával U-SQL parancsfájl](data-lake-analytics-data-lake-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-112">[Develop U-SQL script using Data Lake tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).</span></span>
* <span data-ttu-id="beb2b-113">**Data Lake Analytics-fiók.**</span><span class="sxs-lookup"><span data-stu-id="beb2b-113">**A Data Lake Analytics account.**</span></span>  <span data-ttu-id="beb2b-114">Lásd: [Azure Data Lake Analytics-fiók létrehozása](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-114">See [Create an Azure Data Lake Analytics account](data-lake-analytics-get-started-portal.md).</span></span>
* <span data-ttu-id="beb2b-115">**A minta adatok feltöltése a Data Lake Analytics-fiókhoz.**</span><span class="sxs-lookup"><span data-stu-id="beb2b-115">**Upload the sample data to the Data Lake Analytics account.**</span></span> <span data-ttu-id="beb2b-116">Lásd: [mintaadatfájlok másolása](data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-116">See [To copy sample data files](data-lake-analytics-get-started-portal.md).</span></span>

    <span data-ttu-id="beb2b-117">Egy Data Lake Analytics-feladat futtatásához adatokra lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="beb2b-117">To run a Data Lake Analytics job, you will need some data.</span></span> <span data-ttu-id="beb2b-118">Habár a Data Lake Tools támogatja az adatok feltöltését, az oktatóprogram könnyebb követhetősége érdekében a példaadatokat a portál használatával fogja feltölteni.</span><span class="sxs-lookup"><span data-stu-id="beb2b-118">Even though the Data Lake Tools supports uploading data, you will use the portal to upload the sample data to make this tutorial easier to follow.</span></span>

## <a name="connect-to-azure"></a><span data-ttu-id="beb2b-119">Csatlakozás az Azure szolgáltatáshoz</span><span class="sxs-lookup"><span data-stu-id="beb2b-119">Connect to Azure</span></span>
<span data-ttu-id="beb2b-120">Előtt létrehozhatja és a U-SQL-parancsfájlok tesztelése, először csatlakoztatnia kell az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="beb2b-120">Before you can build and test any U-SQL scripts, you must first connect to Azure.</span></span>

<span data-ttu-id="beb2b-121">**A Data Lake Analytics szolgáltatáshoz való kapcsolódás**</span><span class="sxs-lookup"><span data-stu-id="beb2b-121">**To connect to Data Lake Analytics**</span></span>

1. <span data-ttu-id="beb2b-122">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="beb2b-122">Open Visual Studio.</span></span>
2. <span data-ttu-id="beb2b-123">Kattintson a **a Data Lake > lehetőségek és beállítások**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-123">Click **Data Lake > Options and Settings**.</span></span>
3. <span data-ttu-id="beb2b-124">Kattintson a **bejelentkezés**, vagy **felhasználói módosítása** Ha valaki bejelentkezett-e, és kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="beb2b-124">Click **Sign In**, or **Change User** if someone has signed in, and follow the instructions.</span></span>
4. <span data-ttu-id="beb2b-125">Kattintson a **OK** lehetőségek és beállítások párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="beb2b-125">Click **OK** to close the Options and Settings dialog.</span></span>

<span data-ttu-id="beb2b-126">**A Data Lake Analytics-fiókok tallózással**</span><span class="sxs-lookup"><span data-stu-id="beb2b-126">**To browse your Data Lake Analytics accounts**</span></span>

1. <span data-ttu-id="beb2b-127">Nyissa meg a Visual Studio eszközből **Server Explorer** press által **CTRL + ALT + S**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-127">From Visual Studio, open **Server Explorer** by press **CTRL+ALT+S**.</span></span>
2. <span data-ttu-id="beb2b-128">A **Server Explorer** eszközben bontsa ki az **Azure** elemet, majd a **Data Lake Analytics** elemet.</span><span class="sxs-lookup"><span data-stu-id="beb2b-128">From **Server Explorer**, expand **Azure**, and then expand **Data Lake Analytics**.</span></span> <span data-ttu-id="beb2b-129">Ekkor megjelenik a Data Lake Analytics-fiókok listája, ha vannak ilyenek.</span><span class="sxs-lookup"><span data-stu-id="beb2b-129">You shall see a list of your Data Lake Analytics accounts if there are any.</span></span> <span data-ttu-id="beb2b-130">A Studio Data Lake Analytics-fiókok nem hozható létre.</span><span class="sxs-lookup"><span data-stu-id="beb2b-130">You cannot create Data Lake Analytics accounts from the studio.</span></span> <span data-ttu-id="beb2b-131">A fiókok létrehozásával kapcsolatos információkért lásd: [Az Azure Data Lake Analytics használatának első lépései az Azure portállal](data-lake-analytics-get-started-portal.md) vagy [Az Azure Data Lake Analytics használatának első lépései az Azure PowerShell-lel](data-lake-analytics-get-started-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-131">To create an account, see [Get Started with Azure Data Lake Analytics using Azure Portal](data-lake-analytics-get-started-portal.md) or [Get Started with Azure Data Lake Analytics using Azure PowerShell](data-lake-analytics-get-started-powershell.md).</span></span>

## <a name="develop-u-sql-application"></a><span data-ttu-id="beb2b-132">U-SQL-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="beb2b-132">Develop U-SQL application</span></span>
<span data-ttu-id="beb2b-133">A U-SQL-alkalmazások többnyire U-SQL parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="beb2b-133">A U-SQL application is mostly a U-SQL script.</span></span> <span data-ttu-id="beb2b-134">További információk a U-SQL, [Ismerkedés a U-SQL](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-134">To learn more about U-SQL, see [Get started with U-SQL](data-lake-analytics-u-sql-get-started.md).</span></span>

<span data-ttu-id="beb2b-135">Az alkalmazás hozzáadása felhasználói operátorok adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="beb2b-135">You can add addition user-defined operators to the application.</span></span>  <span data-ttu-id="beb2b-136">További információkért lásd: [fejlesztése U-SQL-felhasználó által definiált Data Lake Analytics-feladatok operátorok](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-136">For more information, see [Develop U-SQL user defined operators for Data Lake Analytics jobs](data-lake-analytics-u-sql-develop-user-defined-operators.md).</span></span>

<span data-ttu-id="beb2b-137">**Data Lake Analytics-feladat létrehozása és elküldése**</span><span class="sxs-lookup"><span data-stu-id="beb2b-137">**To create and submit a Data Lake Analytics job**</span></span>

1. <span data-ttu-id="beb2b-138">Kattintson a **fájl > Új > projekt**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-138">Click the **File > New > Project**.</span></span>
2. <span data-ttu-id="beb2b-139">Válassza ki a U-SQL projekt.</span><span class="sxs-lookup"><span data-stu-id="beb2b-139">Select the U-SQL Project type.</span></span>

    ![új U-SQL Visual Studio-projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. <span data-ttu-id="beb2b-141">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="beb2b-141">Click **OK**.</span></span> <span data-ttu-id="beb2b-142">A Visual studio létrehoz egy megoldást Script.usql fájllal.</span><span class="sxs-lookup"><span data-stu-id="beb2b-142">Visual studio creates a solution with a Script.usql file.</span></span>
4. <span data-ttu-id="beb2b-143">A Script.usql fájlba írja be a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="beb2b-143">Enter the following script into the Script.usql file:</span></span>

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;

        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema.
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

    <span data-ttu-id="beb2b-144">A U-SQL ismertetése: [Ismerkedés a Data Lake Analytics U-SQL nyelv](data-lake-analytics-u-sql-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-144">To understand the U-SQL, see [Get started with Data Lake Analytics U-SQL language](data-lake-analytics-u-sql-get-started.md).</span></span>    
5. <span data-ttu-id="beb2b-145">Új U-SQL parancsfájl hozzáadása a projekthez, és írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="beb2b-145">Add a new U-SQL script to your project and enter the following:</span></span>

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;

        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();
6. <span data-ttu-id="beb2b-146">Váltson vissza az első U-SQL-parancsfájlt, és mellett a **Submit** gombra, adja meg az Analytics-fiókja.</span><span class="sxs-lookup"><span data-stu-id="beb2b-146">Switch back to the first U-SQL script and next to the **Submit** button, specify your Analytics account.</span></span>
7. <span data-ttu-id="beb2b-147">A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Build Script** (Parancsfájl létrehozása) elemre.</span><span class="sxs-lookup"><span data-stu-id="beb2b-147">From **Solution Explorer**, right click **Script.usql**, and then click **Build Script**.</span></span> <span data-ttu-id="beb2b-148">Ellenőrizze az eredményeket a Tesztkimenet ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="beb2b-148">Verify the results in the Output pane.</span></span>
8. <span data-ttu-id="beb2b-149">A **Solution Explorer** eszközben kattintson a jobb gombbal a **Script.usql** fájlra, majd kattintson a **Submit Script** (Parancsfájl elküldése) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="beb2b-149">From **Solution Explorer**, right click **Script.usql**, and then click **Submit Script**.</span></span>
9. <span data-ttu-id="beb2b-150">Ellenőrizze a **Analytics-fiók** , a egy, ahol szeretné futtatni a feladatot, és kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-150">Verify the **Analytics Account** is the one where you want to run the job, and then click **Submit**.</span></span> <span data-ttu-id="beb2b-151">Az elküldés után az eredmények és a feladatra mutató hivatkozás megjelenik a Data Lake Tools for Visual Studio Eredmények ablakában.</span><span class="sxs-lookup"><span data-stu-id="beb2b-151">Submission results and job link are available in the Data Lake Tools for Visual Studio Results window when the submission is completed.</span></span>
10. <span data-ttu-id="beb2b-152">Várjon, amíg a feladat sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="beb2b-152">Wait until the job is completed successfully.</span></span>  <span data-ttu-id="beb2b-153">A feladat meghiúsult, valószínűleg hiányzik a forrásfájl.</span><span class="sxs-lookup"><span data-stu-id="beb2b-153">If the job failed, it is most likely missing the source file.</span></span>  <span data-ttu-id="beb2b-154">Lásd: a jelen oktatóanyag című cikk Előfeltételek szakaszát.</span><span class="sxs-lookup"><span data-stu-id="beb2b-154">Please see the Prerequisite section of this tutorial.</span></span> <span data-ttu-id="beb2b-155">További hibaelhárítási információért lásd: [figyelése és hibaelhárítása az Azure Data Lake Analytics-feladatok](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="beb2b-155">For additional troubleshooting information, see [Monitor and troubleshoot Azure Data Lake Analytics jobs](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md).</span></span>

    <span data-ttu-id="beb2b-156">A feladat befejezése után kell jelenik meg a következő képernyő:</span><span class="sxs-lookup"><span data-stu-id="beb2b-156">When the job is completed, you shall see the following screen:</span></span>

    ![a Data lake analytics webes naplók webhelyek naplóinak elemzése](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)
11. <span data-ttu-id="beb2b-158">Most ismételje meg a 7 – 10 a **Script1.usql**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-158">Now repeat steps 7- 10 for **Script1.usql**.</span></span>

<span data-ttu-id="beb2b-159">**Feladat kimenetének megtekintése**</span><span class="sxs-lookup"><span data-stu-id="beb2b-159">**To see the job output**</span></span>

1. <span data-ttu-id="beb2b-160">A **Server Explorer** eszközben bontsa ki az **Azure** elemet, majd a **Data Lake Analytics** elemet, bontsa ki a saját Data Lake Analytics-fiókjait, bontsa ki a **Storage Accounts** (Tárfiókok) elemet, kattintson a jobb gombbal az alapértelmezett Data Lake-tárfiókra, majd kattintson az **Explorer** elemre.</span><span class="sxs-lookup"><span data-stu-id="beb2b-160">From **Server Explorer**, expand **Azure**, expand **Data Lake Analytics**, expand your Data Lake Analytics account, expand **Storage Accounts**, right-click the default Data Lake Storage account, and then click **Explorer**.</span></span>
2. <span data-ttu-id="beb2b-161">Kattintson duplán a **minták** nyissa meg a mappát, és kattintson duplán a **kimenetek**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-161">Double-click **Samples** to open the folder, and then double-click **Outputs**.</span></span>
3. <span data-ttu-id="beb2b-162">Kattintson duplán a **UnsuccessfulResponsees.log**.</span><span class="sxs-lookup"><span data-stu-id="beb2b-162">Double-click **UnsuccessfulResponsees.log**.</span></span>
4. <span data-ttu-id="beb2b-163">Ahhoz, hogy közvetlenül a kimeneti keresse meg a kimeneti fájl belül a diagram nézet, a feladat is duplán.</span><span class="sxs-lookup"><span data-stu-id="beb2b-163">You can also double-click the output file inside the graph view of the job in order to navigate directly to the output.</span></span>

## <a name="see-also"></a><span data-ttu-id="beb2b-164">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="beb2b-164">See also</span></span>
<span data-ttu-id="beb2b-165">A Data Lake Analytics különböző eszközökkel való használatának megismeréséhez lásd:</span><span class="sxs-lookup"><span data-stu-id="beb2b-165">To get started with Data Lake Analytics using different tools, see:</span></span>

* [<span data-ttu-id="beb2b-166">A Data Lake Analytics használatának első lépései az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="beb2b-166">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="beb2b-167">A Data Lake Analytics használatának első lépései az Azure PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="beb2b-167">Get started with Data Lake Analytics using Azure PowerShell</span></span>](data-lake-analytics-get-started-powershell.md)
* [<span data-ttu-id="beb2b-168">A Data Lake Analytics használatának első lépései a .NET SDK-val</span><span class="sxs-lookup"><span data-stu-id="beb2b-168">Get started with Data Lake Analytics using .NET SDK</span></span>](data-lake-analytics-get-started-net-sdk.md)
