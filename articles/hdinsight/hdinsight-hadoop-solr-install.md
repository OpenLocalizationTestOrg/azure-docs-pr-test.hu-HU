---
title: "Hadoop-fürt - Azure Solr telepítendő parancsfájl művelettel |} Microsoft Docs"
description: "Megtudhatja, hogyan Solr parancsfájlművelet használata a HDInsight-fürtök testreszabása."
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
ms.openlocfilehash: 6efb7ea26c3cdf7748fff4b02b5810c85cc41e1a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-solr-on-windows-based-hdinsight-clusters"></a><span data-ttu-id="a78c2-103">Telepítheti és használhatja a Windows-alapú HDInsight-fürtök Solr</span><span class="sxs-lookup"><span data-stu-id="a78c2-103">Install and use Solr on Windows-based HDInsight clusters</span></span>

<span data-ttu-id="a78c2-104">Útmutató: Windows-alapú HDInsight-fürtök testreszabása a parancsfájl műveletével Solr, és Solr segítségével történő keresés adatokat keressen.</span><span class="sxs-lookup"><span data-stu-id="a78c2-104">Learn how to customize Windows-based HDInsight cluster with Solr using Script Action, and how to use Solr to search data.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a78c2-105">A jelen dokumentumban leírt lépések csak a Windows-alapú HDInsight-fürtök dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="a78c2-105">The steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="a78c2-106">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="a78c2-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="a78c2-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="a78c2-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="a78c2-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="a78c2-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="a78c2-109">Egy Linux-alapú fürttel Solr használatáról információkért lásd: [telepítése és használata Solr a HDinsight Hadoop-fürtök (Linux)](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="a78c2-109">For information on using Solr with a Linux-based cluster, see [Install and use Solr on HDinsight Hadoop clusters (Linux)](hdinsight-hadoop-solr-install-linux.md).</span></span>


<span data-ttu-id="a78c2-110">Telepíthető Solr bármilyen típusú on Azure HDInsight (Hadoop-, Storm, HBase, Spark) fürt segítségével *parancsfájlművelet*.</span><span class="sxs-lookup"><span data-stu-id="a78c2-110">You can install Solr on any type of cluster (Hadoop, Storm, HBase, Spark) on Azure HDInsight by using *Script Action*.</span></span> <span data-ttu-id="a78c2-111">Egy minta parancsfájlt a HDInsight-fürtök Solr telepítendő érhető el, csak olvasható az Azure storage-blobból [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="a78c2-111">A sample script to install Solr on an HDInsight cluster is available from a read-only Azure storage blob at [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

<span data-ttu-id="a78c2-112">A parancsfájlpéldát csak HDInsight-fürt verziószáma 3.1-es verziójával működik.</span><span class="sxs-lookup"><span data-stu-id="a78c2-112">The sample script works only with HDInsight cluster version 3.1.</span></span> <span data-ttu-id="a78c2-113">A HDInsight-fürt verziókról további információkért lásd: [HDInsight-fürt verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="a78c2-113">For more information on HDInsight cluster versions, see [HDInsight cluster versions](hdinsight-component-versioning.md).</span></span>

<span data-ttu-id="a78c2-114">A jelen témakörben használt minta parancsfájlt hoz létre a Windows-alapú Solr fürt adott konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="a78c2-114">The sample script used in this topic creates a Windows-based Solr cluster with a specific configuration.</span></span> <span data-ttu-id="a78c2-115">Ha azt szeretné, a Solr fürt konfigurálásához a különböző gyűjteményekhez, szilánkok, sémákat, replikákat, stb., módosítania kell a parancsfájl és Solr bináris ennek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a78c2-115">If you want to configure the Solr cluster with different collections, shards, schemas, replicas, etc., you must modify the script and Solr binaries accordingly.</span></span>

<span data-ttu-id="a78c2-116">**Kapcsolódó cikkek**</span><span class="sxs-lookup"><span data-stu-id="a78c2-116">**Related articles**</span></span>

* [<span data-ttu-id="a78c2-117">Telepítheti és használhatja Solr HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="a78c2-117">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="a78c2-118">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a78c2-118">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="a78c2-119">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="a78c2-119">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="a78c2-120">[Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="a78c2-120">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>

## <a name="what-is-solr"></a><span data-ttu-id="a78c2-121">Mi az a Solr?</span><span class="sxs-lookup"><span data-stu-id="a78c2-121">What is Solr?</span></span>
<span data-ttu-id="a78c2-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="a78c2-122"><a href="http://lucene.apache.org/solr/features.html" target="_blank">Apache Solr</a> is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="a78c2-123">Hadoop lehetővé teszi a tárolásához és kezeléséhez az adatok óriási mennyiségben, Apache Solr biztosít a keresési képességek gyorsan az adatok lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="a78c2-123">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides the search capabilities to quickly retrieve the data.</span></span>

## <a name="install-solr-using-portal"></a><span data-ttu-id="a78c2-124">Telepítse a portál használatával Solr</span><span class="sxs-lookup"><span data-stu-id="a78c2-124">Install Solr using portal</span></span>
1. <span data-ttu-id="a78c2-125">Indítsa el a fürt létrehozása a **egyéni létrehozás** beállítást, a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-provision-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="a78c2-125">Start creating a cluster by using the **CUSTOM CREATE** option, as described at [Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md).</span></span>
2. <span data-ttu-id="a78c2-126">A a **Parancsfájlműveletek** lapon kattintson a varázsló **parancsfájlművelet hozzáadása** adni a parancsfájlművelet alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="a78c2-126">On the **Script Actions** page of the wizard, click **add script action** to provide details about the script action, as shown below:</span></span>

    <span data-ttu-id="a78c2-127">![Parancsfájlművelet használni ahhoz, hogy a fürt](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "parancsfájlművelet használja a fürt testreszabása")</span><span class="sxs-lookup"><span data-stu-id="a78c2-127">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-solr-install/hdi-script-action-solr.png "Use Script Action to customize a cluster")</span></span>

    <table border='1'>
        <tr><th><span data-ttu-id="a78c2-128">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="a78c2-128">Property</span></span></th><th><span data-ttu-id="a78c2-129">Érték</span><span class="sxs-lookup"><span data-stu-id="a78c2-129">Value</span></span></th></tr>
        <tr><td><span data-ttu-id="a78c2-130">Név</span><span class="sxs-lookup"><span data-stu-id="a78c2-130">Name</span></span></td>
            <td><span data-ttu-id="a78c2-131">Adja meg a parancsfájlművelet nevét.</span><span class="sxs-lookup"><span data-stu-id="a78c2-131">Specify a name for the script action.</span></span> <span data-ttu-id="a78c2-132">Például <b>telepítése Solr</b>.</span><span class="sxs-lookup"><span data-stu-id="a78c2-132">For example, <b>Install Solr</b>.</span></span></td></tr>
        <tr><td><span data-ttu-id="a78c2-133">A parancsfájl URI azonosítója</span><span class="sxs-lookup"><span data-stu-id="a78c2-133">Script URI</span></span></td>
            <td><span data-ttu-id="a78c2-134">Adja meg az egységes erőforrás-azonosító (URI) a parancsfájlt, amelyet a fürt testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="a78c2-134">Specify the Uniform Resource Identifier (URI) to the script that is invoked to customize the cluster.</span></span> <span data-ttu-id="a78c2-135">Például <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span><span class="sxs-lookup"><span data-stu-id="a78c2-135">For example, <i>https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1</i></span></span></td></tr>
        <tr><td><span data-ttu-id="a78c2-136">Csomóponttípus</span><span class="sxs-lookup"><span data-stu-id="a78c2-136">Node Type</span></span></td>
            <td><span data-ttu-id="a78c2-137">Adja meg a csomópontok, amelyen fut a testreszabási parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="a78c2-137">Specify the nodes on which the customization script is run.</span></span> <span data-ttu-id="a78c2-138">Választhat <b>csomópontjaihoz</b>, <b>csak Átjárócsomópontokat</b>, vagy <b>csak a feldolgozó csomópontok</b>.</span><span class="sxs-lookup"><span data-stu-id="a78c2-138">You can choose <b>All nodes</b>, <b>Head nodes only</b>, or <b>Worker nodes only</b>.</span></span>
        <tr><td><span data-ttu-id="a78c2-139">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="a78c2-139">Parameters</span></span></td>
            <td><span data-ttu-id="a78c2-140">Adja meg a paraméterek, ha a parancsfájl által igényelt.</span><span class="sxs-lookup"><span data-stu-id="a78c2-140">Specify the parameters, if required by the script.</span></span> <span data-ttu-id="a78c2-141">Solr telepítéséhez a parancsfájlt nem szükséges paramétereket, ezért üresen hagyhatja, ez.</span><span class="sxs-lookup"><span data-stu-id="a78c2-141">The script to install Solr does not require any parameters, so you can leave this blank.</span></span></td></tr>
    </table>

    <span data-ttu-id="a78c2-142">Több összetevőinek telepítése a fürt több parancsfájlművelet adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="a78c2-142">You can add more than one script action to install multiple components on the cluster.</span></span> <span data-ttu-id="a78c2-143">Miután hozzáadta a parancsfájlok, kattintson a pipára a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a78c2-143">After you have added the scripts, click the checkmark to start creating the cluster.</span></span>

## <a name="use-solr"></a><span data-ttu-id="a78c2-144">A Solr használata</span><span class="sxs-lookup"><span data-stu-id="a78c2-144">Use Solr</span></span>
<span data-ttu-id="a78c2-145">Az indexelő Solr bizonyos adatok fájlokkal kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="a78c2-145">You must start with indexing Solr with some data files.</span></span> <span data-ttu-id="a78c2-146">Solr az indexelt adatokat keresési lekérdezések futtatásához használhatja.</span><span class="sxs-lookup"><span data-stu-id="a78c2-146">You can then use Solr to run search queries on the indexed data.</span></span> <span data-ttu-id="a78c2-147">A következő lépésekkel Solr használata a HDInsight-fürtöt:</span><span class="sxs-lookup"><span data-stu-id="a78c2-147">Perform the following steps to use Solr in an HDInsight cluster:</span></span>

1. <span data-ttu-id="a78c2-148">**Segítségével Remote Desktop Protocol (RDP) távoli a HDInsight-fürthöz a telepített Solr**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-148">**Use Remote Desktop Protocol (RDP) to remote into the HDInsight cluster with Solr installed**.</span></span> <span data-ttu-id="a78c2-149">Azure-portálról a távoli asztal engedélyezése a fürthöz létrehozott Solr telepítve, és ezután távolról, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="a78c2-149">From the Azure portal, enable Remote Desktop for the cluster you created with Solr installed, and then remote into the cluster.</span></span> <span data-ttu-id="a78c2-150">Útmutatásért lásd: [csatlakozás RDP Funkciót használnak a HDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span><span class="sxs-lookup"><span data-stu-id="a78c2-150">For instructions, see [Connect to HDInsight clusters using RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).</span></span>
2. <span data-ttu-id="a78c2-151">**Fájlok feltöltésével index Solr**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-151">**Index Solr by uploading data files**.</span></span> <span data-ttu-id="a78c2-152">Index Solr, amikor a dokumentumok elhelyezése, amely a keresni szeretne.</span><span class="sxs-lookup"><span data-stu-id="a78c2-152">When you index Solr, you put documents in it that you may need to search on.</span></span> <span data-ttu-id="a78c2-153">Index Solr, segítségével RDP távoli a fürthöz, keresse meg az asztalon, nyissa meg a Hadoop parancssort, és keresse meg a **C:\apps\dist\solr-4.7.2\example\exampledocs**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-153">To index Solr, use RDP to remote into the cluster, navigate to the desktop, open the Hadoop command line, and navigate to **C:\apps\dist\solr-4.7.2\example\exampledocs**.</span></span> <span data-ttu-id="a78c2-154">Futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="a78c2-154">Run the following command:</span></span>

        java -jar post.jar solr.xml monitor.xml

    <span data-ttu-id="a78c2-155">A következő eredmény jelenik meg a konzolon:</span><span class="sxs-lookup"><span data-stu-id="a78c2-155">You'll see the following output on the console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="a78c2-156">A post.jar segédprogram Solr indexeli a két minta dokumentumok **solr.xml** és **monitor.xml**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-156">The post.jar utility indexes Solr with two sample documents, **solr.xml** and **monitor.xml**.</span></span> <span data-ttu-id="a78c2-157">A post.jar segédprogram és a minta dokumentumok Solr telepítés érhetők el.</span><span class="sxs-lookup"><span data-stu-id="a78c2-157">The post.jar utility and the sample documents are available with Solr installation.</span></span>
3. <span data-ttu-id="a78c2-158">**Az indexelt dokumentumok keresni a Solr Irányítópult segítségével**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-158">**Use the Solr dashboard to search within the indexed documents**.</span></span> <span data-ttu-id="a78c2-159">A HDInsight-fürthöz RDP-munkamenetet, nyissa meg az Internet Explorert, és indítsa el a következő Solr irányítópult **http://headnodehost:8983/solr / #/**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-159">In the RDP session to the HDInsight cluster, open Internet Explorer, and launch the Solr dashboard at **http://headnodehost:8983/solr/#/**.</span></span> <span data-ttu-id="a78c2-160">A bal oldali ablaktáblán az a **Core választó** legördülő listából válassza **collection1**, belül, amely, kattintson **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-160">From the left pane, from the **Core Selector** drop-down, select **collection1**, and within that, click **Query**.</span></span> <span data-ttu-id="a78c2-161">Tegyük fel válassza ki, és adja vissza a dokumentumok Solr, adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="a78c2-161">As an example, to select and return all the docs in Solr, provide the following values:</span></span>

   * <span data-ttu-id="a78c2-162">Az a **q** szöveget adja meg a  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="a78c2-162">In the **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="a78c2-163">Ezzel visszatér a indexelt dokumentumok a Solr.</span><span class="sxs-lookup"><span data-stu-id="a78c2-163">This will return all the documents that are indexed in Solr.</span></span> <span data-ttu-id="a78c2-164">Ha szeretne keresni egy adott karakterláncot belül a dokumentumokhoz, karakterláncokat Itt adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="a78c2-164">If you want to search for a specific string within the documents, you can enter that string here.</span></span>
   * <span data-ttu-id="a78c2-165">Az a **wt** szöveg mezőben adja meg a kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="a78c2-165">In the **wt** text box, select the output format.</span></span> <span data-ttu-id="a78c2-166">Alapértelmezett érték a **json**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-166">Default is **json**.</span></span> <span data-ttu-id="a78c2-167">Kattintson a **lekérdezés végrehajtása**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-167">Click **Execute Query**.</span></span>

     <span data-ttu-id="a78c2-168">![Parancsfájlművelet használni ahhoz, hogy a fürt](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Solr irányítópult-lekérdezés futtatható")</span><span class="sxs-lookup"><span data-stu-id="a78c2-168">![Use Script Action to customize a cluster](./media/hdinsight-hadoop-solr-install/hdi-solr-dashboard-query.png "Run a query on Solr dashboard")</span></span>

     <span data-ttu-id="a78c2-169">A kimenet az indexelés Solr használt két docs adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a78c2-169">The output returns the two docs that we used for indexing Solr.</span></span> <span data-ttu-id="a78c2-170">A kimenet az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="a78c2-170">The output resembles the following:</span></span>

           "response": {
               "numFound": 2,
               "start": 0,
               "maxScore": 1,
               "docs": [
                 {
                   "id": "SOLR1000",
                   "name": "Solr, the Enterprise Search Server",
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
                     "Scalability - Efficient Replication to other Solr Search Servers",
                     "Flexible and Adaptable with XML configuration and Schema",
                     "Good unicode support: héllo (hello with an accent over the e)"
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
4. <span data-ttu-id="a78c2-171">**Ajánlott: Az adatok biztonsági mentésének indexelt Solr az Azure Blob Storage a HDInsight-fürthöz társított**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-171">**Recommended: Back up the indexed data from Solr to Azure Blob storage associated with the HDInsight cluster**.</span></span> <span data-ttu-id="a78c2-172">Jó gyakorlat készítsen biztonsági másolatot az indexelt adatokat a Solr fürtcsomópontok alakzatot Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a78c2-172">As a good practice, you should back up the indexed data from the Solr cluster nodes onto Azure Blob storage.</span></span> <span data-ttu-id="a78c2-173">A következő lépésekkel ehhez:</span><span class="sxs-lookup"><span data-stu-id="a78c2-173">Perform the following steps to do so:</span></span>

   1. <span data-ttu-id="a78c2-174">Az RDP-munkamenetet nyissa meg az Internet Explorert, és mutasson a következő URL-címe:</span><span class="sxs-lookup"><span data-stu-id="a78c2-174">From the RDP session, open Internet Explorer, and point to the following URL:</span></span>

           http://localhost:8983/solr/replication?command=backup

       <span data-ttu-id="a78c2-175">A következőhöz hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a78c2-175">You should see a response like this:</span></span>

           <?xml version="1.0" encoding="UTF-8"?>
           <response>
             <lst name="responseHeader">
               <int name="status">0</int>
               <int name="QTime">9</int>
             </lst>
             <str name="status">OK</str>
           </response>
   2. <span data-ttu-id="a78c2-176">A távoli munkamenet, lépjen a {SOLR_HOME}\{gyűjtemény} \data.</span><span class="sxs-lookup"><span data-stu-id="a78c2-176">In the remote session, navigate to {SOLR_HOME}\{Collection}\data.</span></span> <span data-ttu-id="a78c2-177">A parancsfájlpéldát létrehozott fürt, amely legyen **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span><span class="sxs-lookup"><span data-stu-id="a78c2-177">For the cluster created via the sample script, this should be **C:\apps\dist\solr-4.7.2\example\solr\collection1\data**.</span></span> <span data-ttu-id="a78c2-178">Ezen a helyen kell megjelennie a pillanatkép mappája létrehozott hasonló nevű  **pillanatkép.* Timestamp típusú***.</span><span class="sxs-lookup"><span data-stu-id="a78c2-178">At this location, you should see a snapshot folder created with a name similar to **snapshot.*timestamp***.</span></span>
   3. <span data-ttu-id="a78c2-179">A ZIP-a pillanatkép mappája, majd töltse fel az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a78c2-179">Zip the snapshot folder and upload it to Azure Blob storage.</span></span> <span data-ttu-id="a78c2-180">A Hadoop parancssorból keresse meg a pillanatkép-mappa helye a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="a78c2-180">From the Hadoop command line, navigate to the location of the snapshot folder by using the following command:</span></span>

             hadoop fs -CopyFromLocal snapshot._timestamp_.zip /example/data

       <span data-ttu-id="a78c2-181">Ez a parancs másolja át a pillanatkép /example/data/belül az alapértelmezett tárfiók a fürthöz tartozó tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a78c2-181">This command copies the snapshot to /example/data/ under the container within the default Storage account associated with the cluster.</span></span>

## <a name="install-solr-using-aure-powershell"></a><span data-ttu-id="a78c2-182">Telepítse a következőkre PowerShell-lel Solr</span><span class="sxs-lookup"><span data-stu-id="a78c2-182">Install Solr using Aure PowerShell</span></span>
<span data-ttu-id="a78c2-183">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="a78c2-183">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span>  <span data-ttu-id="a78c2-184">A Spark az Azure PowerShell telepítése mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a78c2-184">The sample demonstrates how to install Spark using Azure PowerShell.</span></span> <span data-ttu-id="a78c2-185">Meg kell adnia, hogy a használandó parancsfájlt [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="a78c2-185">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="install-solr-using-net-sdk"></a><span data-ttu-id="a78c2-186">Telepítse a .NET SDK használatával Solr</span><span class="sxs-lookup"><span data-stu-id="a78c2-186">Install Solr using .NET SDK</span></span>
<span data-ttu-id="a78c2-187">Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span><span class="sxs-lookup"><span data-stu-id="a78c2-187">See [Customize HDInsight clusters using Script Action](hdinsight-hadoop-customize-cluster.md#call-scripts-using-azure-powershell).</span></span> <span data-ttu-id="a78c2-188">A minta bemutatja, hogyan telepítse a .NET SDK használatával Spark.</span><span class="sxs-lookup"><span data-stu-id="a78c2-188">The sample demonstrates how to install Spark using the .NET SDK.</span></span> <span data-ttu-id="a78c2-189">Meg kell adnia, hogy a használandó parancsfájlt [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span><span class="sxs-lookup"><span data-stu-id="a78c2-189">You need to customize the script to use [https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1).</span></span>

## <a name="see-also"></a><span data-ttu-id="a78c2-190">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a78c2-190">See also</span></span>
* [<span data-ttu-id="a78c2-191">Telepítheti és használhatja Solr HDinsight Hadoop-fürtök (Linux)</span><span class="sxs-lookup"><span data-stu-id="a78c2-191">Install and use Solr on HDinsight Hadoop clusters (Linux)</span></span>](hdinsight-hadoop-solr-install-linux.md)
* <span data-ttu-id="a78c2-192">[Hdinsight Hadoop-fürtök létrehozása](hdinsight-provision-clusters.md): általános információk a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a78c2-192">[Create Hadoop clusters in HDInsight](hdinsight-provision-clusters.md): general information on creating HDInsight clusters.</span></span>
* <span data-ttu-id="a78c2-193">[Testre szabhatja a HDInsight-fürtjéhez parancsfájlművelet][hdinsight-cluster-customize]: parancsfájlművelet HDInsight-fürtök testreszabása általános tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="a78c2-193">[Customize HDInsight cluster using Script Action][hdinsight-cluster-customize]: general information on customizing HDInsight clusters using Script Action.</span></span>
* <span data-ttu-id="a78c2-194">[Parancsfájlművelet-parancsfájlok fejlesztése a HDInsight](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="a78c2-194">[Develop Script Action scripts for HDInsight](hdinsight-hadoop-script-actions.md).</span></span>
* <span data-ttu-id="a78c2-195">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]: Spark telepítéséről parancsfájlművelet-mintát.</span><span class="sxs-lookup"><span data-stu-id="a78c2-195">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]: Script Action sample about installing Spark.</span></span>
* <span data-ttu-id="a78c2-196">[A HDInsight-fürtökön R telepítéséhez][hdinsight-install-r]: parancsfájlművelet minta R. telepítéséről</span><span class="sxs-lookup"><span data-stu-id="a78c2-196">[Install R on HDInsight clusters][hdinsight-install-r]: Script Action sample about installing R.</span></span>
* <span data-ttu-id="a78c2-197">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md): parancsfájlművelet minta Giraph telepítésével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a78c2-197">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md): Script Action sample about installing Giraph.</span></span>

[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
