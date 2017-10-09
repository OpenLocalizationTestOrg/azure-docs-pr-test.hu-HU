---
title: "storage-fiókok biztonságos átvitele az Azure HDInsight-fürtöt aaaCreate Hadoop |} Microsoft Docs"
description: "Ismerje meg, hogyan engedélyezve van a HDInsight-fürtök toocreate biztonságos átvitele az Azure storage-fiókok."
keywords: "hadoop első lépései,hadoop linux,hadoop gyorsútmutató,biztonságos átvitel,azure-tárfiók"
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: jgao
ms.openlocfilehash: 0acb8814ad0d5d5b5652d930b2e3da90f9d7978d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a><span data-ttu-id="24756-104">Biztonságos átvitelű tárfiókokkal rendelkező Hadoop-fürt létrehozása az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="24756-104">Create Hadoop cluster with secure transfer storage accounts in Azure HDInsight</span></span>

<span data-ttu-id="24756-105">Hello [szükséges átviteli biztonságos](../storage/common/storage-require-secure-transfer.md) a szolgáltatás továbbfejleszti hello biztonsági az Azure Storage-fiókjának tartat be az összes kérelem tooyour fiók biztonságos kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="24756-105">hello [Secure transfer required](../storage/common/storage-require-secure-transfer.md) feature enhances hello security of your Azure Storage account by enforcing all requests tooyour account through a secure connection.</span></span> <span data-ttu-id="24756-106">Ez a szolgáltatás és hello wasbs séma csak támogatja HDInsight-fürt verziószáma 3,6 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="24756-106">This feature and hello wasbs scheme are only supported by HDInsight cluster version 3.6 or newer.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="24756-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="24756-107">Prerequisites</span></span>
<span data-ttu-id="24756-108">Az oktatóanyag elindításának feltétele:</span><span class="sxs-lookup"><span data-stu-id="24756-108">Before you begin this tutorial, you must have:</span></span>

