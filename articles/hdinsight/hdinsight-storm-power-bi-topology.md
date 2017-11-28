---
title: "a Power BI - Azure HDInsight alatt futó Apache Storm aaaUse |} Microsoft Docs"
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
ms.openlocfilehash: 194cd8220bd60475af1d64a110e4b23ef92e1bc8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-power-bi-toovisualize-data-from-an-apache-storm-topology"></a><span data-ttu-id="e96b4-103">A Power BI toovisualize adatait használhatja az Apache Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="e96b4-103">Use Power BI toovisualize data from an Apache Storm topology</span></span>

<span data-ttu-id="e96b4-104">A Power BI lehetővé teszi toovisually adatok megjelenítése jelentésként.</span><span class="sxs-lookup"><span data-stu-id="e96b4-104">Power BI allows you toovisually display data as reports.</span></span> <span data-ttu-id="e96b4-105">Ez a dokumentum nyújt arról, hogyan toouse alatt futó Apache Storm a Power BI HDInsight toogenerate adatokon.</span><span class="sxs-lookup"><span data-stu-id="e96b4-105">This document provides an example of how toouse Apache Storm on HDInsight toogenerate data for Power BI.</span></span>

> [!NOTE]
> <span data-ttu-id="e96b4-106">hello jelen dokumentumban leírt lépések használja a Visual Studio a Windows fejlesztői környezetre.</span><span class="sxs-lookup"><span data-stu-id="e96b4-106">hello steps in this document rely on a Windows development environment with Visual Studio.</span></span> <span data-ttu-id="e96b4-107">hello lefordított projekt lehet elküldött tooa Linux-alapú HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e96b4-107">hello compiled project can be submitted tooa Linux-based HDInsight cluster.</span></span> <span data-ttu-id="e96b4-108">Csak Linux-alapú fürtökön után létrehozott 10/28/2016 támogatási SCP.NET topológiákat.</span><span class="sxs-lookup"><span data-stu-id="e96b4-108">Only Linux-based clusters created after 10/28/2016 support SCP.NET topologies.</span></span>
>
> <span data-ttu-id="e96b4-109">toouse C#-topológiák egy Linux-alapú fürttel, frissítés hello Microsoft.SCP.Net.SDK NuGet-csomagot a projekt tooversion 0.10.0.6 által használt vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="e96b4-109">toouse a C# topology with a Linux-based cluster, update hello Microsoft.SCP.Net.SDK NuGet package used by your project tooversion 0.10.0.6 or higher.</span></span> <span data-ttu-id="e96b4-110">hello csomag hello verziójának is egyeznie kell telepíteni a HDInsight alatt futó Storm hello főverziója.</span><span class="sxs-lookup"><span data-stu-id="e96b4-110">hello version of hello package must also match hello major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="e96b4-111">Például, a HDInsight 3.3 és 3.4 verzióján futó Storm a Storm 0.10.x verzióját használja, míg a HDInsight 3.5 a Storm 1.0.x verziót.</span><span class="sxs-lookup"><span data-stu-id="e96b4-111">For example, Storm on HDInsight versions 3.3 and 3.4 use Storm version 0.10.x, while HDInsight 3.5 uses Storm 1.0.x.</span></span>
>
> <span data-ttu-id="e96b4-112">C#-topológiák Linux-alapú fürtökön kell használni a .NET 4.5, és monó toorun hello HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="e96b4-112">C# topologies on Linux-based clusters must use .NET 4.5, and use Mono toorun on hello HDInsight cluster.</span></span> <span data-ttu-id="e96b4-113">A legtöbb dolgot működik.</span><span class="sxs-lookup"><span data-stu-id="e96b4-113">Most things work.</span></span> <span data-ttu-id="e96b4-114">Azonban célszerű lenne ellenőrizni hello [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.</span><span class="sxs-lookup"><span data-stu-id="e96b4-114">However you should check hello [Mono Compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>
>
> <span data-ttu-id="e96b4-115">Ebben a projektben működik a Linux- vagy Windows-alapú hdinsight eszközzel, a Java-verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="e96b4-115">For a Java version of this project, which works with Linux-based or Windows-based HDInsight, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e96b4-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e96b4-116">Prerequisites</span></span>

