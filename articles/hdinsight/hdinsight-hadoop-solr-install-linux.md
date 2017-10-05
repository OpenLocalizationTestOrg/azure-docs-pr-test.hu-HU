---
title: "Solr telepítése Linux-alapú hdinsight - Azure a parancsfájlművelet használatával |} Microsoft Docs"
description: "Ismerje meg, Solr Parancsfájlműveletek Linux-alapú HDInsight Hadoop-fürtök telepítése."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cc93ed5c-a358-456a-91a4-f179185c0e98
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: ad930ca023a36fa5874483873c82fdba11d117c7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="11c0a-103">Telepítheti és használhatja Solr HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="11c0a-103">Install and use Solr on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="11c0a-104">Útmutató: Azure HDInsight Solr telepítése parancsfájlművelet segítségével.</span><span class="sxs-lookup"><span data-stu-id="11c0a-104">Learn how to install Solr on Azure HDInsight by using Script Action.</span></span> <span data-ttu-id="11c0a-105">Solr egy hatékony keresési platform, és a Hadoop által kezelt adatok vállalati szintű keresési funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="11c0a-105">Solr is a powerful search platform and provides enterprise-level search capabilities on data managed by Hadoop.</span></span>

> [!IMPORTANT]
    > <span data-ttu-id="11c0a-106">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="11c0a-106">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="11c0a-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="11c0a-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="11c0a-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="11c0a-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="11c0a-109">A dokumentumban használt parancsfájlpélda Solr 4.9 egy adott konfigurációval telepíti.</span><span class="sxs-lookup"><span data-stu-id="11c0a-109">The sample script used in this document installs Solr 4.9 with a specific configuration.</span></span> <span data-ttu-id="11c0a-110">Ha azt szeretné, a Solr fürt konfigurálásához a különböző gyűjteményekhez, szilánkok, sémákat, replikákat, stb., módosítania kell a parancsfájl és Solr bináris fájljait.</span><span class="sxs-lookup"><span data-stu-id="11c0a-110">If you want to configure the Solr cluster with different collections, shards, schemas, replicas, etc., you must modify the script and Solr binaries.</span></span>

## <span data-ttu-id="11c0a-111"><a name="whatis"></a>Mi az a Solr</span><span class="sxs-lookup"><span data-stu-id="11c0a-111"><a name="whatis"></a>What is Solr</span></span>

