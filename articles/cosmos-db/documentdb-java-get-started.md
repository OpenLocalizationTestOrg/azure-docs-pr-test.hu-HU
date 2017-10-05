---
title: "NoSQL-oktatóanyag: DocumentDB API Azure Cosmos DB Java SDK |} Microsoft Docs"
description: "NoSQL-oktatóanyag, amely létrehoz egy online adatbázist és a Java-konzolalkalmazást a DocumentDB API használatával az Azure Cosmos DB rendszerhez. Az Azure DocumentDB egy NoSQL-alapú adatbázis a JSON formátumhoz."
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
ms.openlocfilehash: 5c4bcda308f001572e1c34e991616fc209250a02
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
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

Üdvözöljük a NoSQL-oktatóanyagban a DocumentDB API Azure Cosmos DB Java SDK-t! Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.

Az oktatóanyag a következőket ismerteti:

* Azure Cosmos DB-fiók létrehozása és csatlakoztatása
* A Visual Studio megoldás konfigurálása
* Online adatbázis létrehozása
* Gyűjtemény létrehozása
* JSON-dokumentumok létrehozása
* A gyűjtemény lekérdezése
* JSON-dokumentumok létrehozása
* A gyűjtemény lekérdezése
* Dokumentum cseréje
* Dokumentum törlése
* Adatbázis törlése

Most pedig lássunk neki!

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg róla, hogy rendelkezik az alábbiakkal:

