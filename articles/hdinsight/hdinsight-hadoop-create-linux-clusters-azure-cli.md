---
title: "aaaCreate Hadoop-fürtök parancssori - hello Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate HDInsight-fürtök hello platformfüggetlen Azure CLI 1.0."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 5295b01054b8c23df0e3b75a3e0e8c933ac48b3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-using-hello-azure-cli"></a><span data-ttu-id="fb927-103">Hello Azure CLI használata a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb927-103">Create HDInsight clusters using hello Azure CLI</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="fb927-104">Ez a dokumentum az útmutató egy HDInsight 3.5 hello Azure CLI 1.0 fürtöt hoz létre hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="fb927-104">hello steps in this document walk-through creating a HDInsight 3.5 cluster using hello Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb927-105">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="fb927-105">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fb927-106">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="fb927-106">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="fb927-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fb927-107">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="fb927-108">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="fb927-108">**An Azure subscription**.</span></span> <span data-ttu-id="fb927-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="fb927-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="fb927-110">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="fb927-110">**Azure CLI**.</span></span> <span data-ttu-id="fb927-111">hello jelen dokumentumban leírt lépések alapján utolsó történő teszteléskor az Azure CLI 0.10.14 verziója.</span><span class="sxs-lookup"><span data-stu-id="fb927-111">hello steps in this document were last tested with Azure CLI version 0.10.14.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fb927-112">Ebben a dokumentumban hello lépések nem használhatóak az Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="fb927-112">hello steps in this document do not work with Azure CLI 2.0.</span></span> <span data-ttu-id="fb927-113">Az Azure CLI 2.0 nem támogatja a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb927-113">Azure CLI 2.0 does not support creating an HDInsight cluster.</span></span>

## <a name="log-in-tooyour-azure-subscription"></a><span data-ttu-id="fb927-114">Jelentkezzen be tooyour Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="fb927-114">Log in tooyour Azure subscription</span></span>

<span data-ttu-id="fb927-115">Részletes ismertetését lásd: hello lépésekkel [tooan Azure-előfizetés csatlakoztatja a hello Azure parancssori felület (CLI)](../xplat-cli-connect.md) , és csatlakozzon a feliratkozás tooyour hello **bejelentkezési** metódust.</span><span class="sxs-lookup"><span data-stu-id="fb927-115">Follow hello steps documented in [Connect tooan Azure subscription from hello Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) and connect tooyour subscription using hello **login** method.</span></span>

## <a name="create-a-cluster"></a><span data-ttu-id="fb927-116">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb927-116">Create a cluster</span></span>

<span data-ttu-id="fb927-117">a parancssorból, például a PowerShell vagy a Bash hello a következő lépéseket kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="fb927-117">hello following steps should be performed from a command line, such as PowerShell or Bash.</span></span>

1. <span data-ttu-id="fb927-118">A következő parancs tooauthenticate tooyour Azure-előfizetés hello használata:</span><span class="sxs-lookup"><span data-stu-id="fb927-118">Use hello following command tooauthenticate tooyour Azure subscription:</span></span>

        azure login

    <span data-ttu-id="fb927-119">Ön felszólító tooprovide vannak, a nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fb927-119">You are prompted tooprovide your name and password.</span></span> <span data-ttu-id="fb927-120">Ha több Azure-előfizetéssel rendelkezik, `azure account set <subscriptionname>` tooset hello-előfizetéssel, amely az Azure parancssori felület hello parancsok használata.</span><span class="sxs-lookup"><span data-stu-id="fb927-120">If you have multiple Azure subscriptions, use `azure account set <subscriptionname>` tooset hello subscription that hello Azure CLI commands use.</span></span>

2. <span data-ttu-id="fb927-121">Váltás tooAzure Resource Manager módra a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="fb927-121">Switch tooAzure Resource Manager mode using hello following command:</span></span>

        azure config mode arm

3. <span data-ttu-id="fb927-122">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="fb927-122">Create a resource group.</span></span> <span data-ttu-id="fb927-123">Ez az erőforráscsoport hello HDInsight-fürtöt tartalmaz, és a kapcsolódó tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fb927-123">This resource group contains hello HDInsight cluster and associated storage account.</span></span>

        azure group create groupname location

    * <span data-ttu-id="fb927-124">Cserélje le `groupname` hello csoport egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="fb927-124">Replace `groupname` with a unique name for hello group.</span></span>

    * <span data-ttu-id="fb927-125">Cserélje le `location` a hello földrajzi régiót, amelyben a kívánt toocreate hello csoport.</span><span class="sxs-lookup"><span data-stu-id="fb927-125">Replace `location` with hello geographic region that you want toocreate hello group in.</span></span>

       <span data-ttu-id="fb927-126">Az érvényese helyek listáját, használja a hello `azure location list` parancsot, és használja a hello hello helyek egyikén `Name` oszlop.</span><span class="sxs-lookup"><span data-stu-id="fb927-126">For a list of valid locations, use hello `azure location list` command, and then use one of hello locations from hello `Name` column.</span></span>

