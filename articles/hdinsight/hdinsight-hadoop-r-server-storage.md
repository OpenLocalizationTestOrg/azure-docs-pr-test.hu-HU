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
# <a name="azure-storage-solutions-for-r-server-on-hdinsight"></a>Az R Server on HDInsight az Azure tárolási megoldások

A Microsoft az R Server on HDInsight számos különféle tárolási megoldások toopersist adatok, kódok vagy elemzés eredményeinek tartalmazó objektumokra. Ezek közé tartozik az alábbi beállítások hello:

- [Az Azure Blob](https://azure.microsoft.com/services/storage/blobs/)
- [Az Azure Data Lake-tároló](https://azure.microsoft.com/services/data-lake-store/)
- [Az Azure File storage](https://azure.microsoft.com/services/storage/files/)

Akkor is férnek hozzá a több Azure storage-fiókok vagy a HDI-fürtnek tárolók hello lehetőséget. Az Azure File storage az adatok tárolási lehetőség használatra, amely lehetővé teszi egy Azure Storage-megosztás és, például hello Linux fájlrendszer toomount hello peremhálózati csomóponton. Azonban az Azure fájlmegosztások csatlakoztatva, és a rendszer, amely rendelkezik egy támogatott operációs rendszer, például a Windows vagy Linux használja. 

Amikor a HDInsight Hadoop-fürtöt hoz létre, vagy megadhatja egy **az Azure storage** fiók vagy egy **Data Lake store**. Egy adott tároló ebből a fiókból hello fájlrendszer hello fürt létrehozásakor (például hello Hadoop elosztott fájlrendszerből) tartalmazza. További információt és útmutatást lásd:

- [Az Azure storage használata a hdinsight eszközzel](hdinsight-hadoop-use-blob-storage.md)
- [Használjon Data Lake Store az Azure HDInsight-fürtök](hdinsight-hadoop-use-data-lake-store.md). 

Hello Azure tárolási megoldásairól további információkért lásd: [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md). 

Hello leginkább megfelelő tárolási lehetőség toouse helyzetnek kiválasztásával útmutatóért lásd: [Deciding, amikor toouse Azure Blobok, Azure-fájlok vagy Azure adatlemezek](../storage/common/storage-decide-blobs-files-disks.md) 


## <a name="use-azure-blob-storage-accounts-with-r-server"></a>R Server Azure Blob storage-fiókok használata

Ha szükséges, az Azure storage-fiókok vagy a tárolókat elérheti a HDI-fürthöz. toodo hello fürt létrehozásakor, és kövesse az alábbi lépéseket toouse, így kell toospecify hello további tárfiókok hello felhasználói felület R Server őket.

> [!WARNING]
> Teljesítmény célokra hello HDInsight-fürt létrehozása a hello azonos adatközpontba, ahol megadott hello elsődleges tárfiók. A storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott.

1. A tárfiók neve a HDInsight-fürtök létrehozása **storage1** és egy alapértelmezett tároló nevű **container1**.
2. Adjon meg egy további nevű tárfiókot **storage2**.  
3. Másolja a hello mycsv.csv toohello /share könyvtárát, és elvégezhetik az elemzést a fájlhoz.  

        hadoop fs –mkdir /share
        hadoop fs –copyFromLocal myscsv.scv /share  

4. Az R-kód beállítása hello neve csomópont túl**alapértelmezés szerint** és állítsa be a könyvtár- és tooprocess.  

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

Az összes könyvtár- és hivatkozások pont toohello tárfiók hello wasb://container1@storage1.blob.core.windows.net. Ez a hello **alapértelmezett tárfiók** , amely hello HDInsight-fürthöz van társítva.

Most, akkor, tegyük fel, hogy tooprocess kívánt fájl neve, amely hello /private található mySpecial.csv mappában található **container2** a **storage2**.

Az R-kód pontot hello neve csomópont hivatkozás toohello **storage2** tárfiók.


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

Az összes hello könyvtár- és hivatkozások most pont toohello tárfiók wasb://container2@storage2.blob.core.windows.net. Ez a hello **neve csomópont** megadott.

Rendelkezik tooconfigure hello/User/RevoShare/<SSH username> könyvtárába **storage2** az alábbiak szerint:


    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare
    hadoop fs -mkdir wasb://container2@storage2.blob.core.windows.net/user/RevoShare/<RDP username>



## <a name="use-an-azure-data-lake-store-with-r-server"></a>R Server az Azure Data Lake store használata

toouse Data Lake tárolja a HDInsight-fiókjával, a fürt hozzáférés tooeach Azure Data Lake store, amelyet az toouse toogive szüksége. Egy Azure Data Lake Store-fiók hello alapértelmezett tároló vagy egy további tárolási utasításokat hogyan toouse hello Azure portál toocreate egy HDInsight-fürtöt, a következő témakörben: [HDInsight-fürtök létrehozása a Data Lake Store az Azure portálon](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

Ezt követően az hello tároló az R-parancsfájl a sokkal mint amilyet egy másodlagos Azure storage-fiók hello előző eljárásban leírtak szerint.

### <a name="add-cluster-access-tooyour-azure-data-lake-stores"></a>Azure Data Lake tárolja a fürt hozzáférés tooyour hozzáadása
A Data Lake store az Azure Active Directory (Azure AD) egyszerű szolgáltatásnév a HDInsight-fürthöz társított használatával éri el.

az Azure AD szolgáltatás egyszerű tooadd:

1. A HDInsight-fürt létrehozásakor válassza ki a **fürt AAD-identitása** a hello **adatforrás** fülre.

2. A hello **fürt AAD-identitása** párbeszédpanel **válasszon AD egyszerű**, jelölje be **hozzon létre új**.

Miután hello szolgáltatás egyszerű adjon egy nevet, és hozzon létre egy jelszót az, kattintson a **ADLS-hozzáférés kezelése** tooassociate hello szolgáltatás egyszerű a Data Lake tárolja.

Is lehetséges tooadd fürt hozzáférés tooone, vagy további Data Lake tárolja a fürt létrehozását követően. Nyissa meg a hello egy Data Lake Store az Azure portál bejegyzést, és nyissa meg túl**adatkezelő > hozzáférés > Hozzáadás**. 

### <a name="how-tooaccess-hello-data-lake-store-from-r-server"></a>Hogyan tooaccess hello Data Lake store R kiszolgálóról

Hozzáférés tooa Data Lake store biztosított, ha a hello tároló R Server a HDInsight hello módon egy másodlagos Azure storage-fiók is használhatja. hello az egyetlen különbség az, hogy hello előtag **wasb: / /** túl változik**adl: / /** az alábbiak szerint:


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


hello következő parancsok használt tooconfigure hello hello RevoShare Directory Data Lake-tárfiókot, és hozzá hello minta .csv fájl hello előző példa:


    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare
    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/user/RevoShare/<user>

    hadoop fs -mkdir adl://rkadl1.azuredatalakestore.net/share

    hadoop fs -copyFromLocal /usr/lib64/R Server-7.4.1/library/RevoScaleR/SampleData/AirlineDemoSmall.csv adl://rkadl1.azuredatalakestore.net/share

    hadoop fs –ls adl://rkadl1.azuredatalakestore.net/share


## <a name="use-azure-file-storage-with-r-server"></a>R Server Azure File storage használata

Is van adatok tárolási lehetőségként használatra hello peremhálózati csomóponton néven [Azure fájlok] ((https://azure.microsoft.com/services/storage/files/). Egy Azure Storage fájl megosztási toohello Linux fájlrendszer toomount lehetővé teszi. Lehet, hogy ez a beállítás lesz szüksége az adatfájlok, R parancsfájlok és esetleg szükséges később, különösen akkor, ha logika toouse hello natív fájlrendszer a HDFS helyett hello élcsomópont teszi eredményobjektumok tárolásához. 

Egy Azure fájlok fő előnye, hogy hello fájl megosztások csatlakoztatva, és bármely, amely rendelkezik egy támogatott operációs rendszer, például a Windows vagy Linux rendszer által használt. Például használhatná, amely rendelkezik a csoport vagy egy másik HDInsight-fürt által, egy Azure virtuális Gépen, vagy akár egy a helyszíni rendszer szerint. További információkért lásd:

- [Hogyan toouse Azure File storage Linux](../storage/files/storage-how-to-use-files-linux.md)
- [Hogyan toouse a Windows Azure File storage](../storage/files/storage-dotnet-how-to-use-files.md)


## <a name="next-steps"></a>Következő lépések

Most, hogy megismerkedett hello Azure tárolási lehetőségek közül választhat, használjon hello következő toodiscover módokat adatok tudományos feladatok az R Server on HDInsight használata hivatkozásokat tartalmaz.

* [Az R Server on HDInsight áttekintése](hdinsight-hadoop-r-server-overview.md)
* [Az R server, a Hadoop első lépései](hdinsight-hadoop-r-server-get-started.md)
* [Adja hozzá a Rstudióból Server tooHDInsight (Ha nem adja hozzá a fürt létrehozása során)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Számítási környezeti beállítások a HDInsighton belüli R Server esetében](hdinsight-hadoop-r-server-compute-contexts.md)

