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
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Ismerkedés az Azure SQL Database JSON szolgáltatásai
Az Azure SQL Database lehetővé teszi elemezni, és JavaScript Object Notation ábrázolt adatok lekérdezése [(JSON)](http://www.json.org/) formázása, és a relációs adatok exportálása JSON-szövegként.

JSON-ja adatcsere modern webes és mobilalkalmazások használt népszerű adatformátum. JSON is félig strukturált adatok tárolásához, naplófájlokban vagy hasonló NoSQL-adatbázisok használt [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/). Sok REST webszolgáltatások ad találatot formázott JSON-szövegként, vagy fogadja el az adatok JSON formátumú. A legtöbb Azure szolgáltatásokkal – például [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/), és [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) REST-végpont vissza vagy JSON felhasználását.

Azure SQL Database lehetővé teszi a JSON-adatok egyszerűen dolgozhat, és az adatbázis olyan modern-szolgáltatásokkal integrálja.

## <a name="overview"></a>Áttekintés
Az Azure SQL-adatbázis a következő funkciók a JSON-adatokkal végzett munka hello biztosítja:

![JSON-funkciók](./media/sql-database-json-features/image_1.png)

Ha JSON-szöveg, JSON kinyeri az adatokat, vagy győződjön meg arról, hogy JSON megfelelően van-e formázva hello beépített funkciókkal [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx), és [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx) . Hello [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) funkció lehetővé teszi, hogy frissítse JSON-szövegben lévő értéket. További speciális lekérdezés és elemzés céljából, [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) függvény a sorokra alakíthatja át a JSON-objektumok tömbjét. SQL-lekérdezés eredményhalmazt adott hello hajtható végre. Végül pedig a [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) záradékot, amely lehetővé teszi, hogy formázni a JSON-szövegként a relációs táblákban tárolt adatokat.

## <a name="formatting-relational-data-in-json-format"></a>Formázási relációs adatok JSON formátumban
Ha egy webszolgáltatás-bővítmény vesz hello adatbázis réteg és biztosít egy válasz JSON formátumú, vagy JSON-ként formázott ügyféloldali JavaScript-keretrendszerek vagy szalagtár szerepel, amely adatokat fogad, közvetlenül az SQL-lekérdezést az adatbázis-tartalom formázhatók JSON-ként. Ön már nem formázza a JSON-NÁ, mert az Azure SQL Database eredményeinek toowrite alkalmazáskód van vagy néhány JSON szerializálási függvénytár tooconvert táblázatos lekérdezési eredmények belefoglalása, és majd szerializálható objektumok tooJSON formátumban. Ehelyett a JSON záradék tooformat SQL lekérdezési eredmények hello használata az Azure SQL Database JSON, és közvetlenül az alkalmazás használatát.

A következő példa hello, hello Sales.Customer tábla azon sorait-ként formázott JSON hello FOR JSON záradék használatával:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

hello FOR JSON Path UTASÍTÁST záradék hello hello lekérdezési eredmények JSON-szövegként formázza. Oszlopnevek használatosak kulcsok, amíg hello cellaértékeket JSON értékként jönnek létre:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

hello eredményhalmaz formátuma egy JSON-tömb, ahol mindegyik sor külön JSON-objektumként van formázva.

Elérési út, az azt jelenti, hogy testre szabhatja a JSON eredményének hello kimeneti formátum oszlopaliasnévként pontjelöléssel. a következő lekérdezés hello hello "CustomerName" kulcs hello kimeneti JSON formátumban hello neve megváltozik, és a telefon-és faxszáma beteszi hello "Ügyfél" altípusa objektum:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Ez a lekérdezés eredményének hello így néz ki:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

Ebben a példában azt vissza egy adott JSON-objektum helyett egy tömb hello megadásával [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) lehetőséget. Ezt a beállítást is használhatja, ha tudja, hogy a lekérdezés egyetlen objektumot ad vissza.

hello fő hello FOR JSON záradék értéke, mellyel vissza összetett hierarchikus adatokat az adatbázisból, beágyazott JSON-objektumok vagy tömbök formázott. a következő példa azt mutatja meg hogyan tooinclude rendelések toohello ügyfél tartozó rendelések beágyazott tömb hello:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Külön lekérdezésekké tooget ügyféladatok és toofetch kapcsolódó rendelések lista küldése helyett minden hello szükséges adatok egyetlen lekérdezéssel, ahogy az alábbi minta kimenet hello szerezheti meg:

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

## <a name="working-with-json-data"></a>JSON-adatok használata
Ha nincs szigorúan strukturált adatok, ha olyan alárendelt összetett objektumok, tömbök vagy hierarchikus adatokhoz, vagy ha a adatstruktúrák verzióinformációk hello JSON formátumban segíthet toorepresent bármely összetett adatstruktúra.

JSON-ja a szöveges formátum, mint minden más karakterlánc az Azure SQL Database használható. Küldjön, vagy JSON-adatok tárolót, mint egy normál NVARCHAR:

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

hello JSON-adatokat ebben a példában használt hello NVARCHAR(MAX) típust jelöli. JSON szúrhatók be ezt a táblázatot, vagy a szabványos Transact-SQL-szintaxis használatával, ahogy az alábbi példa hello hello tárolt eljárás argumentumaként megadott:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Bármely ügyféloldali nyelvi vagy a könyvtárban, amely az Azure SQL-adatbázis karakterlánc-adatokkal is működik a JSON-adatokat. JSON tárolhatja bármilyen, amely támogatja a hello NVARCHAR típus, például egy memóriaoptimalizált vagy tábla rendszerverzióval ellátott tábla. JSON nem vezet be bármilyen korlátozás hello ügyféloldali kódot vagy hello Adatbázisréteg.

## <a name="querying-json-data"></a>JSON-adatok lekérdezése
Ha az Azure SQL-táblák tárolt JSON-ként formázott adatok, JSON funkciók lehetővé teszik, hogy ezeket az adatokat az SQL-lekérdezésben használni.

JSON-funkciók Azure SQL adatbázis lehetővé teszik a rendelkezésre álló úgy kezelje, mint bármely más SQL adattípus JSON formátumú adatok. Könnyen értékek kinyerése hello JSON-szöveg, és JSON-adatokat a lekérdezésben használni:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

hello JSON_VALUE függvény értéket kiolvassa a hello adatok oszlopban tárolt JSON-szöveg. Ez a funkció a JavaScript-szerű elérési tooreference értéket a JSON-szöveg tooextract. hello kiolvasott érték használható SQL-lekérdezésben bármely részén.

hello JSON_QUERY függvény hasonló tooJSON_VALUE. Eltérően JSON_VALUE Ez a függvény ki összetett alárendelt objektumot, például a tömb, vagy a JSON-szövegben kerülnek objektumokat.

hello JSON_MODIFY függvény hello JSON-szöveg, amely frissíteni kell, valamint egy értéket, amely felülírja a régi hello hello elérési útja hello érték megadását teszi lehetővé. Ily módon egyszerűen frissítheti JSON-szöveg hello egészére reparsing nélkül.

Mivel a JSON egyszerű szövegként tárolódnak, nincsenek nem garantálja, hogy a szöveges oszlop tárolt hello értékek megfelelően formázott. Ellenőrizheti, hogy a szöveg JSON oszlopban tárolt megfelelően van formázva szabványos Azure SQL Database ellenőrző korlátozásokat és hello ISJSON függvény használatával:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Ha a bemeneti szöveg hello megfelelően van formázva JSON, a hello ISJSON függvény 1 hello értékét adja vissza. Minden Beszúrás vagy frissítés JSON oszlop ennél a határértéknél fog ellenőrizze, hogy új szöveges érték nem nem megfelelően formázott JSON-NÁ.

## <a name="transforming-json-into-tabular-format"></a>A táblázatos formátumra JSON átalakítása
Az Azure SQL Database emellett lehetővé teszi a JSON-gyűjtemények adatokká táblázatos formátumban és a load vagy a lekérdezés JSON átalakító.

OPENJSON elemzi a JSON-szöveg, megkeresi a JSON-objektumok tömbje, hello tömb elemei hello telepítéseket és egy olyan sor visszaadása hello kimeneti eredménye hello tömb egyes elemei a táblázat értékű függvényből.

![A táblázatos JSON](./media/sql-database-json-features/image_2.png)

Hello a fenti példában a adható meg ahol toolocate hello JSON-tömb, amelyeket meg kell nyitni (a hello $. Rendelések elérési út), mely oszlopok vissza kell adni az eredményként, és ha toofind hello visszaadott értékek JSON regisztrációja, mivel a cellák.

Azt alakíthatja át a JSON-tömb a hello @orders sorokra, a változó eredménykészlet elemzése, vagy sorok beillesztéséhez egy szabványos táblázatot:

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
hello gyűjtemény rendelések formázott JSON-tömb, és a megadott paraméter toohello tárolt eljárás elemzése, és hello rendelések táblába.

## <a name="next-steps"></a>Következő lépések
toolearn hogyan toointegrate JSON az alkalmazásba, tekintse meg ezeket az erőforrásokat:

* [TechNet Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [Az MSDN dokumentációját](https://msdn.microsoft.com/library/dn921897.aspx)
* [A Channel 9 videót](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

a JSON integrálása az alkalmazás különböző forgatókönyvekkel kapcsolatos toolearn lásd: hello bemutatók ezen [Channel 9 videót](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) vagy olyan forgatókönyvekben, amelyek a használati eset a megfelelő található [JSON blogbejegyzéseket](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/).

