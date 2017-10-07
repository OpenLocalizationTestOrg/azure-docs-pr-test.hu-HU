---
title: "Azure Cosmos DB: Egy Java-Konzolalkalmazás létrehozása és a MongoDB API hello |} Microsoft Docs"
description: "Megadja a Java kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB MongoDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: fbe416f6b20ed2bb83a1d41eb70ffc6e3cee2b61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-mongodb-api-console-app-with-java-and-hello-azure-portal"></a>Azure Cosmos DB: Hozza létre egy Java MongoDB API-Konzolalkalmazás, és hello Azure-portálon

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon. Kell majd létrehozása és központi telepítése egy konzolalkalmazás hello épülő [MongoDB Java illesztőprogram](https://docs.mongodb.com/ecosystem/drivers/java/). 

## <a name="prerequisites"></a>Előfeltételek

* Ez a minta futtatásához, a következő előfeltételek hello kell rendelkeznie:
   * JDK 1.7+ (ha még nem rendelkezik a JDK-val, futtassa az `apt-get install default-jdk` parancsot)
   * Maven (ha nem rendelkezik Maven-nel, futtassa az `apt-get install maven` parancsot)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [mongodb-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="add-a-collection"></a>Gyűjtemény hozzáadása

Az új adatbázis neve legyen **db**, az új gyűjteményé pedig **coll**.

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük a githubból, a klón a MongoDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt. Láthatja, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-java-getting-started.git
    ```

3. Ezután nyissa meg a hello megoldásfájlt a Visual Studio. 

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello `Program.cs` fájlt, és látható, hogy ezek a sorok, a kód létrehozni hello Azure Cosmos DB-erőforrásokat. 

* hello DocumentClient inicializálva van.

    ```java
    MongoClientURI uri = new MongoClientURI("FILLME");`

    MongoClient mongoClient = new MongoClient(uri);            
    ```

* A rendszer létrehozza az új adatbázist és a gyűjteményt.

    ```java
    MongoDatabase database = mongoClient.getDatabase("db");

    MongoCollection<Document> collection = database.getCollection("coll");
    ```

* A rendszer beilleszt néhány dokumentumot a `MongoCollection.insertOne` használatával

    ```java
    Document document = new Document("fruit", "apple")
    collection.insertOne(document);
    ```

* A rendszer végrehajt néhány lekérdezést a `MongoCollection.find` használatával

    ```java
    Document queryResult = collection.find(Filters.eq("fruit", "apple")).first();
    System.out.println(queryResult.toJson());       
    ```

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. Hello fiók, jelölje ki **gyors üzembe helyezés**, válassza ki a Java, majd hello kapcsolati karakterlánc tooyour vágólapra másolása

2. Nyissa meg hello `Program.java` fájlt, cserélje le a hello argumentum toohello MongoClientURI konstruktor hello kapcsolati karakterlánccal. Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 
    
## <a name="run-hello-console-app"></a>Hello konzol alkalmazás futtatása

1. Futtatás `mvn package` terminál tooinstall igényelt az npm modult

2. Futtatás `mvn exec:java -D exec.mainClass=GetStarted.Program` a Terminálszolgáltatások toostart a Java-alkalmazást.

Ezután már használhatja [Robomongo](mongodb-robomongo.md) / [Studio 3T](mongodb-mongochef.md) tooquery, módosítására és az új adatokkal dolgozni.

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello adatkezelő használatával, és futtassa egy konzolalkalmazás. További adatok tooyour Cosmos DB fiókot most importálhatja. 

> [!div class="nextstepaction"]
> [MongoDB adatok importálása az Azure Cosmos DB-be](mongodb-migrate.md)


