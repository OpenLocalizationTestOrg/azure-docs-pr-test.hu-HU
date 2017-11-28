---
title: "Használja a parancssori-Azure HDInsight Hadoop-fürtök létrehozása |} Microsoft Docs"
description: "Útmutató a platformok közötti Azure CLI 1.0 HDInsight-fürtök létrehozásához."
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
ms.openlocfilehash: 8f2fcb46789d000cd66164508f1159338dcae5f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a><span data-ttu-id="e7b6b-103">Az Azure parancssori felület használata a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="e7b6b-103">Create HDInsight clusters using the Azure CLI</span></span>

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="e7b6b-104">Ez a dokumentum az útmutató egy HDInsight 3.5-ös verzióját az Azure CLI 1.0 fürtöt hoz létre lépéseit.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-104">The steps in this document walk-through creating a HDInsight 3.5 cluster using the Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e7b6b-105">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-105">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="e7b6b-106">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="e7b6b-106">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="e7b6b-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e7b6b-107">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="e7b6b-108">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-108">**An Azure subscription**.</span></span> <span data-ttu-id="e7b6b-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e7b6b-109">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="e7b6b-110">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-110">**Azure CLI**.</span></span> <span data-ttu-id="e7b6b-111">A jelen dokumentumban leírt lépések alapján utolsó történő teszteléskor az Azure CLI 0.10.14 verziója.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-111">The steps in this document were last tested with Azure CLI version 0.10.14.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e7b6b-112">A jelen dokumentumban leírt lépések az Azure CLI 2.0 nem működik.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-112">The steps in this document do not work with Azure CLI 2.0.</span></span> <span data-ttu-id="e7b6b-113">Az Azure CLI 2.0 nem támogatja a HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-113">Azure CLI 2.0 does not support creating an HDInsight cluster.</span></span>

## <a name="log-in-to-your-azure-subscription"></a><span data-ttu-id="e7b6b-114">Bejelentkezés az Azure-előfizetésbe</span><span class="sxs-lookup"><span data-stu-id="e7b6b-114">Log in to your Azure subscription</span></span>

<span data-ttu-id="e7b6b-115">Kövesse a [Csatlakozás Azure-előfizetéshez az Azure parancssori felületről](../xplat-cli-connect.md) lépéseit, és csatlakoztassa az előfizetését a **bejelentkezéses** módszerrel.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-115">Follow the steps documented in [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) and connect to your subscription using the **login** method.</span></span>

## <a name="create-a-cluster"></a><span data-ttu-id="e7b6b-116">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="e7b6b-116">Create a cluster</span></span>

<span data-ttu-id="e7b6b-117">Az alábbi lépéseket kell elvégezni a parancssorból, például a PowerShell vagy a Bash.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-117">The following steps should be performed from a command line, such as PowerShell or Bash.</span></span>

1. <span data-ttu-id="e7b6b-118">A következő paranccsal, hogy az Azure-előfizetéshez hitelesítést:</span><span class="sxs-lookup"><span data-stu-id="e7b6b-118">Use the following command to authenticate to your Azure subscription:</span></span>

        azure login

    <span data-ttu-id="e7b6b-119">A felhasználónevet és jelszót kéri.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-119">You are prompted to provide your name and password.</span></span> <span data-ttu-id="e7b6b-120">Ha több Azure-előfizetéssel rendelkezik, `azure account set <subscriptionname>` , amelyek az Azure parancssori felület parancsait használják az előfizetés beállításához.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-120">If you have multiple Azure subscriptions, use `azure account set <subscriptionname>` to set the subscription that the Azure CLI commands use.</span></span>

2. <span data-ttu-id="e7b6b-121">Váltson Azure Resource Manager módra az alábbi paranccsal:</span><span class="sxs-lookup"><span data-stu-id="e7b6b-121">Switch to Azure Resource Manager mode using the following command:</span></span>

        azure config mode arm

3. <span data-ttu-id="e7b6b-122">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-122">Create a resource group.</span></span> <span data-ttu-id="e7b6b-123">Ez az erőforráscsoport a HDInsight-fürtöt tartalmaz, és a kapcsolódó tárfiók.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-123">This resource group contains the HDInsight cluster and associated storage account.</span></span>

        azure group create groupname location

    * <span data-ttu-id="e7b6b-124">Cserélje le `groupname` rendelkező egy egyedi nevet a csoport.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-124">Replace `groupname` with a unique name for the group.</span></span>

    * <span data-ttu-id="e7b6b-125">Cserélje le `location` az a földrajzi régiót, amelyet a csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-125">Replace `location` with the geographic region that you want to create the group in.</span></span>

       <span data-ttu-id="e7b6b-126">Az érvényese helyek listáját, használja a `azure location list` parancsot, és használja a helyek egyikét a `Name` oszlop.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-126">For a list of valid locations, use the `azure location list` command, and then use one of the locations from the `Name` column.</span></span>

