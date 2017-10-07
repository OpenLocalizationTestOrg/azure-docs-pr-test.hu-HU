---
title: "aaaAzure Cosmos DB indexelési házirendek |} Microsoft Docs"
description: "Indexelő működésének megismerése az Azure Cosmos-Adatbázisba. Ismerje meg, hogyan tooconfigure és módosítási hello indexelési házirendet az automatikus indexeléshez és jobb teljesítményt nyújt."
keywords: "Indexelő működése, az automatikus indexeléshez, adatbázis indexelő"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: monicar
ms.assetid: d5e8f338-605d-4dff-8a61-7505d5fc46d7
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 08/17/2017
ms.author: arramac
ms.openlocfilehash: 4f77b352b89382aa3352136038cb0e95c7588aac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-cosmos-db-index-data"></a>Hogyan működik az Azure Cosmos DB index adatokat?

Alapértelmezés szerint az összes Azure Cosmos DB adatok indexelése. Míg számos ügyfél boldogok toolet Azure Cosmos DB automatikusan kezelik az indexelő minden szempontját, Azure Cosmos DB is támogat egy egyéni megadó **házirend indexelő** gyűjtemények létrehozása során. Az indexelő házirendek az Azure Cosmos Adatbázisba nincsenek rugalmasabb és hatékonyabb, mint más adatbázis platformokhoz kínált másodlagos indexek, mert a használatukkal megtervezését és testreszabása hello alakzat hello index séma rugalmasságát feláldozása nélkül. toolearn indexelő működéséről az Azure Cosmos Adatbázisba, ismernie kell, hogy kezelésével indexelési házirendet, hogy minden részletre kiterjedő mellékhatásokkal indextárolási terheléssel jár, az írási és a lekérdezés átviteli sebesség és a lekérdezés konzisztencia között.  

Ebben a cikkben azt nézze meg Azure Cosmos DB házirendek indexelő, hogyan testre szabhatja az indexelési házirendet, és hello tartozó kompromisszumot. 

A cikk elolvasása után be fog tudni tooanswer hello a következő kérdéseket:

* Hogyan lehet hello tulajdonságok tooinclude felülbírálás vagy indexelő kizárása?
* Hogyan konfigurálható a végleges frissítéseket hello index?
* Hogyan konfigurálható az indexelő tooperform Order By vagy a tartomány lekérdezések?
* Hogyan tehetem módosítások tooa gyűjtemény indexelési házirendet?
* Hogyan összehasonlítása tárterületi és teljesítménybeli különböző indexelési házirendek?

## <a id="CustomizingIndexingPolicy"></a>Hello indexelési házirendet gyűjtemény testreszabása
A fejlesztők szabhatja hello kompromisszumot közötti tárolás, az írási/lekérdezési teljesítmény és a lekérdezés konzisztencia hello alapértelmezett indexelési házirendet egy Azure Cosmos DB gyűjteményen felülbírálása, és a következő szempontokat hello konfigurálása.

* **Beleértve/dokumentumok és az index elérési utak kizárása**. A fejlesztők választható bizonyos dokumentumok toobe kizárt vagy beszúrása vagy felülírás toohello gyűjtemény hello időpontban hello index része. A fejlesztők is választhat, tooinclude vagy más néven kizárása bizonyos JSON tulajdonságai elérési utak (beleértve a helyettesítő mintákat) toobe indexelt dokumentumok, amelyek szerepelnek az index között.
* **Különböző konfigurálása Index típusok**. Az egyes hello szereplő elérési utak fejlesztők is megadható hello típusú igényelnek-e adatokat és várt lekérdezés-munkaterhelési és hello numerikus vagy karakterlánc "pontosság" az egyes elérési utakat alapján egy gyűjtemény indexe.
* **Index frissítés üzemmódjainak**. Azure Cosmos-adatbázis indexelési házirendet egy Azure Cosmos DB gyűjteményt a hello keresztül állítható be három indexelési módot támogatja: konzisztens, Lazy és "nincs". 

hogyan .NET kódot kódrészletet mutat be a következő hello tooset egyéni indexelési házirendet egy gyűjtemény hello létrehozása során. Itt azt beállítva hello házirend karakterláncok és számok tartományindexszel rendelkező hello maximális pontosság. Ezzel a házirend-karakterláncok Order By lekérdezések végrehajtása teszi lehetővé.

    DocumentCollection collection = new DocumentCollection { Id = "myCollection" };

    collection.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });
    collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), collection);   


