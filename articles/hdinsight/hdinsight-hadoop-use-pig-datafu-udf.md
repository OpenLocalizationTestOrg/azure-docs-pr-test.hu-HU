---
title: "DataFu használata a HDInsight - Azure Pig |} Microsoft Docs"
description: "DataFu gyűjteménye szalagtárak Hadoop való használatra. Ismerje meg, hogyan használható DataFu Pig a HDInsight-fürtre."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 0016721a-82be-4773-88ad-91e6b2c21cbb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 4de55f5f6c5605e9c6c8dd7ccac902b811d1b062
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-datafu-with-pig-on-hdinsight"></a><span data-ttu-id="1480f-104">A HDInsight pig DataFu használata</span><span class="sxs-lookup"><span data-stu-id="1480f-104">Use DataFu with pig on HDInsight</span></span>

<span data-ttu-id="1480f-105">Megtudhatja, hogyan használhat DataFu a hdinsight eszközzel.</span><span class="sxs-lookup"><span data-stu-id="1480f-105">Learn how to use DataFu with HDInsight.</span></span> <span data-ttu-id="1480f-106">DataFu gyűjteményei, nyílt forráskódú szalagtárak Pig hadoop való használatra.</span><span class="sxs-lookup"><span data-stu-id="1480f-106">DataFu is a collection of Open Source libraries for use with Pig on Hadoop.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1480f-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1480f-107">Prerequisites</span></span>

* <span data-ttu-id="1480f-108">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="1480f-108">An Azure subscription.</span></span>

* <span data-ttu-id="1480f-109">Azure HDInsight-fürtök (Linux- vagy Windows-alapú)</span><span class="sxs-lookup"><span data-stu-id="1480f-109">An Azure HDInsight cluster (Linux or Windows based)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="1480f-110">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="1480f-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="1480f-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="1480f-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="1480f-112">Egy alapszintű ismeretét [Pig használata a HDInsight-on](hdinsight-use-pig.md)</span><span class="sxs-lookup"><span data-stu-id="1480f-112">A basic familiarity with [using Pig on HDInsight](hdinsight-use-pig.md)</span></span>

## <a name="install-datafu-on-linux-based-hdinsight"></a><span data-ttu-id="1480f-113">A Linux-alapú HDInsight DataFu telepítése</span><span class="sxs-lookup"><span data-stu-id="1480f-113">Install DataFu on Linux-based HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1480f-114">DataFu telepítve van a Linux-alapú fürtökön verzióján 3.3 és magasabb, és a Windows-alapú fürtökön.</span><span class="sxs-lookup"><span data-stu-id="1480f-114">DataFu is installed on Linux-based clusters version 3.3 and higher, and on Windows-based clusters.</span></span> <span data-ttu-id="1480f-115">Nem telepíti a rendszer a Linux-alapú fürtökön 3.3-as verziójánál.</span><span class="sxs-lookup"><span data-stu-id="1480f-115">It is not installed on Linux-based clusters earlier than 3.3.</span></span>
>
> <span data-ttu-id="1480f-116">Ha egy Windows-alapú fürt, vagy nagyobb, mint 3.3-as verzió Linux-alapú fürt használ, hagyja ki az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="1480f-116">If you are using a Windows-based cluster, or a Linux-based cluster higher than version 3.3, skip this section.</span></span>

<span data-ttu-id="1480f-117">DataFu letölthető, és a Maven tárházból telepítve.</span><span class="sxs-lookup"><span data-stu-id="1480f-117">DataFu can be downloaded and installed from the Maven repository.</span></span> <span data-ttu-id="1480f-118">Az alábbi lépések segítségével DataFu hozzáadása a HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="1480f-118">Use the following steps to add DataFu to your HDInsight cluster:</span></span>

1. <span data-ttu-id="1480f-119">Csatlakoztassa a Linux-alapú HDInsight-fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="1480f-119">Connect to your Linux-based HDInsight cluster using SSH.</span></span> <span data-ttu-id="1480f-120">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="1480f-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="1480f-121">Az alábbi parancs segítségével töltse le a wget segédprogrammal DataFu jar-fájlra vagy másolja és illessze be a hivatkozást a böngészőbe a letöltés megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="1480f-121">Use the following command to download the DataFu jar file using the wget utility, or copy and paste the link into your browser to begin the download.</span></span>

    ```
    wget http://central.maven.org/maven2/com/linkedin/datafu/datafu/1.2.0/datafu-1.2.0.jar
    ```

