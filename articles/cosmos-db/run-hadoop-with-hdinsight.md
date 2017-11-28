---
title: "a Hadoop aaaRun feladat Azure Cosmos DB és HDInsight használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toorun egy egyszerű Hive, Pig és MapReduce feladat Azure Cosmos DB és az Azure HDInsight."
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
ms.openlocfilehash: 2e27499f2c4ba951af9a1ade1bcc9c1b6d298fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="1fa13-103"><a name="Azure Cosmos DB-HDInsight"></a>Egy Azure Cosmos DB és HDInsight Apache Hive, Pig vagy Hadoop feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="1fa13-103"><a name="Azure Cosmos DB-HDInsight"></a>Run an Apache Hive, Pig, or Hadoop job using Azure Cosmos DB and HDInsight</span></span>
<span data-ttu-id="1fa13-104">Az oktatóanyag bemutatja, hogyan toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], és [Apache Hadoop] [ apache-hadoop] MapReduce-feladatok on Azure HDInsight Cosmos DB Hadoop-összekötővel együtt.</span><span class="sxs-lookup"><span data-stu-id="1fa13-104">This tutorial shows you how toorun [Apache Hive][apache-hive], [Apache Pig][apache-pig], and [Apache Hadoop][apache-hadoop] MapReduce jobs on Azure HDInsight with Cosmos DB's Hadoop connector.</span></span> <span data-ttu-id="1fa13-105">Cosmos DB a Hadoop-összekötő lehetővé teszi, hogy a forrás-és a Hive, Pig és MapReduce-feladatok fogadó Cosmos DB tooact.</span><span class="sxs-lookup"><span data-stu-id="1fa13-105">Cosmos DB's Hadoop connector allows Cosmos DB tooact as both a source and sink for Hive, Pig, and MapReduce jobs.</span></span> <span data-ttu-id="1fa13-106">Ez az oktatóanyag hello adatforrás és a célként megadott Cosmos DB Hadoop-feladatokat használja.</span><span class="sxs-lookup"><span data-stu-id="1fa13-106">This tutorial will use Cosmos DB as both hello data source and destination for Hadoop jobs.</span></span>

<span data-ttu-id="1fa13-107">Ez az oktatóanyag befejezése után be fog tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="1fa13-107">After completing this tutorial, you'll be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="1fa13-108">Hogyan betölteni az adatokat a Hive, Pig vagy MapReduce feladat használatával Cosmos-Adatbázisból?</span><span class="sxs-lookup"><span data-stu-id="1fa13-108">How do I load data from Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>
* <span data-ttu-id="1fa13-109">Hogyan adat tárolása Cosmos DB használatával a Hive, Pig vagy MapReduce feladatot?</span><span class="sxs-lookup"><span data-stu-id="1fa13-109">How do I store data in Cosmos DB using a Hive, Pig, or MapReduce job?</span></span>

<span data-ttu-id="1fa13-110">Azt javasoljuk, hogy Kezdésként tekintse a következő videó, ahol azt végigfuttatása egy Cosmos-adatbázis és a HDInsight Hive-feladat hello.</span><span class="sxs-lookup"><span data-stu-id="1fa13-110">We recommend getting started by watching hello following video, where we run through a Hive job using Cosmos DB and HDInsight.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Use-Azure-DocumentDB-Hadoop-Connector-with-Azure-HDInsight/player]
>
>

<span data-ttu-id="1fa13-111">Ezt követően térjen vissza toothis cikk, ahol értesítjük, hogyan futtathat analytics-feladatok a Cosmos DB adatai a hello részletesen.</span><span class="sxs-lookup"><span data-stu-id="1fa13-111">Then, return toothis article, where you'll receive hello full details on how you can run analytics jobs on your Cosmos DB data.</span></span>

> [!TIP]
> <span data-ttu-id="1fa13-112">Ez az oktatóanyag feltételezi, hogy rendelkezik-e előzetes tapasztalata az Apache Hadoop, a Hive és/vagy a Pig használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="1fa13-112">This tutorial assumes that you have prior experience using Apache Hadoop, Hive, and/or Pig.</span></span> <span data-ttu-id="1fa13-113">Ha új tooApache Hadoop Hive és a Pig, ajánlott hello látogató [Apache Hadoop-dokumentáció][apache-hadoop-doc].</span><span class="sxs-lookup"><span data-stu-id="1fa13-113">If you are new tooApache Hadoop, Hive, and Pig, we recommend visiting hello [Apache Hadoop documentation][apache-hadoop-doc].</span></span> <span data-ttu-id="1fa13-114">Ez az oktatóanyag azt is feltételezi, hogy rendelkezik tapasztalattal a Cosmos DB, és Cosmos DB fiókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="1fa13-114">This tutorial also assumes that you have prior experience with Cosmos DB and have a Cosmos DB account.</span></span> <span data-ttu-id="1fa13-115">Ha új tooCosmos DB, vagy Ön nem rendelkezik egy Cosmos-DB-fiókot, vegye ki a [bevezetés] [ getting-started] lap.</span><span class="sxs-lookup"><span data-stu-id="1fa13-115">If you are new tooCosmos DB or you do not have a Cosmos DB account, please check out our [Getting Started][getting-started] page.</span></span>
>
>

<span data-ttu-id="1fa13-116">Nincs ideje toocomplete hello oktatóanyag, és csak szeretné a Hive, Pig és MapReduce tooget hello a teljes minta PowerShell-parancsfájlok?</span><span class="sxs-lookup"><span data-stu-id="1fa13-116">Don't have time toocomplete hello tutorial and just want tooget hello full sample PowerShell scripts for Hive, Pig, and MapReduce?</span></span> <span data-ttu-id="1fa13-117">Nem probléma, azokat [Itt][hdinsight-samples].</span><span class="sxs-lookup"><span data-stu-id="1fa13-117">Not a problem, get them [here][hdinsight-samples].</span></span> <span data-ttu-id="1fa13-118">hello letöltési hello hql, a pig és a java fájljait a következő mintákat is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1fa13-118">hello download also contains hello hql, pig, and java files for these samples.</span></span>

