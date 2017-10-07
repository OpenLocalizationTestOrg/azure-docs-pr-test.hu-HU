---
title: "egy Azure Cosmos DB .NET alkalmazást, amely aaaBuild hello Graph API-val |} Microsoft Docs"
description: "Megadja a .NET kódminta tooconnect tooand használható Azure Cosmos DB lekérdezése"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/28/2017
ms.author: denlee
ms.openlocfilehash: f28790fcb8c9f57c7bb3d844d8276fa04abcc39c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-graph-api"></a>Azure Cosmos DB: Hello Graph API segítségével .NET-alkalmazás létrehozása

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, az adatbázis és a graph (tároló) használatával hello Azure-portálon. Majd létrehozása és futtatása egy konzolalkalmazás hello épülő [Graph API](graph-sdk-dotnet.md) (előzetes verzió).  

## <a name="prerequisites"></a>Előfeltételek

Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Gráf hozzáadása

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük a githubból, a Klónozás egy grafikonon API app hello kapcsolati karakterlánc beállítása, és futtassa azt. Láthatja, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Nyissa meg a Visual Studio és a nyitott hello megoldásfájlt. 

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello Program.cs fájlt, és, hogy ezek a sorok, a kód létrehozása hello Azure Cosmos DB erőforrások találhat. 

* hello DocumentClient inicializálva van. A képen hello egy grafikonon bővítmény API hello Azure Cosmos DB ügyfélen hozzáadott. Egy önálló graph ügyfél hello Azure Cosmos DB ügyfél és az erőforrások különválik dolgozunk.

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* A rendszer létrehozza az új adatbázist.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* A rendszer létrehozza az új gráfot.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* Hello Gremlin lépések egy sorozatát végrehajtása `CreateGremlinQuery` metódust.

    ```csharp
    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. Nyissa meg a Visual Studio 2017 hello App.config fájlt. 

2. Hello Azure portál Azure Cosmos DB fiókjában kattintson **kulcsok** a bal oldali navigációs hello. 

    ![Megtekintése és másolása az elsődleges kulcs hello hello kulcsok oldalon Azure-portálon](./media/create-graph-dotnet/keys.png)

3. Másolás a **URI** hello portálról értékét, és könnyebben hello az App.config fájlban hello végpont kulcs értékét. Képernyőkép toocopy hello érték megelőző hello látható módon használhatja hello Másolás gombra.

    `<add key="Endpoint" value="https://FILLME.documents.azure.com:443" />`

4. Másolás a **elsődleges kulcs** hello portal értéket, és teszi hello az App.config fájlban hello AuthKey kulcs értékét, majd mentse a módosításokat. 

    `<add key="AuthKey" value="FILLME" />`

Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 

## <a name="run-hello-console-app"></a>Hello konzol alkalmazás futtatása

1. A Visual Studióban, kattintson a jobb gombbal a hello **GraphGetStarted** a projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**. 

2. A hello NuGet **Tallózás** mezőbe írja be *Microsoft.Azure.Graphs* , és ellenőrizze a hello **prerelease tartalmazza** mezőbe. 

3. Hello eredmények közül telepítse a hello **Microsoft.Azure.Graphs** könyvtárban. Ezzel telepít hello Azure Cosmos DB diagram bővítmény kódtár csomagja és az összes függősége.

    Ha kapcsolatos módosításokat toohello megoldás áttekintése figyelmeztető hibaüzenet jelenik meg, kattintson a **OK**. Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.

4. Kattintson a CTRL + F5 toorun hello alkalmazás.

   hello konzolablak hello csomópontok és a hozzáadni kívánt toohello graph jeleníti meg. Hello parancsprogram befejezését, nyomja le az ENTER kétszer tooclose hello console ablakban. 

## <a name="browse-using-hello-data-explorer"></a>Keresse meg a hello adatkezelő használatával

Ezután lépjen vissza tooData Explorer hello Azure-portálon a és keresse meg és az új diagram adatait kéri.

1. Az adatok Explorer hello új adatbázis hello diagramjait ablaktáblán jelenik meg. Bontsa ki a **graphdb**, **graphcollz** pontokat, és kattintson a **Gráf** lehetőségre.

2. Kattintson a hello **szűrés** gomb toouse hello alapértelmezett lekérdezési tooview összes hello verticies hello gráfban. hello minta alkalmazás által generált hello adatok hello diagramjait ablaktáblán jelennek meg.

    Mindkét hello graph ráközelíthet, bontsa ki a hello diagram megjelenítési terület, hozzáadhat további verticies, és helyezze át a hello verticies felületét jeleníti meg.

    ![Hello diagram megtekintése az adatok Explorerben a hello Azure-portálon](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello: 

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy grafikonon hello adatkezelő használatával, és futtassa az alkalmazást. Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon. 

> [!div class="nextstepaction"]
> [Lekérdezés a Gremlin használatával](tutorial-query-graph.md)