> [!NOTE]
> hello JSON-séma indexelési házirendet REST API-verzió hello kiadása módosult, 2015-06-03 toosupport tartomány indexek karakterláncok. .NET SDK 1.2.0 és a Java, Python, és a Node.js SDK-k 1.1.0-ás támogatja hello új házirend séma. Régebbi SDK-k hello REST API-t 2015-04-08 verzióját használja, és támogatja a régebbi séma hello indexelő házirend.
> 
> Alapértelmezés szerint a a tartományindexszel rendelkező a Azure Cosmos DB indexeli az összes karakterlánc-dokumentumok következetesen egy belül, és a numerikus tulajdonságai.  
> 
> 

### <a name="customizing-hello-indexing-policy-using-hello-portal"></a>Hello indexelési házirendet hello portálon testreszabása

Házirend-gyűjtemény hello Azure-portál használatával indexelő hello módosíthatja. Nyissa meg az Azure-portálon hello Azure Cosmos DB fiókja válassza ki a gyűjteményt, a hello bal oldali navigációs menüben kattintson **beállítások**, és kattintson a **indexelő házirend**. A hello **indexelő házirend** panelen módosíthatja az indexelési házirendet, és kattintson a **OK** toosave a módosításokat. 

### <a id="indexing-modes"></a>Az indexelő adatbázis-mód
Azure Cosmos-adatbázis három indexelési módot támogat az Azure Cosmos DB gyűjteményt – a házirend indexelő hello keresztül állítható be Consistent, Lazy és "nincs".

**Egységes**: Ha egy Azure Cosmos DB gyűjtési "konzisztens" van kijelölve, egy adott Azure Cosmos DB gyűjtemény kövesse hello lekérdezései hello azonos konzisztencia szinten megadott hello pontot-olvasása (azaz erős, kötött elavulás, munkamenet és végleges). hello index frissítése hello dokumentum frissítéssel (azaz insert, replace, update és delete egy dokumentum, egy Azure Cosmos DB gyűjteményben) szinkron módon történik.  Egységes indexelő hello költségekkel lehetséges csökkentési konzisztens lekérdezéseket támogat, az írási teljesítmény. A csökkentési hello egyedi elérési utak indexelt toobe igénylő függvényében és hello "konzisztenciaszint". Egységes indexelő módban készült "write gyorsan, azonnal lekérdezés" munkaterhelések.

**Lusta**: tooallow maximális dokumentum adatfeldolgozást átviteli, egy Azure Cosmos DB gyűjtemények konfigurálhatók a Lusta konzisztencia; azaz a lekérdezések idővel konzisztenssé. hello index aszinkron módon frissülni fog, amikor egy Azure Cosmos DB gyűjtemény esetén videokártyának azaz hello gyűjtemény átviteli sebesség nem teljesen túlterhelt tooserve felhasználói kérelmek. "Betöltési, később lekérdezés" munkaterhelések akadálytalan dokumentum adatfeldolgozást igénylő "Lusta" indexelő mód alkalmasak lehetnek.

**Nincs**: egy gyűjtemény indexe mód "None" jelölésű nincs társítva index tartozik. Ez általában akkor használatos, ha egy kulcs-érték tárolóként első Azure Cosmos DB és dokumentumok csak az ID tulajdonság által elért. 

> [!NOTE]
> Házirendje "Nincs" indexelő konfigurálása hello hello mellékhatása meglévő index eldobása rendelkezik. Akkor használja, ha a hozzáférési minták csak igénylik, "id" és/vagy "önhivatkozást".
> 
> 

a következő minta megjelenítése hogyan hello létrehozása az Azure Cosmos DB konzisztens automatikus indexelő az összes dokumentum Beszúrások hello .NET SDK használatával.

a következő táblázat hello hello konzisztencia hello indexelési mód (Consistent és Lazy) hello adatgyűjtési és -hello konzisztenciaszint hello lekérdezés kérelemhez megadott konfigurált alapuló lekérdezések jeleníti meg. Ez vonatkozik semmilyen illesztőfelület - REST API-t SDK-k használatával végzett tooqueries vagy belül tárolt eljárások és eseményindítók. 

|Konzisztencia|Indexelő mód: konzisztens|Indexelő mód: Lusta|
|---|---|---|
|Erős|Erős|Végleges|
|Kötött elavulás|Kötött elavulás|Végleges|
|Munkamenet|Munkamenet|Végleges|
|Végleges|Végleges|Végleges|

Azure Cosmos-adatbázis nincs mód indexelő gyűjteményre végrehajtott lekérdezések hibát ad vissza. Lekérdezések továbbra is hajtható végre, mert keresztül hello explicit vizsgálatok `x-ms-documentdb-enable-scan` fejléc a következő hello REST API vagy hello `EnableScanInQuery` hello beállítás használata a .NET SDK kérelem. Néhány lekérdezés szolgáltatások, mint az ORDER BY nem támogatottak a vizsgálatok `EnableScanInQuery`.

