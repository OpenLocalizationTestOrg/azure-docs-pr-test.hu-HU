---
title: HDFS-kompatibilis aaaQuery adatait az Azure storage - Azure HDInsight |} Microsoft Docs
description: "Ismerje meg, hogyan tooquery adatokat az Azure storage és az Azure Data Lake Store toostore eredmények elemzéshez."
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
ms.openlocfilehash: 1032d60424b65e3c0c54a25c7c15970b017a788f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a><span data-ttu-id="4fafd-104">Az Azure Storage és az Azure HDInsight-fürtök együttes használata</span><span class="sxs-lookup"><span data-stu-id="4fafd-104">Use Azure storage with Azure HDInsight clusters</span></span>

<span data-ttu-id="4fafd-105">HDInsight-fürt tooanalyze adatokat, tárolhatja az adatokat hello, vagy az Azure Storage, az Azure Data Lake Store vagy mindkettőt.</span><span class="sxs-lookup"><span data-stu-id="4fafd-105">tooanalyze data in HDInsight cluster, you can store hello data either in Azure Storage, Azure Data Lake Store, or both.</span></span> <span data-ttu-id="4fafd-106">Mindkét tárolási lehetőségek lehetővé teszik a HDInsight-fürtök toosafely törlése, felhasználói adatok elvesztése nélkül számításhoz használt.</span><span class="sxs-lookup"><span data-stu-id="4fafd-106">Both storage options enable you toosafely delete HDInsight clusters that are used for computation without losing user data.</span></span>

<span data-ttu-id="4fafd-107">A Hadoop hello alapértelmezett fájlrendszer támogatja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-107">Hadoop supports a notion of hello default file system.</span></span> <span data-ttu-id="4fafd-108">hello alapértelmezett fájlrendszer egy alapértelmezett sémát és szolgáltatót jelenti.</span><span class="sxs-lookup"><span data-stu-id="4fafd-108">hello default file system implies a default scheme and authority.</span></span> <span data-ttu-id="4fafd-109">Használt tooresolve relatív elérési utakat is lehet.</span><span class="sxs-lookup"><span data-stu-id="4fafd-109">It can also be used tooresolve relative paths.</span></span> <span data-ttu-id="4fafd-110">Során hello HDInsight fürt létrehozásának folyamatát az Azure Storage hello alapértelmezett fájlrendszer egy blob-tároló is megadhat, vagy a HDInsight 3.5 választhat vagy az Azure Storage, vagy az Azure Data Lake Store hello alapértelmezett fájlok rendszert néhány kivételtől eltekintve.</span><span class="sxs-lookup"><span data-stu-id="4fafd-110">During hello HDInsight cluster creation process, you can specify a blob container in Azure Storage as hello default file system, or with HDInsight 3.5, you can select either Azure Storage or Azure Data Lake Store as hello default files system with a few exceptions.</span></span> <span data-ttu-id="4fafd-111">Hello támogatásának Data Lake Store használatának hello alapértelmezett és a csatolt tároló is, lásd: [rendelkezésre álló termékek HDInsight-fürthöz](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="4fafd-111">For hello supportability of using Data Lake Store as both hello default and linked storage, see [Availabilities for HDInsight cluster](./hdinsight-hadoop-use-data-lake-store.md#availabilities-for-hdinsight-clusters).</span></span>