3. <span data-ttu-id="1480f-122">A következő feltölteni a fájlt az alapértelmezett storage a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1480f-122">Next, upload the file to default storage for your HDInsight cluster.</span></span> <span data-ttu-id="1480f-123">Helyezi el a fájlt az alapértelmezett tároló lehetővé teszi az összes csomópont a fürtben.</span><span class="sxs-lookup"><span data-stu-id="1480f-123">Placing the file in default storage makes it available to all nodes in the cluster.</span></span>

    ```
    hdfs dfs -put datafu-1.2.0.jar /example/jars
    ```

    > [!NOTE]
    > <span data-ttu-id="1480f-124">Az előző parancs tárolja a jar `/example/jars` mivel ez a könyvtár már létezik a fürttároló.</span><span class="sxs-lookup"><span data-stu-id="1480f-124">The previous command stores the jar in `/example/jars` since this directory already exists on the cluster storage.</span></span> <span data-ttu-id="1480f-125">A HDInsight fürt tárolón kívánja bárhol használhatja.</span><span class="sxs-lookup"><span data-stu-id="1480f-125">You can use any location you wish on HDInsight cluster storage.</span></span>

## <a name="use-datafu-with-pig"></a><span data-ttu-id="1480f-126">A Pig DataFu használata</span><span class="sxs-lookup"><span data-stu-id="1480f-126">Use DataFu With Pig</span></span>

