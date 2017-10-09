---
title: "Az Azure Cosmos DB: Hello Graph API a .NET fejlesztést |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop Azure Cosmos DB DocumentDB API-t a .NET használatával"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.custom: mvc
ms.openlocfilehash: 12e435d8169aeee6e818dac4a3b66c7a0ec5f2d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-develop-with-hello-graph-api-in-net"></a>Azure Cosmos DB: A Graph API a .NET hello fejlesztést
Azure Cosmos-adatbázis egy Microsoft globálisan elosztott több modellre adatbázis szolgáltatás. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

Ez az oktatóanyag bemutatja, hogyan egy Azure Cosmos DB fiók használatával toocreate hello Azure-portálon, és hogyan toocreate egy grafikonon adatbázis és a tároló. hello alkalmazás ezután létrehoz egy egyszerű közösségi hálózati hello segítségével négy személyekkel [Graph API](graph-sdk-dotnet.md) (előzetes verzió), majd halad át, és lekérdezi a hello graph Gremlin használatával.

Ez az oktatóanyag ismerteti a következő feladatok hello:

> [!div class="checklist"]
> * Azure Cosmos DB-fiók létrehozása 
> * Hozzon létre egy grafikonon adatbázis és a tároló
> * És a csúcsban too.NET objektumokat szerializálni
> * Adja hozzá a csúcsban és élei számára
> * Lekérdezés hello graph Gremlin használatával

## <a name="graphs-in-azure-cosmos-db"></a>Az Azure Cosmos DB grafikonja
Használhat Azure Cosmos DB toocreate, a frissítés és a lekérdezés diagramokat hello segítségével [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) könyvtárban. hello Microsoft.Azure.Graph tár egyetlen bővítmény módszert biztosít a `CreateGremlinQuery<T>` fölött hello `DocumentClient` tooexecute Gremlin lekérdezések osztályban.

Gremlin egy funkcionális programozási nyelv, amely támogatja az írási műveletek (DML) és a lekérdezés- és átjárás műveletei. A lépések Gremlin Ez a cikk tooget található néhány példa azt foglalkozik. Lásd: [Gremlin lekérdezések](gremlin-support.md) Gremlin lehetőségeinek Azure Cosmos DB részletes útmutatást. 

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/). 
    * Másik lehetőségként használhatja a hello [Azure DocumentDB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Adatbázis-fiók létrehozása

Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.  

> [!TIP]
> * Már van Azure Cosmos DB fiókja? Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS)
> * Kellett az Azure DocumentDB-fiókot? Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).  
> * Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>A Visual Studio-megoldás beállítása
1. Nyissa meg a **Visual Studiót** a számítógépén.
2. A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.
3. A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás (.NET-keretrendszer)**, nevezze el a projektet, és kattintson a **OK**.
4. A hello **Megoldáskezelőben**, kattintson az új konzolalkalmazásra, amely a Visual Studio megoldás alatt, a jobb gombbal, és kattintson a **NuGet-csomagok kezelése...**
5. A hello **NuGet** lapra, majd **Tallózás**, és írja be **Microsoft.Azure.Graphs** hello keresési mezőbe, és ellenőrzés hello **előzetes verzióitartalmazza**.
6. Hello eredmények belül található **Microsoft.Azure.Graphs** kattintson **telepítése**.
   
   Ha kapcsolatos módosításokat toohello megoldás áttekintése figyelmeztető hibaüzenet jelenik meg, kattintson a **OK**. Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.
   
    Hello `Microsoft.Azure.Graphs` tár egyetlen bővítmény módszert biztosít a `CreateGremlinQuery<T>` Gremlin műveletek hajthatók végre. Gremlin egy funkcionális programozási nyelv, amely támogatja az írási műveletek (DML) és a lekérdezés- és átjárás műveletei. A lépések Gremlin Ez a cikk tooget található néhány példa azt foglalkozik. [Gremlin lekérdezések](gremlin-support.md) rendelkezik Gremlin képességek részletes útmutató az Azure Cosmos Adatbázisba.

## <a id="add-references"></a>Az alkalmazás kapcsolódni

