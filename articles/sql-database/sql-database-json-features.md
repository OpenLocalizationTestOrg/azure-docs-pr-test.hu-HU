---
title: "Azure SQL adatbázis JSON-funkció |} Microsoft Docs"
description: "Az Azure SQL Database segítségével elemzési, lekérdezés, és adatok formázása a JavaScript Object Notation (JSON) jelöléssel."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 55860105-2f5f-4b10-87a0-99faa32b5653
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.date: 11/15/2016
ms.author: jovanpop
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.openlocfilehash: 883e661107dd838f5c381cdef2c7f891b9a9389c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a><span data-ttu-id="94a88-103">Ismerkedés az Azure SQL Database JSON szolgáltatásai</span><span class="sxs-lookup"><span data-stu-id="94a88-103">Getting started with JSON features in Azure SQL Database</span></span>
<span data-ttu-id="94a88-104">Az Azure SQL Database lehetővé teszi elemezni, és JavaScript Object Notation ábrázolt adatok lekérdezése [(JSON)](http://www.json.org/) formázása, és a relációs adatok exportálása JSON-szövegként.</span><span class="sxs-lookup"><span data-stu-id="94a88-104">Azure SQL Database lets you parse and query data represented in JavaScript Object Notation [(JSON)](http://www.json.org/) format, and export your relational data as JSON text.</span></span>

<span data-ttu-id="94a88-105">JSON-ja adatcsere modern webes és mobilalkalmazások használt népszerű adatformátum.</span><span class="sxs-lookup"><span data-stu-id="94a88-105">JSON is a popular data format used for exchanging data in modern web and mobile applications.</span></span> <span data-ttu-id="94a88-106">JSON is félig strukturált adatok tárolásához, naplófájlokban vagy hasonló NoSQL-adatbázisok használt [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="94a88-106">JSON is also used for storing semi-structured data in log files or in NoSQL databases like [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span></span> <span data-ttu-id="94a88-107">Sok REST webszolgáltatások ad találatot formázott JSON-szövegként, vagy fogadja el az adatok JSON formátumú.</span><span class="sxs-lookup"><span data-stu-id="94a88-107">Many REST web services return results formatted as JSON text or accept data formatted as JSON.</span></span> <span data-ttu-id="94a88-108">A legtöbb Azure szolgáltatásokkal – például [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), és [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) REST-végpont vissza vagy JSON felhasználását.</span><span class="sxs-lookup"><span data-stu-id="94a88-108">Most Azure services such as [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), and [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) have REST endpoints that return or consume JSON.</span></span>

<span data-ttu-id="94a88-109">Azure SQL Database lehetővé teszi a JSON-adatok egyszerűen dolgozhat, és az adatbázis olyan modern-szolgáltatásokkal integrálja.</span><span class="sxs-lookup"><span data-stu-id="94a88-109">Azure SQL Database lets you work with JSON data easily and integrate your database with modern services.</span></span>

## <a name="overview"></a><span data-ttu-id="94a88-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="94a88-110">Overview</span></span>
<span data-ttu-id="94a88-111">Az Azure SQL-adatbázis a következő funkciókat biztosít a JSON-adatokkal végzett munka:</span><span class="sxs-lookup"><span data-stu-id="94a88-111">Azure SQL Database provides the following functions for working with JSON data:</span></span>

![JSON-funkciók](./media/sql-database-json-features/image_1.png)

<span data-ttu-id="94a88-113">Ha JSON-szöveg, JSON kinyeri az adatokat, vagy győződjön meg arról, hogy a beépített funkciókkal megfelelően formázott JSON [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), és [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span><span class="sxs-lookup"><span data-stu-id="94a88-113">If you have JSON text, you can extract data from JSON or verify that JSON is properly formatted by using the built-in functions [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), and [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span></span> <span data-ttu-id="94a88-114">A [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funkció lehetővé teszi, hogy frissítse JSON-szövegben lévő értéket.</span><span class="sxs-lookup"><span data-stu-id="94a88-114">The [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) function lets you update value inside JSON text.</span></span> <span data-ttu-id="94a88-115">További speciális lekérdezés és elemzés céljából, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) függvény a sorokra alakíthatja át a JSON-objektumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="94a88-115">For more advanced querying and analysis, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) function can transform an array of JSON objects into a set of rows.</span></span> <span data-ttu-id="94a88-116">SQL-lekérdezést a visszaadott eredményhalmaz hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="94a88-116">Any SQL query can be executed on the returned result set.</span></span> <span data-ttu-id="94a88-117">Végül pedig a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) záradékot, amely lehetővé teszi, hogy formázni a JSON-szövegként a relációs táblákban tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="94a88-117">Finally, there is a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) clause that lets you format data stored in your relational tables as JSON text.</span></span>

## <a name="formatting-relational-data-in-json-format"></a><span data-ttu-id="94a88-118">Formázási relációs adatok JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="94a88-118">Formatting relational data in JSON format</span></span>
<span data-ttu-id="94a88-119">Ha egy webszolgáltatás-bővítmény vesz az adatbázis adatait réteg és biztosít egy válasz JSON formátumú, vagy JSON-ként formázott ügyféloldali JavaScript-keretrendszerek vagy szalagtár szerepel, amely adatokat fogad, közvetlenül az SQL-lekérdezést az adatbázis-tartalom formázhatók JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="94a88-119">If you have a web service that takes data from the database layer and provides a response in JSON format, or client-side JavaScript frameworks or libraries that accept data formatted as JSON, you can format your database content as JSON directly in a SQL query.</span></span> <span data-ttu-id="94a88-120">Már nem írása, amely az Azure SQL Database JSON-ként eredményeinek formázza alkalmazáskód használ, vagy bizonyos alakítsa át a táblázatos lekérdezési eredményeket, majd a JSON formátumban objektumokat szerializálni a JSON-szerializálási objektumtára tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="94a88-120">You no longer have to write application code that formats results from Azure SQL Database as JSON, or include some JSON serialization library to convert tabular query results and then serialize objects to JSON format.</span></span> <span data-ttu-id="94a88-121">A FOR JSON záradék használatával ehelyett SQL lekérdezési eredmények formázza a JSON-t az Azure SQL Database, és közvetlenül az alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="94a88-121">Instead, you can use the FOR JSON clause to format SQL query results as JSON in Azure SQL Database and use it directly in your application.</span></span>

<span data-ttu-id="94a88-122">A következő példában a Sales.Customer tábla azon sorait-ként formázott JSON a FOR JSON záradék használatával:</span><span class="sxs-lookup"><span data-stu-id="94a88-122">In the following example, rows from the Sales.Customer table are formatted as JSON by using the FOR JSON clause:</span></span>

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

<span data-ttu-id="94a88-123">A FOR JSON Path UTASÍTÁST záradék a lekérdezés eredményeinek JSON-szövegként formázza.</span><span class="sxs-lookup"><span data-stu-id="94a88-123">The FOR JSON PATH clause formats the results of the query as JSON text.</span></span> <span data-ttu-id="94a88-124">Oszlopnevek használatosak kulcsok, amíg az értékek JSON értékként jönnek létre:</span><span class="sxs-lookup"><span data-stu-id="94a88-124">Column names are used as keys, while the cell values are generated as JSON values:</span></span>

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

<span data-ttu-id="94a88-125">A JSON-tömb, ahol mindegyik sor külön JSON-objektumként van formázva az eredményhalmaz formátuma.</span><span class="sxs-lookup"><span data-stu-id="94a88-125">The result set is formatted as a JSON array where each row is formatted as a separate JSON object.</span></span>

<span data-ttu-id="94a88-126">Elérési út azt jelenti, hogy testre szabhatja a kimeneti formátum a JSON eredményének oszlopaliasnévként pontjelöléssel.</span><span class="sxs-lookup"><span data-stu-id="94a88-126">PATH indicates that you can customize the output format of your JSON result by using dot notation in column aliases.</span></span> <span data-ttu-id="94a88-127">A következő lekérdezés módosítja a kimeneti JSON formátumban a "CustomerName" kulcs nevét, és a telefon-és faxszáma helyezi az "Ügyfél" altípusa objektum:</span><span class="sxs-lookup"><span data-stu-id="94a88-127">The following query changes the name of the "CustomerName" key in the output JSON format, and puts phone and fax numbers in the "Contact" sub-object:</span></span>

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

<span data-ttu-id="94a88-128">Ez a lekérdezés eredményének így néz ki:</span><span class="sxs-lookup"><span data-stu-id="94a88-128">The output of this query looks like this:</span></span>

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

<span data-ttu-id="94a88-129">Ebben a példában azt egy adott JSON-objektum helyett egy tömb által visszaadott megadása a [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="94a88-129">In this example we returned a single JSON object instead of an array by specifying the [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) option.</span></span> <span data-ttu-id="94a88-130">Ezt a beállítást is használhatja, ha tudja, hogy a lekérdezés egyetlen objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="94a88-130">You can use this option if you know that you are returning a single object as a result of query.</span></span>

<span data-ttu-id="94a88-131">A fő a FOR JSON záradék értéke, mellyel vissza összetett hierarchikus adatokat az adatbázisból, beágyazott JSON-objektumok vagy tömbök formázott.</span><span class="sxs-lookup"><span data-stu-id="94a88-131">The main value of the FOR JSON clause is that it lets you return complex hierarchical data from your database formatted as nested JSON objects or arrays.</span></span> <span data-ttu-id="94a88-132">A következő példa bemutatja, hogyan tartalmazza, amelyek az ügyfél rendelések beágyazott tömb rendeléseket:</span><span class="sxs-lookup"><span data-stu-id="94a88-132">The following example shows how to include Orders that belong to the Customer as a nested array of Orders:</span></span>

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

<span data-ttu-id="94a88-133">Helyett külön felhasználói adatok lekérdezését, és majd beolvasni a kapcsolódó rendelések, kaphat egyetlen lekérdezést, az összes szükséges adatot küld a következő minta kimenet látható módon:</span><span class="sxs-lookup"><span data-stu-id="94a88-133">Instead of sending separate queries to get Customer data and then to fetch a list of related Orders, you can get all the necessary data with a single query, as shown in the following sample output:</span></span>

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a><span data-ttu-id="94a88-134">JSON-adatok használata</span><span class="sxs-lookup"><span data-stu-id="94a88-134">Working with JSON data</span></span>
<span data-ttu-id="94a88-135">Ha nincs szigorúan strukturált adatok, ha olyan alárendelt összetett objektumok, tömbök vagy hierarchikus adatokhoz, vagy a adatstruktúrák verzióinformációk, a JSON formátum segítségével bármely összetett adatszerkezet képviseli.</span><span class="sxs-lookup"><span data-stu-id="94a88-135">If you don’t have strictly structured data, if you have complex sub-objects, arrays, or hierarchical data, or if your data structures evolve over time, the JSON format can help you to represent any complex data structure.</span></span>

<span data-ttu-id="94a88-136">JSON-ja a szöveges formátum, mint minden más karakterlánc az Azure SQL Database használható.</span><span class="sxs-lookup"><span data-stu-id="94a88-136">JSON is a textual format that can be used like any other string type in Azure SQL Database.</span></span> <span data-ttu-id="94a88-137">Küldjön, vagy JSON-adatok tárolót, mint egy normál NVARCHAR:</span><span class="sxs-lookup"><span data-stu-id="94a88-137">You can send or store JSON data as a standard NVARCHAR:</span></span>

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

<span data-ttu-id="94a88-138">A JSON-adatokat ebben a példában használt jelképezi a NVARCHAR(MAX) típus használatával.</span><span class="sxs-lookup"><span data-stu-id="94a88-138">The JSON data used in this example is represented by using the NVARCHAR(MAX) type.</span></span> <span data-ttu-id="94a88-139">JSON szúrhatók be ezt a táblázatot, vagy a tárolt eljárás szabványos Transact-SQL-szintaxis használatával az alábbi példában látható módon argumentumaként megadott:</span><span class="sxs-lookup"><span data-stu-id="94a88-139">JSON can be inserted into this table or provided as an argument of the stored procedure using standard Transact-SQL syntax as shown in the following example:</span></span>

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

<span data-ttu-id="94a88-140">Bármely ügyféloldali nyelvi vagy a könyvtárban, amely az Azure SQL-adatbázis karakterlánc-adatokkal is működik a JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="94a88-140">Any client-side language or library that works with string data in Azure SQL Database will also work with JSON data.</span></span> <span data-ttu-id="94a88-141">JSON tárolhatja bármilyen, amely támogatja a NVARCHAR típus, például egy memóriaoptimalizált vagy tábla rendszerverzióval ellátott tábla.</span><span class="sxs-lookup"><span data-stu-id="94a88-141">JSON can be stored in any table that supports the NVARCHAR type, such as a Memory-optimized table or a System-versioned table.</span></span> <span data-ttu-id="94a88-142">JSON nem vezet be bármely megkötés vagy az ügyféloldali kódot, vagy az adatbázis-rétegben.</span><span class="sxs-lookup"><span data-stu-id="94a88-142">JSON does not introduce any constraint either in the client-side code or in the database layer.</span></span>

## <a name="querying-json-data"></a><span data-ttu-id="94a88-143">JSON-adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="94a88-143">Querying JSON data</span></span>
<span data-ttu-id="94a88-144">Ha az Azure SQL-táblák tárolt JSON-ként formázott adatok, JSON funkciók lehetővé teszik, hogy ezeket az adatokat az SQL-lekérdezésben használni.</span><span class="sxs-lookup"><span data-stu-id="94a88-144">If you have data formatted as JSON stored in Azure SQL tables, JSON functions let you use this data in any SQL query.</span></span>

<span data-ttu-id="94a88-145">JSON-funkciók Azure SQL adatbázis lehetővé teszik a rendelkezésre álló úgy kezelje, mint bármely más SQL adattípus JSON formátumú adatok.</span><span class="sxs-lookup"><span data-stu-id="94a88-145">JSON functions that are available in Azure SQL database let you treat data formatted as JSON as any other SQL data type.</span></span> <span data-ttu-id="94a88-146">Könnyen értékek kinyerése a JSON-szöveg, és JSON-adatokat a lekérdezésben használni:</span><span class="sxs-lookup"><span data-stu-id="94a88-146">You can easily extract values from the JSON text, and use JSON data in any query:</span></span>

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

<span data-ttu-id="94a88-147">A JSON_VALUE függvény értéket kiolvassa a az adatok oszlopban tárolt JSON-szöveg.</span><span class="sxs-lookup"><span data-stu-id="94a88-147">The JSON_VALUE function extracts a value from JSON text stored in the Data column.</span></span> <span data-ttu-id="94a88-148">Ez a funkció a JavaScript-szerű elérési út egy JSON-szövegben kibontásához értékre hivatkoznak.</span><span class="sxs-lookup"><span data-stu-id="94a88-148">This function uses a JavaScript-like path to reference a value in JSON text to extract.</span></span> <span data-ttu-id="94a88-149">A kiolvasott értékkel használható SQL-lekérdezésben bármely részén.</span><span class="sxs-lookup"><span data-stu-id="94a88-149">The extracted value can be used in any part of SQL query.</span></span>

<span data-ttu-id="94a88-150">A JSON_QUERY függvény JSON_VALUE hasonlít.</span><span class="sxs-lookup"><span data-stu-id="94a88-150">The JSON_QUERY function is similar to JSON_VALUE.</span></span> <span data-ttu-id="94a88-151">Eltérően JSON_VALUE Ez a függvény ki összetett alárendelt objektumot, például a tömb, vagy a JSON-szövegben kerülnek objektumokat.</span><span class="sxs-lookup"><span data-stu-id="94a88-151">Unlike JSON_VALUE, this function extracts complex sub-object such as arrays or objects that are placed in JSON text.</span></span>

<span data-ttu-id="94a88-152">A JSON_MODIFY funkció lehetővé teszi a JSON-szöveg, amelyet frissíteni kell, valamint egy értéket, amely a régit, azzal felülírja a érték elérési útjának megadása.</span><span class="sxs-lookup"><span data-stu-id="94a88-152">The JSON_MODIFY function lets you specify the path of the value in the JSON text that should be updated, as well as a new value that will overwrite the old one.</span></span> <span data-ttu-id="94a88-153">Ily módon egyszerűen frissítheti JSON-szöveg struktúra reparsing nélkül.</span><span class="sxs-lookup"><span data-stu-id="94a88-153">This way you can easily update JSON text without reparsing the entire structure.</span></span>

<span data-ttu-id="94a88-154">Mivel a JSON egyszerű szövegként tárolódnak, nincsenek nem garantálja, hogy a szöveges oszlop tárolt értékek megfelelően formázott.</span><span class="sxs-lookup"><span data-stu-id="94a88-154">Since JSON is stored in a standard text, there are no guarantees that the values stored in text columns are properly formatted.</span></span> <span data-ttu-id="94a88-155">Ellenőrizheti, hogy a szöveg JSON oszlopban tárolt megfelelően van-e formázva szabványos Azure SQL Database ellenőrzési korlátozásokban, és a ISJSON függvény használatával:</span><span class="sxs-lookup"><span data-stu-id="94a88-155">You can verify that text stored in JSON column is properly formatted by using standard Azure SQL Database check constraints and the ISJSON function:</span></span>

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

<span data-ttu-id="94a88-156">Ha a bemeneti szöveg megfelelően formázott JSON-ban, a ISJSON függvény adja vissza az 1.</span><span class="sxs-lookup"><span data-stu-id="94a88-156">If the input text is properly formatted JSON, the ISJSON function returns the value 1.</span></span> <span data-ttu-id="94a88-157">Minden Beszúrás vagy frissítés JSON oszlop ennél a határértéknél fog ellenőrizze, hogy új szöveges érték nem nem megfelelően formázott JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="94a88-157">On every insert or update of JSON column, this constraint will verify that new text value is not malformed JSON.</span></span>

## <a name="transforming-json-into-tabular-format"></a><span data-ttu-id="94a88-158">A táblázatos formátumra JSON átalakítása</span><span class="sxs-lookup"><span data-stu-id="94a88-158">Transforming JSON into tabular format</span></span>
<span data-ttu-id="94a88-159">Az Azure SQL Database emellett lehetővé teszi a JSON-gyűjtemények adatokká táblázatos formátumban és a load vagy a lekérdezés JSON átalakító.</span><span class="sxs-lookup"><span data-stu-id="94a88-159">Azure SQL Database also lets you transform JSON collections into tabular format and load or query JSON data.</span></span>

<span data-ttu-id="94a88-160">OPENJSON JSON-szöveg elemez, megkeresi a JSON-objektumok tömbje, a tömb elemei telepítéseket és több olyan sort adja vissza a kimeneti eredmények a tömb egyes elemei a táblázat értékű függvényből.</span><span class="sxs-lookup"><span data-stu-id="94a88-160">OPENJSON is a table-value function that parses JSON text, locates an array of JSON objects, iterates through the elements of the array, and returns one row in the output result for each element of the array.</span></span>

![A táblázatos JSON](./media/sql-database-json-features/image_2.png)

<span data-ttu-id="94a88-162">A fenti példában hol található a JSON-tömb, amely (az a $ meg kell nyitni adható meg. Rendelések elérési út), az eredmény, hol található a JSON-értékek cellákat visszaadott kell adható vissza oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="94a88-162">In the example above, we can specify where to locate the JSON array that should be opened (in the $.Orders path), what columns should be returned as result, and where to find the JSON values that will be returned as cells.</span></span>

<span data-ttu-id="94a88-163">Egy JSON-tömb, az azt alakíthatja át a @orders sorokra, a változó eredménykészlet elemzése, vagy sorok beillesztéséhez egy szabványos táblázatot:</span><span class="sxs-lookup"><span data-stu-id="94a88-163">We can transform a JSON array in the @orders variable into a set of rows, analyze this result set, or insert rows into a standard table:</span></span>

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
<span data-ttu-id="94a88-164">A gyűjtemény rendelések formázott JSON-tömb, és a tárolt eljárás paramétere elemzése, és a rendeléseket táblába beszúrt egyikéhez.</span><span class="sxs-lookup"><span data-stu-id="94a88-164">The collection of orders formatted as a JSON array and provided as a parameter to the stored procedure can be parsed and inserted into the Orders table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94a88-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94a88-165">Next steps</span></span>
<span data-ttu-id="94a88-166">Megtudhatja, hogyan integrálható a JSON az alkalmazásba, jelölje ki ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="94a88-166">To learn how to integrate JSON into your application, check out these resources:</span></span>

* [<span data-ttu-id="94a88-167">TechNet Blog</span><span class="sxs-lookup"><span data-stu-id="94a88-167">TechNet Blog</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [<span data-ttu-id="94a88-168">Az MSDN dokumentációját</span><span class="sxs-lookup"><span data-stu-id="94a88-168">MSDN documentation</span></span>](https://msdn.microsoft.com/library/dn921897.aspx)
* [<span data-ttu-id="94a88-169">A Channel 9 videót</span><span class="sxs-lookup"><span data-stu-id="94a88-169">Channel 9 video</span></span>](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

<span data-ttu-id="94a88-170">A JSON integrálása az alkalmazás különböző forgatókönyvekkel kapcsolatos további tudnivalókért lásd: ezen bemutatók [Channel 9 videót](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) vagy olyan forgatókönyvekben, amelyek a használati eset a megfelelő található [JSON blogbejegyzéseket](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span><span class="sxs-lookup"><span data-stu-id="94a88-170">To learn about various scenarios for integrating JSON into your application, see the demos in this [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) or find a scenario that matches your use case in [JSON Blog posts](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span></span>