<span data-ttu-id="4fafd-112">Ebből a cikkből megtudhatja, hogyan használható az Azure Storage a HDInsight-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="4fafd-112">In this article, you learn how Azure Storage works with HDInsight clusters.</span></span> <span data-ttu-id="4fafd-113">toolearn Data Lake Store működése HDInsight-fürtökkel, lásd: [használata Azure Data Lake Store az Azure HDInsight-fürtök](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="4fafd-113">toolearn how Data Lake Store works with HDInsight clusters, see [Use Azure Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> <span data-ttu-id="4fafd-114">További információ a HDInsight-fürtök létrehozásáról: [Hadoop-fürtök létrehozása a HDInsightban](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="4fafd-114">For more information about creating an HDInsight cluster, see [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

<span data-ttu-id="4fafd-115">Az Azure Blob Storage egy robusztus, általános célú tárolómegoldás, amely zökkenőmentesen integrálható a HDInsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="4fafd-115">Azure storage is a robust, general-purpose storage solution that integrates seamlessly with HDInsight.</span></span> <span data-ttu-id="4fafd-116">HDInsight használható egy blob-tároló az Azure Storage hello alapértelmezett fájlrendszer hello fürtökhöz.</span><span class="sxs-lookup"><span data-stu-id="4fafd-116">HDInsight can use a blob container in Azure Storage as hello default file system for hello cluster.</span></span> <span data-ttu-id="4fafd-117">Egy Hadoop elosztott fájlrendszer (HDFS) rendszer felületen hello hdinsight összetevők teljes készlete működhet közvetlenül a strukturált vagy strukturálatlan adatok blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-117">Through a Hadoop distributed file system (HDFS) interface, hello full set of components in HDInsight can operate directly on structured or unstructured data stored as blobs.</span></span>

> [!WARNING]
> <span data-ttu-id="4fafd-118">Az Azure Storage-fiók létrehozása során számos beállítás áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="4fafd-118">There are several options available when creating an Azure Storage account.</span></span> <span data-ttu-id="4fafd-119">a következő táblázat hello információt nyújt a hdinsight eszközzel milyen lehetőségeket támogat:</span><span class="sxs-lookup"><span data-stu-id="4fafd-119">hello following table provides information on what options are supported with HDInsight:</span></span>
> 
> | <span data-ttu-id="4fafd-120">Tárfiók típusa</span><span class="sxs-lookup"><span data-stu-id="4fafd-120">Storage account type</span></span> | <span data-ttu-id="4fafd-121">Tárolási réteg</span><span class="sxs-lookup"><span data-stu-id="4fafd-121">Storage tier</span></span> | <span data-ttu-id="4fafd-122">Támogatott a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="4fafd-122">Supported with HDInsight</span></span> |
> | ------- | ------- | ------- |
> | <span data-ttu-id="4fafd-123">Általános célú tárfiókok</span><span class="sxs-lookup"><span data-stu-id="4fafd-123">General-purpose Storage Account</span></span> | <span data-ttu-id="4fafd-124">Standard</span><span class="sxs-lookup"><span data-stu-id="4fafd-124">Standard</span></span> | <span data-ttu-id="4fafd-125">__Igen__</span><span class="sxs-lookup"><span data-stu-id="4fafd-125">__Yes__</span></span> |
> | &nbsp; | <span data-ttu-id="4fafd-126">Prémium</span><span class="sxs-lookup"><span data-stu-id="4fafd-126">Premium</span></span> | <span data-ttu-id="4fafd-127">Nem</span><span class="sxs-lookup"><span data-stu-id="4fafd-127">No</span></span> |
> | <span data-ttu-id="4fafd-128">Blob Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="4fafd-128">Blob Storage Account</span></span> | <span data-ttu-id="4fafd-129">Gyakori</span><span class="sxs-lookup"><span data-stu-id="4fafd-129">Hot</span></span> | <span data-ttu-id="4fafd-130">Nem</span><span class="sxs-lookup"><span data-stu-id="4fafd-130">No</span></span> |
> | &nbsp; | <span data-ttu-id="4fafd-131">Ritka</span><span class="sxs-lookup"><span data-stu-id="4fafd-131">Cool</span></span> | <span data-ttu-id="4fafd-132">Nem</span><span class="sxs-lookup"><span data-stu-id="4fafd-132">No</span></span> |

<span data-ttu-id="4fafd-133">Nem javasoljuk, hogy hello alapértelmezett blob tárolókat használ üzleti adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="4fafd-133">We do not recommend that you use hello default blob container for storing business data.</span></span> <span data-ttu-id="4fafd-134">Hello alapértelmezett blob tároló törlése után minden használata tooreduce tárolási költségű érdemes.</span><span class="sxs-lookup"><span data-stu-id="4fafd-134">Deleting hello default blob container after each use tooreduce storage cost is a good practice.</span></span> <span data-ttu-id="4fafd-135">Megjegyzés: hello alapértelmezett tároló tartalmazza az alkalmazás- és naplókat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-135">Note that hello default container contains application and system logs.</span></span> <span data-ttu-id="4fafd-136">Győződjön meg arról, hogy tooretrieve hello naplók hello tároló törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="4fafd-136">Make sure tooretrieve hello logs before deleting hello container.</span></span>

<span data-ttu-id="4fafd-137">Egy blobtároló több fürt közötti megosztása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fafd-137">Sharing one blob container for multiple clusters is not supported.</span></span>

## <a name="hdinsight-storage-architecture"></a><span data-ttu-id="4fafd-138">HDInsight tároló-architektúra</span><span class="sxs-lookup"><span data-stu-id="4fafd-138">HDInsight storage architecture</span></span>
<span data-ttu-id="4fafd-139">a következő diagram hello hello Azure Storage használata a HDInsight Tárló-architektúra absztrakt nézetét nyújtja:</span><span class="sxs-lookup"><span data-stu-id="4fafd-139">hello following diagram provides an abstract view of hello HDInsight storage architecture of using Azure Storage:</span></span>

<span data-ttu-id="4fafd-140">![Hadoop-fürtök hello HDFS API-val tooaccess és strukturált és strukturálatlan adatokat a Blob Storage tárolóban tárolja. ] (./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight tároló-architektúra")</span><span class="sxs-lookup"><span data-stu-id="4fafd-140">![Hadoop clusters use hello HDFS API tooaccess and store structured and unstructured data in Blob storage.](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")</span></span>

<span data-ttu-id="4fafd-141">HDInsight biztosít hozzáférést toohello elosztott fájlrendszer, amely a helyileg csatlakoztatott toohello számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="4fafd-141">HDInsight provides access toohello distributed file system that is locally attached toohello compute nodes.</span></span> <span data-ttu-id="4fafd-142">Ez a fájlrendszer használatával elérhető például csak a teljesen minősített URI, hello:</span><span class="sxs-lookup"><span data-stu-id="4fafd-142">This file system can be accessed by using hello fully qualified URI, for example:</span></span>

    hdfs://<namenodehost>/<path>

<span data-ttu-id="4fafd-143">Ezenkívül HDInsight lehetővé teszi az Azure Storage tárolt tooaccess adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-143">In addition, HDInsight allows you tooaccess data that is stored in Azure Storage.</span></span> <span data-ttu-id="4fafd-144">hello szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="4fafd-144">hello syntax is:</span></span>

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>

<span data-ttu-id="4fafd-145">Az alábbiakban néhány szempont olvasható Azure Storage-fiók és HDInsight-fürtök együttes használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="4fafd-145">Here are some considerations when using Azure Storage account with HDInsight clusters.</span></span>

* <span data-ttu-id="4fafd-146">**A tárolók hello storage-fiókok, amelyek csatlakoztatott tooa fürt:** hello fióknevet és kulcsot társított hello fürt létrehozása során, mert rendelkezik teljes körű hozzáférési toohello tárolókban lévő összes blobhoz.</span><span class="sxs-lookup"><span data-stu-id="4fafd-146">**Containers in hello storage accounts that are connected tooa cluster:** Because hello account name and key are associated with hello cluster during creation, you have full access toohello blobs in those containers.</span></span>

* <span data-ttu-id="4fafd-147">**Nyilvános tárolók vagy nyilvános blobok a tárfiókokban, amelyek nincsenek csatlakoztatva a tooa fürt:** hello tárolók toohello blobokat olvasási engedéllyel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4fafd-147">**Public containers or public blobs in storage accounts that are NOT connected tooa cluster:** You have read-only permission toohello blobs in hello containers.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="4fafd-148">Nyilvános tárolók lehetővé teszik a tooget, amelyek a tárolóban elérhető, és a tároló metaadatot beszerezni összes BLOB listáját.</span><span class="sxs-lookup"><span data-stu-id="4fafd-148">Public containers allow you tooget a list of all blobs that are available in that container and get container metadata.</span></span> <span data-ttu-id="4fafd-149">A nyilvános blobok meg tooaccess hello blobok csak akkor, ha tudja hello pontos URL-cím.</span><span class="sxs-lookup"><span data-stu-id="4fafd-149">Public blobs allow you tooaccess hello blobs only if you know hello exact URL.</span></span> <span data-ttu-id="4fafd-150">További információkért lásd: <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">korlátozzák a hozzáférést toocontainers és blobok</a>.</span><span class="sxs-lookup"><span data-stu-id="4fafd-150">For more information, see <a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">Restrict access toocontainers and blobs</a>.</span></span>
  > 
  > 
* <span data-ttu-id="4fafd-151">**Személyes tárolók a tárfiókokban, amelyek nincsenek csatlakoztatva tooa fürt:** hello tárolókban lévő hello blobok nem érhetők el, kivéve, ha hello tárfiók adhat meg hello WebHCat-feladatok elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="4fafd-151">**Private containers in storage accounts that are NOT connected tooa cluster:** You can't access hello blobs in hello containers unless you define hello storage account when you submit hello WebHCat jobs.</span></span> <span data-ttu-id="4fafd-152">Ennek a magyarázatát a cikk későbbi részében találja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-152">This is explained later in this article.</span></span>

<span data-ttu-id="4fafd-153">hello létrehozási folyamata és azok kulcsait meghatározott tárfiókok hello hello fürtcsomópontokon %HADOOP_HOME%/conf/core-site.xml fájlban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="4fafd-153">hello storage accounts that are defined in hello creation process and their keys are stored in %HADOOP_HOME%/conf/core-site.xml on hello cluster nodes.</span></span> <span data-ttu-id="4fafd-154">HDInsight alapértelmezett viselkedése hello toouse hello hello core-site.xml fájlban meghatározott tárfiókok.</span><span class="sxs-lookup"><span data-stu-id="4fafd-154">hello default behavior of HDInsight is toouse hello storage accounts defined in hello core-site.xml file.</span></span> <span data-ttu-id="4fafd-155">A beállítást az [Ambari](./hdinsight-hadoop-manage-ambari.md) használatával módosíthatja</span><span class="sxs-lookup"><span data-stu-id="4fafd-155">You can modify this setting using [Ambari](./hdinsight-hadoop-manage-ambari.md)</span></span>

<span data-ttu-id="4fafd-156">Több WebHCat-feladat (beleértve a Hive, MapReduce, Hadoop-stream és Pig-feladatokat) is tartalmazhatja a tárfiókok és a metaadatok leírását.</span><span class="sxs-lookup"><span data-stu-id="4fafd-156">Multiple WebHCat jobs, including Hive, MapReduce, Hadoop streaming, and Pig, can carry a description of storage accounts and metadata with them.</span></span> <span data-ttu-id="4fafd-157">(Ez jelenleg a tárfiókokkal működik a Pig-feladatokkal, a metaadatokkal nem.) További információkért lásd: [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx) (HDInsight-fürtök használata alternatív tárfiókokkal és metaadattárakkal).</span><span class="sxs-lookup"><span data-stu-id="4fafd-157">(This currently works for Pig with storage accounts, but not for metadata.) For more information, see [Using an HDInsight Cluster with Alternate Storage Accounts and Metastores](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).</span></span>

<span data-ttu-id="4fafd-158">A blobok a strukturált és strukturálatlan adatokhoz használhatók.</span><span class="sxs-lookup"><span data-stu-id="4fafd-158">Blobs can be used for structured and unstructured data.</span></span> <span data-ttu-id="4fafd-159">A blobtárolók kulcs/érték párokként tárolnak adatokat, és nincs könyvtár-hierarchia.</span><span class="sxs-lookup"><span data-stu-id="4fafd-159">Blob containers store data as key/value pairs, and there is no directory hierarchy.</span></span> <span data-ttu-id="4fafd-160">Azonban hello perjelet (/) használhatja hello kulcsnév toomake akkor jelenik meg, mintha a fájl könyvtárszerkezetben tárolódik.</span><span class="sxs-lookup"><span data-stu-id="4fafd-160">However hello slash character ( / ) can be used within hello key name toomake it appear as if a file is stored within a directory structure.</span></span> <span data-ttu-id="4fafd-161">Egy blob kulcsa lehet például az *input/log1.txt*.</span><span class="sxs-lookup"><span data-stu-id="4fafd-161">For example, a blob's key may be *input/log1.txt*.</span></span> <span data-ttu-id="4fafd-162">Nem tényleges *bemeneti* könyvtár létezik, de miatt hello perjel karakter hello kulcsnév toohello jelenlétét, egy fájl elérési útját hello megjelenésének rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4fafd-162">No actual *input* directory exists, but due toohello presence of hello slash character in hello key name, it has hello appearance of a file path.</span></span>

## <span data-ttu-id="4fafd-163"><a id="benefits"></a>Az Azure Storage előnyei</span><span class="sxs-lookup"><span data-stu-id="4fafd-163"><a id="benefits"></a>Benefits of Azure Storage</span></span>
<span data-ttu-id="4fafd-164">hello hallgatólagos teljesítményarányos költsége közös elhelyezéskor számítási fürtök és tárolási erőforrásokat elhárítására hello módon hello számítási fürtök Bezárás toohello tárfiók erőforrásainak belül hello Azure-régió, ahol nagy sebességű hálózati hello teszi jönnek létre. a számítási csomópontok hello hatékonyan tooaccess hello az Azure storage található adatok közül.</span><span class="sxs-lookup"><span data-stu-id="4fafd-164">hello implied performance cost of not co-locating compute clusters and storage resources is mitigated by hello way hello compute clusters are created close toohello storage account resources inside hello Azure region, where hello high-speed network makes it efficient for hello compute nodes tooaccess hello data inside Azure storage.</span></span>

<span data-ttu-id="4fafd-165">Hello adatok tárolása a HDFS helyett az Azure storage társított több előnye is van:</span><span class="sxs-lookup"><span data-stu-id="4fafd-165">There are several benefits associated with storing hello data in Azure storage instead of HDFS:</span></span>

* <span data-ttu-id="4fafd-166">**Adatok újbóli használata és megosztása:** hello adatokat HDFS-ben hello számítási fürtön belül található.</span><span class="sxs-lookup"><span data-stu-id="4fafd-166">**Data reuse and sharing:** hello data in HDFS is located inside hello compute cluster.</span></span> <span data-ttu-id="4fafd-167">Csak az hello alkalmazásokat, amelyek hozzáférést toohello számítási fürt használhassa a hello adatokat HDFS API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="4fafd-167">Only hello applications that have access toohello compute cluster can use hello data by using HDFS APIs.</span></span> <span data-ttu-id="4fafd-168">hello HDFS API-k vagy hello keresztül elérhető hello adatokat az Azure storage [Blob Storage REST API-k][blob-storage-restAPI].</span><span class="sxs-lookup"><span data-stu-id="4fafd-168">hello data in Azure storage can be accessed either through hello HDFS APIs or through hello [Blob Storage REST APIs][blob-storage-restAPI].</span></span> <span data-ttu-id="4fafd-169">Emiatt az alkalmazások (beleértve más HDInsight fürtöket) és eszközök nagyobb készlete használt tooproduce kell és hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-169">Thus, a larger set of applications (including other HDInsight clusters) and tools can be used tooproduce and consume hello data.</span></span>
* <span data-ttu-id="4fafd-170">**Adatarchiválás:** adatok Azure Storage tárolóban végzett tárolása lehetővé teszi, hogy biztonságosan törölje a felhasználói adatok elvesztése nélkül számítási toobe használt hello a HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="4fafd-170">**Data archiving:** Storing data in Azure storage enables hello HDInsight clusters used for computation toobe safely deleted without losing user data.</span></span>
* <span data-ttu-id="4fafd-171">**Adattárolási költség:** adattárolás DFS-ben, a hosszú távú hello költségesebb, mint az Azure storage hello adatok tárolására, mert a számítási fürt hello költség magasabb, mint az Azure storage hello költségét.</span><span class="sxs-lookup"><span data-stu-id="4fafd-171">**Data storage cost:** Storing data in DFS for hello long term is more costly than storing hello data in Azure storage because hello cost of a compute cluster is higher than hello cost of Azure storage.</span></span> <span data-ttu-id="4fafd-172">Ezenkívül hello adatokat nem kell újra minden számítási fürt létrehozásakor toobe, mert is megtakaríthatja Adatbetöltési költségeket.</span><span class="sxs-lookup"><span data-stu-id="4fafd-172">In addition, because hello data does not have toobe reloaded for every compute cluster generation, you are also saving data loading costs.</span></span>
* <span data-ttu-id="4fafd-173">**Rugalmas kibővítés:** Bár a HDFS kibővített fájlrendszert biztosít, hello méretezési hello a fürthöz létrehozott csomópontok száma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="4fafd-173">**Elastic scale-out:** Although HDFS provides you with a scaled-out file system, hello scale is determined by hello number of nodes that you create for your cluster.</span></span> <span data-ttu-id="4fafd-174">Hello skála módosítása hello elérhető rugalmas léptékezési képességekre, az Azure storage automatikusan kapott egy bonyolultabb folyamattá válhat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-174">Changing hello scale can become a more complicated process than relying on hello elastic scaling capabilities that you get automatically in Azure storage.</span></span>
* <span data-ttu-id="4fafd-175">**Georeplikáció:** Az Azure Storage tárolókról georeplikálás készíthető.</span><span class="sxs-lookup"><span data-stu-id="4fafd-175">**Geo-replication:** Your Azure storage can be geo-replicated.</span></span> <span data-ttu-id="4fafd-176">Ez lehetővé teszi földrajzi helyreállítást és adatredundanciát, bár egy feladatátvételi toohello georeplikált helyre súlyos hatással van a teljesítményre, és további költségekkel járhat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-176">Although this gives you geographic recovery and data redundancy, a failover toohello geo-replicated location severely impacts your performance, and it may incur additional costs.</span></span> <span data-ttu-id="4fafd-177">Ezért azt javasoljuk, toochoose hello georeplikáció georeplikációt, és csak akkor, ha hello hello adatok értéke érdemes hello további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="4fafd-177">So our recommendation is toochoose hello geo-replication wisely and only if hello value of hello data is worth hello additional cost.</span></span>

<span data-ttu-id="4fafd-178">Bizonyos MapReduce-feladatok és csomagok akkor is, hogy valóban szeretné toostore az Azure storage köztes eredményeket hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="4fafd-178">Certain MapReduce jobs and packages may create intermediate results that you don't really want toostore in Azure storage.</span></span> <span data-ttu-id="4fafd-179">Ebben az esetben választhatja, amelyeket toostore hello adatok hello helyi HDFS.</span><span class="sxs-lookup"><span data-stu-id="4fafd-179">In that case, you can elect toostore hello data in hello local HDFS.</span></span> <span data-ttu-id="4fafd-180">Valójában a HDInsight a DFS-t használja több ilyen köztes eredményhez a Hive-feladatokban és egyéb folyamatokban.</span><span class="sxs-lookup"><span data-stu-id="4fafd-180">In fact, HDInsight uses DFS for several of these intermediate results in Hive jobs and other processes.</span></span>

> [!NOTE]
> <span data-ttu-id="4fafd-181">A legtöbb HDFS parancs (például az <b>ls</b>, a <b>copyFromLocal</b> és a <b>mkdir</b>) továbbra is a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="4fafd-181">Most HDFS commands (for example, <b>ls</b>, <b>copyFromLocal</b> and <b>mkdir</b>) still work as expected.</span></span> <span data-ttu-id="4fafd-182">Csak az adott toohello natív HDFS implementációra (amely hivatkozott tooas DFS), parancsok, mint hello <b>fschk</b> és <b>dfsadmin</b>, az Azure storage másképp viselkednek megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="4fafd-182">Only hello commands that are specific toohello native HDFS implementation (which is referred tooas DFS), such as <b>fschk</b> and <b>dfsadmin</b>, show different behavior in Azure storage.</span></span>
> 
> 

## <a name="create-blob-containers"></a><span data-ttu-id="4fafd-183">Blob tárolók létrehozása</span><span class="sxs-lookup"><span data-stu-id="4fafd-183">Create Blob containers</span></span>
<span data-ttu-id="4fafd-184">toouse blobokat, először létre kell hoznia egy [Azure Storage-fiók][azure-storage-create].</span><span class="sxs-lookup"><span data-stu-id="4fafd-184">toouse blobs, you first create an [Azure Storage account][azure-storage-create].</span></span> <span data-ttu-id="4fafd-185">Ennek keretében adja meg az Azure-régió, ahol hello tárfiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4fafd-185">As part of this, you specify an Azure region where hello storage account is created.</span></span> <span data-ttu-id="4fafd-186">hello fürt és hello tárfiókot kell futnia hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="4fafd-186">hello cluster and hello storage account must be hosted in hello same region.</span></span> <span data-ttu-id="4fafd-187">hello Hive-metaadattár SQL Server adatbázisának és az Oozie metaadattárhoz SQL Server adatbázis is található a hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="4fafd-187">hello Hive metastore SQL Server database and Oozie metastore SQL Server database must also be located in hello same region.</span></span>

<span data-ttu-id="4fafd-188">Akárhol él, mindegyik létrehozott blob tartozik tooa tároló Azure Storage-fiókjában.</span><span class="sxs-lookup"><span data-stu-id="4fafd-188">Wherever it lives, each blob you create belongs tooa container in your Azure Storage account.</span></span> <span data-ttu-id="4fafd-189">Ez a tároló egy már létező, a HDInsight eszközön kívül létrejövő blob vagy egy HDInsight-fürthöz létrehozott tároló lehet.</span><span class="sxs-lookup"><span data-stu-id="4fafd-189">This container may be an existing blob that was created outside of HDInsight, or it may be a container that is created for an HDInsight cluster.</span></span>

<span data-ttu-id="4fafd-190">hello alapértelmezett Blob tároló például a feladatelőzményeket és a naplók fürt-specifikus adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-190">hello default Blob container stores cluster-specific information such as job history and logs.</span></span> <span data-ttu-id="4fafd-191">Ne osszon meg alapértelmezett Blob tárolókat több HDInsight-fürttel.</span><span class="sxs-lookup"><span data-stu-id="4fafd-191">Don't share a default Blob container with multiple HDInsight clusters.</span></span> <span data-ttu-id="4fafd-192">Ez károsíthatja a feladatelőzményeket.</span><span class="sxs-lookup"><span data-stu-id="4fafd-192">This might corrupt job history.</span></span> <span data-ttu-id="4fafd-193">Ajánlott különböző tárolót minden fürt és a megosztott adatok elhelyezése hello alapértelmezett tárfiókot, hanem az összes kapcsolódó fürt üzemelő példányában meghatározott kapcsolt tárfiókra toouse.</span><span class="sxs-lookup"><span data-stu-id="4fafd-193">It is recommended toouse a different container for each cluster and put shared data on a linked storage account specified in deployment of all relevant clusters rather than hello default storage account.</span></span> <span data-ttu-id="4fafd-194">A kapcsolt tárfiókok konfigurálásáról további információért lásd: [HDInsight-fürtök létrehozása][hdinsight-creation].</span><span class="sxs-lookup"><span data-stu-id="4fafd-194">For more information on configuring linked storage accounts, see [Create HDInsight clusters][hdinsight-creation].</span></span> <span data-ttu-id="4fafd-195">Alapértelmezett tárolókat azonban hello eredeti HDInsight fürt törlése után is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-195">However you can reuse a default storage container after hello original HDInsight cluster has been deleted.</span></span> <span data-ttu-id="4fafd-196">A HBase fürtök esetén megőrizheti hello HBase táblasémát és adatokat hozzon létre egy új HBase fürtöt a törölt HBase fürt által használt hello alapértelmezett blob tárolókat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-196">For HBase clusters, you can actually retain hello HBase table schema and data by creating a new HBase cluster using hello default blob container that is used by an HBase cluster that has been deleted.</span></span>

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

### <a name="use-hello-azure-portal"></a><span data-ttu-id="4fafd-197">Hello Azure portál használata</span><span class="sxs-lookup"><span data-stu-id="4fafd-197">Use hello Azure portal</span></span>
<span data-ttu-id="4fafd-198">Ha a HDInsight-fürtök létrehozása a portál hello, akkor hello lehetőségek (lásd alább), tooprovide hello tárfiókadatok.</span><span class="sxs-lookup"><span data-stu-id="4fafd-198">When creating an HDInsight cluster from hello Portal, you have hello options (as shown below) tooprovide hello storage account details.</span></span> <span data-ttu-id="4fafd-199">Megadhatja, hogy kívánja-e egy további tárfiók hello-fürthöz tartozó, és ha igen, a Data Lake Store vagy egy másik Azure Storage-blobba hello további tárolóként választhat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-199">You can also specify whether you want an additional storage account associated with hello cluster, and if so, choose from Data Lake Store or another Azure Storage blob as hello additional storage.</span></span>

![HDInsight hadoop létrehozási adatforrás](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

> [!WARNING]
> <span data-ttu-id="4fafd-201">Egy további storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fafd-201">Using an additional storage account in a different location than hello HDInsight cluster is not supported.</span></span>


### <a name="use-azure-powershell"></a><span data-ttu-id="4fafd-202">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="4fafd-202">Use Azure PowerShell</span></span>
<span data-ttu-id="4fafd-203">Ha Ön [telepítette és konfigurálta az Azure PowerShell][powershell-install], használhatja a tárfiók és tároló hello Azure PowerShell Rákérdezés toocreate követően hello:</span><span class="sxs-lookup"><span data-stu-id="4fafd-203">If you [installed and configured Azure PowerShell][powershell-install], you can use hello following from hello Azure PowerShell prompt toocreate a storage account and container:</span></span>

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

### <a name="use-azure-cli"></a><span data-ttu-id="4fafd-204">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="4fafd-204">Use Azure CLI</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

<span data-ttu-id="4fafd-205">Ha rendelkezik [telepítette és konfigurálta az Azure parancssori felület hello](../cli-install-nodejs.md), hello következő parancsot a használt tooa tárfiók és tároló lehet.</span><span class="sxs-lookup"><span data-stu-id="4fafd-205">If you have [installed and configured hello Azure CLI](../cli-install-nodejs.md), hello following command can be used tooa storage account and container.</span></span>

    azure storage account create <storageaccountname> --type LRS

> [!NOTE]
> <span data-ttu-id="4fafd-206">Hello `--type` paraméter azt jelzi, hogyan hello tárfiók replikálódik.</span><span class="sxs-lookup"><span data-stu-id="4fafd-206">hello `--type` parameter indicates how hello storage account is replicated.</span></span> <span data-ttu-id="4fafd-207">További információ: [Azure Storage replication](../storage/storage-redundancy.md) (Az Azure Storage replikációja).</span><span class="sxs-lookup"><span data-stu-id="4fafd-207">For more information, see [Azure Storage Replication](../storage/storage-redundancy.md).</span></span> <span data-ttu-id="4fafd-208">Na használjon ZRS-t, mivel a ZRS nem támogatja a lapblobokat, a fájlokat, a táblákat és az üzenetsorokat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-208">Don't use ZRS as ZRS doesn't support page blob, file, table, or queue.</span></span>
> 
> 

<span data-ttu-id="4fafd-209">Biztosan felszólító toospecify hello földrajzi régiót, amelyben hello storage-fiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="4fafd-209">You are prompted toospecify hello geographic region that hello storage account is created in.</span></span> <span data-ttu-id="4fafd-210">A hello készítsen hello storage-fiók ugyanabban a régióban, amely a HDInsight-fürt létrehozását tervezi.</span><span class="sxs-lookup"><span data-stu-id="4fafd-210">You should create hello storage account in hello same region that you plan on creating your HDInsight cluster.</span></span>

<span data-ttu-id="4fafd-211">Hello storage-fiók létrehozása után használja a következő parancs tooretrieve hello tárfiókkulcsok hello:</span><span class="sxs-lookup"><span data-stu-id="4fafd-211">Once hello storage account is created, use hello following command tooretrieve hello storage account keys:</span></span>

    azure storage account keys list <storageaccountname>

<span data-ttu-id="4fafd-212">egy tároló toocreate hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="4fafd-212">toocreate a container, use hello following command:</span></span>

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="address-files-in-azure-storage"></a><span data-ttu-id="4fafd-213">Az Azure Storage tárolóban található címfájlok</span><span class="sxs-lookup"><span data-stu-id="4fafd-213">Address files in Azure storage</span></span>
<span data-ttu-id="4fafd-214">hello URI-sémája a HDInsight-ból az Azure storage fájlokhoz való hozzáférést a következő:</span><span class="sxs-lookup"><span data-stu-id="4fafd-214">hello URI scheme for accessing files in Azure storage from HDInsight is:</span></span>

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>

<span data-ttu-id="4fafd-215">hello URI séma titkosítatlan hozzáférést biztosít (az hello *wasb:* előtag) és SSL titkosított hozzáférést (a *wasbs*).</span><span class="sxs-lookup"><span data-stu-id="4fafd-215">hello URI scheme provides unencrypted access (with hello *wasb:* prefix) and SSL encrypted access (with *wasbs*).</span></span> <span data-ttu-id="4fafd-216">Azt javasoljuk, *wasbs* lehetőség, akkor is, ha elérése során adatokat hello Azure-ban ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="4fafd-216">We recommend using *wasbs* wherever possible, even when accessing data that lives inside hello same region in Azure.</span></span>

<span data-ttu-id="4fafd-217">Hello &lt;BlobStorageContainerName&gt; hello blob tároló az Azure storage hello neve azonosítja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-217">hello &lt;BlobStorageContainerName&gt; identifies hello name of hello blob container in Azure storage.</span></span>
<span data-ttu-id="4fafd-218">Hello &lt;StorageAccountName&gt; hello Azure Storage-fiók neve azonosítja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-218">hello &lt;StorageAccountName&gt; identifies hello Azure Storage account name.</span></span> <span data-ttu-id="4fafd-219">Szükség van a teljes tartománynévre (FQDN-re).</span><span class="sxs-lookup"><span data-stu-id="4fafd-219">A fully qualified domain name (FQDN) is required.</span></span>

<span data-ttu-id="4fafd-220">Ha sem &lt;BlobStorageContainerName&gt; sem &lt;StorageAccountName&gt; van megadva, hello alapértelmezett fájlrendszert használja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-220">If neither &lt;BlobStorageContainerName&gt; nor &lt;StorageAccountName&gt; has been specified, hello default file system is used.</span></span> <span data-ttu-id="4fafd-221">Hello alapértelmezett fájlrendszeren hello fájlok esetén relatív elérési utat vagy abszolút elérési utat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4fafd-221">For hello files on hello default file system, you can use a relative path or an absolute path.</span></span> <span data-ttu-id="4fafd-222">Például hello *hadoop-mapreduce-examples.jar* fájl, a HDInsight-fürtök lehet hivatkozott tooby hello következő használatával:</span><span class="sxs-lookup"><span data-stu-id="4fafd-222">For example, hello *hadoop-mapreduce-examples.jar* file that comes with HDInsight clusters can be referred tooby using one of hello following:</span></span>

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="4fafd-223">hello Fájlnév <i>hadoop-examples.jar</i> HDInsight 2.1-es és 1.6-os fürtökben.</span><span class="sxs-lookup"><span data-stu-id="4fafd-223">hello file name is <i>hadoop-examples.jar</i> in HDInsight versions 2.1 and 1.6 clusters.</span></span>
> 
> 

<span data-ttu-id="4fafd-224">Hello &lt;elérési&gt; hello fájl vagy könyvtár HDFS elérési út.</span><span class="sxs-lookup"><span data-stu-id="4fafd-224">hello &lt;path&gt; is hello file or directory HDFS path name.</span></span> <span data-ttu-id="4fafd-225">Mivel az Azure Blob Storage tárolóban lévő tárolók egyszerűen kulcs-érték tárolók, nincs valós hierarchikus fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="4fafd-225">Because containers in Azure storage are simply key-value stores, there is no true hierarchical file system.</span></span> <span data-ttu-id="4fafd-226">A blob kulcson belüli perjel karaktert ( / ) a rendszer könyvtárelválasztóként értelmezi.</span><span class="sxs-lookup"><span data-stu-id="4fafd-226">A slash character ( / ) inside a blob key is interpreted as a directory separator.</span></span> <span data-ttu-id="4fafd-227">Például hello blob neve *hadoop-mapreduce-examples.jar* van:</span><span class="sxs-lookup"><span data-stu-id="4fafd-227">For example, hello blob name for *hadoop-mapreduce-examples.jar* is:</span></span>

    example/jars/hadoop-mapreduce-examples.jar

> [!NOTE]
> <span data-ttu-id="4fafd-228">HDInsight eszközön kívüli blobokkal dolgozik, amikor legtöbb segédprogram nem ismeri fel a WASB formátumot hello és helyette, mint az egy alapvető elérési út formátumot várhatóan `example/jars/hadoop-mapreduce-examples.jar`.</span><span class="sxs-lookup"><span data-stu-id="4fafd-228">When working with blobs outside of HDInsight, most utilities do not recognize hello WASB format and instead expect a basic path format, such as `example/jars/hadoop-mapreduce-examples.jar`.</span></span>
> 
> 

## <a name="access-blobs"></a><span data-ttu-id="4fafd-229">Blobok elérése</span><span class="sxs-lookup"><span data-stu-id="4fafd-229">Access blobs</span></span> 


### <span data-ttu-id="4fafd-230"><a name="access-blobs-using-azure-powershell"></a> Az Azure PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="4fafd-230"><a name="access-blobs-using-azure-powershell"></a> Use Azure PowerShell</span></span>
> [!NOTE]
> <span data-ttu-id="4fafd-231">Ezen szakasz parancsaiban hello adjon meg egy alapszintű példa blobok tárolt PowerShell tooaccess adatok használatára.</span><span class="sxs-lookup"><span data-stu-id="4fafd-231">hello commands in this section provide a basic example of using PowerShell tooaccess data stored in blobs.</span></span> <span data-ttu-id="4fafd-232">A HDInsight használatához testreszabott további teljes körű példa, lásd: hello [a HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span><span class="sxs-lookup"><span data-stu-id="4fafd-232">For a more full-featured example that is customized for working with HDInsight, see hello [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools).</span></span>
> 
> 

<span data-ttu-id="4fafd-233">A következő parancs toolist hello blobbal kapcsolatos parancsmagok hello használata:</span><span class="sxs-lookup"><span data-stu-id="4fafd-233">Use hello following command toolist hello blob-related cmdlets:</span></span>

    Get-Command *blob*

![A blobbal kapcsolatos PowerShell parancsmagok listája.][img-hdi-powershell-blobcommands]

#### <a name="upload-files"></a><span data-ttu-id="4fafd-235">Fájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="4fafd-235">Upload files</span></span>
<span data-ttu-id="4fafd-236">Lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].</span><span class="sxs-lookup"><span data-stu-id="4fafd-236">See [Upload data tooHDInsight][hdinsight-upload-data].</span></span>

#### <a name="download-files"></a><span data-ttu-id="4fafd-237">Fájlok letöltése</span><span class="sxs-lookup"><span data-stu-id="4fafd-237">Download files</span></span>
<span data-ttu-id="4fafd-238">hello következő parancsfájl letölti egy blokk blob toohello aktuális mappába.</span><span class="sxs-lookup"><span data-stu-id="4fafd-238">hello following script downloads a block blob toohello current folder.</span></span> <span data-ttu-id="4fafd-239">Hello a parancsfájl futtatását, mielőtt módosítani hello tooa könyvtármappát ahol írási jogosultsággal rendelkeznek-e.</span><span class="sxs-lookup"><span data-stu-id="4fafd-239">Before running hello script, change hello directory tooa folder where you have write permissions.</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # hello storage account used for hello default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # hello default file system container has hello same name as hello cluster.
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    # Use Add-AzureAccount if you haven't connected tooyour Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"

    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force

    Write-Host "List hello downloaded file ..." -ForegroundColor Green
    cat "./$blob"

<span data-ttu-id="4fafd-240">Hello erőforráscsoport-név és hello fürt nevének megadásával, hello a következő kódot használhatja:</span><span class="sxs-lookup"><span data-stu-id="4fafd-240">Providing hello resource group name and hello cluster name, you can use hello following code:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # hello name of hello blob toobe downloaded.

    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 

    Write-Host "Download hello blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force


#### <a name="delete-files"></a><span data-ttu-id="4fafd-241">Fájlok törlése</span><span class="sxs-lookup"><span data-stu-id="4fafd-241">Delete files</span></span>
    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

#### <a name="list-files"></a><span data-ttu-id="4fafd-242">Fájlok listázása</span><span class="sxs-lookup"><span data-stu-id="4fafd-242">List files</span></span>
    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

#### <a name="run-hive-queries-using-an-undefined-storage-account"></a><span data-ttu-id="4fafd-243">Hive-lekérdezések futtatása nem meghatározott tárfiókkal</span><span class="sxs-lookup"><span data-stu-id="4fafd-243">Run Hive queries using an undefined storage account</span></span>
<span data-ttu-id="4fafd-244">Ez a példa bemutatja, hogyan toolist során nem meghatározott tárfiókból mappa hello létrehozási folyamat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-244">This example shows how toolist a folder from storage account that is not defined during hello creating process.</span></span>
<span data-ttu-id="4fafd-245">$clusterName = "<HDInsightClusterName>"</span><span class="sxs-lookup"><span data-stu-id="4fafd-245">$clusterName = "<HDInsightClusterName>"</span></span>

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

### <a name="use-azure-cli"></a><span data-ttu-id="4fafd-246">Az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="4fafd-246">Use Azure CLI</span></span>
<span data-ttu-id="4fafd-247">A következő parancs toolist hello blobbal kapcsolatos parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="4fafd-247">Use hello following command toolist hello blob-related commands:</span></span>

    azure storage blob

<span data-ttu-id="4fafd-248">**Az Azure parancssori felület tooupload fájl használatának példája**</span><span class="sxs-lookup"><span data-stu-id="4fafd-248">**Example of using Azure CLI tooupload a file**</span></span>

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="4fafd-249">**Az Azure parancssori felület toodownload fájl használatának példája**</span><span class="sxs-lookup"><span data-stu-id="4fafd-249">**Example of using Azure CLI toodownload a file**</span></span>

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="4fafd-250">**Az Azure parancssori felület toodelete fájl használatának példája**</span><span class="sxs-lookup"><span data-stu-id="4fafd-250">**Example of using Azure CLI toodelete a file**</span></span>

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

<span data-ttu-id="4fafd-251">**Toolist fájlok az Azure parancssori felület használatának példája**</span><span class="sxs-lookup"><span data-stu-id="4fafd-251">**Example of using Azure CLI toolist files**</span></span>

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## <a name="use-additional-storage-accounts"></a><span data-ttu-id="4fafd-252">További tárfiókok használata</span><span class="sxs-lookup"><span data-stu-id="4fafd-252">Use additional storage accounts</span></span>

<span data-ttu-id="4fafd-253">HDInsight-fürtök létrehozásakor adja meg azt szeretné, hogy azt tooassociate hello Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="4fafd-253">While creating an HDInsight cluster, you specify hello Azure Storage account you want tooassociate with it.</span></span> <span data-ttu-id="4fafd-254">Ezenkívül toothis tárfiókot, hozzáadhat további tárfiókok a hello azonos Azure-előfizetés vagy különböző Azure-előfizetések hello létrehozási folyamata során, vagy a fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="4fafd-254">In addition toothis storage account, you can add additional storage accounts from hello same Azure subscription or different Azure subscriptions during hello creation process or after a cluster has been created.</span></span> <span data-ttu-id="4fafd-255">Útmutatás további tárfiókok hozzáadásához: [HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="4fafd-255">For instructions about adding additional storage accounts, see [Create HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md).</span></span>

> [!WARNING]
> <span data-ttu-id="4fafd-256">Egy további storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4fafd-256">Using an additional storage account in a different location than hello HDInsight cluster is not supported.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fafd-257">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4fafd-257">Next steps</span></span>
<span data-ttu-id="4fafd-258">Ebben a cikkben megtanulta, hogyan meg toouse HDFS-kompatibilis az Azure storage a hdinsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="4fafd-258">In this article, you learned how toouse HDFS-compatible Azure storage with HDInsight.</span></span> <span data-ttu-id="4fafd-259">Ez lehetővé teszi, hogy toobuild méretezhető, hosszú távú, archiválás beszerzési megoldások kiépítését és -felhasználási HDInsight toounlock hello lévő információkat hello tárolt strukturált és strukturálatlan adatokat.</span><span class="sxs-lookup"><span data-stu-id="4fafd-259">This allows you toobuild scalable, long-term, archiving data acquisition solutions and use HDInsight toounlock hello information inside hello stored structured and unstructured data.</span></span>

<span data-ttu-id="4fafd-260">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="4fafd-260">For more information, see:</span></span>

* <span data-ttu-id="4fafd-261">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="4fafd-261">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* [<span data-ttu-id="4fafd-262">Az Azure Data Lake Store használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="4fafd-262">Get started with Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-get-started-portal.md)
* <span data-ttu-id="4fafd-263">[Adatok tooHDInsight feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="4fafd-263">[Upload data tooHDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="4fafd-264">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="4fafd-264">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="4fafd-265">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="4fafd-265">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="4fafd-266">[Azure Storage megosztott hozzáférési aláírásokkal toorestrict hozzáférés toodata használata a hdinsight eszközzel][hdinsight-use-sas]</span><span class="sxs-lookup"><span data-stu-id="4fafd-266">[Use Azure Storage Shared Access Signatures toorestrict access toodata with HDInsight][hdinsight-use-sas]</span></span>

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