* <span data-ttu-id="24756-109">**Azure-előfizetés**: egy ingyenes egy hónapos próbafiók, toocreate Tallózás túl[azure.microsoft.com/free](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="24756-109">**Azure subscription**: toocreate a free one-month trial account, browse too[azure.microsoft.com/free](https://azure.microsoft.com/free).</span></span>
* <span data-ttu-id="24756-110">**Engedélyezett biztonságos átvitellel rendelkező Azure-tárfiók**</span><span class="sxs-lookup"><span data-stu-id="24756-110">**An Azure Storage account with secure transfer enabled**.</span></span> <span data-ttu-id="24756-111">Hello útmutatásért lásd: [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) és [biztonságos átvitelét igénylő](../storage/common/storage-require-secure-transfer.md).</span><span class="sxs-lookup"><span data-stu-id="24756-111">For hello instructions, see [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) and [Require secure transfer](../storage/common/storage-require-secure-transfer.md).</span></span>
* <span data-ttu-id="24756-112">**A Blob-tároló hello tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="24756-112">**A Blob container on hello storage account**.</span></span> 
## <a name="create-cluster"></a><span data-ttu-id="24756-113">Fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="24756-113">Create cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


<span data-ttu-id="24756-114">Ebben a szakaszban egy Hadoop-fürtöt hozhat létre a HDInsightban egy [Azure Resource Manager-sablonnal](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="24756-114">In this section, you create a Hadoop cluster in HDInsight using an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="24756-115">hello sablon található [Github](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/).</span><span class="sxs-lookup"><span data-stu-id="24756-115">hello template is located in [Gibhub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/).</span></span> <span data-ttu-id="24756-116">Nem kell a Resource Manager-sablonok használatára vonatkozó tapasztalattal rendelkeznie az oktatóanyag követéséhez.</span><span class="sxs-lookup"><span data-stu-id="24756-116">Resource Manager template experience is not required for following this tutorial.</span></span> <span data-ttu-id="24756-117">Egyéb Fürtlétrehozási módszerekhez és ebben az oktatóanyagban használt hello tulajdonságok ismertetése, tanulmányozza a [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="24756-117">For other cluster creation methods and understanding hello properties used in this tutorial, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

1. <span data-ttu-id="24756-118">Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="24756-118">Click hello following image toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span> 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="24756-119">Hello utasításokat toocreate hello fürt kövesse a hello előírásainak megfelelően:</span><span class="sxs-lookup"><span data-stu-id="24756-119">Follow hello instructions toocreate hello cluster with hello following specifications:</span></span> 

    - <span data-ttu-id="24756-120">Adja meg a HDInsight 3.6-os verzióját.</span><span class="sxs-lookup"><span data-stu-id="24756-120">Specify HDInsight version 3.6.</span></span>  <span data-ttu-id="24756-121">hello alapértelmezett verziója 3.5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="24756-121">hello default version is 3.5.</span></span> <span data-ttu-id="24756-122">A 3.6-os vagy újabb verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="24756-122">Version 3.6 or newer is required.</span></span>
    - <span data-ttu-id="24756-123">Adjon meg egy biztonságos átvitel használatára képes tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="24756-123">Specify a secure transfer enabled storage account.</span></span>
    - <span data-ttu-id="24756-124">Hello tárfiók rövid nevét használja.</span><span class="sxs-lookup"><span data-stu-id="24756-124">Use short name for hello storage account.</span></span>
    - <span data-ttu-id="24756-125">Hello tárfiók és a blob-tároló hello előre kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="24756-125">Both hello storage account and hello blob container must be created beforehand.</span></span> 

    <span data-ttu-id="24756-126">Hello útmutatásért lásd: [fürt létrehozása](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="24756-126">For hello instructions, see [Create cluster](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span> 

<span data-ttu-id="24756-127">Parancsfájl művelet tooprovide saját konfigurációs fájlok használatára, ha a következő beállítások hello wasbs kell használnia:</span><span class="sxs-lookup"><span data-stu-id="24756-127">If you use script action tooprovide your own configuration files, you must use wasbs in hello following settings:</span></span>

- <span data-ttu-id="24756-128">fs.defaultFS (core-site)</span><span class="sxs-lookup"><span data-stu-id="24756-128">fs.defaultFS (core-site)</span></span>
- <span data-ttu-id="24756-129">spark.eventLog.dir</span><span class="sxs-lookup"><span data-stu-id="24756-129">spark.eventLog.dir</span></span> 
- <span data-ttu-id="24756-130">spark.history.fs.logDirectory</span><span class="sxs-lookup"><span data-stu-id="24756-130">spark.history.fs.logDirectory</span></span>

## <a name="add-additional-storage-accounts"></a><span data-ttu-id="24756-131">További tárfiókok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="24756-131">Add additional storage accounts</span></span>

<span data-ttu-id="24756-132">Van több beállítások tooadd engedélyezni biztonságos letöltési storage-fiókok:</span><span class="sxs-lookup"><span data-stu-id="24756-132">There are several options tooadd additional secure transfer enabled storage accounts:</span></span>

- <span data-ttu-id="24756-133">Hello utolsó szakaszában hello Azure Resource Manager sablon módosításához.</span><span class="sxs-lookup"><span data-stu-id="24756-133">Modify hello Azure Resource Manager template in hello last section.</span></span>
- <span data-ttu-id="24756-134">Hozzon létre egy fürtöt hello [Azure-portálon](https://portal.azure.com) , és adja meg a kapcsolt tárfiókra.</span><span class="sxs-lookup"><span data-stu-id="24756-134">Create a cluster using hello [Azure portal](https://portal.azure.com) and specify linked storage account.</span></span>
- <span data-ttu-id="24756-135">Engedélyezve használata parancsfájl művelet tooadd további biztonságos átviteli tárolási fiókok tooan meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="24756-135">Use script action tooadd additional secure transfer enabled storage accounts tooan existing HDInsight cluster.</span></span>  <span data-ttu-id="24756-136">További információkért lásd: [adja hozzá a további tárhely fiókok tooHDInsight](hdinsight-hadoop-add-storage.md).</span><span class="sxs-lookup"><span data-stu-id="24756-136">For more information, see [Add additional storage accounts tooHDInsight](hdinsight-hadoop-add-storage.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="24756-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24756-137">Next steps</span></span>
<span data-ttu-id="24756-138">Ebben az oktatóprogramban megtanulhatta, hogyan toocreate HDInsight-fürtöt, és biztonságos engedélyezése átvitele toohello storage-fiókok.</span><span class="sxs-lookup"><span data-stu-id="24756-138">In this tutorial, you have learned how toocreate an HDInsight cluster, and enable secure transfer toohello storage accounts.</span></span>

<span data-ttu-id="24756-139">További információk a HDInsight az adatok elemzése toolearn lásd: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="24756-139">toolearn more about analyzing data with HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="24756-140">További információk a hdinsight eszközzel, hogyan tooperform Hive lekérdezi a Visual Studio eszközből, beleértve a Hive eszközzel toolearn lásd [használata a HDInsight Hive][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="24756-140">toolearn more about using Hive with HDInsight, including how tooperform Hive queries from Visual Studio, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
* <span data-ttu-id="24756-141">Pig kapcsolatos toolearn, nyelv használt tootransform adatokat, lásd: [a Pig használata a hdinsight eszközzel][hdinsight-use-pig].</span><span class="sxs-lookup"><span data-stu-id="24756-141">toolearn about Pig, a language used tootransform data, see [Use Pig with HDInsight][hdinsight-use-pig].</span></span>
* <span data-ttu-id="24756-142">toolearn MapReduce, egy módszer toowrite, Hadoopon adatokat feldolgozó programok kapcsolatban lásd: [és a HDInsight együttes használata MapReduce][hdinsight-use-mapreduce].</span><span class="sxs-lookup"><span data-stu-id="24756-142">toolearn about MapReduce, a way toowrite programs that process data on Hadoop, see [Use MapReduce with HDInsight][hdinsight-use-mapreduce].</span></span>
* <span data-ttu-id="24756-143">toolearn hello HDInsight Tools for Visual Studio tooanalyze adatok, használatával kapcsolatban lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="24756-143">toolearn about using hello HDInsight Tools for Visual Studio tooanalyze data on HDInsight, see [Get started using Visual Studio Hadoop tools for HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

<span data-ttu-id="24756-144">További információk a HDInsight adattárolási módszereiről toolearn vagy hogyan tooget adatok HDInsight, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="24756-144">toolearn more about how HDInsight stores data or how tooget data into HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="24756-145">További információt az Azure Storage HDInsight általi használatáról [az Azure Storage és a HDInsight együttes használatát](hdinsight-hadoop-use-blob-storage.md) ismertető cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="24756-145">For information on how HDInsight uses Azure Storage, see [Use Azure Storage with HDInsight](hdinsight-hadoop-use-blob-storage.md).</span></span>
* <span data-ttu-id="24756-146">Információ tooupload adatok tooHDInsight, lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="24756-146">For information on how tooupload data tooHDInsight, see [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

<span data-ttu-id="24756-147">toolearn bővebben létrehozása vagy kezelése a HDInsight-fürtöt, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="24756-147">toolearn more about creating or managing an HDInsight cluster, see hello following articles:</span></span>

* <span data-ttu-id="24756-148">a Linux-alapú HDInsight-fürt kezeléséhez toolearn lásd: [kezelése HDInsight-fürtök Ambari használatával](hdinsight-hadoop-manage-ambari.md).</span><span class="sxs-lookup"><span data-stu-id="24756-148">toolearn about managing your Linux-based HDInsight cluster, see [Manage HDInsight clusters using Ambari](hdinsight-hadoop-manage-ambari.md).</span></span>
* <span data-ttu-id="24756-149">toolearn kiválasztható egy HDInsight-fürt létrehozásakor hello beállításokról bővebben lásd: [létrehozása HDInsight Linux egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="24756-149">toolearn more about hello options you can select when creating an HDInsight cluster, see [Creating HDInsight on Linux using custom options](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="24756-150">Ha ismeri a Linux és a Hadoop, de szeretné, hogy a hello HDInsight Hadoop kapcsolatos tooknow rögzítésen, lásd: [a HDInsight használata Linux](hdinsight-hadoop-linux-information.md).</span><span class="sxs-lookup"><span data-stu-id="24756-150">If you are familiar with Linux, and Hadoop, but want tooknow specifics about Hadoop on hello HDInsight, see [Working with HDInsight on Linux](hdinsight-hadoop-linux-information.md).</span></span> <span data-ttu-id="24756-151">Ez a cikk többek között az alábbi információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="24756-151">This article provides information such as:</span></span>
  
  * <span data-ttu-id="24756-152">Például az Ambari és a WebHCat hello fürtön tárolt szolgáltatások URL-címek</span><span class="sxs-lookup"><span data-stu-id="24756-152">URLs for services hosted on hello cluster, such as Ambari and WebHCat</span></span>
  * <span data-ttu-id="24756-153">a Hadoop-fájlok és példák a helyi fájlrendszerben hello hello helye</span><span class="sxs-lookup"><span data-stu-id="24756-153">hello location of Hadoop files and examples on hello local file system</span></span>
  * <span data-ttu-id="24756-154">hello alapértelmezett adattár hello használata az Azure Storage (WASB) HDFS helyett.</span><span class="sxs-lookup"><span data-stu-id="24756-154">hello use of Azure Storage (WASB) instead of HDFS as hello default data store</span></span>

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