## <span data-ttu-id="1fa13-119"><a name="NewestVersion"></a>Legújabb verziója</span><span class="sxs-lookup"><span data-stu-id="1fa13-119"><a name="NewestVersion"></a>Newest Version</span></span>
<table border='1'>
    <tr><th><span data-ttu-id="1fa13-120">Hadoop Connector ezen verziója</span><span class="sxs-lookup"><span data-stu-id="1fa13-120">Hadoop Connector Version</span></span></th>
        <td><span data-ttu-id="1fa13-121">1.2.0</span><span class="sxs-lookup"><span data-stu-id="1fa13-121">1.2.0</span></span></td></tr>
    <tr><th><span data-ttu-id="1fa13-122">Parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="1fa13-122">Script Uri</span></span></th>
        <td><span data-ttu-id="1fa13-123">https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</span><span class="sxs-lookup"><span data-stu-id="1fa13-123">https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</span></span></td></tr>
    <tr><th><span data-ttu-id="1fa13-124">Módosítás dátuma</span><span class="sxs-lookup"><span data-stu-id="1fa13-124">Date Modified</span></span></th>
        <td><span data-ttu-id="1fa13-125">04/26/2016</span><span class="sxs-lookup"><span data-stu-id="1fa13-125">04/26/2016</span></span></td></tr>
    <tr><th><span data-ttu-id="1fa13-126">Támogatott HDInsight-verziókról</span><span class="sxs-lookup"><span data-stu-id="1fa13-126">Supported HDInsight Versions</span></span></th>
        <td><span data-ttu-id="1fa13-127">3.1, 3.2</span><span class="sxs-lookup"><span data-stu-id="1fa13-127">3.1, 3.2</span></span></td></tr>
    <tr><th><span data-ttu-id="1fa13-128">Módosítási napló</span><span class="sxs-lookup"><span data-stu-id="1fa13-128">Change Log</span></span></th>
        <td><span data-ttu-id="1fa13-129">Az Azure Cosmos DB Java SDK too1.6.0 frissítése</span><span class="sxs-lookup"><span data-stu-id="1fa13-129">Updated Azure Cosmos DB Java SDK too1.6.0</span></span></br>
            <span data-ttu-id="1fa13-130">A forrás-és a fogadó particionált gyűjtemények támogatása</span><span class="sxs-lookup"><span data-stu-id="1fa13-130">Added support for partitioned collections as both a source and sink</span></span></br>
        </td></tr>
</table>

## <span data-ttu-id="1fa13-131"><a name="Prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1fa13-131"><a name="Prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="1fa13-132">Mielőtt hozzálátna hello utasításokat ebben az oktatóanyagban, ellenőrizze, hogy hello következő:</span><span class="sxs-lookup"><span data-stu-id="1fa13-132">Before following hello instructions in this tutorial, ensure that you have hello following:</span></span>

* <span data-ttu-id="1fa13-133">Egy Cosmos-DB-fiók, adatbázis és a dokumentumok egy gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="1fa13-133">A Cosmos DB account, a database, and a collection with documents inside.</span></span> <span data-ttu-id="1fa13-134">További információkért lásd: [Ismerkedés a Cosmos DB][getting-started].</span><span class="sxs-lookup"><span data-stu-id="1fa13-134">For more information, see [Getting Started with Cosmos DB][getting-started].</span></span> <span data-ttu-id="1fa13-135">Minta adatok importálása hello Cosmos DB fiókját [Cosmos DB import eszközt][import-data].</span><span class="sxs-lookup"><span data-stu-id="1fa13-135">Import sample data into your Cosmos DB account with hello [Cosmos DB import tool][import-data].</span></span>
* <span data-ttu-id="1fa13-136">Átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="1fa13-136">Throughput.</span></span> <span data-ttu-id="1fa13-137">Beolvassa és a HDInsight-ból írások beleszámítanak a meghatározott időn belül kérelemegység a gyűjteményekben felé.</span><span class="sxs-lookup"><span data-stu-id="1fa13-137">Reads and writes from HDInsight will be counted towards your allotted request units for your collections.</span></span>
* <span data-ttu-id="1fa13-138">Egyes belül egy további tárolt eljárást a kapacitás kimeneti gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="1fa13-138">Capacity for an additional stored procedure within each output collection.</span></span> <span data-ttu-id="1fa13-139">hello tárolt eljárások az eredményül kapott dokumentumok átvitele használnak.</span><span class="sxs-lookup"><span data-stu-id="1fa13-139">hello stored procedures are used for transferring resulting documents.</span></span>
* <span data-ttu-id="1fa13-140">Hello eredményül kapott dokumentumok hello Hive, Pig vagy MapReduce-feladatok kapacitása.</span><span class="sxs-lookup"><span data-stu-id="1fa13-140">Capacity for hello resulting documents from hello Hive, Pig, or MapReduce jobs.</span></span>
* <span data-ttu-id="1fa13-141">[*Nem kötelező*] kapacitásának meghatározása egy további gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="1fa13-141">[*Optional*] Capacity for an additional collection.</span></span>

> [!WARNING]
> <span data-ttu-id="1fa13-142">A sorrend tooavoid hello létrehozási hello feladatok az új gyűjtemény vagy hello eredmények toostdout nyomtatása, elmentheti hello kimeneti tooyour WASB tároló, vagy adjon meg egy már létező gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="1fa13-142">In order tooavoid hello creation of a new collection during any of hello jobs, you can either print hello results toostdout, save hello output tooyour WASB container, or specify an already existing collection.</span></span> <span data-ttu-id="1fa13-143">Hello esetben adja meg egy meglévő gyűjteményt, új dokumentumok hello gyűjtemény belül létrejön, és már meglévő dokumentumokat csak az érintett ütközése esetén *azonosítók*.</span><span class="sxs-lookup"><span data-stu-id="1fa13-143">In hello case of specifying an existing collection, new documents will be created inside hello collection and already existing documents will only be affected if there is a conflict in *ids*.</span></span> <span data-ttu-id="1fa13-144">**hello összekötő automatikusan felülírja meglévő dokumentumok ütközését**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-144">**hello connector will automatically overwrite existing documents with id conflicts**.</span></span> <span data-ttu-id="1fa13-145">Ez a szolgáltatás úgy, hogy hello upsert beállítás toofalse kikapcsolható.</span><span class="sxs-lookup"><span data-stu-id="1fa13-145">You can turn off this feature by setting hello upsert option toofalse.</span></span> <span data-ttu-id="1fa13-146">Ha upsert értéke false, és ütközés lép fel, hello Hadoop-feladat meghiúsul; egy azonosító ütközés hibát jelez.</span><span class="sxs-lookup"><span data-stu-id="1fa13-146">If upsert is false and a conflict occurs, hello Hadoop job will fail; reporting an id conflict error.</span></span>
>
>

## <span data-ttu-id="1fa13-147"><a name="ProvisionHDInsight"></a>1. lépés:, Hozzon létre egy új HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="1fa13-147"><a name="ProvisionHDInsight"></a>Step 1: Create a new HDInsight cluster</span></span>
<span data-ttu-id="1fa13-148">Ez az oktatóanyag használ parancsfájlművelet a hello Azure Portal toocustomize a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1fa13-148">This tutorial uses Script Action from hello Azure Portal toocustomize your HDInsight cluster.</span></span> <span data-ttu-id="1fa13-149">Ebben az oktatóanyagban használjuk hello Azure Portal toocreate a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1fa13-149">In this tutorial, we will use hello Azure Portal toocreate your HDInsight cluster.</span></span> <span data-ttu-id="1fa13-150">Útmutatásért hogyan toouse PowerShell-parancsmagok vagy hello HDInsight .NET SDK-t, tekintse meg a [testreszabása HDInsight-fürtök használata parancsfájlművelet] [ hdinsight-custom-provision] cikk.</span><span class="sxs-lookup"><span data-stu-id="1fa13-150">For instructions on how toouse PowerShell cmdlets or hello HDInsight .NET SDK, check out the [Customize HDInsight clusters using Script Action][hdinsight-custom-provision] article.</span></span>

