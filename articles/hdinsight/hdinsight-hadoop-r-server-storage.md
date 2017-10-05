---
title: "Az R Server on HDInsight - Azure Azure tárolási megoldások |} Microsoft Docs"
description: "További információk a HDInsight az R Server rendelkező felhasználók számára elérhető más tárolási lehetőségek"
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
ms.openlocfilehash: aef9c15636ccaecce07d4fa218a40ed26ebad9df
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a><span data-ttu-id="c18b7-103">Az R Server on HDInsight az Azure tárolási megoldások</span><span class="sxs-lookup"><span data-stu-id="c18b7-103">Azure Storage solutions for R Server on HDInsight</span></span>

<span data-ttu-id="c18b7-104">Microsoft az R Server on HDInsight számos különféle tárolási megoldásokkal megőrizni az adatokat, kódok vagy elemzés eredményeinek tartalmazó objektumokra.</span><span class="sxs-lookup"><span data-stu-id="c18b7-104">Microsoft R Server on HDInsight has a variety of storage solutions to persist data, code, or objects that contain results from analysis.</span></span> <span data-ttu-id="c18b7-105">Ezek közé tartozik a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c18b7-105">These include the following options:</span></span>

- [<span data-ttu-id="c18b7-106">Az Azure Blob</span><span class="sxs-lookup"><span data-stu-id="c18b7-106">Azure Blob</span></span>](https://azure.microsoft.com/services/storage/blobs/)
- [<span data-ttu-id="c18b7-107">Az Azure Data Lake-tároló</span><span class="sxs-lookup"><span data-stu-id="c18b7-107">Azure Data Lake Storage</span></span>](https://azure.microsoft.com/services/data-lake-store/)
- [<span data-ttu-id="c18b7-108">Az Azure File storage</span><span class="sxs-lookup"><span data-stu-id="c18b7-108">Azure File storage</span></span>](https://azure.microsoft.com/services/storage/files/)

<span data-ttu-id="c18b7-109">Lehetősége is van a több Azure storage-fiókok vagy tárolók a HDI-fürtnek való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="c18b7-109">You also have the option of accessing multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="c18b7-110">Az Azure File storage az adatok tárolási lehetőség használható a peremhálózati csomóponton, amely lehetővé teszi, hogy csatlakoztassa egy Azure Storage-megosztás és, például a Linux fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="c18b7-110">Azure File storage is a convenient data storage option for use on the edge node that enables you to mount an Azure Storage file share to, for example, the Linux file system.</span></span> <span data-ttu-id="c18b7-111">Azonban az Azure fájlmegosztások csatlakoztatva, és a rendszer, amely rendelkezik egy támogatott operációs rendszer, például a Windows vagy Linux használja.</span><span class="sxs-lookup"><span data-stu-id="c18b7-111">But Azure File shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> 

<span data-ttu-id="c18b7-112">Amikor a HDInsight Hadoop-fürtöt hoz létre, vagy megadhatja egy **az Azure storage** fiók vagy egy **Data Lake store**.</span><span class="sxs-lookup"><span data-stu-id="c18b7-112">When you create an Hadoop cluster in HDInsight, you specify either an **Azure storage** account or a **Data Lake store**.</span></span> <span data-ttu-id="c18b7-113">Ebből a fiókból adott tárolót a fájlrendszer a fürt létrehozásakor (például a Hadoop elosztott fájlrendszerből) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c18b7-113">A specific storage container from that account holds the file system for the cluster that you create (for example, the Hadoop Distributed File System).</span></span> <span data-ttu-id="c18b7-114">További információt és útmutatást lásd:</span><span class="sxs-lookup"><span data-stu-id="c18b7-114">For more information and guidance, see:</span></span>

- [<span data-ttu-id="c18b7-115">Az Azure storage használata a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="c18b7-115">Use Azure storage with HDInsight</span></span>](hdinsight-hadoop-use-blob-storage.md)
- <span data-ttu-id="c18b7-116">[Használjon Data Lake Store az Azure HDInsight-fürtök](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="c18b7-116">[Use Data Lake Store with Azure HDInsight clusters](hdinsight-hadoop-use-data-lake-store.md).</span></span> 

<span data-ttu-id="c18b7-117">Az Azure tárolási megoldásait további információkért lásd: [Microsoft Azure Storage bemutatása](../storage/common/storage-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="c18b7-117">For more information on the Azure storage solutions, see [Introduction to Microsoft Azure Storage](../storage/common/storage-introduction.md).</span></span> 

<span data-ttu-id="c18b7-118">A használatára a forgatókönyvéhez leginkább megfelelő tárolási lehetőség kiválasztásával útmutatóért lásd: [való Azure BLOB, Azure-fájlok vagy Azure adatlemezek használata](../storage/common/storage-decide-blobs-files-disks.md)</span><span class="sxs-lookup"><span data-stu-id="c18b7-118">For guidance on selecting the most appropriate storage option to use for your scenario, see [Deciding when to use Azure Blobs, Azure Files, or Azure Data Disks](../storage/common/storage-decide-blobs-files-disks.md)</span></span> 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a><span data-ttu-id="c18b7-119">R Server Azure Blob storage-fiókok használata</span><span class="sxs-lookup"><span data-stu-id="c18b7-119">Use Azure Blob storage accounts with R Server</span></span>

<span data-ttu-id="c18b7-120">Ha szükséges, az Azure storage-fiókok vagy a tárolókat elérheti a HDI-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="c18b7-120">If necessary, you can access multiple Azure storage accounts or containers with your HDI cluster.</span></span> <span data-ttu-id="c18b7-121">Ehhez meg kell adnia a további tárfiókok a felhasználói felületen, amikor a fürt létrehozása, majd kövesse az alábbi lépéseket az R Server használandó.</span><span class="sxs-lookup"><span data-stu-id="c18b7-121">To do so, you need to specify the additional storage accounts in the UI when you create the cluster, and then follow these steps to use them with R Server.</span></span>

> [!WARNING]
> <span data-ttu-id="c18b7-122">Teljesítmény érdekében a HDInsight-fürt létrehozása a megadott elsődleges tárfiókkal azonos adatközpontba.</span><span class="sxs-lookup"><span data-stu-id="c18b7-122">For performance purposes, the HDInsight cluster is created in the same data center as the primary storage account that you specify.</span></span> <span data-ttu-id="c18b7-123">A storage-fiók egy másik helyen, mint a HDInsight-fürt használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="c18b7-123">Using a storage account in a different location than the HDInsight cluster is not supported.</span></span>

1. <span data-ttu-id="c18b7-124">A tárfiók neve a HDInsight-fürtök létrehozása **storage1** és egy alapértelmezett tároló nevű **container1**.</span><span class="sxs-lookup"><span data-stu-id="c18b7-124">Create an HDInsight cluster with a storage account name of **storage1** and a default container called **container1**.</span></span>
2. <span data-ttu-id="c18b7-125">Adjon meg egy további nevű tárfiókot **storage2**.</span><span class="sxs-lookup"><span data-stu-id="c18b7-125">Specify an additional storage account called **storage2**.</span></span>  
3. <span data-ttu-id="c18b7-126">Másolja a mycsv.csv fájlt a /share könyvtárba, és elvégezhetik az elemzést a fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="c18b7-126">Copy the mycsv.csv file to the /share directory, and perform analysis on that file.</span></span>  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. <span data-ttu-id="c18b7-127">Az R-kód beállítása a név csomópont **alapértelmezés szerint** és állítsa be a könyvtár- és feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="c18b7-127">In R code, set the name node to **default,** and set your directory and file to process.</span></span>  

        myNameNode <- "default"
        myPort <- 0

        #Location of the data:  
        bigDataDirRoot <- "/share"  

        #Define Spark compute context:
        mySparkCluster <- RxSpark(consoleOutput=TRUE)

        #Set compute context:
        rxSetComputeContext(mySparkCluster)

        #Define the Hadoop Distributed File System (HDFS) file system:
        hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

        #Specify the input file to analyze in HDFS:
        inputFile <-file.path(bigDataDirRoot,"mycsv.csv")

<span data-ttu-id="c18b7-128">A könyvtár- és hivatkozások mutasson a tárfiók wasb://container1@storage1.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c18b7-128">All the directory and file references point to the storage account wasb://container1@storage1.blob.core.windows.net.</span></span> <span data-ttu-id="c18b7-129">Ez a **alapértelmezett tárfiók** , amely a HDInsight-fürthöz van társítva.</span><span class="sxs-lookup"><span data-stu-id="c18b7-129">This is the **default storage account** that's associated with the HDInsight cluster.</span></span>

<span data-ttu-id="c18b7-130">Most tegyük fel szeretne dolgozni egy fájlt, amely a /private található mySpecial.csv nevű mappában található **container2** a **storage2**.</span><span class="sxs-lookup"><span data-stu-id="c18b7-130">Now, suppose you want to process a file called mySpecial.csv that's located in the  /private directory of **container2** in **storage2**.</span></span>

<span data-ttu-id="c18b7-131">Az R-kód, mutasson a csomópont hivatkozás a **storage2** tárfiók.</span><span class="sxs-lookup"><span data-stu-id="c18b7-131">In your R code, point the name node reference to the **storage2** storage account.</span></span>


    myNameNode <- "wasb://container2@storage2.blob.core.windows.net"
    myPort <- 0

    #Location of the data:
    bigDataDirRoot <- "/private"

    #Define Spark compute context:
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    #Set compute context:
    rxSetComputeContext(mySparkCluster)

    #Define HDFS file system:
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    #Specify the input file to analyze in HDFS:
    inputFile <-file.path(bigDataDirRoot,"mySpecial.csv")

<span data-ttu-id="c18b7-132">Az összes a könyvtár- és hivatkozást most mutasson a tárfiók wasb://container2@storage2.blob.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c18b7-132">All of the directory and file references now point to the storage account wasb://container2@storage2.blob.core.windows.net.</span></span> <span data-ttu-id="c18b7-133">Ez a **neve csomópont** megadott.</span><span class="sxs-lookup"><span data-stu-id="c18b7-133">This is the **Name Node** that you’ve specified.</span></span>

<span data-ttu-id="c18b7-134">Meg kell adnia a/User/RevoShare/<SSH username> könyvtárába **storage2** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c18b7-134">You have to configure the /user/RevoShare/<SSH username> directory on **storage2** as follows:</span></span>


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a><span data-ttu-id="c18b7-135">R Server az Azure Data Lake store használata</span><span class="sxs-lookup"><span data-stu-id="c18b7-135">Use an Azure Data Lake store with R Server</span></span>

<span data-ttu-id="c18b7-136">Data Lake-tárolók használata a HDInsight-fiókját, a fürt hozzáférést minden használni kívánt Azure Data Lake store kell.</span><span class="sxs-lookup"><span data-stu-id="c18b7-136">To use Data Lake stores with your HDInsight account, you need to give your cluster access to each Azure Data Lake store that you want to use.</span></span> <span data-ttu-id="c18b7-137">Az Azure portál használata a HDInsight-fürt létrehozása az alapértelmezett tároló vagy egy további tárolási Azure Data Lake Store-fiókkal, lásd: [HDInsight-fürtök létrehozása az Azure-portál használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c18b7-137">For instructions on how to use the Azure portal to create a HDInsight cluster with an Azure Data Lake Store account as the default storage or as an additional store, see [Create an HDInsight cluster with Data Lake Store using Azure portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

<span data-ttu-id="c18b7-138">Ezt követően az a tároló az R-parancsfájl a sokkal mint egy másodlagos Azure storage-fiók amilyet az előző eljárásban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="c18b7-138">You then use the store in your R script much like you did a secondary Azure storage account as described in the previous procedure.</span></span>

### <a name="add-cluster-access-to-your-azure-data-lake-stores"></a><span data-ttu-id="c18b7-139">Az Azure Data Lake-áruházak fürt hozzáférés hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c18b7-139">Add cluster access to your Azure Data Lake stores</span></span>
<span data-ttu-id="c18b7-140">A Data Lake store az Azure Active Directory (Azure AD) egyszerű szolgáltatásnév a HDInsight-fürthöz társított használatával éri el.</span><span class="sxs-lookup"><span data-stu-id="c18b7-140">You access a Data Lake store by using an Azure Active Directory (Azure AD) Service Principal that's associated with your HDInsight cluster.</span></span>

<span data-ttu-id="c18b7-141">Az Azure AD szolgáltatás egyszerű hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="c18b7-141">To add an Azure AD Service Principal:</span></span>

1. <span data-ttu-id="c18b7-142">A HDInsight-fürt létrehozásakor válassza ki a **fürt AAD-identitása** a a **adatforrás** fülre.</span><span class="sxs-lookup"><span data-stu-id="c18b7-142">When you create your HDInsight cluster, select **Cluster AAD Identity** from the **Data Source** tab.</span></span>

2. <span data-ttu-id="c18b7-143">Az a **fürt AAD-identitása** párbeszédpanel **válasszon AD egyszerű**, jelölje be **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="c18b7-143">In the **Cluster AAD Identity** dialog box, under **Select AD Service Principal**, select **Create new**.</span></span>

<span data-ttu-id="c18b7-144">Amikor nevezze el az egyszerű szolgáltatás, és hozzon létre egy jelszót az, kattintson a **ADLS-hozzáférés kezelése** a szolgáltatás egyszerű társítja a Data Lake tárolja.</span><span class="sxs-lookup"><span data-stu-id="c18b7-144">After you give the Service Principal a name and create a password for it, click **Manage ADLS Access** to associate the Service Principal with your Data Lake stores.</span></span>

<span data-ttu-id="c18b7-145">Akkor is a fürt létrehozása a következő egy vagy több Data Lake áruházak hozzáférés a fürthöz hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="c18b7-145">It’s also possible to add cluster access to one or more Data Lake stores following cluster creation.</span></span> <span data-ttu-id="c18b7-146">Nyissa meg a Data Lake store az Azure portál bejegyzésre, és navigáljon **adatkezelő > hozzáférés > Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c18b7-146">Open the Azure portal entry for a Data Lake store and go to **Data Explorer > Access > Add**.</span></span> 

### <a name="how-to-access-the-data-lake-store-from-r-server"></a><span data-ttu-id="c18b7-147">A Data Lake store elérése R kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="c18b7-147">How to access the Data Lake store from R Server</span></span>

<span data-ttu-id="c18b7-148">Után hozzáférést biztosított egy Data Lake Store-ba, használhatja a tárban R Server a HDInsight egy másodlagos Azure storage-fiók módon.</span><span class="sxs-lookup"><span data-stu-id="c18b7-148">Once you’ve given access to a Data Lake store, you can use the store in R Server on HDInsight the way you would a secondary Azure storage account.</span></span> <span data-ttu-id="c18b7-149">Az egyetlen különbség, hogy az előtag **wasb: / /** vált **adl: / /** az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c18b7-149">The only difference is that the prefix **wasb://** changes to **adl://** as follows:</span></span>


    # Point to the ADL store (e.g. ADLtest)
    myNameNode <- "adl://rkadl1.azuredatalakestore.net"
    myPort <- 0

    # Location of the data (assumes a /share directory on the ADL account)
    bigDataDirRoot <- "/share"  

    # Define Spark compute context
    mySparkCluster <- RxSpark(consoleOutput=TRUE, nameNode=myNameNode, port=myPort)

    # Set compute context
    rxSetComputeContext(mySparkCluster)

    # Define HDFS file system
    hdfsFS <- RxHdfsFileSystem(hostName=myNameNode, port=myPort)

    # Specify the input file in HDFS to analyze
    inputFile <-file.path(bigDataDirRoot,"AirlineDemoSmall.csv")

    # Create factors for days of the week
    colInfo <- list(DayOfWeek = list(type = "factor",
               levels = c("Monday", "Tuesday", "Wednesday", "Thursday",
                          "Friday", "Saturday", "Sunday")))

    # Define the data source
    airDS <- RxTextData(file = inputFile, missingValueString = "M",
                    colInfo  = colInfo, fileSystem = hdfsFS)

    # Run a linear regression
    model <- rxLinMod(ArrDelay~CRSDepTime+DayOfWeek, data = airDS)


<span data-ttu-id="c18b7-150">Az alábbi parancsokat a RevoShare directory konfigurálni a Data Lake-tárfiókot, és adja hozzá a minta .csv fájlt az előző példából használt:</span><span class="sxs-lookup"><span data-stu-id="c18b7-150">The following commands are used to configure the Data Lake storage account with the RevoShare directory and add the sample .csv file from the previous example:</span></span>


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a><span data-ttu-id="c18b7-151">R Server Azure File storage használata</span><span class="sxs-lookup"><span data-stu-id="c18b7-151">Use Azure File storage with R Server</span></span>

<span data-ttu-id="c18b7-152">Is az adatok tárolási lehetőségként használatra a peremhálózati csomóponton néven [Azure fájlok] ((https://azure.microsoft.com/services/storage/files/).</span><span class="sxs-lookup"><span data-stu-id="c18b7-152">There is also a convenient data storage option for use on the edge node called [Azure Files]((https://azure.microsoft.com/services/storage/files/).</span></span> <span data-ttu-id="c18b7-153">Lehetővé teszi egy Azure Storage-megosztás és a Linux fájlrendszer csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="c18b7-153">It enables you to mount an Azure Storage file share to the Linux file system.</span></span> <span data-ttu-id="c18b7-154">Lehet, hogy ez a beállítás lesz szüksége az adatfájlok, R parancsfájlok és esetleg szükséges később, különösen akkor, ha érdemes a fájlrendszer használatát a HDFS helyett a élcsomópont eredményobjektumok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="c18b7-154">This option can be handy for storing data files, R scripts, and result objects that might be needed later, especially when it makes sense to use the native file system on the edge node rather than HDFS.</span></span> 

<span data-ttu-id="c18b7-155">A fő Azure fájlok előnye, hogy a fájlmegosztások csatlakoztatott-e, és hogy a rendszer, amely rendelkezik egy támogatott operációs rendszer, például a Windows vagy Linux által használt.</span><span class="sxs-lookup"><span data-stu-id="c18b7-155">A major benefit of Azure Files is that the file shares can be mounted and used by any system that has a supported OS such as Windows or Linux.</span></span> <span data-ttu-id="c18b7-156">Például használhatná, amely rendelkezik a csoport vagy egy másik HDInsight-fürt által, egy Azure virtuális Gépen, vagy akár egy a helyszíni rendszer szerint.</span><span class="sxs-lookup"><span data-stu-id="c18b7-156">For example, it can be used by another HDInsight cluster that you or someone on your team has, by an Azure VM, or even by an on-premises system.</span></span> <span data-ttu-id="c18b7-157">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="c18b7-157">For more information, see:</span></span>

- [<span data-ttu-id="c18b7-158">Az Azure File Storage használata Linuxszal</span><span class="sxs-lookup"><span data-stu-id="c18b7-158">How to use Azure File storage with Linux</span></span>](../storage/files/storage-how-to-use-files-linux.md)
- [<span data-ttu-id="c18b7-159">A Windows Azure File storage használata</span><span class="sxs-lookup"><span data-stu-id="c18b7-159">How to use Azure File storage on Windows</span></span>](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a><span data-ttu-id="c18b7-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c18b7-160">Next steps</span></span>

<span data-ttu-id="c18b7-161">Most, hogy megismerkedett az Azure storage-beállítások, az alábbi hivatkozásokra számára az adatok tudományos feladatok az R Server on HDInsight használata módjait.</span><span class="sxs-lookup"><span data-stu-id="c18b7-161">Now that you understand the Azure storage options, use the following links to discover ways of getting data science tasks done with R Server on HDInsight.</span></span>

* [<span data-ttu-id="c18b7-162">Az R Server on HDInsight áttekintése</span><span class="sxs-lookup"><span data-stu-id="c18b7-162">Overview of R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-overview.md)
* [<span data-ttu-id="c18b7-163">Az R server, a Hadoop első lépései</span><span class="sxs-lookup"><span data-stu-id="c18b7-163">Get started with R server on Hadoop</span></span>](hdinsight-hadoop-r-server-get-started.md)
* [<span data-ttu-id="c18b7-164">Rstudióból kiszolgáló felvétele a HDInsight (Ha nincs hozzáadva a fürt létrehozása során)</span><span class="sxs-lookup"><span data-stu-id="c18b7-164">Add RStudio Server to HDInsight (if not added during cluster creation)</span></span>](hdinsight-hadoop-r-server-install-r-studio.md)
* [<span data-ttu-id="c18b7-165">Számítási környezeti beállítások a HDInsighton belüli R Server esetében</span><span class="sxs-lookup"><span data-stu-id="c18b7-165">Compute context options for R Server on HDInsight</span></span>](hdinsight-hadoop-r-server-compute-contexts.md)