hello következő táblázatban a hello konzisztencia hello indexelési mód (Consistent Lazy és None) alapuló lekérdezések Ha EnableScanInQuery meg van adva.

|Konzisztencia|Indexelő mód: konzisztens|Indexelő mód: Lusta|Indexelő mód: nincs|
|---|---|---|---|
|Erős|Erős|Végleges|Erős|
|Kötött elavulás|Kötött elavulás|Végleges|Kötött elavulás|
|Munkamenet|Munkamenet|Végleges|Munkamenet|
|Végleges|Végleges|Végleges|Végleges|

a következő kód a minta megjelenítése hogyan hello létrehozása az Azure Cosmos DB konzisztens indexelő az összes dokumentum Beszúrások hello .NET SDK használatával.

     // Default collection creates a hash index for all string fields and a range index for all numeric    
     // fields. Hash indexes are compact and offer efficient performance for equality queries.

     var collection = new DocumentCollection { Id ="defaultCollection" };

     collection.IndexingPolicy.IndexingMode = IndexingMode.Consistent;

     collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("mydb"), collection);


### <a name="index-paths"></a>Index elérési utak
Azure Cosmos-adatbázis, a modellek JSON-dokumentumok és hello index fákként, és lehetővé teszi tootune toopolicies hello fája elérési út. A dokumentumok kiválaszthatja, mely elérési kell lennie, illetve tiltani szeretné indexelésének. Továbbfejlesztett írási teljesítmény és alacsonyabb index tárolási forgatókönyvek esetén ez kínálhat, előzetesen ismert hello lekérdezési mintáinak.

Index elérési utak hello legfelső szintű (/) kezdődnie, és általában végződhet hello? helyettesítő karakteres operátor szerinti szűrése, azt jelöli, hogy nincsenek-e hello előtag több lehetséges értékei. Például tooserve válasszon * a Families F WHERE F.familyName = "Andersen", meg kell adni egy index elérési útját /familyName/? hello gyűjtemény indexe házirendben.

Index elérési utakat is használhatja hello * helyettesítő operátor toospecify hello viselkedése hello előtag szerinti elérési utak rekurzív. Például/hasznos / * lehet használt tooexclude minden hello hasznos tulajdonság indexelésének elemet.

Az alábbiakban hello közös mintái index útvonalak megadása:

| Elérési út                | Leírás/használati eset                                                                                                                                                                                                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| /                   | A gyűjtemény alapértelmezett elérési utat. Rekurzív és toowhole dokumentumfa vonatkozik.                                                                                                                                                                                                                                   |
| / prop /?             | Index elérési út szükséges tooserve lekérdezések hasonló hello (kivonatoló vagy tartomány típusokat rendre):<br><br>Válassza ki a gyűjtemény-c WHERE c.prop = "érték"<br><br>Válassza ki a gyűjtemény-c WHERE c.prop > 5<br><br>Válassza ki a gyűjtemény c ORDER BY c.prop                                                                       |
| / prop / *             | Index elérési görbékhez hello megadott címke alatt. A következő lekérdezések hello együttműködik<br><br>Válassza ki a gyűjtemény-c WHERE c.prop = "érték"<br><br>Válassza ki a gyűjtemény-c WHERE c.prop.subprop > 5<br><br>Válassza ki a gyűjtemény-c WHERE c.prop.subprop.nextprop = "érték"<br><br>Válassza ki a gyűjtemény c ORDER BY c.prop         |
| [] / tulajdonságai / /?         | Index elérési tooserve iterációs és skaláris értékeket tartalmazhat, például a ["a", "b", "c"] tömbök ILLESZTÉS küldött lekérdezésekre szükséges:<br><br>Válassza ki címke címke IN collection.props a WHERE címke = "érték"<br><br>A gyűjtemény c ILLESZTÉSI címke IN c.props VÁLASSZA címke ahol címke > 5                                                                         |
| [] /subprop/ /props/? | Index elérési út szükséges tooserve iterációs és ILLESZTÉS küldött lekérdezésekre tömbök objektumok, például [{subprop: "a"}, {subprop: "b"}]:<br><br>Válassza ki címke címke IN collection.props a WHERE tag.subprop = "érték"<br><br>A gyűjtemény c ILLESZTÉSI címke IN c.props címke kiválasztása WHERE tag.subprop = "érték"                                  |
| / prop/subprop /?     | Index elérési út szükséges tooserve lekérdezéseket (kivonatoló vagy tartomány típusokat rendre):<br><br>Válassza ki a gyűjtemény-c WHERE c.prop.subprop = "érték"<br><br>Válassza ki a gyűjtemény-c WHERE c.prop.subprop > 5                                                                                                                    |