1. <span data-ttu-id="1fa13-151">Jelentkezzen be toohello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1fa13-151">Sign in toohello [Azure Portal][azure-portal].</span></span>
2. <span data-ttu-id="1fa13-152">Kattintson a **+ új** keresése a bal oldali navigációs hello hello felső részén **HDInsight** hello felső keresősávban hello új panelen.</span><span class="sxs-lookup"><span data-stu-id="1fa13-152">Click **+ New** on hello top of hello left navigation, search for **HDInsight** in hello top search bar on hello New blade.</span></span>
3. <span data-ttu-id="1fa13-153">**HDInsight** által közzétett **Microsoft** hello eredmények hello tetején fog megjelenni.</span><span class="sxs-lookup"><span data-stu-id="1fa13-153">**HDInsight** published by **Microsoft** will appear at hello top of hello Results.</span></span> <span data-ttu-id="1fa13-154">Kattintson rá, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-154">Click on it and then click **Create**.</span></span>
4. <span data-ttu-id="1fa13-155">Hozzon létre új HDInsight-fürt hello panel, adja meg a **fürtnév** és select hello **előfizetés** ehhez az erőforráshoz a tooprovision szeretné.</span><span class="sxs-lookup"><span data-stu-id="1fa13-155">On hello New HDInsight Cluster create blade, enter your **Cluster Name** and select hello **Subscription** you want tooprovision this resource under.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="1fa13-156">Fürt neve</span><span class="sxs-lookup"><span data-stu-id="1fa13-156">Cluster name</span></span></td><td><span data-ttu-id="1fa13-157">Hello fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="1fa13-157">Name hello cluster.</span></span><br/>
<span data-ttu-id="1fa13-158">DNS-beli név kell elindítani egy alfanumerikus karakterrel végződik és kötőjeleket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1fa13-158">DNS name must start and end with an alpha numeric character, and may contain dashes.</span></span><br/>
<span data-ttu-id="1fa13-159">hello mező 3 és 63 karakter közötti karakterláncnak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1fa13-159">hello field must be a string between 3 and 63 characters.</span></span></td></tr>
        <tr><td><span data-ttu-id="1fa13-160">Előfizetés neve</span><span class="sxs-lookup"><span data-stu-id="1fa13-160">Subscription Name</span></span></td>
            <td><span data-ttu-id="1fa13-161">Ha több Azure-előfizetéssel rendelkezik, válassza ki a hello-előfizetéssel, amely a HDInsight-fürt tárolására.</span><span class="sxs-lookup"><span data-stu-id="1fa13-161">If you have more than one Azure Subscription, select hello subscription that will host your HDInsight cluster.</span></span> </td></tr>
    </table><span data-ttu-id="1fa13-162">
5.Kattintson a **fürt típusának kiválasztása** és a következő tulajdonságok toohello set hello megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="1fa13-162">
5. Click **Select Cluster Type** and set hello following properties toohello specified values.</span></span>

    <table border='1'>
        <tr><td><span data-ttu-id="1fa13-163">Fürttípus</span><span class="sxs-lookup"><span data-stu-id="1fa13-163">Cluster type</span></span></td><td><span data-ttu-id="1fa13-164"><strong>Hadoop</strong></span><span class="sxs-lookup"><span data-stu-id="1fa13-164"><strong>Hadoop</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="1fa13-165">Fürt réteg</span><span class="sxs-lookup"><span data-stu-id="1fa13-165">Cluster tier</span></span></td><td><span data-ttu-id="1fa13-166"><strong>Standard</strong></span><span class="sxs-lookup"><span data-stu-id="1fa13-166"><strong>Standard</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="1fa13-167">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="1fa13-167">Operating System</span></span></td><td><span data-ttu-id="1fa13-168"><strong>Windows</strong></span><span class="sxs-lookup"><span data-stu-id="1fa13-168"><strong>Windows</strong></span></span></td></tr>
        <tr><td><span data-ttu-id="1fa13-169">Verzió</span><span class="sxs-lookup"><span data-stu-id="1fa13-169">Version</span></span></td><td><span data-ttu-id="1fa13-170">legújabb verzió</span><span class="sxs-lookup"><span data-stu-id="1fa13-170">latest version</span></span></td></tr>
    </table>

    <span data-ttu-id="1fa13-171">Most kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-171">Now, click **SELECT**.</span></span>

    ![Adja meg a Hadoop HDInsight a fürtcsomópontok kezdeti adatait][image-customprovision-page1]
6. <span data-ttu-id="1fa13-173">Kattintson a **hitelesítő adatok** tooset a bejelentkezési és távelérési hitelesítő adatainak.</span><span class="sxs-lookup"><span data-stu-id="1fa13-173">Click on **Credentials** tooset your login and remote access credentials.</span></span> <span data-ttu-id="1fa13-174">Válassza ki a **a fürt bejelentkezési felhasználónevének** és **a fürt bejelentkezési jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-174">Choose your **Cluster Login Username** and **Cluster Login Password**.</span></span>

    <span data-ttu-id="1fa13-175">Ha tooremote a fürtbe, jelölje be *Igen* hello hello panel alsó részén, és adja meg a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="1fa13-175">If you want tooremote into your cluster, select *yes* at hello bottom of hello blade and provide a username and password.</span></span>
7. <span data-ttu-id="1fa13-176">Kattintson a **adatforrás** tooset az elsődleges helyet adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1fa13-176">Click on **Data Source** tooset your primary location for data access.</span></span> <span data-ttu-id="1fa13-177">Válassza ki a hello **kijelöléséről** , és adjon meg egy már meglévő tárfiókot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="1fa13-177">Choose hello **Selection Method** and specify an already existing storage account or create a new one.</span></span>
8. <span data-ttu-id="1fa13-178">Az azonos panel hello, adjon meg egy **alapértelmezett tároló** és egy **hely**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-178">On hello same blade, specify a **Default Container** and a **Location**.</span></span> <span data-ttu-id="1fa13-179">Majd kattintson a gombra **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-179">And, click **SELECT**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1fa13-180">Válassza ki a hely Bezárás tooyour Cosmos DB régióját a jobb teljesítmény</span><span class="sxs-lookup"><span data-stu-id="1fa13-180">Select a location close tooyour Cosmos DB account region for better performance</span></span>
   >
   >
