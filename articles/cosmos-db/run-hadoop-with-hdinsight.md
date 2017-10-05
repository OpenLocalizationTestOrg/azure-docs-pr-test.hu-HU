---
title: "Egy Azure Cosmos DB és a HDInsight Hadoop-feladat futtatása |} Microsoft Docs"
description: "Útmutató: Azure Cosmos adatbázis és az Azure HDInsight Hive, Pig és MapReduce egyszerű feladat futtatása."
services: cosmos-db
author: dennyglee
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 06f0ea9d-07cb-4593-a9c5-ab912b62ac42
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 06/08/2017
ms.author: denlee
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 427864fc4e494c19fcda4cfd454a9923499f6337
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <span data-ttu-id="8f417-103"><a name="Azure Cosmos DB-HDInsight"></a>Egy Azure Cosmos DB és HDInsight Apache Hive, Pig vagy Hadoop feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="8f417-103"><a name="Azure Cosmos DB-HDInsight"></a>Run an Apache Hive, Pig, or Hadoop job using Azure Cosmos DB and HDInsight</span></span>
<span data-ttu-id="8f417-104">Az oktatóanyag bemutatja, hogyan futtathat [Apache Hive][apache-hive], [Apache Pig][apache-pig], és [Apache Hadoop] [ apache-hadoop] MapReduce-feladatok on Azure HDInsight Cosmos DB Hadoop-összekötővel együtt.</span><span class="sxs-lookup"><span data-stu-id="8f417-104">This tutorial shows you how to run [Apache Hive][apache-hive], [Apache Pig][apache-pig], and [Apache Hadoop][apache-hadoop] MapReduce jobs on Azure HDInsight with Cosmos DB's Hadoop connector.</span></span> <span data-ttu-id="8f417-105">Cosmos DB Hadoop összekötő lehetővé teszi, hogy a forrás és a Hive, Pig és MapReduce-feladatok fogadó Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8f417-105">Cosmos DB's Hadoop connector allows Cosmos DB to act as both a source and sink for Hive, Pig, and MapReduce jobs.</span></span> <span data-ttu-id="8f417-106">Ez az oktatóanyag fog használni Cosmos DB adatforrás mind az átadó a Hadoop-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="8f417-106">This tutorial will use Cosmos DB as both the data source and destination for Hadoop jobs.</span></span>

<span data-ttu-id="8f417-107">Ez az oktatóanyag befejezése után képes lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="8f417-107">After completing this tutorial, you'll be able to answer the following questions:</span></span>

* <span data-ttu-id="8f417-108">Hogyan betölteni az adatokat a Hive, Pig vagy MapReduce feladat használatával Cosmos-Adatbázisból?</span><span class="sxs-lookup"><span data-stu-id="8f417-108">How do I load data from Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>
* <span data-ttu-id="8f417-109">Hogyan adat tárolása Cosmos DB használatával a Hive, Pig vagy MapReduce feladatot?</span><span class="sxs-lookup"><span data-stu-id="8f417-109">How do I store data in Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>

<span data-ttu-id="8f417-110">Azt javasoljuk, hogy Kezdésként tekintse meg az alábbi videót, ahol azt végigfuttatása egy Cosmos-adatbázis és a HDInsight Hive-feladatot.</span><span class="sxs-lookup"><span data-stu-id="8f417-110">We recommend getting started by watching the following video, where we run through a Hive job using Cosmos DB and HDInsight.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

<span data-ttu-id="8f417-111">Ezt követően térjen vissza a cikkhez, ahol fog kapni a teljes adatait, hogyan futtathat analytics-feladatok az Cosmos DB adatokon.</span><span class="sxs-lookup"><span data-stu-id="8f417-111">Then, return to this article, where you'll receive the full details on how you can run analytics jobs on your Cosmos DB data.</span></span>

> [!TIP]
> <span data-ttu-id="8f417-112">Ez az oktatóanyag feltételezi, hogy rendelkezik-e előzetes tapasztalata az Apache Hadoop, a Hive és/vagy a Pig használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8f417-112">This tutorial assumes that you have prior experience using Apache Hadoop, Hive, and/or Pig.</span></span> <span data-ttu-id="8f417-113">Ha most ismerkedik az Apache Hadoop, a Hive és a Pig, ajánlott felkeresi a [Apache Hadoop-dokumentáció][apache-hadoop-doc].</span><span class="sxs-lookup"><span data-stu-id="8f417-113">If you are new to Apache Hadoop, Hive, and Pig, we recommend visiting the [Apache Hadoop documentation][apache-hadoop-doc].</span></span> <span data-ttu-id="8f417-114">Ez az oktatóanyag azt is feltételezi, hogy rendelkezik tapasztalattal a Cosmos DB, és Cosmos DB fiókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8f417-114">This tutorial also assumes that you have prior experience with Cosmos DB and have a Cosmos DB account.</span></span> <span data-ttu-id="8f417-115">Ha még nem ismeri a Cosmos DB, vagy Ön nem rendelkezik egy Cosmos-DB-fiókot, vegye ki a [bevezetés] [ getting-started] lap.</span><span class="sxs-lookup"><span data-stu-id="8f417-115">If you are new to Cosmos DB or you do not have a Cosmos DB account, please check out our [Getting Started][getting-started] page.</span></span>
>
>

<span data-ttu-id="8f417-116">Nincs ideje elvégezni az oktatóanyagot, és csak szeretné, hogy a teljes minta PowerShell-parancsfájlok a Hive, Pig és MapReduce?</span><span class="sxs-lookup"><span data-stu-id="8f417-116">Don't have time to complete the tutorial and just want to get the full sample PowerShell scripts for Hive, Pig, and MapReduce?</span></span> <span data-ttu-id="8f417-117">Nem probléma, azokat [Itt][hdinsight-samples].</span><span class="sxs-lookup"><span data-stu-id="8f417-117">Not a problem, get them [here][hdinsight-samples].</span></span> <span data-ttu-id="8f417-118">A letöltés a következő mintákat hql, a pig és a java fájljait is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8f417-118">The download also contains the hql, pig, and java files for these samples.</span></span>