> [!NOTE]
> Egyéni index elérési utak beállításakor szükséges toospecify hello alapértelmezett indexelési szabály hello teljes dokumentumfa hello különleges elérési kimaradásával a rendszer "/ *". 
> 
> 

hello alábbi példa tartomány indexelő konkrét elérési utat és konfigurálja a 20 bájt egyéni pontosság érték:

    var collection = new DocumentCollection { Id = "rangeSinglePathCollection" };    

    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/Title/?", 
            Indexes = new Collection<Index> { 
                new RangeIndex(DataType.String) { Precision = 20 } } 
            });

    // Default for everything else
    collection.IndexingPolicy.IncludedPaths.Add(
        new IncludedPath { 
            Path = "/*" ,
            Indexes = new Collection<Index> {
                new HashIndex(DataType.String) { Precision = 3 }, 
                new RangeIndex(DataType.Number) { Precision = -1 } 
            }
        });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), pathRange);


### <a name="index-data-types-kinds-and-precisions"></a>Index adattípusok, típusú és szükséges
Most, hogy hogyan egy pillantást azt korábban hozott toospecify elérési utak nézzük, így hello-beállítások tooconfigure hello egy elérési utat az indexelési házirendet. Megadhatja, hogy egy vagy több indexelési definíciót minden az elérési úthoz:

* Adattípus: **karakterlánc**, **szám**, **pont**, **sokszög**, vagy **LineString** (csak egyet tartalmazhat bejegyzés adatok típusonként és példányonként elérési útja)
* Milyen index: **kivonatoló** (egyenlőség lekérdezések), **tartomány** (egyenlőség, tartomány vagy Order By lekérdezések), vagy **Spatial** (térbeli lekérdezéseket) 
* Pontosság: Kivonatoló index Ez eltér a karakterláncokat, mind az alapértelmezett szám 1 too8 a 3. A tartományindexszel Ez az érték lehet -1 (maximális pontosság) és 1-100 (maximális pontosság) a karakterlánc- vagy számértékeknek változhat.

#### <a name="index-kind"></a>Index típusa
Azure Cosmos-adatbázis használatát támogatja kivonatoló és a tartomány index minden az elérési úthoz (amelyeket konfigurálni karakterláncok, számokat, vagy mindkettő).

* **Kivonatoló** hatékony egyenlőség és JOIN lekérdezéseket támogat. A legtöbb használatából adódó kivonatindexek nem kell 3 bájt hello alapértelmezett értéknél nagyobb pontosságú. DataType karakterlánc vagy szám lehet.
* **Tartomány** hatékony egyenlőség lekérdezéseket, a lekérdezések támogat (használatával >, <>, =, < =,! =), és Order By lekérdezések. Order By lekérdezések alapértelmezés szerint is szükség lehet index maximális pontosság (-1). DataType karakterlánc vagy szám lehet.

Azure Cosmos-adatbázis is támogatja hello térbeli index jellegű minden elérési utat, hello pont, sokszög vagy LineString adattípus esetén adható meg. hello érték hello megadott elérési úton kell lennie mint érvényes GeoJSON töredéket `{"type": "Point", "coordinates": [0.0, 10.0]}`.

* **Térbeli** támogatja a hatékony térbeli (belül és távolság) lekérdezések. DataType lehet pont, sokszög vagy LineString.

> [!NOTE]
> Azure Cosmos DB támogatja, pontokat, sokszögek és Linestring automatikus indexeléshez.
> 
> 

Az alábbiakban hello támogatott index különböző és lekérdezések el használt tooserve példák:

| Index típusa | Leírás/használati eset                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Kivonatoló       | Kivonatoló keresztül/prop /? (vagy /) is lehet használt tooserve hello a következő lekérdezések hatékonyan:<br><br>Válassza ki a gyűjtemény-c WHERE c.prop = "érték"<br><br>Kivonatoló keresztül/tulajdonságai / [] /? (és / vagy/tulajdonságai /) is lehet használt tooserve hello a következő lekérdezések hatékonyan:<br><br>A gyűjtemény c ILLESZTÉSI címke IN c.props címke kiválasztása WHERE címke = 5                                                                                                                       |
| tartomány      | Tartomány/prop/keresztül? (vagy /) is lehet használt tooserve hello a következő lekérdezések hatékonyan:<br><br>Válassza ki a gyűjtemény-c WHERE c.prop = "érték"<br><br>Válassza ki a gyűjtemény-c WHERE c.prop > 5<br><br>Válassza ki a gyűjtemény c ORDER BY c.prop                                                                                                                                                                                                              |
| Térbeli     | Tartomány/prop/keresztül? (vagy /) is lehet használt tooserve hello a következő lekérdezések hatékonyan:<br><br>Válassza ki a gyűjtemény c<br><br>HOL ST_DISTANCE (c.prop, {"type": "Terjesztésipont-", "koordináták": [0.0, 10.0]}) < 40<br><br>Válassza ki a gyűjtemény c ahol ST_WITHIN(c.prop, {"type": "Polygon",...}) – a pont is engedélyezve van az indexelő<br><br>Válassza ki a gyűjtemény c ahol ST_WITHIN({"type": "Point",...}, c.prop)--a sokszög engedélyezve van az indexelő              |

Alapértelmezés szerint hibát ad vissza, mint a tartomány operátorok tartalmazó lekérdezések a > =, ha nincs (a bármely pontosság) tartomány index az, hogy a vizsgálatok lehet-e a szükséges tooserve hello lekérdezés order toosignal. Lekérdezések egy tartományindexszel hello x-ms-documentdb-enable-vizsgálat fejléc a következő hello REST API vagy EnableScanInQuery beállítást hello hello .NET SDK használata nélkül is végrehajthatók. Ha más szűrők hello lekérdezés használó Azure Cosmos DB hello index toofilter ellen, majd nem hibaüzenetet küld.

hello ugyanazok a szabályok vonatkoznak a térbeli lekérdezéseket. Alapértelmezés szerint hibát ad vissza térbeli lekérdezésekhez, ha nincs térbeli index, és nincsenek egyéb hello indexből szolgáltatható. Keresés x-ms-documentdb-enable-vizsgálat/EnableScanInQuery használatával elvégezhető.

#### <a name="index-precision"></a>Index pontosság
Index pontosság lehetővé teszi a terhelés index tároló és a lekérdezési teljesítmény közötti kompromisszumot. A számok azt javasoljuk, hello alapértelmezett pontosság konfigurációs a-1 ("maximális"). Mivel a számok 8 bájt a JSON-ban, ez a beállítás egyenértékű tooa 8 bájt. Kiadási kisebb értéket, például 1-7, a pontosság azt jelenti, hogy az értékek egyes tartományok térkép toohello belül azonos index bejegyzés. Ezért az index tárolóhely csökkenti, azonban a lekérdezés-végrehajtás előfordulhat, hogy tooprocess további dokumentumok és következésképpen felhasználását, azaz a további átviteli egységek kérése

Index pontosság konfigurációs karakterlánc címtartományai több alkalmazás van. Karakterláncok bármilyen tetszőleges hosszúságú lehet, mert hello index pontosság hello választott befolyásolhatja a hello teljesítményt karakterlánc lekérdezések és a szükséges index tárhely hatás hello mennyiségét. Karakterlánc tartomány indexek konfigurálható 1-100 vagy -1 ("maximális"). Ha szeretné tooperform Order By lekérdezések kapcsolatikarakterlánc-tulajdonságokat, majd meg kell adnia egy-1 -nek megfelelő útvonalaira hello pontosságát.

A térbeli indexek mindig hello alapértelmezett index pontosság minden típusú (pontok, Linestring és sokszögek) használ, és nem bírálható felül. 

hello a következő példa bemutatja, hogyan tooincrease hello pontosság tartomány indexeli, egy gyűjtemény hello .NET SDK használatával. 

**Hozzon létre egy egyéni index pontosság gyűjteményt**

    var rangeDefault = new DocumentCollection { Id = "rangeCollection" };

    // Override hello default policy for Strings toorange indexing and "max" (-1) precision
    rangeDefault.IndexingPolicy = new IndexingPolicy(new RangeIndex(DataType.String) { Precision = -1 });

    await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), rangeDefault);   


> [!NOTE]
> Azure Cosmos DB hibát ad vissza, ha a lekérdezés Order By használja, de nem rendelkezik a tartományindexszel hello lekérdezett útvonalat, ahol hello maximális pontosság ellen. 
> 
> 

Hasonlóképpen elérési utak teljesen kizárhatók indexelő. hello következő példa bemutatja, hogyan tooexclude egy teljes szakaszáról hello dokumentumok (más néven egy részfájának) használatával hello indexelésének "*" helyettesítő karakter.

    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*" });

    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);