9. <span data-ttu-id="1fa13-181">Kattintson a **árazás** tooselect hello számát és típusát a csomópontok.</span><span class="sxs-lookup"><span data-stu-id="1fa13-181">Click on **Pricing** tooselect hello number and type of nodes.</span></span> <span data-ttu-id="1fa13-182">Beállíthatja, hogy hello alapértelmezett konfiguráció és a feldolgozó csomópontok száma méretezési hello később.</span><span class="sxs-lookup"><span data-stu-id="1fa13-182">You can keep hello default configuration and scale hello number of Worker nodes later on.</span></span>
10. <span data-ttu-id="1fa13-183">Kattintson a **opcionális konfigurációs**, majd **Parancsfájlműveletek** hello opcionális konfigurációs panelen található.</span><span class="sxs-lookup"><span data-stu-id="1fa13-183">Click **Optional Configuration**, then **Script Actions** in hello Optional Configuration Blade.</span></span>

     <span data-ttu-id="1fa13-184">A Parancsfájlműveletek adja meg a következő információk toocustomize hello a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1fa13-184">In Script Actions, enter hello following information toocustomize your HDInsight cluster.</span></span>

     <table border='1'>
         <tr><th><span data-ttu-id="1fa13-185">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="1fa13-185">Property</span></span></th><th><span data-ttu-id="1fa13-186">Érték</span><span class="sxs-lookup"><span data-stu-id="1fa13-186">Value</span></span></th></tr>
         <tr><td><span data-ttu-id="1fa13-187">Név</span><span class="sxs-lookup"><span data-stu-id="1fa13-187">Name</span></span></td>
             <td><span data-ttu-id="1fa13-188">Adja meg a hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="1fa13-188">Specify a name for hello script action.</span></span></td></tr>
         <tr><td><span data-ttu-id="1fa13-189">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="1fa13-189">Script URI</span></span></td>
             <td><span data-ttu-id="1fa13-190">Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</span><span class="sxs-lookup"><span data-stu-id="1fa13-190">Specify hello URI toohello script that is invoked toocustomize hello cluster.</span></span></br></br>
<span data-ttu-id="1fa13-191">Adja meg:</span><span class="sxs-lookup"><span data-stu-id="1fa13-191">Please enter:</span></span> </br> <span data-ttu-id="1fa13-192"><strong>https://portalcontent.BLOB.Core.Windows.NET/scriptaction/documentdb-hadoop-Installer-v04.ps1</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-192"><strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v04.ps1</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="1fa13-193">Fej</span><span class="sxs-lookup"><span data-stu-id="1fa13-193">Head</span></span></td>
             <td><span data-ttu-id="1fa13-194">Kattintson a hello jelölőnégyzet toorun hello hello átjárócsomópont PowerShell parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1fa13-194">Click hello checkbox toorun hello PowerShell script onto hello Head node.</span></span></br></br><span data-ttu-id="1fa13-195">
             <strong>Ezt a jelölőnégyzetet</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-195">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="1fa13-196">Munkavégző</span><span class="sxs-lookup"><span data-stu-id="1fa13-196">Worker</span></span></td>
             <td><span data-ttu-id="1fa13-197">Kattintson a hello jelölőnégyzet toorun hello PowerShell parancsfájlt hello munkavégző csomópont.</span><span class="sxs-lookup"><span data-stu-id="1fa13-197">Click hello checkbox toorun hello PowerShell script onto hello Worker node.</span></span></br></br><span data-ttu-id="1fa13-198">
             <strong>Ezt a jelölőnégyzetet</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-198">
             <strong>Check this checkbox</strong>.</span></span></td></tr>
         <tr><td><span data-ttu-id="1fa13-199">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="1fa13-199">Zookeeper</span></span></td>
             <td><span data-ttu-id="1fa13-200">Kattintson a hello jelölőnégyzet toorun hello PowerShell parancsfájl hello Zookeeper-kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="1fa13-200">Click hello checkbox toorun hello PowerShell script onto hello Zookeeper.</span></span></br></br><span data-ttu-id="1fa13-201">
             <strong>Nincs szükség</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-201">
             <strong>Not needed</strong>.</span></span>
             </td></tr>
         <tr><td><span data-ttu-id="1fa13-202">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="1fa13-202">Parameters</span></span></td>
             <td><span data-ttu-id="1fa13-203">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="1fa13-203">Specify hello parameters, if required by hello script.</span></span></br></br><span data-ttu-id="1fa13-204">
             <strong>Nem szükséges paramétereket</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-204">
             <strong>No Parameters needed</strong>.</span></span></td></tr>
     </table><span data-ttu-id="1fa13-205">
11.Vagy hozzon létre egy új **erőforráscsoport** , vagy használjon egy meglévő erőforráscsoportot az Azure-előfizetéshez tartozó.</span><span class="sxs-lookup"><span data-stu-id="1fa13-205">
11. Create either a new **Resource Group** or use an existing Resource Group under your Azure Subscription.</span></span>
<span data-ttu-id="1fa13-206">12.</span><span class="sxs-lookup"><span data-stu-id="1fa13-206">12.</span></span> <span data-ttu-id="1fa13-207">Ellenőrizze, **PIN-kód toodashboard** tootrack kattintson, és a központi telepítési **létrehozása**!</span><span class="sxs-lookup"><span data-stu-id="1fa13-207">Now, check **Pin toodashboard** tootrack its deployment and click **Create**!</span></span>

## <span data-ttu-id="1fa13-208"><a name="InstallCmdlets"></a>2. lépés: Telepítse és konfigurálja az Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1fa13-208"><a name="InstallCmdlets"></a>Step 2: Install and configure Azure PowerShell</span></span>
1. <span data-ttu-id="1fa13-209">Az Azure PowerShell telepítése.</span><span class="sxs-lookup"><span data-stu-id="1fa13-209">Install Azure PowerShell.</span></span> <span data-ttu-id="1fa13-210">Útmutatás található [Itt][powershell-install-configure].</span><span class="sxs-lookup"><span data-stu-id="1fa13-210">Instructions can be found [here][powershell-install-configure].</span></span>

   > [!NOTE]
   > <span data-ttu-id="1fa13-211">Azt is megteheti csak a Hive-lekérdezéseket, használhatja a HDInsight tartozó online Hive szerkesztő.</span><span class="sxs-lookup"><span data-stu-id="1fa13-211">Alternatively, just for Hive queries, you can use HDInsight's online Hive Editor.</span></span> <span data-ttu-id="1fa13-212">toodo tehát bejelentkezés toohello [Azure Portal][azure-portal], kattintson a **HDInsight** on hello bal oldali ablaktáblán tooview a HDInsight-fürtök listáját.</span><span class="sxs-lookup"><span data-stu-id="1fa13-212">toodo so, sign in toohello [Azure Portal][azure-portal], click **HDInsight** on hello left pane tooview a list of your HDInsight clusters.</span></span> <span data-ttu-id="1fa13-213">Kattintson a kívánt toorun Hive-lekérdezéseket, és kattintson a hello fürt **lekérdezés konzol**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-213">Click hello cluster you want toorun Hive queries on, and then click **Query Console**.</span></span>
   >
   >