## <span data-ttu-id="8f417-119"><a name="NewestVersion"></a>Legújabb verziója</span><span class="sxs-lookup"><span data-stu-id="8f417-119"><a name="NewestVersion"></a>Newest Version</span></span>
<table border='1'>
    <tr><th><span data-ttu-id="8f417-120">Hadoop Connector ezen verziója</span><span class="sxs-lookup"><span data-stu-id="8f417-120">Hadoop Connector Version</span></span></th>
        <td><span data-ttu-id="8f417-121">1.2.0</span><span class="sxs-lookup"><span data-stu-id="8f417-121">1.2.0</span></span></td></tr>
    <tr><th><span data-ttu-id="8f417-122">Parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="8f417-122">Script Uri</span></span></th>
        <td><span data-ttu-id="8f417-123">https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</span><span class="sxs-lookup"><span data-stu-id="8f417-123">https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</span></span></td></tr>
    <tr><th><span data-ttu-id="8f417-124">Módosítás dátuma</span><span class="sxs-lookup"><span data-stu-id="8f417-124">Date Modified</span></span></th>
        <td><span data-ttu-id="8f417-125">04/26/2016</span><span class="sxs-lookup"><span data-stu-id="8f417-125">04/26/2016</span></span></td></tr>
    <tr><th><span data-ttu-id="8f417-126">Támogatott HDInsight-verziókról</span><span class="sxs-lookup"><span data-stu-id="8f417-126">Supported HDInsight Versions</span></span></th>
        <td><span data-ttu-id="8f417-127">3.1, 3.2</span><span class="sxs-lookup"><span data-stu-id="8f417-127">3.1, 3.2</span></span></td></tr>
    <tr><th><span data-ttu-id="8f417-128">Módosítási napló</span><span class="sxs-lookup"><span data-stu-id="8f417-128">Change Log</span></span></th>
        <td><span data-ttu-id="8f417-129">A frissített Azure Cosmos DB Java SDK használatával 1.6.0</span><span class="sxs-lookup"><span data-stu-id="8f417-129">Updated Azure Cosmos DB Java SDK to 1.6.0</span></span></br>
            <span data-ttu-id="8f417-130">A forrás-és a fogadó particionált gyűjtemények támogatása</span><span class="sxs-lookup"><span data-stu-id="8f417-130">Added support for partitioned collections as both a source and sink</span></span></br>
        </td></tr>
</table>

## <span data-ttu-id="8f417-131"><a name="Prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f417-131"><a name="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="8f417-132">Ez az oktatóanyag utasításainak követése, előtt ellenőrizze, hogy a következő:</span><span class="sxs-lookup"><span data-stu-id="8f417-132">Before following the instructions in this tutorial, ensure that you have the following:</span></span>

* <span data-ttu-id="8f417-133">Egy Cosmos-DB-fiók, adatbázis és a dokumentumok egy gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="8f417-133">A Cosmos DB account, a database, and a collection with documents inside.</span></span> <span data-ttu-id="8f417-134">További információkért lásd: [Ismerkedés a Cosmos DB][getting-started].</span><span class="sxs-lookup"><span data-stu-id="8f417-134">For more information, see [Getting Started with Cosmos DB][getting-started].</span></span> <span data-ttu-id="8f417-135">Minta adatok importálása Cosmos DB fiókját a [Cosmos DB import eszközt][import-data].</span><span class="sxs-lookup"><span data-stu-id="8f417-135">Import sample data into your Cosmos DB account with the [Cosmos DB import tool][import-data].</span></span>
* <span data-ttu-id="8f417-136">Átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="8f417-136">Throughput.</span></span> <span data-ttu-id="8f417-137">Beolvassa és a HDInsight-ból írások beleszámítanak a meghatározott időn belül kérelemegység a gyűjteményekben felé.</span><span class="sxs-lookup"><span data-stu-id="8f417-137">Reads and writes from HDInsight will be counted towards your allotted request units for your collections.</span></span>
* <span data-ttu-id="8f417-138">Egyes belül egy további tárolt eljárást a kapacitás kimeneti gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="8f417-138">Capacity for an additional stored procedure within each output collection.</span></span> <span data-ttu-id="8f417-139">A tárolt eljárások az eredményül kapott dokumentumok átvitele használnak.</span><span class="sxs-lookup"><span data-stu-id="8f417-139">The stored procedures are used for transferring resulting documents.</span></span>
* <span data-ttu-id="8f417-140">Az eredményül kapott dokumentumokat a Hive, Pig vagy MapReduce feladatokból kapacitása.</span><span class="sxs-lookup"><span data-stu-id="8f417-140">Capacity for the resulting documents from the Hive, Pig, or MapReduce jobs.</span></span>
* <span data-ttu-id="8f417-141">[*Nem kötelező*] kapacitásának meghatározása egy további gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="8f417-141">[*Optional*] Capacity for an additional collection.</span></span>

> [!WARNING]
> <span data-ttu-id="8f417-142">A feladatok az új gyűjtemény létrehozása elkerülése érdekében az eredményeket a stdout nyomtatása, kimenetét mentse a WASB tárolóhoz, vagy adjon meg egy már létező gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="8f417-142">In order to avoid the creation of a new collection during any of the jobs, you can either print the results to stdout, save the output to your WASB container, or specify an already existing collection.</span></span> <span data-ttu-id="8f417-143">Esetén adja meg egy meglévő gyűjteményt, új dokumentumok a gyűjtemény belül jön létre, és már meglévő dokumentumokat csak az érintett ütközése esetén *azonosítók*.</span><span class="sxs-lookup"><span data-stu-id="8f417-143">In the case of specifying an existing collection, new documents will be created inside the collection and already existing documents will only be affected if there is a conflict in *ids*.</span></span> <span data-ttu-id="8f417-144">**Az összekötő automatikusan felülírja meglévő dokumentumok ütközését**.</span><span class="sxs-lookup"><span data-stu-id="8f417-144">**The connector will automatically overwrite existing documents with id conflicts**.</span></span> <span data-ttu-id="8f417-145">Ez a szolgáltatás kikapcsolása az upsert beállítást FALSE értékre állítását.</span><span class="sxs-lookup"><span data-stu-id="8f417-145">You can turn off this feature by setting the upsert option to false.</span></span> <span data-ttu-id="8f417-146">Ha upsert értéke false, és ütközés lép fel, a Hadoop-feladat meghiúsul; egy azonosító ütközés hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="8f417-146">If upsert is false and a conflict occurs, the Hadoop job will fail; reporting an id conflict error.</span></span>
>
>