4. <span data-ttu-id="e7b6b-127">Hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-127">Create a storage account.</span></span> <span data-ttu-id="e7b6b-128">Ezt a tárfiókot az alapértelmezett tárolóként szolgál a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-128">This storage account is used as the default storage for the HDInsight cluster.</span></span>

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * <span data-ttu-id="e7b6b-129">Cserélje le `groupname` nevű, a csoport az előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-129">Replace `groupname` with the name of the group created in the previous step.</span></span>

    * <span data-ttu-id="e7b6b-130">Cserélje le `location` az ugyanazon a helyen az előző lépésben használt.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-130">Replace `location` with the same location used in the previous step.</span></span>

    * <span data-ttu-id="e7b6b-131">Cserélje le `storagename` a tárfiók egyedi névvel.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-131">Replace `storagename` with a unique name for the storage account.</span></span>

        > [!NOTE]
        > <span data-ttu-id="e7b6b-132">Ez a parancs paraméterei a további információért futtassa `azure storage account create -h` a parancs súgójának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-132">For more information on the parameters used in this command, use `azure storage account create -h` to view help for this command.</span></span>

5. <span data-ttu-id="e7b6b-133">A tárfiók eléréséhez használt kulcs lekérését.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-133">Retrieve the key used to access the storage account.</span></span>

        azure storage account keys list -g groupname storagename

    * <span data-ttu-id="e7b6b-134">Cserélje le `groupname` és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-134">Replace `groupname` with the resource group name.</span></span>
    * <span data-ttu-id="e7b6b-135">Cserélje le `storagename` a tárfiók nevével.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-135">Replace `storagename` with the name of the storage account.</span></span>

     <span data-ttu-id="e7b6b-136">A visszaadott adatokat, mentse a `key` értékének `key1`.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-136">In the data that is returned, save the `key` value for `key1`.</span></span>