2. <span data-ttu-id="1fa13-214">Nyissa meg az Azure PowerShell integrált parancsfájlkezelési környezet hello:</span><span class="sxs-lookup"><span data-stu-id="1fa13-214">Open hello Azure PowerShell Integrated Scripting Environment:</span></span>

   * <span data-ttu-id="1fa13-215">A Windows 8 vagy Windows Server 2012 vagy újabb rendszerű számítógépen is használhatja hello beépített keresési.</span><span class="sxs-lookup"><span data-stu-id="1fa13-215">On a computer running Windows 8 or Windows Server 2012 or higher, you can use hello built-in Search.</span></span> <span data-ttu-id="1fa13-216">Hello kezdőképernyőről írja be a **powershell ise** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-216">From hello Start screen, type **powershell ise** and click **Enter**.</span></span>
   * <span data-ttu-id="1fa13-217">A Windows 8 vagy Windows Server 2012-nél korábbi rendszerű számítógépen hello Start menü használata.</span><span class="sxs-lookup"><span data-stu-id="1fa13-217">On a computer running a version earlier than Windows 8 or Windows Server 2012, use hello Start menu.</span></span> <span data-ttu-id="1fa13-218">Hello Start menüben írja be a **parancssor** hello keresési mezőbe, majd az eredmények hello listában, kattintson a **parancssor**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-218">From hello Start menu, type **Command Prompt** in hello search box, then in hello list of results, click **Command Prompt**.</span></span> <span data-ttu-id="1fa13-219">Hello parancssort, írja be **powershell_ise** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-219">In hello Command Prompt, type **powershell_ise** and click **Enter**.</span></span>
3. <span data-ttu-id="1fa13-220">Adja hozzá az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1fa13-220">Add your Azure Account.</span></span>

   1. <span data-ttu-id="1fa13-221">A konzol ablaktáblában hello, írja be **Add-AzureAccount** kattintson **Enter**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-221">In hello Console Pane, type **Add-AzureAccount** and click **Enter**.</span></span>
   2. <span data-ttu-id="1fa13-222">Írja be az Azure-előfizetéshez társított hello e-mail címét, és kattintson a **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-222">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span>
   3. <span data-ttu-id="1fa13-223">Adja meg az Azure-előfizetéshez hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="1fa13-223">Type in hello password for your Azure subscription.</span></span>
   4. <span data-ttu-id="1fa13-224">Kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1fa13-224">Click **Sign in**.</span></span>
4. <span data-ttu-id="1fa13-225">a következő diagram hello hello fontos részek az Azure PowerShell-parancsfájl-kezelési környezet azonosítja.</span><span class="sxs-lookup"><span data-stu-id="1fa13-225">hello following diagram identifies hello important parts of your Azure PowerShell Scripting Environment.</span></span>

    ![Az Azure PowerShell diagramja][azure-powershell-diagram]

## <span data-ttu-id="1fa13-227"><a name="RunHive"></a>3. lépés: Egy Cosmos-adatbázis és a HDInsight Hive-feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="1fa13-227"><a name="RunHive"></a>Step 3: Run a Hive job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1fa13-228">< > Által jelzett minden értéket meg kell adni a konfigurációs beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="1fa13-228">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="1fa13-229">A következő változók a PowerShell-parancsfájl ablaktáblán hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="1fa13-229">Set hello following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name, hello Azure Storage account and container that is used for hello default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide hello HDInsight cluster name where you want toorun hello Hive job.
        $clusterName = "<HDInsightClusterName>"
2. <p><span data-ttu-id="1fa13-230">Most először hoz létre, a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1fa13-230">Let's begin constructing your query string.</span></span> <span data-ttu-id="1fa13-231">Hive lekérdezés összes dokumentumot (_ts) rendszer által létrehozott időbélyegeket és egyedi azonosítók (_rid) egy Azure Cosmos DB gyűjteményből, összes dokumentumot számolja hello percenként, majd tárolja egy új Azure Cosmos DB gyűjteménybe hello eredmények azt fog írni.</span><span class="sxs-lookup"><span data-stu-id="1fa13-231">We'll write a Hive query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by hello minute, and then stores hello results back into a new Azure Cosmos DB collection.</span></span></p>

    <p><span data-ttu-id="1fa13-232">Először hozzon létre olyan Hive táblát az Azure Cosmos DB gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="1fa13-232">First, let's create a Hive table from our Azure Cosmos DB collection.</span></span> <span data-ttu-id="1fa13-233">Adja hozzá a következő kód részlet toohello PowerShell-parancsfájl ablaktábla hello <strong>után</strong> hello kódrészletet # 1.</span><span class="sxs-lookup"><span data-stu-id="1fa13-233">Add hello following code snippet toohello PowerShell Script pane <strong>after</strong> hello code snippet from #1.</span></span> <span data-ttu-id="1fa13-234">Ellenőrizze, hogy a dokumentumok toojust _ts és _rid hello választható DocumentDB.query paraméter t vágás telepíthet.</span><span class="sxs-lookup"><span data-stu-id="1fa13-234">Make sure you include hello optional DocumentDB.query parameter t trim our documents toojust _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="1fa13-235">**DocumentDB.inputCollections elnevezési nem hiba volt.**</span><span class="sxs-lookup"><span data-stu-id="1fa13-235">**Naming DocumentDB.inputCollections was not a mistake.**</span></span> <span data-ttu-id="1fa13-236">Igen, azt felvételének engedélyezése több gyűjtemény bemenetként:</span><span class="sxs-lookup"><span data-stu-id="1fa13-236">Yes, we allow adding multiple collections as an input:</span></span> </br>
   >
   >

        '*DocumentDB.inputCollections*' = '*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*' A1A</br> hello collection names are separated without spaces, using only a single comma.

        # Create a Hive table using data from DocumentDB. Pass DocumentDB hello query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  +
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " +
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

1. <span data-ttu-id="1fa13-237">Következő lépésként hozzon létre egy Hive tábla hello kimeneti gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="1fa13-237">Next, let's create a Hive table for hello output collection.</span></span> <span data-ttu-id="1fa13-238">hello kimeneti dokumentumtulajdonságok lesz hello hónap, nap, óra, perc és hello előfordulások száma.</span><span class="sxs-lookup"><span data-stu-id="1fa13-238">hello output document properties will be hello month, day, hour, minute, and hello total number of occurrences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1fa13-239">**Még újra DocumentDB.outputCollections elnevezési nem hiba volt.**</span><span class="sxs-lookup"><span data-stu-id="1fa13-239">**Yet again, naming DocumentDB.outputCollections was not a mistake.**</span></span> <span data-ttu-id="1fa13-240">Igen, azt felvételének engedélyezése több gyűjtemény kimenetként:</span><span class="sxs-lookup"><span data-stu-id="1fa13-240">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="1fa13-241">"*DocumentDB.outputCollections*'='*\<DocumentDB-gyűjtemény Kimenetnév 1\>*,*\<DocumentDB-gyűjtemény Kimenetnév 2\>*"</span><span class="sxs-lookup"><span data-stu-id="1fa13-241">'*DocumentDB.outputCollections*' = '*\<DocumentDB Output Collection Name 1\>*,*\<DocumentDB Output Collection Name 2\>*'</span></span> </br> <span data-ttu-id="1fa13-242">hello gyűjteménynevek nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="1fa13-242">hello collection names are separated without spaces, using only a single comma.</span></span> </br></br>
   > <span data-ttu-id="1fa13-243">Dokumentumok lesz elosztott ciklikus multiplexelés több gyűjtemények között.</span><span class="sxs-lookup"><span data-stu-id="1fa13-243">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="1fa13-244">A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni hello tovább gyűjteményben, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="1fa13-244">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>
   >
   >

       # Create a Hive table for hello output data tooDocumentDB.
       $queryStringPart2 = "drop table DocumentDB_analytics; " +
                             "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                             "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " +
                             "tblproperties ( " +
                                 "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                 "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                 "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                 "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "
2. <span data-ttu-id="1fa13-245">Végezetül most egyeztetési hello dokumentumok által hónap, nap, óra és perc és a Beszúrás hello eredmények hello programba kimeneti Hive tábla.</span><span class="sxs-lookup"><span data-stu-id="1fa13-245">Finally, let's tally hello documents by month, day, hour, and minute and insert hello results back into hello output Hive table.</span></span>

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "
3. <span data-ttu-id="1fa13-246">Adja hozzá a hello parancsfájl részlet toocreate Hive feladat definícióját követő hello előző lekérdezésből.</span><span class="sxs-lookup"><span data-stu-id="1fa13-246">Add hello following script snippet toocreate a Hive job definition from hello previous query.</span></span>

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    <span data-ttu-id="1fa13-247">Is hello - fájl kapcsoló toospecify egy HDFS a HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="1fa13-247">You can also use hello -File switch toospecify a HiveQL script file on HDFS.</span></span>
4. <span data-ttu-id="1fa13-248">Adja hozzá a következő kódrészletet toosave hello kezdési ideje hello és hello Hive feladat elküldéséhez.</span><span class="sxs-lookup"><span data-stu-id="1fa13-248">Add hello following snippet toosave hello start time and submit hello Hive job.</span></span>

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition
5. <span data-ttu-id="1fa13-249">Adja hozzá a következő toowait hello Hive feladat toocomplete a hello.</span><span class="sxs-lookup"><span data-stu-id="1fa13-249">Add hello following toowait for hello Hive job toocomplete.</span></span>

        # Wait for hello Hive job toocomplete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600
