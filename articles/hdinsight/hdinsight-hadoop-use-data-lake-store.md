---
title: Data Lake Store az Azure HDInsight Hadoop aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan tooquery adatokat az Azure Data Lake Store és toostore eredmények elemzéshez."
keywords: "blob storage,hdfs,strukturált adatok,strukturálatlan adatok, data lake store"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: jgao
ms.openlocfilehash: 89633218a37a2fe05043e05d61199dcc0252d7f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-data-lake-store-with-azure-hdinsight-clusters"></a><span data-ttu-id="f8aac-104">A Data Lake Store és az Azure HDInsight-fürtök együttes használata</span><span class="sxs-lookup"><span data-stu-id="f8aac-104">Use Data Lake Store with Azure HDInsight clusters</span></span>

<span data-ttu-id="f8aac-105">HDInsight-fürt tooanalyze adatokat, hello adatait tárolhatja vagy a [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="f8aac-105">tooanalyze data in HDInsight cluster, you can store hello data either in [Azure Storage](../storage/common/storage-introduction.md), [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md), or both.</span></span> <span data-ttu-id="f8aac-106">Mindkét tárolási lehetőségek lehetővé teszik a HDInsight-fürtök toosafely törlése, felhasználói adatok elvesztése nélkül számításhoz használt.</span><span class="sxs-lookup"><span data-stu-id="f8aac-106">Both storage options enable you toosafely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="f8aac-107">Ebből a cikkből megtudhatja, hogyan használható a Data Lake Store a HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="f8aac-107">In this article, you learn how Data Lake Store works with HDInsight clusters.</span></span> <span data-ttu-id="f8aac-108">toolearn Azure Storage HDInsight-fürtökkel működése lásd [használata Azure Storage Azure hdinsight-fürtök](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="f8aac-108">toolearn how Azure Storage works with HDInsight clusters, see [Use Azure Storage with Azure HDInsight clusters](hdinsight-hadoop-use-blob-storage.md).</span></span> <span data-ttu-id="f8aac-109">További információ a HDInsight-fürtök létrehozásáról: [Hadoop-fürtök létrehozása a HDInsightban](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="f8aac-109">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f8aac-110">A Data Lake Store elérése mindig biztonságos csatornán keresztül történik, így nincs `adls` fájlrendszer-sémanév.</span><span class="sxs-lookup"><span data-stu-id="f8aac-110">Data Lake Store is always accessed through a secure channel, so there is no `adls` filesystem scheme name.</span></span> <span data-ttu-id="f8aac-111">Mindig `adl` használandó.</span><span class="sxs-lookup"><span data-stu-id="f8aac-111">You always use `adl`.</span></span>
> 
> 

## <a name="availabilities-for-hdinsight-clusters"></a><span data-ttu-id="f8aac-112">Lehetőségek HDInsight-fürtök esetén</span><span class="sxs-lookup"><span data-stu-id="f8aac-112">Availabilities for HDInsight clusters</span></span>

<span data-ttu-id="f8aac-113">A Hadoop hello alapértelmezett fájlrendszer támogatja.</span><span class="sxs-lookup"><span data-stu-id="f8aac-113">Hadoop supports a notion of hello default file system.</span></span> <span data-ttu-id="f8aac-114">hello alapértelmezett fájlrendszer egy alapértelmezett sémát és szolgáltatót jelenti.</span><span class="sxs-lookup"><span data-stu-id="f8aac-114">hello default file system implies a default scheme and authority.</span></span> <span data-ttu-id="f8aac-115">Használt tooresolve relatív elérési utakat is lehet.</span><span class="sxs-lookup"><span data-stu-id="f8aac-115">It can also be used tooresolve relative paths.</span></span> <span data-ttu-id="f8aac-116">Hello HDInsight fürt létrehozási folyamata során az Azure Storage hello alapértelmezett fájlrendszer egy blob-tároló adhat meg, vagy a HDInsight 3.5-ös és újabb verziók, válassza vagy az Azure Storage, vagy az Azure Data Lake Store rendszerként hello alapértelmezett fájlok és a Néhány kivételtől eltekintve.</span><span class="sxs-lookup"><span data-stu-id="f8aac-116">During hello HDInsight cluster creation process, you can specify a blob container in Azure Storage as hello default file system, or with HDInsight 3.5 and newer versions, you can select either Azure Storage or Azure Data Lake Store as hello default files system with a few exceptions.</span></span> 

<span data-ttu-id="f8aac-117">A HDInsight-fürtök kétféleképpen használhatják a Data Lake Store-t:</span><span class="sxs-lookup"><span data-stu-id="f8aac-117">HDInsight clusters can use Data Lake Store in two ways:</span></span>

* <span data-ttu-id="f8aac-118">Hello alapértelmezett tárolóként</span><span class="sxs-lookup"><span data-stu-id="f8aac-118">As hello default storage</span></span>
* <span data-ttu-id="f8aac-119">Kiegészítő tárolóként, ahol az Azure Storage Blob az alapértelmezett tároló.</span><span class="sxs-lookup"><span data-stu-id="f8aac-119">As additional storage, with Azure Storage Blob as default storage.</span></span>

<span data-ttu-id="f8aac-120">Mostantól típusok/verziók támogatás használatával a Data Lake Store alapértelmezett tároló és a további tárfiókok csak néhány hello HDInsight fürt:</span><span class="sxs-lookup"><span data-stu-id="f8aac-120">As of now, only some of hello HDInsight cluster types/versions support using Data Lake Store as default storage and additional storage accounts:</span></span>

| <span data-ttu-id="f8aac-121">A HDInsight-fürt típusa</span><span class="sxs-lookup"><span data-stu-id="f8aac-121">HDInsight cluster type</span></span> | <span data-ttu-id="f8aac-122">A Data Lake Store az alapértelmezett tároló</span><span class="sxs-lookup"><span data-stu-id="f8aac-122">Data Lake Store as default storage</span></span> | <span data-ttu-id="f8aac-123">A Data Lake Store kiegészítő tároló</span><span class="sxs-lookup"><span data-stu-id="f8aac-123">Data Lake Store as additional storage</span></span>| <span data-ttu-id="f8aac-124">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f8aac-124">Notes</span></span> |
|------------------------|------------------------------------|---------------------------------------|------|
| <span data-ttu-id="f8aac-125">A HDInsight 3.6-os verziója</span><span class="sxs-lookup"><span data-stu-id="f8aac-125">HDInsight version 3.6</span></span> | <span data-ttu-id="f8aac-126">Igen</span><span class="sxs-lookup"><span data-stu-id="f8aac-126">Yes</span></span> | <span data-ttu-id="f8aac-127">Igen</span><span class="sxs-lookup"><span data-stu-id="f8aac-127">Yes</span></span> | |
| <span data-ttu-id="f8aac-128">A HDInsight 3.5-ös verziója</span><span class="sxs-lookup"><span data-stu-id="f8aac-128">HDInsight version 3.5</span></span> | <span data-ttu-id="f8aac-129">Igen</span><span class="sxs-lookup"><span data-stu-id="f8aac-129">Yes</span></span> | <span data-ttu-id="f8aac-130">Igen</span><span class="sxs-lookup"><span data-stu-id="f8aac-130">Yes</span></span> | <span data-ttu-id="f8aac-131">A HBase hello kivétellel</span><span class="sxs-lookup"><span data-stu-id="f8aac-131">With hello exception of HBase</span></span>|
| <span data-ttu-id="f8aac-132">A HDInsight 3.4-es verziója</span><span class="sxs-lookup"><span data-stu-id="f8aac-132">HDInsight version 3.4</span></span> | <span data-ttu-id="f8aac-133">Nem</span><span class="sxs-lookup"><span data-stu-id="f8aac-133">No</span></span> | <span data-ttu-id="f8aac-134">Igen</span><span class="sxs-lookup"><span data-stu-id="f8aac-134">Yes</span></span> | |
| <span data-ttu-id="f8aac-135">A HDInsight 3.3-as verziója</span><span class="sxs-lookup"><span data-stu-id="f8aac-135">HDInsight version 3.3</span></span> | <span data-ttu-id="f8aac-136">Nem</span><span class="sxs-lookup"><span data-stu-id="f8aac-136">No</span></span> | <span data-ttu-id="f8aac-137">Nem</span><span class="sxs-lookup"><span data-stu-id="f8aac-137">No</span></span> | |
| <span data-ttu-id="f8aac-138">A HDInsight 3.2-es verziója</span><span class="sxs-lookup"><span data-stu-id="f8aac-138">HDInsight version 3.2</span></span> | <span data-ttu-id="f8aac-139">Nem</span><span class="sxs-lookup"><span data-stu-id="f8aac-139">No</span></span> | <span data-ttu-id="f8aac-140">Igen</span><span class="sxs-lookup"><span data-stu-id="f8aac-140">Yes</span></span> | |
| <span data-ttu-id="f8aac-141">Prémium (szintű) HDInsight</span><span class="sxs-lookup"><span data-stu-id="f8aac-141">HDInsight Premium (tier)</span></span>| <span data-ttu-id="f8aac-142">Nem</span><span class="sxs-lookup"><span data-stu-id="f8aac-142">No</span></span> | <span data-ttu-id="f8aac-143">Nem</span><span class="sxs-lookup"><span data-stu-id="f8aac-143">No</span></span> | |
| <span data-ttu-id="f8aac-144">Storm</span><span class="sxs-lookup"><span data-stu-id="f8aac-144">Storm</span></span> | | |<span data-ttu-id="f8aac-145">A Storm-topológia a Data Lake Store toowrite adatokat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f8aac-145">You can use Data Lake Store toowrite data from a Storm topology.</span></span> <span data-ttu-id="f8aac-146">A Data Lake Store-ban tárolhatók referenciaadatok is, amelyek olvashatók lesznek egy Storm-topológiából.</span><span class="sxs-lookup"><span data-stu-id="f8aac-146">You can also use Data Lake Store for reference data that can then be read by a Storm topology.</span></span>|

<span data-ttu-id="f8aac-147">További tárhely fiókként használatával a Data Lake Store nem befolyásolhatja a teljesítményt vagy hello képességét tooread vagy írási tooAzure tárolási hello fürtből.</span><span class="sxs-lookup"><span data-stu-id="f8aac-147">Using Data Lake Store as an additional storage account does not affect performance or hello ability tooread or write tooAzure storage from hello cluster.</span></span>


## <a name="use-data-lake-store-as-default-storage"></a><span data-ttu-id="f8aac-148">A Data Lake Store használata az alapértelmezett tárolóként</span><span class="sxs-lookup"><span data-stu-id="f8aac-148">Use Data Lake Store as default storage</span></span>

<span data-ttu-id="f8aac-149">HDInsight a Data Lake Store alapértelmezett tárolóként telepítésekor hello fürt kapcsolatos fájlok találhatók a Data Lake Store hello a következő helyen:</span><span class="sxs-lookup"><span data-stu-id="f8aac-149">When HDInsight is deployed with Data Lake Store as default storage, hello cluster-related files are stored in Data Lake Store in hello following location:</span></span>

    adl://mydatalakestore/<cluster_root_path>/

<span data-ttu-id="f8aac-150">Ha `<cluster_root_path>` egy mappát hoz létre a Data Lake Store hello neve.</span><span class="sxs-lookup"><span data-stu-id="f8aac-150">where `<cluster_root_path>` is hello name of a folder you create in Data Lake Store.</span></span> <span data-ttu-id="f8aac-151">Egy elérési útjának gyökeréhez használatos egyes fürtök megadásával használhatja ugyanazt a Data Lake Store fiókot egynél több fürt hello.</span><span class="sxs-lookup"><span data-stu-id="f8aac-151">By specifying a root path for each cluster, you can use hello same Data Lake Store account for more than one cluster.</span></span> <span data-ttu-id="f8aac-152">Így olyan beállítással rendelkezhet, ahol:</span><span class="sxs-lookup"><span data-stu-id="f8aac-152">So, you can have a setup where:</span></span>

* <span data-ttu-id="f8aac-153">Cluster1 használható hello elérési útja`adl://mydatalakestore/cluster1storage`</span><span class="sxs-lookup"><span data-stu-id="f8aac-153">Cluster1 can use hello path `adl://mydatalakestore/cluster1storage`</span></span>
* <span data-ttu-id="f8aac-154">Cluster2 használható hello elérési útja`adl://mydatalakestore/cluster2storage`</span><span class="sxs-lookup"><span data-stu-id="f8aac-154">Cluster2 can use hello path `adl://mydatalakestore/cluster2storage`</span></span>

<span data-ttu-id="f8aac-155">Figyelje meg, hogy mindkét hello fürtök használata hello azonos Data Lake Store-fiók **mydatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="f8aac-155">Notice that both hello clusters use hello same Data Lake Store account **mydatalakestore**.</span></span> <span data-ttu-id="f8aac-156">Az egyes fürtökön saját Data Lake Store a legfelső szintű fájlrendszer hozzáférés tooits rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f8aac-156">Each cluster has access tooits own root filesystem in Data Lake Store.</span></span> <span data-ttu-id="f8aac-157">hello Azure portál üzembe helyezését különösen kéri toouse mappanevet például **/clusters/\<clustername >** hello elérési útjának gyökeréhez tartozó.</span><span class="sxs-lookup"><span data-stu-id="f8aac-157">hello Azure portal deployment experience in particular prompts you toouse a folder name such as **/clusters/\<clustername>** for hello root path.</span></span>

<span data-ttu-id="f8aac-158">toobe képes toouse egy Data Lake Store alapértelmezett tárolóként, engedélyt kell adni a következő elérési utak hello szolgáltatás egyszerű hozzáférés toohello:</span><span class="sxs-lookup"><span data-stu-id="f8aac-158">toobe able toouse a Data Lake Store as default storage, you must grant hello service principal access toohello following paths:</span></span>

- <span data-ttu-id="f8aac-159">hello Data Lake Store-fiók gyökere.</span><span class="sxs-lookup"><span data-stu-id="f8aac-159">hello Data Lake Store account root.</span></span>  <span data-ttu-id="f8aac-160">Példa: adl://mydatalakestore/.</span><span class="sxs-lookup"><span data-stu-id="f8aac-160">For example: adl://mydatalakestore/.</span></span>
- <span data-ttu-id="f8aac-161">hello mappa a fürt összes mappa.</span><span class="sxs-lookup"><span data-stu-id="f8aac-161">hello folder for all cluster folders.</span></span>  <span data-ttu-id="f8aac-162">Példa: adl://mydatalakestore/clusters.</span><span class="sxs-lookup"><span data-stu-id="f8aac-162">For example: adl://mydatalakestore/clusters.</span></span>
- <span data-ttu-id="f8aac-163">hello mappa hello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f8aac-163">hello folder for hello cluster.</span></span>  <span data-ttu-id="f8aac-164">Példa: adl://mydatalakestore/clusters/cluster1storage.</span><span class="sxs-lookup"><span data-stu-id="f8aac-164">For example: adl://mydatalakestore/clusters/cluster1storage.</span></span>

<span data-ttu-id="f8aac-165">A szolgáltatásnevek létrehozásával és a hozzáférés biztosításával kapcsolatos további információkért lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="f8aac-165">For more information for creating service principal and grant access, see [Configure Data Lake store access](#configure-data-lake-store-access).</span></span>


## <a name="use-data-lake-store-as-additional-storage"></a><span data-ttu-id="f8aac-166">A Data Lake Store használata kiegészítő tárolóként</span><span class="sxs-lookup"><span data-stu-id="f8aac-166">Use Data Lake Store as additional storage</span></span>

<span data-ttu-id="f8aac-167">Data Lake Store további tárolóként hello fürt is használja.</span><span class="sxs-lookup"><span data-stu-id="f8aac-167">You can use Data Lake Store as additional storage for hello cluster as well.</span></span> <span data-ttu-id="f8aac-168">Ilyen esetekben hello alapértelmezett fürttároló lehet egy Azure Storage-Blobba vagy egy Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="f8aac-168">In such cases, hello cluster default storage can either be an Azure Storage Blob or a Data Lake Store account.</span></span> <span data-ttu-id="f8aac-169">Ha futtatja a HDInsight-feladatot a Data Lake Store további tárolóként hello adataihoz ellen, hello teljes elérési útja toohello fájlokat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="f8aac-169">If you are running HDInsight jobs against hello data stored in Data Lake Store as additional storage, you must use hello fully-qualified path toohello files.</span></span> <span data-ttu-id="f8aac-170">Példa:</span><span class="sxs-lookup"><span data-stu-id="f8aac-170">For example:</span></span>

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

<span data-ttu-id="f8aac-171">Vegye figyelembe, hogy nincs nincs **cluster_root_path** most hello URL-címben.</span><span class="sxs-lookup"><span data-stu-id="f8aac-171">Note that there's no **cluster_root_path** in hello URL now.</span></span> <span data-ttu-id="f8aac-172">Amely az oka Data Lake Store nem alapértelmezett tárolási ebben az esetben, toodo szüksége hello elérési toohello fájlokkal.</span><span class="sxs-lookup"><span data-stu-id="f8aac-172">That's because Data Lake Store is not a default storage in this case so all you need toodo is provide hello path toohello files.</span></span>

<span data-ttu-id="f8aac-173">toobe képes toouse egy Data Lake Store további tárolóként, csak akkor kell toogrant hello szolgáltatás egyszerű hozzáférés toohello elérési utak hol tárolja a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="f8aac-173">toobe able toouse a Data Lake Store as additional storage, you only need toogrant hello service principal access toohello paths where your files are stored.</span></span>  <span data-ttu-id="f8aac-174">Példa:</span><span class="sxs-lookup"><span data-stu-id="f8aac-174">For example:</span></span>

    adl://mydatalakestore.azuredatalakestore.net/<file_path>

<span data-ttu-id="f8aac-175">A szolgáltatásnevek létrehozásával és a hozzáférés biztosításával kapcsolatos további információkért lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="f8aac-175">For more information for creating service principal and grant access, see [Configure Data Lake store access](#configure-data-lake-store-access).</span></span>


## <a name="use-more-than-one-data-lake-store-accounts"></a><span data-ttu-id="f8aac-176">Több Data Lake Store-fiók használata</span><span class="sxs-lookup"><span data-stu-id="f8aac-176">Use more than one Data Lake Store accounts</span></span>

<span data-ttu-id="f8aac-177">További Data Lake Store-fiók hozzáadása, és több mint egy Data Lake Store hozzáadása fiókok kivitelezése hello HDInsight fürt engedélyt adjon egy vagy több Data Lake Store-fiók lévő adatokon.</span><span class="sxs-lookup"><span data-stu-id="f8aac-177">Adding a Data Lake Store account as additional and adding more than one Data Lake Store accounts are accomplished by giving hello HDInsight cluster permission on data in one ore more Data Lake Store accounts.</span></span> <span data-ttu-id="f8aac-178">Lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](#configure-data-lake-store-access).</span><span class="sxs-lookup"><span data-stu-id="f8aac-178">See [Configure Data Lake Store access](#configure-data-lake-store-access).</span></span>

## <a name="configure-data-lake-store-access"></a><span data-ttu-id="f8aac-179">A Data Lake Store-hoz történő hozzáférés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8aac-179">Configure Data Lake store access</span></span>

<span data-ttu-id="f8aac-180">tooconfigure Data Lake store hozzáférést a HDInsight-fürtjéhez, rendelkeznie kell egy Azure Active directory (Azure AD) szolgáltatás egyszerű.</span><span class="sxs-lookup"><span data-stu-id="f8aac-180">tooconfigure Data Lake store access from your HDInsight cluster, you must have an Azure Active directory (Azure AD) service principal.</span></span> <span data-ttu-id="f8aac-181">Kizárólag Azure AD-rendszergazdák hozhatnak létre szolgáltatásnevet.</span><span class="sxs-lookup"><span data-stu-id="f8aac-181">Only an Azure AD administrator can create a service principal.</span></span> <span data-ttu-id="f8aac-182">hello szolgáltatás egyszerű tanúsítvánnyal kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f8aac-182">hello service principal must be created with a certificate.</span></span> <span data-ttu-id="f8aac-183">További információkért lásd: [A Data Lake Store-hoz történő hozzáférés konfigurálása](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access), és [Szolgáltatásnév létrehozása önaláírt tanúsítvánnyal](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).</span><span class="sxs-lookup"><span data-stu-id="f8aac-183">For more information, see [Configure Data Lake Store access](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md#configure-data-lake-store-access), and [Create service principal with self-signed-certificate](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).</span></span>

> [!NOTE]
> <span data-ttu-id="f8aac-184">Ha azt tervezi, hogy toouse Azure Data Lake Store további tárolóként HDInsight-fürthöz, javasoljuk, hogy ehhez hello fürt létrehozásakor, a cikkben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="f8aac-184">If you are going toouse Azure Data Lake Store as additional storage for HDInsight cluster, we strongly recommend that you do this while you create hello cluster as described in this article.</span></span> <span data-ttu-id="f8aac-185">Azure Data Lake Store további tárhely tooan hozzáadása meglévő HDInsight-fürt egy összetett folyamat és hibalehetőségeket rejt magában tooerrors.</span><span class="sxs-lookup"><span data-stu-id="f8aac-185">Adding Azure Data Lake Store as additional storage tooan existing HDInsight cluster is a complicated process and prone tooerrors.</span></span>
>

## <a name="access-files-from-hello-cluster"></a><span data-ttu-id="f8aac-186">Fájlok elérése hello fürtből</span><span class="sxs-lookup"><span data-stu-id="f8aac-186">Access files from hello cluster</span></span>

<span data-ttu-id="f8aac-187">Számos módon el tudja érni a Data Lake Store hello fájlokat a HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="f8aac-187">There are several ways you can access hello files in Data Lake Store from an HDInsight cluster.</span></span>

* <span data-ttu-id="f8aac-188">**Hello teljesen minősített nevet használó**.</span><span class="sxs-lookup"><span data-stu-id="f8aac-188">**Using hello fully qualified name**.</span></span> <span data-ttu-id="f8aac-189">Hello teljes elérési útja toohello fájl megadta ezt a módszert, amelyet az tooaccess.</span><span class="sxs-lookup"><span data-stu-id="f8aac-189">With this approach, you provide hello full path toohello file that you want tooaccess.</span></span>

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/<file_path>

* <span data-ttu-id="f8aac-190">**Hello rövidebb elérési utat formátumban**.</span><span class="sxs-lookup"><span data-stu-id="f8aac-190">**Using hello shortened path format**.</span></span> <span data-ttu-id="f8aac-191">Ezt a módszert használja, akkor cserélje le hello elérési út mentése toohello fürt legfelső szintű adl: / / /.</span><span class="sxs-lookup"><span data-stu-id="f8aac-191">With this approach, you replace hello path up toohello cluster root with adl:///.</span></span> <span data-ttu-id="f8aac-192">Igen, a hello a fenti példában helyettesíthetők `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` rendelkező `adl:///`.</span><span class="sxs-lookup"><span data-stu-id="f8aac-192">So, in hello example above, you can replace `adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/` with `adl:///`.</span></span>

        adl:///<file path>

* <span data-ttu-id="f8aac-193">**Hello relatív elérési út használatával**.</span><span class="sxs-lookup"><span data-stu-id="f8aac-193">**Using hello relative path**.</span></span> <span data-ttu-id="f8aac-194">Ezt a módszert csak olyan hello relatív elérési út toohello fájl, amelyet az tooaccess.</span><span class="sxs-lookup"><span data-stu-id="f8aac-194">With this approach, you only provide hello relative path toohello file that you want tooaccess.</span></span> <span data-ttu-id="f8aac-195">Például ha hello toohello fájl teljes elérési útja a következő:</span><span class="sxs-lookup"><span data-stu-id="f8aac-195">For example, if hello complete path toohello file is:</span></span>

        adl://mydatalakestore.azuredatalakestore.net/<cluster_root_path>/example/data/sample.log

    <span data-ttu-id="f8aac-196">Van-e hozzáférési hello azonos sample.log fájl helyett a relatív elérési út használatával.</span><span class="sxs-lookup"><span data-stu-id="f8aac-196">You can access hello same sample.log file by using this relative path instead.</span></span>

        /example/data/sample.log

## <a name="create-hdinsight-clusters-with-access-toodata-lake-store"></a><span data-ttu-id="f8aac-197">A HDInsight-fürtök létrehozása a lépjen be az tooData Lake Store</span><span class="sxs-lookup"><span data-stu-id="f8aac-197">Create HDInsight clusters with access tooData Lake Store</span></span>

<span data-ttu-id="f8aac-198">Használja a következő hivatkozások részletes információkra van szüksége a hogyan férhetnek hozzá a HDInsight-fürtök az toocreate tooData Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="f8aac-198">Use hello following links for detailed instructions on how toocreate HDInsight clusters with access tooData Lake Store.</span></span>

* [<span data-ttu-id="f8aac-199">A Portal használata</span><span class="sxs-lookup"><span data-stu-id="f8aac-199">Using Portal</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
* [<span data-ttu-id="f8aac-200">A PowerShell használata (alapértelmezett tárolóként beállított Data Lake Store-ral)</span><span class="sxs-lookup"><span data-stu-id="f8aac-200">Using PowerShell (with Data Lake Store as default storage)</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [<span data-ttu-id="f8aac-201">A PowerShell használata (kiegészítő tárolóként beállított Data Lake Store-ral)</span><span class="sxs-lookup"><span data-stu-id="f8aac-201">Using PowerShell (with Data Lake Store as additional storage)</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md)
* [<span data-ttu-id="f8aac-202">Azure-sablonok használata</span><span class="sxs-lookup"><span data-stu-id="f8aac-202">Using Azure templates</span></span>](../data-lake-store/data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)


## <a name="next-steps"></a><span data-ttu-id="f8aac-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8aac-203">Next steps</span></span>
<span data-ttu-id="f8aac-204">Ebben a cikkben megtanulta, hogyan meg toouse HDFS-kompatibilis Azure Data Lake Store a hdinsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="f8aac-204">In this article, you learned how toouse HDFS-compatible Azure Data Lake Store with HDInsight.</span></span> <span data-ttu-id="f8aac-205">Ez lehetővé teszi, hogy toobuild méretezhető, hosszú távú, archiválás beszerzési megoldások kiépítését és -felhasználási HDInsight toounlock hello lévő információkat hello tárolt strukturált és strukturálatlan adatokat.</span><span class="sxs-lookup"><span data-stu-id="f8aac-205">This allows you toobuild scalable, long-term, archiving data acquisition solutions and use HDInsight toounlock hello information inside hello stored structured and unstructured data.</span></span>

<span data-ttu-id="f8aac-206">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="f8aac-206">For more information, see:</span></span>

* <span data-ttu-id="f8aac-207">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="f8aac-207">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="f8aac-208">Az Azure Data Lake Store használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="f8aac-208">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="f8aac-209">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="f8aac-209">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="f8aac-210">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="f8aac-210">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="f8aac-211">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="f8aac-211">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="f8aac-212">[Azure Storage megosztott hozzáférési aláírásokkal toorestrict hozzáférés toodata használata a hdinsight eszközzel][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="f8aac-212">[Use Azure Storage Shared Access Signatures toorestrict access toodata with HDInsight][hdinsight-use-sas]</span></span>

[hdinsight-use-sas]: hdinsight-storage-sharedaccesssignature-permissions.md
[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-creation]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]:../storage/common/storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  
