---
title: "aaaAzure Cosmos DB méretezés és teljesítmény tesztelése |} Microsoft Docs"
description: "Megtudhatja, hogyan tooperform méretezése és a teljesítmény tesztelése az Azure Cosmos DB"
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
ms.openlocfilehash: 46d1217e11a39ee970a868de9a5c5dfcf52cedf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Teljesítmény és méretezhetőség, az Azure Cosmos DB tesztelése
Teljesítmény- és mérettesztelés az alkalmazásfejlesztés kulcsfontosságú lépés. Számos alkalmazás, az adatbázis-rétegből hello jelentős hatással van hello átfogó teljesítményét és méretezhetőségét, és ezért kritikus összetevője teljesítményét teszteli. [Az Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) erre a célra kialakított rugalmasan méretezhető és kiszámítható teljesítményt, és ezért egy nagyszerű méretezése nagy teljesítményű adatbázis-rétegből igénylő alkalmazásokhoz. 

Ez a cikk a fejlesztők a Cosmos DB munkaterhelések teljesítményét teszt csomagok végrehajtási, vagy nagy teljesítményű alkalmazás forgatókönyvek Cosmos DB kiértékelése hivatkozás. Elsősorban hello adatbázis vizsgálati elkülönített teljesítmény összpontosít, de ajánlott eljárások az üzemi környezetben működő alkalmazásokat is tartalmazza.

A cikk elolvasása után fog tudni tooanswer hello a következő kérdéseket:   

* Hol található a .NET ügyfél mintaalkalmazás Cosmos DB teljesítmény teszteléshez? 
* Hogyan elérése a saját ügyfélalkalmazás magas teljesítmény szintek Cosmos DB?

tooget használatába a kódot, töltse le a hello projekt [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> hello az alkalmazás célja gyakorlati tanácsok toodemonstrate kívül Cosmos DB jobb teljesítményt kibontott ügyfélgépek kis számú. Ez nem tették hello szolgáltatást, amely méretezhető korlátlanul bővíthető toodemonstrate hello maximális kapacitását.
> 
> 

Ha az ügyféloldali konfigurációs beállítások tooimprove Cosmos DB teljesítmény keres, tekintse meg [Azure Cosmos DB teljesítmény tippek](performance-tips.md).

## <a name="run-hello-performance-testing-application"></a>Hello teljesítménytesztelési alkalmazás futtatása
hello leggyorsabb módon tooget lépések van toocompile és futtatási hello .NET mintaalkalmazás az alábbi hello lépéseket leírtak szerint. Is tekintse át a hello forráskódját és hasonló konfigurációkat tooyour saját ügyfélalkalmazások megvalósításához.

**1. lépés:** letöltési hello projektet a [Azure Cosmos DB teljesítményének tesztelése minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), vagy elágazás hello GitHub-tárházban.

**2. lépés:** végponti URL-cím, AuthorizationKey, CollectionThroughput és DocumentTemplate (opcionális) az App.config fájlban hello beállításainak módosítása.

> [!NOTE]
> Gyűjtemények nagy átviteli sebességgel üzembe helyezése, előtt tekintse meg az toohello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/) tooestimate hello költségek gyűjteményenként. Az Azure Cosmos DB váltók tárolási és átviteli sebességet egymástól függetlenül óránként, így költségeket takaríthat törlése vagy a Azure Cosmos DB gyűjtemények hello átviteli lowering tesztelés után.
> 
> 

**3. lépés:** összegzik és hello Konzolalkalmazás hello parancssorból futtassa. Hello hasonló kimenetnek kell megjelennie:

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


**(Ha szükséges), a 4. lépés:** hello átviteli jelentett (RU/mp) hello eszközből hello azonos vagy nagyobb, mint hello kiosztott átviteli sebesség hello gyűjtemény kell lennie. Ha nem, növekvő hello DegreeOfParallelism kis lépésekkel segítenek elérni hello korlátot. Ha az ügyfél alkalmazásból hello átviteli trületek hello alkalmazás több példánya indítása a hello azonos vagy különböző példányok eléri a határértéket kiépített hello hello között különböző gépek nyújtanak segítséget. Ha ebben a lépésben segítségre van szüksége, kérjük, írjon e-mailt tooaskcosmosdb@microsoft.com vagy a fájl egy támogatási jegy a hello [Azure Portal](https://portal.azure.com).

Miután hello alkalmazás fut, próbálja meg különböző [házirendek indexelő](indexing-policies.md) és [konzisztenciaszintek](consistency-levels.md) toounderstand átviteli sebesség és a késleltetés gyakorolt hatásának. Hello forráskód áttekintheti és hasonló konfigurációkat tooyour saját tesztelési programcsomagok vagy üzemi környezetben működő alkalmazásokat is.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben azt venni, hogyan hajthat végre a teljesítmény és méretezhetőség tesztelték Cosmos Adatbázisba egy .NET-Konzolalkalmazás használatával. Azure Cosmos DB használatával kapcsolatos további információkért tekintse meg az alábbi toohello hivatkozásokat követve.

* [Az Azure Cosmos adatbázis teljesítményének tesztelésekor minta](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Ügyfél konfigurációs beállítások tooimprove Azure Cosmos DB teljesítmény](performance-tips.md)
* [Kiszolgálóoldali particionálás az Azure Cosmos-Adatbázisba](partition-data.md)