4. <span data-ttu-id="fb927-127">Hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="fb927-127">Create a storage account.</span></span> <span data-ttu-id="fb927-128">Ez a tárfiók hello alapértelmezett tárolóként szolgál hello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fb927-128">This storage account is used as hello default storage for hello HDInsight cluster.</span></span>

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * <span data-ttu-id="fb927-129">Cserélje le `groupname` hello nevű hello előző lépésben létrehozott hello csoport.</span><span class="sxs-lookup"><span data-stu-id="fb927-129">Replace `groupname` with hello name of hello group created in hello previous step.</span></span>

    * <span data-ttu-id="fb927-130">Cserélje le `location` a hello hello előző lépésben használt ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="fb927-130">Replace `location` with hello same location used in hello previous step.</span></span>

    * <span data-ttu-id="fb927-131">Cserélje le `storagename` hello tárfiók egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="fb927-131">Replace `storagename` with a unique name for hello storage account.</span></span>

        > [!NOTE]
        > <span data-ttu-id="fb927-132">További információt ezen parancs hello paraméterekkel, `azure storage account create -h` tooview súgó ennél a parancsnál.</span><span class="sxs-lookup"><span data-stu-id="fb927-132">For more information on hello parameters used in this command, use `azure storage account create -h` tooview help for this command.</span></span>

5. <span data-ttu-id="fb927-133">Kérje le hello tooaccess hello tárfiókot használja.</span><span class="sxs-lookup"><span data-stu-id="fb927-133">Retrieve hello key used tooaccess hello storage account.</span></span>

        azure storage account keys list -g groupname storagename

    * <span data-ttu-id="fb927-134">Cserélje le `groupname` a hello erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="fb927-134">Replace `groupname` with hello resource group name.</span></span>
    * <span data-ttu-id="fb927-135">Cserélje le `storagename` hello nevű hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="fb927-135">Replace `storagename` with hello name of hello storage account.</span></span>

     <span data-ttu-id="fb927-136">A visszaadott hello adatokat, mentse a hello `key` értékének `key1`.</span><span class="sxs-lookup"><span data-stu-id="fb927-136">In hello data that is returned, save hello `key` value for `key1`.</span></span>