## <a name="opting-in-and-opting-out-of-indexing"></a>Engedélyezés és meggátolható a indexelő
Kiválaszthatja, hogy kívánja-e hello gyűjtemény tooautomatically index összes dokumentumot. Alapértelmezés szerint minden dokumentumok automatikusan indexeli ugyan, de tooturn választhat ki. Ha indexelés ki van kapcsolva, dokumentumokat csak a is elérhetők az erre az önhivatkozások vagy lekérdezések használatával azonosítóját.

Az automatikus indexeléshez ki van kapcsolva, csak az adott dokumentumok toohello index szelektív továbbra is hozzáadhat. Ezzel ellentétben automatikus az indexelő hagyhatja, és szelektív válasszon tooexclude csak bizonyos dokumentumokhoz. Be-és kikapcsolása konfigurációk indexelő akkor hasznos, ha rendelkezik lekérdezett toobe igénylő dokumentumokhoz csak egy részét.

Például a hello következő példa bemutatja hogyan tooinclude a dokumentum explicit módon hello [DocumentDB API .NET SDK](https://docs.microsoft.com/en-us/azure/cosmos-db/documentdb-sdk-dotnet) és hello [RequestOptions.IndexingDirective](http://msdn.microsoft.com/library/microsoft.azure.documents.client.requestoptions.indexingdirective.aspx) tulajdonság.

    // If you want toooverride hello default collection behavior tooeither
    // exclude (or include) a Document from indexing,
    // use hello RequestOptions.IndexingDirective property.
    client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new { id = "AndersenFamily", isRegistered = true },
        new RequestOptions { IndexingDirective = IndexingDirective.Include });

## <a name="modifying-hello-indexing-policy-of-a-collection"></a>Hello indexelési házirendet gyűjtemény módosítása
Azure Cosmos DB lehetővé teszi toomake módosítások toohello indexelési házirendet gyűjtemény hello menet közben a. Egy Azure Cosmos DB gyűjteményen házirend indexelő változása vezethet tooa módosítása hello alakzat hello index, beleértve az elérési utak indexelhetők hello, a pontosság, valamint hello konzisztencia modell hello index magát. Így az indexelési házirendet egy módosítása gyakorlatilag be egy új hello régi index átalakítás igényel. 

**Online indexkészítés átalakítások**

![Indexelő működése – Azure Cosmos DB online indexkészítés átalakítások](./media/indexing-policies/index-transformations.png)

Index átalakításához végzett online, ami azt jelenti, hogy e hello régi házirend indexelt hello dokumentumok hatékonyan átalakítaná e hello új házirend **hello írási rendelkezésre állás vagy a hello kiosztott átviteli sebesség befolyásolása nélkül** a hello gyűjtemény. hello konzisztenciájának olvasási és írási műveletek hello REST API-t SDK-k használatával létrehozott, vagy a tárolt eljárások és eseményindítók nem változik index átalakítása során. Ez azt jelenti, hogy nem teljesítménycsökkenést vagy leállás tooyour alkalmazások amikor módosíthatja az indexelési házirendet.

Azonban a hello idő, amely index átalakítási folyamat alatt lekérdezések nem idővel konzisztenssé függetlenül hello indexelő módjának saját konfigurációja (azonos vagy Lazy). Ez is tooqueries összes illesztő – REST API-t SDK-k, érvényes, és a tárolt eljárások és eseményindítók. Csakúgy, mint az indexelő, Lazy index átalakítása történik aszinkron módon hello háttérben használja hello tartalék forrásanyag is elérhető replika hello replikákon. 

Index átalakítások is **forrásgyűjteményben** (helyen), azaz Azure Cosmos DB nem tart fenn két példányban hello index és a lapozófájl-kapacitás hello régi index ki a hello újat. Ez azt jelenti, hogy nincs további lemezterület szükséges vagy a gyűjtemények felhasznált index átalakítások végrehajtása közben.

Ha megváltoztatja az indexelési házirendet, hogyan hello változtatások hello régi index toohello új egyik függenek elsősorban hello mód konfigurációk több indexelő, mint hello más hasonló értékek/kizárt elérési utak, index különböző és szükséges, az alkalmazott toomove. Ha a régi és az új házirendek egységes indexelő, Azure Cosmos DB egy online indexkészítés átalakítást hajt végre. Egy másik indexelési házirend változásának konzisztens indexelő módban nem tudja alkalmazni, amíg hello átalakítása folyamatban van.

Azonban áthelyezheti tooLazy vagy "None" átalakítás során az indexelő mód van folyamatban. 

