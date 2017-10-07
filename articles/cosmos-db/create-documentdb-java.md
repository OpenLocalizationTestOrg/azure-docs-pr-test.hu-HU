---
title: "egy Azure Cosmos DB dokumentum adatbázist, Java aaaCreate |} Microsoft dokumentumok |} Microsoft Docs"
description: "Megadja a Java kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 08/02/2017
ms.author: mimig
ms.openlocfilehash: 400c9e7780034d3e28d749e734786e950edad22f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-hello-azure-portal"></a>Azure Cosmos DB: Hozzon létre egy dokumentum adatbázist, Java használatával, és hello Azure-portálon

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezés dokumentum létrehozása az Azure portál eszközök adatbázis használatával hello Azure Cosmos DB. A gyors üzembe helyezés azt is bemutatja, hogyan tooquickly hozzon létre egy Java-Konzolalkalmazás használatával hello [DocumentDB Java API](documentdb-sdk-java.md). hello utasításait a gyors üzembe helyezés követhetők bármely operációs rendszeren, amely alkalmas a Java futtatására. A gyors üzembe helyezés befejezése lesz ismeri a létrehozása és módosítása a dokumentum adatbázis erőforrásainak vagy hello felhasználói felületén, vagy programozottan, amelyik igény szerint.

## <a name="prerequisites"></a>Előfeltételek

* [Java fejlesztői készlet (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Ubuntu, futtassa `apt-get install default-jdk` tooinstall hello JDK.
    * Lehet, hogy tooset hello JAVA_HOME környezeti változó toopoint toohello mappa hello JDK futtató.
* [Maven](http://maven.apache.org/download.cgi) bináris archívum [letöltése](http://maven.apache.org/install.html) és [telepítése](http://maven.apache.org/)
    * Ubuntu, futtathatja `apt-get install maven` tooinstall Maven.
* [Git](https://www.git-scm.com/)
    * Ubuntu, futtathatja `sudo apt-get install git` tooinstall Git.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

A dokumentum-adatbázis létrehozása előtt kell toocreate egy Cosmos-DB Azure SQL (DocumentDB) adatbázis fiókja.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Gyűjtemény hozzáadása

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Mintaadatok hozzáadása

Most tooyour új adatgyűjtés adatkezelő használatával adhat hozzá.

1. Új adatbázis hello Data Explorer hello Gyűjtemények ablaktáblán jelenik meg. Bontsa ki a hello **feladatok** adatbázisra, és bontsa ki a hello **elemek** gyűjtemény, kattintson a **dokumentumok**, és kattintson a **új dokumentumok**. 

   ![Hozzon létre új dokumentumok az adatkezelő a hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. A struktúra a következő hello mostantól hozzáadhatja azok toohello dokumentumgyűjteményt.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Hello json toohello hozzáadása után **dokumentumok** lapra, majd **mentése**.

    ![Másolja át json-adatokat, és kattintson a Mentés gombra az adatok Explorer hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Hozzon létre és mentsen egy egyedi értéket hello helyezze egy további dokumentum `id` tulajdonság, és a változás hello más tulajdonságok, ahogyan szeretné. Mivel az Azure Cosmos DB nem kötelezi egy adott adatséma használatára, új dokumentumaihoz bármilyen struktúrát választhat.

     Most használja lekérdezi az adatkezelő tooretrieve az adatok hello kattintva is **szűrő szerkesztése** és **szűrés** gombokat. Alapértelmezés szerint adatkezelő használja `SELECT * FROM c` összes hello gyűjtemény, de a dokumentumok tooretrieve módosíthatja, hogy különböző tooa [SQL-lekérdezés](documentdb-sql-query.md), például a `SELECT * FROM c ORDER BY c._ts DESC`, csökkenő sorrendben minden hello dokumentum alapján tooreturn az időbélyegző. 
 
     Is használhatja adatkezelő toocreate tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooperform kiszolgálóoldali üzleti logikát is mint méretezési átviteli sebesség. Adatkezelő mutatja meg az összes hello beépített programozott adatelérési hello API-k érhető el, de hello Azure-portálon található egyszerű a hozzáférés tooyour adatokat biztosít.

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük kapcsoló tooworking kóddal. Most klónozza a Githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt a DocumentDB API-alkalmazásba. Ön meg, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `CD` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello `Program.java` hello \src\GetStarted mappából fájlt, és ezek a sorok, a kód által létrehozott hello Azure Cosmos DB erőforrások található. 

* Hello `DocumentClient` inicializálva van.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* A rendszer létrehozza az új adatbázist.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* A rendszer létrehozza az új gyűjteményt.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* A rendszer létrehoz néhány dokumentumot.

    ```java
    // Any Java object within your code can be serialized into JSON and written tooAzure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* A egy SQL-lekérdezést hajt végre a JSON-on.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja. Ez lehetővé teszi az alkalmazás toocommunicate az üzemeltetett adatbázissal.

1. A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**. Fogjuk hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs a hello `Program.java` fájl hello következő lépésben.

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-dotnet/keys.png)

2. Hello nyissa meg a `Program.java` fájlt, másolja az URI érték portálról hello (hello Másolás gombra), és könnyebben hello végpont toohello DocumentClient konstruktor a értékének hello `Program.java`. 

    `"https://FILLME.documents.azure.com"`

4. Ezután másolja az elsődleges kulcs-érték hello portálról, és illessze be azt "FILLME", így keresztül hello hello DocumentClient konstruktor második paramétere. Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 
    
## <a name="run-hello-app"></a>Hello alkalmazás futtatása

1. A hello git terminálablakot `cd` toohello azure-cosmos-db-documentdb-java-getting-started mappa.

2. Írja be a hello git terminálablakot, `mvn package` tooinstall hello szükséges Java-csomagok.

3. Hello git terminál-ablakban futtassa `mvn exec:java -D exec.mainClass=GetStarted.Program` a hello terminálablakot toostart a Java-alkalmazást.

    A hello terminálablakot értesíti arról, hogy az adatbázis készült FamilyDB hello és egy kulcs toocontinue toopress kap. Nyomja le az ENTER kulcs toocreate hello adatbázis, majd váltson az adatkezelő toohello, és látni fogja, hogy most már tartalmaz egy FamilyDB adatbázis. Továbbra is toopress kulcsok toocreate hello adatgyűjtési és -hello dokumentumokat, és végezze el a lekérdezést. Hello projekt befejezését követően hello erőforrások törlése fiókjából. 

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés mér megismerte, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény hello adatkezelő használatával, és futtassa az alkalmazást toodo hello ugyanaz programozott módon. További adatok tooyour Cosmos DB fiókot most importálhatja. 

> [!div class="nextstepaction"]
> [Adatok importálása az Azure Cosmos DB-be](import-data.md)


