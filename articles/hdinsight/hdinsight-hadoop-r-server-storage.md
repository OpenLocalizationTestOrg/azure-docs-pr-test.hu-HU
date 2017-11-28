---
title: "az R Server on HDInsight - Azure tárolási megoldások aaaAzure |} Microsoft Docs"
description: "Hello eltérőek a tárolási beállítások elérhető toousers R Server on HDInsight megismerése"
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1cf30096-d3ca-45ea-b526-aa3954402f66
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/19/2017
ms.author: bradsev
ms.openlocfilehash: ff5e80fee14d5e74cbc22e873e6bc1439a3b6037
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a><span data-ttu-id="6c642-103">Az R Server on HDInsight az Azure tárolási megoldások</span><span class="sxs-lookup"><span data-stu-id="6c642-103">Azure Storage solutions for R Server on HDInsight</span></span>

<span data-ttu-id="6c642-104">A Microsoft az R Server on HDInsight számos különféle tárolási megoldások toopersist adatok, kódok vagy elemzés eredményeinek tartalmazó objektumokra.</span><span class="sxs-lookup"><span data-stu-id="6c642-104">Microsoft R Server on HDInsight has a variety of storage solutions toopersist data, code, or objects that contain results from analysis.</span></span> <span data-ttu-id="6c642-105">Ezek közé tartozik az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="6c642-105">These include hello following options:</span></span>

