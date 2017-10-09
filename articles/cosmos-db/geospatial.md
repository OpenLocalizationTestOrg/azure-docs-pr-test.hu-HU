---
title: "Azure Cosmos DB földrajzi adatokat aaaWorking |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, index és az Azure Cosmos DB térbeli objektumok lekérdezéséhez és hello DocumentDB API."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: 82ce2898-a9f9-4acf-af4d-8ca4ba9c7b8f
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 05/22/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1e40b78cb4595631d845d46c21d07a30c8b972f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a>A földrajzi és GeoJSON helyre adatokat az Adatbázisba az Azure Cosmos használata
Ez a cikk egy bevezető toohello földrajzi funkcióit [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Után olvassa, fogja tudni tooanswer hello a következő kérdéseket:

* Térbeli adatok tárolása az Azure Cosmos-Adatbázisba
* Hogyan tudja lekérdezni az SQL és a LINQ Azure Cosmos Adatbázisba földrajzi adatok?
* Hogyan engedélyezése vagy letiltása, az Azure Cosmos Adatbázisba térbeli indexelő?

Ez a cikk bemutatja, hogyan toowork a térbeli adatokat tartalmazó hello DocumentDB API. Lásd: a [GitHub-projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) mintakódok számára.

## <a name="introduction-toospatial-data"></a>Bevezetés toospatial adatok
Térbeli adatok hello és adott hely objektumaira alakját ismerteti. A legtöbb alkalmazásban ezek megegyeznek a tooobjects hello a föld, azaz a földrajzi adatok. Lehet, hogy az térbeli adatokat használt toorepresent hello hely a személy, érdeklő olyan helyen, vagy a város, vagy egy lake hello határát. Gyakori alkalmazási esetei gyakran közelségi kapcsolat lekérdezéseket, pl. "található összes kávézókban a jelenlegi hely közelében" foglalják magukba. 

### <a name="geojson"></a>GeoJSON
Azure Cosmos-adatbázis támogatja az indexelő és földrajzi pont hello segítségével ábrázolt adatok lekérdezését [GeoJSON specification](https://tools.ietf.org/html/rfc7946). GeoJSON adatstruktúrák mindig érvényes JSON-objektumok, hogy tárolhatók és lekérdezett Azure Cosmos DB használatával speciális eszközöket és szalagtárak nélkül. hello Azure Cosmos DB SDK-k segítőosztályok és módszerek, amelyekkel könnyen toowork térbeli adatokat tartalmazó biztosítanak. 

### <a name="points-linestrings-and-polygons"></a>Pontok, Linestring és sokszögek
A **pont** helyre egyetlen pozíció jelöli. A földrajzi adatok, az a pont hello pontos helyet, amely lehet egy címe élelmiszerboltban áruházbeli, a teljes képernyős, egy autó vagy a város jelöli.  A pont a GeoJSON (és Azure Cosmos DB) használata a koordináta pár vagy szélességi és hosszúsági jelzi. Íme egy példa JSON pontnál.

**Az Azure Cosmos DB pontjai**

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> hello GeoJSON meghatározása a földrajzi hosszúság értéke határozza meg az első és az a földrajzi hosszúság második. Például a többi leképezési alkalmazások szélességi és hosszúsági szögek és fok formájában jelennek meg. A szélességi hello mérik és-180 és 180.0 fok, és a szélesség között hosszúsági értékeknek értékek vannak mért hello Egyenlítőtől és-90.0 között és 90.0 fok. 
> 
> Azure Cosmos DB értelmezi a koordináták / hello WGS-84 referenciarendszer határozzák. Lásd az alábbiakat koordináta referenciarendszert kapcsolatos további részletekért.
> 
> 

Ez lehet beágyazni egy Azure Cosmos DB dokumentumban helyre adatokat tartalmazó felhasználói profil e példában látható módon:

**Az Azure Cosmos Adatbázisba tárolási helye a profil használata**

```json
{
    "id":"documentdb-profile",
    "screen_name":"@CosmosDB",
    "city":"Redmond",
    "topics":[ "global", "distributed" ],
    "location":{
        "type":"Point",
        "coordinates":[ 31.9, -4.8 ]
    }
}
```

Ezenkívül toopoints, GeoJSON Linestring és sokszögek is támogatja. **Linestring** területen legalább két pontok jelentenek, és csatlakoztassa őket sor szegmensek hello. A földrajzi adatok Linestring a gyakran használt toorepresent közútvonalakon és folyókat. A **sokszög** összekapcsolja az egy lezárt LineString csatlakoztatott pontok-határ. Sokszögek olyan gyakran használt toorepresent természetes kialakulásához például tavakat vagy például városokat és állapotok politikai joghatóság alá tartozó területeken. Íme egy példa az Azure Cosmos Adatbázisba sokszög. 

**A GeoJSON sokszögek**

```json
{
    "type":"Polygon",
    "coordinates":[
        [ 31.8, -5 ],
        [ 31.8, -4.7 ],
        [ 32, -4.7 ],
        [ 32, -5 ],
        [ 31.8, -5 ]
    ]
}
```

> [!NOTE]
> hello GeoJSON specifikációja megköveteli, hogy érvényes sokszögek hello megadott utolsó koordináta pár kell lennie mint hello első, toocreate zárt alakzat hello azonos.
> 
> Egy Sokszögön belül pontok balra érdekében meg kell adni. A sokszög jobbra sorrendben megadva hello régióját, hello inverzét jelöli.
> 
> 

Továbbá tooPoint, LineString, Polygon, és GeoJSON is megadja, hogy hogyan hello ábrázolását toogroup több földrajzi helyen, és tetszőleges tulajdonság tooassociate földrajzi hely meghatározásának, mint egy **szolgáltatás**. Mivel ezek az objektumok érvényes JSON-adatokat, akkor is összes tárolása és feldolgozása történhet az Azure Cosmos-Adatbázisba. Azonban Azure Cosmos DB csak akkor támogatja a pontok automatikus indexeléshez.

### <a name="coordinate-reference-systems"></a>Ütemezze referenciarendszert
Mivel a hello earth hello alakját szabálytalan, koordinátáját földrajzi adatok a hány koordináta referenciarendszert (CRS), mindegyiket a saját keretek a referencia- és mértékegységek jelzi. Például "Britannia nemzeti rács" hello egy referenciarendszert nagyon pontos hello Egyesült Királyságban, de nem kívül. 

hello használja a legnépszerűbb CRS ma hello World geodéziai rendszer [WGS-84](http://earth-info.nga.mil/GandG/wgs84/). GPS-eszközöket, és számos leképezési szolgáltatás, beleértve a Google térképeket és a Bing térképek API-k használata WGS-84. Azure Cosmos-adatbázis indexelő és WGS-84 CRS hello segítségével a földrajzi adatok lekérdezését támogatja. 

## <a name="creating-documents-with-spatial-data"></a>Térbeli adatokat tartalmazó dokumentumok létrehozása
GeoJSON értékeket tartalmazó dokumentumok létrehozásához automatikusan indexelt az egy térbeli index a összhangban toohello indexelési házirendet hello gyűjtemény. Ha egy Azure Cosmos DB SDK dinamikusan gépelt Python vagy Node.js nyelven dolgozik, érvényes GeoJSON kell létrehoznia.

**A földrajzi adatok node.js dokumentum létrehozása**

```json
var userProfileDocument = {
    "name":"documentdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within hello callback
});
```

Hello DocumentDB API-k használata, ha használható hello `Point` és `Polygon` hello osztályok `Microsoft.Azure.Documents.Spatial` névtér tooembed helyére vonatkozó információkat az alkalmazás objektumának belül. Ezeket az osztályokat hello szerializálás és a térbeli adatok deszerializálása be GeoJSON leegyszerűsíti.

**A .NET földrajzi adatok dokumentum létrehozása**

```json
using Microsoft.Azure.Documents.Spatial;

public class UserProfile
{
    [JsonProperty("name")]
    public string Name { get; set; }

    [JsonProperty("location")]
    public Point Location { get; set; }

    // More properties
}

await client.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("db", "profiles"), 
    new UserProfile 
    { 
        Name = "documentdb", 
        Location = new Point (-122.12, 47.66) 
    });
```

Ha nem rendelkezik hello szélességi és hosszúsági adatokat, de hello fizikai címeket vagy a hely neve, mint a város vagy ország, például a Bing Maps REST szolgáltatások geokódolás szolgáltatás használatával megjeleníthetők hello tényleges koordináták. További információ a Bing Maps geokódolás [Itt](https://msdn.microsoft.com/library/ff701713.aspx).

## <a name="querying-spatial-types"></a>A térbeli típusok lekérdezése
Most, hogy hogyan egy pillantást azt korábban hozott tooinsert földrajzi adatok, vessen egy pillantást hogyan tooquery ezek az adatok Azure Cosmos DB SQL és a LINQ használatával.

### <a name="spatial-sql-built-in-functions"></a>Térbeli SQL beépített funkciók
Azure Cosmos DB nyissa meg a földrajzi konzorcium (OGC) beépített funkciók földrajzi lekérdezése a következő hello támogatja. A hello SQL nyelvi beépített függvények teljes készletének hello további információkért tekintse meg az túl[lekérdezés Azure Cosmos DB](documentdb-sql-query.md).

<table>
<tr>
  <td><strong>Használat</strong></td>
  <td><strong>Leírás</strong></td>
</tr>
<tr>
  <td>ST_DISTANCE (spatial_expr, spatial_expr)</td>
  <td>A két hello GeoJSON-pont, sokszög vagy LineString kifejezések hello távolság visszaadása.</td>
</tr>
<tr>
  <td>ST_WITHIN (spatial_expr, spatial_expr)</td>
  <td>Egy logikai kifejezés, amely azt jelzi, hogy hello első GeoJSON objektum (pont, Polygon, vagy LineString) hello második GeoJSON objektumon belül (pont, sokszög vagy LineString) adja vissza.</td>
</tr>
<tr>
  <td>ST_INTERSECTS (spatial_expr, spatial_expr)</td>
  <td>Egy logikai kifejezés, amely azt jelzi, hogy az intersect hello két megadott GeoJSON objektumokat (pont, Polygon, vagy LineString) adja vissza.</td>
</tr>
<tr>
  <td>ST_ISVALID</td>
  <td>Jelzi, hogy hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</td>
</tr>
<tr>
  <td>ST_ISVALIDDETAILED</td>
  <td>Értéket ad eredményül, ha hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés logikai értéket tartalmazó JSON-érték érvényes, és ha érvénytelen, továbbá hello OK karakterláncként.</td>
</tr>
</table>

Térbeli funkciók lehet használt tooperform közelségi kapcsolat lekérdezések térbeli adatok alapján. Például az itt a lekérdezés, amely visszaadja az összes termékcsalád dokumentumokat, hogy vannak 30 km-hello belül megadott helyen hello ST_DISTANCE beépített függvény használatával. 

**Lekérdezés**

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

**Eredmények**

    [{
      "id": "WakefieldFamily"
    }]

Ha megadja a térbeli indexelő az indexelési házirendet, majd "távolság lekérdezések" szolgáltató hatékonyan hello index keresztül. A térbeli indexelő további részletekért lásd: hello című részhez. Ha még nem rendelkezik a térbeli index hello a megadott elérési utak, akkor is elvégezhetők térbeli lekérdezéseket megadásával `x-ms-documentdb-query-enable-scan` hello értékű kérelem fejléce túl "true" beállítása. A .NET, ehhez által választható átadásakor hello **FeedOptions** argumentum tooqueries rendelkező [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) tootrue beállítása. 

ST_WITHIN használt toocheck lehet, ha a pont egy Sokszögön belül helyezkedik el. Sokszögek általánosan használt toorepresent határok, például az irányítószámot, állapot határokat, illetve természetes kialakulásához. Újra Ha megadja a térbeli indexelő az indexelési házirendet, majd "belül" lekérdezések szolgáltató hatékonyan hello index keresztül. 

ST_WITHIN sokszög argumentumainak tartalmazhat csak csengetés, azaz hello sokszögek nem tartalmazhat lyuk rajtuk. 

**Lekérdezés**

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

**Eredmények**

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> Hasonló toohow eltérő típusú Azure Cosmos DB a lekérdezésben, akkor működik, ha vagy argumentuma helytelen formátumú vagy érvénytelen, akkor túl kiértékelik megadott hello hely érték**nem definiált** és értékelni hello dokumentum toobe kihagyta a hello a lekérdezés eredménye. Ha a lekérdezés eredménytelen, futtassa a ST_ISVALIDDETAILED toodebug miért hello spatail típusa érvénytelen.     
> 
> 

Cosmos. Azure-adatbázis is támogat más néven inverz lekérdezések végrehajtásához, azaz továbbra is sokszögek vagy Azure Cosmos DB sorainak index, majd egy adott pontot tartalmazó hello területek lekérdezése. Ezt a mintát gyakran használják logisztikai tooidentify például amikor egy teherautó be- vagy egy meghatározott terület. 

**Lekérdezés**

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


**Eredmények**

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

ST_ISVALID és ST_ISVALIDDETAILED használt toocheck lehet érvényes a térbeli objektum esetén. Például a következő lekérdezés hello ellenőrzi hello kívüli tartomány szélesség értékét (-132.8) az a pont érvényességét. ST_ISVALID csak egy logikai értéket ad vissza, és ST_ISVALIDDETAILED beolvasása hello logikai érték, és miért akkor tekinthető érvénytelen hello OK tartalmazó karakterláncot.

** Lekérdezése **

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

**Eredmények**

    [{
      "$1": false
    }]

Ezeket a funkciókat is használt toovalidate sokszögek. Például jelen példában használjuk ST_ISVALIDDETAILED toovalidate egy sokszög, amely nincs lezárva. 

**Lekérdezés**

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

**Eredmények**

    [{
       "$1": { 
            "valid": false, 
            "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a Polygon must have hello same start and end points." 
          }
    }]

### <a name="linq-querying-in-hello-net-sdk"></a>A .NET SDK hello lekérdezése LINQ
a DocumentDB .NET SDK hello is szolgáltatók helyettes módszerek `Distance()` és `Within()` LINQ kifejezés belüli használatra. hello DocumentDB LINQ szolgáltatónál fordítja le ezen metódus hívások toohello egyenértékű SQL beépített függvényhívásokat (ST_DISTANCE és ST_WITHIN rendre). 

Az alábbiakban például a LINQ lekérdezés, amely megkeresi az összes dokumentum hello Azure Cosmos DB gyűjtemény, amelynek "hely" értékét egy radius hello 30 km belül megadott pont LINQ használatával.

**Távolság a LINQ lekérdezés**

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

Ehhez hasonlóan az ide a lekérdezés minden hello dokumentumok, amelynek "hely" hello esik a megadott mező/sokszög kereséséhez. 

**LINQ lekérdezése a belül**

    Polygon rectangularArea = new Polygon(
        new[]
        {
            new LinearRing(new [] {
                new Position(31.8, -5),
                new Position(32, -5),
                new Position(32, -4.7),
                new Position(31.8, -4.7),
                new Position(31.8, -5)
            })
        });

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(a => a.Location.Within(rectangularArea)))
    {
        Console.WriteLine("\t" + user);
    }


Most, hogy hogyan egy pillantást azt korábban hozott tooquery dokumentumok LINQ és az SQL, vessen egy pillantást hogyan tooconfigure Azure Cosmos DB térbeli indexeléshez.

## <a name="indexing"></a>Indexelés
A Microsoft hello leírtak [séma Azure Cosmos DB indexelés független](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papír, a Microsoft Azure Cosmos DB database engine toobe tervezett valóban sémát független és első osztályú támogatást nyújtanak a JSON. optimalizált írási adatbázismotor hello Azure Cosmos-adatbázis natív módon megértette hello GeoJSON standard képviselt térbeli adatok (mutat, sokszögek és sorok).

A ez már a vég, hello geometriai alakzatot 2D vezérlősík geodéziai koordinátái a tervezett, akkor cellák fokozatosan osztva egy **quadtree**. E cellák csatlakoztatott too1D hello cellát hello helye alapján egy **Hilbert terület töltés görbe**, amely megőrzi az pontok helység szerint. Továbbá ha helyadatok indexelt, mert végig kell vinnie néven ismert folyamat **tesszellációs**, azaz összes hello cellákat egy helyre intersect azonosítva és hello Azure Cosmos DB index kulcsokat tárolja. Időben argumentumok hasonlóan pontok és a sokszög is Csempézett tooextract hello vonatkozó cella azonosító tartományokat, majd hello index tooretrieve adatait használja.

Ha meg van-e az indexelési házirendet, amely tartalmazza a térbeli index / * (összes elérési utat), majd hello gyűjteményben található összes pontok indexelt hatékony térbeli lekérdezésekhez (ST_WITHIN és ST_DISTANCE). A térbeli indexek nem pontosság értéket, és mindig az alapértelmezett pontosság értéket használjon.

> [!NOTE]
> Azure Cosmos DB támogatja az automatikus indexeléshez pontok, sokszögek és Linestring
> 
> 

hello alábbi JSON kódrészletben láthatja az indexelési házirendet a térbeli indexelő engedélyezve van, vagyis index bármely GeoJSON-pont térbeli lekérdezése dokumentumok belül található. Ha módosítja az indexelő hello Azure portál használatával hello, JSON következő térbeli a gyűjtemény az indexelési házirendet tooenable indexeléshez hello is megadhat.

**Gyűjtemény indexelő házirend JSON-t Spatial pontokhoz, illetve sokszögek engedélyezve**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Range",
                   "dataType":"String",
                   "precision":-1
                },
                {
                   "kind":"Range",
                   "dataType":"Number",
                   "precision":-1
                },
                {
                   "kind":"Spatial",
                   "dataType":"Point"
                },
                {
                   "kind":"Spatial",
                   "dataType":"Polygon"
                }                
             ]
          }
       ],
       "excludedPaths":[
       ]
    }

