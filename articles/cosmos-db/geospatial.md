---
title: "A földrajzi adatok az Azure Cosmos DB |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre, index és az Azure Cosmos DB és a DocumentDB API térbeli objektumok lekérdezése."
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
ms.openlocfilehash: d5785c81fb597e7d30eb7d3a880e7194d8358ed5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a><span data-ttu-id="b4b17-103">A földrajzi és GeoJSON helyre adatokat az Adatbázisba az Azure Cosmos használata</span><span class="sxs-lookup"><span data-stu-id="b4b17-103">Working with geospatial and GeoJSON location data in Azure Cosmos DB</span></span>
<span data-ttu-id="b4b17-104">Ez a cikk az a földrajzi funkcióinak bemutatása [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="b4b17-104">This article is an introduction to the geospatial functionality in [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="b4b17-105">Ez elolvasása, után lesz a következő kérdések megválaszolásához:</span><span class="sxs-lookup"><span data-stu-id="b4b17-105">After reading this, you will be able to answer the following questions:</span></span>

* <span data-ttu-id="b4b17-106">Térbeli adatok tárolása az Azure Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="b4b17-106">How do I store spatial data in Azure Cosmos DB?</span></span>
* <span data-ttu-id="b4b17-107">Hogyan tudja lekérdezni az SQL és a LINQ Azure Cosmos Adatbázisba földrajzi adatok?</span><span class="sxs-lookup"><span data-stu-id="b4b17-107">How can I query geospatial data in Azure Cosmos DB in SQL and LINQ?</span></span>
* <span data-ttu-id="b4b17-108">Hogyan engedélyezése vagy letiltása, az Azure Cosmos Adatbázisba térbeli indexelő?</span><span class="sxs-lookup"><span data-stu-id="b4b17-108">How do I enable or disable spatial indexing in Azure Cosmos DB?</span></span>

<span data-ttu-id="b4b17-109">Ez a cikk bemutatja, hogyan működik a DocumentDB API-val térbeli adatokat tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="b4b17-109">This article shows how to work with spatial data with the DocumentDB API.</span></span> <span data-ttu-id="b4b17-110">Lásd: a [GitHub-projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) mintakódok számára.</span><span class="sxs-lookup"><span data-stu-id="b4b17-110">Please see this [GitHub project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) for code samples.</span></span>

## <a name="introduction-to-spatial-data"></a><span data-ttu-id="b4b17-111">Térbeli adatok bemutatása</span><span class="sxs-lookup"><span data-stu-id="b4b17-111">Introduction to spatial data</span></span>
<span data-ttu-id="b4b17-112">Térbeli adatok pozíció és objektumok terület alakja ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b4b17-112">Spatial data describes the position and shape of objects in space.</span></span> <span data-ttu-id="b4b17-113">A legtöbb alkalmazásban ezek megegyeznek a föld, azaz a földrajzi adatok objektumok.</span><span class="sxs-lookup"><span data-stu-id="b4b17-113">In most applications, these correspond to objects on the earth, i.e. geospatial data.</span></span> <span data-ttu-id="b4b17-114">Térbeli adatok segítségével határoz meg a személy, a hely egyik fontos vagy a város, vagy egy lake határ helyét.</span><span class="sxs-lookup"><span data-stu-id="b4b17-114">Spatial data can be used to represent the location of a person, a place of interest, or the boundary of a city, or a lake.</span></span> <span data-ttu-id="b4b17-115">Gyakori alkalmazási esetei gyakran közelségi kapcsolat lekérdezéseket, pl. "található összes kávézókban a jelenlegi hely közelében" foglalják magukba.</span><span class="sxs-lookup"><span data-stu-id="b4b17-115">Common use cases often involve proximity queries, for e.g., "find all coffee shops near my current location".</span></span> 

### <a name="geojson"></a><span data-ttu-id="b4b17-116">GeoJSON</span><span class="sxs-lookup"><span data-stu-id="b4b17-116">GeoJSON</span></span>
<span data-ttu-id="b4b17-117">Azure Cosmos-adatbázis indexelő, és lekérdezi-e, amely rendelkezik értékét földrajzi pont adatok támogatja a [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span><span class="sxs-lookup"><span data-stu-id="b4b17-117">Azure Cosmos DB supports indexing and querying of geospatial point data that's represented using the [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span></span> <span data-ttu-id="b4b17-118">GeoJSON adatstruktúrák mindig érvényes JSON-objektumok, hogy tárolhatók és lekérdezett Azure Cosmos DB használatával speciális eszközöket és szalagtárak nélkül.</span><span class="sxs-lookup"><span data-stu-id="b4b17-118">GeoJSON data structures are always valid JSON objects, so they can be stored and queried using Azure Cosmos DB without any specialized tools or libraries.</span></span> <span data-ttu-id="b4b17-119">Az Azure Cosmos DB SDK-k biztosítanak a segítőosztályok és módszereket, amelyek megkönnyítik a térbeli adatok használata.</span><span class="sxs-lookup"><span data-stu-id="b4b17-119">The Azure Cosmos DB SDKs provide helper classes and methods that make it easy to work with spatial data.</span></span> 

### <a name="points-linestrings-and-polygons"></a><span data-ttu-id="b4b17-120">Pontok, Linestring és sokszögek</span><span class="sxs-lookup"><span data-stu-id="b4b17-120">Points, LineStrings and Polygons</span></span>
<span data-ttu-id="b4b17-121">A **pont** helyre egyetlen pozíció jelöli.</span><span class="sxs-lookup"><span data-stu-id="b4b17-121">A **Point** denotes a single position in space.</span></span> <span data-ttu-id="b4b17-122">A földrajzi adatok a pont a pontos helyet, amely lehet egy címe élelmiszerboltban áruházbeli, a teljes képernyős, egy autó vagy a város jelöli.</span><span class="sxs-lookup"><span data-stu-id="b4b17-122">In geospatial data, a Point represents the exact location, which could be a street address of a grocery store, a kiosk, an automobile or a city.</span></span>  <span data-ttu-id="b4b17-123">A pont a GeoJSON (és Azure Cosmos DB) használata a koordináta pár vagy szélességi és hosszúsági jelzi.</span><span class="sxs-lookup"><span data-stu-id="b4b17-123">A point is represented in GeoJSON (and Azure Cosmos DB) using its coordinate pair or longitude and latitude.</span></span> <span data-ttu-id="b4b17-124">Íme egy példa JSON pontnál.</span><span class="sxs-lookup"><span data-stu-id="b4b17-124">Here's an example JSON for a point.</span></span>

<span data-ttu-id="b4b17-125">**Az Azure Cosmos DB pontjai**</span><span class="sxs-lookup"><span data-stu-id="b4b17-125">**Points in Azure Cosmos DB**</span></span>

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> <span data-ttu-id="b4b17-126">A GeoJSON megadását a földrajzi hosszúság értéke határozza meg az első és az a földrajzi hosszúság második.</span><span class="sxs-lookup"><span data-stu-id="b4b17-126">The GeoJSON specification specifies longitude first and latitude second.</span></span> <span data-ttu-id="b4b17-127">Például a többi leképezési alkalmazások szélességi és hosszúsági szögek és fok formájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b4b17-127">Like in other mapping applications, longitude and latitude are angles and represented in terms of degrees.</span></span> <span data-ttu-id="b4b17-128">Hosszúsági értékeknek mért vannak a szélességi és között-180 és 180.0 fok és szélességi értékek vannak mért egyenlítőn, és-90.0 között és 90.0 fok.</span><span class="sxs-lookup"><span data-stu-id="b4b17-128">Longitude values are measured from the Prime Meridian and are between -180 and 180.0 degrees, and latitude values are measured from the equator and are between -90.0 and 90.0 degrees.</span></span> 
> 
> <span data-ttu-id="b4b17-129">Azure Cosmos-adatbázis értelmezi a koordináták a WGS-84 referenciarendszer / határozzák.</span><span class="sxs-lookup"><span data-stu-id="b4b17-129">Azure Cosmos DB interprets coordinates as represented per the WGS-84 reference system.</span></span> <span data-ttu-id="b4b17-130">Lásd az alábbiakat koordináta referenciarendszert kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="b4b17-130">Please see below for more details about coordinate reference systems.</span></span>
> 
> 

<span data-ttu-id="b4b17-131">Ez lehet beágyazni egy Azure Cosmos DB dokumentumban helyre adatokat tartalmazó felhasználói profil e példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="b4b17-131">This can be embedded in an Azure Cosmos DB document as shown in this example of a user profile containing location data:</span></span>

<span data-ttu-id="b4b17-132">**Az Azure Cosmos Adatbázisba tárolási helye a profil használata**</span><span class="sxs-lookup"><span data-stu-id="b4b17-132">**Use Profile with Location stored in Azure Cosmos DB**</span></span>

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

<span data-ttu-id="b4b17-133">Pontokon, felül GeoJSON is támogatja a Linestring és sokszögek.</span><span class="sxs-lookup"><span data-stu-id="b4b17-133">In addition to points, GeoJSON also supports LineStrings and Polygons.</span></span> <span data-ttu-id="b4b17-134">**Linestring** jelentenek két vagy több pont és a sor szegmensekből álló csatlakoztassa őket.</span><span class="sxs-lookup"><span data-stu-id="b4b17-134">**LineStrings** represent a series of two or more points in space and the line segments that connect them.</span></span> <span data-ttu-id="b4b17-135">A földrajzi adatok Linestring általában baromfiszállítás vagy folyókat használhatók.</span><span class="sxs-lookup"><span data-stu-id="b4b17-135">In geospatial data, LineStrings are commonly used to represent highways or rivers.</span></span> <span data-ttu-id="b4b17-136">A **sokszög** összekapcsolja az egy lezárt LineString csatlakoztatott pontok-határ.</span><span class="sxs-lookup"><span data-stu-id="b4b17-136">A **Polygon** is a boundary of connected points that forms a closed LineString.</span></span> <span data-ttu-id="b4b17-137">Sokszögek általában használhatók például tavakat természetes kialakulásához vagy például városokat és állapotok politikai joghatóság alá tartozó területeken.</span><span class="sxs-lookup"><span data-stu-id="b4b17-137">Polygons are commonly used to represent natural formations like lakes or political jurisdictions like cities and states.</span></span> <span data-ttu-id="b4b17-138">Íme egy példa az Azure Cosmos Adatbázisba sokszög.</span><span class="sxs-lookup"><span data-stu-id="b4b17-138">Here's an example of a Polygon in Azure Cosmos DB.</span></span> 

<span data-ttu-id="b4b17-139">**A GeoJSON sokszögek**</span><span class="sxs-lookup"><span data-stu-id="b4b17-139">**Polygons in GeoJSON**</span></span>

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
> <span data-ttu-id="b4b17-140">A GeoJSON specifikációja megköveteli, hogy érvényes sokszögek, a megadott utolsó koordináta pár kell ugyanaz, mint az első zárt alakzat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b4b17-140">The GeoJSON specification requires that for valid Polygons, the last coordinate pair provided should be the same as the first, to create a closed shape.</span></span>
> 
> <span data-ttu-id="b4b17-141">Egy Sokszögön belül pontok balra érdekében meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="b4b17-141">Points within a Polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="b4b17-142">A sokszög jobbra sorrendben megadva a régióját inverzét jelöli.</span><span class="sxs-lookup"><span data-stu-id="b4b17-142">A Polygon specified in clockwise order represents the inverse of the region within it.</span></span>
> 
> 

<span data-ttu-id="b4b17-143">Point, LineString és sokszög kívül GeoJSON is meghatározza a több földrajzi helyek csoportosítása, valamint tetszőleges tulajdonságok társítása földrajzi hely meghatározásának, ábrázolását egy **szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="b4b17-143">In addition to Point, LineString and Polygon, GeoJSON also specifies the representation for how to group multiple geospatial locations, as well as how to associate arbitrary properties with geolocation as a **Feature**.</span></span> <span data-ttu-id="b4b17-144">Mivel ezek az objektumok érvényes JSON-adatokat, akkor is összes tárolása és feldolgozása történhet az Azure Cosmos-Adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="b4b17-144">Since these objects are valid JSON, they can all be stored and processed in Azure Cosmos DB.</span></span> <span data-ttu-id="b4b17-145">Azonban Azure Cosmos DB csak akkor támogatja a pontok automatikus indexeléshez.</span><span class="sxs-lookup"><span data-stu-id="b4b17-145">However Azure Cosmos DB only supports automatic indexing of points.</span></span>

### <a name="coordinate-reference-systems"></a><span data-ttu-id="b4b17-146">Ütemezze referenciarendszert</span><span class="sxs-lookup"><span data-stu-id="b4b17-146">Coordinate reference systems</span></span>
<span data-ttu-id="b4b17-147">Mivel a föld alakját szabálytalan, koordinátáját földrajzi adatok a hány koordináta referenciarendszert (CRS), mindegyiket a saját keretek a referencia- és mértékegységek jelzi.</span><span class="sxs-lookup"><span data-stu-id="b4b17-147">Since the shape of the earth is irregular, coordinates of geospatial data is represented in many coordinate reference systems (CRS), each with their own frames of reference and units of measurement.</span></span> <span data-ttu-id="b4b17-148">Például a "nemzeti rács a Britannia" egy referenciarendszert nagyon pontos az Egyesült Királyságban, de nem kívül.</span><span class="sxs-lookup"><span data-stu-id="b4b17-148">For example, the "National Grid of Britain" is a reference system is very accurate for the United Kingdom, but not outside it.</span></span> 

<span data-ttu-id="b4b17-149">Használja a legnépszerűbb CRS ma a globális geodéziai rendszer [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span><span class="sxs-lookup"><span data-stu-id="b4b17-149">The most popular CRS in use today is the World Geodetic System  [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span></span> <span data-ttu-id="b4b17-150">GPS-eszközöket, és számos leképezési szolgáltatás, beleértve a Google térképeket és a Bing térképek API-k használata WGS-84.</span><span class="sxs-lookup"><span data-stu-id="b4b17-150">GPS devices, and many mapping services including Google Maps and Bing Maps APIs use WGS-84.</span></span> <span data-ttu-id="b4b17-151">Azure Cosmos-adatbázis indexelő, és csak a WGS-84 CRS használatával földrajzi adatok lekérdezését támogatja.</span><span class="sxs-lookup"><span data-stu-id="b4b17-151">Azure Cosmos DB supports indexing and querying of geospatial data using the WGS-84 CRS only.</span></span> 

## <a name="creating-documents-with-spatial-data"></a><span data-ttu-id="b4b17-152">Térbeli adatokat tartalmazó dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4b17-152">Creating documents with spatial data</span></span>
<span data-ttu-id="b4b17-153">GeoJSON értékeket tartalmazó dokumentumok létrehozásakor azok automatikusan indexelt a térbeli index a gyűjtemény az indexelési házirendet összhangban.</span><span class="sxs-lookup"><span data-stu-id="b4b17-153">When you create documents that contain GeoJSON values, they are automatically indexed with a spatial index in accordance to the indexing policy of the collection.</span></span> <span data-ttu-id="b4b17-154">Ha egy Azure Cosmos DB SDK dinamikusan gépelt Python vagy Node.js nyelven dolgozik, érvényes GeoJSON kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="b4b17-154">If you're working with an Azure Cosmos DB SDK in a dynamically typed language like Python or Node.js, you must create valid GeoJSON.</span></span>

<span data-ttu-id="b4b17-155">**A földrajzi adatok node.js dokumentum létrehozása**</span><span class="sxs-lookup"><span data-stu-id="b4b17-155">**Create Document with Geospatial data in Node.js**</span></span>

```json
var userProfileDocument = {
    "name":"documentdb",
    "location":{
        "type":"Point",
        "coordinates":[ -122.12, 47.66 ]
    }
};

client.createDocument(`dbs/${databaseName}/colls/${collectionName}`, userProfileDocument, (err, created) => {
    // additional code within the callback
});
```

<span data-ttu-id="b4b17-156">A DocumentDB API-k használata, ha a `Point` és `Polygon` belül osztályokat a `Microsoft.Azure.Documents.Spatial` névtér beágyazható helyére vonatkozó információkat az alkalmazás objektumának belül.</span><span class="sxs-lookup"><span data-stu-id="b4b17-156">If you're working with the DocumentDB APIs, you can use the `Point` and `Polygon` classes within the `Microsoft.Azure.Documents.Spatial` namespace to embed location information within your application objects.</span></span> <span data-ttu-id="b4b17-157">Ezeket az osztályokat a szerializálás és a térbeli adatok deszerializálása be GeoJSON leegyszerűsíti.</span><span class="sxs-lookup"><span data-stu-id="b4b17-157">These classes help simplify the serialization and deserialization of spatial data into GeoJSON.</span></span>

<span data-ttu-id="b4b17-158">**A .NET földrajzi adatok dokumentum létrehozása**</span><span class="sxs-lookup"><span data-stu-id="b4b17-158">**Create Document with Geospatial data in .NET**</span></span>

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

<span data-ttu-id="b4b17-159">Ha nem rendelkezik a szélességi és hosszúsági adatokat, de a fizikai címeket vagy a hely neve, mint a város vagy ország, például a Bing Maps REST szolgáltatások geokódolás szolgáltatás használatával megjeleníthetők a tényleges koordinátái.</span><span class="sxs-lookup"><span data-stu-id="b4b17-159">If you don't have the latitude and longitude information, but have the physical addresses or location name like city or country, you can look up the actual coordinates by using a geocoding service like Bing Maps REST Services.</span></span> <span data-ttu-id="b4b17-160">További információ a Bing Maps geokódolás [Itt](https://msdn.microsoft.com/library/ff701713.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4b17-160">Learn more about Bing Maps geocoding [here](https://msdn.microsoft.com/library/ff701713.aspx).</span></span>

## <a name="querying-spatial-types"></a><span data-ttu-id="b4b17-161">A térbeli típusok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b4b17-161">Querying spatial types</span></span>
<span data-ttu-id="b4b17-162">Most, hogy azt már hozott földrajzi adatok beszúrása egy pillantást, vessen egy pillantást SQL és a LINQ használatával Azure Cosmos DB használatával az adatok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b4b17-162">Now that we've taken a look at how to insert geospatial data, let's take a look at how to query this data using Azure Cosmos DB using SQL and LINQ.</span></span>

### <a name="spatial-sql-built-in-functions"></a><span data-ttu-id="b4b17-163">Térbeli SQL beépített funkciók</span><span class="sxs-lookup"><span data-stu-id="b4b17-163">Spatial SQL built-in functions</span></span>
<span data-ttu-id="b4b17-164">Azure Cosmos DB támogatja a következő nyissa meg a földrajzi konzorcium (OGC) beépített függvények földrajzi lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b4b17-164">Azure Cosmos DB supports the following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> <span data-ttu-id="b4b17-165">Az SQL-nyelv beépített függvények teljes készletének a további részletekért tekintse meg [lekérdezés Azure Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="b4b17-165">For more details on the complete set of built-in functions in the SQL language, please refer to [Query Azure Cosmos DB](documentdb-sql-query.md).</span></span>

<table>
<tr>
  <td><span data-ttu-id="b4b17-166"><strong>Használat</strong></span><span class="sxs-lookup"><span data-stu-id="b4b17-166"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="b4b17-167"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="b4b17-167"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b4b17-168">ST_DISTANCE (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="b4b17-168">ST_DISTANCE (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="b4b17-169">Csoport távolságát adja vissza a két GeoJSON-pont, sokszög vagy LineString kifejezések között.</span><span class="sxs-lookup"><span data-stu-id="b4b17-169">Returns the distance between the two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b4b17-170">ST_WITHIN (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="b4b17-170">ST_WITHIN (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="b4b17-171">Egy logikai kifejezés jelző az első GeoJSON-objektum (pont, sokszög vagy LineString) objektumban a második GeoJSON (pont, sokszög vagy LineString) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b4b17-171">Returns a Boolean expression indicating whether the first GeoJSON object (Point, Polygon, or LineString) is within the second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b4b17-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="b4b17-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="b4b17-173">Egy logikai kifejezés, amely azt jelzi, hogy a két megadott GeoJSON objektumokat (pont, Polygon, vagy LineString) intersect adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b4b17-173">Returns a Boolean expression indicating whether the two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b4b17-174">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="b4b17-174">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="b4b17-175">Azt jelzi, hogy, hogy a megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="b4b17-175">Returns a Boolean value indicating whether the specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="b4b17-176">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="b4b17-176">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="b4b17-177">Egy olyan logikai érték tartalmazó JSON-érték. Ha a megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes, és ha érvénytelen értéket adja vissza, továbbá karakterláncként okát.</span><span class="sxs-lookup"><span data-stu-id="b4b17-177">Returns a JSON value containing a Boolean value if the specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally the reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="b4b17-178">Térbeli funkciók térbeli adatok közelségi kapcsolat lekérdezések végrehajtásához használható.</span><span class="sxs-lookup"><span data-stu-id="b4b17-178">Spatial functions can be used to perform proximity queries against spatial data.</span></span> <span data-ttu-id="b4b17-179">Például ez visszaadó 30 km-ST_DISTANCE beépített funkcióval a megadott helyen belüli összes termékcsalád dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="b4b17-179">For example, here's a query that returns all family documents that are within 30 km of the specified location using the ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="b4b17-180">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b4b17-180">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="b4b17-181">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b4b17-181">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="b4b17-182">Ha megadja a térbeli indexelő az indexelési házirendet, majd "távolság lekérdezések" szolgáltató hatékonyan keresztül az index.</span><span class="sxs-lookup"><span data-stu-id="b4b17-182">If you include spatial indexing in your indexing policy, then "distance queries" will be served efficiently through the index.</span></span> <span data-ttu-id="b4b17-183">A térbeli indexelő további részletekért lásd: a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b4b17-183">For more details on spatial indexing, please see the section below.</span></span> <span data-ttu-id="b4b17-184">Ha nem rendelkezik a megadott elérési út egy térbeli index, akkor is elvégezhetők térbeli lekérdezéseket megadásával `x-ms-documentdb-query-enable-scan` kérelem fejlécben értéke "true"értékre.</span><span class="sxs-lookup"><span data-stu-id="b4b17-184">If you don't have a spatial index for the specified paths, you can still perform spatial queries by specifying `x-ms-documentdb-query-enable-scan` request header with the value set to "true".</span></span> <span data-ttu-id="b4b17-185">A .NET, ezt megteheti úgy, hogy a nem kötelező **FeedOptions** tartalmazó lekérdezések argumentumának [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) igaz értékre állítva.</span><span class="sxs-lookup"><span data-stu-id="b4b17-185">In .NET, this can be done by passing the optional **FeedOptions** argument to queries with [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) set to true.</span></span> 

<span data-ttu-id="b4b17-186">ST_WITHIN segítségével ellenőrizze, hogy ha egy pont egy Sokszögön belül helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="b4b17-186">ST_WITHIN can be used to check if a point lies within a Polygon.</span></span> <span data-ttu-id="b4b17-187">Gyakran sokszögek használatosak irányítószámot, állapot határok vagy természetes kialakulásához határokat.</span><span class="sxs-lookup"><span data-stu-id="b4b17-187">Commonly Polygons are used to represent boundaries like zip codes, state boundaries, or natural formations.</span></span> <span data-ttu-id="b4b17-188">Újra Ha megadja a térbeli indexelő az indexelési házirendet, majd "belül" lekérdezések szolgáltató hatékonyan keresztül az index.</span><span class="sxs-lookup"><span data-stu-id="b4b17-188">Again if you include spatial indexing in your indexing policy, then "within" queries will be served efficiently through the index.</span></span> 

<span data-ttu-id="b4b17-189">ST_WITHIN sokszög argumentumainak tartalmazhat csak csengetés, azaz a sokszög nem tartalmazhat lyuk rajtuk.</span><span class="sxs-lookup"><span data-stu-id="b4b17-189">Polygon arguments in ST_WITHIN can contain only a single ring, i.e. the Polygons must not contain holes in them.</span></span> 

<span data-ttu-id="b4b17-190">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b4b17-190">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

<span data-ttu-id="b4b17-191">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b4b17-191">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> <span data-ttu-id="b4b17-192">Az Azure Cosmos adatbázis-lekérdezés, hogyan nem egyező típusok Works hasonló, ha a hely érték meghatározva vagy argumentuma helytelen formátumú vagy érvénytelen, majd a kiértékelik majd **nem definiált** és a kiértékelt dokumentum figyelmen kívül hagyja a lekérdezési eredmények.</span><span class="sxs-lookup"><span data-stu-id="b4b17-192">Similar to how mismatched types works in Azure Cosmos DB query, if the location value specified in either argument is malformed or invalid, then it will evaluate to **undefined** and the evaluated document to be skipped from the query results.</span></span> <span data-ttu-id="b4b17-193">Ha a lekérdezés eredménytelen, futtassa a ST_ISVALIDDETAILED a hibakeresési miért spatail típusa érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="b4b17-193">If your query returns no results, run ST_ISVALIDDETAILED To debug why the spatail type is invalid.</span></span>     
> 
> 

<span data-ttu-id="b4b17-194">Azure Cosmos-adatbázis is támogat más néven inverz lekérdezések végrehajtásához, azaz továbbra is sokszögek vagy Azure Cosmos DB sorainak index, majd a megadott pontot tartalmazó területekre lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b4b17-194">Azure Cosmos DB also supports performing inverse queries, i.e. you can index Polygons or lines in Azure Cosmos DB, then query for the areas that contain a specified point.</span></span> <span data-ttu-id="b4b17-195">Ezt a mintát gyakran a logisztikai azonosítására szolgál például amikor egy teherautó be- vagy egy meghatározott terület.</span><span class="sxs-lookup"><span data-stu-id="b4b17-195">This pattern is commonly used in logistics to identify e.g. when a truck enters or leaves a designated area.</span></span> 

<span data-ttu-id="b4b17-196">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b4b17-196">**Query**</span></span>

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


<span data-ttu-id="b4b17-197">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b4b17-197">**Results**</span></span>

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

<span data-ttu-id="b4b17-198">ST_ISVALID és ST_ISVALIDDETAILED segítségével ellenőrizze, hogy egy térbeli objektum érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="b4b17-198">ST_ISVALID and ST_ISVALIDDETAILED can be used to check if a spatial object is valid.</span></span> <span data-ttu-id="b4b17-199">Például a következő lekérdezés ellenőrzi a tartomány a földrajzi hosszúság értéke (-132.8) kívüli pont érvényességét.</span><span class="sxs-lookup"><span data-stu-id="b4b17-199">For example, the following query checks the validity of a point with an out of range latitude value (-132.8).</span></span> <span data-ttu-id="b4b17-200">ST_ISVALID csak egy logikai értéket ad vissza, és ST_ISVALIDDETAILED a logikai és a miért akkor tekinthető érvénytelen OK tartalmazó karakterláncot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b4b17-200">ST_ISVALID returns just a Boolean value, and ST_ISVALIDDETAILED returns the Boolean and a string containing the reason why it is considered invalid.</span></span>

<span data-ttu-id="b4b17-201">** Lekérdezése **</span><span class="sxs-lookup"><span data-stu-id="b4b17-201">** Query **</span></span>

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

<span data-ttu-id="b4b17-202">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b4b17-202">**Results**</span></span>

    [{
      "$1": false
    }]

<span data-ttu-id="b4b17-203">Ezeket a funkciókat is sokszögek érvényesítéséhez használható.</span><span class="sxs-lookup"><span data-stu-id="b4b17-203">These functions can also be used to validate Polygons.</span></span> <span data-ttu-id="b4b17-204">Például itt használjuk ST_ISVALIDDETAILED nem lezárt sokszög érvényesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4b17-204">For example, here we use ST_ISVALIDDETAILED to validate a Polygon that is not closed.</span></span> 

<span data-ttu-id="b4b17-205">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b4b17-205">**Query**</span></span>

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

<span data-ttu-id="b4b17-206">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="b4b17-206">**Results**</span></span>

    [{
       "$1": { 
            "valid": false, 
            "reason": "The Polygon input is not valid because the start and end points of the ring number 1 are not the same. Each ring of a Polygon must have the same start and end points." 
          }
    }]

### <a name="linq-querying-in-the-net-sdk"></a><span data-ttu-id="b4b17-207">A .NET SDK lekérdezése LINQ</span><span class="sxs-lookup"><span data-stu-id="b4b17-207">LINQ Querying in the .NET SDK</span></span>
<span data-ttu-id="b4b17-208">A DocumentDB .NET SDK-t is szolgáltatók helyettes módszerek `Distance()` és `Within()` LINQ kifejezés belüli használatra.</span><span class="sxs-lookup"><span data-stu-id="b4b17-208">The DocumentDB .NET SDK also providers stub methods `Distance()` and `Within()` for use within LINQ expressions.</span></span> <span data-ttu-id="b4b17-209">A DocumentDB LINQ szolgáltatónál fordítja le a megfelelő SQL beépített függvényhívásokat módszert hívásokat a (ST_DISTANCE és ST_WITHIN rendre).</span><span class="sxs-lookup"><span data-stu-id="b4b17-209">The DocumentDB LINQ provider translates these method calls to the equivalent SQL built-in function calls (ST_DISTANCE and ST_WITHIN respectively).</span></span> 

<span data-ttu-id="b4b17-210">Íme egy példa a LINQ lekérdezés, amely megkeresi az összes dokumentum a Azure Cosmos DB gyűjtemény, amelynek "hely" érték van belül egy megadott 30 km radius pont LINQ használatával.</span><span class="sxs-lookup"><span data-stu-id="b4b17-210">Here's an example of a LINQ query that finds all documents in the Azure Cosmos DB collection whose "location" value is within a radius of 30km of the specified point using LINQ.</span></span>

<span data-ttu-id="b4b17-211">**Távolság a LINQ lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="b4b17-211">**LINQ query for Distance**</span></span>

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

<span data-ttu-id="b4b17-212">Ehhez hasonlóan az ide a lekérdezés összes dokumentumot, amelynek "hely" megfelel a megadott mező sokszög kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="b4b17-212">Similarly, here's a query for finding all the documents whose "location" is within the specified box/Polygon.</span></span> 

<span data-ttu-id="b4b17-213">**LINQ lekérdezése a belül**</span><span class="sxs-lookup"><span data-stu-id="b4b17-213">**LINQ query for Within**</span></span>

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


<span data-ttu-id="b4b17-214">Most, hogy azt már tett dokumentumok LINQ és SQL lekérdezése egy pillantást, vessen konfigurálása Azure Cosmos DB térbeli indexelő egy pillantást.</span><span class="sxs-lookup"><span data-stu-id="b4b17-214">Now that we've taken a look at how to query documents using LINQ and SQL, let's take a look at how to configure Azure Cosmos DB for spatial indexing.</span></span>

## <a name="indexing"></a><span data-ttu-id="b4b17-215">Indexelés</span><span class="sxs-lookup"><span data-stu-id="b4b17-215">Indexing</span></span>
<span data-ttu-id="b4b17-216">Leírtak azt a [séma Azure Cosmos DB indexelés független](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papír, azt készült Azure Cosmos adatbázis adatbázis-kezelő kell valóban sémát független és első osztályú támogatást nyújtanak a JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="b4b17-216">As we described in the [Schema Agnostic Indexing with Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paper, we designed Azure Cosmos DB’s database engine to be truly schema agnostic and provide first class support for JSON.</span></span> <span data-ttu-id="b4b17-217">Az optimalizált írási adatbázismotor Azure Cosmos-adatbázis natív módon megértette a GeoJSON standard képviselt térbeli adatok (pontok, sokszögek és sorok).</span><span class="sxs-lookup"><span data-stu-id="b4b17-217">The write optimized database engine of Azure Cosmos DB natively understands spatial data (points, Polygons and lines) represented in the GeoJSON standard.</span></span>

<span data-ttu-id="b4b17-218">A geometriai alakzatot 2D vezérlősík geodéziai koordinátái a tervezett akkor cellák fokozatosan osztva a ez már a vég, egy **quadtree**.</span><span class="sxs-lookup"><span data-stu-id="b4b17-218">In a nutshell, the geometry is projected from geodetic coordinates onto a 2D plane then divided progressively into cells using a **quadtree**.</span></span> <span data-ttu-id="b4b17-219">E cellák 1D belüli mátrixcellában helye alapján vannak leképezve a **Hilbert terület töltés görbe**, amely megőrzi az pontok helység szerint.</span><span class="sxs-lookup"><span data-stu-id="b4b17-219">These cells are mapped to 1D based on the location of the cell within a **Hilbert space filling curve**, which preserves locality of points.</span></span> <span data-ttu-id="b4b17-220">Továbbá ha helyadatok indexelt, mert végig kell vinnie néven ismert folyamat **tesszellációs**, azaz összes cella egy helyre metsző azonosított, és az Azure Cosmos DB index kulcsként tárolt.</span><span class="sxs-lookup"><span data-stu-id="b4b17-220">Additionally when location data is indexed, it goes through a process known as **tessellation**, i.e. all the cells that intersect a location are identified and stored as keys in the Azure Cosmos DB index.</span></span> <span data-ttu-id="b4b17-221">Lekérdezések során például pontok és a sokszög argumentumok is Csempézett kinyerni a megfelelő cella azonosító tartományokat, akkor kiolvassa az adatokat az indexből.</span><span class="sxs-lookup"><span data-stu-id="b4b17-221">At query time, arguments like points and Polygons are also tessellated to extract the relevant cell ID ranges, then used to retrieve data from the index.</span></span>

<span data-ttu-id="b4b17-222">Ha meg van-e az indexelési házirendet, amely tartalmazza a térbeli index / * (összes elérési utat), majd a gyűjteményben található összes pontok indexelt hatékony térbeli lekérdezésekhez (ST_WITHIN és ST_DISTANCE).</span><span class="sxs-lookup"><span data-stu-id="b4b17-222">If you specify an indexing policy that includes spatial index for /* (all paths), then all points found within the collection are indexed for efficient spatial queries (ST_WITHIN and ST_DISTANCE).</span></span> <span data-ttu-id="b4b17-223">A térbeli indexek nem pontosság értéket, és mindig az alapértelmezett pontosság értéket használjon.</span><span class="sxs-lookup"><span data-stu-id="b4b17-223">Spatial indexes do not have a precision value, and always use a default precision value.</span></span>

> [!NOTE]
> <span data-ttu-id="b4b17-224">Azure Cosmos DB támogatja az automatikus indexeléshez pontok, sokszögek és Linestring</span><span class="sxs-lookup"><span data-stu-id="b4b17-224">Azure Cosmos DB supports automatic indexing of Points, Polygons, and LineStrings</span></span>
> 
> 

<span data-ttu-id="b4b17-225">Az alábbi JSON kódrészletben láthatja az indexelési házirendet a térbeli indexelő engedélyezve van, vagyis index bármely GeoJSON-pont térbeli lekérdezése dokumentumok belül található.</span><span class="sxs-lookup"><span data-stu-id="b4b17-225">The following JSON snippet shows an indexing policy with spatial indexing enabled, i.e. index any GeoJSON point found within documents for spatial querying.</span></span> <span data-ttu-id="b4b17-226">Ha módosítja az indexelési házirendet, az Azure portál használatával, térbeli a gyűjteményen indexelő engedélyezéséhez a következő JSON az indexelési házirendet is megadhat.</span><span class="sxs-lookup"><span data-stu-id="b4b17-226">If you are modifying the indexing policy using the Azure Portal, you can specify the following JSON for indexing policy to enable spatial indexing on your collection.</span></span>

<span data-ttu-id="b4b17-227">**Gyűjtemény indexelő házirend JSON-t Spatial pontokhoz, illetve sokszögek engedélyezve**</span><span class="sxs-lookup"><span data-stu-id="b4b17-227">**Collection Indexing Policy JSON with Spatial enabled for points and Polygons**</span></span>

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

<span data-ttu-id="b4b17-228">Íme egy kódrészletet a .NET, amely bemutatja, hogyan hozzon létre gyűjteményt térbeli indexelő engedélyezve van a pontokat tartalmazó összes elérési utat.</span><span class="sxs-lookup"><span data-stu-id="b4b17-228">Here's a code snippet in .NET that shows how to create a collection with spatial indexing turned on for all paths containing points.</span></span> 

<span data-ttu-id="b4b17-229">**Hozzon létre gyűjteményt térbeli indexelő**</span><span class="sxs-lookup"><span data-stu-id="b4b17-229">**Create a collection with spatial indexing**</span></span>

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override to turn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

<span data-ttu-id="b4b17-230">És ez hogyan módosíthatja egy meglévő gyűjteményt térbeli indexelő felülírja a dokumentumok tárolt pontokat előnyeit.</span><span class="sxs-lookup"><span data-stu-id="b4b17-230">And here's how you can modify an existing collection to take advantage of spatial indexing over any points that are stored within documents.</span></span>

<span data-ttu-id="b4b17-231">**Módosítsa a térbeli indexelő egy meglevő gyűjteményhez**</span><span class="sxs-lookup"><span data-stu-id="b4b17-231">**Modify an existing collection with spatial indexing**</span></span>

    Console.WriteLine("Updating collection with spatial indexing enabled in indexing policy...");
    collection.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point));
    await client.ReplaceDocumentCollectionAsync(collection);

    Console.WriteLine("Waiting for indexing to complete...");
    long indexTransformationProgress = 0;
    while (indexTransformationProgress < 100)
    {
        ResourceResponse<DocumentCollection> response = await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"));
        indexTransformationProgress = response.IndexTransformationProgress;

        await Task.Delay(TimeSpan.FromSeconds(1));
    }

> [!NOTE]
> <span data-ttu-id="b4b17-232">Ha a helyen belül a dokumentum GeoJSON érték helytelen formátumú vagy érvénytelen, majd azt nem get indexelve térbeli lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="b4b17-232">If the location GeoJSON value within the document is malformed or invalid, then it will not get indexed for spatial querying.</span></span> <span data-ttu-id="b4b17-233">ST_ISVALID és ST_ISVALIDDETAILED értékei ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4b17-233">You can validate location values using ST_ISVALID and ST_ISVALIDDETAILED.</span></span>
> 
> <span data-ttu-id="b4b17-234">Ha a gyűjtemény definíciója tartalmaz egy partíciókulcsot, átalakítási folyamat indexelő nem jelzett.</span><span class="sxs-lookup"><span data-stu-id="b4b17-234">If your collection definition includes a partition key, indexing transformation progress is not reported.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="b4b17-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b4b17-235">Next steps</span></span>
<span data-ttu-id="b4b17-236">Most, hogy korábban szerzett tapasztalatok Ismerkedés az Azure Cosmos Adatbázisba földrajzi támogatásával kapcsolatos, akkor a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b4b17-236">Now that you've learnt about how to get started with geospatial support in Azure Cosmos DB, you can:</span></span>

* <span data-ttu-id="b4b17-237">A elkezdésére a [földrajzi .NET mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="b4b17-237">Start coding with the [Geospatial .NET code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span></span>
* <span data-ttu-id="b4b17-238">Beavatkozás nélküli lekérdezni földrajzi kérdez le a [Azure Cosmos DB Tesztlekérdezéseket](http://www.documentdb.com/sql/demo#geospatial)</span><span class="sxs-lookup"><span data-stu-id="b4b17-238">Get hands on with geospatial querying at the [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span></span>
* <span data-ttu-id="b4b17-239">További információ [Azure Cosmos adatbázis-lekérdezés](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="b4b17-239">Learn more about [Azure Cosmos DB Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="b4b17-240">További információ [Azure Cosmos DB indexelő házirendek](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="b4b17-240">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>