* TooLazy áthelyezésekor hello index házirend módosításakor hatékony azonnal és Azure Cosmos DB kezdődik hello index létrehozása aszinkron módon történik. 
* Amikor tooNone helyezi át, majd hello index megszakad a hatékony azonnal. Áthelyezi a tooNone akkor hasznos, ha azt szeretné, hogy egy folyamatban lévő toocancel átalakítása és egy másik indexelési házirendet az alapoktól start. 

Ha hello .NET SDK használata esetén is indítsa el egy új használatával hello indexelési házirend módosítása **ReplaceDocumentCollectionAsync** metódus, és nyomon követése hello előrehaladás százalékosan kifejezve hello index átalakítás hello segítségével  **IndexTransformationProgress** válasz tulajdonságot egy **ReadDocumentCollectionAsync** hívható meg. Egyéb SDK-k és hello REST API támogatja egyenértékű tulajdonságai és metódusai indexelési házirend módosítása közben.

Íme egy kódrészletet, amely bemutatja, hogyan toomodify egy gyűjtemény által indexelő a konzisztens indexelési mód tooLazy házirendje.

**Az egységes tooLazy indexelő házirend módosítása**

    // Switch toolazy indexing.
    Console.WriteLine("Changing from Default tooLazy IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.Lazy;

    await client.ReplaceDocumentCollectionAsync(collection);


Hello előrehaladását egy index átalakítást által hívó ReadDocumentCollectionAsync, például ellenőrizheti a lent látható módon.

**Index átalakítása előrehaladását úgy követheti nyomon**

    long smallWaitTimeMilliseconds = 1000;
    long progress = 0;

    while (progress < 100)
    {
        ResourceResponse<DocumentCollection> collectionReadResponse = await client.ReadDocumentCollectionAsync(
            UriFactory.CreateDocumentCollectionUri("db", "coll"));

        progress = collectionReadResponse.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromMilliseconds(smallWaitTimeMilliseconds));
    }

Egy gyűjtemény indexe hello nincs mód indexelő toohello mozgatásával dobhatja el. Ez akkor lehet hasznos működési eszköz, ha szeretné, hogy a folyamatban lévő átalakulása toocancel és egy új azonnali indítása.

**Egy gyűjtemény hello index elvetése**

    // Switch toolazy indexing.
    Console.WriteLine("Dropping index by changing tootoohello None IndexingMode.");

    collection.IndexingPolicy.IndexingMode = IndexingMode.None;

    await client.ReplaceDocumentCollectionAsync(collection);

Ha szeretné módosít indexelési házirend tooyour Azure Cosmos DB gyűjtemények? Az alábbiakban hello hello leggyakoribb használati esetek:

* Konzisztens eredmények szolgálnak a normál működés során, ugyanakkor tartalék megoldásként toolazy indexelő tömeges adatok importálása során
* Indítsa el a jelenlegi Azure Cosmos DB-gyűjteményekben, például az új indexelési funkciókat használ, például a földrajzi kérdez le, amely igényel hello térbeli index ilyen, vagy Order By / hello karakterlánc tartomány index jellegű igénylő lekérdezések karakterlánc
* Az aktuális kijelölés indexelt tulajdonságok toobe hello és változnak az idők
* Az indexelő pontosság tooimprove lekérdezési teljesítmény hangolására, vagy csökkentse a felhasznált tárhely

> [!NOTE]
> házirend indexelő toomodify ReplaceDocumentCollectionAsync használ, akkor verzióját kell > = a .NET SDK hello 1.3.0
> 
> Index átalakítása toocomplete sikeresen, gondoskodnia kell róla, hogy van elég szabad tárterület elérhető hello gyűjteményen. Ha hello gyűjtemény eléri a tárolási kvótát, hello index átalakítása lehet szüneteltetni. Index átalakítása működése automatikusan folytatódik, ha tárolóhely érhető el, például egyes dokumentumok törlésekor.
> 
> 

## <a name="performance-tuning"></a>Teljesítmény-finomhangolás
hello DocumentDB API-k teljesítménymutatók kapcsolatos információkkal szolgálnak, például hello index tárolót használja, és minden műveletet a hello átviteli költsége (kérelemegység). Ez az információ indexelési különböző házirendeket lehet használt toocompare és teljesítményhangolás.

toocheck hello tárhelykvótát a gyűjtemény használati HEAD vagy GET kérelmet futtatni hello a gyűjtemény-erőforráshoz és hello x-ms-kérelem-kvóta és hello x-ms-kérelem-használati fejlécek vizsgálata. A .NET SDK hello, hello [DocumentSizeQuota](http://msdn.microsoft.com/library/dn850325.aspx) és [DocumentSizeUsage](http://msdn.microsoft.com/library/azure/dn850324.aspx) tulajdonságok [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) tartalmaznia kell a megfelelő értékeket .

     // Measure hello document size usage (which includes hello index size) against   
     // different policies.
     ResourceResponse<DocumentCollection> collectionInfo = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));  
     Console.WriteLine("Document size quota: {0}, usage: {1}", collectionInfo.DocumentQuota, collectionInfo.DocumentUsage);


