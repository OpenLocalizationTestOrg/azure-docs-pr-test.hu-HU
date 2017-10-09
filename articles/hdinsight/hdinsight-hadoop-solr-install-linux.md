---
title: "aaaUse parancsfájlművelet tooinstall a Linux-alapú HDInsight - Azure Solr |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall Solr a Linux-alapú HDInsight Hadoop clusters Parancsfájlműveletek használatával."
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
ms.openlocfilehash: 4c179032b95ae187f1830d8927f8796372fa8ebe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-solr-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="fbfd2-103">Telepítheti és használhatja Solr HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="fbfd2-103">Install and use Solr on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="fbfd2-104">Megtudhatja, hogyan tooinstall Solr on Azure HDInsight-parancsfájlművelet használatával.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-104">Learn how tooinstall Solr on Azure HDInsight by using Script Action.</span></span> <span data-ttu-id="fbfd2-105">Solr egy hatékony keresési platform, és a Hadoop által kezelt adatok vállalati szintű keresési funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-105">Solr is a powerful search platform and provides enterprise-level search capabilities on data managed by Hadoop.</span></span>

> [!IMPORTANT]
    > <span data-ttu-id="fbfd2-106">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-106">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="fbfd2-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fbfd2-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fbfd2-109">a dokumentumban használt hello mintaparancsfájl Solr 4.9 egy adott konfigurációval telepíti.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-109">hello sample script used in this document installs Solr 4.9 with a specific configuration.</span></span> <span data-ttu-id="fbfd2-110">Ha azt szeretné, hogy tooconfigure hello Solr fürt különböző gyűjteményeket, szilánkok, sémákat, replikákat, stb., módosítania kell hello parancsfájl és Solr bináris fájljait.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-110">If you want tooconfigure hello Solr cluster with different collections, shards, schemas, replicas, etc., you must modify hello script and Solr binaries.</span></span>

## <span data-ttu-id="fbfd2-111"><a name="whatis"></a>Mi az a Solr</span><span class="sxs-lookup"><span data-stu-id="fbfd2-111"><a name="whatis"></a>What is Solr</span></span>

<span data-ttu-id="fbfd2-112">[Apache Solr](http://lucene.apache.org/solr/features.html) egy vállalati keresési platform, amely lehetővé teszi az adatok hatékony teljes szöveges keresés.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-112">[Apache Solr](http://lucene.apache.org/solr/features.html) is an enterprise search platform that enables powerful full-text search on data.</span></span> <span data-ttu-id="fbfd2-113">Hadoop lehetővé teszi a tárolásához és kezeléséhez az adatok óriási mennyiségben, Apache Solr hello keresési képességeket biztosít tooquickly lekérése hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-113">While Hadoop enables storing and managing vast amounts of data, Apache Solr provides hello search capabilities tooquickly retrieve hello data.</span></span>

> [!WARNING]
> <span data-ttu-id="fbfd2-114">A Microsoft hello HDInsight-fürt összetevői teljes mértékben támogatja.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-114">Components provided with hello HDInsight cluster are fully supported by Microsoft.</span></span>
>
> <span data-ttu-id="fbfd2-115">Egyéni összetevők, például Solr, minden üzleti szempontból ésszerű támogatási toohelp fogadni, toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-115">Custom components, such as Solr, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="fbfd2-116">A Microsoft támogatási nem lehet egyéni összetevőkkel tudja tooresolve kapcsolatos problémák.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-116">Microsoft support may not be able tooresolve problems with custom components.</span></span> <span data-ttu-id="fbfd2-117">Szükség lehet tooengage hello nyílt forráskódú Közösségek segítségért.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-117">You may need tooengage hello open source communities for assistance.</span></span> <span data-ttu-id="fbfd2-118">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-118">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>

## <a name="what-hello-script-does"></a><span data-ttu-id="fbfd2-119">Milyen hello parancsprogram</span><span class="sxs-lookup"><span data-stu-id="fbfd2-119">What hello script does</span></span>

<span data-ttu-id="fbfd2-120">Ezt a parancsfájlt a következő módosításokat toohello HDInsight-fürt hello teszi:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-120">This script makes hello following changes toohello HDInsight cluster:</span></span>

* <span data-ttu-id="fbfd2-121">4.9 Solr történő telepítése`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="fbfd2-121">Installs Solr 4.9 into `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="fbfd2-122">Létrehoz egy felhasználói **solrusr**, vagyis használt toorun hello Solr szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="fbfd2-122">Creates a user, **solrusr**, which is used toorun hello Solr service</span></span>
* <span data-ttu-id="fbfd2-123">Készletek **solruser** hello tulajdonosaként`/usr/hdp/current/solr`</span><span class="sxs-lookup"><span data-stu-id="fbfd2-123">Sets **solruser** as hello owner of `/usr/hdp/current/solr`</span></span>
* <span data-ttu-id="fbfd2-124">Hozzáad egy [Upstart](http://upstart.ubuntu.com/) konfigurációjához, amely Solr automatikusan elindul.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-124">Adds an [Upstart](http://upstart.ubuntu.com/) configuration that starts Solr automatically.</span></span>

## <span data-ttu-id="fbfd2-125"><a name="install"></a>A Parancsfájlműveletek segítségével Solr telepítése</span><span class="sxs-lookup"><span data-stu-id="fbfd2-125"><a name="install"></a>Install Solr using Script Actions</span></span>

<span data-ttu-id="fbfd2-126">Egy minta parancsfájlt tooinstall Solr egy HDInsight-fürt hello a következő helyen érhető el:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-126">A sample script tooinstall Solr on an HDInsight cluster is available at hello following location:</span></span>

    https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh

<span data-ttu-id="fbfd2-127">toocreate telepítve Solr rendelkező fürtnek, használjon hello szükséges lépések hello [HDInsight-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-portal.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-127">toocreate a cluster that has Solr installed, use hello steps in hello [Create HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md) document.</span></span> <span data-ttu-id="fbfd2-128">Hello létrehozási folyamat során a következő lépéseket tooinstall Solr hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-128">During hello creation process, use hello following steps tooinstall Solr:</span></span>

1. <span data-ttu-id="fbfd2-129">A hello __fürt összefoglaló__ panelen, select__Advanced settings__, majd __parancsfájl-műveletek__.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-129">From hello __Cluster summary__ blade, select__Advanced settings__, then __Script actions__.</span></span> <span data-ttu-id="fbfd2-130">A következő információk toopopulate hello űrlap hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-130">Use hello following information toopopulate hello form:</span></span>

   * <span data-ttu-id="fbfd2-131">**NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-131">**NAME**: Enter a friendly name for hello script action.</span></span>
   * <span data-ttu-id="fbfd2-132">**PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span><span class="sxs-lookup"><span data-stu-id="fbfd2-132">**SCRIPT URI**: https://hdiconfigactions.blob.core.windows.net/linuxsolrconfigactionv01/solr-installer-v01.sh</span></span>
   * <span data-ttu-id="fbfd2-133">**HEAD**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="fbfd2-133">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="fbfd2-134">**MUNKAVÉGZŐ**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="fbfd2-134">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="fbfd2-135">**ZOOKEEPER**: Ez a beállítás tooinstall hello Zookeeper csomóponton ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="fbfd2-135">**ZOOKEEPER**: Check this option tooinstall on hello Zookeeper node</span></span>
   * <span data-ttu-id="fbfd2-136">**PARAMÉTEREK**: ezt a mezőt hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="fbfd2-136">**PARAMETERS**: Leave this field blank</span></span>

2. <span data-ttu-id="fbfd2-137">Hello hello alján **parancsfájl-műveletek** panelen, használjon hello **válasszon** gombok toosave hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-137">At hello bottom of hello **Script actions** blade, use hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="fbfd2-138">Végül, használja a hello **következő** gomb tooreturn toohello __fürt összegzése__</span><span class="sxs-lookup"><span data-stu-id="fbfd2-138">Finally, use hello **Next** button tooreturn toohello __Cluster summary__</span></span>

3. <span data-ttu-id="fbfd2-139">A hello __fürt összefoglaló__ lapon jelölje be __létrehozása__ toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-139">From hello __Cluster summary__ page, select __Create__ toocreate hello cluster.</span></span>

## <span data-ttu-id="fbfd2-140"><a name="usesolr"></a>Solr használata a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="fbfd2-140"><a name="usesolr"></a>How do I use Solr in HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fbfd2-141">Ebben a szakaszban található lépéseket hello alapszolgáltatásai Solr bemutatása.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-141">hello steps in this section demonstrate basic Solr functionality.</span></span> <span data-ttu-id="fbfd2-142">Solr használatával kapcsolatos további információkért lásd: hello [Apache Solr hely](http://lucene.apache.org/solr/).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-142">For more information on using Solr, see hello [Apache Solr site](http://lucene.apache.org/solr/).</span></span>

### <a name="index-data"></a><span data-ttu-id="fbfd2-143">Index adatok</span><span class="sxs-lookup"><span data-stu-id="fbfd2-143">Index data</span></span>

<span data-ttu-id="fbfd2-144">A következő lépéseket tooadd példa adatok tooSolr hello használja, és majd lekérdezése:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-144">Use hello following steps tooadd example data tooSolr, and then query it:</span></span>

1. <span data-ttu-id="fbfd2-145">Csatlakozzon az SSH használatával toohello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-145">Connect toohello HDInsight cluster using SSH:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fbfd2-146">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-146">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="fbfd2-147">A jelen dokumentum lépéseit az SSL protokollbújtatás tooconnect toohello Solr webes felhasználói felület használja.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-147">Steps later in this document use an SSL tunnel tooconnect toohello Solr web UI.</span></span> <span data-ttu-id="fbfd2-148">Ezek a lépések toouse, kell létesítenie az SSL protokollbújtatás, és konfigurálja a böngésző toouse azt.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-148">toouse these steps, you must establish an SSL tunnel and then configure your browser toouse it.</span></span>
     >
     > <span data-ttu-id="fbfd2-149">További információkért lásd: hello [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-149">For more information, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="fbfd2-150">A következő parancsok toohave Solr index mintaadatok hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-150">Use hello following commands toohave Solr index sample data:</span></span>

    ```bash
    cd /usr/hdp/current/solr/example/exampledocs
    java -jar post.jar solr.xml monitor.xml
    ```

    <span data-ttu-id="fbfd2-151">hello következő kimenetet visszaadja toohello konzol:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-151">hello following output is returned toohello console:</span></span>

        POSTing file solr.xml
        POSTing file monitor.xml
        2 files indexed.
        COMMITting Solr index changes toohttp://localhost:8983/solr/update..
        Time spent: 0:00:01.624

    <span data-ttu-id="fbfd2-152">Hello `post.jar` segédprogram hozzáadja hello **solr.xml** és **monitor.xml** dokumentumok toohello index.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-152">hello `post.jar` utility adds hello **solr.xml** and **monitor.xml** documents toohello index.</span></span>
  
3. <span data-ttu-id="fbfd2-153">A következő parancs tooquery hello Solr REST API hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-153">Use hello following command tooquery hello Solr REST API:</span></span>

    ```bash
    curl "http://localhost:8983/solr/collection1/select?q=*%3A*&wt=json&indent=true"
    ```

    <span data-ttu-id="fbfd2-154">Ez a parancs keres **collection1** megfelelő dokumentumokhoz  **\*:\***  (kódolva \*% 3A\* hello a lekérdezési karakterláncban).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-154">This command searches **collection1** for any documents matching **\*:\*** (encoded as \*%3A\* in hello query string).</span></span> <span data-ttu-id="fbfd2-155">a következő JSON-dokumentum hello hello válasz példája:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-155">hello following JSON document is an example of hello response:</span></span>

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

### <a name="using-hello-solr-dashboard"></a><span data-ttu-id="fbfd2-156">Hello Solr irányítópult használata</span><span class="sxs-lookup"><span data-stu-id="fbfd2-156">Using hello Solr dashboard</span></span>

<span data-ttu-id="fbfd2-157">hello Solr irányítópult a webes felhasználói felület, amely lehetővé teszi a Solr toowork webböngésző segítségével.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-157">hello Solr dashboard is a web UI that allows you toowork with Solr through your web browser.</span></span> <span data-ttu-id="fbfd2-158">hello Solr irányítópult közvetlenül a hello Internet a HDInsight-fürtjéhez nem lesz közzétéve.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-158">hello Solr dashboard is not exposed directly on hello Internet from your HDInsight cluster.</span></span> <span data-ttu-id="fbfd2-159">Egy SSH-alagút tooaccess használhatja azt.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-159">You can use an SSH tunnel tooaccess it.</span></span> <span data-ttu-id="fbfd2-160">További információ az SSH-alagút használatával, lásd: hello [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-160">For more information on using an SSH tunnel, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

<span data-ttu-id="fbfd2-161">Az SSH-alagút létesítése, használja a következő lépéseket toouse hello Solr irányítópult hello:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-161">Once you have established an SSH tunnel, use hello following steps toouse hello Solr dashboard:</span></span>

1. <span data-ttu-id="fbfd2-162">Hello host name hello elsődleges headnode határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-162">Determine hello host name for hello primary headnode:</span></span>

   1. <span data-ttu-id="fbfd2-163">SSH tooconnect toohello átjárócsomóponthoz használja.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-163">Use SSH tooconnect toohello cluster head node.</span></span> <span data-ttu-id="fbfd2-164">Például: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-164">For example, `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

       <span data-ttu-id="fbfd2-165">További információ az SSH használatával, lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-165">For more information on using SSH, see hello [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

   2. <span data-ttu-id="fbfd2-166">A következő parancs tooget hello teljesen minősített állomásnév hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-166">Use hello following command tooget hello fully qualified hostname:</span></span>

        ```bash
        hostname -f
        ```

        <span data-ttu-id="fbfd2-167">Ez a parancs visszaadja a gazdagép neve a következő érték hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-167">This command returns a value similar toohello following host name:</span></span>

            hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net

        <span data-ttu-id="fbfd2-168">Hello érték lett visszaadva, akkor menteni, mert a rendszer később.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-168">Save hello value returned, as it is used later.</span></span>

2. <span data-ttu-id="fbfd2-169">A böngészőben csatlakozzon túl**http://HOSTNAME:8983/solr / #/**, ahol **ÁLLOMÁSNÉV** hello előző lépésben maghatározott hello neve.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-169">In your browser, connect too**http://HOSTNAME:8983/solr/#/**, where **HOSTNAME** is hello name you determined in hello previous steps.</span></span>

    <span data-ttu-id="fbfd2-170">hello kérelem hello SSH alagút toohello Solr webes felhasználói felület a fürt keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-170">hello request is routed through hello SSH tunnel toohello Solr web UI on your cluster.</span></span> <span data-ttu-id="fbfd2-171">hello lap jelenik meg a következő kép hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-171">hello page appears similar toohello following image:</span></span>

    ![Solr irányítópult képe](./media/hdinsight-hadoop-solr-install-linux/solrdashboard.png)

3. <span data-ttu-id="fbfd2-173">Hello bal oldali ablaktáblán, használja a hello **Core választó** legördülő tooselect **collection1**.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-173">From hello left pane, use hello **Core Selector** drop-down tooselect **collection1**.</span></span> <span data-ttu-id="fbfd2-174">Több bejegyzést kell őket alatt jelennek meg **collection1**.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-174">Several entries should them appear below **collection1**.</span></span>

4. <span data-ttu-id="fbfd2-175">Az alábbi hello bejegyzései **collection1**, jelölje be **lekérdezés**.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-175">From hello entries below **collection1**, select **Query**.</span></span> <span data-ttu-id="fbfd2-176">A következő értékek toopopulate hello keresőoldalt hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-176">Use hello following values toopopulate hello search page:</span></span>

   * <span data-ttu-id="fbfd2-177">A hello **q** szöveget adja meg a  **\*:**\*.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-177">In hello **q** text box, enter **\*:**\*.</span></span> <span data-ttu-id="fbfd2-178">Ez a lekérdezés minden olyan hello dokumentumok indexelt Solr adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-178">This query returns all hello documents that are indexed in Solr.</span></span> <span data-ttu-id="fbfd2-179">Ha azt szeretné, toosearch hello dokumentumok belül egy adott karakterláncot, karakterláncokat Itt adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-179">If you want toosearch for a specific string within hello documents, you can enter that string here.</span></span>
   * <span data-ttu-id="fbfd2-180">A hello **wt** szövegmezőben, jelölje be hello kimeneti formátum.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-180">In hello **wt** text box, select hello output format.</span></span> <span data-ttu-id="fbfd2-181">Alapértelmezett érték a **json**.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-181">Default is **json**.</span></span>

     <span data-ttu-id="fbfd2-182">Végül válassza ki a hello **lekérdezés végrehajtása** hello keresési pate hello alján gombra.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-182">Finally, select hello **Execute Query** button at hello bottom of hello search pate.</span></span>

     ![Parancsfájlművelet toocustomize egy fürt használja.](./media/hdinsight-hadoop-solr-install-linux/hdi-solr-dashboard-query.png)

     <span data-ttu-id="fbfd2-184">hello kimeneti toohello hozzáadásának két dokumentumok korábbi index hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-184">hello output returns hello two documents that you added toohello index earlier.</span></span> <span data-ttu-id="fbfd2-185">a kimeneti hello van a hasonló toohello következő JSON-dokumentum:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-185">hello output is similar toohello following JSON document:</span></span>

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

### <a name="starting-and-stopping-solr"></a><span data-ttu-id="fbfd2-186">Indítása és leállítása Solr</span><span class="sxs-lookup"><span data-stu-id="fbfd2-186">Starting and stopping Solr</span></span>

<span data-ttu-id="fbfd2-187">A következő parancsok toomanually állítsa le és indítsa el a Solr hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-187">Use hello following commands toomanually stop and start Solr:</span></span>

```bash
sudo stop solr
sudo start solr
```

## <a name="backup-indexed-data"></a><span data-ttu-id="fbfd2-188">Biztonsági mentési indexelt adatokat</span><span class="sxs-lookup"><span data-stu-id="fbfd2-188">Backup indexed data</span></span>

<span data-ttu-id="fbfd2-189">A következő lépéseket tooback Solr adatok toohello alapértelmezett szolgáltatás a fürt telepítése hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-189">Use hello following steps tooback up Solr data toohello default storage for your cluster:</span></span>

1. <span data-ttu-id="fbfd2-190">Csatlakozzon az SSH használatával toohello fürt, majd a következő parancs tooget hello host name hello átjárócsomópont hello használata:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-190">Connect toohello cluster using SSH, then use hello following command tooget hello host name for hello head node:</span></span>

    ```bash
    hostname -f
    ```

2. <span data-ttu-id="fbfd2-191">A következő parancs toocreate indexelt hello adatok pillanatképe hello használata.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-191">Use hello following command toocreate a snapshot of hello indexed data.</span></span> <span data-ttu-id="fbfd2-192">Cserélje le **ÁLLOMÁSNÉV** hello előző parancs által visszaadott hello nevű:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-192">Replace **HOSTNAME** with hello name returned from hello previous command:</span></span>

    ```bash
    curl http://HOSTNAME:8983/solr/replication?command=backup
    ```

    <span data-ttu-id="fbfd2-193">a rendszer a következő XML hasonló toohello hello választ:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-193">hello response is similar toohello following XML:</span></span>

        <?xml version="1.0" encoding="UTF-8"?>
        <response>
          <lst name="responseHeader">
            <int name="status">0</int>
            <int name="QTime">9</int>
          </lst>
          <str name="status">OK</str>
        </response>

3. <span data-ttu-id="fbfd2-194">Módosítsa a könyvtárat túl`/usr/hdp/current/solr/example/solr`.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-194">Change directories too`/usr/hdp/current/solr/example/solr`.</span></span> <span data-ttu-id="fbfd2-195">Nincs gyűjtemény itt alkönyvtárat.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-195">There is a subdirectory here for each collection.</span></span> <span data-ttu-id="fbfd2-196">Minden gyűjtemény könyvtár neve tartalmazza a `data` hello pillanatkép hello gyűjtemény tartalmazó könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-196">Each collection directory contains a `data` directory that contains hello snapshot for hello collection.</span></span>

4. <span data-ttu-id="fbfd2-197">a tömörített archívum hello pillanatkép mappa, a következő parancs használata hello toocreate:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-197">toocreate a compressed archive of hello snapshot folder, use hello following command:</span></span>

    ```bash
    tar -zcf snapshot.20150806185338855.tgz snapshot.20150806185338855
    ```

    <span data-ttu-id="fbfd2-198">Cserélje le a hello `snapshot.20150806185338855` hello nevet a gyűjtemény hello pillanatkép értékeket.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-198">Replace hello `snapshot.20150806185338855` values with hello name of hello snapshot for your collection.</span></span>

    <span data-ttu-id="fbfd2-199">Ez a parancs létrehozza az archívumot nevű **snapshot.20150806185338855.tgz**, hello hello tartalmát tartalmazó **snapshot.20150806185338855** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-199">This command creates an archive named **snapshot.20150806185338855.tgz**, which contains hello contents of hello **snapshot.20150806185338855** directory.</span></span>

5. <span data-ttu-id="fbfd2-200">Majd tárolhatja elsődleges tárolóhely hello archív toohello fürt hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fbfd2-200">You can then store hello archive toohello cluster's primary storage using hello following command:</span></span>

    ```bash
    hdfs dfs -put snapshot.20150806185338855.tgz /example/data
    ```

<span data-ttu-id="fbfd2-201">Solr biztonsági mentés és visszaállítás használatához további információkért lásd: [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-201">For more information on working with Solr backup and restores, see [https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups](https://cwiki.apache.org/confluence/display/solr/Making+and+Restoring+Backups).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fbfd2-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbfd2-202">Next steps</span></span>

* <span data-ttu-id="fbfd2-203">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-203">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="fbfd2-204">Fürt testreszabási tooinstall Giraph a HDInsight Hadoop-fürtök használata.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-204">Use cluster customization tooinstall Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="fbfd2-205">Giraph lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-205">Giraph allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="fbfd2-206">[A HDInsight-fürtökön Hue telepítése](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="fbfd2-206">[Install Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="fbfd2-207">HDInsight Hadoop-fürtök fürt testreszabási tooinstall Hue használja.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-207">Use cluster customization tooinstall Hue on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="fbfd2-208">Hue, a webalkalmazások a használt toointeract egy Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fbfd2-208">Hue is a set of Web applications used toointeract with a Hadoop cluster.</span></span>

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
