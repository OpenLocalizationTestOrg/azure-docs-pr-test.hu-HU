---
title: "NoSQL-oktatóanyag: DocumentDB API Azure Cosmos DB Java SDK |} Microsoft Docs"
description: "NoSQL-oktatóanyag, amely létrehoz egy online adatbázist és a DocumentDB API hello használata Azure Cosmos DB Java Konzolalkalmazás. Az Azure DocumentDB egy NoSQL-alapú adatbázis a JSON formátumhoz."
keywords: "nosql-oktatóanyag, online adatbázis, java konzolalkalmazás"
services: cosmos-db
documentationcenter: Java
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 75a9efa1-7edd-4fed-9882-c0177274cbb2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 05/22/2017
ms.author: arramac
ms.openlocfilehash: 1a298a15ab911d140b9df30ad52cfe0fa07c55b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nosql-tutorial-build-a-documentdb-api-java-console-application"></a>NoSQL-oktatóanyag: a DocumentDB API Java Konzolalkalmazás létrehozása
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js MongoDB-hez](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Toohello NoSQL-oktatóanyag a DocumentDB API hello Azure Cosmos DB Java SDK – Üdvözöljük! Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.

Az oktatóanyag a következőket ismerteti:

* Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók
* A Visual Studio megoldás konfigurálása
* Online adatbázis létrehozása
* Gyűjtemény létrehozása
* JSON-dokumentumok létrehozása
* Hello gyűjtemény lekérdezése
* JSON-dokumentumok létrehozása
* Hello gyűjtemény lekérdezése
* Dokumentum cseréje
* Dokumentum törlése
* Hello adatbázis törlése

Most pedig lássunk neki!

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/). Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.
* [Git](https://git-scm.com/downloads)
* [Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. lépés: Azure Cosmos DB-fiók létrehozása
Hozzunk létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[Klónozás hello GitHub projekt](#GitClone). Ha hello Azure Cosmos DB Emulator használata esetén kövesse a hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) tooset hello emulátor kialakításához, és hagyja ki azokat, amelyek túl[Klónozás hello GitHub projekt](#GitClone).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>2. lépés: Másolat hello GitHub-projekt
Ismerkedés a GitHub-tárházban hello klónozásával [Ismerkedés az Azure Cosmos DB és Java](https://github.com/Azure-Samples/documentdb-java-getting-started). Például egy helyi könyvtárból futtassa a következő tooretrieve hello minta-projekt helyi hello.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

hello könyvtár neve tartalmazza a `pom.xml` hello projekt és egy `src` Java forrás kód beleértve tartalmazó mappa `Program.java` mely bemutatja hogyan rendelkező Azure Cosmos DB például a dokumentumok létrehozása és adatainak lekérdezése egyszerű műveleteket hajtanak végre egy gyűjtemény. Hello `pom.xml` hello függőség tartalmaz [DocumentDB Java SDK a Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>3. lépés: Csatlakozás tooan Azure Cosmos DB fiók
A következő head biztonsági toohello [Azure Portal](https://portal.azure.com) tooretrieve a végpont és az elsődleges kulcs. hello Azure Cosmos DB végpont és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.

A hello Azure portál, keresse meg a tooyour Azure Cosmos DB fiókot, és kattintson **kulcsok**. Hello portálról hello URI másolja és illessze be azt `https://FILLME.documents.azure.com` hello Program.java fájlban. Ezután másolási elsődleges kulcs hello hello portálról, és illessze be azt `FILLME`.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Képernyőfelvétel a hello hello NoSQL-oktatóanyag toocreate konzol Java-alkalmazások által használt Azure-portálról. Egy Azure Cosmos DB megjeleníti a fiók, hello ACTIVE központ, a hello hello Azure Cosmos DB fiók panelén lévő KEYS gomb és hello URI, elsődleges és másodlagos kulcsot értékek kiemelésével a hello (kulcsok) panelén][keys]

## <a name="step-4-create-a-database"></a>4. lépés: Adatbázis létrehozása
Az Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) hello metódusában **DocumentClient** osztály. Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása
> [!WARNING]
> A **createCollection** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után. További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) hello metódusában **DocumentClient** osztály. A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása
A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) hello metódusában **DocumentClient** osztály. A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak. Most már beilleszthetünk egy vagy több dokumentumot. Ha van olyan adat, milyen toostore az adatbázisban, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) tooimport hello adatokat az adatbázisba.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Diagram szemléltető hello hierarchikus kapcsolatát hello fiók, hello online adatbázis, gyűjtemény hello és hello NoSQL-oktatóanyag toocreate konzol Java-alkalmazások által használt hello dokumentumok](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése
Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).  hello a következő mintakód bemutatja, hogyan tooquery dokumentumokat az Azure Cosmos Adatbázisba SQL-szintaxis használata hello [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metódust.

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje
Azure Cosmos-adatbázis frissítése JSON-dokumentumok hello segítségével támogatja [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metódust.

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése
Hasonlóképpen, Azure Cosmos DB támogatja a JSON-dokumentumok törlését hello segítségével [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metódust.  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>10. lépés: Hello adatbázis törlése
Törlése hello létrehozott adatbázis eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>11. lépés: Futtassa a teljes Java konzolalkalmazást!
toorun hello alkalmazás hello-konzolon keresse meg a toohello projektmappa és fordítási Maven használatával:
    
    mvn package

Futó `mvn package` hello legújabb Azure Cosmos DB könyvtár Maven tölt le, és hozza létre `GetStarted-0.0.1-SNAPSHOT.jar`. Ezután futtassa a hello alkalmazás futtatásával:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Gratulálunk! Elvégezte a NoSQL-oktatóanyagot, és egy működőképes Java konzolalkalmazással rendelkezik!

## <a name="next-steps"></a>Következő lépések
* Szüksége van egy Java-webalkalmazás létrehozására vonatkozó oktatóanyagra? Tekintse meg a [Java-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-java-application.md) című cikket.
* Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).
* A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).
* További tudnivalók a programozási modellt hello hello hello Develop szakasza [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/documentdb/).

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