Adja hozzá ezt a két állandót és a *ügyfél* változó az alkalmazás. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
A következő head biztonsági toohello [Azure-portálon](https://portal.azure.com) tooretrieve a végponti URL-cím és az elsődleges kulcs. hello végponti URL-cím és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban. 

Az Azure-portálon hello lépjen tooyour Azure Cosmos DB fiókot, kattintson **kulcsok**, és kattintson a **írható-olvasható kulcsok**. 

Hello portálról hello URI másolása és beillesztése azt `Endpoint` a fenti hello endpoint tulajdonság. Ezután másolása az elsődleges kulcs hello hello portálról, és illessze be hello `AuthKey` fenti tulajdonság. 

! [Képernyőfelvétel a hello Azure-portál által használt hello oktatóanyag toocreate C#-alkalmazás. Megjeleníti egy Azure Cosmos DB fiók hello a KEYS gomb kiemelve meg hello Azure Cosmos DB navigációs és hello URI és PRIMARY KEY értékek kiemelésével hello a kulcsok panelen] [kulcsok] 
 
## <a id="instantiate"></a>Hello DocumentClient példányosítható 
Ezután hozzon létre egy új példányát hello **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Adatbázis létrehozása 

Ezután hozzon létre egy Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódus vagy [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) hello metódusában  **DocumentClient** hello osztály [DocumentDB .NET SDK](documentdb-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Hozzon létre egy grafikonon 

Ezután hozzon létre egy grafikonon tároló hello segítségével hello segítségével [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódus vagy [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) hello metódusában **DocumentClient**  osztály. A gyűjtemény egy grafikonon entitások tároló. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>És a csúcsban too.NET objektumokat szerializálni
Azure Cosmos DB használ hello [GraphSON egybeírt](gremlin-support.md), amely megadja, hogy a csúcsban, szélek és tulajdonságait egy JSON-séma. hello Azure Cosmos DB .NET SDK magában foglalja a függőség beállításához JSON.NET, és ez lehetővé teszi tooserialize/deszerializálás GraphSON .NET objektumokba azt kód együtt is használható.

Tegyük fel most dolgozni egy egyszerű közösségi hálózati négy személyekkel. Úgy tekintünk, hogyan toocreate `Person` csúcsban, adja hozzá `Knows` köztük lévő viszonyt is, majd lekérdezési és hello graph toofind "" Friend "friend" kapcsolatokat haladnak át. 

Hello `Microsoft.Azure.Graphs.Elements` névteret biztosít `Vertex`, `Edge`, `Property` és `VertexProperty` osztályok GraphSON válaszok toowell által definiált .NET objektum deszerializálása során.

## <a name="run-gremlin-using-creategremlinquery"></a>Gremlin CreateGremlinQuery használatával futtassa
Gremlin, például az SQL, olvasási, írási és lekérdezési műveletek támogatja. Például hello alábbi kódrészletben láthatja, hogyan hajtsa végre a toocreate csúcsban, szélén, az egyes mintalekérdezések `CreateGremlinQuery<T>`, és aszinkron módon iterációt ezekkel az eredményekkel használatával `ExecuteNextAsync` és "HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // hello CreateGremlinQuery method extensions allow you tooexecute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Adja hozzá a csúcsban és élei számára

Nézzük hello Gremlin utasítások megjelenő szakasz megelőző hello további információkhoz juthat. Első azt néhány Gremlin tartozó használatával csúcsban `addV` metódust. Például hello következő kódrészlettel hoz létre a "Személy" típusú "Thomas Andersen" csúcspont utónevét, vezetéknevét és kora tulajdonságait.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Ezután azt néhány szélek között ezek csúcsban Gremlin tartozó használatával hoz létre `addE` metódust. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Egy meglévő csúcspont segítségével frissítheti azt `properties` Gremlin lépést. A Microsoft hello hívás tooexecute hello lekérdezés keresztül kihagyása `HasMoreResults` és `ExecuteNextAsync` hello többi hello példák.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Szegélyek és Gremlin tartozó használatával csúcsban elvetné `drop` lépés. Íme egy kódrészletet, amely bemutatja, hogyan toodelete a csomópontra, és az edge. Vegye figyelembe, hogy az eldobása csúcspont végez a hello kaszkádolt delete társított szélén.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-hello-graph"></a>Lekérdezés hello diagramhoz

Lekérdezések és traversals Gremlin is használatával végezheti el. Például a következő kódrészletet hello bemutatja, hogyan toocount hello hello Graph csúcsban száma:

```cs
// Run a query toocount vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Szűrők Gremlin tartozó használatával végezheti el `has` és `hasLabel` lépéseit, és egyesíthet használatával `and`, `or`, és `not` toobuild összetettebb szűrők:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Kivetítheti az egyes tulajdonságok hello lekérdezés eredményében hello segítségével `values` . lépés:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Eddig is csak láttuk lekérdezési operátorok, amelyek működnek a bármely adatbázis. Diagramok esetén gyors és hatékony átjárás műveletekhez toonavigate toorelated szélek és csúcsban van szükség. Keressük Thomas összes barátok. Jelenleg ezt úgy teheti meg Gremlin `outE` toofind összes hello kimenő éleinek Thomas a lépést, majd toohello a-csúcsban áthaladó Gremlin tartozó használatával szegélyek a `inV` . lépés:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

hello a következő lekérdezés hajt végre két ugrások toofind összes Thomas' "ismerősök olyan ismerőseinek", meghívásával `outE` és `inV` kétszer. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Összetettebb lekérdezések létrehozhatja és hatékony graph átjárás logika használatával Gremlin, többek között a következőket keverési szűrő kifejezések használatával ismétlési végrehajtása hello megvalósítása `loop` lépést, és végrehajtási feltételes navigációs hello segítségével `choose` lépés. További információ a teendők [Gremlin támogatási](gremlin-support.md)!

Ennyi az egész, az Azure Cosmos DB oktatóanyag befejeződött! 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha használja a következő lépéseket toodelete hello Azure-portálon az oktatóprogram során létrehozott összes erőforrás hello.  

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Egy Azure Cosmos DB-fiók létrehozása 
> * Egy grafikonon adatbázis és a tároló létrehozása
> * A szerializált és a csúcsban too.NET objektumok
> * A hozzáadott csúcsban és élei számára
> * Lekérdezett hello graph Gremlin használatával

Most már készen áll arra, hogy a Gremlin használatával összetettebb lekérdezéseket hozzon létre és hatékony gráfbejárási logikákat implementáljon. 

> [!div class="nextstepaction"]
> [Lekérdezés a Gremlin használatával](tutorial-query-graph.md)
