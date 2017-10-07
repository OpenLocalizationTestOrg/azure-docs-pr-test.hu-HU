---
title: "Azure Cosmos DB: Fejlesztése a DocumentDB API .NET hello |} Microsoft Docs"
description: "Megtudhatja, hogyan toodevelop Azure Cosmos DB DocumentDB API-t a .NET használatával"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 0d3d17afa782054c8fdf3cbac421e5a5d0a6800c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmosdb-develop-with-hello-documentdb-api-in-net"></a>Azure CosmosDB: A DocumentDB API .NET hello fejleszthet

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

Ez az oktatóanyag bemutatja, hogyan egy Azure Cosmos DB fiók használatával toocreate hello Azure-portálon, és majd a dokumentum-adatbázis és gyűjtemény létrehozása egy [partíciókulcs](documentdb-partition-data.md#partition-keys) hello segítségével [DocumentDB .NET API](documentdb-introduction.md). Hozzon létre egy partíciókulcsot, ha létrehoz egy gyűjteményt, amelyet az alkalmazás készen áll a testreszabásra tooscale egyszerűen lehet az adatok növekedésével. 

Az útmutató tartalma hello alábbi feladatokat hello segítségével [DocumentDB .NET API](documentdb-sdk-dotnet.md):

> [!div class="checklist"]
> * Azure Cosmos DB-fiók létrehozása
> * Egy adatbázis és gyűjtemény létrehozása egy partíciós kulccsal
> * JSON-dokumentumok létrehozása
> * Dokumentum frissítése
> * A particionált gyűjtemények lekérdezése
> * A tárolt eljárások futtatása
> * Dokumentum törlése
> * Adatbázis törlése

## <a name="prerequisites"></a>Előfeltételek
Győződjön meg arról, hogy a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/). 
    * Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz, ha azt szeretné, hogy toouse emulálja a fejlesztéshez hello Azure DocumentDB szolgáltatás egy helyi környezet.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB-fiók létrehozása

Először hozzon létre egy Azure Cosmos DB fiókot a hello Azure-portálon.