## <span data-ttu-id="8f417-147"><a name="ProvisionHDInsight"></a>1. lépés:, Hozzon létre egy új HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="8f417-147"><a name="ProvisionHDInsight"></a>Step 1: Create a new HDInsight cluster</span></span>
<span data-ttu-id="8f417-148">Ez az oktatóanyag az Azure portálról parancsfájlművelet használatával testre szabhatja a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8f417-148">This tutorial uses Script Action from the Azure Portal to customize your HDInsight cluster.</span></span> <span data-ttu-id="8f417-149">Ebben az oktatóanyagban a HDInsight-fürt létrehozása az Azure portál használjuk.</span><span class="sxs-lookup"><span data-stu-id="8f417-149">In this tutorial, we will use the Azure Portal to create your HDInsight cluster.</span></span> <span data-ttu-id="8f417-150">PowerShell-parancsmagokkal vagy a HDInsight .NET SDK használatával útmutatásért tekintse meg a [testreszabása HDInsight-fürtök használata parancsfájlművelet] [ hdinsight-custom-provision] cikk.</span><span class="sxs-lookup"><span data-stu-id="8f417-150">For instructions on how to use PowerShell cmdlets or the HDInsight .NET SDK, check out the [Customize HDInsight clusters using Script Action][hdinsight-custom-provision] article.</span></span>

