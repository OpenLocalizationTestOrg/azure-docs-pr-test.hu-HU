---
title: "Data Lake Store más Azure-szolgáltatásokkal aaaIntegrating |} Microsoft Docs"
description: "Megérteni, hogyan integrálható a Data Lake Store más Azure-szolgáltatásokkal"
documentationcenter: 
services: data-lake-store
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 48a5d1f4-3850-4c22-bbc4-6d1d394fba8a
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 839689f04623f8225884e7aa9c0b533a09323e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-data-lake-store-with-other-azure-services"></a>A Data Lake Store integrálása más Azure-szolgáltatásokkal
Azure Data Lake Store más Azure-szolgáltatások tooenable a lehetséges forgatókönyvek bővítése együtt használható. hello következő sorolja fel, hogy a Data Lake Store integrálható hello szolgáltatások.

## <a name="use-data-lake-store-with-azure-hdinsight"></a>Használjon Data Lake Store az Azure hdinsightban
Megadhat egy [Azure HDInsight](https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/) HDFS-kompatibilis hello tárolóként Data Lake Store használó fürt. Ebben a kiadásban a Windows és Linux, a Hadoop és a Storm fürtök használhatja Data Lake Store további tárterületként csak. A fürtök Azure Storage (WASB) továbbra is használja a hello alapértelmezett tárolására. Azonban a Windows és Linux HBase fürtök esetén is használhatja Data Lake Store hello alapértelmezett tároló, vagy további tárterületet, vagy mindkettőt.

Hogyan tooprovision egy HDInsight-fürtöt Data Lake Store vonatkozó utasításokért lásd:

* [HDInsight-fürtök kiépítése a Data Lake Store Azure portál használatával](data-lake-store-hdinsight-hadoop-use-portal.md)
* [A Data Lake Store HDInsight-fürtök kiépítése alapértelmezett tárolóként Azure PowerShell használatával](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
* [További tárhely az Azure PowerShell, a Data Lake Store HDInsight-fürtök kiépítése](data-lake-store-hdinsight-hadoop-use-powershell.md)

## <a name="use-data-lake-store-with-azure-data-lake-analytics"></a>Használjon Data Lake Store az Azure Data Lake Analytics
[Az Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-overview.md) lehetővé teszi a Big Data típusú adatok toowork a felhőbeli skálázással. Dinamikusan kiosztja az erőforrásokat, és lehetővé teszi terabájtjain vagy akár exabájtjain való elemzést által támogatott adatforrások, az egyik legyen a Data Lake Store alatt száma tárolhatja. A Data Lake Analytics kifejezetten optimalizált toowork az Azure Data Lake Store - biztosító hello legmagasabb szintű teljesítményt, adatátvitelt és párhuzamos folyamatkezelést biztosítja azt a big data-számítási feladatok.

Útmutatást toouse Data Lake Analytics a Data Lake Store, lásd: [az Data Lake Analytics Data Lake Store használatának első lépései](../data-lake-analytics/data-lake-analytics-get-started-portal.md).

## <a name="use-data-lake-store-with-azure-data-factory"></a>Használjon Data Lake Store az Azure Data Factoryvel
Használhat [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) Azure-táblákban, az Azure SQL Database, Azure SQL-adattárház, Azure Storage Blobs és a helyszíni adatbázisok tooingest adatait. Folyamatban egy első osztályú polgára hello Azure ökoszisztéma, Azure Data Factory lehet használt tooorchestrate hello adatfeldolgozást ezek forrás tooAzure Data Lake Store származó adatok.

Hogyan toouse a Data Lake Store az Azure Data Factory: kapcsolatos utasításokat [adatok tooand áthelyezését a Data Factory használatával a Data Lake Store](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="copy-data-from-azure-storage-blobs-into-data-lake-store"></a>Adatok másolása az Azure Storage blobs szolgáltatásban a Data Lake Store
Azure Data Lake Store biztosítja a parancssori eszközzel, AdlCopy, amely lehetővé teszi az Azure Blob Storage toocopy adatok a Data Lake Store-fiók. További információkért lásd: [adatok másolása az Azure Storage Blobs tooData Lake Store](data-lake-store-copy-data-azure-storage-blob.md).

## <a name="copy-data-between-azure-sql-database-and-data-lake-store"></a>Adatok másolása az Azure SQL Database és a Data Lake Store között
Apache Sqoop tooimport használja, és exportálhatja az adatokat az Azure SQL Database és a Data Lake Store között. További információkért lásd: [Data Lake Store és a Sqoop használatával Azure SQL-adatbázis közötti adatok másolása](data-lake-store-data-transfer-sql-sqoop.md).

## <a name="use-data-lake-store-with-stream-analytics"></a>Használjon Data Lake Store az Stream Analytics használ
Data Lake Store hello egyik kimenete toostore adatok folyamatos átviteli Azure Stream Analytics segítségével használhatja. További információkért lásd: [adatfolyam-adatokat az Azure Storage-Blobból az Azure Stream Analytics használata a Data Lake Store](data-lake-store-stream-analytics.md).

## <a name="use-data-lake-store-with-power-bi"></a>Használjon Data Lake Store a Power BI
Egy Data Lake Store-fiók tooanalyze adatait a Power BI tooimport használják, és hello adatainak megjelenítése. További információkért lásd: [elemezhetik a Power BI használatával a Data Lake Store](data-lake-store-power-bi.md).

## <a name="use-data-lake-store-with-data-catalog"></a>Használjon Data Lake Store a Data Catalog
Rögzítheti a Data Lake Store-ból adatokat hello Azure Data Catalog toomake hello adatok felderíthető hello szervezeten belül. További információ: [Data Lake Store-ból adatokat regisztrálni kell az Azure Data Catalog](data-lake-store-with-data-catalog.md).

## <a name="use-data-lake-store-with-sql-server-integration-services-ssis"></a>Használjon Data Lake Store az SQL Server Integration Services (SSIS)
A SSIS tooconnect egy SSIS-csomagot az Azure Data Lake Store hello Azure Data Lake Store Csatlakozáskezelő is használhatja. További információkért lásd: [használata Data Lake Store az SSIS](https://docs.microsoft.com/sql/integration-services/connection-manager/azure-data-lake-store-connection-manager).

## <a name="use-data-lake-store-with-sql-data-warehouse"></a>Használjon Data Lake Store az SQL Data Warehouse szolgáltatással
PolyBase tooload adatokat az Azure Data Lake Store az SQL Data Warehouse használja. További információ: [használata Data Lake Store az SQL Data Warehouse szolgáltatással](../sql-data-warehouse/sql-data-warehouse-load-from-azure-data-lake-store.md).

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Lake Store áttekintése](data-lake-store-overview.md)
* [Ismerkedés a Data Lake Store-portál használatával](data-lake-store-get-started-portal.md)
* [Ismerkedés a Data Lake Store PowerShell használatával](data-lake-store-get-started-powershell.md)  

