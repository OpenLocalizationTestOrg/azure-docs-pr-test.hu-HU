---
title: "A Hive-lekérdezéseket a Hive táblák adatokba |} Microsoft Docs"
description: "A Hive-lekérdezésekkel Hive táblák adatokba."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0d46cea5-2b4c-4384-9bfa-fa20f6f75148
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 67a33a9abc3d3dcdd2fc7205e11feff97e3582a3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a><span data-ttu-id="4e79a-103">A Hive-táblákban tárolt adatok megismerése Hive-lekérdezésekkel</span><span class="sxs-lookup"><span data-stu-id="4e79a-103">Explore data in Hive tables with Hive queries</span></span>
<span data-ttu-id="4e79a-104">Ez a dokumentum minta Hive parancsfájlok, amely segítségével a Hive táblák egy HDInsight Hadoop-fürt adatokba nyújt.</span><span class="sxs-lookup"><span data-stu-id="4e79a-104">This document provides sample Hive scripts that are used to explore data in Hive tables in an HDInsight Hadoop cluster.</span></span>

<span data-ttu-id="4e79a-105">A következő **menü** eszközök segítségével áttekintheti az különböző tárolási környezetekben adatokat leíró témakörökre mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4e79a-105">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="4e79a-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4e79a-106">Prerequisites</span></span>
<span data-ttu-id="4e79a-107">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="4e79a-107">This article assumes that you have:</span></span>

* <span data-ttu-id="4e79a-108">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="4e79a-108">Created an Azure storage account.</span></span> <span data-ttu-id="4e79a-109">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="4e79a-109">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="4e79a-110">A HDInsight szolgáltatásban egy testreszabott Hadoop-fürt üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="4e79a-110">Provisioned a customized Hadoop cluster with the HDInsight service.</span></span> <span data-ttu-id="4e79a-111">Ha módosítania kell az utasításokat, lásd: [testreszabása Azure HDInsight Hadoop-fürtök az Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="4e79a-111">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="4e79a-112">Az adatok az Azure HDInsight Hadoop-fürtök Hive táblák fel lett töltve.</span><span class="sxs-lookup"><span data-stu-id="4e79a-112">The data has been uploaded to Hive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="4e79a-113">Ha még nem, kövesse az utasításokat a [létrehozása és az adatok betöltése a Hive táblák](machine-learning-data-science-move-hive-tables.md) feltölteni az adatokat a Hive táblák először.</span><span class="sxs-lookup"><span data-stu-id="4e79a-113">If it has not, follow the instructions in [Create and load data to Hive tables](machine-learning-data-science-move-hive-tables.md) to upload data to Hive tables first.</span></span>
* <span data-ttu-id="4e79a-114">Engedélyezve van a fürt távoli eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="4e79a-114">Enabled remote access to the cluster.</span></span> <span data-ttu-id="4e79a-115">Ha módosítania kell az utasításokat, lásd: [a Head csomópont a Hadoop-fürt eléréséhez](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="4e79a-115">If you need instructions, see [Access the Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>
* <span data-ttu-id="4e79a-116">Ha útmutatást nyújt a Hive-lekérdezések van szüksége, tekintse meg [hogyan küldhetnek Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit)</span><span class="sxs-lookup"><span data-stu-id="4e79a-116">If you need instructions on how to submit Hive queries, see [How to Submit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit)</span></span>

## <a name="example-hive-query-scripts-for-data-exploration"></a><span data-ttu-id="4e79a-117">Példa Hive lekérdezés parancsfájlok adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="4e79a-117">Example Hive query scripts for data exploration</span></span>
1. <span data-ttu-id="4e79a-118">A szám megfigyelések partíciónként`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span><span class="sxs-lookup"><span data-stu-id="4e79a-118">Get the count of observations per partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span></span>
2. <span data-ttu-id="4e79a-119">Napi megfigyeléseket szám`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span><span class="sxs-lookup"><span data-stu-id="4e79a-119">Get the count of observations per day  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span></span>
3. <span data-ttu-id="4e79a-120">A szintek kategorikus oszlopban beolvasása</span><span class="sxs-lookup"><span data-stu-id="4e79a-120">Get the levels in a categorical column</span></span>  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. <span data-ttu-id="4e79a-121">A szintek számának beolvasása a kombinációja kategorikus kétoszlopos`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span><span class="sxs-lookup"><span data-stu-id="4e79a-121">Get the number of levels in combination of two categorical columns  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span></span>
5. <span data-ttu-id="4e79a-122">A numerikus oszlopok terjesztése beolvasása</span><span class="sxs-lookup"><span data-stu-id="4e79a-122">Get the distribution for numerical columns</span></span>  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. <span data-ttu-id="4e79a-123">Rekordok kinyerése két tábla illesztése</span><span class="sxs-lookup"><span data-stu-id="4e79a-123">Extract records from joining two tables</span></span>
   
        SELECT
            a.<common_columnname1> as <new_name1>,
            a.<common_columnname2> as <new_name2>,
            a.<a_column_name1> as <new_name3>,
            a.<a_column_name2> as <new_name4>,
            b.<b_column_name1> as <new_name5>,
            b.<b_column_name2> as <new_name6>
        FROM
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <a_column_name1>,
                <a_column_name2>,
            FROM <databasename>.<tablename1>
            ) a
            join
            (
            SELECT <common_columnname1>,
                <common_columnname2>,
                <b_column_name1>,
                <b_column_name2>,
            FROM <databasename>.<tablename2>
            ) b
            ON a.<common_columnname1>=b.<common_columnname1> and a.<common_columnname2>=b.<common_columnname2>

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a><span data-ttu-id="4e79a-124">A taxi út adatáttelepítések esetében a lekérdezés további parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="4e79a-124">Additional query scripts for taxi trip data scenarios</span></span>
<span data-ttu-id="4e79a-125">Példák az adott lekérdezések [NYC Taxi út adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) forgatókönyvek is szerepelnek [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="4e79a-125">Examples of queries that are specific to [NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="4e79a-126">Ezeket a lekérdezéseket már rendelkezik az adatok séma van megadva, és készen áll elküldésre váró futtatásához.</span><span class="sxs-lookup"><span data-stu-id="4e79a-126">These queries already have data schema specified and are ready to be submitted to run.</span></span>