6. <span data-ttu-id="1fa13-250">Adja hozzá a következő szabványos-es számú tooprint hello kimeneti és hello rendszerhez készült start hello és befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="1fa13-250">Add hello following tooprint hello standard output and hello start and end times.</span></span>

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
7. <span data-ttu-id="1fa13-251">**Futtatás** az új parancsfájl!</span><span class="sxs-lookup"><span data-stu-id="1fa13-251">**Run** your new script!</span></span> <span data-ttu-id="1fa13-252">**Kattintson a** hello zöld végrehajtás gombra.</span><span class="sxs-lookup"><span data-stu-id="1fa13-252">**Click** hello green execute button.</span></span>
8. <span data-ttu-id="1fa13-253">Hello eredmények ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1fa13-253">Check hello results.</span></span> <span data-ttu-id="1fa13-254">Jelentkezzen be a hello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1fa13-254">Sign into hello [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="1fa13-255">Kattintson a <strong>Tallózás</strong> hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="1fa13-255">Click <strong>Browse</strong> on hello left-side panel.</span></span> </br>
   2. <span data-ttu-id="1fa13-256">Kattintson a <strong>mindent</strong> hello felső – jobb hello Tallózás panel.</span><span class="sxs-lookup"><span data-stu-id="1fa13-256">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span> </br>
   3. <span data-ttu-id="1fa13-257">Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-257">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
   4. <span data-ttu-id="1fa13-258">Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> megadott hello kimeneti gyűjteményhez társított a Hive-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="1fa13-258">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your Hive query.</span></span></br>
   5. <span data-ttu-id="1fa13-259">Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-259">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

   <span data-ttu-id="1fa13-260">A Hive-lekérdezések eredményeinek hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1fa13-260">You will see hello results of your Hive query.</span></span>

   ![Hive lekérdezés eredményei][image-hive-query-results]

## <span data-ttu-id="1fa13-262"><a name="RunPig"></a>4. lépés: A Pig feladatot Cosmos DB és HDInsight használatával futtassa</span><span class="sxs-lookup"><span data-stu-id="1fa13-262"><a name="RunPig"></a>Step 4: Run a Pig job using Cosmos DB and HDInsight</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1fa13-263">< > Által jelzett minden értéket meg kell adni a konfigurációs beállítások használata.</span><span class="sxs-lookup"><span data-stu-id="1fa13-263">All variables indicated by < > must be filled in using your configuration settings.</span></span>
>
>

1. <span data-ttu-id="1fa13-264">A következő változók a PowerShell-parancsfájl ablaktáblán hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="1fa13-264">Set hello following variables in your PowerShell Script pane.</span></span>

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want toorun hello Pig job.
        $clusterName = "Azure HDInsight Cluster Name"
2. <p><span data-ttu-id="1fa13-265">Most először hoz létre, a lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1fa13-265">Let's begin constructing your query string.</span></span> <span data-ttu-id="1fa13-266">Pig lekérdezés összes dokumentumot (_ts) rendszer által létrehozott időbélyegeket és egyedi azonosítók (_rid) egy Azure Cosmos DB gyűjteményből, összes dokumentumot számolja hello percenként, majd tárolja egy új Azure Cosmos DB gyűjteménybe hello eredmények azt fog írni.</span><span class="sxs-lookup"><span data-stu-id="1fa13-266">We'll write a Pig query that takes all documents' system generated timestamps (_ts) and unique ids (_rid) from an Azure Cosmos DB collection, tallies all documents by hello minute, and then stores hello results back into a new Azure Cosmos DB collection.</span></span></p>
    <p><span data-ttu-id="1fa13-267">Első lépésként betölteni a HDInsight a Cosmos-Adatbázisból dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="1fa13-267">First, load documents from Cosmos DB into HDInsight.</span></span> <span data-ttu-id="1fa13-268">Adja hozzá a következő kód részlet toohello PowerShell-parancsfájl ablaktábla hello <strong>után</strong> hello kódrészletet # 1.</span><span class="sxs-lookup"><span data-stu-id="1fa13-268">Add hello following code snippet toohello PowerShell Script pane <strong>after</strong> hello code snippet from #1.</span></span> <span data-ttu-id="1fa13-269">Győződjön meg arról, hogy a dokumentumok toojust _ts és _rid tooadd a DocumentDB lekérdezése az toohello választható DocumentDB lekérdezési paraméter tootrim.</span><span class="sxs-lookup"><span data-stu-id="1fa13-269">Make sure tooadd a DocumentDB query toohello optional DocumentDB query parameter tootrim our documents toojust _ts and _rid.</span></span></p>

   > [!NOTE]
   > <span data-ttu-id="1fa13-270">Igen, azt felvételének engedélyezése több gyűjtemény bemenetként:</span><span class="sxs-lookup"><span data-stu-id="1fa13-270">Yes, we allow adding multiple collections as an input:</span></span> </br>
   > <span data-ttu-id="1fa13-271">"*\<DocumentDB bemeneti gyűjtemény neve 1\>*,*\<DocumentDB bemeneti gyűjteménynév 2\>*"</span><span class="sxs-lookup"><span data-stu-id="1fa13-271">'*\<DocumentDB Input Collection Name 1\>*,*\<DocumentDB Input Collection Name 2\>*'</span></span></br> <span data-ttu-id="1fa13-272">hello gyűjteménynevek nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="1fa13-272">hello collection names are separated without spaces, using only a single comma.</span></span> </b>
   >
   >

    <span data-ttu-id="1fa13-273">Dokumentumok lesz elosztott ciklikus multiplexelés több gyűjtemények között.</span><span class="sxs-lookup"><span data-stu-id="1fa13-273">Documents will be distributed round-robin across multiple collections.</span></span> <span data-ttu-id="1fa13-274">A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni hello tovább gyűjteményben, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="1fa13-274">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>

        # Load data from Cosmos DB. Pass DocumentDB query toofilter transferred data too_rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
3. <span data-ttu-id="1fa13-275">A következő most egyeztetési hello dokumentumok hello hónap, nap, óra, perc és hello előfordulások száma szerint.</span><span class="sxs-lookup"><span data-stu-id="1fa13-275">Next, let's tally hello documents by hello month, day, hour, minute, and hello total number of occurrences.</span></span>

       # GROUP BY minute and COUNT entries for each.
       $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                           "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                           "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "
4. <span data-ttu-id="1fa13-276">Végezetül most hello eredmények tárolására az új kimeneti gyűjteménybe.</span><span class="sxs-lookup"><span data-stu-id="1fa13-276">Finally, let's store hello results into our new output collection.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1fa13-277">Igen, azt felvételének engedélyezése több gyűjtemény kimenetként:</span><span class="sxs-lookup"><span data-stu-id="1fa13-277">Yes, we allow adding multiple collections as an output:</span></span> </br>
   > <span data-ttu-id="1fa13-278">"\<DocumentDB-gyűjtemény Kimenetnév 1\>,\<DocumentDB-gyűjtemény Kimenetnév 2\>"</span><span class="sxs-lookup"><span data-stu-id="1fa13-278">'\<DocumentDB Output Collection Name 1\>,\<DocumentDB Output Collection Name 2\>'</span></span></br> <span data-ttu-id="1fa13-279">hello gyűjteménynevek nélkül szóközt tartalmaz, csak egyetlen vesszővel válassza el egymástól.</span><span class="sxs-lookup"><span data-stu-id="1fa13-279">hello collection names are separated without spaces, using only a single comma.</span></span></br>
   > <span data-ttu-id="1fa13-280">Dokumentumok lesz több gyűjteményt kell elosztott ciklikus multiplexelés hello között.</span><span class="sxs-lookup"><span data-stu-id="1fa13-280">Documents will be distributed round-robin across hello multiple collections.</span></span> <span data-ttu-id="1fa13-281">A kötegelt dokumentumok fogja tárolni egy gyűjteményt, majd a második kötegelt dokumentumok fogja tárolni hello tovább gyűjteményben, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="1fa13-281">A batch of documents will be stored in one collection, then a second batch of documents will be stored in hello next collection, and so forth.</span></span>
   >
   >

        # Store output data tooCosmos DB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "
5. <span data-ttu-id="1fa13-282">Adja hozzá a következő parancsfájl részlet toocreate egy olyan feladatdefinícióban hello előző lekérdezésből hello.</span><span class="sxs-lookup"><span data-stu-id="1fa13-282">Add hello following script snippet toocreate a Pig job definition from hello previous query.</span></span>

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    <span data-ttu-id="1fa13-283">Is hello - fájl kapcsoló toospecify HDFS a Pig parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="1fa13-283">You can also use hello -File switch toospecify a Pig script file on HDFS.</span></span>
6. <span data-ttu-id="1fa13-284">Adja hozzá a következő kódrészletet toosave hello kezdési ideje hello és elküldeni a Pig feladatot hello.</span><span class="sxs-lookup"><span data-stu-id="1fa13-284">Add hello following snippet toosave hello start time and submit hello Pig job.</span></span>

        # Save hello start time and submit hello job toohello cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition
7. <span data-ttu-id="1fa13-285">Adja hozzá a következő toowait hello Pig feladatot toocomplete a hello.</span><span class="sxs-lookup"><span data-stu-id="1fa13-285">Add hello following toowait for hello Pig job toocomplete.</span></span>

        # Wait for hello Pig job toocomplete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600
8. <span data-ttu-id="1fa13-286">Adja hozzá a következő szabványos-es számú tooprint hello kimeneti és hello rendszerhez készült start hello és befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="1fa13-286">Add hello following tooprint hello standard output and hello start and end times.</span></span>

        # Print hello standard error, hello standard output of hello Hive job, and hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
9. <span data-ttu-id="1fa13-287">**Futtatás** az új parancsfájl!</span><span class="sxs-lookup"><span data-stu-id="1fa13-287">**Run** your new script!</span></span> <span data-ttu-id="1fa13-288">**Kattintson a** hello zöld végrehajtás gombra.</span><span class="sxs-lookup"><span data-stu-id="1fa13-288">**Click** hello green execute button.</span></span>
10. <span data-ttu-id="1fa13-289">Hello eredmények ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1fa13-289">Check hello results.</span></span> <span data-ttu-id="1fa13-290">Jelentkezzen be a hello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1fa13-290">Sign into hello [Azure Portal][azure-portal].</span></span>

    1. <span data-ttu-id="1fa13-291">Kattintson a <strong>Tallózás</strong> hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="1fa13-291">Click <strong>Browse</strong> on hello left-side panel.</span></span> </br>
    2. <span data-ttu-id="1fa13-292">Kattintson a <strong>mindent</strong> hello felső – jobb hello Tallózás panel.</span><span class="sxs-lookup"><span data-stu-id="1fa13-292">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span> </br>
    3. <span data-ttu-id="1fa13-293">Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-293">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span> </br>
    4. <span data-ttu-id="1fa13-294">Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> megadott hello kimeneti gyűjteményhez társított a Pig-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="1fa13-294">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your Pig query.</span></span></br>
    5. <span data-ttu-id="1fa13-295">Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-295">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span></br></p>

    <span data-ttu-id="1fa13-296">A Pig lekérdezési eredmények hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1fa13-296">You will see hello results of your Pig query.</span></span>

    ![A Pig lekérdezés eredményei][image-pig-query-results]

## <span data-ttu-id="1fa13-298"><a name="RunMapReduce"></a>5. lépés: Azure Cosmos DB és HDInsight MapReduce feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="1fa13-298"><a name="RunMapReduce"></a>Step 5: Run a MapReduce job using Azure Cosmos DB and HDInsight</span></span>
1. <span data-ttu-id="1fa13-299">A következő változók a PowerShell-parancsfájl ablaktáblán hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="1fa13-299">Set hello following variables in your PowerShell Script pane.</span></span>

        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
2. <span data-ttu-id="1fa13-300">A MapReduce feladatot, amely minden egyes dokumentum tulajdonság az Azure Cosmos DB gyűjteményből előfordulások száma hello számolja azt fogja végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="1fa13-300">We'll execute a MapReduce job that tallies hello number of occurrences for each Document property from your Azure Cosmos DB collection.</span></span> <span data-ttu-id="1fa13-301">Adja hozzá a parancsfájl részlet **után** fenti hello részlet.</span><span class="sxs-lookup"><span data-stu-id="1fa13-301">Add this script snippet **after** hello snippet above.</span></span>

        # Define hello MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

   > [!NOTE]
   > <span data-ttu-id="1fa13-302">Egyéni telepítése hello hello Cosmos DB Hadoop összekötő TallyProperties-v01.jar tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1fa13-302">TallyProperties-v01.jar comes with hello custom installation of hello Cosmos DB Hadoop Connector.</span></span>
   >
   >
3. <span data-ttu-id="1fa13-303">Adja hozzá a következő parancs toosubmit hello MapReduce feladatot hello.</span><span class="sxs-lookup"><span data-stu-id="1fa13-303">Add hello following command toosubmit hello MapReduce job.</span></span>

        # Save hello start time and submit hello job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    <span data-ttu-id="1fa13-304">Ezenkívül toohello MapReduce feladatdefiníció, itt is megadni hello HDInsight fürt toorun hello MapReduce feladatot, és hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1fa13-304">In addition toohello MapReduce job definition, you also provide hello HDInsight cluster name where you want toorun hello MapReduce job, and hello credentials.</span></span> <span data-ttu-id="1fa13-305">hello Start-AzureHDInsightJob egy aszinkron módjára való átkapcsolás hívás.</span><span class="sxs-lookup"><span data-stu-id="1fa13-305">hello Start-AzureHDInsightJob is an asynchronized call.</span></span> <span data-ttu-id="1fa13-306">hello feladat, használjon hello toocheck hello megvalósításának *várakozási-AzureHDInsightJob* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="1fa13-306">toocheck hello completion of hello job, use hello *Wait-AzureHDInsightJob* cmdlet.</span></span>
4. <span data-ttu-id="1fa13-307">Adja hozzá a következő parancs toocheck hello futó hello MapReduce feladatot hibákat.</span><span class="sxs-lookup"><span data-stu-id="1fa13-307">Add hello following command toocheck any errors with running hello MapReduce job.</span></span>

        # Get hello job output and print hello start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
5. <span data-ttu-id="1fa13-308">**Futtatás** az új parancsfájl!</span><span class="sxs-lookup"><span data-stu-id="1fa13-308">**Run** your new script!</span></span> <span data-ttu-id="1fa13-309">**Kattintson a** hello zöld végrehajtás gombra.</span><span class="sxs-lookup"><span data-stu-id="1fa13-309">**Click** hello green execute button.</span></span>
6. <span data-ttu-id="1fa13-310">Hello eredmények ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1fa13-310">Check hello results.</span></span> <span data-ttu-id="1fa13-311">Jelentkezzen be a hello [Azure Portal][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="1fa13-311">Sign into hello [Azure Portal][azure-portal].</span></span>

   1. <span data-ttu-id="1fa13-312">Kattintson a <strong>Tallózás</strong> hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="1fa13-312">Click <strong>Browse</strong> on hello left-side panel.</span></span>
   2. <span data-ttu-id="1fa13-313">Kattintson a <strong>mindent</strong> hello felső – jobb hello Tallózás panel.</span><span class="sxs-lookup"><span data-stu-id="1fa13-313">Click <strong>everything</strong> at hello top-right of hello browse panel.</span></span>
   3. <span data-ttu-id="1fa13-314">Keresse meg és kattintson a <strong>Azure Cosmos DB fiókok</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-314">Find and click <strong>Azure Cosmos DB Accounts</strong>.</span></span>
   4. <span data-ttu-id="1fa13-315">Ezt követően keresse meg a <strong>Azure Cosmos DB fiók</strong>, majd <strong>Azure Cosmos DB adatbázis</strong> és a <strong>Azure Cosmos DB gyűjtemény</strong> megadott hello kimeneti gyűjteményhez társított a MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="1fa13-315">Next, find your <strong>Azure Cosmos DB Account</strong>, then <strong>Azure Cosmos DB Database</strong> and your <strong>Azure Cosmos DB Collection</strong> associated with hello output collection specified in your MapReduce job.</span></span>
   5. <span data-ttu-id="1fa13-316">Végezetül kattintson <strong>dokumentumkezelő</strong> alatt <strong>fejlesztői eszközök</strong>.</span><span class="sxs-lookup"><span data-stu-id="1fa13-316">Finally, click <strong>Document Explorer</strong> underneath <strong>Developer Tools</strong>.</span></span>

      <span data-ttu-id="1fa13-317">A MapReduce feladatot hello eredményeit jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="1fa13-317">You will see hello results of your MapReduce job.</span></span>

      ![MapReduce lekérdezés eredményei][image-mapreduce-query-results]

## <span data-ttu-id="1fa13-319"><a name="NextSteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1fa13-319"><a name="NextSteps"></a>Next Steps</span></span>
<span data-ttu-id="1fa13-320">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="1fa13-320">Congratulations!</span></span> <span data-ttu-id="1fa13-321">Csak az első Hive, Pig és MapReduce feladatok Azure Cosmos DB és HDInsight futtatta.</span><span class="sxs-lookup"><span data-stu-id="1fa13-321">You just ran your first Hive, Pig, and MapReduce jobs using Azure Cosmos DB and HDInsight.</span></span>

<span data-ttu-id="1fa13-322">Tudunk nyissa meg a Hadoop összekötő forrása.</span><span class="sxs-lookup"><span data-stu-id="1fa13-322">We have open sourced our Hadoop Connector.</span></span> <span data-ttu-id="1fa13-323">Ha szeretné használni, akkor a járulhat [GitHub][github].</span><span class="sxs-lookup"><span data-stu-id="1fa13-323">If you're interested, you can contribute on [GitHub][github].</span></span>

<span data-ttu-id="1fa13-324">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="1fa13-324">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="1fa13-325">[A Documentdb Java-alkalmazások fejlesztése][documentdb-java-application]</span><span class="sxs-lookup"><span data-stu-id="1fa13-325">[Develop a Java application with Documentdb][documentdb-java-application]</span></span>
* <span data-ttu-id="1fa13-326">[A hdinsight Hadoop Java MapReduce programok fejlesztése][hdinsight-develop-deploy-java-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="1fa13-326">[Develop Java MapReduce programs for Hadoop in HDInsight][hdinsight-develop-deploy-java-mapreduce]</span></span>
* <span data-ttu-id="1fa13-327">[Hadoop használatának megkezdésében a Hive HDInsight tooanalyze mobil kézibeszélő használatban][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="1fa13-327">[Get started using Hadoop with Hive in HDInsight tooanalyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="1fa13-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="1fa13-328">[Use MapReduce with HDInsight][hdinsight-use-mapreduce]</span></span>
* <span data-ttu-id="1fa13-329">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="1fa13-329">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="1fa13-330">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="1fa13-330">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="1fa13-331">[Parancsfájlművelet HDInsight-fürtök testreszabása][hdinsight-hadoop-customize-cluster]</span><span class="sxs-lookup"><span data-stu-id="1fa13-331">[Customize HDInsight clusters using Script Action][hdinsight-hadoop-customize-cluster]</span></span>

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