1. <span data-ttu-id="8f417-151">Jelentkezzen be a [Azure-portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="8f417-151">Sign in to the [Azure Portal][azure-portal].</span></span>
2. <span data-ttu-id="8f417-152">Kattintson a **+ új** tetején a bal oldali navigációs keressen **HDInsight** a felső keresősávban az új panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-152">Click **+ New** on the top of the left navigation, search for **HDInsight** in the top search bar on the New blade.</span></span>
3. <span data-ttu-id="8f417-153">**HDInsight** által közzétett **Microsoft** az eredményeket a lap tetején fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="8f417-153">**HDInsight** published by **Microsoft** will appear at the top of the Results.</span></span> <span data-ttu-id="8f417-154">Kattintson rá, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8f417-154">Click on it and then click **Create**.</span></span>
4. <span data-ttu-id="8f417-155">Az új HDInsight-fürt létrehozása a panelen, adja meg a **fürtnév** válassza ki a **előfizetés** ki kívánja építeni a ehhez az erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="8f417-155">On the New HDInsight Cluster create blade, enter your **Cluster Name** and select the **Subscription** you want to provision this resource under.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="8f417-156">Fürt neve</span><span class="sxs-lookup"><span data-stu-id="8f417-156">Cluster name</span></span></td><td><span data-ttu-id="8f417-157">A fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="8f417-157">Name the cluster.</span></span><br/>
<span data-ttu-id="8f417-158">DNS-beli név kell elindítani egy alfanumerikus karakterrel végződik és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="8f417-158">DNS name must start and end with an alpha numeric character, and may contain dashes.</span></span><br/>
<span data-ttu-id="8f417-159">A mező 3 és 63 karakter közötti karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8f417-159">The field must be a string between 3 and 63 characters.</span></span></td></tr>
        <tr><td><span data-ttu-id="8f417-160">Előfizetés neve</span><span class="sxs-lookup"><span data-stu-id="8f417-160">Subscription Name</span></span></td>
            <td><span data-ttu-id="8f417-161">Ha több Azure-előfizetéssel rendelkezik, válassza ki az előfizetést, amely a HDInsight-fürt üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="8f417-161">If you have more than one Azure Subscription, select the subscription that will host your HDInsight cluster.</span></span> </td></tr>
    </table><span data-ttu-id="8f417-162">
5.Kattintson a **fürt típusának kiválasztása** és állítsa be a következő tulajdonságokat a megadott értékre.</span><span class="sxs-lookup"><span data-stu-id="8f417-162">
5. Click **Select Cluster Type** and set the following properties to the specified values.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="8f417-163">Fürttípus</span><span class="sxs-lookup"><span data-stu-id="8f417-163">Cluster type</span></span></td><td><span data-ttu-id="8f417-164"><strong>Hadoop</strong></span><span class="sxs-lookup"><span data-stu-id="8f417-164"><strong>Hadoop</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="8f417-165">Fürt réteg</span><span class="sxs-lookup"><span data-stu-id="8f417-165">Cluster tier</span></span></td><td><span data-ttu-id="8f417-166"><strong>Standard</strong></span><span class="sxs-lookup"><span data-stu-id="8f417-166"><strong>Standard</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="8f417-167">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="8f417-167">Operating System</span></span></td><td><span data-ttu-id="8f417-168"><strong>Windows</strong></span><span class="sxs-lookup"><span data-stu-id="8f417-168"><strong>Windows</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="8f417-169">Verzió</span><span class="sxs-lookup"><span data-stu-id="8f417-169">Version</span></span></td><td><span data-ttu-id="8f417-170">legújabb verzió</span><span class="sxs-lookup"><span data-stu-id="8f417-170">latest version</span></span></td></tr>
    </table>

    <span data-ttu-id="8f417-171">Most kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8f417-171">Now, click **SELECT**.</span></span>

    ![Adja meg a Hadoop HDInsight a fürtcsomópontok kezdeti adatait][image-customprovision-page1]
6. <span data-ttu-id="8f417-173">Kattintson a **hitelesítő adatok** a bejelentkezési és távelérési hitelesítő adatainak beállításához.</span><span class="sxs-lookup"><span data-stu-id="8f417-173">Click on **Credentials** to set your login and remote access credentials.</span></span> <span data-ttu-id="8f417-174">Válassza ki a **a fürt bejelentkezési felhasználónevének** és **a fürt bejelentkezési jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8f417-174">Choose your **Cluster Login Username** and **Cluster Login Password**.</span></span>

    <span data-ttu-id="8f417-175">Ha a távoli a fürtbe, jelölje be *Igen* a panel alján, és adja meg a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="8f417-175">If you want to remote into your cluster, select *yes* at the bottom of the blade and provide a username and password.</span></span>
7. <span data-ttu-id="8f417-176">Kattintson a **adatforrás** beállítása az adatok az elsődleges helyen.</span><span class="sxs-lookup"><span data-stu-id="8f417-176">Click on **Data Source** to set your primary location for data access.</span></span> <span data-ttu-id="8f417-177">Válassza ki a **kijelöléséről** , és adjon meg egy már meglévő tárfiókot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="8f417-177">Choose the **Selection Method** and specify an already existing storage account or create a new one.</span></span>
8. <span data-ttu-id="8f417-178">Adja meg az azonos panel egy **alapértelmezett tároló** és egy **hely**.</span><span class="sxs-lookup"><span data-stu-id="8f417-178">On the same blade, specify a **Default Container** and a **Location**.</span></span> <span data-ttu-id="8f417-179">Majd kattintson a gombra **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8f417-179">And, click **SELECT**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f417-180">Válassza ki a teljesítmény növelése érdekében fiók Cosmos DB régiójától legközelebb eső helyet</span><span class="sxs-lookup"><span data-stu-id="8f417-180">Select a location close to your Cosmos DB account region for better performance</span></span>
   >
   >
9. <span data-ttu-id="8f417-181">Kattintson a **árazás** számát és típusát csomópont kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="8f417-181">Click on **Pricing** to select the number and type of nodes.</span></span> <span data-ttu-id="8f417-182">Tartani az alapértelmezett konfigurációt, és később a méretezni a feldolgozó csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="8f417-182">You can keep the default configuration and scale the number of Worker nodes later on.</span></span>
10. <span data-ttu-id="8f417-183">Kattintson a **opcionális konfigurációs**, majd **parancsfájl-műveletek** az opcionális konfigurációs panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-183">Click **Optional Configuration**, then **Script Actions** in the Optional Configuration Blade.</span></span>

     <span data-ttu-id="8f417-184">Adja meg a következő adatokat a HDInsight-fürt testreszabása Parancsfájlműveletek.</span><span class="sxs-lookup"><span data-stu-id="8f417-184">In Script Actions, enter the following information to customize your HDInsight cluster.</span></span>

     <table border='1'>
         <tr><th><span data-ttu-id="8f417-185">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8f417-185">Property</span></span></th><th><span data-ttu-id="8f417-186">Érték</span><span class="sxs-lookup"><span data-stu-id="8f417-186">Value</span></span></th></tr>
         <tr><td><span data-ttu-id="8f417-187">Név</span><span class="sxs-lookup"><span data-stu-id="8f417-187">Name</span></span></td>
             <td><span data-ttu-id="8f417-188">Adja meg a parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="8f417-188">Specify a name for the script action.</span></span></td></tr>
         <tr><td><span data-ttu-id="8f417-189">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="8f417-189">Script URI</span></span></td>
             <td><span data-ttu-id="8f417-190">Adja meg az URI-t a parancsfájlt, amelyet a fürt testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="8f417-190">Specify the URI to the script that is invoked to customize the cluster.</span></span></br></br>
<span data-ttu-id="8f417-191">Adja meg:</span><span class="sxs-lookup"><span data-stu-id="8f417-191">Please enter:</span></span> </br> <span data-ttu-id="8f417-192"><strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-192"><strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="8f417-193">Fej</span><span class="sxs-lookup"><span data-stu-id="8f417-193">Head</span></span></td>
             <td><span data-ttu-id="8f417-194">Kattintson a jelölőnégyzetbe, az átjárócsomópont a PowerShell parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="8f417-194">Click the checkbox to run the PowerShell script onto the Head node.</span></span></br></br><span data-ttu-id="8f417-195">
             <strong>Ezt a jelölőnégyzetet</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-195">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="8f417-196">Munkavégző</span><span class="sxs-lookup"><span data-stu-id="8f417-196">Worker</span></span></td>
             <td><span data-ttu-id="8f417-197">Kattintson a jelölőnégyzetbe, a munkavégző csomópont a PowerShell parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="8f417-197">Click the checkbox to run the PowerShell script onto the Worker node.</span></span></br></br><span data-ttu-id="8f417-198">
             <strong>Ezt a jelölőnégyzetet</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-198">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="8f417-199">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="8f417-199">Zookeeper</span></span></td>
             <td><span data-ttu-id="8f417-200">Kattintson a jelölőnégyzetbe, a Zookeeper a PowerShell parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="8f417-200">Click the checkbox to run the PowerShell script onto the Zookeeper.</span></span></br></br><span data-ttu-id="8f417-201">
             <strong>Nincs szükség</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-201">
             <strong>Not needed</strong>.</span></span>
             </td></tr>
         <tr><td><span data-ttu-id="8f417-202">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="8f417-202">Parameters</span></span></td>
             <td><span data-ttu-id="8f417-203">Adja meg a paraméterek, ha a parancsfájl által igényelt.</span><span class="sxs-lookup"><span data-stu-id="8f417-203">Specify the parameters, if required by the script.</span></span></br></br><span data-ttu-id="8f417-204">
             <strong>Nem szükséges paramétereket</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-204">
             <strong>No Parameters needed</strong>.</span></span></td></tr>
     </table><span data-ttu-id="8f417-205">
11.Vagy hozzon létre egy új **erőforráscsoport** , vagy használjon egy meglévő erőforráscsoportot az Azure-előfizetéshez tartozó.</span><span class="sxs-lookup"><span data-stu-id="8f417-205">
11. Create either a new **Resource Group** or use an existing Resource Group under your Azure Subscription.</span></span>
<span data-ttu-id="8f417-206">12.</span><span class="sxs-lookup"><span data-stu-id="8f417-206">12.</span></span> <span data-ttu-id="8f417-207">Ellenőrizze, **rögzítés az irányítópulton** nyomon követhető a központi telepítés, és kattintson a **létrehozása**!</span><span class="sxs-lookup"><span data-stu-id="8f417-207">Now, check **Pin to dashboard** to track its deployment and click **Create**!</span></span>

## <span data-ttu-id="8f417-208"><a name="InstallCmdlets"></a>2. lépés: Telepítse és konfigurálja az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="8f417-208"><a name="InstallCmdlets"></a>Step 2: Install and configure Azure PowerShell</span></span>
1. <span data-ttu-id="8f417-209">Az Azure PowerShell telepítése.</span><span class="sxs-lookup"><span data-stu-id="8f417-209">Install Azure PowerShell.</span></span> <span data-ttu-id="8f417-210">Útmutatás található [Itt][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="8f417-210">Instructions can be found [here][powershell-install-configure].</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f417-211">Azt is megteheti csak a Hive-lekérdezéseket, használhatja a HDInsight tartozó online Hive szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="8f417-211">Alternatively, just for Hive queries, you can use HDInsight's online Hive Editor.</span></span> <span data-ttu-id="8f417-212">Ehhez jelentkezzen be a [Azure Portal][azure-portal], kattintson a **HDInsight** a HDInsight-fürtök listájának megtekintése a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8f417-212">To do so, sign in to the [Azure Portal][azure-portal], click **HDInsight** on the left pane to view a list of your HDInsight clusters.</span></span> <span data-ttu-id="8f417-213">Kattintson arra a fürtre, Hive-lekérdezések futtatása, és kattintson a kívánt **lekérdezés konzol**.</span><span class="sxs-lookup"><span data-stu-id="8f417-213">Click the cluster you want to run Hive queries on, and then click **Query Console**.</span></span>
   >
   >
2. <span data-ttu-id="8f417-214">Nyissa meg az Azure PowerShell integrált parancsprogram-kezelési környezetet:</span><span class="sxs-lookup"><span data-stu-id="8f417-214">Open the Azure PowerShell Integrated Scripting Environment:</span></span>

   * <span data-ttu-id="8f417-215">A Windows 8 vagy Windows Server 2012 vagy újabb rendszerű számítógépen a beépített keresési is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8f417-215">On a computer running Windows 8 or Windows Server 2012 or higher, you can use the built-in Search.</span></span> <span data-ttu-id="8f417-216">Írja be a kezdőképernyőről **powershell ise** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8f417-216">From the Start screen, type **powershell ise** and click **Enter**.</span></span>
   * <span data-ttu-id="8f417-217">A Start menü használata a Windows 8 vagy Windows Server 2012-nél korábbi rendszerű számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8f417-217">On a computer running a version earlier than Windows 8 or Windows Server 2012, use the Start menu.</span></span> <span data-ttu-id="8f417-218">A Start menüben írja be a **parancssor** a keresési mezőbe, majd az eredmények listájában kattintson **parancssor**.</span><span class="sxs-lookup"><span data-stu-id="8f417-218">From the Start menu, type **Command Prompt** in the search box, then in the list of results, click **Command Prompt**.</span></span> <span data-ttu-id="8f417-219">Írja be a parancssorba **powershell_ise** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8f417-219">In the Command Prompt, type **powershell_ise** and click **Enter**.</span></span>
3. <span data-ttu-id="8f417-220">Adja hozzá az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="8f417-220">Add your Azure Account.</span></span>

   1. <span data-ttu-id="8f417-221">Írja be a konzol ablaktáblájában **Add-AzureAccount** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8f417-221">In the Console Pane, type **Add-AzureAccount** and click **Enter**.</span></span>
   2. <span data-ttu-id="8f417-222">Írja be az Azure-előfizetéshez társított e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="8f417-222">Type in the email address associated with your Azure subscription and click **Continue**.</span></span>
   3. <span data-ttu-id="8f417-223">Adja meg az Azure-előfizetéshez tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="8f417-223">Type in the password for your Azure subscription.</span></span>
   4. <span data-ttu-id="8f417-224">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8f417-224">Click **Sign in**.</span></span>
4. <span data-ttu-id="8f417-225">Az alábbi ábra a fontos részek az Azure PowerShell-parancsfájl-kezelési környezet azonosítja.</span><span class="sxs-lookup"><span data-stu-id="8f417-225">The following diagram identifies the important parts of your Azure PowerShell Scripting Environment.</span></span>

    ![Az Azure PowerShell diagramja][azure-powershell-diagram]

## <span data-ttu-id="8f417-227"><a name="RunHive"></a>3. lépés: Egy Cosmos-adatbázis és a HDInsight Hive-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="8f417-227"><a name="RunHive"></a>Step 3: Run a Hive job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8f417-228">< > Által jelzett minden értéket meg kell adni a konfigurációs beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="8f417-228">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="8f417-229">A következő változók megadása a PowerShell-parancsfájl ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8f417-229">Set the following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p><span data-ttu-id="8f417-230">Most először hoz létre, a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8f417-230">Let's begin constructing your query string.</span></span> <span data-ttu-id="8f417-231">A Microsoft fog írni Hive lekérdezés összes dokumentumot (_ts) rendszer által létrehozott időbélyegeket és egyedi azonosítók (_rid) egy Azure Cosmos DB gyűjteményből, összes dokumentumot összeadja a percenként, majd tárolja az eredményeket egy új Azure Cosmos DB gyűjteménybe.</span><span class="sxs-lookup"><span data-stu-id="8f417-231">We'll write a Hive query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by the minute, and then stores the results back into a new Azure Cosmos DB collection.</span></span></p>

    <p><span data-ttu-id="8f417-232">Először hozzon létre olyan Hive táblát az Azure Cosmos DB gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="8f417-232">First, let's create a Hive table from our Azure Cosmos DB collection.</span></span> <span data-ttu-id="8f417-233">Adja hozzá a következő kódrészletet a PowerShell-parancsfájlt tartalmazó ablaktáblájához való <strong>után</strong> a kódrészletet # 1.</span><span class="sxs-lookup"><span data-stu-id="8f417-233">Add the following code snippet to the PowerShell Script pane <strong>after</strong> the code snippet from #1.</span></span> <span data-ttu-id="8f417-234">Ellenőrizze, hogy tartalmazzák a választható DocumentDB.query paraméter t vágás csak _ts a dokumentumok és _rid.</span><span class="sxs-lookup"><span data-stu-id="8f417-234">Make sure you include the optional DocumentDB.query parameter t trim our documents to just _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="8f417-235">**DocumentDB.inputCollections elnevezési nem hiba volt.**</span><span class="sxs-lookup"><span data-stu-id="8f417-235">**Naming DocumentDB.inputCollections was not a mistake.**</span></span> <span data-ttu-id="8f417-236">Igen, azt felvételének engedélyezése több gyűjtemény bemenetként:</span><span class="sxs-lookup"><span data-stu-id="8f417-236">Yes, we allow adding multiple collections as an input:</span></span> </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> The collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. <span data-ttu-id="8f417-237">Következő lépésként hozzon létre olyan Hive táblát a kimeneti gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="8f417-237">Next, let's create a Hive table for the output collection.</span></span> <span data-ttu-id="8f417-238">A kimeneti dokumentumtulajdonságok lesz a hónap, nap, óra, perc és előfordulások száma összesen.</span><span class="sxs-lookup"><span data-stu-id="8f417-238">The output document properties will be the month, day, hour, minute, and the total number of occurrences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f417-239">**Még újra DocumentDB.outputCollections elnevezési nem hiba volt.**</span><span class="sxs-lookup"><span data-stu-id="8f417-239">**Yet again, naming DocumentDB.outputCollections was not a mistake.**</span></span> <span data-ttu-id="8f417-240">Igen, azt felvételének engedélyezése több gyűjtemény kimenetként:</span><span class="sxs-lookup"><span data-stu-id="8f417-240">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="8f417-241">"*DocumentDB.outputCollections*'='*\<DocumentDB-gyűjtemény Kimenetnév 1\>*,*\<DocumentDB-gyűjtemény Kimenetnév 2\>*"</span><span class="sxs-lookup"><span data-stu-id="8f417-241">'*DocumentDB.outputCollections*' = '*\<DocumentDB Output Collection Name 1\>*,*\<DocumentDB Output Collection Name 2\>*'</span></span> </br> <span data-ttu-id="8f417-242">A gyűjtemény nevét nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="8f417-242">The collection names are separated without spaces, using only a single comma.</span></span> </br></br>
   > <span data-ttu-id="8f417-243">Dokumentumok lesz elosztott ciklikus multiplexelés több gyűjtemények között.</span><span class="sxs-lookup"><span data-stu-id="8f417-243">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="8f417-244">A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni a következő gyűjteményben, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="8f417-244">A batch of documents will be stored in one collection, then a second batch of documents will be stored in the next collection, and so forth.</span></span>
   >
   >

       # Create a Hive table for the output data to DocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. <span data-ttu-id="8f417-245">Végül tegyük a dokumentumok egyeztetési hónap, nap, óra és perc által és az eredmények beszúrása vissza a kimeneti Hive tábla.</span><span class="sxs-lookup"><span data-stu-id="8f417-245">Finally, let's tally the documents by month, day, hour, and minute and insert the results back into the output Hive table.</span></span>

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. <span data-ttu-id="8f417-246">Adja hozzá a következő parancsfájl részlet használatával hozhat létre az előző lekérdezés egy Hive feladat definíciójához.</span><span class="sxs-lookup"><span data-stu-id="8f417-246">Add the following script snippet to create a Hive job definition from the previous query.</span></span>

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    <span data-ttu-id="8f417-247">Is meg kell adni egy HiveQL-parancsfájlt a HDFS a - fájl kapcsoló.</span><span class="sxs-lookup"><span data-stu-id="8f417-247">You can also use the -File switch to specify a HiveQL script file on HDFS.</span></span>
4. <span data-ttu-id="8f417-248">Adja hozzá a következő kódrészletet a kezdési időt és elküldeni a Hive feladatot.</span><span class="sxs-lookup"><span data-stu-id="8f417-248">Add the following snippet to save the start time and submit the Hive job.</span></span>

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. <span data-ttu-id="8f417-249">Adja hozzá a következő, a Hive feladat befejeződésére vár.</span><span class="sxs-lookup"><span data-stu-id="8f417-249">Add the following to wait for the Hive job to complete.</span></span>

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. <span data-ttu-id="8f417-250">Adja hozzá a következő, a standard kimenet és a kezdő és záró időpontjának nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="8f417-250">Add the following to print the standard output and the start and end times.</span></span>

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. <span data-ttu-id="8f417-251">**Futtatás** az új parancsfájl!</span><span class="sxs-lookup"><span data-stu-id="8f417-251">**Run** your new script!</span></span> <span data-ttu-id="8f417-252">**Kattintson a** a zöld végrehajtás gombra.</span><span class="sxs-lookup"><span data-stu-id="8f417-252">**Click** the green execute button.</span></span>
8. <span data-ttu-id="8f417-253">Ellenőrizze az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="8f417-253">Check the results.</span></span> <span data-ttu-id="8f417-254">Jelentkezzen be a [Azure-portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="8f417-254">Sign into the [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="8f417-255">Kattintson a <strong>Tallózás</strong> a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-255">Click <strong>Browse</strong> on the left-side panel.</span></span> </br>
   2. <span data-ttu-id="8f417-256">Kattintson a <strong>mindent</strong> , a jobb felső részén a böngészés panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-256">Click <strong>everything</strong> at the top-right of the browse panel.</span></span> </br>
   3. <span data-ttu-id="8f417-257">Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-257">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
   4. <span data-ttu-id="8f417-258">Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> a Hive-lekérdezést a megadott kimeneti gyűjtemény társítva.</span><span class="sxs-lookup"><span data-stu-id="8f417-258">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with the output collection specified in your Hive query.</span></span></br>
   5. <span data-ttu-id="8f417-259">Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-259">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

   <span data-ttu-id="8f417-260">A Hive-lekérdezések eredményeinek jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8f417-260">You will see the results of your Hive query.</span></span>

   ![Hive lekérdezés eredményei][image-hive-query-results]

## <span data-ttu-id="8f417-262"><a name="RunPig"></a>4. lépés: A Pig feladatot Cosmos DB és HDInsight használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="8f417-262"><a name="RunPig"></a>Step 4: Run a Pig job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="8f417-263">< > Által jelzett minden értéket meg kell adni a konfigurációs beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="8f417-263">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="8f417-264">A következő változók megadása a PowerShell-parancsfájl ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8f417-264">Set the following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p><span data-ttu-id="8f417-265">Most először hoz létre, a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="8f417-265">Let's begin constructing your query string.</span></span> <span data-ttu-id="8f417-266">A Microsoft fog írni Pig lekérdezés összes dokumentumot (_ts) rendszer által létrehozott időbélyegeket és egyedi azonosítók (_rid) egy Azure Cosmos DB gyűjteményből, összes dokumentumot összeadja a percenként, majd tárolja az eredményeket egy új Azure Cosmos DB gyűjteménybe.</span><span class="sxs-lookup"><span data-stu-id="8f417-266">We'll write a Pig query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by the minute, and then stores the results back into a new Azure Cosmos DB collection.</span></span></p>
    <p><span data-ttu-id="8f417-267">Első lépésként betölteni a HDInsight a Cosmos-Adatbázisból dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="8f417-267">First, load documents from Cosmos DB into HDInsight.</span></span> <span data-ttu-id="8f417-268">Adja hozzá a következő kódrészletet a PowerShell-parancsfájlt tartalmazó ablaktáblájához való <strong>után</strong> a kódrészletet # 1.</span><span class="sxs-lookup"><span data-stu-id="8f417-268">Add the following code snippet to the PowerShell Script pane <strong>after</strong> the code snippet from #1.</span></span> <span data-ttu-id="8f417-269">Ügyeljen arra, hogy egy DocumentDB-lekérdezés hozzáadása a nem kötelező a DocumentDB lekérdezési paraméter lehet levágni a dokumentumokat csak _ts és _rid.</span><span class="sxs-lookup"><span data-stu-id="8f417-269">Make sure to add a DocumentDB query to the optional DocumentDB query parameter to trim our documents to just _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="8f417-270">Igen, azt felvételének engedélyezése több gyűjtemény bemenetként:</span><span class="sxs-lookup"><span data-stu-id="8f417-270">Yes, we allow adding multiple collections as an input:</span></span> </br>
   > <span data-ttu-id="8f417-271">"*\<DocumentDB bemeneti gyűjtemény neve 1\>*,*\<DocumentDB bemeneti gyűjteménynév 2\>*"</span><span class="sxs-lookup"><span data-stu-id="8f417-271">'*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*'</span></span></br> <span data-ttu-id="8f417-272">A gyűjtemény nevét nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="8f417-272">The collection names are separated without spaces, using only a single comma.</span></span> </b>
   >
   >

    <span data-ttu-id="8f417-273">Dokumentumok lesz elosztott ciklikus multiplexelés több gyűjtemények között.</span><span class="sxs-lookup"><span data-stu-id="8f417-273">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="8f417-274">A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni a következő gyűjteményben, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="8f417-274">A batch of documents will be stored in one collection, then a second batch of documents will be stored in the next collection, and so forth.</span></span>

        # Load data from Cosmos DB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. <span data-ttu-id="8f417-275">A következő most megegyeznek a dokumentumok a hónap, nap, óra, perc és előfordulások száma szerint.</span><span class="sxs-lookup"><span data-stu-id="8f417-275">Next, let's tally the documents by the month, day, hour, minute, and the total number of occurrences.</span></span>

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. <span data-ttu-id="8f417-276">Végezetül most az eredmények tárolásához az új kimeneti gyűjteménybe.</span><span class="sxs-lookup"><span data-stu-id="8f417-276">Finally, let's store the results into our new output collection.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8f417-277">Igen, azt felvételének engedélyezése több gyűjtemény kimenetként:</span><span class="sxs-lookup"><span data-stu-id="8f417-277">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="8f417-278">"\<DocumentDB-gyűjtemény Kimenetnév 1\>,\<DocumentDB-gyűjtemény Kimenetnév 2\>"</span><span class="sxs-lookup"><span data-stu-id="8f417-278">'\<DocumentDB Output Collection Name 1\>,\<DocumentDB Output Collection Name 2\>'</span></span></br> <span data-ttu-id="8f417-279">A gyűjtemény nevét nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="8f417-279">The collection names are separated without spaces, using only a single comma.</span></span></br>
   > <span data-ttu-id="8f417-280">Dokumentumok elosztott ciklikus multiplexelés lesz a több gyűjtemények között.</span><span class="sxs-lookup"><span data-stu-id="8f417-280">Documents will be distributed round-robin across the multiple collections.</span></span> <span data-ttu-id="8f417-281">A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni a következő gyűjteményben, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="8f417-281">A batch of documents will be stored in one collection, then a second batch of documents will be stored in the next collection, and so forth.</span></span>
   >
   >

        # Store output data to Cosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. <span data-ttu-id="8f417-282">Adja hozzá az alábbi parancsfájl kódrészletet az előző lekérdezés egy olyan feladatdefinícióban létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8f417-282">Add the following script snippet to create a Pig job definition from the previous query.</span></span>

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    <span data-ttu-id="8f417-283">Használhatja a - fájl kapcsoló a HDFS Pig-parancsprogram fájlt ad meg.</span><span class="sxs-lookup"><span data-stu-id="8f417-283">You can also use the -File switch to specify a Pig script file on HDFS.</span></span>
6. <span data-ttu-id="8f417-284">Adja hozzá a következő kódrészletet a kezdési időt és elküldeni a Pig feladatot.</span><span class="sxs-lookup"><span data-stu-id="8f417-284">Add the following snippet to save the start time and submit the Pig job.</span></span>

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. <span data-ttu-id="8f417-285">Adja hozzá a következő, a Pig futó feladat befejeződésére vár.</span><span class="sxs-lookup"><span data-stu-id="8f417-285">Add the following to wait for the Pig job to complete.</span></span>

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. <span data-ttu-id="8f417-286">Adja hozzá a következő, a standard kimenet és a kezdő és záró időpontjának nyomtatása.</span><span class="sxs-lookup"><span data-stu-id="8f417-286">Add the following to print the standard output and the start and end times.</span></span>

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. <span data-ttu-id="8f417-287">**Futtatás** az új parancsfájl!</span><span class="sxs-lookup"><span data-stu-id="8f417-287">**Run** your new script!</span></span> <span data-ttu-id="8f417-288">**Kattintson a** a zöld végrehajtás gombra.</span><span class="sxs-lookup"><span data-stu-id="8f417-288">**Click** the green execute button.</span></span>
10. <span data-ttu-id="8f417-289">Ellenőrizze az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="8f417-289">Check the results.</span></span> <span data-ttu-id="8f417-290">Jelentkezzen be a [Azure-portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="8f417-290">Sign into the [Azure Portal][azure-portal].</span></span>

    1. <span data-ttu-id="8f417-291">Kattintson a <strong>Tallózás</strong> a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-291">Click <strong>Browse</strong> on the left-side panel.</span></span> </br>
    2. <span data-ttu-id="8f417-292">Kattintson a <strong>mindent</strong> , a jobb felső részén a böngészés panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-292">Click <strong>everything</strong> at the top-right of the browse panel.</span></span> </br>
    3. <span data-ttu-id="8f417-293">Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-293">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
    4. <span data-ttu-id="8f417-294">Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> a Pig-lekérdezésben megadott kimeneti gyűjtemény társítva.</span><span class="sxs-lookup"><span data-stu-id="8f417-294">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with the output collection specified in your Pig query.</span></span></br>
    5. <span data-ttu-id="8f417-295">Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-295">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

    <span data-ttu-id="8f417-296">Látni fogja a Pig lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="8f417-296">You will see the results of your Pig query.</span></span>

    ![A Pig lekérdezés eredményei][image-pig-query-results]

## <span data-ttu-id="8f417-298"><a name="RunMapReduce"></a>5. lépés: Azure Cosmos DB és HDInsight MapReduce feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="8f417-298"><a name="RunMapReduce"></a>Step 5: Run a MapReduce job using Azure Cosmos DB and HDInsight</span></span>
1. <span data-ttu-id="8f417-299">A következő változók megadása a PowerShell-parancsfájl ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="8f417-299">Set the following variables in your PowerShell Script pane.</span></span>

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. <span data-ttu-id="8f417-300">A MapReduce feladatot, amely minden egyes dokumentum tulajdonság az Azure Cosmos DB gyűjteményből előfordulások számát határozza meg azt fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="8f417-300">We'll execute a MapReduce job that tallies the number of occurrences for each Document property from your Azure Cosmos DB collection.</span></span> <span data-ttu-id="8f417-301">Adja hozzá a parancsfájl részlet **után** a fenti kódrészletet.</span><span class="sxs-lookup"><span data-stu-id="8f417-301">Add this script snippet **after** the snippet above.</span></span>

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > <span data-ttu-id="8f417-302">A Cosmos DB Hadoop összekötő egyéni telepítését TallyProperties-v01.jar tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8f417-302">TallyProperties-v01.jar comes with the custom installation of the Cosmos DB Hadoop Connector.</span></span>
   >
   >
3. <span data-ttu-id="8f417-303">Adja hozzá a következő parancs futtatásával elküldeni a MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="8f417-303">Add the following command to submit the MapReduce job.</span></span>

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    <span data-ttu-id="8f417-304">A MapReduce feladatdefiníció mellett is megadta a HDInsight-fürt nevét, ahol szeretné indítani a MapReduce feladatot, és a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="8f417-304">In addition to the MapReduce job definition, you also provide the HDInsight cluster name where you want to run the MapReduce job, and the credentials.</span></span> <span data-ttu-id="8f417-305">A Start-AzureHDInsightJob egy aszinkron módjára való átkapcsolás hívás.</span><span class="sxs-lookup"><span data-stu-id="8f417-305">The Start-AzureHDInsightJob is an asynchronized call.</span></span> <span data-ttu-id="8f417-306">A feladat befejezésére ellenőrzéséhez használja a *várakozási-AzureHDInsightJob* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="8f417-306">To check the completion of the job, use the *Wait-AzureHDInsightJob* cmdlet.</span></span>
4. <span data-ttu-id="8f417-307">Adja hozzá a következő parancs futtatásával ellenőrizze a hibákat a MapReduce feladat futtatásával.</span><span class="sxs-lookup"><span data-stu-id="8f417-307">Add the following command to check any errors with running the MapReduce job.</span></span>

        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. <span data-ttu-id="8f417-308">**Futtatás** az új parancsfájl!</span><span class="sxs-lookup"><span data-stu-id="8f417-308">**Run** your new script!</span></span> <span data-ttu-id="8f417-309">**Kattintson a** a zöld végrehajtás gombra.</span><span class="sxs-lookup"><span data-stu-id="8f417-309">**Click** the green execute button.</span></span>
6. <span data-ttu-id="8f417-310">Ellenőrizze az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="8f417-310">Check the results.</span></span> <span data-ttu-id="8f417-311">Jelentkezzen be a [Azure-portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="8f417-311">Sign into the [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="8f417-312">Kattintson a <strong>Tallózás</strong> a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-312">Click <strong>Browse</strong> on the left-side panel.</span></span>
   2. <span data-ttu-id="8f417-313">Kattintson a <strong>mindent</strong> , a jobb felső részén a böngészés panelen.</span><span class="sxs-lookup"><span data-stu-id="8f417-313">Click <strong>everything</strong> at the top-right of the browse panel.</span></span>
   3. <span data-ttu-id="8f417-314">Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-314">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span>
   4. <span data-ttu-id="8f417-315">Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> a MapReduce feladatot a megadott kimeneti gyűjtemény társítva.</span><span class="sxs-lookup"><span data-stu-id="8f417-315">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with the output collection specified in your MapReduce job.</span></span>
   5. <span data-ttu-id="8f417-316">Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</span><span class="sxs-lookup"><span data-stu-id="8f417-316">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span>

      <span data-ttu-id="8f417-317">Látni fogja a MapReduce feladatot eredményeit.</span><span class="sxs-lookup"><span data-stu-id="8f417-317">You will see the results of your MapReduce job.</span></span>

      ![MapReduce lekérdezés eredményei][image-mapreduce-query-results]

## <span data-ttu-id="8f417-319"><a name="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f417-319"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="8f417-320">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="8f417-320">Congratulations!</span></span> <span data-ttu-id="8f417-321">Csak az első Hive, Pig és MapReduce feladatok Azure Cosmos DB és HDInsight futtatta.</span><span class="sxs-lookup"><span data-stu-id="8f417-321">You just ran your first Hive, Pig, and MapReduce jobs using Azure Cosmos DB and HDInsight.</span></span>

<span data-ttu-id="8f417-322">Tudunk nyissa meg a Hadoop összekötő forrása.</span><span class="sxs-lookup"><span data-stu-id="8f417-322">We have open sourced our Hadoop Connector.</span></span> <span data-ttu-id="8f417-323">Ha szeretné használni, akkor a járulhat [GitHub][github].</span><span class="sxs-lookup"><span data-stu-id="8f417-323">If you're interested, you can contribute on [GitHub][github].</span></span>

<span data-ttu-id="8f417-324">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="8f417-324">To learn more, see the following articles:</span></span>

* <span data-ttu-id="8f417-325">[A Documentdb Java-alkalmazások fejlesztése][documentdb-java-application]</span><span class="sxs-lookup"><span data-stu-id="8f417-325">[Develop a Java application with Documentdb][documentdb-java-application]</span></span>
* <span data-ttu-id="8f417-326">[A hdinsight Hadoop Java MapReduce programok fejlesztése][hdinsight-develop-deploy-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="8f417-326">[Develop Java MapReduce programs for Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]</span></span>
* <span data-ttu-id="8f417-327">[Hadoop használatának megkezdésében a hdinsight Hive elemzéséhez mobil kézibeszélő használata][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="8f417-327">[Get started using Hadoop with Hive in HDInsight to analyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="8f417-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="8f417-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span></span>
* <span data-ttu-id="8f417-329">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="8f417-329">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="8f417-330">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="8f417-330">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="8f417-331">[Parancsfájlművelet HDInsight-fürtök testreszabása][hdinsight-hadoop-customize-cluster]</span><span class="sxs-lookup"><span data-stu-id="8f417-331">[Customize HDInsight clusters using Script Action][hdinsight-hadoop-customize-cluster]</span></span>

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-portal]: https://portal.azure.com/
[azure-powershell-diagram]: ./media/run-hadoop-with-hdinsight/azurepowershell-diagram-med.png

[hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[import-data]: import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight/hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-storage]: ../hdinsight/hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/run-hadoop-with-hdinsight/customprovision-page1.png
[image-hive-query-results]: ./media/run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0
