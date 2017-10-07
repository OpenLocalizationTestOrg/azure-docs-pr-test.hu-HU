---
title: "Azure Cosmos DB aaaDocumentDB API Python példák |} Microsoft Docs"
description: "Python példák a githubon található Azure Cosmos DB, beleértve a CRUD műveletek az általános feladatok."
keywords: "Python példák"
services: cosmos-db
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: python
ms.assetid: 7f4f8db3-e9db-4645-92ef-7819d486a349
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2016
ms.author: moderakh
ms.openlocfilehash: d8f240782b0997f2d32b68d310dc6f4ff6cb36d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-python-examples"></a>Az Azure Cosmos DB Python-példák
> [!div class="op_single_selector"]
> * [.NET-példák](documentdb-dotnet-samples.md)
> * [NODE.js-példák](documentdb-nodejs-samples.md)
> * [Python példák](documentdb-python-samples.md)
> * [A minta Azure Kódgalériából.](https://azure.microsoft.com/documentation/samples/?service=documentdb)
> 
> 

Minta megoldások, amelyek a CRUD műveletek és más olyan gyakori műveleteket Azure Cosmos DB erőforrások szerepelnek hello [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) GitHub-tárházban. Ez a cikk a következő információkat tartalmazza:

* Hivatkozások toohello feladatok hello Python példa projektfájlokat mindegyikében. 
* Hivatkozások toohello kapcsolódó API-referencia-tartalom.

**Előfeltételek**

1. Egy Azure-fiók toouse kell a Python példákat:
   * Is [nyissa meg az Azure-fiók szabad](https://azure.microsoft.com/pricing/free-trial/): jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például webhelyekhez. A hitelkártya soha nem lesz felszámítva, kivéve, ha explicit módon a beállítások módosításához és kérje meg a toobe számítjuk fel.
     * Is [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/): A Visual Studio-előfizetéssel biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.
2. Szükség hello [Python SDK](documentdb-sdk-python.md). 
   
   > [!NOTE]
   > Minden minta önálló, maga beállítja, és a szükségtelenné vált maga után. Így a hello minták ki több hívások túl[document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Minden alkalommal, amikor ez történik, az előfizetés alapján számlázzuk használati hello teljesítményszinttel hello gyűjtemény létrehozása / 1 órában. 
   > 
   > 

## <a name="database-examples"></a>Adatbázis-példák
Hello [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) hello fájljának [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projekt bemutatja, hogyan tooperform hello a következő feladatokat.

| Tevékenység | API-referencia |
| --- | --- |
| [Adatbázis létrehozása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client. CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Lekérdezés-adatbázis fiók](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Olvassa el az adatbázis-azonosító szerint](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Egy olyan fiók lista adatbázisok](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Adatbázis törlése](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client. DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |

## <a name="collection-examples"></a>Gyűjtemény példák
Hello [Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) hello fájljának [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projekt bemutatja, hogyan tooperform hello a következő feladatokat.

| Tevékenység | API-referencia |
| --- | --- |
| [Gyűjtemény létrehozása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Olvassa el az adatbázis összes gyűjteményt listája](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) |[document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Egy gyűjtemény tagjaira azonosító lekérése](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [A gyűjtemény teljesítményszinttel beolvasása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) |[DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Egy gyűjtemény teljesítmény szintjének módosítása](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) |[document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [A gyűjtemény törlése](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |

