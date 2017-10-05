---
title: "Azure Cosmos DB méretezés és teljesítmény tesztelése |} Microsoft Docs"
description: "Ismerje meg, hogyan hajthat végre a méretezés és teljesítmény az Azure Cosmos DB tesztelése"
keywords: "A teljesítmény tesztelése"
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: arramac
ms.openlocfilehash: b5a1edd08819e82437c5b22d8eb131665d7c9645
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése
Teljesítmény- és mérettesztelés az alkalmazásfejlesztés kulcsfontosságú lépés. Több alkalmazás az adatbázis-rétegből jelentős hatással az átfogó teljesítményét és méretezhetőségét, és ezért a teljesítmény tesztelése kritikus összetevője. [Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) erre a célra kialakított rugalmasan méretezhető és kiszámítható teljesítményt, és ezért egy nagyszerű méretezése nagy teljesítményű adatbázis-rétegből igénylő alkalmazásokhoz. 

Ez a cikk a fejlesztők a Cosmos DB munkaterhelések teljesítményét teszt csomagok végrehajtási, vagy nagy teljesítményű alkalmazás forgatókönyvek Cosmos DB kiértékelése hivatkozás. Elsősorban az adatbázis vizsgálati elkülönített teljesítmény összpontosít, de ajánlott eljárások az üzemi környezetben működő alkalmazásokat is tartalmazza.

A cikk elolvasása után lesz a következő kérdések megválaszolásához:   

* Hol található a .NET ügyfél mintaalkalmazás Cosmos DB teljesítmény teszteléshez? 
* Hogyan elérése a saját ügyfélalkalmazás magas teljesítmény szintek Cosmos DB?

