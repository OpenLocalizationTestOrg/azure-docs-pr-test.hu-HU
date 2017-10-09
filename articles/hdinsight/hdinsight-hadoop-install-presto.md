---
title: aaaInstall Presto Azure HDInsight Linux clusters |} Microsoft Docs
description: "Ismerje meg, hogyan tooinstall Presto és Airpal a Linux-alapú HDInsight Hadoop-fürtök Parancsfájlműveletek használatával."
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
ms.openlocfilehash: 5d54d0efc3e5fdc6f5a8d3a94ad2f61d16df24c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-and-use-presto-on-hdinsight-hadoop-clusters"></a><span data-ttu-id="1f881-103">Telepítheti és használhatja Presto HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="1f881-103">Install and use Presto on HDInsight Hadoop clusters</span></span>

<span data-ttu-id="1f881-104">Ebben a témakörben elsajátíthatja, hogyan tooinstall Presto a HDInsight Hadoop-fürtök parancsfájlművelet használatával.</span><span class="sxs-lookup"><span data-stu-id="1f881-104">In this topic, you learn how tooinstall Presto on HDInsight Hadoop clusters by using Script Action.</span></span> <span data-ttu-id="1f881-105">Azt is megtudhatja, hogyan tooinstall Airpal egy meglévő Presto HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="1f881-105">You also learn how tooinstall Airpal on an existing Presto HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f881-106">hello jelen dokumentumban leírt lépések szükséges egy **HDInsight 3.5 Hadoop-fürt** , amely Linux használ.</span><span class="sxs-lookup"><span data-stu-id="1f881-106">hello steps in this document require an **HDInsight 3.5 Hadoop cluster** that uses Linux.</span></span> <span data-ttu-id="1f881-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="1f881-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1f881-108">További információkért lásd: [HDInsight-verziókról](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-108">For more information, see [HDInsight versions](hdinsight-component-versioning.md).</span></span>

## <a name="what-is-presto"></a><span data-ttu-id="1f881-109">Mi az az Presto?</span><span class="sxs-lookup"><span data-stu-id="1f881-109">What is Presto?</span></span>
<span data-ttu-id="1f881-110">[Presto](https://prestodb.io/overview.html) egy gyors elosztott SQL lekérdezési motor a big data.</span><span class="sxs-lookup"><span data-stu-id="1f881-110">[Presto](https://prestodb.io/overview.html) is a fast distributed SQL query engine for big data.</span></span> <span data-ttu-id="1f881-111">Presto alkalmas adatmennyiségig interaktív lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="1f881-111">Presto is suitable for interactive querying of petabytes of data.</span></span> <span data-ttu-id="1f881-112">Presto, és hogyan működnek együtt hello összetevők további információkért lásd: [Presto fogalmak](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span><span class="sxs-lookup"><span data-stu-id="1f881-112">For more information on hello components of Presto and how they work together, see [Presto concepts](https://github.com/prestodb/presto/blob/master/presto-docs/src/main/sphinx/overview/concepts.rst).</span></span>

> [!WARNING]
> <span data-ttu-id="1f881-113">Hello HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="1f881-113">Components provided with hello HDInsight cluster are fully supported and Microsoft Support will help tooisolate and resolve issues related toothese components.</span></span>
> 
> <span data-ttu-id="1f881-114">Egyéni összetevők, például Presto, minden üzleti szempontból ésszerű támogatási toohelp fogadni, toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1f881-114">Custom components, such as Presto, receive commercially reasonable support toohelp you toofurther troubleshoot hello issue.</span></span> <span data-ttu-id="1f881-115">Hello probléma megoldását, vagy kérni a tooengage elérhető csatorna az hello megnyitja részletes segítséget, hogy a technológiát találhatók technológiák eredményezhet.</span><span class="sxs-lookup"><span data-stu-id="1f881-115">This might result in resolving hello issue OR asking you tooengage available channels for hello open source technologies where deep expertise for that technology is found.</span></span> <span data-ttu-id="1f881-116">Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="1f881-116">For example, there are many community sites that can be used, like: [MSDN forum for HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Also Apache projects have project sites on [http://apache.org](http://apache.org), for example: [Hadoop](http://hadoop.apache.org/).</span></span>
> 
> 


## <a name="install-presto-using-script-action"></a><span data-ttu-id="1f881-117">Parancsfájl műveletével Presto telepítése</span><span class="sxs-lookup"><span data-stu-id="1f881-117">Install Presto using script action</span></span>

<span data-ttu-id="1f881-118">Ez a szakasz útmutatás hogyan toouse hello parancsfájlpélda használatával új fürt létrehozásakor hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="1f881-118">This section provides instructions on how toouse hello sample script when creating a new cluster by using hello Azure portal.</span></span> 

1. <span data-ttu-id="1f881-119">Fürt elkezdhessen a hello lépések segítségével [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-119">Start provisioning a cluster by using hello steps in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span> <span data-ttu-id="1f881-120">Ellenőrizze, hogy hello segítségével hello fürtöt hoz létre **egyéni** fürt létrehozási folyamata.</span><span class="sxs-lookup"><span data-stu-id="1f881-120">Make sure you create hello cluster using hello **Custom** cluster creation flow.</span></span> <span data-ttu-id="1f881-121">Gondoskodnia kell arról, létrehozhat hello fürthöz megfelel-e hello követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="1f881-121">You must ensure that hello cluster you create meets hello following requirements.</span></span>

    <span data-ttu-id="1f881-122">a.</span><span class="sxs-lookup"><span data-stu-id="1f881-122">a.</span></span> <span data-ttu-id="1f881-123">A 3.5-ös verziója HDInsight Hadoop-fürttel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1f881-123">It must be a Hadoop cluster with HDInsight version 3.5.</span></span>

    <span data-ttu-id="1f881-124">b.</span><span class="sxs-lookup"><span data-stu-id="1f881-124">b.</span></span> <span data-ttu-id="1f881-125">Azure Storage azt kell használnia, mint hello adattár.</span><span class="sxs-lookup"><span data-stu-id="1f881-125">It must use Azure Storage as hello data store.</span></span> <span data-ttu-id="1f881-126">Presto használatával olyan fürtön, használó Azure Data Lake Store hello tárolási lehetőség még nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="1f881-126">Using Presto on a cluster that uses Azure Data Lake Store as hello storage option is not yet supported.</span></span> 

    ![Egyéni beállítások használata a HDInsight-fürt létrehozása](./media/hdinsight-hadoop-install-presto/hdinsight-install-custom.png)

2. <span data-ttu-id="1f881-128">A hello **speciális beállítások** panelen válassza **Parancsfájlműveletek**, és adja meg az alábbi részleteket hello:</span><span class="sxs-lookup"><span data-stu-id="1f881-128">On hello **Advanced settings** blade, select **Script Actions**, and provide hello information below:</span></span>
   
   * <span data-ttu-id="1f881-129">**NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.</span><span class="sxs-lookup"><span data-stu-id="1f881-129">**NAME**: Enter a friendly name for hello script action.</span></span>
   * <span data-ttu-id="1f881-130">**Bash-szkript URI azonosítója**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span><span class="sxs-lookup"><span data-stu-id="1f881-130">**Bash script URI**: `https://raw.githubusercontent.com/hdinsight/presto-hdinsight/master/installpresto.sh`</span></span>
   * <span data-ttu-id="1f881-131">**HEAD**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="1f881-131">**HEAD**: Check this option</span></span>
   * <span data-ttu-id="1f881-132">**MUNKAVÉGZŐ**: ezt a beállítást</span><span class="sxs-lookup"><span data-stu-id="1f881-132">**WORKER**: Check this option</span></span>
   * <span data-ttu-id="1f881-133">**ZOOKEEPER**: törölje a jelet a jelölőnégyzetből</span><span class="sxs-lookup"><span data-stu-id="1f881-133">**ZOOKEEPER**: Clear this check box</span></span>
   * <span data-ttu-id="1f881-134">**PARAMÉTEREK**: ezt a mezőt hagyja üresen</span><span class="sxs-lookup"><span data-stu-id="1f881-134">**PARAMETERS**: Leave this field blank</span></span>


3. <span data-ttu-id="1f881-135">Hello hello alján **Parancsfájlműveletek** panelen hello kattintson **válasszon** gombok toosave hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="1f881-135">At hello bottom of hello **Script Actions** blade, click hello **Select** button toosave hello configuration.</span></span> <span data-ttu-id="1f881-136">Végül kattintson a hello **válassza** hello hello alján gomb **speciális beállítások** panel toosave hello konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="1f881-136">Finally, click  hello **Select** button at hello bottom of hello **Advanced Settings** blade toosave hello configuration information.</span></span>

4. <span data-ttu-id="1f881-137">Kiépítés hello fürt, a folytatáshoz [Provision Linux-alapú HDInsight-fürtök](hdinsight-hadoop-create-linux-clusters-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-137">Continue provisioning hello cluster as described in [Provision Linux-based HDInsight clusters](hdinsight-hadoop-create-linux-clusters-portal.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="1f881-138">Az Azure PowerShell, a hello Azure parancssori felület, a hello HDInsight .NET SDK vagy az Azure Resource Manager-sablonok is használt tooapply Parancsfájlműveletek.</span><span class="sxs-lookup"><span data-stu-id="1f881-138">Azure PowerShell, hello Azure CLI, hello HDInsight .NET SDK, or Azure Resource Manager templates can also be used tooapply script actions.</span></span> <span data-ttu-id="1f881-139">A futó fürtök parancsfájl műveletek tooalready is alkalmazhat.</span><span class="sxs-lookup"><span data-stu-id="1f881-139">You can also apply script actions tooalready running clusters.</span></span> <span data-ttu-id="1f881-140">További információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-140">For more information, see [Customize HDInsight clusters with Script Actions](hdinsight-hadoop-customize-cluster-linux.md).</span></span>
    > 
    > 

## <a name="use-presto-with-hdinsight"></a><span data-ttu-id="1f881-141">Presto használata a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="1f881-141">Use Presto with HDInsight</span></span>

<span data-ttu-id="1f881-142">Hajtsa végre a fent ismertetett lépéseket hello segítségével telepítése után a következő lépéseket toouse Presto a HDInsight-fürtök hello.</span><span class="sxs-lookup"><span data-stu-id="1f881-142">Perform hello following steps toouse Presto in an HDInsight cluster after you have installed it using hello steps described above.</span></span>

1. <span data-ttu-id="1f881-143">Csatlakozzon az SSH használatával toohello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="1f881-143">Connect toohello HDInsight cluster using SSH:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="1f881-144">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-144">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>
     

2. <span data-ttu-id="1f881-145">Indítsa el a hello Presto rendszerhéj hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="1f881-145">Start hello Presto shell using hello following command.</span></span>
   
        presto --schema default

3. <span data-ttu-id="1f881-146">Lekérdezés futtatható egy mintatáblát **hivesampletable**, alapértelmezés szerint az összes HDInsight-fürtök elérhető.</span><span class="sxs-lookup"><span data-stu-id="1f881-146">Run a query on a sample table, **hivesampletable**, which is available on all HDInsight clusters by default.</span></span>
   
        select count (*) from hivesampletable;
   
    <span data-ttu-id="1f881-147">Alapértelmezés szerint [Hive](https://prestodb.io/docs/current/connector/hive.html) és [TPCH](https://prestodb.io/docs/current/connector/tpch.html) csatlakozók a Presto már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="1f881-147">By default, [Hive](https://prestodb.io/docs/current/connector/hive.html) and [TPCH](https://prestodb.io/docs/current/connector/tpch.html) connectors for Presto are already configured.</span></span> <span data-ttu-id="1f881-148">Hive összekötő konfigurált toouse hello alapértelmezés szerint telepített Hive telepítési, így a Hive táblák összes hello Presto automatikusan látható lesz.</span><span class="sxs-lookup"><span data-stu-id="1f881-148">Hive connector is configured toouse hello default installed Hive installation, so all hello tables from Hive will be automatically visible in Presto.</span></span>

    <span data-ttu-id="1f881-149">A Presto használatát egy részletes ismertetését lásd: [Presto dokumentáció](https://prestodb.io/docs/current/index.html).</span><span class="sxs-lookup"><span data-stu-id="1f881-149">For a detailed description on how you can use Presto, see [Presto documentation](https://prestodb.io/docs/current/index.html).</span></span>

## <a name="use-airpal-with-presto"></a><span data-ttu-id="1f881-150">Presto Airpal használata</span><span class="sxs-lookup"><span data-stu-id="1f881-150">Use Airpal with Presto</span></span>

<span data-ttu-id="1f881-151">[Airpal](https://github.com/airbnb/airpal#airpal) Presto van egy nyílt forráskódú webes lekérdezési felületet.</span><span class="sxs-lookup"><span data-stu-id="1f881-151">[Airpal](https://github.com/airbnb/airpal#airpal) is an open-source web-based query interface for Presto.</span></span> <span data-ttu-id="1f881-152">A Airpal további információkért lásd: [Airpal dokumentáció](https://github.com/airbnb/airpal#airpal).</span><span class="sxs-lookup"><span data-stu-id="1f881-152">For more information on Airpal, see [Airpal documentation](https://github.com/airbnb/airpal#airpal).</span></span>

<span data-ttu-id="1f881-153">Ebben a szakaszban úgy tekintünk hello lépéseket túl**Airpal telepíthető hello edgenode** egy HDInsight Hadoop-fürt, amelyen már megtalálható a Presto telepítve.</span><span class="sxs-lookup"><span data-stu-id="1f881-153">In this section, we look at hello steps too**install Airpal on hello edgenode** of an HDInsight Hadoop cluster, that already has Presto installed.</span></span> <span data-ttu-id="1f881-154">Ez biztosítja, hogy hello Airpal webes lekérdezés illesztő elérhető hello interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="1f881-154">This ensures that hello Airpal web query interface is available over hello Internet.</span></span>

1. <span data-ttu-id="1f881-155">SSH használatával csatlakozzon a toohello headnode Presto telepített hello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="1f881-155">Using SSH, connect toohello headnode of hello HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="1f881-156">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-156">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="1f881-157">Miután csatlakozott, futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="1f881-157">Once you are connected, run hello following command.</span></span>

        sudo slider registry  --name presto1 --getexp presto 
   
    <span data-ttu-id="1f881-158">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="1f881-158">You should see an output like hello following:</span></span>

        {
            "coordinator_address" : [ {
                "value" : "10.0.0.12:9090",
                "level" : "application",
                "updatedTime" : "Mon Apr 03 20:13:41 UTC 2017"
        } ]

3. <span data-ttu-id="1f881-159">A hello kimenet, vegye figyelembe a hello hello értéke **érték** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="1f881-159">From hello output, note hello value for hello **value** property.</span></span> <span data-ttu-id="1f881-160">Szüksége lesz a hello fürt edgenode Airpal telepítése közben.</span><span class="sxs-lookup"><span data-stu-id="1f881-160">You will need this while installing Airpal on hello cluster edgenode.</span></span> <span data-ttu-id="1f881-161">Hello kimenetéről fent hello érték, amelyre szüksége lesz az **10.0.0.12:9090**.</span><span class="sxs-lookup"><span data-stu-id="1f881-161">From hello output above, hello value that you will need is **10.0.0.12:9090**.</span></span>

4. <span data-ttu-id="1f881-162">Hello sablon használata  **[Itt](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)**  toocreate egy HDInsight fürt edgenode, és adjon meg hello értékeket, ahogy az alábbi képernyőfelvétel a hello.</span><span class="sxs-lookup"><span data-stu-id="1f881-162">Use hello template **[here](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2Fpresto-hdinsight%2Fmaster%2Fairpal-deploy.json)** toocreate an HDInsight cluster edgenode and provide hello values as shown in hello following screenshot.</span></span>

    ![HDInsight telepítés Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-install-airpal.png)

5. <span data-ttu-id="1f881-164">Kattintson a **Purchase** (Vásárlás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1f881-164">Click **Purchase**.</span></span>

6. <span data-ttu-id="1f881-165">Hello módosítások alkalmazott toohello fürtkonfiguráció, után a hello Airpal webes felület hello lépések használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="1f881-165">Once hello changes are applied toohello cluster configuration, you can access hello Airpal web interface by using hello following steps.</span></span>

    <span data-ttu-id="1f881-166">a.</span><span class="sxs-lookup"><span data-stu-id="1f881-166">a.</span></span> <span data-ttu-id="1f881-167">Hello-fürt panelén kattintson **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1f881-167">From hello cluster blade, click **Applications**.</span></span>

    ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal.png)

    <span data-ttu-id="1f881-169">b.</span><span class="sxs-lookup"><span data-stu-id="1f881-169">b.</span></span> <span data-ttu-id="1f881-170">A hello **telepített alkalmazások** panelen kattintson a **Portal** airpal ellen.</span><span class="sxs-lookup"><span data-stu-id="1f881-170">From hello **Installed Apps** blade, click **Portal** against airpal.</span></span>

    ![HDInsight indítási Airpal Presto fürtön](./media/hdinsight-hadoop-install-presto/hdinsight-presto-launch-airpal-1.png)

    <span data-ttu-id="1f881-172">c.</span><span class="sxs-lookup"><span data-stu-id="1f881-172">c.</span></span> <span data-ttu-id="1f881-173">Amikor a rendszer kéri, adja meg a hello rendszergazdai hitelesítő adataival megadott hello HDInsight Hadoop-fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="1f881-173">When prompted, enter hello admin credentials that you specified while creating hello HDInsight Hadoop cluster.</span></span>

## <a name="customize-a-presto-installation-on-hdinsight-cluster"></a><span data-ttu-id="1f881-174">A HDInsight-fürt Presto telepítés testreszabása</span><span class="sxs-lookup"><span data-stu-id="1f881-174">Customize a Presto installation on HDInsight cluster</span></span>

<span data-ttu-id="1f881-175">Presto egy HDInsight Hadoop-fürt telepítése után hello telepítési toomake módosítások – például a frissítés memóriabeállításait testreszabása, módosítsa az összekötők stb. Hajtsa végre a következő lépéseket toodo így hello.</span><span class="sxs-lookup"><span data-stu-id="1f881-175">After you have installed Presto on an HDInsight Hadoop cluster, you can customize hello installation toomake changes such as update memory settings, change connectors, etc. Perform hello following steps toodo so.</span></span>

1. <span data-ttu-id="1f881-176">SSH használatával csatlakozzon a toohello headnode Presto telepített hello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="1f881-176">Using SSH, connect toohello headnode of hello HDInsight cluster that has Presto installed:</span></span>
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="1f881-177">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-177">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="1f881-178">A konfigurációs módosításokat hello fájlban `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span><span class="sxs-lookup"><span data-stu-id="1f881-178">Make your configuration changes in hello file `/var/lib/presto/presto-hdinsight-master/appConfig-default.json`.</span></span> <span data-ttu-id="1f881-179">Presto konfiguráció további információkért lásd: [YARN-alapú fürtök Presto konfigurációs](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span><span class="sxs-lookup"><span data-stu-id="1f881-179">For more information on Presto configuration, see [Presto configuration for YARN-based clusters](https://prestodb.io/presto-yarn/installation-yarn-configuration-options.html).</span></span>

3. <span data-ttu-id="1f881-180">Állítsa le és hello aktuális futó példányát Presto kill.</span><span class="sxs-lookup"><span data-stu-id="1f881-180">Stop and kill hello current running instance of Presto.</span></span>

        sudo slider stop presto1 --force
        sudo slider destroy presto1 --force

4. <span data-ttu-id="1f881-181">Egy új példányát Presto kezdődnie hello testreszabása.</span><span class="sxs-lookup"><span data-stu-id="1f881-181">Start a new instance of Presto with hello customization.</span></span>

       sudo slider create presto1 --template /var/lib/presto/presto-hdinsight-master/appConfig-default.json --resources /var/lib/presto/presto-hdinsight-master/resources-default.json

5. <span data-ttu-id="1f881-182">Várjon, amíg hello új példány toobe készen áll, és jegyezze fel a presto koordinátorának címe.</span><span class="sxs-lookup"><span data-stu-id="1f881-182">Wait for hello new instance toobe ready and note presto coordinator address.</span></span>


       sudo slider registry --name presto1 --getexp presto

## <a name="generate-benchmark-data-for-hdinsight-clusters-that-run-presto"></a><span data-ttu-id="1f881-183">Teljesítményteszt adatok Presto futtató HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1f881-183">Generate benchmark data for HDInsight clusters that run Presto</span></span>

<span data-ttu-id="1f881-184">TPC-DS hello iparági szabvány sok döntési támogatási rendszerek, beleértve a big data-rendszereket hello teljesítményének méréséhez.</span><span class="sxs-lookup"><span data-stu-id="1f881-184">TPC-DS is hello industry standard for measuring hello performance of many decision support systems, including big data systems.</span></span> <span data-ttu-id="1f881-185">HDInsight-fürtök toogenerate adatok Presto használatát, és értékelje ki, hogyan összehasonlítja saját HDInsight teljesítményteszt adatokkal.</span><span class="sxs-lookup"><span data-stu-id="1f881-185">You can use Presto on HDInsight clusters toogenerate data and evaluate how it compares with your own HDInsight benchmark data.</span></span> <span data-ttu-id="1f881-186">További információkért lásd: [Itt](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-186">For more information, see [here](https://github.com/hdinsight/tpcds-datagen-as-hive-query/blob/master/README.md).</span></span>



## <a name="see-also"></a><span data-ttu-id="1f881-187">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1f881-187">See also</span></span>
* <span data-ttu-id="1f881-188">[Telepítheti és használhatja a HDInsight-fürtök Hue](hdinsight-hadoop-hue-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-188">[Install and use Hue on HDInsight clusters](hdinsight-hadoop-hue-linux.md).</span></span> <span data-ttu-id="1f881-189">Hue webes felhasználói felületén, így könnyen toocreate, futtassa az és mentse a Pig és Hive-feladatok, is, keresse meg hello alapértelmezett storage a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1f881-189">Hue is a web UI that makes it easy toocreate, run and save Pig and Hive jobs, as well as browse hello default storage for your HDInsight cluster.</span></span>

* <span data-ttu-id="1f881-190">[Giraph telepíthető a HDInsight-fürtök](hdinsight-hadoop-giraph-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-190">[Install Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install-linux.md).</span></span> <span data-ttu-id="1f881-191">Fürt testreszabási tooinstall Giraph a HDInsight Hadoop-fürtök használata.</span><span class="sxs-lookup"><span data-stu-id="1f881-191">Use cluster customization tooinstall Giraph on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="1f881-192">Giraph lehetővé teszi a Hadoop használatával tooperform diagramfeldolgozás, és az Azure HDInsight használható.</span><span class="sxs-lookup"><span data-stu-id="1f881-192">Giraph allows you tooperform graph processing by using Hadoop, and can be used with Azure HDInsight.</span></span>

* <span data-ttu-id="1f881-193">[Solr telepíthető a HDInsight-fürtök](hdinsight-hadoop-solr-install-linux.md).</span><span class="sxs-lookup"><span data-stu-id="1f881-193">[Install Solr on HDInsight clusters](hdinsight-hadoop-solr-install-linux.md).</span></span> <span data-ttu-id="1f881-194">Fürt testreszabási tooinstall Solr a HDInsight Hadoop-fürtök használata.</span><span class="sxs-lookup"><span data-stu-id="1f881-194">Use cluster customization tooinstall Solr on HDInsight Hadoop clusters.</span></span> <span data-ttu-id="1f881-195">Solr lehetővé teszi tooperform hatékony keresési műveletek tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="1f881-195">Solr allows you tooperform powerful search operations on stored data.</span></span>

[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