6. <span data-ttu-id="e7b6b-137">HDInsight-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-137">Create an HDInsight cluster.</span></span>

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * <span data-ttu-id="e7b6b-138">Cserélje le `groupname` és az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-138">Replace `groupname` with the resource group name.</span></span>

    * <span data-ttu-id="e7b6b-139">Cserélje le `Hadoop` létrehozza a fürt típussal.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-139">Replace `Hadoop` with the cluster type that you wish to create.</span></span> <span data-ttu-id="e7b6b-140">Például `Hadoop`, `HBase`, `Kafka`, `Spark`, vagy `Storm`.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-140">For example, `Hadoop`, `HBase`, `Kafka`, `Spark`, or `Storm`.</span></span>

     > [!IMPORTANT]
     > <span data-ttu-id="e7b6b-141">HDInsight fürtök térjen különféle, a munkaterhelés vagy technológia, amely a fürt úgy van beállítva, a megfelelő.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-141">HDInsight clusters come in various types, which correspond to the workload or technology that the cluster is tuned for.</span></span> <span data-ttu-id="e7b6b-142">Nincs támogatott módszer, amely többféle, például a Storm és HBase egy fürtön fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-142">There is no supported method to create a cluster that combines multiple types, such as Storm and HBase on one cluster.</span></span>

    * <span data-ttu-id="e7b6b-143">Cserélje le `location` az előző lépésben használt ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-143">Replace `location` with the same location used in previous steps.</span></span>

    * <span data-ttu-id="e7b6b-144">Cserélje le `storagename` fiók nevével.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-144">Replace `storagename` with the storage account name.</span></span>

    * <span data-ttu-id="e7b6b-145">Cserélje le `storagekey` az előző lépésben kapott a kulccsal.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-145">Replace `storagekey` with the key obtained in the previous step.</span></span>

    * <span data-ttu-id="e7b6b-146">Az a `--defaultStorageContainer` paraméter, a neve megegyezik, akkor a fürt használ.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-146">For the `--defaultStorageContainer` parameter, use the same name as you are using for the cluster.</span></span>

    * <span data-ttu-id="e7b6b-147">Cserélje le `admin` és `httppassword` a nevét és a jelszavát szeretné használni, ha HTTPS keresztül fér hozzá a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-147">Replace `admin` and `httppassword` with the name and password you wish to use when accessing the cluster through HTTPS.</span></span>

    * <span data-ttu-id="e7b6b-148">Cserélje le `sshuser` és `sshuserpassword` a felhasználónévvel és SSH használ a fürt eléréséhez használandó jelszót</span><span class="sxs-lookup"><span data-stu-id="e7b6b-148">Replace `sshuser` and `sshuserpassword` with the username and password you wish to use when accessing the cluster using SSH</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="e7b6b-149">Ebben a példában a fürt két munkavégző megjegyzéseket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-149">This example creates a cluster with two worker notes.</span></span> <span data-ttu-id="e7b6b-150">Skálázási műveletek elvégzésével fürt létrehozása után is módosíthatja a feldolgozó csomópontok száma.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-150">You can also change the number of worker nodes after cluster creation by performing scaling operations.</span></span> <span data-ttu-id="e7b6b-151">Ha több mint 32 munkavégző csomópontokhoz használatát tervezi, majd ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-151">If you plan on using more than 32 worker nodes, then you must select a head node size with at least 8 cores and 14-GB RAM.</span></span> <span data-ttu-id="e7b6b-152">Beállíthatja az átjárócsomópont méret használatával a `--headNodeSize` paraméter fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-152">You can set the head node size by using the `--headNodeSize` parameter during cluster creation.</span></span>
    >
    > <span data-ttu-id="e7b6b-153">A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e7b6b-153">For more information on node sizes and associated costs, see [HDInsight pricing](https://azure.microsoft.com/pricing/details/hdinsight/).</span></span>

    <span data-ttu-id="e7b6b-154">A Fürtlétrehozási folyamat befejezéséhez több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-154">It may take several minutes for the cluster creation process to finish.</span></span> <span data-ttu-id="e7b6b-155">Általában körülbelül 15.</span><span class="sxs-lookup"><span data-stu-id="e7b6b-155">Usually around 15.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="e7b6b-156">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e7b6b-156">Troubleshoot</span></span>

<span data-ttu-id="e7b6b-157">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="e7b6b-157">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e7b6b-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e7b6b-158">Next steps</span></span>

<span data-ttu-id="e7b6b-159">Most, hogy sikeresen létrehozta az Azure parancssori felület használatával HDInsight-fürtöt, használja a következő áttekintésével megismerheti, hogyan használható a fürt:</span><span class="sxs-lookup"><span data-stu-id="e7b6b-159">Now that you have successfully created an HDInsight cluster using the Azure CLI, use the following to learn how to work with your cluster:</span></span>

### <a name="hadoop-clusters"></a><span data-ttu-id="e7b6b-160">Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="e7b6b-160">Hadoop clusters</span></span>

* [<span data-ttu-id="e7b6b-161">A Hive használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e7b6b-161">Use Hive with HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="e7b6b-162">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e7b6b-162">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="e7b6b-163">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="e7b6b-163">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a><span data-ttu-id="e7b6b-164">HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="e7b6b-164">HBase clusters</span></span>

* [<span data-ttu-id="e7b6b-165">Az a HDInsight HBase első lépései</span><span class="sxs-lookup"><span data-stu-id="e7b6b-165">Get started with HBase on HDInsight</span></span>](hdinsight-hbase-tutorial-get-started-linux.md)
* [<span data-ttu-id="e7b6b-166">A HDInsight HBase Java-alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="e7b6b-166">Develop Java applications for HBase on HDInsight</span></span>](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a><span data-ttu-id="e7b6b-167">Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="e7b6b-167">Storm clusters</span></span>

* [<span data-ttu-id="e7b6b-168">Java-topológiák fejlesztése hdinsight alatt futó Storm</span><span class="sxs-lookup"><span data-stu-id="e7b6b-168">Develop Java topologies for Storm on HDInsight</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="e7b6b-169">A HDInsight alatt futó Storm Python-összetevőket használnak</span><span class="sxs-lookup"><span data-stu-id="e7b6b-169">Use Python components in Storm on HDInsight</span></span>](hdinsight-storm-develop-python-topology.md)
* [<span data-ttu-id="e7b6b-170">Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák</span><span class="sxs-lookup"><span data-stu-id="e7b6b-170">Deploy and monitor topologies with Storm on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology-linux.md)
