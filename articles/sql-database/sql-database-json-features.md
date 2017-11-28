---
title: "SQL-adatbázis JSON-szolgáltatások aaaAzure |} Microsoft Docs"
description: "Az Azure SQL Database lehetővé teszi tooparse, a lekérdezés és a JavaScript Object Notation (JSON) jelöléssel adatok formázása."
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
ms.openlocfilehash: 30a31a1b01482ec276646b6fd6ca0c1f581168d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a><span data-ttu-id="b6601-103">Ismerkedés az Azure SQL Database JSON szolgáltatásai</span><span class="sxs-lookup"><span data-stu-id="b6601-103">Getting started with JSON features in Azure SQL Database</span></span>
<span data-ttu-id="b6601-104">Az Azure SQL Database lehetővé teszi elemezni, és JavaScript Object Notation ábrázolt adatok lekérdezése [(JSON)](http://www.json.org/) formázása, és a relációs adatok exportálása JSON-szövegként.</span><span class="sxs-lookup"><span data-stu-id="b6601-104">Azure SQL Database lets you parse and query data represented in JavaScript Object Notation [(JSON)](http://www.json.org/) format, and export your relational data as JSON text.</span></span>

<span data-ttu-id="b6601-105">JSON-ja adatcsere modern webes és mobilalkalmazások használt népszerű adatformátum.</span><span class="sxs-lookup"><span data-stu-id="b6601-105">JSON is a popular data format used for exchanging data in modern web and mobile applications.</span></span> <span data-ttu-id="b6601-106">JSON is félig strukturált adatok tárolásához, naplófájlokban vagy hasonló NoSQL-adatbázisok használt [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="b6601-106">JSON is also used for storing semi-structured data in log files or in NoSQL databases like [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/).</span></span> <span data-ttu-id="b6601-107">Sok REST webszolgáltatások ad találatot formázott JSON-szövegként, vagy fogadja el az adatok JSON formátumú.</span><span class="sxs-lookup"><span data-stu-id="b6601-107">Many REST web services return results formatted as JSON text or accept data formatted as JSON.</span></span> <span data-ttu-id="b6601-108">A legtöbb Azure szolgáltatásokkal – például [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), és [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) REST-végpont vissza vagy JSON felhasználását.</span><span class="sxs-lookup"><span data-stu-id="b6601-108">Most Azure services such as [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), and [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) have REST endpoints that return or consume JSON.</span></span>

<span data-ttu-id="b6601-109">Azure SQL Database lehetővé teszi a JSON-adatok egyszerűen dolgozhat, és az adatbázis olyan modern-szolgáltatásokkal integrálja.</span><span class="sxs-lookup"><span data-stu-id="b6601-109">Azure SQL Database lets you work with JSON data easily and integrate your database with modern services.</span></span>

## <a name="overview"></a><span data-ttu-id="b6601-110">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b6601-110">Overview</span></span>
<span data-ttu-id="b6601-111">Az Azure SQL-adatbázis a következő funkciók a JSON-adatokkal végzett munka hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b6601-111">Azure SQL Database provides hello following functions for working with JSON data:</span></span>

![JSON-funkciók](./media/sql-database-json-features/image_1.png)

<span data-ttu-id="b6601-113">Ha JSON-szöveg, JSON kinyeri az adatokat, vagy győződjön meg arról, hogy JSON megfelelően van-e formázva hello beépített funkciókkal [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), és [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx) .</span><span class="sxs-lookup"><span data-stu-id="b6601-113">If you have JSON text, you can extract data from JSON or verify that JSON is properly formatted by using hello built-in functions [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), and [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx).</span></span> <span data-ttu-id="b6601-114">Hello [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funkció lehetővé teszi, hogy frissítse JSON-szövegben lévő értéket.</span><span class="sxs-lookup"><span data-stu-id="b6601-114">hello [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) function lets you update value inside JSON text.</span></span> <span data-ttu-id="b6601-115">További speciális lekérdezés és elemzés céljából, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) függvény a sorokra alakíthatja át a JSON-objektumok tömbjét.</span><span class="sxs-lookup"><span data-stu-id="b6601-115">For more advanced querying and analysis, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) function can transform an array of JSON objects into a set of rows.</span></span> <span data-ttu-id="b6601-116">SQL-lekérdezés eredményhalmazt adott hello hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="b6601-116">Any SQL query can be executed on hello returned result set.</span></span> <span data-ttu-id="b6601-117">Végül pedig a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) záradékot, amely lehetővé teszi, hogy formázni a JSON-szövegként a relációs táblákban tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="b6601-117">Finally, there is a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) clause that lets you format data stored in your relational tables as JSON text.</span></span>

