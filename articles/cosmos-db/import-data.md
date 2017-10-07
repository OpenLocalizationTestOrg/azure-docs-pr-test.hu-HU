---
title: "az Azure Cosmos DB aaaDatabase áttelepítési eszköz |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello nyissa meg a forrás Azure Cosmos DB adatok áttelepítési eszközök tooimport adatok tooAzure Cosmos DB különböző forrásokból, beleértve a MongoDB, SQL Server, Table storage, Amazon DynamoDB, CSV és JSON fájlokat. Fürt megosztott kötetei szolgáltatás tooJSON átalakítás."
keywords: "fürt megosztott kötetei szolgáltatás toojson, adatbázis-áttelepítési eszközök, alakítsa át a fürt megosztott kötetei szolgáltatás toojson"
services: cosmos-db
author: andrewhoh
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: d173581d-782a-445c-98d9-5e3c49b00e25
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 997648a31602d854db75bb6ce4e2ecff36fc1069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimport-data-into-azure-cosmos-db-for-hello-documentdb-api"></a>Hogyan tooimport adatok Azure Cosmos DB hello DocumentDB API?

Ez az oktatóanyag biztosít használatával hello Azure Cosmos DB: DocumentDB API adatáttelepítés eszközt, amely a különböző forrásokból, beleértve a JSON-fájlokat importálhatja az adatok CSV-fájlok, SQL, a MongoDB, az Azure Table storage, Amazon DynamoDB és Azure Cosmos DB DocumentDB API-gyűjtemények a gyűjteményekbe használható az Azure Cosmos DB és hello DocumentDB API. hello adatáttelepítési eszközét is használható, a DocumentDB API hello egypartíciós gyűjtemény tooa több partíció gyűjteményét áttelepítésekor.

hello adatáttelepítési eszköz csak akkor működik, amikor Azure Cosmos DB importáló adatimportáláshoz hello DocumentDB API használata. A hello tábla API és a Graph API adatok importálása jelenleg nem támogatott. 

tooimport adatok hello MongoDB API-val való használatra, lásd: [Azure Cosmos DB: hogyan toomigrate adatainak hello MongoDB API?](mongodb-migrate.md).

Ez az oktatóanyag ismerteti a következő feladatok hello:

> [!div class="checklist"]
> * Hello adatáttelepítési eszköz telepítése
> * Különböző forrásokból származó adatok importálása
> * Azure Cosmos DB tooJSON exportálása

## <a id="Prerequisites"></a>Előfeltételek
Hello jelen cikkben lévő utasítások követése, előtt győződjön meg arról, hogy hello következőkkel:

* [A Microsoft .NET-keretrendszer 4.51](https://www.microsoft.com/download/developer-tools.aspx) vagy újabb verzióját.

## <a id="Overviewl"></a>Hello adatáttelepítési eszköz áttekintése
hello adatáttelepítési eszközét egy nyílt forráskódú megoldás, amellyel különféle adatok tooAzure Cosmos DB móddal, többek között számos különböző:

* JSON-fájlok
* MongoDB
* SQL Server
* CSV-fájlok
* Azure Table Storage
* Amazon DynamoDB
* HBase
* Az Azure Cosmos DB gyűjtemények

Amíg hello import eszközt tartalmaz egy grafikus felhasználói felületen (dtui.exe), azt is is vezeti hello parancssorból (dt.exe). Valójában van egy beállítás toooutput tartozó hello parancs hello felhasználói felületén keresztül importálás beállítása után. Táblázatos forrásadatok (pl. az SQL Server vagy CSV-fájlokban) is kell alakítani, úgy, hogy az importálás során hozható létre a is a hierarchikus kapcsolat (aldokumentumok). További információk az adatforrás beállításai minta parancssor tooimport minden forrás toolearn olvasása megőrzéséhez a cél lehetőségekkel, valamint a Megtekintés eredmények importálása.

## <a id="Install"></a>Hello adatáttelepítési eszköz telepítése
hello áttelepítési eszköz a forráskód nem elérhető a Githubon található [ebben a tárházban](https://github.com/azure/azure-documentdb-datamigrationtool) és egy lefordított elérhető a [Microsoft Download Center](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d). Akkor lehet, hogy vagy hello megoldás összeállításához vagy egyszerűen letöltéséhez és kibontásához hello összeállított tooa könyvtár az Ön által választott. Ezután futtassa vagy:

* **Dtui.exe**: hello eszköz grafikus felület verziója
* **DT.exe**: hello eszköz parancssori verziója

## <a name="import-data"></a>Adatok importálása

Hello eszköz telepítését követően idő tooimport az adatok. Milyen adatok szeretné, hogy tooimport?

* [JSON-fájlok](#JSON)
* [MongoDB](#MongoDB)
* [MongoDB exportfájlok](#MongoDBExport)
* [SQL Server](#SQL)
* [CSV-fájlok](#CSV)
* [Azure Table storage](#AzureTableSource)
* [Amazon DynamoDB](#DynamoDBSource)
* [A BLOB](#BlobImport)
* [Az Azure Cosmos DB gyűjtemények](#DocumentDBSource)
* [HBase](#HBaseSource)
* [Az Azure Cosmos DB tömeges importálással](#DocumentDBBulkImport)
* [Az Azure Cosmos DB szekvenciális rekord importálása](#DocumentDSeqTarget)


## <a id="JSON"></a>tooimport JSON-fájlok
hello JSON fájl forrás importáló beállítás lehetővé teszi tooimport egy vagy több egyetlen dokumentum JSON-fájlokat vagy JSON-fájlokat, hogy minden egyes tartalmazza JSON-dokumentumok tömbjét. JSON-fájlok tooimport tartalmazó mappák hozzáadásakor lehetősége van hello a rekurzív módon almappákban lévő fájlok keresése.

![Képernyőfelvétel a JSON fájl forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/jsonsource.png)

Az alábbiakban néhány parancssor minták tooimport JSON-fájlok:

    #Import a single JSON file
    dt.exe /s:JsonFile /s.Files:.\Sessions.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory of JSON files
    dt.exe /s:JsonFile /s.Files:C:\TESessions\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Sessions /t.CollectionThroughput:2500

    #Import a directory (including sub-directories) of JSON files
    dt.exe /s:JsonFile /s.Files:C:\LastFMMusic\**\*.json /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Music /t.CollectionThroughput:2500

    #Import a directory (single), directory (recursive), and individual JSON files
    dt.exe /s:JsonFile /s.Files:C:\Tweets\*.*;C:\LargeDocs\**\*.*;C:\TESessions\Session48172.json;C:\TESessions\Session48173.json;C:\TESessions\Session48174.json;C:\TESessions\Session48175.json;C:\TESessions\Session48177.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:subs /t.CollectionThroughput:2500

    #Import a single JSON file and partition hello data across 4 collections
    dt.exe /s:JsonFile /s.Files:D:\\CompanyData\\Companies.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:comp[1-4] /t.PartitionKey:name /t.CollectionThroughput:2500

## <a id="MongoDB"></a>a MongoDB tooimport

> [!IMPORTANT]
> Ha tooan Azure Cosmos DB fiók mongodb-Protokolltámogatással rendelkező importál, kövesse az alábbi [utasításokat](mongodb-migrate.md).
> 
> 

hello MongoDB forrás importáló beállítás lehetővé teszi az egyes MongoDB gyűjteményből tooimport és opcionálisan szűrése lekérdezéssel dokumentumok és/vagy módosításához hello dokumentumstruktúrával leképezés használatával.  

![Képernyőfelvétel a MongoDB forrására vonatkozó beállítások](./media/import-data/mongodbsource.png)

hello kapcsolati karakterlánc hello szabványos MongoDB formátumban kell megadni:

    mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database>

> [!NOTE]
> Használjon hello ellenőrizze parancs tooensure, amelyek a MongoDB-példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

Adja meg, amelyből adatok importálódnak hello gyűjtemény hello nevét. Szükség lehet, hogy adja meg, vagy adjon meg egy fájlt egy lekérdezés (pl. {pop: {$gt: 5000}}) és/vagy a leképezése (például {loc:0}) tooboth szűrő és alakzat hello adatok toobe importálva.

Az alábbiakban néhány parancssor minták tooimport a MongoDB:

    #Import all documents from a MongoDB collection
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZips /t.IdField:_id /t.CollectionThroughput:2500

    #Import documents from a MongoDB collection which match hello query and exclude hello loc field
    dt.exe /s:MongoDB /s.ConnectionString:mongodb://<dbuser>:<dbpassword>@<host>:<port>/<database> /s.Collection:zips /s.Query:{pop:{$gt:50000}} /s.Projection:{loc:0} /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:BulkZipsTransform /t.IdField:_id/t.CollectionThroughput:2500

## <a id="MongoDBExport"></a>tooimport MongoDB exportfájlok

> [!IMPORTANT]
> Ha tooan Azure Cosmos DB fiók mongodb-protokolltámogatással rendelkező importál, kövesse az alábbi [utasításokat](mongodb-migrate.md).
> 
> 

hello MongoDB exportálási JSON fájl forrás importáló beállítás lehetővé teszi egy tooimport, vagy további JSON-fájlok előállított hello mongoexport segédprogram.  

![Képernyőfelvétel a MongoDB exportálási forrására vonatkozó beállítások](./media/import-data/mongodbexportsource.png)

MongoDB exportálási JSON-fájlok importálása tartalmazó mappák hozzáadásakor lehetősége van hello a rekurzív módon almappákban lévő fájlok keresése.

Ez a parancssor minta tooimport a MongoDB-exportálás JSON-fájlok:

    dt.exe /s:MongoDBExport /s.Files:D:\mongoemployees.json /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:employees /t.IdField:_id /t.Dates:Epoch /t.CollectionThroughput:2500

## <a id="SQL"></a>az SQL Server tooimport
hello SQL importáló forrásbeállítás lehetővé teszi tooimport egy egyéni SQL Server-adatbázisból, és opcionálisan szűrése hello rekordok toobe importált lekérdezés segítségével. Emellett módosíthatja hello dokumentumstruktúrával (további sablonokat, amelyek később) beágyazási elválasztó megadásával.  

![Képernyőkép az SQL - adatbázis áttelepítési eszközök forrása](./media/import-data/sqlexportsource.png)

hello hello kapcsolati karakterlánc formátuma hello szabványos SQL kapcsolati karakterlánc-formátum.

> [!NOTE]
> Használjon hello ellenőrizze parancs tooensure, amely hello hello kapcsolati karakterlánc mezőben megadott SQL Server-példány segítségével érhető el.
> 
> 

elválasztó tulajdonság beágyazási hello importálása során használt toocreate hierarchikus kapcsolat (alárendelt dokumentumok). Vegye figyelembe a következő SQL-lekérdezés hello:

*Válassza ki a TÍPUSKONVERZIÓ (BusinessEntityID AS varchar) azonosítója, neve, mint [Address.AddressType] AddressType, AddressLine1 [Address.AddressLine1], [Address.Location.City] városát, StateProvinceName [Address.Location.StateProvinceName], irányítószám szerint [Address.PostalCode], CountryRegionName [Address.CountryRegionName], a Sales.vStoreWithAddresses ahol AddressType = "Main Office"*

A következő (részleges) eredmények hello visszaadó:

![Képernyőkép az SQL-lekérdezés eredményei](./media/import-data/sqlqueryresults.png)

Vegye figyelembe például Address.AddressType és Address.Location.StateProvinceName hello aliasok. A beágyazási elválasztó megadásával '.', hello import eszközt cím hoz létre, és Address.Location aldokumentumok során hello importálása. Íme egy példa az Azure Cosmos Adatbázisba az eredményül kapott dokumentum:

*{"id": "956", "Name": "Egyeztetését értékesítés és a szolgáltatás", "Cím": {"AddressType": "Main Office", "AddressLine1": "#500-75 O'Connor utca", "Hely": {"Város": "Ottawai", "StateProvinceName": "Ontario"}, "Irányítószám": "K4B 1S2", "CountryRegionName": "Kanada"}}*

Az alábbiakban néhány parancssor minták tooimport az SQL-kiszolgálóról:

    #Import records from SQL which match a query
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, * from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Stores /t.IdField:Id /t.CollectionThroughput:2500

    #Import records from sql which match a query and create hierarchical relationships
    dt.exe /s:SQL /s.ConnectionString:"Data Source=<server>;Initial Catalog=AdventureWorks;User Id=advworks;Password=<password>;" /s.Query:"select CAST(BusinessEntityID AS varchar) as Id, Name, AddressType as [Address.AddressType], AddressLine1 as [Address.AddressLine1], City as [Address.Location.City], StateProvinceName as [Address.Location.StateProvinceName], PostalCode as [Address.PostalCode], CountryRegionName as [Address.CountryRegionName] from Sales.vStoreWithAddresses WHERE AddressType='Main Office'" /s.NestingSeparator:. /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:StoresSub /t.IdField:Id /t.CollectionThroughput:2500

## <a id="CSV"></a>tooimport CSV-fájlok és a konvertálás CSV tooJSON
hello CSV fájl forrásbeállítás importáló lehetővé teszi, hogy Ön tooimport egy vagy több CSV-fájlt. Az Importálás CSV-fájlokat tartalmazó mappák hozzáadásakor lehetősége van hello a rekurzív módon almappákban lévő fájlok keresése.

![Képernyőfelvétel a CSV-forrására vonatkozó beállítások - CSV tooJSON](media/import-data/csvsource.png)

Hasonló toohello SQL-forrás, beágyazási elválasztó tulajdonság hello lehet használt toocreate hierarchikus kapcsolat (alárendelt dokumentumok) importálása során. Vegye figyelembe a következő CSV-fejléc és az adatsorok hello:

![Képernyőfelvétel a fürt megosztott kötetei szolgáltatás minta rekordok - CSV tooJSON](./media/import-data/csvsample.png)

Vegye figyelembe például DomainInfo.Domain_Name és RedirectInfo.Redirecting hello aliasok. A beágyazási elválasztó megadásával '.', hello import eszközt DomainInfo hoz létre, és közben hello RedirectInfo aldokumentumok importálása. Íme egy példa az Azure Cosmos Adatbázisba az eredményül kapott dokumentum:

*{"DomainInfo": {"Tartománynév": "ACUS.GOV", "Domain_Name_Address": "http://www. ACUS.GOV"},"Szövetségi ügynökség":"Az Amerikai Egyesült Államokban hello felügyeleti konferencia","RedirectInfo": {"Átirányítása":"0","Redirect_Destination":" "},"id":"9cc565c5-ebcd-1c03-ebd3-cc3e2ecd814d"}*

hello import eszközt megpróbál tooinfer információja nem jegyzett értékek CSV-fájlok (idézőjelek közé zárt értékek mindig tekintendők karakterláncok).  Típusok sorrend hello azonosítja: szám, dátum és idő, logikai érték.  

Két más dolgokat toonote kapcsolatos CSV import van:

1. Alapértelmezés szerint nem jegyzett értékek mindig elrejti a program lapok és szóközt tartalmaz, idézőjelek közé zárt értékek őrződnek meg amíg-van. Ez a viselkedés felülbírálható a hello vágást idézőjelek között értékek jelölőnégyzet vagy hello /s.TrimQuoted parancssori kapcsolóval együtt.
2. Alapértelmezés szerint egy nem jegyzett null értéke null értékű. Ez a viselkedés felülbírálható (azaz tekinti egy nem jegyzett null "null" karakterlánc) rendelkező hello Treat utasítás nem null értékű karakterlánc jelölőnégyzet vagy hello /s.NoUnquotedNulls parancssori kapcsolóval, jegyzett.

A CSV-importálási parancssori minta a következő:

    dt.exe /s:CsvFile /s.Files:.\Employees.csv /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:Employees /t.IdField:EntityID /t.CollectionThroughput:2500

## <a id="AzureTableSource"></a>az Azure Table storage tooimport
hello Azure Table storage forrásbeállítás importáló lehetővé teszi az egyes Azure Table storage táblából tooimport, és opcionálisan a hello tábla entitások toobe importált szűréséhez. Vegye figyelembe, hogy nem használható hello adatáttelepítési eszköz tooimport Azure Table storage adatok az Azure Cosmos DB hello tábla API való használatra. Most támogatja az csak importálását tooAzure Cosmos DB hello DocumentDB API való használatra.

![Képernyőfelvétel az Azure Table storage forrására vonatkozó beállítások](./media/import-data/azuretablesource.png)

hello hello Azure Table storage kapcsolati karakterlánc formátuma:

    DefaultEndpointsProtocol=<protocol>;AccountName=<Account Name>;AccountKey=<Account Key>;

> [!NOTE]
> Használjon hello ellenőrizze parancs tooensure, amely az Azure Table storage példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

Adja meg hello Azure hello nevét, amelyből adatok importálódnak tábla. Opcionálisan megadhat egy [szűrő](https://msdn.microsoft.com/library/azure/ff683669.aspx).

hello Azure Table storage forrásbeállítás importáló alábbi további beállítások hello rendelkezik:

1. Belső mezők szerepelhetnek
   1. Az összes - mezők szerepelhetnek, az összes belső (PartitionKey, RowKey, vagy időbélyeg)
   2. Nincs – összes belső mező kizárása
   3. RowKey - csak a hello RowKey mező is
2. Oszlopok kiválasztása
   1. Az Azure Table storage szűrők nem támogatják a leképezések. Ha azt szeretné, hogy tooonly importálása adott Azure Table entitás tulajdonságai, vegye fel őket a toohello Select Columns listája. Minden más entitás tulajdonságait a rendszer figyelmen kívül hagyja.

Az Azure Table storage egy parancssor minta tooimport itt található:

    dt.exe /s:AzureTable /s.ConnectionString:"DefaultEndpointsProtocol=https;AccountName=<Account Name>;AccountKey=<Account Key>" /s.Table:metrics /s.InternalFields:All /s.Filter:"PartitionKey eq 'Partition1' and RowKey gt '00001'" /s.Projection:ObjectCount;ObjectSize  /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:metrics /t.CollectionThroughput:2500

## <a id="DynamoDBSource"></a>az Amazon DynamoDB tooimport
hello Amazon DynamoDB importáló forrásbeállítás lehetővé teszi az egyes Amazon DynamoDB táblából tooimport, és opcionálisan a hello entitások toobe importált szűréséhez. Több sablonok találhatók, így a lehető legkönnyebben állítja be az importálás nem.

![Képernyőkép az Amazon DynamoDB forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/dynamodbsource1.png)

![Képernyőkép az Amazon DynamoDB forrására vonatkozó beállítások – adatbázis-áttelepítési eszközök](./media/import-data/dynamodbsource2.png)

hello hello Amazon DynamoDB kapcsolati karakterlánc formátuma:

    ServiceURL=<Service Address>;AccessKey=<Access Key>;SecretKey=<Secret Key>;

> [!NOTE]
> Használjon hello ellenőrizze parancs tooensure, Amazon DynamoDB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

Íme egy parancssor minta tooimport az Amazon DynamoDB:

    dt.exe /s:DynamoDB /s.ConnectionString:ServiceURL=https://dynamodb.us-east-1.amazonaws.com;AccessKey=<accessKey>;SecretKey=<secretKey> /s.Request:"{   """TableName""": """ProductCatalog""" }" /t:DocumentDBBulk /t.ConnectionString:"AccountEndpoint=<Azure Cosmos DB Endpoint>;AccountKey=<Azure Cosmos DB Key>;Database=<Azure Cosmos DB Database>;" /t.Collection:catalogCollection /t.CollectionThroughput:2500

## <a id="BlobImport"></a>az Azure Blob storage tooimport fájlok
hello JSON-fájl, a MongoDB exportfájl és a fürt megosztott kötetei szolgáltatás fájl forrás importáló beállítások lehetővé teszik tooimport az Azure Blob storage egy vagy több fájlt. Adja meg a Blob-tároló URL-cím és a Fiókkulcsot, adjon meg egy reguláris kifejezést tooselect hello (oka) t tooimport.

![Képernyőfelvétel a Blob fájl forrására vonatkozó beállítások](./media/import-data/blobsource.png)

Parancssori minta tooimport JSON-fájlokat az Azure Blob storage a következő:

    dt.exe /s:JsonFile /s.Files:"blobs://<account key>@account.blob.core.windows.net:443/importcontainer/.*" /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:doctest

## <a id="DocumentDBSource"></a>egy Azure Cosmos DB DocumentDB API gyűjteményből tooimport
hello Azure Cosmos DB adatforrás-importáló beállítást lehetővé teszi egy vagy több Azure Cosmos DB gyűjtemények tooimport adatait, és opcionálisan a dokumentumok lekérdezéssel szűréséhez.  

![Képernyőfelvétel az Azure Cosmos DB adatforrás-beállítások](./media/import-data/documentdbsource.png)

hello hello Azure Cosmos DB kapcsolati karakterlánc formátuma:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

hello Azure Cosmos DB fiók kapcsolati karakterlánc olvasható be a hello (kulcsok) panelén hello Azure-portálon a [hogyan toomanage Azure Cosmos DB fiók](manage-account.md), azonban meg kell fűzött toobe toohello hello adatbázis hello nevét kapcsolati karakterlánc formátuma a következő hello:

    Database=<CosmosDB Database>;

> [!NOTE]
> Használja hello ellenőrizze parancs tooensure, amely Azure Cosmos DB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

egyetlen Azure Cosmos DB gyűjteményből tooimport adja meg, amelyből adatok importálódnak hello gyűjtemény hello nevét. a több Azure Cosmos DB ügyfélgyűjteményekből tooimport adja meg a reguláris kifejezés toomatch egy vagy több gyűjtemény neve (pl. collection01 |} collection02 |} collection03). Ön nem kötelezően adja meg, vagy adjon meg egy fájlt, egy lekérdezés tooboth szűrő és hello adatok toobe importált alakul.

> [!NOTE]
> Mivel a hello gyűjtemény mezőben reguláris kifejezések fogad el, ha importál egy egyetlen gyűjteményből, amelynek a neve reguláris kifejezés karaktereket tartalmaz, majd ezeket a karaktereket kell megjelölni ennek megfelelően.
> 
> 

hello Azure Cosmos DB adatforrás-importáló beállítást rendelkezik hello következő speciális beállítások:

1. Belső mezők szerepelhetnek: Határozza meg, függetlenül attól, tooinclude Azure Cosmos DB rendszer dokumentumtulajdonságok hello az Exportálás (pl. _rid, _ts).
2. Hiba újbóli próbálkozások számát: tooretry hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hiba esetén hányszor hello számát határozza meg.
3. Újrapróbálkozási időköz: Megadja, mennyi ideig toowait közötti újrapróbálkozás hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén.
4. Csatlakozási mód: Hello csatlakozási mód toouse rendelkező Azure Cosmos DB határozza meg. hello elérhető lehetőségek a következők: DirectTcp, DirectHttps és átjáró. hello közvetlen csatlakozási mód gyorsabb, addig, amíg hello átjáró módja rövid több tűzfal, mert csak 443-as portot.

![Képernyőfelvétel az Azure Cosmos DB adatforrás speciális beállítások](./media/import-data/documentdbsourceoptions.png)

> [!TIP]
> hello eszköz alapértelmezett tooconnection mód DirectTcp importálni. Ha tűzfal problémák, váltson tooconnection mód átjáró, csak 443-as portra van szüksége.
> 
> 

Az alábbiakban néhány parancssor minták tooimport az Azure Cosmos Adatbázisból:

    #Migrate data from one Azure Cosmos DB collection tooanother Azure Cosmos DB collections
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:TEColl /t:CosmosDBBulk /t.ConnectionString:" AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:TESessions /t.CollectionThroughput:2500

    #Migrate data from multiple Azure Cosmos DB collections tooa single Azure Cosmos DB collection
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:comp1|comp2|comp3|comp4 /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:singleCollection /t.CollectionThroughput:2500

    #Export an Azure Cosmos DB collection tooa JSON file
    dt.exe /s:CosmosDB /s.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /s.Collection:StoresSub /t:JsonFile /t.File:StoresExport.json /t.Overwrite /t.CollectionThroughput:2500

> [!TIP]
> hello Azure Cosmos DB adatok Import eszközt is támogat hello származó adatok importálása [Azure Cosmos DB emulátor](local-emulator.md). Amikor adatokat importál egy helyi emulátor, állítsa be az hello végpont túl`https://localhost:<port>`. 
> 
> 

## <a id="HBaseSource"></a>a HBase tooimport
hello HBase importáló forrásbeállítás lehetővé teszi egy HBase tábla tooimport adatait, és opcionálisan szűrje hello adatokat. Több sablonok találhatók, így a lehető legkönnyebben állítja be az importálás nem.

![Képernyőfelvétel a HBase forrására vonatkozó beállítások](./media/import-data/hbasesource1.png)

![Képernyőfelvétel a HBase forrására vonatkozó beállítások](./media/import-data/hbasesource2.png)

hello hello HBase Stargate kapcsolati karakterlánc formátuma:

    ServiceURL=<server-address>;Username=<username>;Password=<password>

> [!NOTE]
> Használjon hello ellenőrizze parancs tooensure, amely a HBase példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

A HBase egy parancssor minta tooimport itt található:

    dt.exe /s:HBase /s.ConnectionString:ServiceURL=<server-address>;Username=<username>;Password=<password> /s.Table:Contacts /t:CosmosDBBulk /t.ConnectionString:"AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;" /t.Collection:hbaseimport

## <a id="DocumentDBBulkTarget"></a>tooimport toohello DocumentDB API (tömeges importálással)
hello Azure Cosmos DB tömeges importáló lehetővé teszi a tooimport bármelyik hello elérhető forrására vonatkozó beállítások, egy Azure Cosmos DB tárolt eljárás használatával hatékonyságot biztosít. hello eszköz támogatja a tooone egy particionált Azure Cosmos DB importgyűjteményhez, valamint a szilánkos importálása, amelynek során az adatok több egy particionált Azure Cosmos DB gyűjtemények között particionált van. Adatok partícionálásra vonatkozó további információkért lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md). hello eszköz létrehozása, hajtható végre, és törölje a hello cél következő gyűjtemény(ek) készleteit szinkronizálja hello tárolt eljárás.  

![Képernyőfelvétel az Azure Cosmos DB tömeges beállítások](./media/import-data/documentdbbulk.png)

hello hello Azure Cosmos DB kapcsolati karakterlánc formátuma:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

hello Azure Cosmos DB fiók kapcsolati karakterlánc olvasható be a hello (kulcsok) panelén hello Azure-portálon a [hogyan toomanage Azure Cosmos DB fiók](manage-account.md), azonban meg kell fűzött toobe toohello hello adatbázis hello nevét kapcsolati karakterlánc formátuma a következő hello:

    Database=<CosmosDB Database>;

> [!NOTE]
> Használja hello ellenőrizze parancs tooensure, amely Azure Cosmos DB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

tooimport tooa egyetlen gyűjtemény, adjon meg hello hello gyűjtemény toowhich adatok Importálja, majd kattintson hello Hozzáadás gombra. tooimport toomultiple gyűjtemények, adjon meg külön-külön minden gyűjtemény nevét, vagy használja a következő szintaxist toospecify hello több gyűjteményt: *collection_prefix*[kezdő index - end index]. Hello fent említett szintaxis használatával több gyűjtemény meghatározásakor vegye figyelembe hello a következőket:

1. Csak az egész tartomány neve minták támogatottak. Például adja meg a gyűjtemény [0 – 3] eredményez a következő gyűjteményeket hello: collection0, collection1, collection2, collection3.
2. Használhat egy rövidített Szintaxis: gyűjtemény [3] fog kibocsátás ugyanazokat a gyűjtemények az 1. lépésben említett.
3. Egynél több helyettesítési megadható. Például [0-1] [0-9] gyűjteményt hoz létre a 20 gyűjteménynevek vezető nullák (collection01,... 02... (03).

Miután hello gyűjtemény neve van megadva, válassza ki a kívánt átviteli hello hello következő gyűjtemény(ek) készleteit szinkronizálja (400 RUs too10, 000 RUs). Importálás legjobb teljesítmény érdekében válasszon egy nagyobb átviteli sebesség. Teljesítmény szintekkel kapcsolatos további információkért lásd: [teljesítményszintek az Azure Cosmos Adatbázisba](performance-levels.md).

> [!NOTE]
> hello teljesítmény átviteli beállítás csak akkor érvényes toocollection létrehozása. Hello megadott gyűjtemény már létezik, az átviteli sebesség nem módosítható.
> 
> 

Toomultiple gyűjtemények importálásakor hello import eszközt a kivonat-alapú horizontális támogatja. Ebben az esetben adja meg, partíciós kulcs hello toouse kívánja hello dokumentumtulajdonság (Ha üresen marad partíciós kulcs, dokumentumok lesz szilánkos véletlenszerűen hello cél gyűjtemények között).

Opcionálisan megadhat melyik hello importálási forrás mezője hello importálás (vegye figyelembe, hogy ha dokumentumok nem tartalmazzák ezt a tulajdonságot, majd hello importálási eszköz létrehoz egy GUID hello azonosító tulajdonság értékeként) alatt kell használható hello Azure Cosmos DB dokumentum id tulajdonságának.

Nincsenek speciális beállítások számos elérhető importálása során. Először amíg hello eszköz tartalmazza a alapértelmezett tömeges importálásához tárolt eljárás (BulkInsert.js), illetve előfordulhat, hogy toospecify saját importálási tárolt eljárás:

 ![Képernyőfelvétel az Azure Cosmos DB tömeges beszúrási sproc beállítást](./media/import-data/bulkinsertsp.png)

Emellett dátum típusú (pl. az SQL Server vagy a MongoDB) importálásakor választhat három importálási beállításokat:

 ![Képernyőfelvétel az Azure Cosmos DB dátumbeállításainak idő importálása](./media/import-data/datetimeoptions.png)

* Karakterlánc: Egy karakterláncértéket áll fenn
* Epoch: Egy érték szám Epoch áll fenn
* Mindkét: Továbbra is fennáll, karakterlánc és a Epoch számértékeit. Ez a beállítás létrehoz aldokumentum, például: "date_joined": {"Érték": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}

hello Azure Cosmos DB tömeges importáló rendelkezik hello következő további speciális beállítások:

1. Köteg mérete: hello eszköz alapértelmezett tooa köteg mérete 50.  Ha hello dokumentumok toobe importált nagy, fontolja meg hello a köteg méretének csökkentésével. Ezzel szemben ha importált hello dokumentumok toobe kicsi, fontolja meg hello a köteg méretének emelés.
2. Max. parancsfájl mérete (bájt): hello eszköz alapértelmezés szerint használt érték tooa maximális parancsfájl 512 KB-os méret
3. Tiltsa le automatikus azonosító létrehozása: Ha minden importált dokumentum toobe azonosító mezőjéhez tartalmaz, majd ezzel a beállítással növelheti a teljesítményt. Hiányzik egy egyedi azonosító mező dokumentumok lesz importálva.
4. Frissítés meglévő dokumentumok: hello eszköz alapértelmezett toonot meglévő dokumentumok cseréje ütközését. Ezzel a beállítással lehetővé teszi az egyező azonosítók felülírja a meglévő dokumentumokat. A szolgáltatás akkor hasznos, amelyek frissítik a dokumentumok meglévő ütemezett áttelepítésre.
5. Hiba újbóli próbálkozások számát: tooretry hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hiba esetén hányszor hello számát határozza meg.
6. Újrapróbálkozási időköz: Megadja, mennyi ideig toowait közötti újrapróbálkozás hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén.
7. Csatlakozási mód: Hello csatlakozási mód toouse rendelkező Azure Cosmos DB határozza meg. hello elérhető lehetőségek a következők: DirectTcp, DirectHttps és átjáró. hello közvetlen csatlakozási mód gyorsabb, addig, amíg hello átjáró módja rövid több tűzfal, mert csak 443-as portot.

![Képernyőfelvétel az Azure Cosmos DB tömeges importálással speciális beállítások](./media/import-data/docdbbulkoptions.png)

> [!TIP]
> hello eszköz alapértelmezett tooconnection mód DirectTcp importálni. Ha tűzfal problémák, váltson tooconnection mód átjáró, csak 443-as portra van szüksége.
> 
> 

## <a id="DocumentDBSeqTarget"></a>tooimport toohello DocumentDB API (egymást követő rekord importálása)
hello Azure Cosmos DB szekvenciális rekord importáló lehetővé teszi a hello az elérhető forráskiszolgálók beállításokat rekord által rekord alapon tooimport. Akkor célszerű használni ezt a beállítást, ha importál tooan meglévő gyűjteményt, amely elérte a tárolt eljárások vonatkozó kvótáját. hello eszköz támogatja importálási tooa egyetlen Azure Cosmos DB gyűjtemény (egypartíciós és több partíció is), valamint a szilánkos importálása, amelynek során az adatok több egypartíciós és/vagy több partíció Azure Cosmos DB gyűjtemények között particionált van. Adatok partícionálásra vonatkozó további információkért lásd: [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).

![Képernyőfelvétel az Azure Cosmos DB szekvenciális rekord importálási beállítások](./media/import-data/documentdbsequential.png)

hello hello Azure Cosmos DB kapcsolati karakterlánc formátuma:

    AccountEndpoint=<CosmosDB Endpoint>;AccountKey=<CosmosDB Key>;Database=<CosmosDB Database>;

hello Azure Cosmos DB fiók kapcsolati karakterlánc olvasható be a hello (kulcsok) panelén hello Azure-portálon a [hogyan toomanage Azure Cosmos DB fiók](manage-account.md), azonban meg kell fűzött toobe toohello hello adatbázis hello nevét kapcsolati karakterlánc formátuma a következő hello:

    Database=<Azure Cosmos DB Database>;

> [!NOTE]
> Használja hello ellenőrizze parancs tooensure, amely Azure Cosmos DB példány hello kapcsolati karakterlánc mezőben megadott hello segítségével érhető el.
> 
> 

tooimport tooa egyetlen gyűjtemény, adjon meg hello hello gyűjtemény toowhich adatok Importálja, majd kattintson hello Hozzáadás gombra. tooimport toomultiple gyűjtemények, adjon meg külön-külön minden gyűjtemény nevét, vagy használja a következő szintaxist toospecify hello több gyűjteményt: *collection_prefix*[kezdő index - end index]. Hello fent említett szintaxis használatával több gyűjtemény meghatározásakor vegye figyelembe hello a következőket:

1. Csak az egész tartomány neve minták támogatottak. Például adja meg a gyűjtemény [0 – 3] eredményez a következő gyűjteményeket hello: collection0, collection1, collection2, collection3.
2. Használhat egy rövidített Szintaxis: gyűjtemény [3] fog kibocsátás ugyanazokat a gyűjtemények az 1. lépésben említett.
3. Egynél több helyettesítési megadható. Például [0-1] [0-9] gyűjteményt hoz létre a 20 gyűjteménynevek vezető nullák (collection01,... 02... (03).

Miután hello gyűjtemény neve van megadva, válassza ki a kívánt átviteli hello hello következő gyűjtemény(ek) készleteit szinkronizálja (400 RUs too250, 000 RUs). Importálás legjobb teljesítmény érdekében válasszon egy nagyobb átviteli sebesség. Teljesítmény szintekkel kapcsolatos további információkért lásd: [teljesítményszintek az Azure Cosmos Adatbázisba](performance-levels.md). Bármely importálása átviteli toocollections > 10 000 RUs partíciós kulcs szükséges. Ha úgy dönt, toohave több mint 250 000 RUs, szüksége lesz a kérést hello portál toohave fiókja nagyobb toofile.

> [!NOTE]
> hello átviteli beállítás csak akkor érvényes toocollection létrehozása. Hello megadott gyűjtemény már létezik, az átviteli sebesség nem módosítható.
> 
> 

Toomultiple gyűjtemények importálásakor hello import eszközt a kivonat-alapú horizontális támogatja. Ebben az esetben adja meg, partíciós kulcs hello toouse kívánja hello dokumentumtulajdonság (Ha üresen marad partíciós kulcs, dokumentumok lesz szilánkos véletlenszerűen hello cél gyűjtemények között).

Opcionálisan megadhat melyik hello importálási forrás mezője hello importálás (vegye figyelembe, hogy ha dokumentumok nem tartalmazzák ezt a tulajdonságot, majd hello importálási eszköz létrehoz egy GUID hello azonosító tulajdonság értékeként) alatt kell használható hello Azure Cosmos DB dokumentum id tulajdonságának.

Nincsenek speciális beállítások számos elérhető importálása során. Első lépésként dátum típusú (pl. az SQL Server vagy a MongoDB) importálásakor választhat három importálási beállításokat:

 ![Képernyőfelvétel az Azure Cosmos DB dátumbeállításainak idő importálása](./media/import-data/datetimeoptions.png)

* Karakterlánc: Egy karakterláncértéket áll fenn
* Epoch: Egy érték szám Epoch áll fenn
* Mindkét: Továbbra is fennáll, karakterlánc és a Epoch számértékeit. Ez a beállítás létrehoz aldokumentum, például: "date_joined": {"Érték": "2013-10-21T21:17:25.2410000Z", "Epoch": 1382390245}

hello Azure Cosmos DB - szekvenciális rekord importáló rendelkezik hello következő további speciális beállítások:

1. Párhuzamos kérelem: hello eszköz alapértelmezés szerint használt érték too2 párhuzamos kérelmeket. Ha hello dokumentumok toobe importált kicsi, fontolja meg hello száma párhuzamos kérelem kiváltása. Vegye figyelembe, hogy ez a szám túl sok következik be, ha hello importálási problémákat tapasztalhat a sávszélesség-szabályozás.
2. Tiltsa le automatikus azonosító létrehozása: Ha minden importált dokumentum toobe azonosító mezőjéhez tartalmaz, majd ezzel a beállítással növelheti a teljesítményt. Hiányzik egy egyedi azonosító mező dokumentumok lesz importálva.
3. Frissítés meglévő dokumentumok: hello eszköz alapértelmezett toonot meglévő dokumentumok cseréje ütközését. Ezzel a beállítással lehetővé teszi az egyező azonosítók felülírja a meglévő dokumentumokat. A szolgáltatás akkor hasznos, amelyek frissítik a dokumentumok meglévő ütemezett áttelepítésre.
4. Hiba újbóli próbálkozások számát: tooretry hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hiba esetén hányszor hello számát határozza meg.
5. Újrapróbálkozási időköz: Megadja, mennyi ideig toowait közötti újrapróbálkozás hello kapcsolat tooAzure Cosmos DB (pl.: hálózati kapcsolat megszakadása) átmeneti hibák esetén.
6. Csatlakozási mód: Hello csatlakozási mód toouse rendelkező Azure Cosmos DB határozza meg. hello elérhető lehetőségek a következők: DirectTcp, DirectHttps és átjáró. hello közvetlen csatlakozási mód gyorsabb, addig, amíg hello átjáró módja rövid több tűzfal, mert csak 443-as portot.

![Képernyőfelvétel az Azure Cosmos DB szekvenciális rekord importálása speciális beállítások](./media/import-data/documentdbsequentialoptions.png)

> [!TIP]
> hello eszköz alapértelmezett tooconnection mód DirectTcp importálni. Ha tűzfal problémák, váltson tooconnection mód átjáró, csak 443-as portra van szüksége.
> 
> 

## <a id="IndexingPolicy"></a>Adjon meg egy indexelési házirendet, amikor Azure Cosmos DB gyűjtemények létrehozása
Ha engedélyezi a hello áttelepítési eszköz toocreate gyűjtemények importálása során, az indexelési házirendet hello hello gyűjtemények is megadhat. Speciális beállítások szakasz hello Azure Cosmos DB tömeges importálással és Azure Cosmos DB szekvenciális rekord beállítások hello lépjen a toohello indexelő házirend szakaszban.

![Képernyőfelvétel az Azure Cosmos DB indexelő házirend Speciális beállítások](./media/import-data/indexingpolicy1.png)

Hello speciális indexelő házirend-beállítást használja, akkor is indexelési házirend fájlt válasszon ki, manuálisan adja meg az indexelési házirendet, vagy kattintva válassza ki az alapértelmezett sablonok közül (jobb gombbal a házirend szövegmező indexelő hello).

hello sablonok hello eszközt biztosít a következők:

* Alapértelmezés szerint. Ez a házirend akkor ajánlott, ha Ön karakterláncok egyenlőséglekérdezéséhez végrehajtása és a számok ORDER BY, tartomány és egyenlőség lekérdezések használatával. Ez a házirend rendelkezik egy alacsonyabb indextárolási terheléssel jár mint tartomány.
* Tartomány. Ezzel a házirend-érdemes a számok és karakterláncok ORDER BY, tartomány-és egyenlőséglekérdezéséhez használ. Ez a házirend egy magasabb indextárolási terheléssel jár, mint az alapértelmezett vagy az ujjlenyomat-rendelkezik.

![Képernyőfelvétel az Azure Cosmos DB indexelő házirend Speciális beállítások](./media/import-data/indexingpolicy2.png)

> [!NOTE]
> Ha nem adja meg az indexelési házirendet, majd hello alapértelmezett házirend lépnek érvénybe. Az indexelő házirendekkel kapcsolatos további információkért lásd: [Azure Cosmos DB házirendek indexelő](indexing-policies.md).
> 
> 

## <a name="export-toojson-file"></a>TooJSON fájl exportálása
hello Azure Cosmos DB JSON-exportáló tooexport lehetővé teszi bármely hello az elérhető forráskiszolgálók beállítások tooa JSON-fájl, amely tartalmazza a JSON-dokumentumok tömbjét. hello eszköz hello exportálási meg fogja kezelni, vagy választhat tooview hello eredményül kapott áttelepítési parancsot, és saját kezűleg hello parancs futtatása. hello letöltött JSON-fájlt helyben vagy az Azure Blob storage tárolhatók.

![Az Azure Cosmos DB JSON képernyőkép helyi fájl exportálási beállítás](./media/import-data/jsontarget.png)

![Képernyőkép Azure Cosmos DB JSON Azure Blob storage exportálási beállítás](./media/import-data/jsontarget2.png)

Úgy is dönthet, JSON, ami növeli a hello eredményül kapott dokumentum hello mérete közben végzett hello tartalom olvasható további emberi eredő tooprettify hello nem kötelező.

    Standard JSON export
    [{"id":"Sample","Title":"About Paris","Language":{"Name":"English"},"Author":{"Name":"Don","Location":{"City":"Paris","Country":"France"}},"Content":"Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.","PageViews":10000,"Topics":[{"Title":"History of Paris"},{"Title":"Places toosee in Paris"}]}]

    Prettified JSON export
    [
     {
    "id": "Sample",
    "Title": "About Paris",
    "Language": {
      "Name": "English"
    },
    "Author": {
      "Name": "Don",
      "Location": {
        "City": "Paris",
        "Country": "France"
      }
    },
    "Content": "Don's document in Azure Cosmos DB is a valid JSON document as defined by hello JSON spec.",
    "PageViews": 10000,
    "Topics": [
      {
        "Title": "History of Paris"
      },
      {
        "Title": "Places toosee in Paris"
      }
    ]
    }]

## <a name="advanced-configuration"></a>Speciális konfiguráció
Hello speciális konfigurálására szolgáló képernyőn adja meg a hello napló fájl toowhich írt hibák milyen hello elhelyezkedését. a következő szabályok hello toothis lap vonatkoznak:

1. Ha egy fájl neve nem áll rendelkezésre, majd a program az összes hiba visszatér hello eredmények lapon.
2. Ha a fájl nevét a címtár nélkül áll rendelkezésre, majd hello fájl létrejön (vagy felül) hello aktuális környezet könyvtárban található.
3. Ha egy meglévő fájlt, majd hello fájl felül lesznek írva, nincs append beállítás.

Ezután válassza e toolog minden, a kritikus, vagy hibaüzeneteket. Végül döntse el, hogy milyen gyakran a képernyő-átviteli üzenetet hello frissíti a rendszer a telepítés előrehaladását.

    ![Screenshot of Advanced configuration screen](./media/import-data/AdvancedConfiguration.png)

## <a name="confirm-import-settings-and-view-command-line"></a>Erősítse meg a beállításokat importálhat és parancs sor megtekintése
1. Adja meg az adatforrásra vonatkozó információ, a cél adatokat és a speciális konfigurációs, áttekintheti hello áttelepítési összefoglaló, és szükség esetén megtekintése és másolása hello eredő áttelepítési parancs (Másolás hello parancs hasznos tooautomate importálási műveletek):
   
    ![Képernyőkép a összegző képernyő](./media/import-data/summary.png)
   
    ![Képernyőkép a összegző képernyő](./media/import-data/summarycommand.png)
2. Ha elégedett a forrás- és beállításokat, kattintson **importálási**. hello eltelt idő, átvitt száma és (Ha nem adott meg a fájl nevét a hello speciális konfigurációs) hibaadatokat frissíti hello-import van folyamatban. Művelet befejeződése után exportálhatja hello eredmények (pl. bármely importálás hibákkal toodeal).
   
    ![Az Azure Cosmos DB JSON képernyőkép exportálási beállítás](./media/import-data/viewresults.png)
3. Egy új importálhat indulhat tartani a meglévő beállítások hello (pl. kapcsolati karakterlánc adatokat, a forrás és cél kiválasztása, stb.), vagy alaphelyzetbe állítása az összes értéket.
   
    ![Az Azure Cosmos DB JSON képernyőkép exportálási beállítás](./media/import-data/newimport.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Hello adatáttelepítési eszköz telepítése
> * Különböző adatforrásokból importált adatok
> * Exportált Azure Cosmos DB tooJSON

Ezután folytassa a következő oktatóanyag toohello és megtudhatja, hogyan tooquery adatok Azure Cosmos DB használatával. 

> [!div class="nextstepaction"]
>[Hogyan tooquery adatokat?](../cosmos-db/tutorial-query-documentdb.md)
