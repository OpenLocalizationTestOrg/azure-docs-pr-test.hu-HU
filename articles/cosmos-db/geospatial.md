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
# <a name="working-with-geospatial-and-geojson-location-data-in-azure-cosmos-db"></a><span data-ttu-id="bf78c-103">A földrajzi és GeoJSON helyre adatokat az Adatbázisba az Azure Cosmos használata</span><span class="sxs-lookup"><span data-stu-id="bf78c-103">Working with geospatial and GeoJSON location data in Azure Cosmos DB</span></span>
<span data-ttu-id="bf78c-104">Ez a cikk egy bevezető toohello földrajzi funkcióit [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="bf78c-104">This article is an introduction toohello geospatial functionality in [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/).</span></span> <span data-ttu-id="bf78c-105">Után olvassa, fogja tudni tooanswer hello a következő kérdéseket:</span><span class="sxs-lookup"><span data-stu-id="bf78c-105">After reading this, you will be able tooanswer hello following questions:</span></span>

* <span data-ttu-id="bf78c-106">Térbeli adatok tárolása az Azure Cosmos-Adatbázisba</span><span class="sxs-lookup"><span data-stu-id="bf78c-106">How do I store spatial data in Azure Cosmos DB?</span></span>
* <span data-ttu-id="bf78c-107">Hogyan tudja lekérdezni az SQL és a LINQ Azure Cosmos Adatbázisba földrajzi adatok?</span><span class="sxs-lookup"><span data-stu-id="bf78c-107">How can I query geospatial data in Azure Cosmos DB in SQL and LINQ?</span></span>
* <span data-ttu-id="bf78c-108">Hogyan engedélyezése vagy letiltása, az Azure Cosmos Adatbázisba térbeli indexelő?</span><span class="sxs-lookup"><span data-stu-id="bf78c-108">How do I enable or disable spatial indexing in Azure Cosmos DB?</span></span>

<span data-ttu-id="bf78c-109">Ez a cikk bemutatja, hogyan toowork a térbeli adatokat tartalmazó hello DocumentDB API.</span><span class="sxs-lookup"><span data-stu-id="bf78c-109">This article shows how toowork with spatial data with hello DocumentDB API.</span></span> <span data-ttu-id="bf78c-110">Lásd: a [GitHub-projekt](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) mintakódok számára.</span><span class="sxs-lookup"><span data-stu-id="bf78c-110">Please see this [GitHub project](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Geospatial/Program.cs) for code samples.</span></span>

## <a name="introduction-toospatial-data"></a><span data-ttu-id="bf78c-111">Bevezetés toospatial adatok</span><span class="sxs-lookup"><span data-stu-id="bf78c-111">Introduction toospatial data</span></span>
<span data-ttu-id="bf78c-112">Térbeli adatok hello és adott hely objektumaira alakját ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bf78c-112">Spatial data describes hello position and shape of objects in space.</span></span> <span data-ttu-id="bf78c-113">A legtöbb alkalmazásban ezek megegyeznek a tooobjects hello a föld, azaz a földrajzi adatok.</span><span class="sxs-lookup"><span data-stu-id="bf78c-113">In most applications, these correspond tooobjects on hello earth, i.e. geospatial data.</span></span> <span data-ttu-id="bf78c-114">Lehet, hogy az térbeli adatokat használt toorepresent hello hely a személy, érdeklő olyan helyen, vagy a város, vagy egy lake hello határát.</span><span class="sxs-lookup"><span data-stu-id="bf78c-114">Spatial data can be used toorepresent hello location of a person, a place of interest, or hello boundary of a city, or a lake.</span></span> <span data-ttu-id="bf78c-115">Gyakori alkalmazási esetei gyakran közelségi kapcsolat lekérdezéseket, pl. "található összes kávézókban a jelenlegi hely közelében" foglalják magukba.</span><span class="sxs-lookup"><span data-stu-id="bf78c-115">Common use cases often involve proximity queries, for e.g., "find all coffee shops near my current location".</span></span> 

### <a name="geojson"></a><span data-ttu-id="bf78c-116">GeoJSON</span><span class="sxs-lookup"><span data-stu-id="bf78c-116">GeoJSON</span></span>
<span data-ttu-id="bf78c-117">Azure Cosmos-adatbázis támogatja az indexelő és földrajzi pont hello segítségével ábrázolt adatok lekérdezését [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span><span class="sxs-lookup"><span data-stu-id="bf78c-117">Azure Cosmos DB supports indexing and querying of geospatial point data that's represented using hello [GeoJSON specification](https://tools.ietf.org/html/rfc7946).</span></span> <span data-ttu-id="bf78c-118">GeoJSON adatstruktúrák mindig érvényes JSON-objektumok, hogy tárolhatók és lekérdezett Azure Cosmos DB használatával speciális eszközöket és szalagtárak nélkül.</span><span class="sxs-lookup"><span data-stu-id="bf78c-118">GeoJSON data structures are always valid JSON objects, so they can be stored and queried using Azure Cosmos DB without any specialized tools or libraries.</span></span> <span data-ttu-id="bf78c-119">hello Azure Cosmos DB SDK-k segítőosztályok és módszerek, amelyekkel könnyen toowork térbeli adatokat tartalmazó biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="bf78c-119">hello Azure Cosmos DB SDKs provide helper classes and methods that make it easy toowork with spatial data.</span></span> 

### <a name="points-linestrings-and-polygons"></a><span data-ttu-id="bf78c-120">Pontok, Linestring és sokszögek</span><span class="sxs-lookup"><span data-stu-id="bf78c-120">Points, LineStrings and Polygons</span></span>
<span data-ttu-id="bf78c-121">A **pont** helyre egyetlen pozíció jelöli.</span><span class="sxs-lookup"><span data-stu-id="bf78c-121">A **Point** denotes a single position in space.</span></span> <span data-ttu-id="bf78c-122">A földrajzi adatok, az a pont hello pontos helyet, amely lehet egy címe élelmiszerboltban áruházbeli, a teljes képernyős, egy autó vagy a város jelöli.</span><span class="sxs-lookup"><span data-stu-id="bf78c-122">In geospatial data, a Point represents hello exact location, which could be a street address of a grocery store, a kiosk, an automobile or a city.</span></span>  <span data-ttu-id="bf78c-123">A pont a GeoJSON (és Azure Cosmos DB) használata a koordináta pár vagy szélességi és hosszúsági jelzi.</span><span class="sxs-lookup"><span data-stu-id="bf78c-123">A point is represented in GeoJSON (and Azure Cosmos DB) using its coordinate pair or longitude and latitude.</span></span> <span data-ttu-id="bf78c-124">Íme egy példa JSON pontnál.</span><span class="sxs-lookup"><span data-stu-id="bf78c-124">Here's an example JSON for a point.</span></span>

<span data-ttu-id="bf78c-125">**Az Azure Cosmos DB pontjai**</span><span class="sxs-lookup"><span data-stu-id="bf78c-125">**Points in Azure Cosmos DB**</span></span>

```json
{
    "type":"Point",
    "coordinates":[ 31.9, -4.8 ]
}
```

> [!NOTE]
> <span data-ttu-id="bf78c-126">hello GeoJSON meghatározása a földrajzi hosszúság értéke határozza meg az első és az a földrajzi hosszúság második.</span><span class="sxs-lookup"><span data-stu-id="bf78c-126">hello GeoJSON specification specifies longitude first and latitude second.</span></span> <span data-ttu-id="bf78c-127">Például a többi leképezési alkalmazások szélességi és hosszúsági szögek és fok formájában jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bf78c-127">Like in other mapping applications, longitude and latitude are angles and represented in terms of degrees.</span></span> <span data-ttu-id="bf78c-128">A szélességi hello mérik és-180 és 180.0 fok, és a szélesség között hosszúsági értékeknek értékek vannak mért hello Egyenlítőtől és-90.0 között és 90.0 fok.</span><span class="sxs-lookup"><span data-stu-id="bf78c-128">Longitude values are measured from hello Prime Meridian and are between -180 and 180.0 degrees, and latitude values are measured from hello equator and are between -90.0 and 90.0 degrees.</span></span> 
> 
> <span data-ttu-id="bf78c-129">Azure Cosmos DB értelmezi a koordináták / hello WGS-84 referenciarendszer határozzák.</span><span class="sxs-lookup"><span data-stu-id="bf78c-129">Azure Cosmos DB interprets coordinates as represented per hello WGS-84 reference system.</span></span> <span data-ttu-id="bf78c-130">Lásd az alábbiakat koordináta referenciarendszert kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="bf78c-130">Please see below for more details about coordinate reference systems.</span></span>
> 
> 

<span data-ttu-id="bf78c-131">Ez lehet beágyazni egy Azure Cosmos DB dokumentumban helyre adatokat tartalmazó felhasználói profil e példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="bf78c-131">This can be embedded in an Azure Cosmos DB document as shown in this example of a user profile containing location data:</span></span>

<span data-ttu-id="bf78c-132">**Az Azure Cosmos Adatbázisba tárolási helye a profil használata**</span><span class="sxs-lookup"><span data-stu-id="bf78c-132">**Use Profile with Location stored in Azure Cosmos DB**</span></span>

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

<span data-ttu-id="bf78c-133">Ezenkívül toopoints, GeoJSON Linestring és sokszögek is támogatja.</span><span class="sxs-lookup"><span data-stu-id="bf78c-133">In addition toopoints, GeoJSON also supports LineStrings and Polygons.</span></span> <span data-ttu-id="bf78c-134">**Linestring** területen legalább két pontok jelentenek, és csatlakoztassa őket sor szegmensek hello.</span><span class="sxs-lookup"><span data-stu-id="bf78c-134">**LineStrings** represent a series of two or more points in space and hello line segments that connect them.</span></span> <span data-ttu-id="bf78c-135">A földrajzi adatok Linestring a gyakran használt toorepresent közútvonalakon és folyókat.</span><span class="sxs-lookup"><span data-stu-id="bf78c-135">In geospatial data, LineStrings are commonly used toorepresent highways or rivers.</span></span> <span data-ttu-id="bf78c-136">A **sokszög** összekapcsolja az egy lezárt LineString csatlakoztatott pontok-határ.</span><span class="sxs-lookup"><span data-stu-id="bf78c-136">A **Polygon** is a boundary of connected points that forms a closed LineString.</span></span> <span data-ttu-id="bf78c-137">Sokszögek olyan gyakran használt toorepresent természetes kialakulásához például tavakat vagy például városokat és állapotok politikai joghatóság alá tartozó területeken.</span><span class="sxs-lookup"><span data-stu-id="bf78c-137">Polygons are commonly used toorepresent natural formations like lakes or political jurisdictions like cities and states.</span></span> <span data-ttu-id="bf78c-138">Íme egy példa az Azure Cosmos Adatbázisba sokszög.</span><span class="sxs-lookup"><span data-stu-id="bf78c-138">Here's an example of a Polygon in Azure Cosmos DB.</span></span> 

<span data-ttu-id="bf78c-139">**A GeoJSON sokszögek**</span><span class="sxs-lookup"><span data-stu-id="bf78c-139">**Polygons in GeoJSON**</span></span>

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
> <span data-ttu-id="bf78c-140">hello GeoJSON specifikációja megköveteli, hogy érvényes sokszögek hello megadott utolsó koordináta pár kell lennie mint hello első, toocreate zárt alakzat hello azonos.</span><span class="sxs-lookup"><span data-stu-id="bf78c-140">hello GeoJSON specification requires that for valid Polygons, hello last coordinate pair provided should be hello same as hello first, toocreate a closed shape.</span></span>
> 
> <span data-ttu-id="bf78c-141">Egy Sokszögön belül pontok balra érdekében meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="bf78c-141">Points within a Polygon must be specified in counter-clockwise order.</span></span> <span data-ttu-id="bf78c-142">A sokszög jobbra sorrendben megadva hello régióját, hello inverzét jelöli.</span><span class="sxs-lookup"><span data-stu-id="bf78c-142">A Polygon specified in clockwise order represents hello inverse of hello region within it.</span></span>
> 
> 

<span data-ttu-id="bf78c-143">Továbbá tooPoint, LineString, Polygon, és GeoJSON is megadja, hogy hogyan hello ábrázolását toogroup több földrajzi helyen, és tetszőleges tulajdonság tooassociate földrajzi hely meghatározásának, mint egy **szolgáltatás**.</span><span class="sxs-lookup"><span data-stu-id="bf78c-143">In addition tooPoint, LineString and Polygon, GeoJSON also specifies hello representation for how toogroup multiple geospatial locations, as well as how tooassociate arbitrary properties with geolocation as a **Feature**.</span></span> <span data-ttu-id="bf78c-144">Mivel ezek az objektumok érvényes JSON-adatokat, akkor is összes tárolása és feldolgozása történhet az Azure Cosmos-Adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="bf78c-144">Since these objects are valid JSON, they can all be stored and processed in Azure Cosmos DB.</span></span> <span data-ttu-id="bf78c-145">Azonban Azure Cosmos DB csak akkor támogatja a pontok automatikus indexeléshez.</span><span class="sxs-lookup"><span data-stu-id="bf78c-145">However Azure Cosmos DB only supports automatic indexing of points.</span></span>

### <a name="coordinate-reference-systems"></a><span data-ttu-id="bf78c-146">Ütemezze referenciarendszert</span><span class="sxs-lookup"><span data-stu-id="bf78c-146">Coordinate reference systems</span></span>
<span data-ttu-id="bf78c-147">Mivel a hello earth hello alakját szabálytalan, koordinátáját földrajzi adatok a hány koordináta referenciarendszert (CRS), mindegyiket a saját keretek a referencia- és mértékegységek jelzi.</span><span class="sxs-lookup"><span data-stu-id="bf78c-147">Since hello shape of hello earth is irregular, coordinates of geospatial data is represented in many coordinate reference systems (CRS), each with their own frames of reference and units of measurement.</span></span> <span data-ttu-id="bf78c-148">Például "Britannia nemzeti rács" hello egy referenciarendszert nagyon pontos hello Egyesült Királyságban, de nem kívül.</span><span class="sxs-lookup"><span data-stu-id="bf78c-148">For example, hello "National Grid of Britain" is a reference system is very accurate for hello United Kingdom, but not outside it.</span></span> 

<span data-ttu-id="bf78c-149">hello használja a legnépszerűbb CRS ma hello World geodéziai rendszer [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span><span class="sxs-lookup"><span data-stu-id="bf78c-149">hello most popular CRS in use today is hello World Geodetic System  [WGS-84](http://earth-info.nga.mil/GandG/wgs84/).</span></span> <span data-ttu-id="bf78c-150">GPS-eszközöket, és számos leképezési szolgáltatás, beleértve a Google térképeket és a Bing térképek API-k használata WGS-84.</span><span class="sxs-lookup"><span data-stu-id="bf78c-150">GPS devices, and many mapping services including Google Maps and Bing Maps APIs use WGS-84.</span></span> <span data-ttu-id="bf78c-151">Azure Cosmos-adatbázis indexelő és WGS-84 CRS hello segítségével a földrajzi adatok lekérdezését támogatja.</span><span class="sxs-lookup"><span data-stu-id="bf78c-151">Azure Cosmos DB supports indexing and querying of geospatial data using hello WGS-84 CRS only.</span></span> 

## <a name="creating-documents-with-spatial-data"></a><span data-ttu-id="bf78c-152">Térbeli adatokat tartalmazó dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="bf78c-152">Creating documents with spatial data</span></span>
<span data-ttu-id="bf78c-153">GeoJSON értékeket tartalmazó dokumentumok létrehozásához automatikusan indexelt az egy térbeli index a összhangban toohello indexelési házirendet hello gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="bf78c-153">When you create documents that contain GeoJSON values, they are automatically indexed with a spatial index in accordance toohello indexing policy of hello collection.</span></span> <span data-ttu-id="bf78c-154">Ha egy Azure Cosmos DB SDK dinamikusan gépelt Python vagy Node.js nyelven dolgozik, érvényes GeoJSON kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="bf78c-154">If you're working with an Azure Cosmos DB SDK in a dynamically typed language like Python or Node.js, you must create valid GeoJSON.</span></span>

<span data-ttu-id="bf78c-155">**A földrajzi adatok node.js dokumentum létrehozása**</span><span class="sxs-lookup"><span data-stu-id="bf78c-155">**Create Document with Geospatial data in Node.js**</span></span>

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

<span data-ttu-id="bf78c-156">Hello DocumentDB API-k használata, ha használható hello `Point` és `Polygon` hello osztályok `Microsoft.Azure.Documents.Spatial` névtér tooembed helyére vonatkozó információkat az alkalmazás objektumának belül.</span><span class="sxs-lookup"><span data-stu-id="bf78c-156">If you're working with hello DocumentDB APIs, you can use hello `Point` and `Polygon` classes within hello `Microsoft.Azure.Documents.Spatial` namespace tooembed location information within your application objects.</span></span> <span data-ttu-id="bf78c-157">Ezeket az osztályokat hello szerializálás és a térbeli adatok deszerializálása be GeoJSON leegyszerűsíti.</span><span class="sxs-lookup"><span data-stu-id="bf78c-157">These classes help simplify hello serialization and deserialization of spatial data into GeoJSON.</span></span>

<span data-ttu-id="bf78c-158">**A .NET földrajzi adatok dokumentum létrehozása**</span><span class="sxs-lookup"><span data-stu-id="bf78c-158">**Create Document with Geospatial data in .NET**</span></span>

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

<span data-ttu-id="bf78c-159">Ha nem rendelkezik hello szélességi és hosszúsági adatokat, de hello fizikai címeket vagy a hely neve, mint a város vagy ország, például a Bing Maps REST szolgáltatások geokódolás szolgáltatás használatával megjeleníthetők hello tényleges koordináták.</span><span class="sxs-lookup"><span data-stu-id="bf78c-159">If you don't have hello latitude and longitude information, but have hello physical addresses or location name like city or country, you can look up hello actual coordinates by using a geocoding service like Bing Maps REST Services.</span></span> <span data-ttu-id="bf78c-160">További információ a Bing Maps geokódolás [Itt](https://msdn.microsoft.com/library/ff701713.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf78c-160">Learn more about Bing Maps geocoding [here](https://msdn.microsoft.com/library/ff701713.aspx).</span></span>

## <a name="querying-spatial-types"></a><span data-ttu-id="bf78c-161">A térbeli típusok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="bf78c-161">Querying spatial types</span></span>
<span data-ttu-id="bf78c-162">Most, hogy hogyan egy pillantást azt korábban hozott tooinsert földrajzi adatok, vessen egy pillantást hogyan tooquery ezek az adatok Azure Cosmos DB SQL és a LINQ használatával.</span><span class="sxs-lookup"><span data-stu-id="bf78c-162">Now that we've taken a look at how tooinsert geospatial data, let's take a look at how tooquery this data using Azure Cosmos DB using SQL and LINQ.</span></span>

### <a name="spatial-sql-built-in-functions"></a><span data-ttu-id="bf78c-163">Térbeli SQL beépített funkciók</span><span class="sxs-lookup"><span data-stu-id="bf78c-163">Spatial SQL built-in functions</span></span>
<span data-ttu-id="bf78c-164">Azure Cosmos DB nyissa meg a földrajzi konzorcium (OGC) beépített funkciók földrajzi lekérdezése a következő hello támogatja.</span><span class="sxs-lookup"><span data-stu-id="bf78c-164">Azure Cosmos DB supports hello following Open Geospatial Consortium (OGC) built-in functions for geospatial querying.</span></span> <span data-ttu-id="bf78c-165">A hello SQL nyelvi beépített függvények teljes készletének hello további információkért tekintse meg az túl[lekérdezés Azure Cosmos DB](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="bf78c-165">For more details on hello complete set of built-in functions in hello SQL language, please refer too[Query Azure Cosmos DB](documentdb-sql-query.md).</span></span>

<table>
<tr>
  <td><span data-ttu-id="bf78c-166"><strong>Használat</strong></span><span class="sxs-lookup"><span data-stu-id="bf78c-166"><strong>Usage</strong></span></span></td>
  <td><span data-ttu-id="bf78c-167"><strong>Leírás</strong></span><span class="sxs-lookup"><span data-stu-id="bf78c-167"><strong>Description</strong></span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="bf78c-168">ST_DISTANCE (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="bf78c-168">ST_DISTANCE (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="bf78c-169">A két hello GeoJSON-pont, sokszög vagy LineString kifejezések hello távolság visszaadása.</span><span class="sxs-lookup"><span data-stu-id="bf78c-169">Returns hello distance between hello two GeoJSON Point, Polygon, or LineString expressions.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="bf78c-170">ST_WITHIN (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="bf78c-170">ST_WITHIN (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="bf78c-171">Egy logikai kifejezés, amely azt jelzi, hogy hello első GeoJSON objektum (pont, Polygon, vagy LineString) hello második GeoJSON objektumon belül (pont, sokszög vagy LineString) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bf78c-171">Returns a Boolean expression indicating whether hello first GeoJSON object (Point, Polygon, or LineString) is within hello second GeoJSON object (Point, Polygon, or LineString).</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="bf78c-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span><span class="sxs-lookup"><span data-stu-id="bf78c-172">ST_INTERSECTS (spatial_expr, spatial_expr)</span></span></td>
  <td><span data-ttu-id="bf78c-173">Egy logikai kifejezés, amely azt jelzi, hogy az intersect hello két megadott GeoJSON objektumokat (pont, Polygon, vagy LineString) adja vissza.</span><span class="sxs-lookup"><span data-stu-id="bf78c-173">Returns a Boolean expression indicating whether hello two specified GeoJSON objects (Point, Polygon, or LineString) intersect.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="bf78c-174">ST_ISVALID</span><span class="sxs-lookup"><span data-stu-id="bf78c-174">ST_ISVALID</span></span></td>
  <td><span data-ttu-id="bf78c-175">Jelzi, hogy hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés érvényes logikai érték beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bf78c-175">Returns a Boolean value indicating whether hello specified GeoJSON Point, Polygon, or LineString expression is valid.</span></span></td>
</tr>
<tr>
  <td><span data-ttu-id="bf78c-176">ST_ISVALIDDETAILED</span><span class="sxs-lookup"><span data-stu-id="bf78c-176">ST_ISVALIDDETAILED</span></span></td>
  <td><span data-ttu-id="bf78c-177">Értéket ad eredményül, ha hello megadott GeoJSON-pont, sokszög vagy LineString kifejezés logikai értéket tartalmazó JSON-érték érvényes, és ha érvénytelen, továbbá hello OK karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="bf78c-177">Returns a JSON value containing a Boolean value if hello specified GeoJSON Point, Polygon, or LineString expression is valid, and if invalid, additionally hello reason as a string value.</span></span></td>
</tr>
</table>

<span data-ttu-id="bf78c-178">Térbeli funkciók lehet használt tooperform közelségi kapcsolat lekérdezések térbeli adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="bf78c-178">Spatial functions can be used tooperform proximity queries against spatial data.</span></span> <span data-ttu-id="bf78c-179">Például az itt a lekérdezés, amely visszaadja az összes termékcsalád dokumentumokat, hogy vannak 30 km-hello belül megadott helyen hello ST_DISTANCE beépített függvény használatával.</span><span class="sxs-lookup"><span data-stu-id="bf78c-179">For example, here's a query that returns all family documents that are within 30 km of hello specified location using hello ST_DISTANCE built-in function.</span></span> 

<span data-ttu-id="bf78c-180">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="bf78c-180">**Query**</span></span>

    SELECT f.id 
    FROM Families f 
    WHERE ST_DISTANCE(f.location, {'type': 'Point', 'coordinates':[31.9, -4.8]}) < 30000

<span data-ttu-id="bf78c-181">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="bf78c-181">**Results**</span></span>

    [{
      "id": "WakefieldFamily"
    }]

<span data-ttu-id="bf78c-182">Ha megadja a térbeli indexelő az indexelési házirendet, majd "távolság lekérdezések" szolgáltató hatékonyan hello index keresztül.</span><span class="sxs-lookup"><span data-stu-id="bf78c-182">If you include spatial indexing in your indexing policy, then "distance queries" will be served efficiently through hello index.</span></span> <span data-ttu-id="bf78c-183">A térbeli indexelő további részletekért lásd: hello című részhez.</span><span class="sxs-lookup"><span data-stu-id="bf78c-183">For more details on spatial indexing, please see hello section below.</span></span> <span data-ttu-id="bf78c-184">Ha még nem rendelkezik a térbeli index hello a megadott elérési utak, akkor is elvégezhetők térbeli lekérdezéseket megadásával `x-ms-documentdb-query-enable-scan` hello értékű kérelem fejléce túl "true" beállítása.</span><span class="sxs-lookup"><span data-stu-id="bf78c-184">If you don't have a spatial index for hello specified paths, you can still perform spatial queries by specifying `x-ms-documentdb-query-enable-scan` request header with hello value set too"true".</span></span> <span data-ttu-id="bf78c-185">A .NET, ehhez által választható átadásakor hello **FeedOptions** argumentum tooqueries rendelkező [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) tootrue beállítása.</span><span class="sxs-lookup"><span data-stu-id="bf78c-185">In .NET, this can be done by passing hello optional **FeedOptions** argument tooqueries with [EnableScanInQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.enablescaninquery.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.EnableScanInQuery) set tootrue.</span></span> 

<span data-ttu-id="bf78c-186">ST_WITHIN használt toocheck lehet, ha a pont egy Sokszögön belül helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="bf78c-186">ST_WITHIN can be used toocheck if a point lies within a Polygon.</span></span> <span data-ttu-id="bf78c-187">Sokszögek általánosan használt toorepresent határok, például az irányítószámot, állapot határokat, illetve természetes kialakulásához.</span><span class="sxs-lookup"><span data-stu-id="bf78c-187">Commonly Polygons are used toorepresent boundaries like zip codes, state boundaries, or natural formations.</span></span> <span data-ttu-id="bf78c-188">Újra Ha megadja a térbeli indexelő az indexelési házirendet, majd "belül" lekérdezések szolgáltató hatékonyan hello index keresztül.</span><span class="sxs-lookup"><span data-stu-id="bf78c-188">Again if you include spatial indexing in your indexing policy, then "within" queries will be served efficiently through hello index.</span></span> 

<span data-ttu-id="bf78c-189">ST_WITHIN sokszög argumentumainak tartalmazhat csak csengetés, azaz hello sokszögek nem tartalmazhat lyuk rajtuk.</span><span class="sxs-lookup"><span data-stu-id="bf78c-189">Polygon arguments in ST_WITHIN can contain only a single ring, i.e. hello Polygons must not contain holes in them.</span></span> 

<span data-ttu-id="bf78c-190">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="bf78c-190">**Query**</span></span>

    SELECT * 
    FROM Families f 
    WHERE ST_WITHIN(f.location, {
        'type':'Polygon', 
        'coordinates': [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
    })

<span data-ttu-id="bf78c-191">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="bf78c-191">**Results**</span></span>

    [{
      "id": "WakefieldFamily",
    }]

> [!NOTE]
> <span data-ttu-id="bf78c-192">Hasonló toohow eltérő típusú Azure Cosmos DB a lekérdezésben, akkor működik, ha vagy argumentuma helytelen formátumú vagy érvénytelen, akkor túl kiértékelik megadott hello hely érték**nem definiált** és értékelni hello dokumentum toobe kihagyta a hello a lekérdezés eredménye.</span><span class="sxs-lookup"><span data-stu-id="bf78c-192">Similar toohow mismatched types works in Azure Cosmos DB query, if hello location value specified in either argument is malformed or invalid, then it will evaluate too**undefined** and hello evaluated document toobe skipped from hello query results.</span></span> <span data-ttu-id="bf78c-193">Ha a lekérdezés eredménytelen, futtassa a ST_ISVALIDDETAILED toodebug miért hello spatail típusa érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="bf78c-193">If your query returns no results, run ST_ISVALIDDETAILED toodebug why hello spatail type is invalid.</span></span>     
> 
> 

<span data-ttu-id="bf78c-194">Cosmos. Azure-adatbázis is támogat más néven inverz lekérdezések végrehajtásához, azaz továbbra is sokszögek vagy Azure Cosmos DB sorainak index, majd egy adott pontot tartalmazó hello területek lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="bf78c-194">Azure Cosmos DB also supports performing inverse queries, i.e. you can index Polygons or lines in Azure Cosmos DB, then query for hello areas that contain a specified point.</span></span> <span data-ttu-id="bf78c-195">Ezt a mintát gyakran használják logisztikai tooidentify például amikor egy teherautó be- vagy egy meghatározott terület.</span><span class="sxs-lookup"><span data-stu-id="bf78c-195">This pattern is commonly used in logistics tooidentify e.g. when a truck enters or leaves a designated area.</span></span> 

<span data-ttu-id="bf78c-196">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="bf78c-196">**Query**</span></span>

    SELECT * 
    FROM Areas a 
    WHERE ST_WITHIN({'type': 'Point', 'coordinates':[31.9, -4.8]}, a.location)


<span data-ttu-id="bf78c-197">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="bf78c-197">**Results**</span></span>

    [{
      "id": "MyDesignatedLocation",
      "location": {
        "type":"Polygon", 
        "coordinates": [[[31.8, -5], [32, -5], [32, -4.7], [31.8, -4.7], [31.8, -5]]]
      }
    }]

<span data-ttu-id="bf78c-198">ST_ISVALID és ST_ISVALIDDETAILED használt toocheck lehet érvényes a térbeli objektum esetén.</span><span class="sxs-lookup"><span data-stu-id="bf78c-198">ST_ISVALID and ST_ISVALIDDETAILED can be used toocheck if a spatial object is valid.</span></span> <span data-ttu-id="bf78c-199">Például a következő lekérdezés hello ellenőrzi hello kívüli tartomány szélesség értékét (-132.8) az a pont érvényességét.</span><span class="sxs-lookup"><span data-stu-id="bf78c-199">For example, hello following query checks hello validity of a point with an out of range latitude value (-132.8).</span></span> <span data-ttu-id="bf78c-200">ST_ISVALID csak egy logikai értéket ad vissza, és ST_ISVALIDDETAILED beolvasása hello logikai érték, és miért akkor tekinthető érvénytelen hello OK tartalmazó karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="bf78c-200">ST_ISVALID returns just a Boolean value, and ST_ISVALIDDETAILED returns hello Boolean and a string containing hello reason why it is considered invalid.</span></span>

<span data-ttu-id="bf78c-201">** Lekérdezése **</span><span class="sxs-lookup"><span data-stu-id="bf78c-201">** Query **</span></span>

    SELECT ST_ISVALID({ "type": "Point", "coordinates": [31.9, -132.8] })

<span data-ttu-id="bf78c-202">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="bf78c-202">**Results**</span></span>

    [{
      "$1": false
    }]

<span data-ttu-id="bf78c-203">Ezeket a funkciókat is használt toovalidate sokszögek.</span><span class="sxs-lookup"><span data-stu-id="bf78c-203">These functions can also be used toovalidate Polygons.</span></span> <span data-ttu-id="bf78c-204">Például jelen példában használjuk ST_ISVALIDDETAILED toovalidate egy sokszög, amely nincs lezárva.</span><span class="sxs-lookup"><span data-stu-id="bf78c-204">For example, here we use ST_ISVALIDDETAILED toovalidate a Polygon that is not closed.</span></span> 

<span data-ttu-id="bf78c-205">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="bf78c-205">**Query**</span></span>

    SELECT ST_ISVALIDDETAILED({ "type": "Polygon", "coordinates": [[ 
        [ 31.8, -5 ], [ 31.8, -4.7 ], [ 32, -4.7 ], [ 32, -5 ] 
        ]]})

<span data-ttu-id="bf78c-206">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="bf78c-206">**Results**</span></span>

    [{
       "$1": { 
            "valid": false, 
            "reason": "hello Polygon input is not valid because hello start and end points of hello ring number 1 are not hello same. Each ring of a Polygon must have hello same start and end points." 
          }
    }]

### <a name="linq-querying-in-hello-net-sdk"></a><span data-ttu-id="bf78c-207">A .NET SDK hello lekérdezése LINQ</span><span class="sxs-lookup"><span data-stu-id="bf78c-207">LINQ Querying in hello .NET SDK</span></span>
<span data-ttu-id="bf78c-208">a DocumentDB .NET SDK hello is szolgáltatók helyettes módszerek `Distance()` és `Within()` LINQ kifejezés belüli használatra.</span><span class="sxs-lookup"><span data-stu-id="bf78c-208">hello DocumentDB .NET SDK also providers stub methods `Distance()` and `Within()` for use within LINQ expressions.</span></span> <span data-ttu-id="bf78c-209">hello DocumentDB LINQ szolgáltatónál fordítja le ezen metódus hívások toohello egyenértékű SQL beépített függvényhívásokat (ST_DISTANCE és ST_WITHIN rendre).</span><span class="sxs-lookup"><span data-stu-id="bf78c-209">hello DocumentDB LINQ provider translates these method calls toohello equivalent SQL built-in function calls (ST_DISTANCE and ST_WITHIN respectively).</span></span> 

<span data-ttu-id="bf78c-210">Az alábbiakban például a LINQ lekérdezés, amely megkeresi az összes dokumentum hello Azure Cosmos DB gyűjtemény, amelynek "hely" értékét egy radius hello 30 km belül megadott pont LINQ használatával.</span><span class="sxs-lookup"><span data-stu-id="bf78c-210">Here's an example of a LINQ query that finds all documents in hello Azure Cosmos DB collection whose "location" value is within a radius of 30km of hello specified point using LINQ.</span></span>

<span data-ttu-id="bf78c-211">**Távolság a LINQ lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="bf78c-211">**LINQ query for Distance**</span></span>

    foreach (UserProfile user in client.CreateDocumentQuery<UserProfile>(UriFactory.CreateDocumentCollectionUri("db", "profiles"))
        .Where(u => u.ProfileType == "Public" && a.Location.Distance(new Point(32.33, -4.66)) < 30000))
    {
        Console.WriteLine("\t" + user);
    }

<span data-ttu-id="bf78c-212">Ehhez hasonlóan az ide a lekérdezés minden hello dokumentumok, amelynek "hely" hello esik a megadott mező/sokszög kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="bf78c-212">Similarly, here's a query for finding all hello documents whose "location" is within hello specified box/Polygon.</span></span> 

<span data-ttu-id="bf78c-213">**LINQ lekérdezése a belül**</span><span class="sxs-lookup"><span data-stu-id="bf78c-213">**LINQ query for Within**</span></span>

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


<span data-ttu-id="bf78c-214">Most, hogy hogyan egy pillantást azt korábban hozott tooquery dokumentumok LINQ és az SQL, vessen egy pillantást hogyan tooconfigure Azure Cosmos DB térbeli indexeléshez.</span><span class="sxs-lookup"><span data-stu-id="bf78c-214">Now that we've taken a look at how tooquery documents using LINQ and SQL, let's take a look at how tooconfigure Azure Cosmos DB for spatial indexing.</span></span>

## <a name="indexing"></a><span data-ttu-id="bf78c-215">Indexelés</span><span class="sxs-lookup"><span data-stu-id="bf78c-215">Indexing</span></span>
<span data-ttu-id="bf78c-216">A Microsoft hello leírtak [séma Azure Cosmos DB indexelés független](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) papír, a Microsoft Azure Cosmos DB database engine toobe tervezett valóban sémát független és első osztályú támogatást nyújtanak a JSON.</span><span class="sxs-lookup"><span data-stu-id="bf78c-216">As we described in hello [Schema Agnostic Indexing with Azure Cosmos DB](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) paper, we designed Azure Cosmos DB’s database engine toobe truly schema agnostic and provide first class support for JSON.</span></span> <span data-ttu-id="bf78c-217">optimalizált írási adatbázismotor hello Azure Cosmos-adatbázis natív módon megértette hello GeoJSON standard képviselt térbeli adatok (mutat, sokszögek és sorok).</span><span class="sxs-lookup"><span data-stu-id="bf78c-217">hello write optimized database engine of Azure Cosmos DB natively understands spatial data (points, Polygons and lines) represented in hello GeoJSON standard.</span></span>

<span data-ttu-id="bf78c-218">A ez már a vég, hello geometriai alakzatot 2D vezérlősík geodéziai koordinátái a tervezett, akkor cellák fokozatosan osztva egy **quadtree**.</span><span class="sxs-lookup"><span data-stu-id="bf78c-218">In a nutshell, hello geometry is projected from geodetic coordinates onto a 2D plane then divided progressively into cells using a **quadtree**.</span></span> <span data-ttu-id="bf78c-219">E cellák csatlakoztatott too1D hello cellát hello helye alapján egy **Hilbert terület töltés görbe**, amely megőrzi az pontok helység szerint.</span><span class="sxs-lookup"><span data-stu-id="bf78c-219">These cells are mapped too1D based on hello location of hello cell within a **Hilbert space filling curve**, which preserves locality of points.</span></span> <span data-ttu-id="bf78c-220">Továbbá ha helyadatok indexelt, mert végig kell vinnie néven ismert folyamat **tesszellációs**, azaz összes hello cellákat egy helyre intersect azonosítva és hello Azure Cosmos DB index kulcsokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="bf78c-220">Additionally when location data is indexed, it goes through a process known as **tessellation**, i.e. all hello cells that intersect a location are identified and stored as keys in hello Azure Cosmos DB index.</span></span> <span data-ttu-id="bf78c-221">Időben argumentumok hasonlóan pontok és a sokszög is Csempézett tooextract hello vonatkozó cella azonosító tartományokat, majd hello index tooretrieve adatait használja.</span><span class="sxs-lookup"><span data-stu-id="bf78c-221">At query time, arguments like points and Polygons are also tessellated tooextract hello relevant cell ID ranges, then used tooretrieve data from hello index.</span></span>

<span data-ttu-id="bf78c-222">Ha meg van-e az indexelési házirendet, amely tartalmazza a térbeli index / * (összes elérési utat), majd hello gyűjteményben található összes pontok indexelt hatékony térbeli lekérdezésekhez (ST_WITHIN és ST_DISTANCE).</span><span class="sxs-lookup"><span data-stu-id="bf78c-222">If you specify an indexing policy that includes spatial index for /* (all paths), then all points found within hello collection are indexed for efficient spatial queries (ST_WITHIN and ST_DISTANCE).</span></span> <span data-ttu-id="bf78c-223">A térbeli indexek nem pontosság értéket, és mindig az alapértelmezett pontosság értéket használjon.</span><span class="sxs-lookup"><span data-stu-id="bf78c-223">Spatial indexes do not have a precision value, and always use a default precision value.</span></span>

> [!NOTE]
> <span data-ttu-id="bf78c-224">Azure Cosmos DB támogatja az automatikus indexeléshez pontok, sokszögek és Linestring</span><span class="sxs-lookup"><span data-stu-id="bf78c-224">Azure Cosmos DB supports automatic indexing of Points, Polygons, and LineStrings</span></span>
> 
> 

<span data-ttu-id="bf78c-225">hello alábbi JSON kódrészletben láthatja az indexelési házirendet a térbeli indexelő engedélyezve van, vagyis index bármely GeoJSON-pont térbeli lekérdezése dokumentumok belül található.</span><span class="sxs-lookup"><span data-stu-id="bf78c-225">hello following JSON snippet shows an indexing policy with spatial indexing enabled, i.e. index any GeoJSON point found within documents for spatial querying.</span></span> <span data-ttu-id="bf78c-226">Ha módosítja az indexelő hello Azure portál használatával hello, JSON következő térbeli a gyűjtemény az indexelési házirendet tooenable indexeléshez hello is megadhat.</span><span class="sxs-lookup"><span data-stu-id="bf78c-226">If you are modifying hello indexing policy using hello Azure Portal, you can specify hello following JSON for indexing policy tooenable spatial indexing on your collection.</span></span>

<span data-ttu-id="bf78c-227">**Gyűjtemény indexelő házirend JSON-t Spatial pontokhoz, illetve sokszögek engedélyezve**</span><span class="sxs-lookup"><span data-stu-id="bf78c-227">**Collection Indexing Policy JSON with Spatial enabled for points and Polygons**</span></span>

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

<span data-ttu-id="bf78c-228">Íme egy kódrészletet a .NET, amely bemutatja, hogyan toocreate egy gyűjteménybe és térbeli indexelő engedélyezve van a pontokat tartalmazó összes elérési utat.</span><span class="sxs-lookup"><span data-stu-id="bf78c-228">Here's a code snippet in .NET that shows how toocreate a collection with spatial indexing turned on for all paths containing points.</span></span> 

<span data-ttu-id="bf78c-229">**Hozzon létre gyűjteményt térbeli indexelő**</span><span class="sxs-lookup"><span data-stu-id="bf78c-229">**Create a collection with spatial indexing**</span></span>

    DocumentCollection spatialData = new DocumentCollection()
    spatialData.IndexingPolicy = new IndexingPolicy(new SpatialIndex(DataType.Point)); //override tooturn spatial on by default
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), spatialData);

<span data-ttu-id="bf78c-230">És ez hogyan módosíthatja egy meglévő gyűjteményt tootake előnyeit térbeli indexelő felülírja a dokumentumok tárolt pontokat.</span><span class="sxs-lookup"><span data-stu-id="bf78c-230">And here's how you can modify an existing collection tootake advantage of spatial indexing over any points that are stored within documents.</span></span>

<span data-ttu-id="bf78c-231">**Módosítsa a térbeli indexelő egy meglevő gyűjteményhez**</span><span class="sxs-lookup"><span data-stu-id="bf78c-231">**Modify an existing collection with spatial indexing**</span></span>

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
> <span data-ttu-id="bf78c-232">Ha hello hely hello dokumentumban GeoJSON érték helytelen formátumú vagy érvénytelen, majd azt nem get indexelve térbeli lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="bf78c-232">If hello location GeoJSON value within hello document is malformed or invalid, then it will not get indexed for spatial querying.</span></span> <span data-ttu-id="bf78c-233">ST_ISVALID és ST_ISVALIDDETAILED értékei ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="bf78c-233">You can validate location values using ST_ISVALID and ST_ISVALIDDETAILED.</span></span>
> 
> <span data-ttu-id="bf78c-234">Ha a gyűjtemény definíciója tartalmaz egy partíciókulcsot, átalakítási folyamat indexelő nem jelzett.</span><span class="sxs-lookup"><span data-stu-id="bf78c-234">If your collection definition includes a partition key, indexing transformation progress is not reported.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="bf78c-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf78c-235">Next steps</span></span>
<span data-ttu-id="bf78c-236">Most, hogy értesült kapcsolatos tooget indításának Azure Cosmos DB földrajzi támogatása, is:</span><span class="sxs-lookup"><span data-stu-id="bf78c-236">Now that you've learnt about how tooget started with geospatial support in Azure Cosmos DB, you can:</span></span>

* <span data-ttu-id="bf78c-237">A hello elkezdésére [földrajzi .NET mintakódjainak megtekintése a Githubon](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="bf78c-237">Start coding with hello [Geospatial .NET code samples on GitHub](https://github.com/Azure/azure-documentdb-dotnet/blob/fcf23d134fc5019397dcf7ab97d8d6456cd94820/samples/code-samples/Geospatial/Program.cs)</span></span>
* <span data-ttu-id="bf78c-238">A földrajzi lekérdezése: hello lekérdezni beavatkozás nélküli [Azure Cosmos DB Tesztlekérdezéseket](http://www.documentdb.com/sql/demo#geospatial)</span><span class="sxs-lookup"><span data-stu-id="bf78c-238">Get hands on with geospatial querying at hello [Azure Cosmos DB Query Playground](http://www.documentdb.com/sql/demo#geospatial)</span></span>
* <span data-ttu-id="bf78c-239">További információ [Azure Cosmos adatbázis-lekérdezés](documentdb-sql-query.md)</span><span class="sxs-lookup"><span data-stu-id="bf78c-239">Learn more about [Azure Cosmos DB Query](documentdb-sql-query.md)</span></span>
* <span data-ttu-id="bf78c-240">További információ [Azure Cosmos DB indexelő házirendek](indexing-policies.md)</span><span class="sxs-lookup"><span data-stu-id="bf78c-240">Learn more about [Azure Cosmos DB Indexing Policies](indexing-policies.md)</span></span>

