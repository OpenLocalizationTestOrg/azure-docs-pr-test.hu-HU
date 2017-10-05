---
title: "Adatok lekérdezése HDFS-kompatibilis Azure Storage-ból – Azure HDInsight | Microsoft Docs"
description: "Megtudhatja, hogyan kérdezhet le adatokat az Azure Blob Storage-ból és az Azure Data Lake Store-ból, és hogyan tárolhatja az elemzések eredményeit."
keywords: "blob storage,hdfs,strukturált adatok,strukturálatlan adatok, data lake store,Hadoop-bemenet,Hadoop-kimenet, hadoop-tárolás, hdfs-bemenet,hdfs-kimenet,hdfs-tárolás,wasb azure"
services: hdinsight,storage
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1d2e65f2-16de-449e-915f-3ffbc230f815
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/09/2017
ms.author: jgao
ms.openlocfilehash: a44c2b363f7ebb593b9a9c5bd9e0d4fc3b4c31bb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a><span data-ttu-id="7db29-104">Az Azure Storage és az Azure HDInsight-fürtök együttes használata</span><span class="sxs-lookup"><span data-stu-id="7db29-104">Use Azure storage with Azure HDInsight clusters</span></span>

<span data-ttu-id="7db29-105">A HDInsight-fürtben lévő adatok elemzéséhez az adatokat tárolhatja az Azure Storage-ban, az Azure Data Lake Store-ban vagy mindkettőben.</span><span class="sxs-lookup"><span data-stu-id="7db29-105">To analyze data in HDInsight cluster, you can store the data either in Azure Storage, Azure Data Lake Store, or both.</span></span> <span data-ttu-id="7db29-106">Mindkét tárolási lehetőség lehetővé teszi a számításhoz használt HDInsight-fürtök biztonságos törlését a felhasználói adatok elvesztése nélkül.</span><span class="sxs-lookup"><span data-stu-id="7db29-106">Both storage options enable you to safely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="7db29-107">A Hadoop támogatja az alapértelmezett fájlrendszert.</span><span class="sxs-lookup"><span data-stu-id="7db29-107">Hadoop supports a notion of the default file system.</span></span> <span data-ttu-id="7db29-108">Az alapértelmezett fájlrendszer egy alapértelmezett sémát és szolgáltatót is jelent.</span><span class="sxs-lookup"><span data-stu-id="7db29-108">The default file system implies a default scheme and authority.</span></span> <span data-ttu-id="7db29-109">A relatív elérési utak feloldásához is használható.</span><span class="sxs-lookup"><span data-stu-id="7db29-109">It can also be used to resolve relative paths.</span></span> <span data-ttu-id="7db29-110">A HDInsight-fürt létrehozása során blobtárolók adhatók meg alapértelmezett fájlrendszerként, illetve a HDInsight 3.5 esetében választhat, hogy az Azure Storage vagy az Azure Data Lake Store legyen az alapértelmezett fájlrendszer, néhány kivétellel.</span><span class="sxs-lookup"><span data-stu-id="7db29-110">During the HDInsight cluster creation process, you can specify a blob container in Azure Storage as the default file system, or with HDInsight 3.5, you can select either Azure Storage or Azure Data Lake Store as the default files system with a few exceptions.</span></span> <span data-ttu-id="7db29-111">A Data Lake Store alapértelmezett és kapcsolt tárként való egyidejű használatának támogatási lehetőségeiről a [Lehetőségek HDInsight-fürtök esetén](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters) című cikk ad tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="7db29-111">For the supportability of using Data Lake Store as both the default and linked storage, see [Availabilities for HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span></span>

<span data-ttu-id="7db29-112">Ebből a cikkből megtudhatja, hogyan használható az Azure Storage a HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="7db29-112">In this article, you learn how Azure Storage works with HDInsight clusters.</span></span> <span data-ttu-id="7db29-113">A Data Lake Store HDInsight-fürtökkel való használatáról az [Azure Data Lake Store használata Azure HDInsight-fürtökkel](hdinsight-hadoop-use-data-lake-store.md) című cikk szól.</span><span class="sxs-lookup"><span data-stu-id="7db29-113">To learn how Data Lake Store works with HDInsight clusters, see [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> <span data-ttu-id="7db29-114">További információ a HDInsight-fürtök létrehozásáról: [Hadoop-fürtök létrehozása a HDInsightban](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="7db29-114">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="7db29-115">Az Azure Blob Storage egy robusztus, általános célú tárolómegoldás, amely zökkenőmentesen integrálható a HDInsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="7db29-115">Azure storage is a robust, general-purpose storage solution that integrates seamlessly with HDInsight.</span></span> <span data-ttu-id="7db29-116">A HDInsight egy blobtárolót használhat az Azure Storage-ben a fürt alapértelmezett fájlrendszereként.</span><span class="sxs-lookup"><span data-stu-id="7db29-116">HDInsight can use a blob container in Azure Storage as the default file system for the cluster.</span></span> <span data-ttu-id="7db29-117">A Hadoop elosztott fájlrendszer (HDFS) felületen keresztül a HDInsight összetevők teljes készlete működhet közvetlenül a strukturált vagy strukturálatlan adatokon a Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7db29-117">Through a Hadoop distributed file system (HDFS) interface, the full set of components in HDInsight can operate directly on structured or unstructured data stored as blobs.</span></span>

> [!WARNING]
> <span data-ttu-id="7db29-118">Az Azure Storage-fiók létrehozása során számos beállítás áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="7db29-118">There are several options available when creating an Azure Storage account.</span></span> <span data-ttu-id="7db29-119">A következő táblázat információkat nyújt arról, hogy milyen beállítások támogatottak a HDInsight használatakor:</span><span class="sxs-lookup"><span data-stu-id="7db29-119">The following table provides information on what options are supported with HDInsight:</span></span>
> 
> | <span data-ttu-id="7db29-120">Tárfiók típusa</span><span class="sxs-lookup"><span data-stu-id="7db29-120">Storage account type</span></span> | <span data-ttu-id="7db29-121">Tárolási réteg</span><span class="sxs-lookup"><span data-stu-id="7db29-121">Storage tier</span></span> | <span data-ttu-id="7db29-122">Támogatott a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="7db29-122">Supported with HDInsight</span></span> |
> | ------- | ------- | ------- |
> | <span data-ttu-id="7db29-123">Általános célú tárfiókok</span><span class="sxs-lookup"><span data-stu-id="7db29-123">General-purpose Storage Account</span></span> | <span data-ttu-id="7db29-124">Standard</span><span class="sxs-lookup"><span data-stu-id="7db29-124">Standard</span></span> | <span data-ttu-id="7db29-125">__Igen__</span><span class="sxs-lookup"><span data-stu-id="7db29-125">__Yes__</span></span> |
> | &nbsp; | <span data-ttu-id="7db29-126">Prémium</span><span class="sxs-lookup"><span data-stu-id="7db29-126">Premium</span></span> | <span data-ttu-id="7db29-127">Nem</span><span class="sxs-lookup"><span data-stu-id="7db29-127">No</span></span> |
> | <span data-ttu-id="7db29-128">Blob Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="7db29-128">Blob Storage Account</span></span> | <span data-ttu-id="7db29-129">Gyakori</span><span class="sxs-lookup"><span data-stu-id="7db29-129">Hot</span></span> | <span data-ttu-id="7db29-130">Nem</span><span class="sxs-lookup"><span data-stu-id="7db29-130">No</span></span> |
> | &nbsp; | <span data-ttu-id="7db29-131">Ritka</span><span class="sxs-lookup"><span data-stu-id="7db29-131">Cool</span></span> | <span data-ttu-id="7db29-132">Nem</span><span class="sxs-lookup"><span data-stu-id="7db29-132">No</span></span> |

<span data-ttu-id="7db29-133">Az alapértelmezett blobtárolóban nem ajánlott üzleti adatokat tárolni.</span><span class="sxs-lookup"><span data-stu-id="7db29-133">We do not recommend that you use the default blob container for storing business data.</span></span> <span data-ttu-id="7db29-134">Az alapértelmezett blobtárolót ajánlatos törölni minden egyes használat után.</span><span class="sxs-lookup"><span data-stu-id="7db29-134">Deleting the default blob container after each use to reduce storage cost is a good practice.</span></span> <span data-ttu-id="7db29-135">Fontos, hogy az alapértelmezett tároló alkalmazás- és rendszernaplókat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7db29-135">Note that the default container contains application and system logs.</span></span> <span data-ttu-id="7db29-136">A tároló törlése előtt gondoskodjon a naplók begyűjtéséről.</span><span class="sxs-lookup"><span data-stu-id="7db29-136">Make sure to retrieve the logs before deleting the container.</span></span>

<span data-ttu-id="7db29-137">Egy blobtároló több fürt közötti megosztása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7db29-137">Sharing one blob container for multiple clusters is not supported.</span></span>

## <a name="hdinsight-storage-architecture"></a><span data-ttu-id="7db29-138">HDInsight tároló-architektúra</span><span class="sxs-lookup"><span data-stu-id="7db29-138">HDInsight storage architecture</span></span>
<span data-ttu-id="7db29-139">A következő ábra az Azure Storage-ot használó HDInsight tároló-architektúra absztrakt nézetét nyújtja:</span><span class="sxs-lookup"><span data-stu-id="7db29-139">The following diagram provides an abstract view of the HDInsight storage architecture of using Azure Storage:</span></span>

<span data-ttu-id="7db29-140">![A Hadoop-fürtök a HDFS API-val érik el és tárolják a strukturált és strukturálatlan adatokat a Blob Storage-ban.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight-tárarchitektúra")</span><span class="sxs-lookup"><span data-stu-id="7db29-140">![Hadoop clusters use the HDFS API to access and store structured and unstructured data in Blob storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")</span></span>

<span data-ttu-id="7db29-141">A HDInsight hozzáférést nyújt a helyileg a számítási csomópontokhoz csatlakozó elosztott fájlrendszerhez.</span><span class="sxs-lookup"><span data-stu-id="7db29-141">HDInsight provides access to the distributed file system that is locally attached to the compute nodes.</span></span> <span data-ttu-id="7db29-142">Ez a fájlrendszer a teljes URI használatával érhető el, például:</span><span class="sxs-lookup"><span data-stu-id="7db29-142">This file system can be accessed by using the fully qualified URI, for example:</span></span>

    hdfs://<namenodehost>/<path>

<span data-ttu-id="7db29-143">Emellett a HDInsight lehetővé teszi az Azure Storage tárolóban tárolt adatok elérését.</span><span class="sxs-lookup"><span data-stu-id="7db29-143">In addition, HDInsight allows you to access data that is stored in Azure Storage.</span></span> <span data-ttu-id="7db29-144">A szintaxis a következő:</span><span class="sxs-lookup"><span data-stu-id="7db29-144">The syntax is:</span></span>

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

<span data-ttu-id="7db29-145">Az alábbiakban néhány szempont olvasható Azure Storage-fiók és HDInsight-fürtök együttes használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7db29-145">Here are some considerations when using Azure Storage account with HDInsight clusters.</span></span>

* <span data-ttu-id="7db29-146">**Fürthöz csatlakozó tárolók a tárfiókokban:** Mivel a fiók neve és kulcsa társítva van a fürttel a létrehozás során, teljes hozzáféréssel rendelkezik a tárolókban lévő összes blobhoz.</span><span class="sxs-lookup"><span data-stu-id="7db29-146">**Containers in the storage accounts that are connected to a cluster:** Because the account name and key are associated with the cluster during creation, you have full access to the blobs in those containers.</span></span>

* <span data-ttu-id="7db29-147">**Fürthöz NEM csatlakozó nyilvános tárolók vagy nyilvános blobok a tárfiókokban:** Csak olvasási engedéllyel rendelkezik a tárolókban lévő blobokhoz.</span><span class="sxs-lookup"><span data-stu-id="7db29-147">**Public containers or public blobs in storage accounts that are NOT connected to a cluster:** You have read-only permission to the blobs in the containers.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="7db29-148">A nyilvános tárolók esetén a tárolóban elérhető összes blob listáját és a tároló metaadatait is lekérheti.</span><span class="sxs-lookup"><span data-stu-id="7db29-148">Public containers allow you to get a list of all blobs that are available in that container and get container metadata.</span></span> <span data-ttu-id="7db29-149">A nyilvános blobok esetén csak akkor érheti el a blobokat, ha ismeri a pontos URL-t.</span><span class="sxs-lookup"><span data-stu-id="7db29-149">Public blobs allow you to access the blobs only if you know the exact URL.</span></span> <span data-ttu-id="7db29-150">További információkért lásd: <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access to containers and blobs</a> (A tárolókhoz és blobokhoz való hozzáférés korlátozása).</span><span class="sxs-lookup"><span data-stu-id="7db29-150">For more information, see <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access to containers and blobs</a>.</span></span>
  > 
  > 
* <span data-ttu-id="7db29-151">**Fürthöz NEM csatlakozó személyes tárolók a tárfiókokban:** Nem érheti el a tárolókban lévő blobokat, ha nem határozza meg a tárfiókot a WebHCat-feladatok elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="7db29-151">**Private containers in storage accounts that are NOT connected to a cluster:** You can't access the blobs in the containers unless you define the storage account when you submit the WebHCat jobs.</span></span> <span data-ttu-id="7db29-152">Ennek a magyarázatát a cikk későbbi részében találja.</span><span class="sxs-lookup"><span data-stu-id="7db29-152">This is explained later in this article.</span></span>

<span data-ttu-id="7db29-153">A létrehozási folyamat során meghatározott tárfiókok és a kulcsaik a %HADOOP_HOME%/conf/core-site.xml fájlban találhatók a fürtcsomópontokon.</span><span class="sxs-lookup"><span data-stu-id="7db29-153">The storage accounts that are defined in the creation process and their keys are stored in %HADOOP_HOME%/conf/core-site.xml on the cluster nodes.</span></span> <span data-ttu-id="7db29-154">A HDInsight alapértelmezett viselkedése a core-site.xml fájlban meghatározott tárfiókok használata.</span><span class="sxs-lookup"><span data-stu-id="7db29-154">The default behavior of HDInsight is to use the storage accounts defined in the core-site.xml file.</span></span> <span data-ttu-id="7db29-155">A beállítást az [Ambari](./hdinsight-hadoop-manage-ambari.md) használatával módosíthatja</span><span class="sxs-lookup"><span data-stu-id="7db29-155">You can modify this setting using [Ambari](./hdinsight-hadoop-manage-ambari.md)</span></span>

<span data-ttu-id="7db29-156">Több WebHCat-feladat (beleértve a Hive, MapReduce, Hadoop-stream és Pig-feladatokat) is tartalmazhatja a tárfiókok és a metaadatok leírását.</span><span class="sxs-lookup"><span data-stu-id="7db29-156">Multiple WebHCat jobs, including Hive, MapReduce, Hadoop streaming, and Pig, can carry a description of storage accounts and metadata with them.</span></span> <span data-ttu-id="7db29-157">(Ez jelenleg a tárfiókokkal működik a Pig-feladatokkal, a metaadatokkal nem.) További információkért lásd: [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (HDInsight-fürtök használata alternatív tárfiókokkal és metaadattárakkal).</span><span class="sxs-lookup"><span data-stu-id="7db29-157">(This currently works for Pig with storage accounts, but not for metadata.) For more information, see [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span></span>

<span data-ttu-id="7db29-158">A blobok a strukturált és strukturálatlan adatokhoz használhatók.</span><span class="sxs-lookup"><span data-stu-id="7db29-158">Blobs can be used for structured and unstructured data.</span></span> <span data-ttu-id="7db29-159">A blobtárolók kulcs/érték párokként tárolnak adatokat, és nincs könyvtár-hierarchia.</span><span class="sxs-lookup"><span data-stu-id="7db29-159">Blob containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="7db29-160">A perjel karakter ( / ) azonban használható a kulcsnévben, hogy úgy tűnjön, mintha a fájl könyvtárszerkezetben lenne tárolva.</span><span class="sxs-lookup"><span data-stu-id="7db29-160">However the slash character ( / ) can be used within the key name to make it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="7db29-161">Egy blob kulcsa lehet például az *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="7db29-161">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="7db29-162">Nem létezik tényleges *input* könyvtár, de mivel jelen van a perjel karakter a kulcsnévben, úgy néz ki, mint egy fájlútvonal.</span><span class="sxs-lookup"><span data-stu-id="7db29-162">No actual *input* directory exists, but due to the presence of the slash character in the key name, it has the appearance of a file path.</span></span>

## <span data-ttu-id="7db29-163"><a id="benefits"></a>Az Azure Storage előnyei</span><span class="sxs-lookup"><span data-stu-id="7db29-163"><a id="benefits"></a>Benefits of Azure Storage</span></span>
<span data-ttu-id="7db29-164">A számítási fürtök és tárolási erőforrások nem egy helyre helyezésével járó teljesítményköltségeket csökkenti a számítási fürtöknek a tárfiók erőforrásainak közelében való létrehozása az Azure-régión belül, ahol a nagysebességű hálózat hatékonnyá teszi a számítási csomópontok számára az Azure Storage-ban lévő adatok elérését.</span><span class="sxs-lookup"><span data-stu-id="7db29-164">The implied performance cost of not co-locating compute clusters and storage resources is mitigated by the way the compute clusters are created close to the storage account resources inside the Azure region, where the high-speed network makes it efficient for the compute nodes to access the data inside Azure storage.</span></span>

<span data-ttu-id="7db29-165">Több előnye is van annak, ha az adatokat a HDFS helyett az Azure Blob Storage tárolóban tárolja:</span><span class="sxs-lookup"><span data-stu-id="7db29-165">There are several benefits associated with storing the data in Azure storage instead of HDFS:</span></span>

* <span data-ttu-id="7db29-166">**Adatok újbóli használata és megosztása:** A HDFS-ben az adatok a számítási fürtön belül találhatóak.</span><span class="sxs-lookup"><span data-stu-id="7db29-166">**Data reuse and sharing:** The data in HDFS is located inside the compute cluster.</span></span> <span data-ttu-id="7db29-167">Csak a számítási fürtöt elérő alkalmazások használhatják az adatokat HDFS API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="7db29-167">Only the applications that have access to the compute cluster can use the data by using HDFS APIs.</span></span> <span data-ttu-id="7db29-168">Az Azure Blob Storage-ban lévő adatok a HDFS API-kon vagy a [Blob Storage REST API-kon][blob-storage-restAPI] keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7db29-168">The data in Azure storage can be accessed either through the HDFS APIs or through the [Blob Storage REST APIs][blob-storage-restAPI].</span></span> <span data-ttu-id="7db29-169">Így az alkalmazások (beleértve más HDInsight fürtöket) és eszközök nagyobb készlete használható az adatok előállításához és fogyasztásához.</span><span class="sxs-lookup"><span data-stu-id="7db29-169">Thus, a larger set of applications (including other HDInsight clusters) and tools can be used to produce and consume the data.</span></span>
* <span data-ttu-id="7db29-170">**Adatarchiválás:** Az adatok Azure Blob Storage tárolóban végzett tárolása lehetővé teszi, hogy biztonságosan törölje a számításhoz használt HDInsight fürtöket a felhasználói adatok elvesztése nélkül.</span><span class="sxs-lookup"><span data-stu-id="7db29-170">**Data archiving:** Storing data in Azure storage enables the HDInsight clusters used for computation to be safely deleted without losing user data.</span></span>
* <span data-ttu-id="7db29-171">**Adattárolási költség:** Az adatok DFS-ben végzett hosszú távú tárolása költségesebb, mint az adatok Azure Storage tárolóban végzett tárolása, mert a számítási fürt költsége nagyobb, mint az Azure Storage költsége.</span><span class="sxs-lookup"><span data-stu-id="7db29-171">**Data storage cost:** Storing data in DFS for the long term is more costly than storing the data in Azure storage because the cost of a compute cluster is higher than the cost of Azure storage.</span></span> <span data-ttu-id="7db29-172">Ezenkívül mivel az adatokat nem kell újból betölteni minden számítási fürt létrehozásakor, az adatbetöltési költségeket is megtakaríthatja.</span><span class="sxs-lookup"><span data-stu-id="7db29-172">In addition, because the data does not have to be reloaded for every compute cluster generation, you are also saving data loading costs.</span></span>
* <span data-ttu-id="7db29-173">**Rugalmas kibővítés:** Bár a HDFS kibővített fájlrendszert biztosít, a léptéket a fürthöz létrehozott csomópontok száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="7db29-173">**Elastic scale-out:** Although HDFS provides you with a scaled-out file system, the scale is determined by the number of nodes that you create for your cluster.</span></span> <span data-ttu-id="7db29-174">A lépték módosítása bonyolultabb folyamattá válhat, mintha az Azure Blob Storage tárolóban automatikusan elérhető rugalmas léptékezési képességekre támaszkodna.</span><span class="sxs-lookup"><span data-stu-id="7db29-174">Changing the scale can become a more complicated process than relying on the elastic scaling capabilities that you get automatically in Azure storage.</span></span>
* <span data-ttu-id="7db29-175">**Georeplikáció:** Az Azure Storage tárolókról georeplikálás készíthető.</span><span class="sxs-lookup"><span data-stu-id="7db29-175">**Geo-replication:** Your Azure storage can be geo-replicated.</span></span> <span data-ttu-id="7db29-176">Bár ez földrajzi helyreállítást és adatredundanciát biztosít, a georeplikált helyre végzett feladatátvétel súlyos hatással van a teljesítményre, és további költségekkel járhat.</span><span class="sxs-lookup"><span data-stu-id="7db29-176">Although this gives you geographic recovery and data redundancy, a failover to the geo-replicated location severely impacts your performance, and it may incur additional costs.</span></span> <span data-ttu-id="7db29-177">Így azt javasoljuk, hogy válassza körültekintően a georeplikációt, és csak akkor, ha az adatok megérik a további költségeket.</span><span class="sxs-lookup"><span data-stu-id="7db29-177">So our recommendation is to choose the geo-replication wisely and only if the value of the data is worth the additional cost.</span></span>

<span data-ttu-id="7db29-178">Bizonyos MapReduce-feladatok és csomagok olyan köztes eredményeket hozhatnak létre, amelyeket nem érdemes az Azure Blob Storage tárolóban tárolni.</span><span class="sxs-lookup"><span data-stu-id="7db29-178">Certain MapReduce jobs and packages may create intermediate results that you don't really want to store in Azure storage.</span></span> <span data-ttu-id="7db29-179">Ebben az esetben a helyi HDFS-ben is tárolhatja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7db29-179">In that case, you can elect to store the data in the local HDFS.</span></span> <span data-ttu-id="7db29-180">Valójában a HDInsight a DFS-t használja több ilyen köztes eredményhez a Hive-feladatokban és egyéb folyamatokban.</span><span class="sxs-lookup"><span data-stu-id="7db29-180">In fact, HDInsight uses DFS for several of these intermediate results in Hive jobs and other processes.</span></span>

> [!NOTE]
> <span data-ttu-id="7db29-181">A legtöbb HDFS parancs (például az <b>ls</b>, a <b>copyFromLocal</b> és a <b>mkdir</b>) továbbra is a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="7db29-181">Most HDFS commands (for example, <b>ls</b>, <b>copyFromLocal</b> and <b>mkdir</b>) still work as expected.</span></span> <span data-ttu-id="7db29-182">Csak a natív HDFS implementációra (más néven DFS-re) jellemző parancsok, például az <b>fschk</b> és a <b>dfsadmin</b> viselkednek máshogy az Azure Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7db29-182">Only the commands that are specific to the native HDFS implementation (which is referred to as DFS), such as <b>fschk</b> and <b>dfsadmin</b>, show different behavior in Azure storage.</span></span>
> 
> 

## <a name="create-blob-containers"></a><span data-ttu-id="7db29-183">Blob tárolók létrehozása</span><span class="sxs-lookup"><span data-stu-id="7db29-183">Create Blob containers</span></span>
<span data-ttu-id="7db29-184">A blobok használatához először hozzon létre egy [Azure Storage-fiókot][azure-storage-create].</span><span class="sxs-lookup"><span data-stu-id="7db29-184">To use blobs, you first create an [Azure Storage account][azure-storage-create].</span></span> <span data-ttu-id="7db29-185">Ennek részeként meg kell adnia egy Azure-régiót, amelyben a tárfiók létrejön.</span><span class="sxs-lookup"><span data-stu-id="7db29-185">As part of this, you specify an Azure region where the storage account is created.</span></span> <span data-ttu-id="7db29-186">A fürtnek és a tárfióknak ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7db29-186">The cluster and the storage account must be hosted in the same region.</span></span> <span data-ttu-id="7db29-187">A Hive-metaadattár SQL Server adatbázisának és az Oozie-metaadattár SQL Server adatbázisának is ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7db29-187">The Hive metastore SQL Server database and Oozie metastore SQL Server database must also be located in the same region.</span></span>

<span data-ttu-id="7db29-188">Akárhol él, mindegyik létrehozott blob az Azure Storage-fiókban lévő tárolóhoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="7db29-188">Wherever it lives, each blob you create belongs to a container in your Azure Storage account.</span></span> <span data-ttu-id="7db29-189">Ez a tároló egy már létező, a HDInsight eszközön kívül létrejövő blob vagy egy HDInsight-fürthöz létrehozott tároló lehet.</span><span class="sxs-lookup"><span data-stu-id="7db29-189">This container may be an existing blob that was created outside of HDInsight, or it may be a container that is created for an HDInsight cluster.</span></span>

<span data-ttu-id="7db29-190">Az alapértelmezett Blob-tároló a fürtre jellemző információkat, például a feladatelőzményeket és a naplókat tárolja.</span><span class="sxs-lookup"><span data-stu-id="7db29-190">The default Blob container stores cluster-specific information such as job history and logs.</span></span> <span data-ttu-id="7db29-191">Ne osszon meg alapértelmezett Blob tárolókat több HDInsight-fürttel.</span><span class="sxs-lookup"><span data-stu-id="7db29-191">Don't share a default Blob container with multiple HDInsight clusters.</span></span> <span data-ttu-id="7db29-192">Ez károsíthatja a feladatelőzményeket.</span><span class="sxs-lookup"><span data-stu-id="7db29-192">This might corrupt job history.</span></span> <span data-ttu-id="7db29-193">Ajánlott különböző tárolót használni mindegyik fürthöz és a megosztott adatokat az összes kapcsolódó fürt üzemelő példányában meghatározott kapcsolt tárfiókra helyezni az alapértelmezett tárfiók helyett.</span><span class="sxs-lookup"><span data-stu-id="7db29-193">It is recommended to use a different container for each cluster and put shared data on a linked storage account specified in deployment of all relevant clusters rather than the default storage account.</span></span> <span data-ttu-id="7db29-194">A kapcsolt tárfiókok konfigurálásáról további információért lásd: [HDInsight-fürtök létrehozása][hdinsight-creation].</span><span class="sxs-lookup"><span data-stu-id="7db29-194">For more information on configuring linked storage accounts, see [Create HDInsight clusters][hdinsight-creation].</span></span> <span data-ttu-id="7db29-195">De újból felhasználhatja az alapértelmezett tárolókat az eredeti HDInsight fürt törlése után.</span><span class="sxs-lookup"><span data-stu-id="7db29-195">However you can reuse a default storage container after the original HDInsight cluster has been deleted.</span></span> <span data-ttu-id="7db29-196">Az HBase fürtök esetén megőrizheti az HBase táblasémát és adatokat, ha létrehoz egy új HBase-fürtöt a törölt HBase-fürt által használt alapértelmezett blobtárolóval.</span><span class="sxs-lookup"><span data-stu-id="7db29-196">For HBase clusters, you can actually retain the HBase table schema and data by creating a new HBase cluster using the default blob container that is used by an HBase cluster that has been deleted.</span></span>

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-the-azure-portal"></a><span data-ttu-id="7db29-197">Az Azure Portal használata</span><span class="sxs-lookup"><span data-stu-id="7db29-197">Use the Azure portal</span></span>
<span data-ttu-id="7db29-198">Amikor HDInsight-fürtöt hoz létre a portálról, megadhatja a tárfiók részleteit (az alábbiakban látható módon).</span><span class="sxs-lookup"><span data-stu-id="7db29-198">When creating an HDInsight cluster from the Portal, you have the options (as shown below) to provide the storage account details.</span></span> <span data-ttu-id="7db29-199">Azt is megadhatja, hogy szeretne-e további tárfiókot társítani a fürttel, és ha igen, a Data Lake Store-t vagy más Azure Storage-blobot választhat további tárolóként.</span><span class="sxs-lookup"><span data-stu-id="7db29-199">You can also specify whether you want an additional storage account associated with the cluster, and if so, choose from Data Lake Store or another Azure Storage blob as the additional storage.</span></span>

![HDInsight hadoop létrehozási adatforrás](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> <span data-ttu-id="7db29-201">A rendszer nem támogatja további tárfiókok használatát a HDInsight-fürtön kívül eső helyeken.</span><span class="sxs-lookup"><span data-stu-id="7db29-201">Using an additional storage account in a different location than the HDInsight cluster is not supported.</span></span>


### <a name="use-azure-powershell"></a><span data-ttu-id="7db29-202">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="7db29-202">Use Azure PowerShell</span></span>
<span data-ttu-id="7db29-203">Ha [telepítette és konfigurálta az Azure PowerShellt][powershell-install], a következőt használhatja az Azure PowerShell-parancssorról egy tárfiók és tároló létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="7db29-203">If you [installed and configured Azure PowerShell][powershell-install], you can use the following from the Azure PowerShell prompt to create a storage account and container:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"

    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"

    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID

    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location

    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 

    # Create default blob containers
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

### <a name="use-azure-cli"></a><span data-ttu-id="7db29-204">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="7db29-204">Use Azure CLI</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

<span data-ttu-id="7db29-205">Ha [telepítette és konfigurálta az Azure CLI parancssori felületet](../cli-install-nodejs.md), a következő parancs használható a tárfiókokhoz és tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="7db29-205">If you have [installed and configured the Azure CLI](../cli-install-nodejs.md), the following command can be used to a storage account and container.</span></span>

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> <span data-ttu-id="7db29-206">A `--type` paraméter jelzi, hogyan lesz replikálva a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="7db29-206">The `--type` parameter indicates how the storage account is replicated.</span></span> <span data-ttu-id="7db29-207">További információ: [Azure Storage replication](../storage/storage-redundancy.md) (Az Azure Storage replikációja).</span><span class="sxs-lookup"><span data-stu-id="7db29-207">For more information, see [Azure Storage Replication](../storage/storage-redundancy.md).</span></span> <span data-ttu-id="7db29-208">Na használjon ZRS-t, mivel a ZRS nem támogatja a lapblobokat, a fájlokat, a táblákat és az üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="7db29-208">Don't use ZRS as ZRS doesn't support page blob, file, table, or queue.</span></span>
> 
> 

<span data-ttu-id="7db29-209">A rendszer kéri, hogy adja meg a földrajzi régiót, amelyben a tárfiók létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="7db29-209">You are prompted to specify the geographic region that the storage account is created in.</span></span> <span data-ttu-id="7db29-210">Ugyanabban a régióban kell létrehoznia a tárfiókot, mint amelyben a HDInsight-fürt létrehozását tervezi.</span><span class="sxs-lookup"><span data-stu-id="7db29-210">You should create the storage account in the same region that you plan on creating your HDInsight cluster.</span></span>

<span data-ttu-id="7db29-211">A tárfiók létrehozása után a következő paranccsal kérheti le a tárfiók kulcsait:</span><span class="sxs-lookup"><span data-stu-id="7db29-211">Once the storage account is created, use the following command to retrieve the storage account keys:</span></span>

    azure storage account keys list <storageaccountname>

<span data-ttu-id="7db29-212">Egy tároló létrehozásához használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="7db29-212">To create a container, use the following command:</span></span>

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a><span data-ttu-id="7db29-213">Az Azure Storage tárolóban található címfájlok</span><span class="sxs-lookup"><span data-stu-id="7db29-213">Address files in Azure storage</span></span>
<span data-ttu-id="7db29-214">Az Azure Storage tárolóban a HDInsight eszközről végzett fájlelérés URI sémája a következő:</span><span class="sxs-lookup"><span data-stu-id="7db29-214">The URI scheme for accessing files in Azure storage from HDInsight is:</span></span>

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

<span data-ttu-id="7db29-215">Az URI séma titkosítatlan hozzáférést (a *wasb:* előtaggal) és SSL titkosított hozzáférést (a *wasbs* előtaggal) biztosít.</span><span class="sxs-lookup"><span data-stu-id="7db29-215">The URI scheme provides unencrypted access (with the *wasb:* prefix) and SSL encrypted access (with *wasbs*).</span></span> <span data-ttu-id="7db29-216">Ajánlott a *wasbs* előtagot használnia, amikor lehetséges, még akkor is, amikor az Azure-ban ugyanabban a régióban lévő adatokat éri el.</span><span class="sxs-lookup"><span data-stu-id="7db29-216">We recommend using *wasbs* wherever possible, even when accessing data that lives inside the same region in Azure.</span></span>

<span data-ttu-id="7db29-217">A &lt;BlobStorageContainerName&gt; azonosítja a blobtároló nevét az Azure Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7db29-217">The &lt;BlobStorageContainerName&gt; identifies the name of the blob container in Azure storage.</span></span>
<span data-ttu-id="7db29-218">A &lt;StorageAccountName&gt; azonosítja az Azure Storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="7db29-218">The &lt;StorageAccountName&gt; identifies the Azure Storage account name.</span></span> <span data-ttu-id="7db29-219">Szükség van a teljes tartománynévre (FQDN-re).</span><span class="sxs-lookup"><span data-stu-id="7db29-219">A fully qualified domain name (FQDN) is required.</span></span>

<span data-ttu-id="7db29-220">Ha a &lt;BlobStorageContainerName&gt; és a &lt;StorageAccountName&gt; sincs meghatározva, a rendszer az alapértelmezett fájlrendszert használja.</span><span class="sxs-lookup"><span data-stu-id="7db29-220">If neither &lt;BlobStorageContainerName&gt; nor &lt;StorageAccountName&gt; has been specified, the default file system is used.</span></span> <span data-ttu-id="7db29-221">Az alapértelmezett fájlrendszeren tárolt fájlok esetén relatív elérési utat vagy abszolút elérési utat használhat.</span><span class="sxs-lookup"><span data-stu-id="7db29-221">For the files on the default file system, you can use a relative path or an absolute path.</span></span> <span data-ttu-id="7db29-222">A HDInsight-fürtökben lévő *hadoop-mapreduce-examples.jar* fájlra például a következők egyikével lehet hivatkozni:</span><span class="sxs-lookup"><span data-stu-id="7db29-222">For example, the *hadoop-mapreduce-examples.jar* file that comes with HDInsight clusters can be referred to by using one of the following:</span></span>

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="7db29-223">A fájlnév a <i>hadoop-examples.jar</i> a HDInsight 2.1-es és 1.6-s verziójú fürtökben.</span><span class="sxs-lookup"><span data-stu-id="7db29-223">The file name is <i>hadoop-examples.jar</i> in HDInsight versions 2.1 and 1.6 clusters.</span></span>
> 
> 

<span data-ttu-id="7db29-224">Az &lt;elérési út&gt; a fájl vagy könyvtár HDFS elérési útja neve.</span><span class="sxs-lookup"><span data-stu-id="7db29-224">The &lt;path&gt; is the file or directory HDFS path name.</span></span> <span data-ttu-id="7db29-225">Mivel az Azure Blob Storage tárolóban lévő tárolók egyszerűen kulcs-érték tárolók, nincs valós hierarchikus fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="7db29-225">Because containers in Azure storage are simply key-value stores, there is no true hierarchical file system.</span></span> <span data-ttu-id="7db29-226">A blob kulcson belüli perjel karaktert ( / ) a rendszer könyvtárelválasztóként értelmezi.</span><span class="sxs-lookup"><span data-stu-id="7db29-226">A slash character ( / ) inside a blob key is interpreted as a directory separator.</span></span> <span data-ttu-id="7db29-227">A *hadoop-mapreduce-examples.jar* blobneve például a következő:</span><span class="sxs-lookup"><span data-stu-id="7db29-227">For example, the blob name for *hadoop-mapreduce-examples.jar* is:</span></span>

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="7db29-228">Amikor a HDInsight eszközön kívüli blobokkal dolgozik, a legtöbb segédprogram nem ismeri fel a WASB formátumot, és ehelyett alapvető elérési út formátumot vár, például a következőt: `example/jars/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="7db29-228">When working with blobs outside of HDInsight, most utilities do not recognize the WASB format and instead expect a basic path format, such as `example/jars/hadoop-mapreduce-examples.jar`.</span></span>
> 
> 

## <a name="access-blobs"></a><span data-ttu-id="7db29-229">Blobok elérése</span><span class="sxs-lookup"><span data-stu-id="7db29-229">Access blobs</span></span> 


### <span data-ttu-id="7db29-230"><a name="access-blobs-using-azure-powershell"></a> Az Azure PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="7db29-230"><a name="access-blobs-using-azure-powershell"></a> Use Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="7db29-231">Ezen szakasz parancsai a blobokban tárolt adatok PowerShell eszközön keresztüli elérésének egyszeri példáit nyújtják.</span><span class="sxs-lookup"><span data-stu-id="7db29-231">The commands in this section provide a basic example of using PowerShell to access data stored in blobs.</span></span> <span data-ttu-id="7db29-232">A HDInsight használatához testreszabott további teljes példákért lásd: [HDInsight eszközök](https://github.com/Blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="7db29-232">For a more full-featured example that is customized for working with HDInsight, see the [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span></span>
> 
> 

<span data-ttu-id="7db29-233">Használja az alábbi parancsot a blobbal kapcsolatos parancsmagok listázásához:</span><span class="sxs-lookup"><span data-stu-id="7db29-233">Use the following command to list the blob-related cmdlets:</span></span>

    Get-Command *blob*

![A blobbal kapcsolatos PowerShell parancsmagok listája.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a><span data-ttu-id="7db29-235">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="7db29-235">Upload files</span></span>
<span data-ttu-id="7db29-236">Lásd: [Adatok feltöltése a HDInsightba][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="7db29-236">See [Upload data to HDInsight][hdinsight-upload-data].</span></span>

#### <a name="download-files"></a><span data-ttu-id="7db29-237">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="7db29-237">Download files</span></span>
<span data-ttu-id="7db29-238">A következő szkript egy blokkblobot tölt le az aktuális mappába.</span><span class="sxs-lookup"><span data-stu-id="7db29-238">The following script downloads a block blob to the current folder.</span></span> <span data-ttu-id="7db29-239">A parancsfájl futtatása előtt módosítsa a könyvtárt olyan mappára, ahol írási engedélyei vannak.</span><span class="sxs-lookup"><span data-stu-id="7db29-239">Before running the script, change the directory to a folder where you have write permissions.</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

<span data-ttu-id="7db29-240">Az erőforráscsoport nevét és a fürt nevét megadva a következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="7db29-240">Providing the resource group name and the cluster name, you can use the following code:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a><span data-ttu-id="7db29-241">Fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="7db29-241">Delete files</span></span>
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a><span data-ttu-id="7db29-242">Fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="7db29-242">List files</span></span>
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a><span data-ttu-id="7db29-243">Hive-lekérdezések futtatása nem meghatározott tárfiókkal</span><span class="sxs-lookup"><span data-stu-id="7db29-243">Run Hive queries using an undefined storage account</span></span>
<span data-ttu-id="7db29-244">Ez a példa bemutatja, hogyan listázhat mappákat a létrehozási folyamat során nem meghatározott tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="7db29-244">This example shows how to list a folder from storage account that is not defined during the creating process.</span></span>
<span data-ttu-id="7db29-245">$clusterName = "<HDInsightClusterName>"</span><span class="sxs-lookup"><span data-stu-id="7db29-245">$clusterName = "<HDInsightClusterName>"</span></span>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a><span data-ttu-id="7db29-246">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="7db29-246">Use Azure CLI</span></span>
<span data-ttu-id="7db29-247">Használja az alábbi parancsot a blobbal kapcsolatos parancsok listázásához:</span><span class="sxs-lookup"><span data-stu-id="7db29-247">Use the following command to list the blob-related commands:</span></span>

    azure storage blob

<span data-ttu-id="7db29-248">**Az Azure parancssori felület használatának példája fájlok feltöltéséhez**</span><span class="sxs-lookup"><span data-stu-id="7db29-248">**Example of using Azure CLI to upload a file**</span></span>

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="7db29-249">**Az Azure parancssori felület használatának példája fájlok letöltéséhez**</span><span class="sxs-lookup"><span data-stu-id="7db29-249">**Example of using Azure CLI to download a file**</span></span>

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="7db29-250">**Az Azure parancssori felület használatának példája fájlok törléséhez**</span><span class="sxs-lookup"><span data-stu-id="7db29-250">**Example of using Azure CLI to delete a file**</span></span>

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="7db29-251">**Az Azure parancssori felület használatának példája fájlok listázásához**</span><span class="sxs-lookup"><span data-stu-id="7db29-251">**Example of using Azure CLI to list files**</span></span>

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a><span data-ttu-id="7db29-252">További tárfiókok használata</span><span class="sxs-lookup"><span data-stu-id="7db29-252">Use additional storage accounts</span></span>

<span data-ttu-id="7db29-253">HDInsight-fürt létrehozásakor meg kell adnia azt az Azure Storage-fiókot, amelyet a fürthöz társítani kívánja.</span><span class="sxs-lookup"><span data-stu-id="7db29-253">While creating an HDInsight cluster, you specify the Azure Storage account you want to associate with it.</span></span> <span data-ttu-id="7db29-254">Ezen a tárfiókon kívül további tárfiókokat vehet fel ugyanabból az Azure-előfizetésből vagy más Azure-előfizetésekből a létrehozási folyamat során vagy a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="7db29-254">In addition to this storage account, you can add additional storage accounts from the same Azure subscription or different Azure subscriptions during the creation process or after a cluster has been created.</span></span> <span data-ttu-id="7db29-255">Útmutatás további tárfiókok hozzáadásához: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="7db29-255">For instructions about adding additional storage accounts, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!WARNING]
> <span data-ttu-id="7db29-256">A rendszer nem támogatja további tárfiókok használatát a HDInsight-fürtön kívül eső helyeken.</span><span class="sxs-lookup"><span data-stu-id="7db29-256">Using an additional storage account in a different location than the HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7db29-257">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7db29-257">Next steps</span></span>
<span data-ttu-id="7db29-258">Ebből a cikkből megtanulta, hogyan használhat HDFS-kompatibilis Azure-tárolót a HDInsighttal.</span><span class="sxs-lookup"><span data-stu-id="7db29-258">In this article, you learned how to use HDFS-compatible Azure storage with HDInsight.</span></span> <span data-ttu-id="7db29-259">Ez lehetővé teszi a skálázható, hosszú távú adatarchiváló beszerzési megoldások kiépítését, valamint hogy a HDInsighttal kinyerje a strukturált és strukturálatlan tárolt adatokban lévő információkat.</span><span class="sxs-lookup"><span data-stu-id="7db29-259">This allows you to build scalable, long-term, archiving data acquisition solutions and use HDInsight to unlock the information inside the stored structured and unstructured data.</span></span>

<span data-ttu-id="7db29-260">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="7db29-260">For more information, see:</span></span>

* <span data-ttu-id="7db29-261">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="7db29-261">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="7db29-262">Az Azure Data Lake Store használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="7db29-262">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="7db29-263">[Adatok feltöltése a HDInsightba][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="7db29-263">[Upload data to HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="7db29-264">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="7db29-264">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="7db29-265">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="7db29-265">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="7db29-266">[Az Azure Storage közös hozzáférésű jogosultságkódok használata az adathozzáférés korlátozásához a HDInsightban][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="7db29-266">[Use Azure Storage Shared Access Signatures to restrict access to data with HDInsight][hdinsight-use-sas]</span></span>

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
