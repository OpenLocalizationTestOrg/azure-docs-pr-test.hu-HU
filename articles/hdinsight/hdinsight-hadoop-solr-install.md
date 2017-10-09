---
title: "a Hadoop-fürt - Azure aaaUse parancsfájlművelet tooinstall Solr |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize HDInsight-fürtöt Solr parancsfájlművelet használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b1e6f338-8ac1-4b38-bbb5-2f7388b9de3b
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 022ba56b7499390a91bfe869e5069893e56b6503
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="510f7-103">Telepítheti és használhatja a Windows-alapú HDInsight-fürtök Solr</span><span class="sxs-lookup"><span data-stu-id="510f7-103">Install and use Solr on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="510f7-104">Megtudhatja, hogyan toocustomize Windows-alapú HDInsight-fürtöt használó parancsfájlművelet Solr, és hogyan toouse Solr toosearch adatokat.</span><span class="sxs-lookup"><span data-stu-id="510f7-104">Learn how toocustomize Windows-based HDInsight cluster with Solr using Script Action, and how toouse Solr toosearch data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="510f7-105">Ez a dokumentum csak olyan feladaton végezhető Windows-alapú HDInsight-fürtökkel hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="510f7-105">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="510f7-106">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="510f7-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="510f7-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="510f7-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="510f7-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="510f7-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="510f7-109">Egy Linux-alapú fürttel Solr használatáról információkért lásd: [telepítése és használata Solr a HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="510f7-109">For information on using Solr with a Linux-based cluster, see [Install and use Solr on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).</span></span>


<span data-ttu-id="510f7-110">Telepíthető Solr bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*.</span><span class="sxs-lookup"><span data-stu-id="510f7-110">You can install Solr on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="510f7-111">Egy minta parancsfájlt tooinstall Solr egy HDInsight-fürt érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="510f7-111">A sample script tooinstall Solr on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

<span data-ttu-id="510f7-112">hello mintaparancsfájl csak HDInsight-fürt verziószáma 3.1-es verziójával működik.</span><span class="sxs-lookup"><span data-stu-id="510f7-112">hello sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="510f7-113">A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="510f7-113">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="510f7-114">a jelen témakörben használt hello mintaparancsfájl egy Windows-alapú Solr fürtöt hoz létre a adott konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="510f7-114">hello sample script used in this topic creates a Windows-based Solr cluster with a specific configuration.</span></span> <span data-ttu-id="510f7-115">Ha azt szeretné, hogy tooconfigure hello Solr fürt különböző gyűjteményeket, szilánkok, sémákat, replikákat, stb., módosítania kell hello parancsfájl és Solr bináris ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="510f7-115">If you want tooconfigure hello Solr cluster with different collections, shards, schemas, replicas, etc., you must modify hello script and Solr binaries accordingly.</span></span>

<span data-ttu-id="510f7-116">**Kapcsolódó cikkek**</span><span class="sxs-lookup"><span data-stu-id="510f7-116">**Related articles**</span></span>