- [<span data-ttu-id="6c642-106">Az Azure Blob</span><span class="sxs-lookup"><span data-stu-id="6c642-106">Azure Blob</span></span>](https://azure.microsoft.com/services/storage/blobs/)
- [<span data-ttu-id="6c642-107">Az Azure Data Lake-tároló</span><span class="sxs-lookup"><span data-stu-id="6c642-107">Azure Data Lake Storage</span></span>](https://azure.microsoft.com/services/data-lake-store/)
- [<span data-ttu-id="6c642-108">Az Azure File storage</span><span class="sxs-lookup"><span data-stu-id="6c642-108">Azure File storage</span></span>](https://azure.microsoft.com/services/storage/files/)

<span data-ttu-id="6c642-109">Akkor is férnek hozzá a több Azure storage-fiókok vagy a HDI-fürtnek tárolók hello lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6c642-109">You also have hello option of accessing multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="6c642-110">Az Azure File storage az adatok tárolási lehetőség használatra, amely lehetővé teszi egy Azure Storage-megosztás és, például hello Linux fájlrendszer toomount hello peremhálózati csomóponton.</span><span class="sxs-lookup"><span data-stu-id="6c642-110">Azure File storage is a convenient data storage option for use on hello edge node that enables you toomount an Azure Storage file share to, for example, hello Linux file system.</span></span> <span data-ttu-id="6c642-111">Azonban az Azure fájlmegosztások csatlakoztatva, és a rendszer, amely rendelkezik egy támogatott operációs rendszer, például a Windows vagy Linux használja.</span><span class="sxs-lookup"><span data-stu-id="6c642-111">But Azure File shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> 

<span data-ttu-id="6c642-112">Amikor a HDInsight Hadoop-fürtöt hoz létre, vagy megadhatja egy **az Azure storage** fiók vagy egy **Data Lake store**.</span><span class="sxs-lookup"><span data-stu-id="6c642-112">When you create an Hadoop cluster in HDInsight, you specify either an **Azure storage** account or a **Data Lake store**.</span></span> <span data-ttu-id="6c642-113">Egy adott tároló ebből a fiókból hello fájlrendszer hello fürt létrehozásakor (például hello Hadoop elosztott fájlrendszerből) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6c642-113">A specific storage container from that account holds hello file system for hello cluster that you create (for example, hello Hadoop Distributed File System).</span></span> <span data-ttu-id="6c642-114">További információt és útmutatást lásd:</span><span class="sxs-lookup"><span data-stu-id="6c642-114">For more information and guidance, see:</span></span>

- [<span data-ttu-id="6c642-115">Az Azure storage használata a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="6c642-115">Use Azure storage with HDInsight</span></span>](hdinsight-hadoop-use-blob-storage.md)
- <span data-ttu-id="6c642-116">[Használjon Data Lake Store az Azure HDInsight-fürtök](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="6c642-116">[Use Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> 

<span data-ttu-id="6c642-117">Hello Azure tárolási megoldásairól további információkért lásd: [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="6c642-117">For more information on hello Azure storage solutions, see [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md).</span></span> 

<span data-ttu-id="6c642-118">Hello leginkább megfelelő tárolási lehetőség toouse helyzetnek kiválasztásával útmutatóért lásd: [Deciding, amikor toouse Azure Blobok, Azure-fájlok vagy Azure adatlemezek](../storage/common/storage-decide-blobs-files-disks.md)</span><span class="sxs-lookup"><span data-stu-id="6c642-118">For guidance on selecting hello most appropriate storage option toouse for your scenario, see [Deciding when toouse Azure Blobs, Azure Files, or Azure Data Disks](../storage/common/storage-decide-blobs-files-disks.md)</span></span> 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a><span data-ttu-id="6c642-119">R Server Azure Blob storage-fiókok használata</span><span class="sxs-lookup"><span data-stu-id="6c642-119">Use Azure Blob storage accounts with R Server</span></span>

<span data-ttu-id="6c642-120">Ha szükséges, az Azure storage-fiókok vagy a tárolókat elérheti a HDI-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6c642-120">If necessary, you can access multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="6c642-121">toodo hello fürt létrehozásakor, és kövesse az alábbi lépéseket toouse, így kell toospecify hello további tárfiókok hello felhasználói felület R Server őket.</span><span class="sxs-lookup"><span data-stu-id="6c642-121">toodo so, you need toospecify hello additional storage accounts in hello UI when you create hello cluster, and then follow these steps toouse them with R Server.</span></span>

> [!WARNING]
> <span data-ttu-id="6c642-122">Teljesítmény célokra hello HDInsight-fürt létrehozása a hello azonos adatközpontba, ahol megadott hello elsődleges tárfiók.</span><span class="sxs-lookup"><span data-stu-id="6c642-122">For performance purposes, hello HDInsight cluster is created in hello same data center as hello primary storage account that you specify.</span></span> <span data-ttu-id="6c642-123">A storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="6c642-123">Using a storage account in a different location than hello HDInsight cluster is not supported.</span></span>

1. <span data-ttu-id="6c642-124">A tárfiók neve a HDInsight-fürtök létrehozása **storage1** és egy alapértelmezett tároló nevű **container1**.</span><span class="sxs-lookup"><span data-stu-id="6c642-124">Create an HDInsight cluster with a storage account name of **storage1** and a default container called **container1**.</span></span>
2. <span data-ttu-id="6c642-125">Adjon meg egy további nevű tárfiókot **storage2**.</span><span class="sxs-lookup"><span data-stu-id="6c642-125">Specify an additional storage account called **storage2**.</span></span>  
3. <span data-ttu-id="6c642-126">Másolja a hello mycsv.csv toohello /share könyvtárát, és elvégezhetik az elemzést a fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="6c642-126">Copy hello mycsv.csv file toohello /share directory, and perform analysis on that file.</span></span>  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. <span data-ttu-id="6c642-127">Az R-kód beállítása hello neve csomópont túl**alapértelmezés szerint** és állítsa be a könyvtár- és tooprocess.</span><span class="sxs-lookup"><span data-stu-id="6c642-127">In R code, set hello name node too**default,** and set your directory and file tooprocess.</span></span>  

        myNameNode <- "default"
        myPort <- 0

        #Location of hello data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define hello Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify hello input file tooanalyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

<span data-ttu-id="6c642-128">Az összes könyvtár- és hivatkozások pont toohello tárfiók hello wasb://container1@storage1.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="6c642-128">All hello directory and file references point toohello storage account wasb://container1@storage1.blob.core.windows.net.</span></span> <span data-ttu-id="6c642-129">Ez a hello **alapértelmezett tárfiók** , amely hello HDInsight-fürthöz van társítva.</span><span class="sxs-lookup"><span data-stu-id="6c642-129">This is hello **default storage account** that's associated with hello HDInsight cluster.</span></span>

<span data-ttu-id="6c642-130">Most, akkor, tegyük fel, hogy tooprocess kívánt fájl neve, amely hello /private található mySpecial.csv mappában található **container2** a **storage2**.</span><span class="sxs-lookup"><span data-stu-id="6c642-130">Now, suppose you want tooprocess a file called mySpecial.csv that's located in hello  /private directory of **container2** in **storage2**.</span></span>

<span data-ttu-id="6c642-131">Az R-kód pontot hello neve csomópont hivatkozás toohello **storage2** tárfiók.</span><span class="sxs-lookup"><span data-stu-id="6c642-131">In your R code, point hello name node reference toohello **storage2** storage account.</span></span>


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of hello data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify hello input file tooanalyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

<span data-ttu-id="6c642-132">Az összes hello könyvtár- és hivatkozások most pont toohello tárfiók wasb://container2@storage2.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="6c642-132">All of hello directory and file references now point toohello storage account wasb://container2@storage2.blob.core.windows.net.</span></span> <span data-ttu-id="6c642-133">Ez a hello **neve csomópont** megadott.</span><span class="sxs-lookup"><span data-stu-id="6c642-133">This is hello **Name Node** that you’ve specified.</span></span>

<span data-ttu-id="6c642-134">Rendelkezik tooconfigure hello/User/RevoShare/<SSH username> könyvtárába **storage2** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6c642-134">You have tooconfigure hello /user/RevoShare/<SSH username> directory on **storage2** as follows:</span></span>


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a><span data-ttu-id="6c642-135">R Server az Azure Data Lake store használata</span><span class="sxs-lookup"><span data-stu-id="6c642-135">Use an Azure Data Lake store with R Server</span></span>

<span data-ttu-id="6c642-136">toouse Data Lake tárolja a HDInsight-fiókjával, a fürt hozzáférés tooeach Azure Data Lake store, amelyet az toouse toogive szüksége.</span><span class="sxs-lookup"><span data-stu-id="6c642-136">toouse Data Lake stores with your HDInsight account, you need toogive your cluster access tooeach Azure Data Lake store that you want toouse.</span></span> <span data-ttu-id="6c642-137">Egy Azure Data Lake Store-fiók hello alapértelmezett tároló vagy egy további tárolási utasításokat hogyan toouse hello Azure portál toocreate egy HDInsight-fürtöt, a következő témakörben: [HDInsight-fürtök létrehozása a Data Lake Store az Azure portálon](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="6c642-137">For instructions on how toouse hello Azure portal toocreate a HDInsight cluster with an Azure Data Lake Store account as hello default storage or as an additional store, see [Create an HDInsight cluster with Data Lake Store using Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

<span data-ttu-id="6c642-138">Ezt követően az hello tároló az R-parancsfájl a sokkal mint amilyet egy másodlagos Azure storage-fiók hello előző eljárásban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="6c642-138">You then use hello store in your R script much like you did a secondary Azure storage account as described in hello previous procedure.</span></span>

### <a name="add-cluster-access-tooyour-azure-data-lake-stores"></a><span data-ttu-id="6c642-139">Azure Data Lake tárolja a fürt hozzáférés tooyour hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6c642-139">Add cluster access tooyour Azure Data Lake stores</span></span>
<span data-ttu-id="6c642-140">A Data Lake store az Azure Active Directory (Azure AD) egyszerű szolgáltatásnév a HDInsight-fürthöz társított használatával éri el.</span><span class="sxs-lookup"><span data-stu-id="6c642-140">You access a Data Lake store by using an Azure Active Directory (Azure AD) Service Principal that's associated with your HDInsight cluster.</span></span>

<span data-ttu-id="6c642-141">az Azure AD szolgáltatás egyszerű tooadd:</span><span class="sxs-lookup"><span data-stu-id="6c642-141">tooadd an Azure AD Service Principal:</span></span>

1. <span data-ttu-id="6c642-142">A HDInsight-fürt létrehozásakor válassza ki a **fürt AAD-identitása** a hello **adatforrás** fülre.</span><span class="sxs-lookup"><span data-stu-id="6c642-142">When you create your HDInsight cluster, select **Cluster AAD Identity** from hello **Data Source** tab.</span></span>

2. <span data-ttu-id="6c642-143">A hello **fürt AAD-identitása** párbeszédpanel **válasszon AD egyszerű**, jelölje be **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="6c642-143">In hello **Cluster AAD Identity** dialog box, under **Select AD Service Principal**, select **Create new**.</span></span>

<span data-ttu-id="6c642-144">Miután hello szolgáltatás egyszerű adjon egy nevet, és hozzon létre egy jelszót az, kattintson a **ADLS-hozzáférés kezelése** tooassociate hello szolgáltatás egyszerű a Data Lake tárolja.</span><span class="sxs-lookup"><span data-stu-id="6c642-144">After you give hello Service Principal a name and create a password for it, click **Manage ADLS Access** tooassociate hello Service Principal with your Data Lake stores.</span></span>

<span data-ttu-id="6c642-145">Is lehetséges tooadd fürt hozzáférés tooone, vagy további Data Lake tárolja a fürt létrehozását követően.</span><span class="sxs-lookup"><span data-stu-id="6c642-145">It’s also possible tooadd cluster access tooone or more Data Lake stores following cluster creation.</span></span> <span data-ttu-id="6c642-146">Nyissa meg a hello egy Data Lake Store az Azure portál bejegyzést, és nyissa meg túl**adatkezelő > hozzáférés > Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6c642-146">Open hello Azure portal entry for a Data Lake store and go too**Data Explorer > Access > Add**.</span></span> 

### <a name="how-tooaccess-hello-data-lake-store-from-r-server"></a><span data-ttu-id="6c642-147">Hogyan tooaccess hello Data Lake store R kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="6c642-147">How tooaccess hello Data Lake store from R Server</span></span>

<span data-ttu-id="6c642-148">Hozzáférés tooa Data Lake store biztosított, ha a hello tároló R Server a HDInsight hello módon egy másodlagos Azure storage-fiók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="6c642-148">Once you’ve given access tooa Data Lake store, you can use hello store in R Server on HDInsight hello way you would a secondary Azure storage account.</span></span> <span data-ttu-id="6c642-149">hello az egyetlen különbség az, hogy hello előtag **wasb: / /** túl változik**adl: / /** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6c642-149">hello only difference is that hello prefix **wasb://** changes too**adl://** as follows:</span></span>


    # Point toohello ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of hello data (assumes a /share directory on hello ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify hello input file in HDFS tooanalyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of hello week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define hello data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


<span data-ttu-id="6c642-150">hello következő parancsok használt tooconfigure hello hello RevoShare Directory Data Lake-tárfiókot, és hozzá hello minta .csv fájl hello előző példa:</span><span class="sxs-lookup"><span data-stu-id="6c642-150">hello following commands are used tooconfigure hello Data Lake storage account with hello RevoShare directory and add hello sample .csv file from hello previous example:</span></span>


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a><span data-ttu-id="6c642-151">R Server Azure File storage használata</span><span class="sxs-lookup"><span data-stu-id="6c642-151">Use Azure File storage with R Server</span></span>

<span data-ttu-id="6c642-152">Is van adatok tárolási lehetőségként használatra hello peremhálózati csomóponton néven [Azure fájlok] ((https://azure.microsoft.com/services/storage/files/).</span><span class="sxs-lookup"><span data-stu-id="6c642-152">There is also a convenient data storage option for use on hello edge node called [Azure Files]((https://azure.microsoft.com/services/storage/files/).</span></span> <span data-ttu-id="6c642-153">Egy Azure Storage fájl megosztási toohello Linux fájlrendszer toomount lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="6c642-153">It enables you toomount an Azure Storage file share toohello Linux file system.</span></span> <span data-ttu-id="6c642-154">Lehet, hogy ez a beállítás lesz szüksége az adatfájlok, R parancsfájlok és esetleg szükséges később, különösen akkor, ha logika toouse hello natív fájlrendszer a HDFS helyett hello élcsomópont teszi eredményobjektumok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="6c642-154">This option can be handy for storing data files, R scripts, and result objects that might be needed later, especially when it makes sense toouse hello native file system on hello edge node rather than HDFS.</span></span> 

<span data-ttu-id="6c642-155">Egy Azure fájlok fő előnye, hogy hello fájl megosztások csatlakoztatva, és bármely, amely rendelkezik egy támogatott operációs rendszer, például a Windows vagy Linux rendszer által használt.</span><span class="sxs-lookup"><span data-stu-id="6c642-155">A major benefit of Azure Files is that hello file shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> <span data-ttu-id="6c642-156">Például használhatná, amely rendelkezik a csoport vagy egy másik HDInsight-fürt által, egy Azure virtuális Gépen, vagy akár egy a helyszíni rendszer szerint.</span><span class="sxs-lookup"><span data-stu-id="6c642-156">For example, it can be used by another HDInsight cluster that you or someone on your team has, by an Azure VM, or even by an on-premises system.</span></span> <span data-ttu-id="6c642-157">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="6c642-157">For more information, see:</span></span>

- [<span data-ttu-id="6c642-158">Hogyan toouse Azure File storage Linux</span><span class="sxs-lookup"><span data-stu-id="6c642-158">How toouse Azure File storage with Linux</span></span>](../storage/files/storage-how-to-use-files-linux.md)
- [<span data-ttu-id="6c642-159">Hogyan toouse a Windows Azure File storage</span><span class="sxs-lookup"><span data-stu-id="6c642-159">How toouse Azure File storage on Windows</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a><span data-ttu-id="6c642-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c642-160">Next steps</span></span>

<span data-ttu-id="6c642-161">Most, hogy megismerkedett hello Azure tárolási lehetőségek közül választhat, használjon hello következő toodiscover módokat adatok tudományos feladatok az R Server on HDInsight használata hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6c642-161">Now that you understand hello Azure storage options, use hello following links toodiscover ways of getting data science tasks done with R Server on HDInsight.</span></span>

* [<span data-ttu-id="6c642-162">Az R Server on HDInsight áttekintése</span><span class="sxs-lookup"><span data-stu-id="6c642-162">Overview of R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-overview.md)
* [<span data-ttu-id="6c642-163">Az R server, a Hadoop első lépései</span><span class="sxs-lookup"><span data-stu-id="6c642-163">Get started with R server on Hadoop</span></span>](hdinsight-hadoop-r-server-get-started.md)
* [<span data-ttu-id="6c642-164">Adja hozzá a Rstudióból Server tooHDInsight (Ha nem adja hozzá a fürt létrehozása során)</span><span class="sxs-lookup"><span data-stu-id="6c642-164">Add RStudio Server tooHDInsight (if not added during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="6c642-165">Számítási környezeti beállítások a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="6c642-165">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)

