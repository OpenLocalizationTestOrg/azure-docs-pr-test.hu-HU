---
title: "Azure HDInsight Linux fürtökön Presto telepítése |} Microsoft Docs"
description: "Megtudhatja, hogyan telepíthetők Presto és Airpal Parancsfájlműveletek Linux-alapú HDInsight Hadoop-fürtök."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2017
ms.author: nitinme
ms.openlocfilehash: 406ef84e72d253fec51a0b37c48f326dafd511b6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="774f8-103">Telepítheti és használhatja Presto HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="774f8-103">Install and use Presto on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="774f8-104">Ebben a témakörben elsajátíthatja, hogyan telepítse Presto a HDInsight Hadoop-fürtök parancsfájlművelet.</span><span class="sxs-lookup"><span data-stu-id="774f8-104">In this topic, you learn how to install Presto on HDInsight Hadoop clusters by using Script Action.</span></span> <span data-ttu-id="774f8-105">Is megismerheti, hogyan Airpal telepítsen egy meglévő Presto HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="774f8-105">You also learn how to install Airpal on an existing Presto HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="774f8-106">A jelen dokumentumban leírt lépések szükséges egy **HDInsight 3.5 Hadoop-fürt** , amely Linux használ.</span><span class="sxs-lookup"><span data-stu-id="774f8-106">The steps in this document require an **HDInsight 3.5 Hadoop cluster** that uses Linux.</span></span> <span data-ttu-id="774f8-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="774f8-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="774f8-108">További információkért lásd: [HDInsight-verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-108">For more information, see [HDInsight versions](hdinsight-component-versioning.md).</span></span>

## <a name="what-is-presto"></a><span data-ttu-id="774f8-109">Mi az az Presto?</span><span class="sxs-lookup"><span data-stu-id="774f8-109">What is Presto?</span></span>
<span data-ttu-id="774f8-110">[Presto](https://prestodb.io/overview.html) egy gyors elosztott SQL lekérdezési motor a big data.</span><span class="sxs-lookup"><span data-stu-id="774f8-110">[Presto](https://prestodb.io/overview.html) is a fast distributed SQL query engine for big data.</span></span> <span data-ttu-id="774f8-111">Presto alkalmas adatmennyiségig interaktív lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="774f8-111">Presto is suitable for interactive querying of petabytes of data.</span></span> <span data-ttu-id="774f8-112">Presto, és hogyan működnek együtt a-összetevők további információkért lásd: [Presto fogalmak](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span><span class="sxs-lookup"><span data-stu-id="774f8-112">For more information on the components of Presto and how they work together, see [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span></span>

> [!WARNING]
> <span data-ttu-id="774f8-113">A HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog help elkülönítésére, és ezeket az összetevőket kapcsolatos problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="774f8-113">Components provided with the HDInsight cluster are fully supported and Microsoft Support will help to isolate and resolve issues related to these components.</span></span>
> 
> <span data-ttu-id="774f8-114">Egyéni összetevők, például Presto, minden üzleti szempontból ésszerű terméktámogatási segítséget nyújtanak a probléma további hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="774f8-114">Custom components, such as Presto, receive commercially reasonable support to help you to further troubleshoot the issue.</span></span> <span data-ttu-id="774f8-115">A probléma megoldását, vagy kéri fel, a nyílt forráskódú technológiák, ahol a részletes segítséget, hogy a technológiát található elérhető csatorna végezhetnek eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="774f8-115">This might result in resolving the issue OR asking you to engage available channels for the open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="774f8-116">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span><span class="sxs-lookup"><span data-stu-id="774f8-116">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com).</span></span> <span data-ttu-id="774f8-117">Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="774f8-117">Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>
> 
> 


## <a name="install-presto-using-script-action"></a><span data-ttu-id="774f8-118">Parancsfájl műveletével Presto telepítése</span><span class="sxs-lookup"><span data-stu-id="774f8-118">Install Presto using script action</span></span>

<span data-ttu-id="774f8-119">Ez a szakasz útmutatásai parancsfájlpélda használatával, ha az új fürt létrehozása az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="774f8-119">This section provides instructions on how to use the sample script when creating a new cluster by using the Azure portal.</span></span> 

1. <span data-ttu-id="774f8-120">Indítsa el a fürt kiépítése a lépések segítségével [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-120">Start provisioning a cluster by using the steps in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span> <span data-ttu-id="774f8-121">Győződjön meg arról, hogy a fürt használata a **egyéni** fürt létrehozási folyamata.</span><span class="sxs-lookup"><span data-stu-id="774f8-121">Make sure you create the cluster using the **Custom** cluster creation flow.</span></span> <span data-ttu-id="774f8-122">Győződjön meg arról, hogy a fürt létrehozása megfelel-e az alábbi követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="774f8-122">You must ensure that the cluster you create meets the following requirements.</span></span>

    <span data-ttu-id="774f8-123">a.</span><span class="sxs-lookup"><span data-stu-id="774f8-123">a.</span></span> <span data-ttu-id="774f8-124">A 3.5-ös verziója HDInsight Hadoop-fürttel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="774f8-124">It must be a Hadoop cluster with HDInsight version 3.5.</span></span>

    <span data-ttu-id="774f8-125">b.</span><span class="sxs-lookup"><span data-stu-id="774f8-125">b.</span></span> <span data-ttu-id="774f8-126">Azure Storage azt kell használnia, mint a tárolót.</span><span class="sxs-lookup"><span data-stu-id="774f8-126">It must use Azure Storage as the data store.</span></span> <span data-ttu-id="774f8-127">Presto használatával olyan fürtön, a tárolási lehetőség az Azure Data Lake Store használó még nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="774f8-127">Using Presto on a cluster that uses Azure Data Lake Store as the storage option is not yet supported.</span></span> 

    ![Egyéni beállítások használata a HDInsight-fürt létrehozása](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. <span data-ttu-id="774f8-129">Az a **speciális beállítások** panelen válassza **Parancsfájlműveletek**, és az alábbi adatokat:</span><span class="sxs-lookup"><span data-stu-id="774f8-129">On the **Advanced settings** blade, select **Script Actions**, and provide the information below:</span></span>
   
   * <span data-ttu-id="774f8-130">**NÉV**: Adja meg a parancsfájlművelet rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="774f8-130">**NAME**: Enter a friendly name for the script action.</span></span>
   * <span data-ttu-id="774f8-131">**Bash-szkript URI azonosítója**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span><span class="sxs-lookup"><span data-stu-id="774f8-131">**Bash script URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span></span>
   * <span data-ttu-id="774f8-132">**HEAD**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="774f8-132">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="774f8-133">**MUNKAVÉGZŐ**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="774f8-133">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="774f8-134">**ZOOKEEPER**: törölje a jelet a jelölőnégyzetből</span><span class="sxs-lookup"><span data-stu-id="774f8-134">**ZOOKEEPER**: Clear this check box</span></span>
   * <span data-ttu-id="774f8-135">**PARAMÉTEREK**: ezt a mezőt hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="774f8-135">**PARAMETERS**: Leave this field blank</span></span>


3. <span data-ttu-id="774f8-136">Alján a **Parancsfájlműveletek** panelen kattintson a **válasszon** gombra kattintva mentse a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="774f8-136">At the bottom of the **Script Actions** blade, click the **Select** button to save the configuration.</span></span> <span data-ttu-id="774f8-137">Végül kattintson a **válasszon** gomb alján a **speciális beállítások** panelt, és mentse a konfigurációs adatokat.</span><span class="sxs-lookup"><span data-stu-id="774f8-137">Finally, click  the **Select** button at the bottom of the **Advanced Settings** blade to save the configuration information.</span></span>

4. <span data-ttu-id="774f8-138">Továbbra is a fürt kiépítése a [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-138">Continue provisioning the cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="774f8-139">Az Azure PowerShell, az Azure parancssori felület, a HDInsight .NET SDK vagy Azure Resource Manager-sablonok Parancsfájlműveletek alkalmazandó is használható.</span><span class="sxs-lookup"><span data-stu-id="774f8-139">Azure PowerShell, the Azure CLI, the HDInsight .NET SDK, or Azure Resource Manager templates can also be used to apply script actions.</span></span> <span data-ttu-id="774f8-140">Már fut a fürtök Parancsfájlműveletek is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="774f8-140">You can also apply script actions to already running clusters.</span></span> <span data-ttu-id="774f8-141">További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-141">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    > 
    > 

## <a name="use-presto-with-hdinsight"></a><span data-ttu-id="774f8-142">Presto használata a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="774f8-142">Use Presto with HDInsight</span></span>

<span data-ttu-id="774f8-143">A következő lépésekkel Presto a HDInsight-fürtök használata után telepítette, akkor a fent leírt lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="774f8-143">Perform the following steps to use Presto in an HDInsight cluster after you have installed it using the steps described above.</span></span>

1. <span data-ttu-id="774f8-144">Csatlakozzon SSH-val a HDInsight-fürthöz:</span><span class="sxs-lookup"><span data-stu-id="774f8-144">Connect to the HDInsight cluster using SSH:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="774f8-145">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
     

2. <span data-ttu-id="774f8-146">Indítsa el a következő parancsot a Presto rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="774f8-146">Start the Presto shell using the following command.</span></span>
   
        presto --schema default

3. <span data-ttu-id="774f8-147">Lekérdezés futtatható egy mintatáblát **hivesampletable**, alapértelmezés szerint az összes HDInsight-fürtök elérhető.</span><span class="sxs-lookup"><span data-stu-id="774f8-147">Run a query on a sample table, **hivesampletable**, which is available on all HDInsight clusters by default.</span></span>
   
        select count (*) from hivesampletable;
   
    <span data-ttu-id="774f8-148">Alapértelmezés szerint [Hive](https://prestodb.io/docs/current/connector/hive.html) és [TPCH](https://prestodb.io/docs/current/connector/tpch.html) csatlakozók a Presto már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="774f8-148">By default, [Hive](https://prestodb.io/docs/current/connector/hive.html) and [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors for Presto are already configured.</span></span> <span data-ttu-id="774f8-149">Hive összekötő használatát a alapértelmezés szerint telepített Hive telepítés Hive összes táblájának automatikusan láthatók lesznek a Presto van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="774f8-149">Hive connector is configured to use the default installed Hive installation, so all the tables from Hive will be automatically visible in Presto.</span></span>

    <span data-ttu-id="774f8-150">A Presto használatát egy részletes ismertetését lásd: [Presto dokumentáció](https://prestodb.io/docs/current/index.html).</span><span class="sxs-lookup"><span data-stu-id="774f8-150">For a detailed description on how you can use Presto, see [Presto documentation](https://prestodb.io/docs/current/index.html).</span></span>

## <a name="use-airpal-with-presto"></a><span data-ttu-id="774f8-151">Presto Airpal használata</span><span class="sxs-lookup"><span data-stu-id="774f8-151">Use Airpal with Presto</span></span>

<span data-ttu-id="774f8-152">[Airpal](https://github.com/airbnb/airpal#airpal) Presto van egy nyílt forráskódú webes lekérdezési felületet.</span><span class="sxs-lookup"><span data-stu-id="774f8-152">[Airpal](https://github.com/airbnb/airpal#airpal) is an open-source web-based query interface for Presto.</span></span> <span data-ttu-id="774f8-153">A Airpal további információkért lásd: [Airpal dokumentáció](https://github.com/airbnb/airpal#airpal).</span><span class="sxs-lookup"><span data-stu-id="774f8-153">For more information on Airpal, see [Airpal documentation](https://github.com/airbnb/airpal#airpal).</span></span>

<span data-ttu-id="774f8-154">Ebben a szakaszban úgy tekintünk lépéseit **Airpal telepíthető a edgenode** egy HDInsight Hadoop-fürt, amelyen már megtalálható a Presto telepítve.</span><span class="sxs-lookup"><span data-stu-id="774f8-154">In this section, we look at the steps to **install Airpal on the edgenode** of an HDInsight Hadoop cluster, that already has Presto installed.</span></span> <span data-ttu-id="774f8-155">Ez biztosítja, hogy a Airpal webes lekérdezési felületet az interneten keresztül elérhető legyen.</span><span class="sxs-lookup"><span data-stu-id="774f8-155">This ensures that the Airpal web query interface is available over the Internet.</span></span>

1. <span data-ttu-id="774f8-156">A HDInsight-fürt Presto telepített headnode csatlakozni SSH használatával:</span><span class="sxs-lookup"><span data-stu-id="774f8-156">Using SSH, connect to the headnode of the HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="774f8-157">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-157">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="774f8-158">Miután csatlakozott, a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="774f8-158">Once you are connected, run the following command.</span></span>

        sudo slider registry  --name presto1 --getexp presto 
   
    <span data-ttu-id="774f8-159">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="774f8-159">You should see an output like the following:</span></span>

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. <span data-ttu-id="774f8-160">A kimenetben jegyezze fel az értéket a **érték** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="774f8-160">From the output, note the value for the **value** property.</span></span> <span data-ttu-id="774f8-161">Szüksége lesz a fürt edgenode Airpal telepítése közben.</span><span class="sxs-lookup"><span data-stu-id="774f8-161">You will need this while installing Airpal on the cluster edgenode.</span></span> <span data-ttu-id="774f8-162">A fenti kimenetben, amelyre szüksége lesz értéke **10.0.0.12:9090**.</span><span class="sxs-lookup"><span data-stu-id="774f8-162">From the output above, the value that you will need is **10.0.0.12:9090**.</span></span>

4. <span data-ttu-id="774f8-163">A sablon  **[Itt](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  egy HDInsight-fürt edgenode létrehozásához, és adja meg az értékeket, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="774f8-163">Use the template **[here](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** to create an HDInsight cluster edgenode and provide the values as shown in the following screenshot.</span></span>

    ![HDInsight telepítés Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. <span data-ttu-id="774f8-165">Kattintson a **Purchase** (Vásárlás) gombra.</span><span class="sxs-lookup"><span data-stu-id="774f8-165">Click **Purchase**.</span></span>

6. <span data-ttu-id="774f8-166">Miután a módosításai érvényesek lesznek a fürtkonfiguráció, a Airpal webes felülete az alábbi lépéseket követve végezheti el.</span><span class="sxs-lookup"><span data-stu-id="774f8-166">Once the changes are applied to the cluster configuration, you can access the Airpal web interface by using the following steps.</span></span>

    <span data-ttu-id="774f8-167">a.</span><span class="sxs-lookup"><span data-stu-id="774f8-167">a.</span></span> <span data-ttu-id="774f8-168">A fürt paneljén kattintson **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="774f8-168">From the cluster blade, click **Applications**.</span></span>

    ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    <span data-ttu-id="774f8-170">b.</span><span class="sxs-lookup"><span data-stu-id="774f8-170">b.</span></span> <span data-ttu-id="774f8-171">Az a **telepített alkalmazások** panelen kattintson a **Portal** airpal ellen.</span><span class="sxs-lookup"><span data-stu-id="774f8-171">From the **Installed Apps** blade, click **Portal** against airpal.</span></span>

    ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    <span data-ttu-id="774f8-173">c.</span><span class="sxs-lookup"><span data-stu-id="774f8-173">c.</span></span> <span data-ttu-id="774f8-174">Amikor a rendszer kéri, írja be a HDInsight Hadoop-fürt létrehozásakor megadott rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="774f8-174">When prompted, enter the admin credentials that you specified while creating the HDInsight Hadoop cluster.</span></span>

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a><span data-ttu-id="774f8-175">A HDInsight-fürt Presto telepítés testreszabása</span><span class="sxs-lookup"><span data-stu-id="774f8-175">Customize a Presto installation on HDInsight cluster</span></span>

<span data-ttu-id="774f8-176">A frissítés telepítése után Presto egy HDInsight Hadoop-fürt, testre szabhatja a telepítést, a módosítások például memória-beállítások frissítése, módosítsa az összekötők stb. Ehhez kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="774f8-176">After you have installed Presto on an HDInsight Hadoop cluster, you can customize the installation to make changes such as update memory settings, change connectors, etc. Perform the following steps to do so.</span></span>

1. <span data-ttu-id="774f8-177">A HDInsight-fürt Presto telepített headnode csatlakozni SSH használatával:</span><span class="sxs-lookup"><span data-stu-id="774f8-177">Using SSH, connect to the headnode of the HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="774f8-178">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-178">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="774f8-179">A konfigurációs módosításokat a fájlban `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span><span class="sxs-lookup"><span data-stu-id="774f8-179">Make your configuration changes in the file `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span></span> <span data-ttu-id="774f8-180">Presto konfiguráció további információkért lásd: [YARN-alapú fürtök Presto konfigurációs](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span><span class="sxs-lookup"><span data-stu-id="774f8-180">For more information on Presto configuration, see [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span></span>

3. <span data-ttu-id="774f8-181">Állítsa le és kill Presto aktuális futó példányát.</span><span class="sxs-lookup"><span data-stu-id="774f8-181">Stop and kill the current running instance of Presto.</span></span>

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. <span data-ttu-id="774f8-182">Egy új példányát Presto kezdje a Testreszabás.</span><span class="sxs-lookup"><span data-stu-id="774f8-182">Start a new instance of Presto with the customization.</span></span>

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. <span data-ttu-id="774f8-183">Várjon, amíg az új példány készen áll, és jegyezze fel az presto koordinátorának címe.</span><span class="sxs-lookup"><span data-stu-id="774f8-183">Wait for the new instance to be ready and note presto coordinator address.</span></span>


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a><span data-ttu-id="774f8-184">Teljesítményteszt adatok Presto futtató HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="774f8-184">Generate benchmark data for HDInsight clusters that run Presto</span></span>

<span data-ttu-id="774f8-185">TPC-DS iparági szabvány sok döntési támogatási rendszerek, beleértve a big data-rendszereket teljesítményének méréséhez.</span><span class="sxs-lookup"><span data-stu-id="774f8-185">TPC-DS is the industry standard for measuring the performance of many decision support systems, including big data systems.</span></span> <span data-ttu-id="774f8-186">Segítségével Presto a HDInsight-fürtökön hozhat létre adatokat, és összehasonlítja azt a saját HDInsight teljesítményteszt adataival kiértékeléséhez.</span><span class="sxs-lookup"><span data-stu-id="774f8-186">You can use Presto on HDInsight clusters to generate data and evaluate how it compares with your own HDInsight benchmark data.</span></span> <span data-ttu-id="774f8-187">További információkért lásd: [Itt](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-187">For more information, see [here](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span></span>



## <a name="see-also"></a><span data-ttu-id="774f8-188">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="774f8-188">See also</span></span>
* <span data-ttu-id="774f8-189">[Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-189">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="774f8-190">Hue webes felhasználói felület, amellyel könnyedén hozhat létre, futtassa, és mentse a Pig és Hive-feladatok, valamint keresse meg az alapértelmezett storage a HDInsight fürt.</span><span class="sxs-lookup"><span data-stu-id="774f8-190">Hue is a web UI that makes it easy to create, run and save Pig and Hive jobs, as well as browse the default storage for your HDInsight cluster.</span></span>

* <span data-ttu-id="774f8-191">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-191">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="774f8-192">A HDInsight Hadoop-fürtök Giraph telepítése a fürt testreszabási használatával.</span><span class="sxs-lookup"><span data-stu-id="774f8-192">Use cluster customization to install Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="774f8-193">Giraph lehetővé teszi a végrehajtását diagramfeldolgozás Hadoop használatával, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="774f8-193">Giraph allows you to perform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="774f8-194">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="774f8-194">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> <span data-ttu-id="774f8-195">A HDInsight Hadoop-fürtök Solr telepítése a fürt testreszabási használatával.</span><span class="sxs-lookup"><span data-stu-id="774f8-195">Use cluster customization to install Solr on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="774f8-196">Solr tárolt adatok hatékony keresési műveletek végrehajtását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="774f8-196">Solr allows you to perform powerful search operations on stored data.</span></span>

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