> [!TIP]
> * Már van Azure Cosmos DB fiókja? Ha igen, hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS)
> * Kellett az Azure DocumentDB-fiókot? Ha igen, a fiókját most már Azure Cosmos DB fiókkal, és kihagyhatja azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS).  
> * Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Visual Studio megoldás beállítása](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>A Visual Studio-megoldás beállítása
1. Nyissa meg a **Visual Studiót** a számítógépén.
2. A hello **fájl** menüjében válassza **új**, és válassza a **projekt**.
3. A hello **új projekt** párbeszédablakban válassza **sablonok** / **Visual C#** / **Konzolalkalmazás (.NET-keretrendszer)**, nevezze el a projektet, és kattintson a **OK**.
   ![Képernyőfelvétel a hello új projekt ablakról](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-new-project-2.png)

4. A hello **Megoldáskezelőben**, kattintson az új konzolalkalmazásra, amely a Visual Studio megoldás alatt, a jobb gombbal, és kattintson a **NuGet-csomagok kezelése...**
    
    ![Képernyőfelvétel a hello jobbra a hello projekt helyi](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges.png)
5. A hello **NuGet** lapra, majd **Tallózás**, és írja be **documentdb** hello Keresés mezőbe.
<!---stopped here--->
6. Hello eredmények belül található **Microsoft.Azure.DocumentDB** kattintson **telepítése**.
   hello csomag azonosítója hello Azure Cosmos DB ügyféloldali kódtár [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB).
   ![Képernyőfelvétel a hello NuGet menüről a Azure Cosmos DB ügyfél SDK-kereséshez](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-manage-nuget-pacakges-2.png)

    Ha kapcsolatos módosításokat toohello megoldás áttekintése figyelmeztető hibaüzenet jelenik meg, kattintson a **OK**. Ha a licenc elfogadásáról szóló üzenetet kap, kattintson az **Elfogadom** gombra.

## <a id="Connect"></a>Hivatkozások tooyour projekt hozzáadása
az oktatóanyag adjon meg hello DocumentDB API kód kódtöredékek szükséges toocreate és a frissítés Azure Cosmos DB erőforrásokat a projekt fennmaradó hello szükséges lépések.

Először adja hozzá ezeket a hivatkozásokat tooyour alkalmazás.
<!---These aren't added by default when you install hello pkg?--->

```csharp
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

## <a id="add-references"></a>Az alkalmazás kapcsolódni

Ezután adja hozzá ezt a két állandót és a *ügyfél* változó az alkalmazás.

```csharp
private const string EndpointUrl = "<your endpoint URL>";
private const string PrimaryKey = "<your primary key>";
private DocumentClient client;
```

Ezt követően a head biztonsági toohello [Azure-portálon](https://portal.azure.com) tooretrieve a végponti URL-cím és az elsődleges kulcs. hello végponti URL-cím és elsődleges kulcs szükség az alkalmazás toounderstand ahol tooconnect esetén és Azure Cosmos DB tootrust az alkalmazás által létesített kapcsolatban.

Az Azure-portálon hello lépjen tooyour Azure Cosmos DB fiókot, kattintson **kulcsok**, és kattintson a **írható-olvasható kulcsok**.

Hello portálról hello URI másolása és beillesztése azt `<your endpoint URL>` hello program.cs fájlban. Akkor másolási elsődleges kulcs hello hello portálról, és illessze be keresztül `<your primary key>`. Lehet, hogy tooremove hello `<` és `>` a értékeiből.

![Képernyőfelvétel a hello Azure-portálon hello NoSQL-oktatóanyag toocreate által használt egy C# konzolalkalmazást. Egy Azure Cosmos DB fiókhoz, hello hello Azure Cosmos DB-fiók panelen lévő kulcsok és hello URI és hello kulcsok panelen lévő elsődleges kulcs értékét jeleníti meg](./media/tutorial-develop-documentdb-dotnet/nosql-tutorial-keys.png)

## <a id="instantiate"></a>Hello DocumentClient példányosítható

Most, hozzon létre egy új példányát hello **DocumentClient**.

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
```

## <a id="create-database"></a>Adatbázis létrehozása

Ezután hozzon létre egy Azure Cosmos DB [adatbázis](documentdb-resources.md#databases) hello segítségével [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) metódus vagy [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) hello metódusában ** DocumentClient** hello osztály [DocumentDB .NET SDK](documentdb-sdk-dotnet.md). Egy adatbázis a JSON-dokumentumtároló gyűjtemények között particionált logikai tárolója hello.

```csharp
await client.CreateDatabaseAsync(new Database { Id = "db" });
```
## <a name="decide-on-a-partition-key"></a>Adja meg a partíciós kulcs 

Gyűjtemények tárolói dokumentumok tárolására. Ezek a logikai erőforrások és is [egy vagy több fizikai partíció span](partition-data.md). A [partíciókulcs](documentdb-partition-data.md) a tulajdonság (vagy elérési út) belül van, amely használt toodistribute dokumentumok hello kiszolgálók között vagy partíció adatait. Hello ugyanazzal a partíciókulccsal tárolja az összes dokumentum hello egyazon partícióra kerüljenek. 

A partíciós kulcs egy fontos döntés toomake, egy gyűjtemény létrehozása előtt. Partíciókulcsok a tulajdonság (vagy elérési út) tartoznak, amelyek lehetnek dokumentumok Azure Cosmos DB toodistribute használja az adatok több kiszolgálók között vagy partíció. Cosmos DB hello a partíciós kulcs értéke csak, és használja a kivonatolt hello eredmény toodetermine hello partíció mely toostore hello dokumentumban. Hello ugyanazzal a partíciókulccsal tárolja az összes dokumentum hello egyazon partícióra kerüljenek, és partíciókulcsok gyűjtemény létrehozása után nem módosítható. 

Ebben az oktatóanyagban az oktatóanyagban módosítjuk tooset hello partíciós kulcs túl`/deviceId` úgy, amely hello hello minden adatot az egyetlen eszközt egyetlen partícióra van tárolva. Azt szeretné, hogy toochoose, amely rendelkezik, amelyek mindegyike használt értékek nagy számú partíciós kulcsa: hello kapcsolatos azonos gyakoriság tooensure Cosmos DB is a terhelést, ha az adatok növekszik, és hello teljes átviteli képessége – hello gyűjtemény elérése. 

Partícionálásra vonatkozó további információkért lásd: [hogyan toopartition és a skála Azure Cosmos DB?](partition-data.md) 

## <a id="CreateColl"></a>Gyűjtemény létrehozása 

Most, hogy tudjuk, hogy a partíciós kulcs `/deviceId`, lehetővé teszi, hogy hozzon létre egy [gyűjtemény](documentdb-resources.md#collections) hello segítségével [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) metódus vagy [ CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) hello metódusában **DocumentClient** osztály. A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló. 

> [!WARNING]
> Gyűjtemény létrehozása megegyezik megvalósítását, árképzési lefoglalja hello alkalmazás toocommunicate rendelkező Azure Cosmos DB adatátviteli sebességét. További részletekért tekintse meg a [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

```csharp
// Collection for device telemetry. Here hello JSON property deviceId is used  
// as hello partition key toospread across partitions. Configured for 2500 RU/s  
// throughput and an indexing policy that supports sorting against any  
// number or string property. .
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 2500 });
```

A metódus elérhetővé válnak a REST API-t hívja fel a tooAzure Cosmos DB, és hello szolgáltatás rendelkezések hello kért átviteli sebesség alapján létrehozott partícióknak számos. A teljesítmény igények fejlődnek hello SDK vagy hello segítségével módosíthatja hello átviteli gyűjtemény [Azure-portálon](set-throughput.md).

## <a id="CreateDoc"></a>JSON-dokumentumok létrehozása
Az Azure Cosmos DB szúrjon be néhány JSON-dokumentumokat. A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) hello metódusában **DocumentClient** osztály. A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak. Ez a minta az osztály tartalmaz egy eszköz olvasásra, és egy hívás tooCreateDocumentAsync tooinsert egy új eszközt egy gyűjteményhez olvasásakor.

```csharp
public class DeviceReading
{
    [JsonProperty("id")]
    public string Id;

    [JsonProperty("deviceId")]
    public string DeviceId;

    [JsonConverter(typeof(IsoDateTimeConverter))]
    [JsonProperty("readingTime")]
    public DateTime ReadingTime;

    [JsonProperty("metricType")]
    public string MetricType;

    [JsonProperty("unit")]
    public string Unit;

    [JsonProperty("metricValue")]
    public double MetricValue;
  }

// Create a document. Here hello partition key is extracted 
// as "XMS-0001" based on hello collection definition
await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "coll"),
    new DeviceReading
    {
        Id = "XMS-001-FE24C",
        DeviceId = "XMS-0001",
        MetricType = "Temperature",
        MetricValue = 105.00,
        Unit = "Fahrenheit",
        ReadingTime = DateTime.UtcNow
    });
```
## <a name="read-data"></a>Adatok olvasása

Most elolvasni hello dokumentumot a partíciókulcs és hello ReadDocumentAsync metódussal azonosítója. Vegye figyelembe, hogy a hello olvasások PartitionKey értéket tartalmazza (megfelelő toohello `x-ms-documentdb-partitionkey` hello REST API-t a kérelem fejlécében).

```csharp
// Read document. Needs hello partition key and hello Id toobe specified
Document result = await client.ReadDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

DeviceReading reading = (DeviceReading)(dynamic)result;
```

## <a name="update-data"></a>Adatok frissítése

Most tegyük a néhány hello ReplaceDocumentAsync metódussal adatainak frissítése.

```csharp
// Update hello document. Partition key is not required, again extracted from hello document
reading.MetricValue = 104;
reading.ReadingTime = DateTime.UtcNow;

await client.ReplaceDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  reading);
```

## <a name="delete-data"></a>Adat törlése

Most már lehetővé teszi, hogy a dokumentum által partíciókulcs és azonosító törlése hello DeleteDocumentAsync módszer használatával.

```csharp
// Delete a document. hello partition key is required.
await client.DeleteDocumentAsync(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```
## <a name="query-partitioned-collections"></a>A particionált gyűjtemények lekérdezése

Amikor a adatait a particionált gyűjtemények, Azure Cosmos DB automatikusan útvonalak hello lekérdezési toohello partíciók megfelelő toohello partíció megadott kulcsértékek hello szűrő (ha vannak ilyenek). Például egy, a lekérdezés nem útválasztásos toojust hello partíció tartalmazó hello partíciós kulcs "XMS-0001".

```csharp
// Query using partition key
IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"))
    .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");
```
    
hello következő lekérdezés nincs szűrő hello partíciókulcs (DeviceId) és tooall partíciók hol végre hello partíció index alapján van rendezve. Vegye figyelembe, hogy rendelkezik-e toospecify hello EnableCrossPartitionQuery (`x-ms-documentdb-query-enablecrosspartition` a hello REST API-t) toohave hello SDK tooexecute közötti partíciók lekérdezést.

```csharp
// Query across partition keys
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true })
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);
```

## <a name="parallel-query-execution"></a>Párhuzamos lekérdezés-végrehajtás
hello Azure Cosmos DB DocumentDB SDK 1.9.0 és támogatási fent párhuzamos lekérdezés végrehajtási beállítások, melyek lehetővé teszik tooperform kis késleltetésű lekérdezi a particionált gyűjtemények szemben még akkor is, ha szükségük van tootouch partíciók nagy számú. Például a következő lekérdezés hello partíciók nem konfigurált toorun párhuzamosan.

```csharp
// Cross-partition Order By queries
IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
    UriFactory.CreateDocumentCollectionUri("db", "coll"), 
    new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
    .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
    .OrderBy(m => m.MetricValue);
```
    
A következő paraméterek hello hangolása kezelheti párhuzamos lekérdezés-végrehajtás:

* Úgy, hogy `MaxDegreeOfParallelism`, azaz hello egyidejű hálózati kapcsolatok toohello gyűjtemény a partíciók száma párhuzamos hello fokú szabályozhatja. Ha ez túl-1, párhuzamossági hello fokát hello SDK kezeli. Ha hello `MaxDegreeOfParallelism` értéke nincs megadva, illetve too0, amely hello alapértelmezett érték, egy egyetlen hálózati kapcsolat toohello gyűjtemény partíciók lesz.
* Úgy, hogy `MaxBufferedItemCount`, akkor is kompromisszumot lekérdezés-késleltetés és ügyféloldali memóriahasználata. Ha kihagyja ezt a paramétert, vagy állítsa 1 túl, hello száma párhuzamos lekérdezés-végrehajtás során pufferelt elemek hello SDK kezeli.

Hello megadott hello gyűjtemény olyan állapotban, a párhuzamos lekérdezések lesz az eredményeket hello ugyanaz, mint soros végrehajtási sorrendben. Rendezés (ORDER BY és/vagy felső) tartalmazó kereszt-partíció lekérdezés végrehajtásakor a hello DocumentDB SDK-t állít ki hello lekérdezés párhuzamosan partíciók között, és egyesíti hello ügyfél oldalán tooproduce globálisan rendezett eredmények részben rendezett eredményez.

## <a name="execute-stored-procedures"></a>Tárolt eljárások végrehajtása
Végül, Ön is végrehajthatja a dokumentumokon végzett atomi tranzakciók ugyanazon Eszközazonosítót, pl. hello Ha éppen karbantartása összesítések, vagy hello egyetlen dokumentum eszköz aktuális állapotát a következő kód tooyour projekt hello hozzáadásával.

```csharp
await client.ExecuteStoredProcedureAsync<DeviceReading>(
    UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
    new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
    "XMS-001-FE24C");
```

És ennyi az egész! most már az hello fő összetevői közötti partíciók partíciós kulcs tooefficiently méretezési adatok terjesztési használó Azure Cosmos DB alkalmazás.  

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást oktatóprogram során létrehozott hello az Azure-portálon az alábbi lépésekkel hello:

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello egyedi nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek: 

> [!div class="checklist"]
> * Egy Azure Cosmos DB-fiók létrehozása
> * Egy adatbázis és gyűjtemény létrehozása egy partíciós kulccsal
> * Létrehozott JSON-dokumentumok
> * A dokumentumok frissítése
> * A particionált gyűjtemények lekérdezése
> * A tárolt eljárás futott
> * A dokumentum törlése
> * Adatbázis törlése

Ezután folytassa a következő oktatóanyag toohello és további adatok tooyour Cosmos DB fiók importálása. 

> [!div class="nextstepaction"]
> [Adatok importálása az Azure Cosmos DB-be](import-data.md)