Íme egy kódrészletet a .NET, amely bemutatja, hogyan toocreate egy gyűjteménybe és térbeli indexelő engedélyezve van a pontokat tartalmazó összes elérési utat. 

**Hozzon létre gyűjteményt térbeli indexelő**

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override tooturn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

És ez hogyan módosíthatja egy meglévő gyűjteményt tootake előnyeit térbeli indexelő felülírja a dokumentumok tárolt pontokat.

**Módosítsa a térbeli indexelő egy meglevő gyűjteményhez**

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing toocomplete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> Ha hello hely hello dokumentumban GeoJSON érték helytelen formátumú vagy érvénytelen, majd azt nem get indexelve térbeli lekérdezése. ST_ISVALID és ST_ISVALIDDETAILED értékei ellenőrzéséhez.
> 
> Ha a gyűjtemény definíciója tartalmaz egy partíciókulcsot, átalakítási folyamat indexelő nem jelzett. 
> 
> 

## <a name="next-steps"></a>Következő lépések
Most, hogy értesült kapcsolatos tooget indításának Azure Cosmos DB földrajzi támogatása, is:

* A hello elkezdésére [földrajzi .NET mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)
* A földrajzi lekérdezése: hello lekérdezni beavatkozás nélküli [Azure Cosmos DB Tesztlekérdezéseket](http://www.documentdb.com/sql/demo#geospatial)
* További információ [Azure Cosmos adatbázis-lekérdezés](documentdb-sql-query.md)
* További információ [Azure Cosmos DB indexelő házirendek](indexing-policies.md)