<span data-ttu-id="11c0a-112">[Apache Solr](http://lucene.apache.org/solr/features.html) egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="11c0a-112">[Apache Solr](http://lucene.apache.org/solr/features.html) is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="11c0a-113">Hadoop lehetővé teszi a tárolásához és kezeléséhez az adatok óriási mennyiségben, Apache Solr biztosít a keresési képességek gyorsan az adatok lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="11c0a-113">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides the search capabilities to quickly retrieve the data.</span></span>

> [!WARNING]
> <span data-ttu-id="11c0a-114">A HDInsight-fürt összetevői Microsoft teljes mértékben támogatja.</span><span class="sxs-lookup"><span data-stu-id="11c0a-114">Components provided with the HDInsight cluster are fully supported by Microsoft.</span></span>
>
> <span data-ttu-id="11c0a-115">Egyéni összetevők, például Solr, minden üzleti szempontból ésszerű terméktámogatási segítséget nyújtanak a probléma további hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="11c0a-115">Custom components, such as Solr, receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="11c0a-116">Előfordulhat, hogy a Microsoft támogatási nem egyéni összetevőkkel kapcsolatos problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="11c0a-116">Microsoft support may not be able to resolve problems with custom components.</span></span> <span data-ttu-id="11c0a-117">Szükség lehet a nyílt forráskódú Közösségek szólítsa meg, ha segítségre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="11c0a-117">You may need to engage the open source communities for assistance.</span></span> <span data-ttu-id="11c0a-118">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="11c0a-118">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="11c0a-119">Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="11c0a-119">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

## <a name="what-the-script-does"></a><span data-ttu-id="11c0a-120">A parancsfájl funkciója</span><span class="sxs-lookup"><span data-stu-id="11c0a-120">What the script does</span></span>

<span data-ttu-id="11c0a-121">Ezt a parancsfájlt a HDInsight-fürt hajt végre a következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="11c0a-121">This script makes the following changes to the HDInsight cluster:</span></span>

* <span data-ttu-id="11c0a-122">4.9 Solr történő telepítése`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="11c0a-122">Installs Solr 4.9 into `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="11c0a-123">Létrehoz egy felhasználói **solrusr**, amely a Solr szolgáltatás futtatására szolgál</span><span class="sxs-lookup"><span data-stu-id="11c0a-123">Creates a user, **solrusr**, which is used to run the Solr service</span></span>
* <span data-ttu-id="11c0a-124">Készletek **solruser** tulajdonosa`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="11c0a-124">Sets **solruser** as the owner of `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="11c0a-125">Hozzáad egy [Upstart](http://upstart.ubuntu.com/) konfigurációjához, amely Solr automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="11c0a-125">Adds an [Upstart](http://upstart.ubuntu.com/) configuration that starts Solr automatically.</span></span>

## <span data-ttu-id="11c0a-126"><a name="install"></a>A Parancsfájlműveletek segítségével Solr telepítése</span><span class="sxs-lookup"><span data-stu-id="11c0a-126"><a name="install"></a>Install Solr using Script Actions</span></span>

<span data-ttu-id="11c0a-127">Egy HDInsight-fürtök Solr telepítendő parancsfájlt a következő helyen érhető el:</span><span class="sxs-lookup"><span data-stu-id="11c0a-127">A sample script to install Solr on an HDInsight cluster is available at the following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

<span data-ttu-id="11c0a-128">Hozzon létre egy fürtöt, amely rendelkezik a telepített Solr, kövesse a lépéseket a a [HDInsight-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="11c0a-128">To create a cluster that has Solr installed, use the steps in the [Create HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md) document.</span></span> <span data-ttu-id="11c0a-129">A létrehozási folyamat során a következő lépések segítségével Solr telepítése:</span><span class="sxs-lookup"><span data-stu-id="11c0a-129">During the creation process, use the following steps to install Solr:</span></span>

1. <span data-ttu-id="11c0a-130">Az a __fürt összefoglaló__ panelen, select__Advanced settings__, majd __parancsfájl-műveletek__.</span><span class="sxs-lookup"><span data-stu-id="11c0a-130">From the __Cluster summary__ blade, select__Advanced settings__, then __Script actions__.</span></span> <span data-ttu-id="11c0a-131">Az űrlap feltöltéséhez használja az alábbi információkat:</span><span class="sxs-lookup"><span data-stu-id="11c0a-131">Use the following information to populate the form:</span></span>

   * <span data-ttu-id="11c0a-132">**NÉV**: Adja meg a parancsfájlművelet rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="11c0a-132">**NAME**: Enter a friendly name for the script action.</span></span>
   * <span data-ttu-id="11c0a-133">**PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="11c0a-133">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span></span>
   * <span data-ttu-id="11c0a-134">**HEAD**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="11c0a-134">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="11c0a-135">**MUNKAVÉGZŐ**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="11c0a-135">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="11c0a-136">**ZOOKEEPER**: ezt a beállítást a Zookeeper csomóponton telepítése</span><span class="sxs-lookup"><span data-stu-id="11c0a-136">**ZOOKEEPER**: Check this option to install on the Zookeeper node</span></span>
   * <span data-ttu-id="11c0a-137">**PARAMÉTEREK**: ezt a mezőt hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="11c0a-137">**PARAMETERS**: Leave this field blank</span></span>

2. <span data-ttu-id="11c0a-138">Alján a **parancsfájl-műveletek** panelen, használja a **válasszon** gombra kattintva mentse a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="11c0a-138">At the bottom of the **Script actions** blade, use the **Select** button to save the configuration.</span></span> <span data-ttu-id="11c0a-139">Végül a **következő** gombra kattintva visszatérhet a __fürt összegzése__</span><span class="sxs-lookup"><span data-stu-id="11c0a-139">Finally, use the **Next** button to return to the __Cluster summary__</span></span>

3. <span data-ttu-id="11c0a-140">Az a __fürt összefoglaló__ lapon jelölje be __létrehozása__ a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="11c0a-140">From the __Cluster summary__ page, select __Create__ to create the cluster.</span></span>

## <span data-ttu-id="11c0a-141"><a name="usesolr"></a>Solr használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="11c0a-141"><a name="usesolr"></a>How do I use Solr in HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="11c0a-142">Ebben a szakaszban a lépések bemutatják, Solr alapvető funkciókat.</span><span class="sxs-lookup"><span data-stu-id="11c0a-142">The steps in this section demonstrate basic Solr functionality.</span></span> <span data-ttu-id="11c0a-143">Solr használatával kapcsolatos további információkért lásd: a [Apache Solr hely](http://lucene.apache.org/solr/).</span><span class="sxs-lookup"><span data-stu-id="11c0a-143">For more information on using Solr, see the [Apache Solr site](http://lucene.apache.org/solr/).</span></span>

### <a name="index-data"></a><span data-ttu-id="11c0a-144">Index adatok</span><span class="sxs-lookup"><span data-stu-id="11c0a-144">Index data</span></span>

<span data-ttu-id="11c0a-145">Az alábbi lépések segítségével Példa adatok hozzáadása a Solr, és majd lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="11c0a-145">Use the following steps to add example data to Solr, and then query it:</span></span>

1. <span data-ttu-id="11c0a-146">Csatlakozzon SSH-val a HDInsight-fürthöz:</span><span class="sxs-lookup"><span data-stu-id="11c0a-146">Connect to the HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="11c0a-147">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="11c0a-147">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="11c0a-148">A jelen dokumentum lépéseit az SSL-protokollbújtatás való csatlakozáskor használandó a Solr webes felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="11c0a-148">Steps later in this document use an SSL tunnel to connect to the Solr web UI.</span></span> <span data-ttu-id="11c0a-149">Ezen lépések használatához, az SSL-protokollbújtatás létrehozására és a böngésző használják majd beállítása.</span><span class="sxs-lookup"><span data-stu-id="11c0a-149">To use these steps, you must establish an SSL tunnel and then configure your browser to use it.</span></span>
     >
     > <span data-ttu-id="11c0a-150">További információkért lásd: a [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="11c0a-150">For more information, see the [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="11c0a-151">Az alábbi parancsokkal Solr index mintaadatok rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="11c0a-151">Use the following commands to have Solr index sample data:</span></span>

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    <span data-ttu-id="11c0a-152">A következő kimenetet visszaadja a konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="11c0a-152">The following output is returned to the console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes to http://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="11c0a-153">A `post.jar` segédprogram hozzáadja a **solr.xml** és **monitor.xml** az index a dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="11c0a-153">The `post.jar` utility adds the **solr.xml** and **monitor.xml** documents to the index.</span></span>
  
3. <span data-ttu-id="11c0a-154">Használja a következő parancsot lekérdezéséhez Solr REST API:</span><span class="sxs-lookup"><span data-stu-id="11c0a-154">Use the following command to query the Solr REST API:</span></span>

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    <span data-ttu-id="11c0a-155">Ez a parancs keres **collection1** megfelelő dokumentumokhoz  **\*:\***  (kódolva \*% 3A\* a lekérdezési karakterláncban).</span><span class="sxs-lookup"><span data-stu-id="11c0a-155">This command searches **collection1** for any documents matching **\*:\*** (encoded as \*%3A\* in the query string).</span></span> <span data-ttu-id="11c0a-156">A következő JSON-dokumentum a válasz példája:</span><span class="sxs-lookup"><span data-stu-id="11c0a-156">The following JSON document is an example of the response:</span></span>

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

### <a name="using-the-solr-dashboard"></a><span data-ttu-id="11c0a-157">A Solr irányítópult használata</span><span class="sxs-lookup"><span data-stu-id="11c0a-157">Using the Solr dashboard</span></span>

<span data-ttu-id="11c0a-158">A Solr irányítópult a webes felhasználói felület, amely lehetővé teszi, hogy a webböngészőn keresztül Solr alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="11c0a-158">The Solr dashboard is a web UI that allows you to work with Solr through your web browser.</span></span> <span data-ttu-id="11c0a-159">A Solr irányítópult nincs felfedve, közvetlenül a HDInsight-fürtök az interneten.</span><span class="sxs-lookup"><span data-stu-id="11c0a-159">The Solr dashboard is not exposed directly on the Internet from your HDInsight cluster.</span></span> <span data-ttu-id="11c0a-160">Az SSH-alagút segítségével-e férni.</span><span class="sxs-lookup"><span data-stu-id="11c0a-160">You can use an SSH tunnel to access it.</span></span> <span data-ttu-id="11c0a-161">További információ az SSH-alagút használatával, lásd: a [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="11c0a-161">For more information on using an SSH tunnel, see the [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="11c0a-162">Az SSH-alagút létesítése, tegye a következőket a Solr irányítópult használata:</span><span class="sxs-lookup"><span data-stu-id="11c0a-162">Once you have established an SSH tunnel, use the following steps to use the Solr dashboard:</span></span>

1. <span data-ttu-id="11c0a-163">Határozza meg az elsődleges headnode a host name:</span><span class="sxs-lookup"><span data-stu-id="11c0a-163">Determine the host name for the primary headnode:</span></span>

   1. <span data-ttu-id="11c0a-164">SSH használatával csatlakozhat az átjárócsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="11c0a-164">Use SSH to connect to the cluster head node.</span></span> <span data-ttu-id="11c0a-165">Például: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="11c0a-165">For example, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

       <span data-ttu-id="11c0a-166">További információ az SSH használatával, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="11c0a-166">For more information on using SSH, see the [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

   2. <span data-ttu-id="11c0a-167">Az alábbi parancs segítségével a teljesen minősített nevének beolvasása:</span><span class="sxs-lookup"><span data-stu-id="11c0a-167">Use the following command to get the fully qualified hostname:</span></span>

        ```bash
        hostname -f
        ```

        <span data-ttu-id="11c0a-168">Ez a parancs értéket ad vissza a következő állomás neve hasonlít:</span><span class="sxs-lookup"><span data-stu-id="11c0a-168">This command returns a value similar to the following host name:</span></span>

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        <span data-ttu-id="11c0a-169">Az érték lett visszaadva, akkor akkor menteni, mert a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="11c0a-169">Save the value returned, as it is used later.</span></span>

2. <span data-ttu-id="11c0a-170">A böngészőben csatlakozzon a **http://HOSTNAME:8983/solr / #/**, ahol **ÁLLOMÁSNÉV** az előző lépésben maghatározott neve.</span><span class="sxs-lookup"><span data-stu-id="11c0a-170">In your browser, connect to **http://HOSTNAME:8983/solr/#/**, where **HOSTNAME** is the name you determined in the previous steps.</span></span>

    <span data-ttu-id="11c0a-171">A kérelem az SSH-alagút, a fürt Solr webes felhasználói felületén keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="11c0a-171">The request is routed through the SSH tunnel to the Solr web UI on your cluster.</span></span> <span data-ttu-id="11c0a-172">A lap jelenik meg az alábbi képen hasonlít:</span><span class="sxs-lookup"><span data-stu-id="11c0a-172">The page appears similar to the following image:</span></span>

    ![Solr irányítópult képe](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. <span data-ttu-id="11c0a-174">A bal oldali ablaktáblán, használja a **Core választó** legördülő listán válassza ki a **collection1**.</span><span class="sxs-lookup"><span data-stu-id="11c0a-174">From the left pane, use the **Core Selector** drop-down to select **collection1**.</span></span> <span data-ttu-id="11c0a-175">Több bejegyzést kell őket alatt jelennek meg **collection1**.</span><span class="sxs-lookup"><span data-stu-id="11c0a-175">Several entries should them appear below **collection1**.</span></span>

4. <span data-ttu-id="11c0a-176">Az alábbi bejegyzései **collection1**, jelölje be **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="11c0a-176">From the entries below **collection1**, select **Query**.</span></span> <span data-ttu-id="11c0a-177">Töltse fel adatokkal a lapon a következő értékeket használja:</span><span class="sxs-lookup"><span data-stu-id="11c0a-177">Use the following values to populate the search page:</span></span>

   * <span data-ttu-id="11c0a-178">Az a **q** szöveget adja meg a  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="11c0a-178">In the **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="11c0a-179">Ez a lekérdezés a indexelt dokumentumok Solr adja vissza.</span><span class="sxs-lookup"><span data-stu-id="11c0a-179">This query returns all the documents that are indexed in Solr.</span></span> <span data-ttu-id="11c0a-180">Ha szeretne keresni egy adott karakterláncot belül a dokumentumokhoz, karakterláncokat Itt adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="11c0a-180">If you want to search for a specific string within the documents, you can enter that string here.</span></span>
   * <span data-ttu-id="11c0a-181">Az a **wt** szöveg mezőben adja meg a kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="11c0a-181">In the **wt** text box, select the output format.</span></span> <span data-ttu-id="11c0a-182">Alapértelmezett érték a **json**.</span><span class="sxs-lookup"><span data-stu-id="11c0a-182">Default is **json**.</span></span>

     <span data-ttu-id="11c0a-183">Végül válassza ki a **lekérdezés végrehajtása** gombra a keresési pate alján.</span><span class="sxs-lookup"><span data-stu-id="11c0a-183">Finally, select the **Execute Query** button at the bottom of the search pate.</span></span>

     ![Parancsfájlművelet használni ahhoz, hogy a fürt](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     <span data-ttu-id="11c0a-185">A kimenet a korábban hozzáadott az indexhez két dokumentumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="11c0a-185">The output returns the two documents that you added to the index earlier.</span></span> <span data-ttu-id="11c0a-186">A kimenet a következő JSON-dokumentum hasonlít:</span><span class="sxs-lookup"><span data-stu-id="11c0a-186">The output is similar to the following JSON document:</span></span>

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

### <a name="starting-and-stopping-solr"></a><span data-ttu-id="11c0a-187">Indítása és leállítása Solr</span><span class="sxs-lookup"><span data-stu-id="11c0a-187">Starting and stopping Solr</span></span>

<span data-ttu-id="11c0a-188">A következő parancsok segítségével manuálisan állítsa le és indítsa el a Solr:</span><span class="sxs-lookup"><span data-stu-id="11c0a-188">Use the following commands to manually stop and start Solr:</span></span>

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a><span data-ttu-id="11c0a-189">Biztonsági mentési indexelt adatokat</span><span class="sxs-lookup"><span data-stu-id="11c0a-189">Backup indexed data</span></span>

<span data-ttu-id="11c0a-190">Adatok biztonsági mentésének Solr az alapértelmezett tárhelyre, a fürt tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="11c0a-190">Use the following steps to back up Solr data to the default storage for your cluster:</span></span>

1. <span data-ttu-id="11c0a-191">Csatlakozzon a fürthöz SSH használatával, akkor a gazdagép nevét, az átjárócsomópont a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="11c0a-191">Connect to the cluster using SSH, then use the following command to get the host name for the head node:</span></span>

    ```bash
    hostname -f
    ```

2. <span data-ttu-id="11c0a-192">Az alábbi parancs segítségével készít pillanatképet az indexelt adatokat.</span><span class="sxs-lookup"><span data-stu-id="11c0a-192">Use the following command to create a snapshot of the indexed data.</span></span> <span data-ttu-id="11c0a-193">Cserélje le **ÁLLOMÁSNÉV** az előző parancs által visszaadott nevű:</span><span class="sxs-lookup"><span data-stu-id="11c0a-193">Replace **HOSTNAME** with the name returned from the previous command:</span></span>

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    <span data-ttu-id="11c0a-194">A válasz a következő XML hasonlít:</span><span class="sxs-lookup"><span data-stu-id="11c0a-194">The response is similar to the following XML:</span></span>

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. <span data-ttu-id="11c0a-195">Módosítsa a könyvtárat a `/usr/hdp/current/solr/example/solr`.</span><span class="sxs-lookup"><span data-stu-id="11c0a-195">Change directories to `/usr/hdp/current/solr/example/solr`.</span></span> <span data-ttu-id="11c0a-196">Nincs gyűjtemény itt alkönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="11c0a-196">There is a subdirectory here for each collection.</span></span> <span data-ttu-id="11c0a-197">Minden gyűjtemény könyvtár neve tartalmazza a `data` ahhoz a gyűjteményhez a pillanatképet tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="11c0a-197">Each collection directory contains a `data` directory that contains the snapshot for the collection.</span></span>

4. <span data-ttu-id="11c0a-198">A pillanatkép mappája a tömörített archívum létrehozása, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="11c0a-198">To create a compressed archive of the snapshot folder, use the following command:</span></span>

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    <span data-ttu-id="11c0a-199">Cserélje le a `snapshot.20150806185338855` a nevet a gyűjtemény a pillanatkép értékeket.</span><span class="sxs-lookup"><span data-stu-id="11c0a-199">Replace the `snapshot.20150806185338855` values with the name of the snapshot for your collection.</span></span>

    <span data-ttu-id="11c0a-200">Ezzel a paranccsal létrejön az archívum neve **snapshot.20150806185338855.tgz**, amely tartalmazza a tartalmát a **snapshot.20150806185338855** directory.</span><span class="sxs-lookup"><span data-stu-id="11c0a-200">This command creates an archive named **snapshot.20150806185338855.tgz**, which contains the contents of the **snapshot.20150806185338855** directory.</span></span>

5. <span data-ttu-id="11c0a-201">Majd tárolhatja az archívum a fürt elsődleges Storage a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="11c0a-201">You can then store the archive to the cluster's primary storage using the following command:</span></span>

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

<span data-ttu-id="11c0a-202">Solr biztonsági mentés és visszaállítás használatához további információkért lásd: [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span><span class="sxs-lookup"><span data-stu-id="11c0a-202">For more information on working with Solr backup and restores, see [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span></span>

## <a name="next-steps"></a><span data-ttu-id="11c0a-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11c0a-203">Next steps</span></span>

* <span data-ttu-id="11c0a-204">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="11c0a-204">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="11c0a-205">A HDInsight Hadoop-fürtök Giraph telepítése a fürt testreszabási használatával.</span><span class="sxs-lookup"><span data-stu-id="11c0a-205">Use cluster customization to install Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="11c0a-206">Giraph lehetővé teszi a végrehajtását diagramfeldolgozás Hadoop használatával, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="11c0a-206">Giraph allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="11c0a-207">[A HDInsight-fürtökön Hue telepítése](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="11c0a-207">[Install Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="11c0a-208">HDInsight Hadoop-fürtök a Hue telepítése fürt testreszabási használata</span><span class="sxs-lookup"><span data-stu-id="11c0a-208">Use cluster customization to install Hue on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="11c0a-209">Hue olyan fürtökkel a Hadoop fürtök folytatott kommunikációhoz használható webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="11c0a-209">Hue is a set of Web applications used to interact with a Hadoop cluster.</span></span>

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