## <a name="formatting-relational-data-in-json-format"></a><span data-ttu-id="b6601-118">Formázási relációs adatok JSON formátumban</span><span class="sxs-lookup"><span data-stu-id="b6601-118">Formatting relational data in JSON format</span></span>
<span data-ttu-id="b6601-119">Ha egy webszolgáltatás-bővítmény vesz hello adatbázis réteg és biztosít egy válasz JSON formátumú, vagy JSON-ként formázott ügyféloldali JavaScript-keretrendszerek vagy szalagtár szerepel, amely adatokat fogad, közvetlenül az SQL-lekérdezést az adatbázis-tartalom formázhatók JSON-ként.</span><span class="sxs-lookup"><span data-stu-id="b6601-119">If you have a web service that takes data from hello database layer and provides a response in JSON format, or client-side JavaScript frameworks or libraries that accept data formatted as JSON, you can format your database content as JSON directly in a SQL query.</span></span> <span data-ttu-id="b6601-120">Ön már nem formázza a JSON-NÁ, mert az Azure SQL Database eredményeinek toowrite alkalmazáskód van vagy néhány JSON szerializálási függvénytár tooconvert táblázatos lekérdezési eredmények belefoglalása, és majd szerializálható objektumok tooJSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="b6601-120">You no longer have toowrite application code that formats results from Azure SQL Database as JSON, or include some JSON serialization library tooconvert tabular query results and then serialize objects tooJSON format.</span></span> <span data-ttu-id="b6601-121">Ehelyett a JSON záradék tooformat SQL lekérdezési eredmények hello használata az Azure SQL Database JSON, és közvetlenül az alkalmazás használatát.</span><span class="sxs-lookup"><span data-stu-id="b6601-121">Instead, you can use hello FOR JSON clause tooformat SQL query results as JSON in Azure SQL Database and use it directly in your application.</span></span>

<span data-ttu-id="b6601-122">A következő példa hello, hello Sales.Customer tábla azon sorait-ként formázott JSON hello FOR JSON záradék használatával:</span><span class="sxs-lookup"><span data-stu-id="b6601-122">In hello following example, rows from hello Sales.Customer table are formatted as JSON by using hello FOR JSON clause:</span></span>

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

<span data-ttu-id="b6601-123">hello FOR JSON Path UTASÍTÁST záradék hello hello lekérdezési eredmények JSON-szövegként formázza.</span><span class="sxs-lookup"><span data-stu-id="b6601-123">hello FOR JSON PATH clause formats hello results of hello query as JSON text.</span></span> <span data-ttu-id="b6601-124">Oszlopnevek használatosak kulcsok, amíg hello cellaértékeket JSON értékként jönnek létre:</span><span class="sxs-lookup"><span data-stu-id="b6601-124">Column names are used as keys, while hello cell values are generated as JSON values:</span></span>

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

<span data-ttu-id="b6601-125">hello eredményhalmaz formátuma egy JSON-tömb, ahol mindegyik sor külön JSON-objektumként van formázva.</span><span class="sxs-lookup"><span data-stu-id="b6601-125">hello result set is formatted as a JSON array where each row is formatted as a separate JSON object.</span></span>

<span data-ttu-id="b6601-126">Elérési út, az azt jelenti, hogy testre szabhatja a JSON eredményének hello kimeneti formátum oszlopaliasnévként pontjelöléssel.</span><span class="sxs-lookup"><span data-stu-id="b6601-126">PATH indicates that you can customize hello output format of your JSON result by using dot notation in column aliases.</span></span> <span data-ttu-id="b6601-127">a következő lekérdezés hello hello "CustomerName" kulcs hello kimeneti JSON formátumban hello neve megváltozik, és a telefon-és faxszáma beteszi hello "Ügyfél" altípusa objektum:</span><span class="sxs-lookup"><span data-stu-id="b6601-127">hello following query changes hello name of hello "CustomerName" key in hello output JSON format, and puts phone and fax numbers in hello "Contact" sub-object:</span></span>

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

<span data-ttu-id="b6601-128">Ez a lekérdezés eredményének hello így néz ki:</span><span class="sxs-lookup"><span data-stu-id="b6601-128">hello output of this query looks like this:</span></span>

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