<span data-ttu-id="1480f-127">Ebben a szakaszban a lépések feltételezik, hogy ismeri a Pig használata a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="1480f-127">The steps in this section assume that you are familiar with using Pig on HDInsight.</span></span> <span data-ttu-id="1480f-128">A Pig használata a HDInsight további információkért lásd: [a Pig használata a hdinsight eszközzel](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="1480f-128">For more information on using Pig with HDInsight, see [Use Pig with HDInsight](hdinsight-use-pig.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1480f-129">Ha manuálisan telepíti az előző szakaszban leírtak DataFu, akkor regisztrálnia kell azt annak használata előtt.</span><span class="sxs-lookup"><span data-stu-id="1480f-129">If you manually installed DataFu using the steps in the previous section, you must register it before using it.</span></span>
>
> * <span data-ttu-id="1480f-130">Ha a fürt Azure tárterületet használja, használja a `wasb://` elérési útja.</span><span class="sxs-lookup"><span data-stu-id="1480f-130">If your cluster uses Azure Storage, use a `wasb://` path.</span></span> <span data-ttu-id="1480f-131">Például: `register wasb:///example/jars/datafu-1.2.0.jar`.</span><span class="sxs-lookup"><span data-stu-id="1480f-131">For example, `register wasb:///example/jars/datafu-1.2.0.jar`.</span></span>
>
> * <span data-ttu-id="1480f-132">Ha a fürt az Azure Data Lake Store használ, egy `adl://` elérési útja.</span><span class="sxs-lookup"><span data-stu-id="1480f-132">If your cluster uses Azure Data Lake Store, use an `adl://` path.</span></span> <span data-ttu-id="1480f-133">Például: `register adl://home/example/jars/datafu-1.2.0.jar`.</span><span class="sxs-lookup"><span data-stu-id="1480f-133">For example, `register adl://home/example/jars/datafu-1.2.0.jar`.</span></span>

<span data-ttu-id="1480f-134">Gyakran határozza meg az DataFu függvény aliasa.</span><span class="sxs-lookup"><span data-stu-id="1480f-134">You often define an alias for DataFu functions.</span></span> <span data-ttu-id="1480f-135">Az alábbi példa meghatározza az alias `SHA`:</span><span class="sxs-lookup"><span data-stu-id="1480f-135">The following example defines an alias of `SHA`:</span></span>

```piglatin
DEFINE SHA datafu.pig.hash.SHA();
```

<span data-ttu-id="1480f-136">A bemeneti adatok a kivonat létrehozásához használhatja ezt az aliast a Pig Latin parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="1480f-136">You can then use this alias in a Pig Latin script to generate a hash for the input data.</span></span> <span data-ttu-id="1480f-137">Például az alábbi kód kivonatot lecseréli a bemeneti adatok helye:</span><span class="sxs-lookup"><span data-stu-id="1480f-137">For example, the following code replaces the location in the input data with a hash value:</span></span>

```piglatin
raw = LOAD '/HdiSamples/HdiSamples/SensorSampleData/building/building.csv' USING
    org.apache.pig.piggybank.storage.CSVExcelStorage(',', 'NO_MULTILINE', 'UNIX', 'SKIP_INPUT_HEADER') AS
    (int1:int,
     id1:chararray,
     int2:int,
     id2:chararray,
     location:chararray);
mask = FOREACH raw GENERATE int1, id1, int2, id2, SHA(location);
DUMP mask;
```

<span data-ttu-id="1480f-138">A következő kimenetet hoz létre:</span><span class="sxs-lookup"><span data-stu-id="1480f-138">It generates the following output:</span></span>

    (1,M1,25,AC1000,aa5ab35a9174c2062b7f7697b33fafe5ce404cf5fecf6bfbbf0dc96ba0d90046)
    (2,M2,27,FN39TG,7a1ca4ef7515f7276bae7230545829c27810c9d9e98ab2c06066bee6270d5153)
    (3,M3,28,JDNS77,07f62b021771d3cf67e2e1faf18769cc5e5c119ad7d4d1847a11e11d6d5a7ecb)
    (4,M4,17,GG1919,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (5,M5,3,ACMAX22,1ed8c7e56c947bebc0cfcf88c4ea0f02c944568f71df893a99970e4f0c78cddc)
    (6,M6,9,AC1000,c96dd81db196cca5f57bd4270bbb9d9e9d1b242d67f9364005ee1dfdc2632523)
    (7,M7,13,FN39TG,3e049d78d958038ac6bd5dcf038075cc73362b4956aaf7308c3a69c8eca76297)
    (8,M8,25,JDNS77,c1ef40ce0484c698eb4bd27fe56c1e7b68d74f9780ed674210d0e5013dae45e9)
    (9,M9,11,GG1919,a7d355b26bda6bf1196ccffead0b2cf2b81f0a9de5b4876b44407f1dc07e51e6)
    (10,M10,23,ACMAX22,10436829032f361a3de50048de41755140e581467bc1895e6c1a17f423e42d10)
    (11,M11,14,AC1000,348841ef53dbd5a64008a86be432973db790774fb28b59b0d99702a3188b3705)
    (12,M12,26,FN39TG,aed8f531aa7e785be255bc435e2582e74c58defedebcb629eeabf365b809bd6f)
    (13,M13,25,JDNS77,ed9ad13611d7164839dd3feaf9a7f6a75a4138f389e0d45f8e07fa38da1116a2)
    (14,M14,17,GG1919,80db4ccdca106d37b920206331fcfe3e9e50a9e763d89b54ce3ad5ac8cf30f03)
    (15,M15,19,ACMAX22,3552ac0c032467fbf592cb4d10c5c9267800c01e343ad8ca557256d882ae9327)
    (16,M16,23,AC1000,07c67d76ef92ac9853588e098983fefba4ba5965f8fec95f42ab0d04c27865ba)
    (17,M17,11,FN39TG,557c1599d9a04895d3817d293e0806a4419a14de31958386182798d0d2ed3a56)
    (18,M18,25,JDNS77,dbc74a36d8e7439c45c64d856388506cc9b1218619cef979c3d605115a7a4546)
    (19,M19,14,GG1919,be55ef3f4c4e6c2d9c2afe2a33ac90ad0f50d4de7f9163999877e2a9ca5a54f8)
    (20,M20,19,ACMAX22,ea0b937ea317101ee2c26b03a4843a19ceced8a2b9673c3cf409a726ca2b0fd8)

## <a name="next-steps"></a><span data-ttu-id="1480f-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1480f-139">Next steps</span></span>

<span data-ttu-id="1480f-140">DataFu vagy Pig további információkért lásd a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="1480f-140">For more information on DataFu or Pig, see the following documents:</span></span>

* <span data-ttu-id="1480f-141">[Apache DataFu Pig útmutató](http://datafu.incubator.apache.org/docs/datafu/guide.html).</span><span class="sxs-lookup"><span data-stu-id="1480f-141">[Apache DataFu Pig Guide](http://datafu.incubator.apache.org/docs/datafu/guide.html).</span></span>
* [<span data-ttu-id="1480f-142">A Pig használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="1480f-142">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