* <span data-ttu-id="e96b4-117">Egy Azure Active Directory-felhasználó a [Power BI](https://powerbi.com) hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="e96b4-117">An Azure Active Directory user with [Power BI](https://powerbi.com) access.</span></span>
* <span data-ttu-id="e96b4-118">HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e96b4-118">An HDInsight cluster.</span></span> <span data-ttu-id="e96b4-119">További információkért lásd: [beolvasása használatába a HDInsight alatt futó Storm](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="e96b4-119">For more information, see [Get started with Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="e96b4-120">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="e96b4-120">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e96b4-121">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e96b4-121">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="e96b4-122">A Visual Studio (hello a következő verziók egyike)</span><span class="sxs-lookup"><span data-stu-id="e96b4-122">Visual Studio (one of hello following versions)</span></span>

  * <span data-ttu-id="e96b4-123">A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="e96b4-123">Visual Studio 2012 with [update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>
  * <span data-ttu-id="e96b4-124">A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="e96b4-124">Visual Studio 2013 with [update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?linkid=517284&clcid=0x409)</span></span>
  * [<span data-ttu-id="e96b4-125">A Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="e96b4-125">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
  * <span data-ttu-id="e96b4-126">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="e96b4-126">Visual Studio 2017 (any edition)</span></span>

* <span data-ttu-id="e96b4-127">a HDInsight Tools for Visual Studio hello: lásd: [Ismerkedés hello HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) információt a telepítési információkat.</span><span class="sxs-lookup"><span data-stu-id="e96b4-127">hello HDInsight Tools for Visual Studio: See [Get started using hello HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) for information on installation information.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="e96b4-128">Működés</span><span class="sxs-lookup"><span data-stu-id="e96b4-128">How it works</span></span>

<span data-ttu-id="e96b4-129">Ebben a példában a C# Storm-topológia véletlenszerűen előállított az Internet Information Services (IIS) naplóadatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e96b4-129">This example contains a C# Storm topology that randomly generates Internet Information Services (IIS) log data.</span></span> <span data-ttu-id="e96b4-130">Ezek az adatok majd írása tooa SQL-adatbázis, és ott használt toogenerate jelentéseket a Power bi-ban.</span><span class="sxs-lookup"><span data-stu-id="e96b4-130">This data is then written tooa SQL Database, and from there it is used toogenerate reports in Power BI.</span></span>

<span data-ttu-id="e96b4-131">a következő fájlok megvalósítása hello fő funkcióit a jelen példában hello:</span><span class="sxs-lookup"><span data-stu-id="e96b4-131">hello following files implement hello main functionality of this example:</span></span>

* <span data-ttu-id="e96b4-132">**SqlAzureBolt.cs**: hello Storm-topológia tooSQL adatbázis előállított adatokat ír az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e96b4-132">**SqlAzureBolt.cs**: Writes information produced in hello Storm topology tooSQL Database.</span></span>
* <span data-ttu-id="e96b4-133">**IISLogsTable.sql**: hello Transact-SQL utasítás használt toogenerate hello adatbázis, amely hello adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="e96b4-133">**IISLogsTable.sql**: hello Transact-SQL statements used toogenerate hello database that hello data is stored in.</span></span>

> [!WARNING]
> <span data-ttu-id="e96b4-134">Hello tábla létrehozása az SQL-adatbázis a HDInsight-fürt hello topológia megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="e96b4-134">Create hello table in SQL Database before starting hello topology on your HDInsight cluster.</span></span>

## <a name="download-hello-example"></a><span data-ttu-id="e96b4-135">Töltse le a hello – példa</span><span class="sxs-lookup"><span data-stu-id="e96b4-135">Download hello example</span></span>

<span data-ttu-id="e96b4-136">Töltse le a hello [HDInsight C# Storm a Power BI példa](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span><span class="sxs-lookup"><span data-stu-id="e96b4-136">Download hello [HDInsight C# Storm Power BI example](https://github.com/Azure-Samples/hdinsight-dotnet-storm-powerbi).</span></span> <span data-ttu-id="e96b4-137">toodownload, vagy elágazás/Klónozás használatával [git](http://git-scm.com/), vagy használjon hello **letöltése** hivatkozás toodownload egy .zip hello archívum.</span><span class="sxs-lookup"><span data-stu-id="e96b4-137">toodownload it, either fork/clone it using [git](http://git-scm.com/), or use hello **Download** link toodownload a .zip of hello archive.</span></span>

## <a name="create-a-database"></a><span data-ttu-id="e96b4-138">Adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="e96b4-138">Create a database</span></span>

1. <span data-ttu-id="e96b4-139">egy adatbázis toocreate lépésekkel hello a hello [SQL Database oktatóanyag](../sql-database/sql-database-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="e96b4-139">toocreate a database, use hello steps in hello [SQL Database tutorial](../sql-database/sql-database-get-started.md) document.</span></span>

2. <span data-ttu-id="e96b4-140">Csatlakozás toohello adatbázis által következő hello szükséges lépések hello [tooa SQL-adatbázis csatlakozzon a Visual Studio](../sql-database/sql-database-connect-query.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="e96b4-140">Connect toohello database by following hello steps in hello [Connect tooa SQL Database with Visual Studio](../sql-database/sql-database-connect-query.md) document.</span></span>

3. <span data-ttu-id="e96b4-141">Az Object Explorer ablaktáblában kattintson a jobb gombbal a hello adatbázis, és válassza ki **új lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-141">In Object Explorer, right-click hello database and select  **New Query**.</span></span> <span data-ttu-id="e96b4-142">Illessze be a hello hello tartalmát **IISLogsTable.sql** hello fájl letöltése hello lekérdezési ablakba, és a Ctrl + Shift + E tooexecute hello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="e96b4-142">Paste hello contents of hello **IISLogsTable.sql** file included in hello downloaded project into hello query window, and then use Ctrl + Shift + E tooexecute hello query.</span></span> <span data-ttu-id="e96b4-143">Egy üzenet, amely a parancs sikeresen végrehajtva hello kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="e96b4-143">You should receive a message that hello commands completed successfully.</span></span>

## <a name="configure-hello-sample"></a><span data-ttu-id="e96b4-144">Hello minta konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e96b4-144">Configure hello sample</span></span>

1. <span data-ttu-id="e96b4-145">A hello [Azure-portálon](https://portal.azure.com), válassza ki az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e96b4-145">From hello [Azure portal](https://portal.azure.com), select your SQL database.</span></span> <span data-ttu-id="e96b4-146">A hello **Essentials** hello SQL adatbázis panelen válassza szakasza **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-146">From hello **Essentials** section of hello SQL database blade, select **Show database connection strings**.</span></span> <span data-ttu-id="e96b4-147">Hello listában megjelenő, másolja a hello **ADO.NET (SQL-hitelesítés)** információkat.</span><span class="sxs-lookup"><span data-stu-id="e96b4-147">From hello list that appears, copy hello **ADO.NET (SQL authentication)** information.</span></span>

2. <span data-ttu-id="e96b4-148">Nyissa meg a Visual Studio hello minta.</span><span class="sxs-lookup"><span data-stu-id="e96b4-148">Open hello sample in Visual Studio.</span></span> <span data-ttu-id="e96b4-149">A **Megoldáskezelőben**, nyissa meg hello **App.config** fájlt, és keresse meg a következő bejegyzés hello:</span><span class="sxs-lookup"><span data-stu-id="e96b4-149">From **Solution Explorer**, open hello **App.config** file, and then find hello following entry:</span></span>

        <add key="SqlAzureConnectionString" value="##TOBEFILLED##" />

    <span data-ttu-id="e96b4-150">Cserélje le a hello **TOBEFILLED ##** hello előző lépésben másolt érték hello adatbázis kapcsolati karakterlánccal.</span><span class="sxs-lookup"><span data-stu-id="e96b4-150">Replace hello **##TOBEFILLED##** value with hello database connection string copied in hello previous step.</span></span> <span data-ttu-id="e96b4-151">Cserélje le **{a\_felhasználónév}** és **{a\_jelszó}** hello felhasználónévvel és jelszóval hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="e96b4-151">Replace **{your\_username}** and **{your\_password}** with hello username and password for hello database.</span></span>

3. <span data-ttu-id="e96b4-152">Mentse és zárja be a hello fájlok.</span><span class="sxs-lookup"><span data-stu-id="e96b4-152">Save and close hello files.</span></span>

## <a name="deploy-hello-sample"></a><span data-ttu-id="e96b4-153">Hello mintaalkalmazás telepítését</span><span class="sxs-lookup"><span data-stu-id="e96b4-153">Deploy hello sample</span></span>

1. <span data-ttu-id="e96b4-154">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **StormToSQL** projektre, és válassza ki **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-154">From **Solution Explorer**, right-click hello **StormToSQL** project and select **Submit tooStorm on HDInsight**.</span></span> <span data-ttu-id="e96b4-155">SELECT hello HDInsight fürt hello **Storm-fürt** legördülő párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e96b4-155">Select hello HDInsight cluster from hello **Storm Cluster** dropdown dialog.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e96b4-156">A hello néhány másodpercet vehet igénybe **Storm-fürt** legördülő toopopulate kiszolgálónevekkel.</span><span class="sxs-lookup"><span data-stu-id="e96b4-156">It may take a few seconds for hello **Storm Cluster** dropdown toopopulate with server names.</span></span>
   >
   > <span data-ttu-id="e96b4-157">Ha a rendszer kéri, adja meg hello bejelentkezési hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e96b4-157">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="e96b4-158">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.</span><span class="sxs-lookup"><span data-stu-id="e96b4-158">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

2. <span data-ttu-id="e96b4-159">Hello topológia elküldésekor hello __topológia Viewer__ jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e96b4-159">When hello topology has been submitted, hello __Topology Viewer__ appears.</span></span> <span data-ttu-id="e96b4-160">tooview a Kiszolgálótopológia, a select hello SqlAzureWriterTopology bejegyzést hello listából.</span><span class="sxs-lookup"><span data-stu-id="e96b4-160">tooview this topology, select hello SqlAzureWriterTopology entry from hello list.</span></span>

    ![hello topológiák kijelölt hello topológia](./media/hdinsight-storm-power-bi-topology/topologyview.png)

    <span data-ttu-id="e96b4-162">Használja a nézet toosee információk hello topológia, vagy kattintson duplán egy bejegyzés (például hello SqlAzureBolt) toosee információt adott tooa összetevő hello topológiában.</span><span class="sxs-lookup"><span data-stu-id="e96b4-162">You can use this view toosee information on hello topology, or double-click an entry (such as hello SqlAzureBolt) toosee information specific tooa component in hello topology.</span></span>

3. <span data-ttu-id="e96b4-163">Hello után topológia rendelkezik futott néhány percig, visszatérési toohello SQL lekérdezési ablakba toocreate hello adatbázis használta.</span><span class="sxs-lookup"><span data-stu-id="e96b4-163">After hello topology has ran for a few minutes, return toohello SQL query window you used toocreate hello database.</span></span> <span data-ttu-id="e96b4-164">Cserélje le a következő lekérdezés hello hello meglévő utasításokat:</span><span class="sxs-lookup"><span data-stu-id="e96b4-164">Replace hello existing statements with hello following query:</span></span>

        select * from iislogs;

    <span data-ttu-id="e96b4-165">Használja a Ctrl + Shift + E tooexecute hello lekérdezést, és meg kell kapnia eredmények hasonló toohello a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="e96b4-165">Use Ctrl + Shift + E tooexecute hello query, and you should receive results similar toohello following data:</span></span>

        1    2016-05-27 17:57:14.797    255.255.255.255    /bar    GET    200
        2    2016-05-27 17:57:14.843    127.0.0.1    /spam/eggs    POST    500
        3    2016-05-27 17:57:14.850    123.123.123.123    /eggs    DELETE    200
        4    2016-05-27 17:57:14.853    127.0.0.1    /foo    POST    404
        5    2016-05-27 17:57:14.853    10.9.8.7    /bar    GET    200
        6    2016-05-27 17:57:14.857    192.168.1.1    /spam    DELETE    200

    <span data-ttu-id="e96b4-166">Ezeket az adatokat a Storm-topológia hello van írva.</span><span class="sxs-lookup"><span data-stu-id="e96b4-166">This data has been written from hello Storm topology.</span></span>

## <a name="create-a-report"></a><span data-ttu-id="e96b4-167">Jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="e96b4-167">Create a report</span></span>

1. <span data-ttu-id="e96b4-168">Csatlakozás toohello [Azure SQL Database összekötő](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) a Power BI.</span><span class="sxs-lookup"><span data-stu-id="e96b4-168">Connect toohello [Azure SQL Database connector](https://app.powerbi.com/getdata/bigdata/azure-sql-database-with-live-connect) for Power BI.</span></span> 

2. <span data-ttu-id="e96b4-169">Belül **adatbázisok**, jelölje be **beolvasása**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-169">Within **Databases**, select **Get**.</span></span>

3. <span data-ttu-id="e96b4-170">Válassza ki **Azure SQL Database**, majd válassza ki **Connect**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-170">Select **Azure SQL Database**, and then select **Connect**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e96b4-171">Előfordulhat, hogy toodownload hello Power BI Desktop toocontinue kéri.</span><span class="sxs-lookup"><span data-stu-id="e96b4-171">You may be asked toodownload hello Power BI Desktop toocontinue.</span></span> <span data-ttu-id="e96b4-172">Ha igen, használja a következő lépéseket tooconnect hello:</span><span class="sxs-lookup"><span data-stu-id="e96b4-172">If so, use hello following steps tooconnect:</span></span>
    >
    > 1. <span data-ttu-id="e96b4-173">Nyissa meg a Power BI Desktopban, és válassza ki __adatok beolvasása__.</span><span class="sxs-lookup"><span data-stu-id="e96b4-173">Open Power BI Desktop and select __Get Data__.</span></span>
    > <span data-ttu-id="e96b4-174">2 válassza __Azure__, majd __Azure SQL adatbázis__.</span><span class="sxs-lookup"><span data-stu-id="e96b4-174">2  Select __Azure__, and then __Azure SQL database__.</span></span>

4. <span data-ttu-id="e96b4-175">Adja meg a hello információk tooconnect tooyour Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="e96b4-175">Enter hello information tooconnect tooyour Azure SQL Database.</span></span> <span data-ttu-id="e96b4-176">Ezt az információt találja hello felkeresésével [Azure-portálon](https://portal.azure.com) és az SQL-adatbázis kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="e96b4-176">You can find this information by visiting hello [Azure portal](https://portal.azure.com) and selecting your SQL database.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e96b4-177">Emellett beállíthatja hello frissítési időköz és egyéni szűrők használatával **speciális beállítások engedélyezése** hello a csatlakozás a párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="e96b4-177">You can also set hello refresh interval and custom filters by using **Enable Advanced Options** from hello connect dialog.</span></span>

5. <span data-ttu-id="e96b4-178">Miután csatlakozott, megjelenik egy új adatkészlet hello azonos nevét hello adatbázisként, csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="e96b4-178">After you've connected, you will see a new dataset with hello same name as hello database you connected to.</span></span> <span data-ttu-id="e96b4-179">Válassza ki a hello dataset toobegin jelentés tervezéséhez.</span><span class="sxs-lookup"><span data-stu-id="e96b4-179">Select hello dataset toobegin designing a report.</span></span>

6. <span data-ttu-id="e96b4-180">A **mezők**, bontsa ki a hello **IISLOGS** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="e96b4-180">From **Fields**, expand hello **IISLOGS** entry.</span></span> <span data-ttu-id="e96b4-181">toocreate egy jelentést, hogy listák hello URI-törzs, válassza ki a hello jelölőnégyzetét **lévő egyedi AZONOSÍTÓHOZ**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-181">toocreate a report that lists hello URI stems, select hello checkbox for **URISTEM**.</span></span>

    ![A jelentés létrehozása](./media/hdinsight-storm-power-bi-topology/createreport.png)

7. <span data-ttu-id="e96b4-183">A következő húzza **METÓDUS** toohello jelentés.</span><span class="sxs-lookup"><span data-stu-id="e96b4-183">Next, drag **METHOD** toohello report.</span></span> <span data-ttu-id="e96b4-184">hello jelentés frissítések toolist hello törzs, és HTTP-kérelem hello hello megfelelő HTTP-metódus használható.</span><span class="sxs-lookup"><span data-stu-id="e96b4-184">hello report updates toolist hello stems and hello corresponding HTTP method used for hello HTTP request.</span></span>

    ![hello metódus adatok hozzáadása](./media/hdinsight-storm-power-bi-topology/uristemandmethod.png)

8. <span data-ttu-id="e96b4-186">A hello **képi megjelenítések** oszlop, jelölje be hello **mezők** ikonra, és válassza hello lefelé mutató nyíl mellett túl**METÓDUS** a hello **értékek**szakasz.</span><span class="sxs-lookup"><span data-stu-id="e96b4-186">From hello **Visualizations** column, select hello **Fields** icon, and then select hello down arrow next too**METHOD** in hello **Values** section.</span></span> <span data-ttu-id="e96b4-187">toodisplay hány alkalommal URI számát elérése, válassza ki **száma**.</span><span class="sxs-lookup"><span data-stu-id="e96b4-187">toodisplay a count of how many times a URI has been accessed, select **Count**.</span></span>

    ![Módszerek tooa számának módosítása](./media/hdinsight-storm-power-bi-topology/count.png)

9. <span data-ttu-id="e96b4-189">Ezután válassza ki a hello **halmozott oszlopdiagram** toochange hogyan hello információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e96b4-189">Next, select hello **Stacked column chart** toochange how hello information is displayed.</span></span>

    ![Változó tooa halmozott diagram](./media/hdinsight-storm-power-bi-topology/stackedcolumn.png)

10. <span data-ttu-id="e96b4-191">toosave hello jelentés, jelölje be **mentése** , és írja be a hello jelentés nevét.</span><span class="sxs-lookup"><span data-stu-id="e96b4-191">toosave hello report, select **Save** and enter a name for hello report.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="e96b4-192">Hello topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="e96b4-192">Stop hello topology</span></span>

<span data-ttu-id="e96b4-193">hello topológia toorun folytatódik, amíg megállíthatja vagy hello Storm on HDInsight-fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="e96b4-193">hello topology continues toorun until you stop it or delete hello Storm on HDInsight cluster.</span></span> <span data-ttu-id="e96b4-194">toostop hello topológia, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e96b4-194">toostop hello topology, perform hello following steps:</span></span>

1. <span data-ttu-id="e96b4-195">A Visual Studio toohello topológia viewer vissza, és válassza a hello topológia.</span><span class="sxs-lookup"><span data-stu-id="e96b4-195">In Visual Studio, return toohello topology viewer and select hello topology.</span></span>

2. <span data-ttu-id="e96b4-196">Jelölje be hello **Kill** gomb toostop hello topológia.</span><span class="sxs-lookup"><span data-stu-id="e96b4-196">Select hello **Kill** button toostop hello topology.</span></span>

    ![A Kill hello topológia összefoglaló gomb](./media/hdinsight-storm-power-bi-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="e96b4-198">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="e96b4-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="e96b4-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e96b4-199">Next steps</span></span>

<span data-ttu-id="e96b4-200">Ebből a dokumentumból megtanulta, hogyan toosend adatait egy Storm-topológia tooSQL adatbázis, majd adato hello Power BI használatával.</span><span class="sxs-lookup"><span data-stu-id="e96b4-200">In this document, you learned how toosend data from a Storm topology tooSQL Database, then visualize hello data using Power BI.</span></span> <span data-ttu-id="e96b4-201">Információ és a többi Azure HDInsight a Storm használatának technológia toowork, tekintse meg a következő dokumentum hello:</span><span class="sxs-lookup"><span data-stu-id="e96b4-201">For information on how toowork with other Azure technologies using Storm on HDInsight, see hello following document:</span></span>

* [<span data-ttu-id="e96b4-202">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="e96b4-202">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
