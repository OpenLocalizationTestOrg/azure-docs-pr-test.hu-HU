---
title: "Adatok átalakítása: Folyamat & átalakítási adatok |} Microsoft Docs"
description: "Megtudhatja, hogyan tootransform adatok vagy az Azure Data Factory a Hadoop, a Machine Learning vagy az Azure Data Lake Analytics folyamat közé."
keywords: "adatok átalakítása, a folyamat az adatokat, adatok, transformation tevékenység"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 917d617259699b0e71de3a0e0c17463d00f2e0a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-in-azure-data-factory"></a>Az Azure Data Factoryben az adatok átalakítása
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
> * [Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
> * [Tárolt eljárás](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [Egyéni .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti az Azure Data Factory tootransform használnia, és feldolgozza a nyers adatok előrejelzéseket és elemzések adatok átalakítása tevékenységek. Egy átalakítási tevékenységet hajtja végre, például az Azure HDInsight-fürt vagy egy Azure Batch számítási környezetben. Hivatkozások tooarticles átalakítása tevékenységeiről részletes információkat biztosít.

Adat-előállító támogatja az adatok átalakítása tevékenységek túl adható hozzá a következő hello[folyamatok](data-factory-create-pipelines.md) vagy önállóan vagy egy másik tevékenységgel kapcsolt.

> [!NOTE]
> Lépésenkénti utasításokat talál útmutatást lásd: [hozzon létre egy folyamatot Hive átalakítása](data-factory-build-your-first-pipeline.md) cikk.  
> 
> 

## <a name="hdinsight-hive-activity"></a>HDInsight Hive tevékenység
a Data Factory-folyamathoz HDInsight Hive tevékenység hello Hive-lekérdezéseket a saját vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre. Lásd: [Hive tevékenység](data-factory-hive-activity.md) szóló cikkben olvashat ennek a tevékenységnek. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig tevékenység
a Data Factory-folyamathoz HDInsight Pig tevékenység hello Pig lekérdezések saját vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre. Lásd: [Pig tevékenység](data-factory-pig-activity.md) szóló cikkben olvashat ennek a tevékenységnek. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce művelethez
hello HDInsight MapReduce művelethez a Data Factory-folyamat saját MapReduce programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre. Lásd: [MapReduce művelethez](data-factory-map-reduce.md) szóló cikkben olvashat ennek a tevékenységnek.

## <a name="hdinsight-streaming-activity"></a>HDInsight Streamelési tevékenységben
hello Streamelési tevékenységben HDInsight a Data Factory-folyamat saját Hadoop Streamelési programok vagy igény szerinti Windows/Linux-alapú HDInsight-fürt hajt végre. Lásd: [HDInsight Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md) ezzel a tevékenységgel kapcsolatos részletekért.

## <a name="hdinsight-spark-activity"></a>HDInsight Spark-tevékenység
a Data Factory-folyamathoz HDInsight Spark tevékenység hello Spark programok saját HDInsight-fürt hajtja végre. További információkért lásd: [meghívása Spark programok az Azure Data Factory](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning tevékenységek
Az Azure Data Factory lehetővé teszi, hogy Ön tooeasily létrehozása folyamatok, amelyek egy közzétett Azure Machine Learning a prediktív elemzés webszolgáltatás. Hello segítségével [kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) egy Azure Data Factory-folyamat a hívhat meg a Machine Learning web toomake előrejelzések kötegben hello adatokon.

Idővel hello saját prediktív modelljeit hello pontozási kísérletek kell toobe retrained új Machine Learning bemeneti adatkészletek. Miután elkészült, az átképezési, kívánt hello webszolgáltatás pontozási tooupdate hello retrained gépi tanulási modell. Használhatja a hello [Update-Erőforrástevékenység](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) tooupdate hello webszolgáltatás hello újonnan betanított modell.  

Lásd: [használata a Machine Learning tevékenységek](data-factory-azure-ml-batch-execution-activity.md) ezeket a Machine Learning tevékenységeket vonatkozó további információért. 

## <a name="stored-procedure-activity"></a>A tárolt eljárási tevékenység
Hello SQL Server tárolt eljárási tevékenység használható egy adat-előállító adatcsatorna tooinvoke valamelyik hello adattárolókhoz a következő tárolt eljárást: Azure SQL Database, Azure SQL Data Warehouse szolgáltatásban a vállalat SQL Server-adatbázis vagy egy Azure virtuális Gépen. Lásd: [tárolt eljárási tevékenység](data-factory-stored-proc-activity.md) cikkben alább.  

## <a name="data-lake-analytics-u-sql-activity"></a>Data Lake Analytics U-SQL-tevékenység
Data Lake Analytics U-SQL-tevékenység egy Azure Data Lake Analytics-fürt U-SQL parancsfájlt futtat. Lásd: [Analytics U-SQL tevékenységet](data-factory-usql-activity.md) cikkben alább. 

## <a name="net-custom-activity"></a>.NET egyéni tevékenység
Ha tootransform adatokat oly módon, amely nem támogatja a Data Factory van szüksége, hozzon létre egy egyéni tevékenység saját adatokat feldolgozó logika, és hello tevékenység hello folyamat használja. Hello egyéni .NET tevékenység toorun az Azure Batch szolgáltatás vagy az Azure HDInsight-fürtöt is konfigurálhat. Lásd: [egyéni tevékenységeket felhasználni](data-factory-use-custom-activities.md) cikkben alább. 

Létrehozhat egyéni tevékenység toorun R parancsfájlok a HDInsight-fürt az R telepítve. Lásd: [Run R Script using Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) (R-parancsfájl futtatása az Azure Data Factory használatával). 

## <a name="compute-environments"></a>Számítási környezetek
Ön hello számítási környezet társított szolgáltatás létrehozása és a kapcsolódó hello szolgáltatást egy átalakítási tevékenységet meghatározásakor. A Data Factory által támogatott számítási környezetek két típusa van. 

1. **Igény szerinti**: Ebben az esetben hello számítási környezet teljes által kezelt adat-Előállítóban. Ez automatikusan hello Data Factory szolgáltatásnak által létrehozott, mielőtt egy feladatot az elküldött tooprocess adatai és távolítja el, amikor hello feladat befejezését. Konfigurálja, és a feladat végrehajtása, a kiszolgálófürt-felügyelet és a műveletek rendszerindítása hello igény szerinti számítási környezet részletes beállításainak kezeléséhez. 
2. **Bring Your Own**: Ebben az esetben regisztrálhatja a saját informatikai környezetben (például HDInsight-fürtök) a Data Factory kapcsolt szolgáltatásként. hello számítási környezet által felügyelt, és a Data Factory szolgáltatásnak hello tooexecute hello tevékenységek használja. 

Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md) kapcsolatos cikk toolearn számítási adat-előállító által támogatott szolgáltatások. 

## <a name="summary"></a>Összefoglalás
Az Azure Data Factory támogatja a következő adatok átalakítása tevékenységek hello, és hello hello tevékenységek számítási környezeteket. hello átalakítása tevékenységek lehet hozzáadott toopipelines vagy külön-külön vagy egy másik tevékenységgel kapcsolt.

| Adatátalakítási tevékenység | Számítási környezet |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning-tevékenységek: kötegelt végrehajtás és erőforrás frissítése](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Tárolt eljárás](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse vagy SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] vagy Azure Batch |