<span data-ttu-id="b6601-129">Ebben a példában azt vissza egy adott JSON-objektum helyett egy tömb hello megadásával [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b6601-129">In this example we returned a single JSON object instead of an array by specifying hello [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) option.</span></span> <span data-ttu-id="b6601-130">Ezt a beállítást is használhatja, ha tudja, hogy a lekérdezés egyetlen objektumot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="b6601-130">You can use this option if you know that you are returning a single object as a result of query.</span></span>

<span data-ttu-id="b6601-131">hello fő hello FOR JSON záradék értéke, mellyel vissza összetett hierarchikus adatokat az adatbázisból, beágyazott JSON-objektumok vagy tömbök formázott.</span><span class="sxs-lookup"><span data-stu-id="b6601-131">hello main value of hello FOR JSON clause is that it lets you return complex hierarchical data from your database formatted as nested JSON objects or arrays.</span></span> <span data-ttu-id="b6601-132">a következő példa azt mutatja meg hogyan tooinclude rendelések toohello ügyfél tartozó rendelések beágyazott tömb hello:</span><span class="sxs-lookup"><span data-stu-id="b6601-132">hello following example shows how tooinclude Orders that belong toohello Customer as a nested array of Orders:</span></span>

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

<span data-ttu-id="b6601-133">Külön lekérdezésekké tooget ügyféladatok és toofetch kapcsolódó rendelések lista küldése helyett minden hello szükséges adatok egyetlen lekérdezéssel, ahogy az alábbi minta kimenet hello szerezheti meg:</span><span class="sxs-lookup"><span data-stu-id="b6601-133">Instead of sending separate queries tooget Customer data and then toofetch a list of related Orders, you can get all hello necessary data with a single query, as shown in hello following sample output:</span></span>

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

## <a name="working-with-json-data"></a><span data-ttu-id="b6601-134">JSON-adatok használata</span><span class="sxs-lookup"><span data-stu-id="b6601-134">Working with JSON data</span></span>
<span data-ttu-id="b6601-135">Ha nincs szigorúan strukturált adatok, ha olyan alárendelt összetett objektumok, tömbök vagy hierarchikus adatokhoz, vagy ha a adatstruktúrák verzióinformációk hello JSON formátumban segíthet toorepresent bármely összetett adatstruktúra.</span><span class="sxs-lookup"><span data-stu-id="b6601-135">If you don’t have strictly structured data, if you have complex sub-objects, arrays, or hierarchical data, or if your data structures evolve over time, hello JSON format can help you toorepresent any complex data structure.</span></span>

<span data-ttu-id="b6601-136">JSON-ja a szöveges formátum, mint minden más karakterlánc az Azure SQL Database használható.</span><span class="sxs-lookup"><span data-stu-id="b6601-136">JSON is a textual format that can be used like any other string type in Azure SQL Database.</span></span> <span data-ttu-id="b6601-137">Küldjön, vagy JSON-adatok tárolót, mint egy normál NVARCHAR:</span><span class="sxs-lookup"><span data-stu-id="b6601-137">You can send or store JSON data as a standard NVARCHAR:</span></span>

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

<span data-ttu-id="b6601-138">hello JSON-adatokat ebben a példában használt hello NVARCHAR(MAX) típust jelöli.</span><span class="sxs-lookup"><span data-stu-id="b6601-138">hello JSON data used in this example is represented by using hello NVARCHAR(MAX) type.</span></span> <span data-ttu-id="b6601-139">JSON szúrhatók be ezt a táblázatot, vagy a szabványos Transact-SQL-szintaxis használatával, ahogy az alábbi példa hello hello tárolt eljárás argumentumaként megadott:</span><span class="sxs-lookup"><span data-stu-id="b6601-139">JSON can be inserted into this table or provided as an argument of hello stored procedure using standard Transact-SQL syntax as shown in hello following example:</span></span>

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

<span data-ttu-id="b6601-140">Bármely ügyféloldali nyelvi vagy a könyvtárban, amely az Azure SQL-adatbázis karakterlánc-adatokkal is működik a JSON-adatokat.</span><span class="sxs-lookup"><span data-stu-id="b6601-140">Any client-side language or library that works with string data in Azure SQL Database will also work with JSON data.</span></span> <span data-ttu-id="b6601-141">JSON tárolhatja bármilyen, amely támogatja a hello NVARCHAR típus, például egy memóriaoptimalizált vagy tábla rendszerverzióval ellátott tábla.</span><span class="sxs-lookup"><span data-stu-id="b6601-141">JSON can be stored in any table that supports hello NVARCHAR type, such as a Memory-optimized table or a System-versioned table.</span></span> <span data-ttu-id="b6601-142">JSON nem vezet be bármilyen korlátozás hello ügyféloldali kódot vagy hello Adatbázisréteg.</span><span class="sxs-lookup"><span data-stu-id="b6601-142">JSON does not introduce any constraint either in hello client-side code or in hello database layer.</span></span>

## <a name="querying-json-data"></a><span data-ttu-id="b6601-143">JSON-adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="b6601-143">Querying JSON data</span></span>
<span data-ttu-id="b6601-144">Ha az Azure SQL-táblák tárolt JSON-ként formázott adatok, JSON funkciók lehetővé teszik, hogy ezeket az adatokat az SQL-lekérdezésben használni.</span><span class="sxs-lookup"><span data-stu-id="b6601-144">If you have data formatted as JSON stored in Azure SQL tables, JSON functions let you use this data in any SQL query.</span></span>

<span data-ttu-id="b6601-145">JSON-funkciók Azure SQL adatbázis lehetővé teszik a rendelkezésre álló úgy kezelje, mint bármely más SQL adattípus JSON formátumú adatok.</span><span class="sxs-lookup"><span data-stu-id="b6601-145">JSON functions that are available in Azure SQL database let you treat data formatted as JSON as any other SQL data type.</span></span> <span data-ttu-id="b6601-146">Könnyen értékek kinyerése hello JSON-szöveg, és JSON-adatokat a lekérdezésben használni:</span><span class="sxs-lookup"><span data-stu-id="b6601-146">You can easily extract values from hello JSON text, and use JSON data in any query:</span></span>

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

<span data-ttu-id="b6601-147">hello JSON_VALUE függvény értéket kiolvassa a hello adatok oszlopban tárolt JSON-szöveg.</span><span class="sxs-lookup"><span data-stu-id="b6601-147">hello JSON_VALUE function extracts a value from JSON text stored in hello Data column.</span></span> <span data-ttu-id="b6601-148">Ez a funkció a JavaScript-szerű elérési tooreference értéket a JSON-szöveg tooextract.</span><span class="sxs-lookup"><span data-stu-id="b6601-148">This function uses a JavaScript-like path tooreference a value in JSON text tooextract.</span></span> <span data-ttu-id="b6601-149">hello kiolvasott érték használható SQL-lekérdezésben bármely részén.</span><span class="sxs-lookup"><span data-stu-id="b6601-149">hello extracted value can be used in any part of SQL query.</span></span>

<span data-ttu-id="b6601-150">hello JSON_QUERY függvény hasonló tooJSON_VALUE.</span><span class="sxs-lookup"><span data-stu-id="b6601-150">hello JSON_QUERY function is similar tooJSON_VALUE.</span></span> <span data-ttu-id="b6601-151">Eltérően JSON_VALUE Ez a függvény ki összetett alárendelt objektumot, például a tömb, vagy a JSON-szövegben kerülnek objektumokat.</span><span class="sxs-lookup"><span data-stu-id="b6601-151">Unlike JSON_VALUE, this function extracts complex sub-object such as arrays or objects that are placed in JSON text.</span></span>

<span data-ttu-id="b6601-152">hello JSON_MODIFY függvény hello JSON-szöveg, amely frissíteni kell, valamint egy értéket, amely felülírja a régi hello hello elérési útja hello érték megadását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="b6601-152">hello JSON_MODIFY function lets you specify hello path of hello value in hello JSON text that should be updated, as well as a new value that will overwrite hello old one.</span></span> <span data-ttu-id="b6601-153">Ily módon egyszerűen frissítheti JSON-szöveg hello egészére reparsing nélkül.</span><span class="sxs-lookup"><span data-stu-id="b6601-153">This way you can easily update JSON text without reparsing hello entire structure.</span></span>

<span data-ttu-id="b6601-154">Mivel a JSON egyszerű szövegként tárolódnak, nincsenek nem garantálja, hogy a szöveges oszlop tárolt hello értékek megfelelően formázott.</span><span class="sxs-lookup"><span data-stu-id="b6601-154">Since JSON is stored in a standard text, there are no guarantees that hello values stored in text columns are properly formatted.</span></span> <span data-ttu-id="b6601-155">Ellenőrizheti, hogy a szöveg JSON oszlopban tárolt megfelelően van formázva szabványos Azure SQL Database ellenőrző korlátozásokat és hello ISJSON függvény használatával:</span><span class="sxs-lookup"><span data-stu-id="b6601-155">You can verify that text stored in JSON column is properly formatted by using standard Azure SQL Database check constraints and hello ISJSON function:</span></span>

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

<span data-ttu-id="b6601-156">Ha a bemeneti szöveg hello megfelelően van formázva JSON, a hello ISJSON függvény 1 hello értékét adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b6601-156">If hello input text is properly formatted JSON, hello ISJSON function returns hello value 1.</span></span> <span data-ttu-id="b6601-157">Minden Beszúrás vagy frissítés JSON oszlop ennél a határértéknél fog ellenőrizze, hogy új szöveges érték nem nem megfelelően formázott JSON-NÁ.</span><span class="sxs-lookup"><span data-stu-id="b6601-157">On every insert or update of JSON column, this constraint will verify that new text value is not malformed JSON.</span></span>

## <a name="transforming-json-into-tabular-format"></a><span data-ttu-id="b6601-158">A táblázatos formátumra JSON átalakítása</span><span class="sxs-lookup"><span data-stu-id="b6601-158">Transforming JSON into tabular format</span></span>
<span data-ttu-id="b6601-159">Az Azure SQL Database emellett lehetővé teszi a JSON-gyűjtemények adatokká táblázatos formátumban és a load vagy a lekérdezés JSON átalakító.</span><span class="sxs-lookup"><span data-stu-id="b6601-159">Azure SQL Database also lets you transform JSON collections into tabular format and load or query JSON data.</span></span>

<span data-ttu-id="b6601-160">OPENJSON elemzi a JSON-szöveg, megkeresi a JSON-objektumok tömbje, hello tömb elemei hello telepítéseket és egy olyan sor visszaadása hello kimeneti eredménye hello tömb egyes elemei a táblázat értékű függvényből.</span><span class="sxs-lookup"><span data-stu-id="b6601-160">OPENJSON is a table-value function that parses JSON text, locates an array of JSON objects, iterates through hello elements of hello array, and returns one row in hello output result for each element of hello array.</span></span>

![A táblázatos JSON](./media/sql-database-json-features/image_2.png)

<span data-ttu-id="b6601-162">Hello a fenti példában a adható meg ahol toolocate hello JSON-tömb, amelyeket meg kell nyitni (a hello $. Rendelések elérési út), mely oszlopok vissza kell adni az eredményként, és ha toofind hello visszaadott értékek JSON regisztrációja, mivel a cellák.</span><span class="sxs-lookup"><span data-stu-id="b6601-162">In hello example above, we can specify where toolocate hello JSON array that should be opened (in hello $.Orders path), what columns should be returned as result, and where toofind hello JSON values that will be returned as cells.</span></span>

<span data-ttu-id="b6601-163">Azt alakíthatja át a JSON-tömb a hello @orders sorokra, a változó eredménykészlet elemzése, vagy sorok beillesztéséhez egy szabványos táblázatot:</span><span class="sxs-lookup"><span data-stu-id="b6601-163">We can transform a JSON array in hello @orders variable into a set of rows, analyze this result set, or insert rows into a standard table:</span></span>

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
<span data-ttu-id="b6601-164">hello gyűjtemény rendelések formázott JSON-tömb, és a megadott paraméter toohello tárolt eljárás elemzése, és hello rendelések táblába.</span><span class="sxs-lookup"><span data-stu-id="b6601-164">hello collection of orders formatted as a JSON array and provided as a parameter toohello stored procedure can be parsed and inserted into hello Orders table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6601-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6601-165">Next steps</span></span>
<span data-ttu-id="b6601-166">toolearn hogyan toointegrate JSON az alkalmazásba, tekintse meg ezeket az erőforrásokat:</span><span class="sxs-lookup"><span data-stu-id="b6601-166">toolearn how toointegrate JSON into your application, check out these resources:</span></span>

* [<span data-ttu-id="b6601-167">TechNet Blog</span><span class="sxs-lookup"><span data-stu-id="b6601-167">TechNet Blog</span></span>](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [<span data-ttu-id="b6601-168">Az MSDN dokumentációját</span><span class="sxs-lookup"><span data-stu-id="b6601-168">MSDN documentation</span></span>](https://msdn.microsoft.com/library/dn921897.aspx)
* [<span data-ttu-id="b6601-169">A Channel 9 videót</span><span class="sxs-lookup"><span data-stu-id="b6601-169">Channel 9 video</span></span>](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

<span data-ttu-id="b6601-170">a JSON integrálása az alkalmazás különböző forgatókönyvekkel kapcsolatos toolearn lásd: hello bemutatók ezen [Channel 9 videót](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) vagy olyan forgatókönyvekben, amelyek a használati eset a megfelelő található [JSON blogbejegyzéseket](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span><span class="sxs-lookup"><span data-stu-id="b6601-170">toolearn about various scenarios for integrating JSON into your application, see hello demos in this [Channel 9 video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) or find a scenario that matches your use case in [JSON Blog posts](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).</span></span>