Első lépésként kóddal, töltse le a projektet a [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> Ez az alkalmazás célja a jobb teljesítmény érdekében kívül Cosmos DB kibontása ügyfélgépek kis számú ajánlott eljárásai bemutatják. Ez nem tették bemutatása a szolgáltatást, amely méretezhető korlátlanul bővíthető maximális kapacitását.
> 
> 

Ha a keresett Cosmos DB teljesítmény javítása érdekében az ügyféloldali konfigurációs beállítások, lásd: [Azure Cosmos DB teljesítmény tippek](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>Futtatható az alkalmazás tesztelése
Első lépésként a leggyorsabban fordításához és futtatásához a .NET-példa az alábbi az alábbi lépések elvégzésével. Is ajánlott felülvizsgálni a forráskódot, és a saját ügyfélalkalmazásait hasonló konfigurációkat valósítható meg.

**1. lépés:** töltse le a projektet a [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), vagy oszthatja ketté a GitHub-tárházban.

**2. lépés:** módosítsa a beállításokat a végponti URL-cím, AuthorizationKey, CollectionThroughput és DocumentTemplate az App.config fájlban (nem kötelező).

> [!NOTE]
> Gyűjtemények nagy átviteli sebességgel üzembe helyezése, előtt tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/) becsléséhez gyűjteményenként költségeket. Az Azure Cosmos DB váltók tárolási és átviteli sebességet egymástól függetlenül óránként, így költségeket takaríthat törlése, és így csökkenti az átviteli sebessége a Azure Cosmos DB gyűjtemények tesztelés után.
> 
> 

**3. lépés:** fordítsa le és futtassa a konzolalkalmazást a parancssorból. A következőhöz hasonló kimenetnek kell megjelennie:

    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://docdb-scale-demo.documents.azure.com:443/
    Collection : db.testdata at 50000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: 500
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Creating database db
    Creating collection testdata
    Creating metric collection metrics
    Retrying after sleeping for 00:03:34.1720000
    Starting Inserts with 500 tasks
    Inserted 661 docs @ 656 writes/s, 6860 RU/s (18B max monthly 1KB reads)
    Inserted 6505 docs @ 2668 writes/s, 27962 RU/s (72B max monthly 1KB reads)
    Inserted 11756 docs @ 3240 writes/s, 33957 RU/s (88B max monthly 1KB reads)
    Inserted 17076 docs @ 3590 writes/s, 37627 RU/s (98B max monthly 1KB reads)
    Inserted 22106 docs @ 3748 writes/s, 39281 RU/s (102B max monthly 1KB reads)
    Inserted 28430 docs @ 3902 writes/s, 40897 RU/s (106B max monthly 1KB reads)
    Inserted 33492 docs @ 3928 writes/s, 41168 RU/s (107B max monthly 1KB reads)
    Inserted 38392 docs @ 3963 writes/s, 41528 RU/s (108B max monthly 1KB reads)
    Inserted 43371 docs @ 4012 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 48477 docs @ 4035 writes/s, 42282 RU/s (110B max monthly 1KB reads)
    Inserted 53845 docs @ 4088 writes/s, 42845 RU/s (111B max monthly 1KB reads)
    Inserted 59267 docs @ 4138 writes/s, 43364 RU/s (112B max monthly 1KB reads)
    Inserted 64703 docs @ 4197 writes/s, 43981 RU/s (114B max monthly 1KB reads)
    Inserted 70428 docs @ 4216 writes/s, 44181 RU/s (115B max monthly 1KB reads)
    Inserted 75868 docs @ 4247 writes/s, 44505 RU/s (115B max monthly 1KB reads)
    Inserted 81571 docs @ 4280 writes/s, 44852 RU/s (116B max monthly 1KB reads)
    Inserted 86271 docs @ 4273 writes/s, 44783 RU/s (116B max monthly 1KB reads)
    Inserted 91993 docs @ 4299 writes/s, 45056 RU/s (117B max monthly 1KB reads)
    Inserted 97469 docs @ 4292 writes/s, 44984 RU/s (117B max monthly 1KB reads)
    Inserted 99736 docs @ 4192 writes/s, 43930 RU/s (114B max monthly 1KB reads)
    Inserted 99997 docs @ 4013 writes/s, 42051 RU/s (109B max monthly 1KB reads)
    Inserted 100000 docs @ 3846 writes/s, 40304 RU/s (104B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 100000 docs @ 3834 writes/s, 40180 RU/s (104B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.


**(Ha szükséges), a 4. lépés:** az eszköze (RU/mp) jelentett átviteli azonosnak kell lennie, vagy nagyobb, mint a gyűjtemény a létesített átviteli sebesség. Ha nem, a kis lépésekkel DegreeOfParallelism növelése segítenek eléri a határértéket. Ha az ügyfél alkalmazásból átviteli trületek elindítani az alkalmazást az azonos vagy azoktól eltérő gépeken több példánya segítséget a különböző példányai között létesített eléri. Ha ebben a lépésben segítségre van szüksége, kérjük, írjon egy e-mailek askcosmosdb@microsoft.com vagy a támogatási jegy fájl a [Azure Portal](https://portal.azure.com).

Miután az alkalmazás futását, próbálkozzon másik [házirendek indexelő](indexing-policies.md) és [konzisztenciaszintek](consistency-levels.md) átviteli sebesség és a késleltetés gyakorolt hatásuk megértéséhez. Is ajánlott felülvizsgálni a forráskódot, és a saját tesztelési programcsomagok vagy üzemi környezetben működő alkalmazásokhoz hasonló konfigurációkat valósítja meg.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben azt venni, hogyan hajthat végre a teljesítmény és méretezhetőség tesztelték Cosmos Adatbázisba egy .NET-Konzolalkalmazás használatával. Tekintse meg az Azure Cosmos DB használatáról további információt az alábbi hivatkozásokat követve.

* [Az Azure Cosmos adatbázis teljesítményének tesztelésekor minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Ügyfél-konfigurációs beállítások Azure Cosmos DB teljesítmény javítása érdekében](performance-tips.md)
* [Kiszolgálóoldali particionálás az Azure Cosmos-Adatbázisba](partition-data.md)


