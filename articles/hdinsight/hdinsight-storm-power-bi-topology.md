---
title: "A Power BI - az Azure HDInsight alatt futó Apache Storm használható |} Microsoft Docs"
description: "A C#-topológiák egy hdinsight alatt futó Apache Storm-fürt futó használ az adatok Power BI-jelentés létrehozása."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 36fe3b9c-5232-4464-8d75-95403b6da7a1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/31/2017
ms.author: larryfr
ms.openlocfilehash: 36487c0c34e5a4bb955bbc15c8c96b9e838aeb44
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-power-bi-to-visualize-data-from-an-apache-storm-topology"></a><span data-ttu-id="81440-103">Az Apache Storm-topológia adatainak megjelenítése Power BI használatával</span><span class="sxs-lookup"><span data-stu-id="81440-103">Use Power BI to visualize data from an Apache Storm topology</span></span>

<span data-ttu-id="81440-104">A Power BI lehetővé teszi jelentések adatok vizuálisan megjeleníteni.</span><span class="sxs-lookup"><span data-stu-id="81440-104">Power BI allows you to visually display data as reports.</span></span> <span data-ttu-id="81440-105">Ez a dokumentum egy HDInsight alatt futó Apache Storm segítségével hozhat létre az adatokat a Power BI példát.</span><span class="sxs-lookup"><span data-stu-id="81440-105">This document provides an example of how to use Apache Storm on HDInsight to generate data for Power BI.</span></span>

> [!NOTE]
> <span data-ttu-id="81440-106">A jelen dokumentumban leírt lépések a Visual Studio Windows fejlesztői környezetre támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="81440-106">The steps in this document rely on a Windows development environment with Visual Studio.</span></span> <span data-ttu-id="81440-107">A lefordított projekt küldheti el a Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="81440-107">The compiled project can be submitted to a Linux-based HDInsight cluster.</span></span> <span data-ttu-id="81440-108">Csak Linux-alapú fürtökön után létrehozott 10/28/2016 támogatási SCP.NET topológiákat.</span><span class="sxs-lookup"><span data-stu-id="81440-108">Only Linux-based clusters created after 10/28/2016 support SCP.NET topologies.</span></span>
>
> <span data-ttu-id="81440-109">C#-topológiák használata a Linux-alapú fürtöt, frissítse a Microsoft.SCP.Net.SDK NuGet-csomagot a projekt által használt 0.10.0.6 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="81440-109">To use a C# topology with a Linux-based cluster, update the Microsoft.SCP.Net.SDK NuGet package used by your project to version 0.10.0.6 or higher.</span></span> <span data-ttu-id="81440-110">A csomag verziójának a HDInsightban telepített Storm főverziójával is egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="81440-110">The version of the package must also match the major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="81440-111">Például, a HDInsight 3.3 és 3.4 verzióján futó Storm a Storm 0.10.x verzióját használja, míg a HDInsight 3.5 a Storm 1.0.x verziót.</span><span class="sxs-lookup"><span data-stu-id="81440-111">For example, Storm on HDInsight versions 3.3 and 3.4 use Storm version 0.10.x, while HDInsight 3.5 uses Storm 1.0.x.</span></span>
>
> <span data-ttu-id="81440-112">A Linux-alapú fürtök C#-topológiáinak a .NET 4.5-öt kell használnia, és a Mono segítségével futhatnak a HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="81440-112">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono to run on the HDInsight cluster.</span></span> <span data-ttu-id="81440-113">A legtöbb dolgot működik.</span><span class="sxs-lookup"><span data-stu-id="81440-113">Most things work.</span></span> <span data-ttu-id="81440-114">Azonban ellenőrizni kell a [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.</span><span class="sxs-lookup"><span data-stu-id="81440-114">However you should check the [Mono Compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>
>
> <span data-ttu-id="81440-115">Ebben a projektben működik a Linux- vagy Windows-alapú hdinsight eszközzel, a Java-verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="81440-115">For a Java version of this project, which works with Linux-based or Windows-based HDInsight, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="81440-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81440-116">Prerequisites</span></span>

* <span data-ttu-id="81440-117">Egy Azure Active Directory-felhasználó a [Power BI](https://powerbi.com) hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="81440-117">An Azure Active Directory user with [Power BI](https://powerbi.com) access.</span></span>
* <span data-ttu-id="81440-118">HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="81440-118">An HDInsight cluster.</span></span> <span data-ttu-id="81440-119">További információkért lásd: [beolvasása használatába a HDInsight alatt futó Storm](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="81440-119">For more information, see [Get started with Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="81440-120">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="81440-120">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="81440-121">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="81440-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="81440-122">A Visual Studio (a következő verziók egyike)</span><span class="sxs-lookup"><span data-stu-id="81440-122">Visual Studio (one of the following versions)</span></span>

  * <span data-ttu-id="81440-123">A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="81440-123">Visual Studio 2012 with [update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>
  * <span data-ttu-id="81440-124">A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="81440-124">Visual Studio 2013 with [update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span></span>
  * [<span data-ttu-id="81440-125">A Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="81440-125">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * <span data-ttu-id="81440-126">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="81440-126">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="81440-127">A HDInsight Tools for Visual Studio: lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) információt a telepítési információkat.</span><span class="sxs-lookup"><span data-stu-id="81440-127">The HDInsight Tools for Visual Studio: See [Get started using the HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installation information.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="81440-128">Működés</span><span class="sxs-lookup"><span data-stu-id="81440-128">How it works</span></span>

<span data-ttu-id="81440-129">Ebben a példában a C# Storm-topológia véletlenszerűen előállított az Internet Information Services (IIS) naplóadatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="81440-129">This example contains a C# Storm topology that randomly generates Internet Information Services (IIS) log data.</span></span> <span data-ttu-id="81440-130">Ezek az adatok majd írni egy SQL-adatbázis, és az ott szolgál a Power BI-jelentések készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="81440-130">This data is then written to a SQL Database, and from there it is used to generate reports in Power BI.</span></span>

<span data-ttu-id="81440-131">A következő fájlok valósítja meg a jelen példában a fő funkciókat:</span><span class="sxs-lookup"><span data-stu-id="81440-131">The following files implement the main functionality of this example:</span></span>

* <span data-ttu-id="81440-132">**SqlAzureBolt.cs**: az SQL Database Storm-topológia előállított adatokat ír az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="81440-132">**SqlAzureBolt.cs**: Writes information produced in the Storm topology to SQL Database.</span></span>
* <span data-ttu-id="81440-133">**IISLogsTable.sql**: az adatbázis tárolt adatok létrehozásához használja a Transact-SQL utasítások.</span><span class="sxs-lookup"><span data-stu-id="81440-133">**IISLogsTable.sql**: The Transact-SQL statements used to generate the database that the data is stored in.</span></span>

> [!WARNING]
> <span data-ttu-id="81440-134">A tábla létrehozása az SQL-adatbázis a topológia a HDInsight-fürt megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="81440-134">Create the table in SQL Database before starting the topology on your HDInsight cluster.</span></span>

## <a name="download-the-example"></a><span data-ttu-id="81440-135">A példa letöltése</span><span class="sxs-lookup"><span data-stu-id="81440-135">Download the example</span></span>

<span data-ttu-id="81440-136">Töltse le a [HDInsight C# Storm a Power BI példa](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span><span class="sxs-lookup"><span data-stu-id="81440-136">Download the [HDInsight C# Storm Power BI example](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span></span> <span data-ttu-id="81440-137">Letöltheti, vagy elágazás/Klónozás használatával [git](http://git-scm.com/), vagy használja a **letöltése** egy .zip az archívum letöltésére mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="81440-137">To download it, either fork/clone it using [git](http://git-scm.com/), or use the **Download** link to download a .zip of the archive.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="81440-138">Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="81440-138">Create a database</span></span>

1. <span data-ttu-id="81440-139">Hozzon létre egy adatbázist, használja a lépéseket a [SQL Database oktatóanyag](../sql-database/sql-database-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="81440-139">To create a database, use the steps in the [SQL Database tutorial](../sql-database/sql-database-get-started.md) document.</span></span>

2. <span data-ttu-id="81440-140">Az adatbázis lépéseit követve csatlakozhat a [Csatlakozás SQL adatbázishoz a Visual Studio](../sql-database/sql-database-connect-query.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="81440-140">Connect to the database by following the steps in the [Connect to a SQL Database with Visual Studio](../sql-database/sql-database-connect-query.md) document.</span></span>

3. <span data-ttu-id="81440-141">Az Object Explorer ablaktáblában kattintson a jobb gombbal az adatbázist, és válassza ki **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="81440-141">In Object Explorer, right-click the database and select  **New Query**.</span></span> <span data-ttu-id="81440-142">Tartalmának beillesztése a **IISLogsTable.sql** fájlt a letöltött projektből szerepel a lekérdezési ablakba, és majd használja a Ctrl + Shift + E végrehajtsák a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="81440-142">Paste the contents of the **IISLogsTable.sql** file included in the downloaded project into the query window, and then use Ctrl + Shift + E to execute the query.</span></span> <span data-ttu-id="81440-143">A parancsok sikeresen befejeződött a következő üzenetet kell látnia.</span><span class="sxs-lookup"><span data-stu-id="81440-143">You should receive a message that the commands completed successfully.</span></span>

## <a name="configure-the-sample"></a><span data-ttu-id="81440-144">A minta konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81440-144">Configure the sample</span></span>

1. <span data-ttu-id="81440-145">Az a [Azure-portálon](https://portal.azure.com), válassza ki az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="81440-145">From the [Azure portal](https://portal.azure.com), select your SQL database.</span></span> <span data-ttu-id="81440-146">Az a **Essentials** SQL adatbázis paneljén válassza szakasza **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="81440-146">From the **Essentials** section of the SQL database blade, select **Show database connection strings**.</span></span> <span data-ttu-id="81440-147">A megjelenő listában másolja a **ADO.NET (SQL-hitelesítés)** információkat.</span><span class="sxs-lookup"><span data-stu-id="81440-147">From the list that appears, copy the **ADO.NET (SQL authentication)** information.</span></span>

2. <span data-ttu-id="81440-148">Nyissa meg a minta a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="81440-148">Open the sample in Visual Studio.</span></span> <span data-ttu-id="81440-149">A **Megoldáskezelőben**, nyissa meg a **App.config** fájlt, és keresse meg a következő bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="81440-149">From **Solution Explorer**, open the **App.config** file, and then find the following entry:</span></span>

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    <span data-ttu-id="81440-150">Cserélje le a **TOBEFILLED ##** az előző lépésben másolt érték és az adatbázis-kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="81440-150">Replace the **##TOBEFILLED##** value with the database connection string copied in the previous step.</span></span> <span data-ttu-id="81440-151">Cserélje le **{a\_felhasználónév}** és **{a\_jelszó}** a felhasználónevet és jelszót az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="81440-151">Replace **{your\_username}** and **{your\_password}** with the username and password for the database.</span></span>

3. <span data-ttu-id="81440-152">Mentse és zárja be a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="81440-152">Save and close the files.</span></span>

## <a name="deploy-the-sample"></a><span data-ttu-id="81440-153">A minta telepítése</span><span class="sxs-lookup"><span data-stu-id="81440-153">Deploy the sample</span></span>

1. <span data-ttu-id="81440-154">A **Megoldáskezelőben**, kattintson a jobb gombbal a **StormToSQL** projektre, és válassza ki **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="81440-154">From **Solution Explorer**, right-click the **StormToSQL** project and select **Submit to Storm on HDInsight**.</span></span> <span data-ttu-id="81440-155">Válassza ki a HDInsight-fürt a **Storm-fürt** legördülő párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="81440-155">Select the HDInsight cluster from the **Storm Cluster** dropdown dialog.</span></span>

   > [!NOTE]
   > <span data-ttu-id="81440-156">Néhány másodpercet vehet igénybe a **Storm-fürt** legördülő kiszolgálónevekkel feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="81440-156">It may take a few seconds for the **Storm Cluster** dropdown to populate with server names.</span></span>
   >
   > <span data-ttu-id="81440-157">Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatok az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="81440-157">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="81440-158">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be, amely tartalmazza a Storm on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="81440-158">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="81440-159">A topológia elküldésekor a __topológia Viewer__ jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="81440-159">When the topology has been submitted, the __Topology Viewer__ appears.</span></span> <span data-ttu-id="81440-160">Ez a topológia megtekintéséhez jelölje ki a SqlAzureWriterTopology bejegyzést a listából.</span><span class="sxs-lookup"><span data-stu-id="81440-160">To view this topology, select the SqlAzureWriterTopology entry from the list.</span></span>

    ![A topológia esetén a kiválasztott topológia](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    <span data-ttu-id="81440-162">A topológia információk megjelenítéséhez használja ezt a nézetet, vagy a topológia egy összetevő vonatkozó információk megjelenítéséhez kattintson duplán a (például a SqlAzureBolt) bejegyzése.</span><span class="sxs-lookup"><span data-stu-id="81440-162">You can use this view to see information on the topology, or double-click an entry (such as the SqlAzureBolt) to see information specific to a component in the topology.</span></span>

3. <span data-ttu-id="81440-163">Után a topológia rendelkezik néhány percig, lépjen vissza az adatbázis létrehozásához használt SQL-lekérdezés ablak futott.</span><span class="sxs-lookup"><span data-stu-id="81440-163">After the topology has ran for a few minutes, return to the SQL query window you used to create the database.</span></span> <span data-ttu-id="81440-164">Cserélje le a meglévő utasítások a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="81440-164">Replace the existing statements with the following query:</span></span>

        select * from iislogs;

    <span data-ttu-id="81440-165">Használja a Ctrl + Shift + E végrehajtani a lekérdezést, és eredmények eléréséhez a következő adatokat kell kapnia:</span><span class="sxs-lookup"><span data-stu-id="81440-165">Use Ctrl + Shift + E to execute the query, and you should receive results similar to the following data:</span></span>

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    <span data-ttu-id="81440-166">Ezeket az adatokat a Storm-topológia a van írva.</span><span class="sxs-lookup"><span data-stu-id="81440-166">This data has been written from the Storm topology.</span></span>

## <a name="create-a-report"></a><span data-ttu-id="81440-167">Jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="81440-167">Create a report</span></span>

1. <span data-ttu-id="81440-168">Csatlakozás a [Azure SQL Database összekötő](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) a Power BI.</span><span class="sxs-lookup"><span data-stu-id="81440-168">Connect to the [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) for Power BI.</span></span> 

2. <span data-ttu-id="81440-169">Belül **adatbázisok**, jelölje be **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="81440-169">Within **Databases**, select **Get**.</span></span>

3. <span data-ttu-id="81440-170">Válassza ki **Azure SQL Database**, majd válassza ki **Connect**.</span><span class="sxs-lookup"><span data-stu-id="81440-170">Select **Azure SQL Database**, and then select **Connect**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="81440-171">A folytatáshoz a Power BI Desktop letöltése kérheti.</span><span class="sxs-lookup"><span data-stu-id="81440-171">You may be asked to download the Power BI Desktop to continue.</span></span> <span data-ttu-id="81440-172">Ha igen, tegye a következőket való csatlakozáshoz:</span><span class="sxs-lookup"><span data-stu-id="81440-172">If so, use the following steps to connect:</span></span>
    >
    > 1. <span data-ttu-id="81440-173">Nyissa meg a Power BI Desktopban, és válassza ki __adatok beolvasása__.</span><span class="sxs-lookup"><span data-stu-id="81440-173">Open Power BI Desktop and select __Get Data__.</span></span>
    > <span data-ttu-id="81440-174">2 válassza __Azure__, majd __Azure SQL adatbázis__.</span><span class="sxs-lookup"><span data-stu-id="81440-174">2  Select __Azure__, and then __Azure SQL database__.</span></span>

4. <span data-ttu-id="81440-175">Adja meg az adatokat az Azure SQL Database adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="81440-175">Enter the information to connect to your Azure SQL Database.</span></span> <span data-ttu-id="81440-176">Ezt az információt találja látogasson el a [Azure-portálon](https://portal.azure.com) és az SQL-adatbázis kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="81440-176">You can find this information by visiting the [Azure portal](https://portal.azure.com) and selecting your SQL database.</span></span>

   > [!NOTE]
   > <span data-ttu-id="81440-177">Is beállíthatja a frissítési időköz és egyéni szűrők használatával **speciális beállítások engedélyezése** a connect párbeszédpanelről.</span><span class="sxs-lookup"><span data-stu-id="81440-177">You can also set the refresh interval and custom filters by using **Enable Advanced Options** from the connect dialog.</span></span>

5. <span data-ttu-id="81440-178">Miután csatlakozott, látni fogja az azonos nevű új adatkészlet csatlakozva-adatbázisként.</span><span class="sxs-lookup"><span data-stu-id="81440-178">After you've connected, you will see a new dataset with the same name as the database you connected to.</span></span> <span data-ttu-id="81440-179">Válassza ki a jelentés elkezd adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="81440-179">Select the dataset to begin designing a report.</span></span>

6. <span data-ttu-id="81440-180">A **mezők**, bontsa ki a **IISLOGS** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="81440-180">From **Fields**, expand the **IISLOGS** entry.</span></span> <span data-ttu-id="81440-181">Az URI szárakat felsoroló jelentés létrehozásához jelölje be a **lévő egyedi AZONOSÍTÓHOZ**.</span><span class="sxs-lookup"><span data-stu-id="81440-181">To create a report that lists the URI stems, select the checkbox for **URISTEM**.</span></span>

    ![A jelentés létrehozása](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. <span data-ttu-id="81440-183">A következő húzza **METÓDUS** a jelentéshez.</span><span class="sxs-lookup"><span data-stu-id="81440-183">Next, drag **METHOD** to the report.</span></span> <span data-ttu-id="81440-184">A jelentés frissítése a szárakat és a megfelelő használja a HTTP-kérelem HTTP-metódus.</span><span class="sxs-lookup"><span data-stu-id="81440-184">The report updates to list the stems and the corresponding HTTP method used for the HTTP request.</span></span>

    ![a metódus adatok hozzáadása](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. <span data-ttu-id="81440-186">Az a **képi megjelenítések** oszlopból válassza ki a **mezők** ikonra, majd jelölje be a lefelé mutató nyílra, és **METÓDUS** a a **értékek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="81440-186">From the **Visualizations** column, select the **Fields** icon, and then select the down arrow next to **METHOD** in the **Values** section.</span></span> <span data-ttu-id="81440-187">Megjeleníti az URI elérése hány alkalommal számát, jelölje be **száma**.</span><span class="sxs-lookup"><span data-stu-id="81440-187">To display a count of how many times a URI has been accessed, select **Count**.</span></span>

    ![A metódusok számát módosítása](./media/hdinsight-storm-power-bi-topology/count.png)

9. <span data-ttu-id="81440-189">Ezután válassza ki a **halmozott oszlopdiagram** az információk megjelenítésének módosítása.</span><span class="sxs-lookup"><span data-stu-id="81440-189">Next, select the **Stacked column chart** to change how the information is displayed.</span></span>

    ![Váltás halmozott diagram](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. <span data-ttu-id="81440-191">Mentse a jelentést, jelölje be **mentése** , és adja meg a jelentés nevét.</span><span class="sxs-lookup"><span data-stu-id="81440-191">To save the report, select **Save** and enter a name for the report.</span></span>

## <a name="stop-the-topology"></a><span data-ttu-id="81440-192">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="81440-192">Stop the topology</span></span>

<span data-ttu-id="81440-193">A topológia továbbra is futni megállíthatja vagy a Storm on HDInsight-fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="81440-193">The topology continues to run until you stop it or delete the Storm on HDInsight cluster.</span></span> <span data-ttu-id="81440-194">A topológia leállítása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="81440-194">To stop the topology, perform the following steps:</span></span>

1. <span data-ttu-id="81440-195">A Visual Studióban lépjen vissza a topológia megjelenítő, és válassza ki a topológiát.</span><span class="sxs-lookup"><span data-stu-id="81440-195">In Visual Studio, return to the topology viewer and select the topology.</span></span>

2. <span data-ttu-id="81440-196">Válassza ki a **Kill** a topológia leállítása gombra.</span><span class="sxs-lookup"><span data-stu-id="81440-196">Select the **Kill** button to stop the topology.</span></span>

    ![A topológia összefoglaló gombjára leállítása](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="81440-198">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="81440-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="81440-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81440-199">Next steps</span></span>

<span data-ttu-id="81440-200">Ebben a dokumentumban megtanulhatta a Storm-topológia adatokat küldeni az SQL-adatbázis, majd a Power BI használatával adatainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="81440-200">In this document, you learned how to send data from a Storm topology to SQL Database, then visualize the data using Power BI.</span></span> <span data-ttu-id="81440-201">Az egyéb Azure hdinsight alatt futó Storm használatával technológiák használata információkért lásd: a következő dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="81440-201">For information on how to work with other Azure technologies using Storm on HDInsight, see the following document:</span></span>

* [<span data-ttu-id="81440-202">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="81440-202">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
