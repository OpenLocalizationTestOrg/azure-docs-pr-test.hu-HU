---
title: "a Hive-lekérdezéseket a Hive táblák aaaExplore adatok |} Microsoft Docs"
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
ms.openlocfilehash: 2ede3d41682aa08ced19284f7a83ec95e0c2a93a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-hive-tables-with-hive-queries"></a><span data-ttu-id="93883-103">A Hive-táblákban tárolt adatok megismerése Hive-lekérdezésekkel</span><span class="sxs-lookup"><span data-stu-id="93883-103">Explore data in Hive tables with Hive queries</span></span>
<span data-ttu-id="93883-104">Ez a dokumentum minta Hive parancsfájlokat egy HDInsight Hadoop-fürt Hive táblák használt tooexplore adatait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="93883-104">This document provides sample Hive scripts that are used tooexplore data in Hive tables in an HDInsight Hadoop cluster.</span></span>

<span data-ttu-id="93883-105">hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toouse eszközök különböző tárolási környezetekben tooexplore adatait tootopics.</span><span class="sxs-lookup"><span data-stu-id="93883-105">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="93883-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="93883-106">Prerequisites</span></span>
<span data-ttu-id="93883-107">Ez a cikk feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="93883-107">This article assumes that you have:</span></span>

* <span data-ttu-id="93883-108">Egy Azure storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="93883-108">Created an Azure storage account.</span></span> <span data-ttu-id="93883-109">Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="93883-109">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="93883-110">A testre szabott Hadoop-fürt a HDInsight-szolgáltatás hello kiépítve.</span><span class="sxs-lookup"><span data-stu-id="93883-110">Provisioned a customized Hadoop cluster with hello HDInsight service.</span></span> <span data-ttu-id="93883-111">Ha módosítania kell az utasításokat, lásd: [testreszabása Azure HDInsight Hadoop-fürtök az Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="93883-111">If you need instructions, see [Customize Azure HDInsight Hadoop Clusters for Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).</span></span>
* <span data-ttu-id="93883-112">hello már feltöltött tooHive táblák az Azure HDInsight Hadoop-fürtök.</span><span class="sxs-lookup"><span data-stu-id="93883-112">hello data has been uploaded tooHive tables in Azure HDInsight Hadoop clusters.</span></span> <span data-ttu-id="93883-113">Ha még nem, kövesse a hello utasításait [létrehozása és a betöltés tooHive adattáblák](machine-learning-data-science-move-hive-tables.md) tooupload adatok tooHive először táblázatot.</span><span class="sxs-lookup"><span data-stu-id="93883-113">If it has not, follow hello instructions in [Create and load data tooHive tables](machine-learning-data-science-move-hive-tables.md) tooupload data tooHive tables first.</span></span>
* <span data-ttu-id="93883-114">Engedélyezett távoli hozzáférési toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="93883-114">Enabled remote access toohello cluster.</span></span> <span data-ttu-id="93883-115">Ha módosítania kell az utasításokat, lásd: [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span><span class="sxs-lookup"><span data-stu-id="93883-115">If you need instructions, see [Access hello Head Node of Hadoop Cluster](machine-learning-data-science-customize-hadoop-cluster.md#headnode).</span></span>
* <span data-ttu-id="93883-116">Ha útmutatást szeretne toosubmit Hive-lekérdezéseket, lásd: [hogyan tooSubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit)</span><span class="sxs-lookup"><span data-stu-id="93883-116">If you need instructions on how toosubmit Hive queries, see [How tooSubmit Hive Queries](machine-learning-data-science-move-hive-tables.md#submit)</span></span>

## <a name="example-hive-query-scripts-for-data-exploration"></a><span data-ttu-id="93883-117">Példa Hive lekérdezés parancsfájlok adatok feltárása</span><span class="sxs-lookup"><span data-stu-id="93883-117">Example Hive query scripts for data exploration</span></span>
1. <span data-ttu-id="93883-118">Partíciónként megfigyelések hello számbavétele`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span><span class="sxs-lookup"><span data-stu-id="93883-118">Get hello count of observations per partition  `SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`</span></span>
2. <span data-ttu-id="93883-119">Napi megfigyeléseket hello számbavétele`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span><span class="sxs-lookup"><span data-stu-id="93883-119">Get hello count of observations per day  `SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`</span></span>
3. <span data-ttu-id="93883-120">Hello szintek kategorikus oszlopban beolvasása</span><span class="sxs-lookup"><span data-stu-id="93883-120">Get hello levels in a categorical column</span></span>  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. <span data-ttu-id="93883-121">Hello szintek száma kombinációja kategorikus kétoszlopos lekérése`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span><span class="sxs-lookup"><span data-stu-id="93883-121">Get hello number of levels in combination of two categorical columns  `SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`</span></span>
5. <span data-ttu-id="93883-122">Hello terjesztési numerikus oszlopok az beszerzése</span><span class="sxs-lookup"><span data-stu-id="93883-122">Get hello distribution for numerical columns</span></span>  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. <span data-ttu-id="93883-123">Rekordok kinyerése két tábla illesztése</span><span class="sxs-lookup"><span data-stu-id="93883-123">Extract records from joining two tables</span></span>
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a><span data-ttu-id="93883-124">A taxi út adatáttelepítések esetében a lekérdezés további parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="93883-124">Additional query scripts for taxi trip data scenarios</span></span>
<span data-ttu-id="93883-125">Példák a lekérdezések, amelyek adott túl[NYC Taxi út adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) forgatókönyvek is szerepelnek [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span><span class="sxs-lookup"><span data-stu-id="93883-125">Examples of queries that are specific too[NYC Taxi Trip Data](http://chriswhong.com/open-data/foil_nyc_taxi/) scenarios are also provided in [GitHub repository](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts).</span></span> <span data-ttu-id="93883-126">Ezeket a lekérdezéseket már adatok séma van megadva, és készen áll a toobe benyújtott toorun.</span><span class="sxs-lookup"><span data-stu-id="93883-126">These queries already have data schema specified and are ready toobe submitted toorun.</span></span>