* [<span data-ttu-id="510f7-117">Telepítheti és használhatja Solr HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="510f7-117">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="510f7-118">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="510f7-118">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="510f7-119">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="510f7-119">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="510f7-120">[Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="510f7-120">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-solr"></a><span data-ttu-id="510f7-121">Mi az a Solr?</span><span class="sxs-lookup"><span data-stu-id="510f7-121">What is Solr?</span></span>
<span data-ttu-id="510f7-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="510f7-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="510f7-123">Hadoop lehetővé teszi a tárolásához és kezeléséhez az adatok óriási mennyiségben, Apache Solr hello keresési képességeket biztosít tooquickly lekérése hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="510f7-123">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides hello search capabilities tooquickly retrieve hello data.</span></span>

## <a name="install-solr-using-portal"></a><span data-ttu-id="510f7-124">Telepítse a portál használatával Solr</span><span class="sxs-lookup"><span data-stu-id="510f7-124">Install Solr using portal</span></span>
1. <span data-ttu-id="510f7-125">Indítsa el a fürt létrehozása hello segítségével **egyéni létrehozás** beállítást, a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="510f7-125">Start creating a cluster by using hello **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="510f7-126">A hello **Parancsfájlműveletek** lap hello varázsló, kattintson a **parancsfájlművelet hozzáadása** tooprovide részleteinek hello parancsfájlművelet, alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="510f7-126">On hello **Script Actions** page of hello wizard, click **add script action** tooprovide details about hello script action, as shown below:</span></span>

    <span data-ttu-id="510f7-127">![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "használata parancsfájlművelet toocustomize fürt")</span><span class="sxs-lookup"><span data-stu-id="510f7-127">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Use Script Action toocustomize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="510f7-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="510f7-128">Property</span></span></th><th><span data-ttu-id="510f7-129">Érték</span><span class="sxs-lookup"><span data-stu-id="510f7-129">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="510f7-130">Név</span><span class="sxs-lookup"><span data-stu-id="510f7-130">Name</span></span></td>
            <td><span data-ttu-id="510f7-131">Adja meg a hello parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="510f7-131">Specify a name for hello script action.</span></span> <span data-ttu-id="510f7-132">Például <b>telepítése Solr</b>.</span><span class="sxs-lookup"><span data-stu-id="510f7-132">For example, <b>Install Solr</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="510f7-133">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="510f7-133">Script URI</span></span></td>
            <td><span data-ttu-id="510f7-134">Hello egységes erőforrás-azonosító (URI) toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt.</span><span class="sxs-lookup"><span data-stu-id="510f7-134">Specify hello Uniform Resource Identifier (URI) toohello script that is invoked toocustomize hello cluster.</span></span> <span data-ttu-id="510f7-135">Például <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="510f7-135">For example, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="510f7-136">Csomóponttípus</span><span class="sxs-lookup"><span data-stu-id="510f7-136">Node Type</span></span></td>
            <td><span data-ttu-id="510f7-137">Adja meg, amelyen hello testreszabási parancsfájl futtatása hello csomópontok.</span><span class="sxs-lookup"><span data-stu-id="510f7-137">Specify hello nodes on which hello customization script is run.</span></span> <span data-ttu-id="510f7-138">Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>csak a feldolgozó csomópontok</b>.</span><span class="sxs-lookup"><span data-stu-id="510f7-138">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="510f7-139">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="510f7-139">Parameters</span></span></td>
            <td><span data-ttu-id="510f7-140">Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="510f7-140">Specify hello parameters, if required by hello script.</span></span> <span data-ttu-id="510f7-141">hello parancsfájl tooinstall Solr kell paramétert, így ez üresen hagyhatja.</span><span class="sxs-lookup"><span data-stu-id="510f7-141">hello script tooinstall Solr does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="510f7-142">Hello fürtön egynél több parancsprogram művelet tooinstall több összetevőből is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="510f7-142">You can add more than one script action tooinstall multiple components on hello cluster.</span></span> <span data-ttu-id="510f7-143">Miután hozzáadta a hello parancsfájlok, kattintson a hello pipa toostart hello fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="510f7-143">After you have added hello scripts, click hello checkmark toostart creating hello cluster.</span></span>

## <a name="use-solr"></a><span data-ttu-id="510f7-144">A Solr használata</span><span class="sxs-lookup"><span data-stu-id="510f7-144">Use Solr</span></span>
<span data-ttu-id="510f7-145">Az indexelő Solr bizonyos adatok fájlokkal kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="510f7-145">You must start with indexing Solr with some data files.</span></span> <span data-ttu-id="510f7-146">Solr toorun keresési lekérdezések használhatja, ha az indexelt hello adatokon.</span><span class="sxs-lookup"><span data-stu-id="510f7-146">You can then use Solr toorun search queries on hello indexed data.</span></span> <span data-ttu-id="510f7-147">Hajtsa végre a következő lépéseket toouse Solr a HDInsight-fürtök hello:</span><span class="sxs-lookup"><span data-stu-id="510f7-147">Perform hello following steps toouse Solr in an HDInsight cluster:</span></span>

1. <span data-ttu-id="510f7-148">**Remote Desktop Protocol (RDP) tooremote hello HDInsight-fürthöz történő használata telepített Solr**.</span><span class="sxs-lookup"><span data-stu-id="510f7-148">**Use Remote Desktop Protocol (RDP) tooremote into hello HDInsight cluster with Solr installed**.</span></span> <span data-ttu-id="510f7-149">Hello Azure-portálon, a távoli asztal engedélyezése Solr telepített, és be, majd távolról hello fürthöz létrehozott hello fürt számára.</span><span class="sxs-lookup"><span data-stu-id="510f7-149">From hello Azure portal, enable Remote Desktop for hello cluster you created with Solr installed, and then remote into hello cluster.</span></span> <span data-ttu-id="510f7-150">Útmutatásért lásd: [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="510f7-150">For instructions, see [Connect tooHDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="510f7-151">**Fájlok feltöltésével index Solr**.</span><span class="sxs-lookup"><span data-stu-id="510f7-151">**Index Solr by uploading data files**.</span></span> <span data-ttu-id="510f7-152">Index Solr, amikor a rendszer, amely toosearch szükség lehet a put dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="510f7-152">When you index Solr, you put documents in it that you may need toosearch on.</span></span> <span data-ttu-id="510f7-153">tooindex Solr, használja az RDP tooremote hello fürtbe, keresse meg a toohello asztali, hello Hadoop parancssor megnyitásához, és keresse meg a túl**C:\apps\dist\solr-4.7.2\example\exampledocs**.</span><span class="sxs-lookup"><span data-stu-id="510f7-153">tooindex Solr, use RDP tooremote into hello cluster, navigate toohello desktop, open hello Hadoop command line, and navigate too**C:\apps\dist\solr-4.7.2\example\exampledocs**.</span></span> <span data-ttu-id="510f7-154">Futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="510f7-154">Run hello following command:</span></span>

        java -jar post.jar solr.xml monitor.xml

    <span data-ttu-id="510f7-155">Hello kimeneti hello konzolban, a következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="510f7-155">You'll see hello following output on hello console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="510f7-156">hello post.jar segédprogram Solr indexeli a két minta dokumentumok **solr.xml** és **monitor.xml**.</span><span class="sxs-lookup"><span data-stu-id="510f7-156">hello post.jar utility indexes Solr with two sample documents, **solr.xml** and **monitor.xml**.</span></span> <span data-ttu-id="510f7-157">hello post.jar segédprogram és hello minta dokumentumok Solr telepítés érhetők el.</span><span class="sxs-lookup"><span data-stu-id="510f7-157">hello post.jar utility and hello sample documents are available with Solr installation.</span></span>
3. <span data-ttu-id="510f7-158">**Használjon hello Solr irányítópult toosearch belül hello indexelt dokumentumok**.</span><span class="sxs-lookup"><span data-stu-id="510f7-158">**Use hello Solr dashboard toosearch within hello indexed documents**.</span></span> <span data-ttu-id="510f7-159">A hello RDP munkamenet toohello HDInsight fürt, nyissa meg az Internet Explorert, indítsa el a következő hello Solr irányítópult **http://headnodehost:8983/solr / #/**.</span><span class="sxs-lookup"><span data-stu-id="510f7-159">In hello RDP session toohello HDInsight cluster, open Internet Explorer, and launch hello Solr dashboard at **http://headnodehost:8983/solr/#/**.</span></span> <span data-ttu-id="510f7-160">Hello bal oldali ablaktáblán, a hello **Core választó** legördülő listából válassza **collection1**, majd belül, hogy a **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="510f7-160">From hello left pane, from hello **Core Selector** drop-down, select **collection1**, and within that, click **Query**.</span></span> <span data-ttu-id="510f7-161">Egy példa, tooselect és visszatérési összes hello dokumentumok a Solr, adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="510f7-161">As an example, tooselect and return all hello docs in Solr, provide hello following values:</span></span>

   * <span data-ttu-id="510f7-162">A hello **q** szöveget adja meg a  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="510f7-162">In hello **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="510f7-163">Ez visszaállítja az összes hello dokumentumok indexelt a Solr.</span><span class="sxs-lookup"><span data-stu-id="510f7-163">This will return all hello documents that are indexed in Solr.</span></span> <span data-ttu-id="510f7-164">Ha azt szeretné, toosearch hello dokumentumok belül egy adott karakterláncot, karakterláncokat Itt adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="510f7-164">If you want toosearch for a specific string within hello documents, you can enter that string here.</span></span>
   * <span data-ttu-id="510f7-165">A hello **wt** szövegmezőben, jelölje be hello kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="510f7-165">In hello **wt** text box, select hello output format.</span></span> <span data-ttu-id="510f7-166">Alapértelmezett érték a **json**.</span><span class="sxs-lookup"><span data-stu-id="510f7-166">Default is **json**.</span></span> <span data-ttu-id="510f7-167">Kattintson a **lekérdezés végrehajtása**.</span><span class="sxs-lookup"><span data-stu-id="510f7-167">Click **Execute Query**.</span></span>

     <span data-ttu-id="510f7-168">![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Solr irányítópult-lekérdezés futtatható")</span><span class="sxs-lookup"><span data-stu-id="510f7-168">![Use Script Action toocustomize a cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Run a query on Solr dashboard")</span></span>

     <span data-ttu-id="510f7-169">hello kimeneti hello az indexelési Solr használt két docs adja vissza.</span><span class="sxs-lookup"><span data-stu-id="510f7-169">hello output returns hello two docs that we used for indexing Solr.</span></span> <span data-ttu-id="510f7-170">hello kimeneti hello következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="510f7-170">hello output resembles hello following:</span></span>

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, hello Enterprise Search Server",
                   "manu": "Apache Software Foundation",
                   "cat": [
                     "software",
                     "search"
                   ],
                   "features": [
                     "Advanced Full-Text Search Capabilities using Lucene",
                     "Optimized for High Volume Web Traffic",
                     "Standards Based Open Interfaces - XML and HTTP",
                     "Comprehensive HTML Administration Interfaces",
                     "Scalability - Efficient Replication tooother Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over hello e)"
                   ],
                   "price": 0,
                   "price_c": "0,USD",
                   "popularity": 10,
                   "inStock": true,
                   "incubationdate_dt": "2006-01-17T00:00:00Z",
                   "_version_": 1486960636996878300
                 },
                 {
                   "id": "3007WFP",
                   "name": "Dell Widescreen UltraSharp 3007WFP",
                   "manu": "Dell, Inc.",
                   "manu_id_s": "dell",
                   "cat": [
                     "electronics and computer1"
                   ],
                   "features": [
                     "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                   ],
                   "includes": "USB cable",
                   "weight": 401.6,
                   "price": 2199,
                   "price_c": "2199,USD",
                   "popularity": 6,
                   "inStock": true,
                   "store": "43.17614,-90.57341",
                   "_version_": 1486960637584081000
                 }
               ]
             }
4. <span data-ttu-id="510f7-171">**Ajánlott: Biztonságimásolat-hello indexelt Solr tooAzure hello HDInsight-fürthöz társított Blob-tároló adatait**.</span><span class="sxs-lookup"><span data-stu-id="510f7-171">**Recommended: Back up hello indexed data from Solr tooAzure Blob storage associated with hello HDInsight cluster**.</span></span> <span data-ttu-id="510f7-172">Jó gyakorlat készítsen biztonsági másolatot indexelt hello adatok hello Solr fürt csomópontjának alakzatot Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="510f7-172">As a good practice, you should back up hello indexed data from hello Solr cluster nodes onto Azure Blob storage.</span></span> <span data-ttu-id="510f7-173">Hajtsa végre a következő lépéseket toodo így hello:</span><span class="sxs-lookup"><span data-stu-id="510f7-173">Perform hello following steps toodo so:</span></span>

   1. <span data-ttu-id="510f7-174">Hello RDP-munkamenetet nyissa meg az Internet Explorer és a következő URL-cím pont toohello:</span><span class="sxs-lookup"><span data-stu-id="510f7-174">From hello RDP session, open Internet Explorer, and point toohello following URL:</span></span>

           http://localhost:8983/solr/replication?command=backup

       <span data-ttu-id="510f7-175">A következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="510f7-175">You should see a response like this:</span></span>

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. <span data-ttu-id="510f7-176">Hello távoli munkamenet, lépjen a túl {SOLR_HOME}\{gyűjtemény} \data.</span><span class="sxs-lookup"><span data-stu-id="510f7-176">In hello remote session, navigate too{SOLR_HOME}\{Collection}\data.</span></span> <span data-ttu-id="510f7-177">Hello fürthöz létrehozott hello minta parancsfájlt, amely legyen **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span><span class="sxs-lookup"><span data-stu-id="510f7-177">For hello cluster created via hello sample script, this should be **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span></span> <span data-ttu-id="510f7-178">Ezen a helyen kell megjelennie a pillanatkép mappája létrejön, melynek a neve hasonló túl**pillanatkép.* Timestamp típusú***.</span><span class="sxs-lookup"><span data-stu-id="510f7-178">At this location, you should see a snapshot folder created with a name similar too**snapshot.*timestamp***.</span></span>
   3. <span data-ttu-id="510f7-179">A ZIP-hello pillanatképmappáját, és töltse fel tooAzure Blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="510f7-179">Zip hello snapshot folder and upload it tooAzure Blob storage.</span></span> <span data-ttu-id="510f7-180">Hello Hadoop parancssorban keresse meg a hello pillanatkép mappája helyének toohello hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="510f7-180">From hello Hadoop command line, navigate toohello location of hello snapshot folder by using hello following command:</span></span>

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       <span data-ttu-id="510f7-181">Ez a parancs másolatok túl/példa/pillanatképadatok hello/hello tárolóban belül hello alapértelmezett tárolási hello-fürthöz tartozó fiókot.</span><span class="sxs-lookup"><span data-stu-id="510f7-181">This command copies hello snapshot too/example/data/ under hello container within hello default Storage account associated with hello cluster.</span></span>

## <a name="install-solr-using-aure-powershell"></a><span data-ttu-id="510f7-182">Telepítse a következőkre PowerShell-lel Solr</span><span class="sxs-lookup"><span data-stu-id="510f7-182">Install Solr using Aure PowerShell</span></span>
<span data-ttu-id="510f7-183">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="510f7-183">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="510f7-184">hello minta bemutatja, hogyan tooinstall Spark on Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="510f7-184">hello sample demonstrates how tooinstall Spark using Azure PowerShell.</span></span> <span data-ttu-id="510f7-185">Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="510f7-185">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="install-solr-using-net-sdk"></a><span data-ttu-id="510f7-186">Telepítse a .NET SDK használatával Solr</span><span class="sxs-lookup"><span data-stu-id="510f7-186">Install Solr using .NET SDK</span></span>
<span data-ttu-id="510f7-187">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="510f7-187">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="510f7-188">hello minta bemutatja, hogyan tooinstall Spark .NET SDK használatával hello.</span><span class="sxs-lookup"><span data-stu-id="510f7-188">hello sample demonstrates how tooinstall Spark using hello .NET SDK.</span></span> <span data-ttu-id="510f7-189">Toocustomize hello parancsfájl toouse kell [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="510f7-189">You need toocustomize hello script toouse [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="510f7-190">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="510f7-190">See also</span></span>
* [<span data-ttu-id="510f7-191">Telepítheti és használhatja Solr HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="510f7-191">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="510f7-192">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="510f7-192">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="510f7-193">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="510f7-193">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="510f7-194">[Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="510f7-194">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="510f7-195">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-mintát.</span><span class="sxs-lookup"><span data-stu-id="510f7-195">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="510f7-196">[A HDInsight-fürtökön R telepítéséhez][hdinsight-install-r]: parancsfájlművelet minta R. telepítéséről</span><span class="sxs-lookup"><span data-stu-id="510f7-196">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="510f7-197">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md): parancsfájlművelet minta Giraph telepítésével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="510f7-197">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
