---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: aaaLoad adatokat az Azure blob storage az Azure SQL Data Warehouse (Azure Data Factory) |} Microsoft Docs
description: "További tudnivalók az Azure Data Factory tooload adatok"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 689fb02e-eb98-4f7c-81e6-6c1d22d53901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: barbkess
ms.custom: loading
ms.openlocfilehash: 29a220679a11cedefb0dfd06c0a6838f81a90447
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a>Adatok betöltése az Azure Blob Storage-ből az Azure SQL Data Warehouse-ba (Azure Data Factory)
> [!div class="op_single_selector"]
> * [Data Factory](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [PolyBase](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 Az oktatóanyag bemutatja, hogyan toocreate egy folyamatot az Azure Data Factory toomove adatok Azure Storage-Blobba tooSQL Data warehouse-bA. Az alábbi lépésekkel hello tartalma:

* Mintaadatokat telepíthet egy Azure Storage-blobba.
* Csatlakoztassa az erőforrások tooAzure adat-Előállítóban.
* Hozzon létre egy folyamat toomove adatok Storage Blobsba tooSQL Data warehouse-bA.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Előkészületek
toofamiliarize saját Azure Data Factory, lásd: [Data Factory bemutatása tooAzure][Introduction tooAzure Data Factory].

### <a name="create-or-identify-resources"></a>Erőforrások létrehozása és azonosítása
Az oktatóanyag elindítása előtt kell a következő erőforrások toohave hello.

* **Azure Storage-Blob**: Ebben az oktatóanyagban használt Azure Storage-Blobba hello adatforrásként hello Azure Data Factory-folyamathoz, és így toohave egy elérhető toostore hello mintaadatokat kell. Ha Ön nem rendelkezik ilyennel, megtudhatja, hogyan túl[hozzon létre egy tárfiókot][Create a storage account].
* **Az SQL Data Warehouse**: az Azure Storage-Blobból az oktatóanyag a kurzor hello adatok túl az SQL Data Warehouse, ezért kell egy online adatraktárral, amely be van töltve az AdventureWorksDW-mintaadatok hello toohave. Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan túl[hozhat létre egyet][Create a SQL Data Warehouse]. Ha rendelkezik adatraktárral, de nem töltötte fel hello mintaadatokkal, akkor [töltse be manuálisan][Load sample data into SQL Data Warehouse].
* **Az Azure Data Factory**: Azure Data Factory hello tényleges betöltése befejeződik, és így használható toobuild hello adatok mozgása folyamat egyik toohave kell. Ha Ön nem rendelkezik ilyennel, megtudhatja, hogyan toocreate egy 1. lépésben a [Ismerkedés az Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].
* **AZCopy**: a helyi ügyfél tooyour Azure Storage-Blobba az AZCopy toocopy hello mintaadatok van szüksége. Telepítési útmutatásért lásd: hello [AZCopy dokumentációját][AZCopy documentation].

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a>1. lépés: A minta adatok tooAzure tárolási Blob másolása
Miután az összes hello rendelkezésre, készen áll a toocopy minta adatok tooyour Azure Storage-Blobba áll.

1. [Mintaadatok letöltése][Download sample data]. Ezek az adatok további három év értékesítési adatait tooyour AdventureWorksDW-mintaadatok adja hozzá.
2. Használja az AZCopy parancs toocopy hello három évnyi adat tooyour Azure Storage-Blobba.

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-tooazure-data-factory"></a>2. lépés: Csatlakozás erőforrások tooAzure adat-előállító
Most, hogy rendelkezésre áll hello adatok létrehozhatjuk hello Azure Data Factory adatcsatorna toomove hello adatokat az Azure blob storage az SQL Data Warehouse.

tooget elindult, nyissa meg hello [Azure-portálon] [ Azure portal] válassza ki a data factory hello bal oldali menüből.

### <a name="step-21-create-linked-service"></a>2.1. lépés: Társított szolgáltatás létrehozása
Az Azure storage-fiókok és az SQL Data Warehouse tooyour adat-előállító hivatkozásra.  

1. Első lépésként hello regisztrációs folyamat megkezdéséhez kattintson a data factory "Összekapcsolt szolgáltatások" szakaszára hello, és kattintson az "Új adattároló." Válasszon egy nevet tooregister alatt, az típusaként válassza az Azure Storage az azure tárterületet, és írja be a fiók nevét és a Fiókkulcsot.
2. az SQL Data Warehouse tooregister toohello "Fejlesztés és üzembe helyezés" szakaszban keresse meg, jelölje ki "Új adattároló", majd az "Azure SQL Data Warehouse". Másolja és illessze be ezt a sablont, majd adja meg a kért adatokat.

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-hello-dataset"></a>2.2. lépés: Hello adatkészlet meghatározása
Miután hello létrehozása kapcsolódó szolgáltatások, azt kell toodefine hello adatkészletek.  Itt Ez azt jelenti, a tárolási tooyour adatraktárból adatbázisba Mozgatott adatok hello hello szerkezete meghatározása.  További információk a létrehozással kapcsolatban

1. A folyamat indításához lépjen a data factory toohello "Fejlesztés és üzembe helyezés" szakaszában.
2. Kattintson "Új adathalmaz" majd "Azure Blob storage" toolink a tárolási tooyour adat-előállítóban.  Használható parancsfájl toodefine alatt hello az adatokat az Azure Blob storage:

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
            "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": "\n"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```


1. Most meghatározzuk az adatkészletet az SQL Data Warehouse-hoz.  Először hello ugyanúgy "Új adathalmaz", majd az "Azure SQL Data Warehouse".

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a>3. lépés: A folyamat létrehozása és futtatása
Végezetül fedi le az Azure Data Factory beállításról és a Futtatás hello folyamat.  Ez az hello művelet végzi el a hello adatok tényleges mozgatását.  Hello műveleteket hajthatja végre az SQL-adatraktár és az Azure Data Factory teljes egészében található [Itt][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].

Hello "Fejlesztés és üzembe helyezés" szakaszban most kattintson a "Több parancs" és "Új adatcsatorna".  Hello folyamat létrehozása után a hello kód tootransfer hello adatok tooyour adatraktár alatt is használhatja:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Következő lépések
több, először toolearn megtekintése:

* [Azure Data Factory képzési terv][Azure Data Factory learning path].
* [Azure SQL Data Warehouse-összekötő][Azure SQL Data Warehouse Connector]. Ez a hello fő referencia-témakör az Azure Data Factory használatával az Azure SQL Data Warehouse szolgáltatással.

Ezek a témakörök további információkat adnak az Azure Data Factory szolgáltatással kapcsolatban. Azure SQL Database és a HDinsight Témájuk, de hello információk az SQL Data Warehouse tooAzure.

* [Oktatóanyag: Ismerkedés az Azure Data Factoryvel] [ Tutorial: Get started with Azure Data Factory] hello fő oktatóanyag az Azure Data Factory adatfeldolgozás azt. Ebben az oktatóanyagban felépítheti első folyamatát, amely a HDInsight tootransform használja, és havonta webes naplók elemzése. Megjegyzés: Ebben az oktatóanyagban nincs másolási tevékenység.
* [Oktatóanyag: Adatok másolása az Azure Storage-Blobba tooAzure SQL-adatbázis][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]. Ebben az oktatóanyagban az Azure Storage-Blobba tooAzure SQL-adatbázis egy folyamatot az Azure Data Factory toocopy adatokat hoz létre.

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction tooAzure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data tooand from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