toomeasure hello terhelését, minden egyes írási műveletnél az indexelő (létrehozása, frissítése vagy törlése), vizsgálja meg az x-ms-kérelem-kell fizetni fejléc hello (vagy ezzel egyenértékű hello [RequestCharge](http://msdn.microsoft.com/library/dn799099.aspx) tulajdonság [ResourceResponse < T\> ](http://msdn.microsoft.com/library/dn799209.aspx) a .NET SDK hello) toomeasure hello száma kérelemegység használni ezeket a műveleteket.

     // Measure hello performance (request units) of writes.     
     ResourceResponse<Document> response = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), myDocument);              
     Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);

     // Measure hello performance (request units) of queries.    
     IDocumentQuery<dynamic> queryable =  client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"), queryString).AsDocumentQuery();

     double totalRequestCharge = 0;
     while (queryable.HasMoreResults)
     {
        FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>(); 
        Console.WriteLine("Query batch consumed {0} request units",queryResponse.RequestCharge);
        totalRequestCharge += queryResponse.RequestCharge;
     }

     Console.WriteLine("Query consumed {0} request units in total", totalRequestCharge);

## <a name="changes-toohello-indexing-policy-specification"></a>Módosítások toohello indexelő házirend meghatározása
2015. július 7. REST API-t 2015-06-03 verzió hello séma az indexelési házirendet egy változást jelent meg. hello megfelelő osztályok hello SDK verzióiban sémája új megvalósítások toomatch hello. 

a következő módosításokat hello bevezetett hello JSON megadását:

* Házirend indexelő támogatja a tartomány indexek karakterláncok
* Mindegyik elérési út lehet egy, az egyes adattípusokhoz több index definíciója
* Pontosság indexelő támogatja az 1-8, számok, 1-100 karakterláncok és -1 (maximális pontosság)
* Elérési utak szegmensek nem igénylik a dupla idézőjelek tooescape mindegyik elérési út. Például hozzáadhat egy elérési utat/cím /? Ahelyett, hogy / "title" /?
* hello gyökérútvonalát "görbékhez" jelző is jelenik meg vagy * (továbbá túl /)

Ha látja el gyűjtemények 1.1.0-ás verziójával készült egyéni indexelési házirendet a kód .NET SDK hello vagy régebbi, szüksége lesz a toochange code toohandle rendelés toomove tooSDK verziójában 1.2.0 a módosítások az alkalmazás. Ha nem konfigurálja az indexelési házirendet kódot, vagy egy régebbi verziójával SDK toocontinue megtervezése, akkor nem módosításokra szükség.

Gyakorlati összehasonlítása Íme egy példa használatával készítettek egyéni indexelési házirendet hello REST API verziója 2015-06-03, valamint a korábbi verziót 2015-04-08 hello.

**Előző indexelési házirendet JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "IncludedPaths":[
          {
             "IndexType":"Hash",
             "Path":"/",
             "NumericPrecision":7,
             "StringPrecision":3
          }
       ],
       "ExcludedPaths":[
          "/\"nonIndexedContent\"/*"
       ]
    }

**Aktuális indexelési házirendet JSON**

    {
       "automatic":true,
       "indexingMode":"Consistent",
       "includedPaths":[
          {
             "path":"/*",
             "indexes":[
                {
                   "kind":"Hash",
                   "dataType":"String",
                   "precision":3
                },
                {
                   "kind":"Hash",
                   "dataType":"Number",
                   "precision":7
                }
             ]
          }
       ],
       "ExcludedPaths":[
          {
             "path":"/nonIndexedContent/*"
          }
       ]
    }

## <a name="next-steps"></a>Következő lépések
Az index házirend felügyeleti mintákat és toolearn Azure Cosmos DB lekérdezési nyelv kapcsolatos további alatt hello hivatkozásokat követve.

1. [A DocumentDB API .NET Indexkezelés mintakódok](https://github.com/Azure/azure-documentdb-net/blob/master/samples/code-samples/IndexManagement/Program.cs)
2. [A DocumentDB API REST adatgyűjtési műveletek](https://msdn.microsoft.com/library/azure/dn782195.aspx)
3. [Az SQL lekérdezés](documentdb-sql-query.md)