* Aktív Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/). Másik lehetőségként használhatja az [Azure Cosmos DB Emulatort](local-emulator.md) az oktatóanyaghoz.
* [Git](https://git-scm.com/downloads)
* [Java fejlesztői készlet (JDK) 7+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. lépés: Azure Cosmos DB-fiók létrehozása
Hozzunk létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik egy használni kívánt fiókkal, folytassa [A GitHub-projekt klónozása](#GitClone) című lépéssel. Ha az Azure Cosmos DB Emulatort használja, kövesse az [Azure Cosmos DB Emulatornál](local-emulator.md) leírt lépéseket az emulátor beállításához, majd ugorjon [A GitHub-projekt klónozása](#GitClone) című lépésre.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>2. lépés: A GitHub-projekt klónozása
A GitHub-adattár klónozásával kezdheti meg [az Azure Cosmos DB és a Java használatának első lépéseit](https://github.com/Azure-Samples/documentdb-java-getting-started). Futtassa például egy helyi könyvtárból az alábbi parancsot a mintaprojekt helyi lekéréséhez.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

A könyvtár neve tartalmazza a `pom.xml` a projekt és egy `src` Java forrás kód beleértve tartalmazó mappa `Program.java` mely bemutatja hogyan Azure Cosmos DB például a dokumentumok létrehozása és egy gyűjteményben lévő adatok lekérdezése az egyszerű műveleteket hajtanak végre. A `pom.xml` fájl tartalmaz egy [Maven DocumentDB Java SDK](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb)-függőséget.

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>3. lépés: Csatlakozás egy Azure Cosmos DB-fiókhoz
Térjen vissza az [Azure Portalra](https://portal.azure.com) a végpont és az elsődleges főkulcs beszerzéséért. Az Azure Cosmos DB végpont és az elsődleges kulcs ahhoz szükséges, hogy az alkalmazás tudja, hova kell csatlakoznia, az Azure Cosmos DB pedig megbízzon az alkalmazás által létesített kapcsolatban.

Az Azure Portalon lépjen a Azure Cosmos DB-fiókra, majd kattintson a **Kulcsok** elemre. Másolja ki az URI-t a portálról, és illessze be a Program.java fájl `https://FILLME.documents.azure.com` elemébe. Ezután másolja ki a PRIMARY KEY kulcsot a portálról, és illessze be a `FILLME` elembe.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Képernyőfelvétel a NoSQL-oktatóanyagban a Java konzolalkalmazás létrehozásához használt Azure Portalról. Megjelenít egy Azure Cosmos DB-fiókot, amelyen az ACTIVE központ, az Azure Cosmos DB-fiók panelén lévő KEYS gomb, valamint a Kulcsok panelen lévő URI, PRIMARY KEY és SECONDARY KEY értékek vannak kiemelve][keys]

## <a name="step-4-create-a-database"></a>4. lépés: Adatbázis létrehozása
Az Azure Cosmos [DB-adatbázis](documentdb-resources.md#databases) a **DocumentClient** osztály [createDatabase](/java/api/com.microsoft.azure.documentdb._document_client.createdatabase) metódusának használatával hozható létre. Az adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>5. lépés: Gyűjtemény létrehozása
> [!WARNING]
> A **createCollection** létrehoz egy fenntartott adattovábbítási kapacitással rendelkező új gyűjteményt, amely költségeket von maga után. További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

Egy [gyűjtemény](documentdb-resources.md#collections) a **DocumentClient** osztály [createCollection](/java/api/com.microsoft.azure.documentdb._document_client.createcollection) metódusával hozható létre. A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>6. lépés: JSON-dokumentumok létrehozása
A [dokumentumok](documentdb-resources.md#documents) a **DocumentClient** osztály [createDocument](/java/api/com.microsoft.azure.documentdb._document_client.createdocument) metódusával hozhatók létre. A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak. Most már beilleszthetünk egy vagy több dokumentumot. Ha már rendelkezik az adatbázisban tárolni kívánt adatokat, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) az adatok importálása az adatbázisba.

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

![A diagram a NoSQL-oktatóanyagban a Java konzolalkalmazás létrehozásához használt fiók, online adatbázis, gyűjtemény és dokumentumok hierarchikus kapcsolatát ábrázolja.](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. lépés: Az Azure Cosmos DB-erőforrások lekérdezése
Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).  Az alábbi mintakód bemutatja, hogyan kérdezheti le a dokumentumokat az Azure Cosmos DB-ben az SQL-szintaxis és a [queryDocuments](/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) metódus együttes használatával.

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>8. lépés: JSON-dokumentumok cseréje
Az Azure Cosmos DB támogatja a JSON-dokumentumok [replaceDocument](/java/api/com.microsoft.azure.documentdb._document_client.replacedocument) metódussal végrehajtott frissítését.

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>9. lépés: JSON-dokumentumok törlése
Hasonlóképpen az Azure Cosmos DB támogatja a JSON-dokumentumok [deleteDocument](/java/api/com.microsoft.azure.documentdb._document_client.deletedocument) metódussal végrehajtott törlését.  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>10. lépés: Az adatbázis törlése
A létrehozott adatbázis törlésével az adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.) is törlődik.

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>11. lépés: Futtassa a teljes Java konzolalkalmazást!
Az alkalmazás futtatásához a konzolról, keresse meg a projekt mappát, és fordítási Maven használatával:
    
    mvn package

A `mvn package` futtatása letölti a legújabb Azure Cosmos DB-erőforrástárat a Mavenről, és létrehozza a `GetStarted-0.0.1-SNAPSHOT.jar` fájlt. Ezután futtassa az alkalmazást az alábbi paranccsal:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Gratulálunk! Elvégezte a NoSQL-oktatóanyagot, és egy működőképes Java konzolalkalmazással rendelkezik!

## <a name="next-steps"></a>Következő lépések
* Szüksége van egy Java-webalkalmazás létrehozására vonatkozó oktatóanyagra? Tekintse meg a [Java-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-java-application.md) című cikket.
* Ismerje meg, hogyan [figyelhet egy Azure Cosmos DB-fiókot](monitor-accounts.md).
* Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.
* A programozási modellel kapcsolatos további tudnivalókat az [Azure Cosmos DB-dokumentációs oldalának](https://azure.microsoft.com/documentation/services/documentdb/) Develop (Fejlesztés) szakaszában találja.

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
