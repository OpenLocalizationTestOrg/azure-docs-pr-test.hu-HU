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
# <a name="explore-data-in-hive-tables-with-hive-queries"></a>A Hive-táblákban tárolt adatok megismerése Hive-lekérdezésekkel
Ez a dokumentum minta Hive parancsfájlokat egy HDInsight Hadoop-fürt Hive táblák használt tooexplore adatait tartalmazza.

hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan toouse eszközök különböző tárolási környezetekben tooexplore adatait tootopics.

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk feltételezi, hogy rendelkezik:

* Egy Azure storage-fiók létrehozása. Ha módosítania kell az utasításokat, lásd: [egy Azure Storage-fiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* A testre szabott Hadoop-fürt a HDInsight-szolgáltatás hello kiépítve. Ha módosítania kell az utasításokat, lásd: [testreszabása Azure HDInsight Hadoop-fürtök az Advanced Analytics](machine-learning-data-science-customize-hadoop-cluster.md).
* hello már feltöltött tooHive táblák az Azure HDInsight Hadoop-fürtök. Ha még nem, kövesse a hello utasításait [létrehozása és a betöltés tooHive adattáblák](machine-learning-data-science-move-hive-tables.md) tooupload adatok tooHive először táblázatot.
* Engedélyezett távoli hozzáférési toohello fürt. Ha módosítania kell az utasításokat, lásd: [hozzáférés hello Head csomópont a Hadoop-fürt](machine-learning-data-science-customize-hadoop-cluster.md#headnode).
* Ha útmutatást szeretne toosubmit Hive-lekérdezéseket, lásd: [hogyan tooSubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit)

## <a name="example-hive-query-scripts-for-data-exploration"></a>Példa Hive lekérdezés parancsfájlok adatok feltárása
1. Partíciónként megfigyelések hello számbavétele`SELECT <partitionfieldname>, count(*) from <databasename>.<tablename> group by <partitionfieldname>;`
2. Napi megfigyeléseket hello számbavétele`SELECT to_date(<date_columnname>), count(*) from <databasename>.<tablename> group by to_date(<date_columnname>);`
3. Hello szintek kategorikus oszlopban beolvasása  
    `SELECT  distinct <column_name> from <databasename>.<tablename>`
4. Hello szintek száma kombinációja kategorikus kétoszlopos lekérése`SELECT <column_a>, <column_b>, count(*) from <databasename>.<tablename> group by <column_a>, <column_b>`
5. Hello terjesztési numerikus oszlopok az beszerzése  
    `SELECT <column_name>, count(*) from <databasename>.<tablename> group by <column_name>`
6. Rekordok kinyerése két tábla illesztése
   
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

## <a name="additional-query-scripts-for-taxi-trip-data-scenarios"></a>A taxi út adatáttelepítések esetében a lekérdezés további parancsfájlok
Példák a lekérdezések, amelyek adott túl[NYC Taxi út adatok](http://chriswhong.com/open-data/foil_nyc_taxi/) forgatókönyvek is szerepelnek [GitHub-tárházban](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts). Ezeket a lekérdezéseket már adatok séma van megadva, és készen áll a toobe benyújtott toorun.