6. <span data-ttu-id="fb927-137">HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fb927-137">Create an HDInsight cluster.</span></span>

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * <span data-ttu-id="fb927-138">Cserélje le `groupname` a hello erőforráscsoport-név.</span><span class="sxs-lookup"><span data-stu-id="fb927-138">Replace `groupname` with hello resource group name.</span></span>

    * <span data-ttu-id="fb927-139">Cserélje le `Hadoop` , hogy kívánja-e toocreate hello fürt típussal.</span><span class="sxs-lookup"><span data-stu-id="fb927-139">Replace `Hadoop` with hello cluster type that you wish toocreate.</span></span> <span data-ttu-id="fb927-140">Például `Hadoop`, `HBase`, `Kafka`, `Spark`, vagy `Storm`.</span><span class="sxs-lookup"><span data-stu-id="fb927-140">For example, `Hadoop`, `HBase`, `Kafka`, `Spark`, or `Storm`.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="fb927-141">HDInsight fürtök különböző típusainak, amely toohello munkaterhelés térjen vagy technológia, amely a fürt hello hangolt.</span><span class="sxs-lookup"><span data-stu-id="fb927-141">HDInsight clusters come in various types, which correspond toohello workload or technology that hello cluster is tuned for.</span></span> <span data-ttu-id="fb927-142">Nincs nem támogatott metódus toocreate egy fürt, amely többféle, például a Storm és HBase egy fürtön egyesíti.</span><span class="sxs-lookup"><span data-stu-id="fb927-142">There is no supported method toocreate a cluster that combines multiple types, such as Storm and HBase on one cluster.</span></span>

    * <span data-ttu-id="fb927-143">Cserélje le `location` az előző lépésben használt ugyanazon a helyen hello.</span><span class="sxs-lookup"><span data-stu-id="fb927-143">Replace `location` with hello same location used in previous steps.</span></span>

    * <span data-ttu-id="fb927-144">Cserélje le `storagename` a hello tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="fb927-144">Replace `storagename` with hello storage account name.</span></span>

    * <span data-ttu-id="fb927-145">Cserélje le `storagekey` hello előző lépésben beolvasott hello kulccsal.</span><span class="sxs-lookup"><span data-stu-id="fb927-145">Replace `storagekey` with hello key obtained in hello previous step.</span></span>

    * <span data-ttu-id="fb927-146">A hello `--defaultStorageContainer` paraméter, használjon hello azonos nevét, ahogy hello fürt használja.</span><span class="sxs-lookup"><span data-stu-id="fb927-146">For hello `--defaultStorageContainer` parameter, use hello same name as you are using for hello cluster.</span></span>

    * <span data-ttu-id="fb927-147">Cserélje le `admin` és `httppassword` hello névvel és jelszóval kívánja toouse hello fürt keresztül HTTPS elérésekor.</span><span class="sxs-lookup"><span data-stu-id="fb927-147">Replace `admin` and `httppassword` with hello name and password you wish toouse when accessing hello cluster through HTTPS.</span></span>

    * <span data-ttu-id="fb927-148">Cserélje le `sshuser` és `sshuserpassword` hello felhasználónévvel és jelszóval kívánja toouse hello fürt SSH használatával való hozzáféréskor</span><span class="sxs-lookup"><span data-stu-id="fb927-148">Replace `sshuser` and `sshuserpassword` with hello username and password you wish toouse when accessing hello cluster using SSH</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fb927-149">Ebben a példában a fürt két munkavégző megjegyzéseket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fb927-149">This example creates a cluster with two worker notes.</span></span> <span data-ttu-id="fb927-150">Skálázási műveletek elvégzésével fürt létrehozása után is módosíthatja hello feldolgozó csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="fb927-150">You can also change hello number of worker nodes after cluster creation by performing scaling operations.</span></span> <span data-ttu-id="fb927-151">Ha több mint 32 munkavégző csomópontokhoz használatát tervezi, majd ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM.</span><span class="sxs-lookup"><span data-stu-id="fb927-151">If you plan on using more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14-GB RAM.</span></span> <span data-ttu-id="fb927-152">Hello segítségével beállíthatja a hello átjárócsomópont mérete `--headNodeSize` paraméter fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="fb927-152">You can set hello head node size by using hello `--headNodeSize` parameter during cluster creation.</span></span>
    >
    > <span data-ttu-id="fb927-153">A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="fb927-153">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

    <span data-ttu-id="fb927-154">Hello fürt létrehozási folyamat toofinish több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="fb927-154">It may take several minutes for hello cluster creation process toofinish.</span></span> <span data-ttu-id="fb927-155">Általában körülbelül 15.</span><span class="sxs-lookup"><span data-stu-id="fb927-155">Usually around 15.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="fb927-156">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="fb927-156">Troubleshoot</span></span>

<span data-ttu-id="fb927-157">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="fb927-157">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb927-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb927-158">Next steps</span></span>

<span data-ttu-id="fb927-159">Most, hogy sikeresen létrehozott egy HDInsight-fürtjéhez hello Azure CLI, használja a következő toolearn hogyan hello toowork a fürthöz:</span><span class="sxs-lookup"><span data-stu-id="fb927-159">Now that you have successfully created an HDInsight cluster using hello Azure CLI, use hello following toolearn how toowork with your cluster:</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="fb927-160">Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="fb927-160">Hadoop clusters</span></span>

* [<span data-ttu-id="fb927-161">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="fb927-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="fb927-162">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="fb927-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="fb927-163">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="fb927-163">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="fb927-164">HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="fb927-164">HBase clusters</span></span>

* [<span data-ttu-id="fb927-165">Az a HDInsight HBase első lépései</span><span class="sxs-lookup"><span data-stu-id="fb927-165">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="fb927-166">A HDInsight HBase Java-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="fb927-166">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="fb927-167">Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="fb927-167">Storm clusters</span></span>

* [<span data-ttu-id="fb927-168">Java-topológiák fejlesztése hdinsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="fb927-168">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="fb927-169">A HDInsight alatt futó Storm Python-összetevőket használnak</span><span class="sxs-lookup"><span data-stu-id="fb927-169">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="fb927-170">Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="fb927-170">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
