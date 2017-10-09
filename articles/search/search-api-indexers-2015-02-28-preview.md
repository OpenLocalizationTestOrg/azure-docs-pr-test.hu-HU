---
title: "Az indexelési műveletek (az Azure Search szolgáltatás REST API-ja: 2015-02-28 – előzetes verzió) |} Microsoft Docs"
description: "Az indexelési műveletek (az Azure Search szolgáltatás REST API-ja: 2015-02-28 – előzetes verzió)"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 42b5f85c-8304-4bc7-8e1e-a9c76b8ca25a
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/01/2017
ms.author: eugenesh
ms.openlocfilehash: 4dd9591072b44eeabae6eac1182b4eea10fd4a22
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="indexer-operations-azure-search-service-rest-api-2015-02-28-preview"></a>Az indexelési műveletek (az Azure Search szolgáltatás REST API-ja: 2015-02-28 – előzetes verzió)
> [!NOTE]
> Ez a cikk ismerteti a hello indexelők [2015-02-28-Preview REST API](search-api-2015-02-28-preview.md). Az API verziója ad hozzá a dokumentum kibontási az Azure Blob Storage indexelő és az Azure Table Storage indexelő, valamint az egyéb fejlesztések előzetes verziói. hello API (GA) általánosan elérhető indexelő, beleértve az indexelők az Azure SQL Database, SQL Server Azure virtuális gépeken, és Azure Cosmos DB is támogatja.
> 
> 

## <a name="overview"></a>Áttekintés
Az Azure Search integrálható közvetlenül néhány általános adatforrás hello kell toowrite kód tooindex eltávolítása az adatokat. a másolat tooset, és beállíthatja, hello Azure Search API toocreate hívás **indexelők** és **adatforrások**. 

Egy **indexelő** kapcsoló cél keresési indexek, az adatforrások erőforrás. Az indexelő használja a következő módokon hello: 

* Hello adatok toopopulate index egyszeri árnyékmásolata.
* Az ütemezés szerint hello adatforrás módosításokkal index szinkronizálása. hello ütemezés hello indexelő definíció részét képezi.
* Igény szerinti tooupdate igény szerint index meghívni. 

Egy **indexelő** akkor hasznos, ha azt szeretné, hogy a rendszeres frissítések tooan index. Az indexelő definíciójának részeként egy beágyazott ütemezés beállítása, vagy futtassa az igény szerinti használatával [indexelő futtatása](#RunIndexer). 

A **adatforrás** határozza meg, milyen adatokat kell toobe indexelt, hitelesítő adatok tooaccess hello adatokat, és házirendek tooenable Azure Search tooefficiently – hello adatokat (például a módosított vagy törölt sorok egy adatbázis tábláinak) a változásokat. Az definiálva van egy független erőforrásként, hogy több indexelők használható.

a következő adatforrások hello jelenleg támogatottak:

* **Az Azure SQL adatbázis** és **az Azure virtuális gépeken futó SQL Server**. A célként megadott segédlet, lásd: [Ez a cikk](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 
* **Az Azure Cosmos DB**. A célként megadott segédlet, lásd: [Ez a cikk](search-howto-index-documentdb.md). 
* **Az Azure Blob Storage**, többek között a következőket hello következő dokumentum formátumok: PDF, Microsoft Office (DOCX/DOC, XSLX/XLS, PPTX/PPT, Adatköltségek), HTML, XML, ZIP és egyszerű szövegként fájlok (beleértve a JSON). A célként megadott segédlet, lásd: [Ez a cikk](search-howto-indexing-azure-blob-storage.md).
* **Az Azure Table Storage**. A célként megadott segédlet, lásd: [Ez a cikk](search-howto-indexing-azure-tables.md).

A Microsoft fontolóra hozzáadása a további adatforrások támogatása a jövőbeli hello. velünk rangsorolására, ezek a döntések toohelp adja meg a visszajelzést hello [Azure Search-visszajelzési fórumon](http://feedback.azure.com/forums/263029-azure-search).

Lásd: [szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md) a felső határérték tooindexer és az adatok forrása kapcsolódó erőforrások.

## <a name="typical-usage-flow"></a>Tipikus használati folyamata
Létrehozhat és egyszerű HTTP-kérelmek (POST, GET, PUT, DELETE) keresztül indexelők és adatforrások kezelése szemben egy adott `data source` vagy `indexer` erőforrás.

Automatikus indexelő beállítása az általában egy négy lépésben folyamat:

1. Azonosítsa a hello adatforrást, amelynek indexelt toobe igénylő hello adatokat tartalmaz. Ne feledje, hogy Azure Search előfordulhat, hogy nem támogatja az összes hello adattípusok az adatforrás található. Lásd: [a támogatott adattípusokat](https://msdn.microsoft.com/library/azure/dn798938.aspx) hello listáját.
2. Hozzon létre egy Azure Search-index, amelynek séma összeegyeztethető az adatokat az adatforrásból.
3. Hozzon létre egy Azure Search adatforrás leírtak [adatforrás létrehozása](#CreateDataSource).
4. Hozzon létre egy Azure Search-indexelőt, leírtak [létrehozása indexelő](#CreateIndexer).

Kell egy indexelő minden célként megadott index és az adatforrás kombinációjához létrehozását tervezi. Több indexelők írása a hello azonos lehet indexet, és felhasználhatja hello több indexelők ugyanazt az adatforrást. Az indexelő azonban csak vehet igénybe, egyszerre több adatforrást, és csak írhat tooa egyetlen index. 

Miután létrehozta az indexelő, hello segítségével végrehajtási állapota le [indexelő állapotának beolvasása](#GetIndexerStatus) műveletet. Az indexelő is bármikor futtathatja (helyett vagy hozzáadása toorunning azt rendszeresen az ütemezés szerint) hello segítségével [indexelő futtatása](#RunIndexer) műveletet.

<!-- MSDN has 2 art files plus a API topic link list -->


## <a name="create-data-source"></a>Adatforrás létrehozása
Az Azure Search indexelők hello kapcsolati adatokat az alkalmi vagy ütemezett Adatfrissítés egy cél index ad használt adatforrás. Létrehozhat egy új adatforrás belül az Azure Search szolgáltatást használja a HTTP POST-kérelmet.

    POST https://[service name].search.windows.net/datasources?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Azt is megteheti hogy PUT használja, és adja meg hello adatforrás neve hello URI. Ha hello adatforrás nem létezik, a rendszer létrehozza.

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]

> [!NOTE]
> a tarifacsomag függ a hello engedélyezett adatforrások maximális számát. hello szabad szolgáltatás lehetővé teszi, hogy too3 adatforrást. Standard szolgáltatás lehetővé teszi, hogy 50 adatforrások. Lásd: [szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md) részleteiről.
> 
> 

**Kérés**

HTTPS-kapcsolat szükséges az összes szolgáltatási kérelmek. Hello **adatforrás létrehozása** kérelem egy POST vagy a PUT metódust használó lehet létrehozni. POST használatakor meg kell adnia egy adatforrás neve mellett hello adatforrása definíciója hello kérés törzsében. A PUT a hello értéke hello URL-cím része. Ha hello adatforrás nem létezik, akkor jön létre. Ha már létezik, akkor frissített toohello új definíciója. 

hello adatforrás neve kell kell kisbetű, betűvel vagy számmal kezdődhet, nincs perjeleket vagy pontokból és legfeljebb 128 karakter lehet. Hello adatforrás neve a névnek betűvel vagy számmal indítás után hello részeinek hello neve tartalmazhat bármely betű, szám és kötőjeleket, mindaddig, amíg nincsenek egymást követő kötőjeleket hello. Lásd: [elnevezési szabályait](https://msdn.microsoft.com/library/azure/dn857353.aspx) részleteiről.

Hello `api-version` szükséges. hello verziószámának `2015-02-28`.

**Kérelem fejlécei**

a következő lista hello hello szükséges és választható kérelemfejléc ismerteti. 

* `Content-Type`: Kötelező. Állítsa ezt túl`application/json`
* `api-key`: Kötelező. Hello `api-key` használt tooauthenticate hello kérelem tooyour keresőszolgáltatás. Egy karakterláncértéket, egyedi tooyour szolgáltatást is. Hello **adatforrás létrehozása** kérelem tartalmaznia kell egy `api-key` fejlécet beállítani tooyour adminisztrációs kulcsot (a megakadályozását tooa lekérdezési kulcsot). 

Hello szolgáltatás tooconstruct hello kérelem URL-CÍMÉT is szüksége lesz. Hello szolgáltatás nevet is kaphat és `api-key` a szolgáltatás irányítópultján a hello [Azure Portal](https://portal.azure.com/). Lásd: [hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) navigációs segítségét.

<a name="CreateDataSourceRequestSyntax"></a>
**Törzs szintaxis**

hello hello kérés törzsében egy adatforrás-definíciót, beleértve típusú hello adatforrás, hitelesítő adatok tooread hello adatok, valamint egy opcionális adatok címváltozásának felderítését és adatok törlését észlelési azonosítására használt tooefficiently házirendeket megváltozott vagy hello adatforrás egy rendszeresen ütemezett indexelő használata esetén a törölt adatokat. 

hello hello-kérések forgalma rendszerezésére szolgál szintaxisa a következő. Egy minta kérelem biztosítja az ebben a témakörben további.

    { 
        "name" : "Required for POST, optional for PUT. hello name of hello data source",
        "description" : "Optional. Anything you want, or nothing at all",
        "type" : "Required. Must be one of 'azuresql', 'documentdb', 'azureblob', or 'azuretable'",
        "credentials" : { "connectionString" : "Required. Connection string for your data source" },
        "container" : { "name" : "Required. hello name of hello table, collection, or blob container you wish tooindex" },
        "dataChangeDetectionPolicy" : { Optional. See below for details }, 
        "dataDeletionDetectionPolicy" : { Optional. See below for details }
    }

Kérelem tartalmaz hello következő tulajdonságai: 

* `name`: Kötelező. hello adatforrás hello neve. Adatforrás nevét kell csak kisbetűk, számjegyek és kötőjelek szerepelhetnek, nem kezdődhet vagy végződhet kötőjelekből és korlátozott too128 karakter hosszú lehet.
* `description`: Egy leírást. 
* `type`: Kötelező. Hello támogatott adatforrásokat valamelyike lehet:
  * `azuresql`-Azure SQL Database vagy az SQL Server Azure virtuális gépeken
  * `documentdb`-Azure Cosmos DB
  * `azureblob`-Azure Blobtárolóba
  * `azuretable`-Azure Táblatárolói
* `credentials`:
  * szükséges hello `connectionString` tulajdonság határozza meg a kapcsolati karakterlánc hello hello az adatforrásra vonatkozóan. hello kapcsolati karakterlánc formátuma hello hello adatforrástípust függ: 
    * Azure SQL ez pedig hello szokásos SQL Server kapcsolati karakterláncát. Ha hello Azure portál tooretrieve hello kapcsolati karakterláncot használ, akkor hello `ADO.NET connection string` lehetőséget.
    * Azure Cosmos DB, hello kapcsolati karakterláncot kell, hogy a formátum a következő hello: `"AccountEndpoint=https://[your account name].documents.azure.com;AccountKey=[your account key];Database=[your database id]"`. Hello értékek összes szükséges. Hello választhatók [Azure-portálon](https://portal.azure.com/).  
    * Azure-Blob és Table Storage ez pedig hello tárolási fiók kapcsolati karakterlánc. hello formátum leírt [Itt](https://azure.microsoft.com/documentation/articles/storage-configure-connection-string/). HTTPS-végpont protokoll megadása kötelező.  
* `container`, szükséges: hello adatok tooindex hello használatával megadja `name` és `query` tulajdonságok: 
  * `name`, szükséges:
    * Az Azure SQL: hello tábla vagy nézet határozza meg. Használhatja például a séma minősített nevek, `[dbo].[mytable]`.
    * A DocumentDB: hello gyűjtemény határozza meg. 
    * Az Azure Blob Storage: hello tároló határozza meg.
    * Az Azure Table Storage: hello tábla hello nevét adja meg. 
  * `query`, nem kötelező:
    * A DocumentDB: lehetővé teszi egy lekérdezést, amely egy tetszőleges JSON-dokumentum elrendezés simítja Azure Search indexelheti strukturálatlan sémába toospecify.  
    * Az Azure Blob Storage: lehetővé teszi a toospecify hello blob-tároló virtuális mappában. Például blob elérési út `mycontainer/documents/blob.pdf`, `documents` hello virtuális mappa használható.
    * Az Azure Table Storage: lehetővé teszi, hogy a szűrők importált sorok toobe készlete hello lekérdezés toospecify.
    * Az Azure SQL: a lekérdezés nem támogatott. Ha ez a funkció van szüksége, adjon szavazzon [Ez a javaslat](https://feedback.azure.com/forums/263029-azure-search/suggestions/9893490-support-user-provided-query-in-sql-indexer)
* nem kötelező hello `dataChangeDetectionPolicy` és `dataDeletionDetectionPolicy` tulajdonságok az alábbiakban található.

<a name="DataChangeDetectionPolicies"></a>
**Adatok módosítása szabályzatok**

hello célja az adatok szabályzat módosítása van tooefficiently módosított adatelemek azonosításához. Támogatott házirendek hello adatforrástípust függően változhat. Az alábbi szakaszok ismertetik a minden egyes házirend. 

***Magas vízjel módosítása szabályzat*** 

Ez a házirend felhasználhatja az adatforrást tartalmaz egy oszlop vagy egy tulajdonság, amely megfelel a következő feltételek hello:

* Minden Beszúrások hello oszlop értékét adja meg. 
* Minden frissítések tooan elem hello oszlop értéke hello is módosíthatja. 
* Ebben az oszlopban hello értékének fokozódik minden módosításakor.
* Egy szűrési záradékot hasonló toohello alábbi lekérdezések `WHERE [High Water Mark Column] > [Current High Water Mark Value]` hatékonyan hajtható végre.

Például, ha az Azure SQL adatforrásokat, egy indexelt használó `rowversion` oszlop hello ideális jelölt hello magas vízjel alapján házirenddel való használatra. 

Ez a házirend az alábbiak szerint adható meg:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
        "highWaterMarkColumnName" : "[a row version or last_updated column name]" 
    } 

Ha Azure Cosmos DB-adatforrásokhoz, használnia kell a hello `_ts` Azure Cosmos DB által biztosított tulajdonság. 

Azure Blob adatforrásokat, Azure Search automatikusan használatakor használ magas vízjel módosítása egy blob last-modified időbélyeg; alapján szabályzat nincs szükség ilyen házirend toospecify saját maga.   

***Az SQL integrált szabályzat módosítása***

Ha az SQL-adatbázis támogatja [a változáskövetés](https://msdn.microsoft.com/library/bb933875.aspx), javasoljuk, hogy az SQL integrált módosítása követési házirenddel. Ez a házirend lehetővé teszi hello leghatékonyabb változások követését, és Azure Search tooidentify törölt sorok anélkül, hogy toohave egy explicit "helyreállítható törlésre" oszlop a sémában.

Az integrált változáskövetés kezdve a következő SQL Server adatbázis-verziók hello támogatott: 

* SQL Server 2008 R2, SQL Server Azure virtuális gépeken használatakor.
* Az Azure SQL Database 12-es, az Azure SQL Database használata.  

Ha a házirend az SQL integrált változáskövetés ne adjon meg egy külön adatok törlési szabályzat – a ezzel a házirend-azonosító beépített támogatása törölt sorok. 

Ez a házirend csak akkor használható táblákkal; a nézetek nem használható. Használata esetén ez a házirend használata előtt hello tábla tooenable változáskövetése van szüksége. Lásd: [engedélyezése és letiltása a változáskövetés](https://msdn.microsoft.com/library/bb964713.aspx) utasításokat.    

Hello felépítésekor **adatforrás létrehozása** kérelem, az SQL integrált változáskövetés házirend adható meg az alábbiak szerint:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SqlIntegratedChangeTrackingPolicy" 
    }

<a name="DataDeletionDetectionPolicies"></a>
**Adatok törlése szabályzatok**

hello egy szabályzat adatok törlés célja tooefficiently törölt adatelemek azonosításához. Csak a támogatott hello házirend jelenleg hello `Soft Delete` házirendet, amely lehetővé teszi, hogy azonosító törölte a hello értéke alapján egy `soft delete` oszlop vagy hello adatforrás tulajdonság. Ez a házirend az alábbiak szerint adható meg:

    { 
        "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
        "softDeleteColumnName" : "hello column that specifies whether a row was deleted", 
        "softDeleteMarkerValue" : "hello value that identifies a row as deleted" 
    }

> [!NOTE]
> Karakterlánc, egész szám vagy logikai értékek csak oszlopok támogatottak. hello szerint használt érték `softDeleteMarkerValue` olyan karakterláncot kell megadni, akkor is, ha a megfelelő oszlop hello egész számok vagy logikai rendelkezik. Például, ha az adatforrás megjelenő hello érték 1, használja `"1"` , hello `softDeleteMarkerValue`.    
> 
> 

<a name="CreateDataSourceRequestExamples"></a>
**Példák a szervezet**

Ha azt tervezi, toouse hello azonosítójú adatforrás. az indexelő, amelyek ütemezés szerint fut, ez a példa bemutatja, hogyan toospecify módosítása és törlése szabályzatok: 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy", "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy", "softDeleteColumnName" : "IsDeleted", "softDeleteMarkerValue" : "true" }
    }

Ha csak toouse hello adatforrás hello adatok egyszeri példányát, nem hagyható hello házirendek:

    { 
        "name" : "asqldatasource",
        "description" : "anything you want, or nothing at all",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" }
    } 

**Válasz**

A kérelem sikeres: 201 létrehozva. 

<a name="UpdateDataSource"></a>

## <a name="update-data-source"></a>Adatforrás frissítése.
Egy meglévő adatforráson egy HTTP PUT-kérelmet használatával frissítheti. Hello adatok forrás tooupdate hello neve meg hello kérelem URI-azonosítója:

    PUT https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Hello `api-version` szükséges. hello verziószámának `2015-02-28`. [Az Azure Search API-verziók](https://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióival kapcsolatos további információk.

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Kérés**

hello kérés törzsében szintaktikai van hello megegyezik a [adatforrás létrehozása kérelmek](#CreateDataSourceRequestSyntax).

Néhány tulajdonság nem lehet frissíteni, egy meglévő adatforráson. Például egy meglévő adatforráson hello típusa nem módosítható.  

Ha egy meglévő adatforrás toochange hello kapcsolati karakterlánc nem szeretné, megadhatja a literális hello `<unchanged>` hello kapcsolati karakterláncot kell megadnia. Ez akkor hasznos, ahol tooupdate adatforrás szükséges, de nem rendelkezik kényelmes hozzáférési toohello kapcsolati karakterlánc, mert ez biztonsági szempontból kényes adatokat.

**Válasz**

A kérelem sikeres: 201 létrehozva, ha új adatforrás nem létezik, és 204 nem tartalom Ha egy meglévő adatforráson frissítették.

<a name="ListDataSource"></a>

## <a name="list-data-sources"></a>Az adatforrások listája
Hello **lista adatforrások** művelet az Azure Search szolgáltatás hello adatforrások listáját adja vissza. 

    GET https://[service name].search.windows.net/datasources?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` szükséges. hello verziószámának `2015-02-28`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

A kérelem sikeres: 200 OK gombra.

Íme egy példa adott válasz törzse:

    {
      "value" : [
        {
          "name": "datasource1",
          "type": "azuresql",
          ... other data source properties
        }]
    }

Vegye figyelembe, hogy le toojust hello tulajdonságok kíváncsiak vagyunk hello válasz szűrheti. Például, ha azt szeretné, hogy csak az adatforrások nevének listája, használja a hello OData `$select` lekérdezési lehetőség:

    GET /datasources?api-version=205-02-28&$select=name

Ebben az esetben a fenti példa hello hello válasz távolságban jelenjen meg az alábbiak szerint: 

    {
      "value" : [ { "name": "datasource1" }, ... ]
    }

Ez az egy hasznos módszer toosave sávszélesség, ha nagy mennyiségű adatforrások szerepel a keresési szolgáltatáshoz.

<a name="GetDataSource"></a>

## <a name="get-data-source"></a>Az adatforrás beolvasása
Hello **beolvasni az adatforrás** művelet lekérdezi a hello adatforrása definíciója az Azure Search.

    GET https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` szükséges. hello verziószámának `2015-02-28`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

a rendszer a hasonló tooexamples hello választ [adatforrás létrehozása példa kérelmek](#CreateDataSourceRequestExamples): 

    { 
        "name" : "asqldatasource",
        "description" : "a description",
        "type" : "azuresql",
        "credentials" : { "connectionString" : "Server=tcp:....database.windows.net,1433;Database=...;User ID=...;Password=...;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;" },
        "container" : { "name" : "sometable" },
        "dataChangeDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.HighWaterMarkChangeDetectionPolicy",
            "highWaterMarkColumnName" : "RowVersion" }, 
        "dataDeletionDetectionPolicy" : { 
            "@odata.type" : "#Microsoft.Azure.Search.SoftDeleteColumnDeletionDetectionPolicy",
            "softDeleteColumnName" : "IsDeleted", 
            "softDeleteMarkerValue" : "true" }
    }

> [!NOTE]
> Nincs beállítva a hello `Accept` kérelemfejléc túl`application/json;odata.metadata=none` amikor hívja az API-t, így hatására `@odata.type` hello választ, és nincs megadva attribútum toobe nem fogja tudni toodifferentiate adatainak módosítása és törlése észlelési adatok között házirendek a különböző típusú. 
> 
> 

<a name="DeleteDataSource"></a>

## <a name="delete-data-source"></a>Az adatforrás törlése
Hello **adatforrás törlése** művelet adatforrást eltávolítja az Azure Search szolgáltatás.

    DELETE https://[service name].search.windows.net/datasources/[datasource name]?api-version=[api-version]
    api-key: [admin key]

> [!NOTE]
> Ha bármely indexelők törölni hello adatforrásra hivatkozik, hello delete művelet továbbra is folytatódik. Azonban ezen indexelők ismerhetik hibaállapotba után a következő futtatáskor.  
> 
> 

Hello `api-version` szükséges. hello verziószámának `2015-02-28`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkód: 204 nem tartalom visszaküldött a sikeres válasz.

<a name="CreateIndexer"></a>

## <a name="create-indexer"></a>Az indexelő létrehozása
Létrehozhat egy új indexelő belül az Azure Search szolgáltatást használja a HTTP POST-kérelmet.

    POST https://[service name].search.windows.net/indexers?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Azt is megteheti hogy PUT használja, és adja meg hello adatforrás neve hello URI. Ha hello adatforrás nem létezik, a rendszer létrehozza.

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]

> [!NOTE]
> hello indexelők engedélyezett maximális száma függ a tarifacsomagra vált. hello szabad szolgáltatás lehetővé teszi, hogy másolatot too3 indexelők. Standard szolgáltatás lehetővé teszi, hogy 50 indexelők. Lásd: [szolgáltatásra vonatkozó korlátozások](search-limits-quotas-capacity.md) részleteiről.
> 
> 

Hello `api-version` szükséges. hello verziószámának `2015-02-28`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

<a name="CreateIndexerRequestSyntax"></a>
**Törzs szintaxis**

hello hello kérés törzsében egy indexelő definíciót, amely meghatározza a hello adatforrás és hello cél indexe indexelő, valamint a nem kötelező indexelési ütemezés és a paraméterek. 

hello hello-kérések forgalma rendszerezésére szolgál szintaxisa a következő. Egy minta kérelem biztosítja az ebben a témakörben további.

    { 
        "name" : "Required for POST, optional for PUT. hello name of hello indexer",
        "description" : "Optional. Anything you want, or null",
        "dataSourceName" : "Required. hello name of an existing data source",
        "targetIndexName" : "Required. hello name of an existing index",
        "schedule" : { Optional. See Indexing Schedule below. },
        "parameters" : { Optional. See Indexing Parameters below. },
        "fieldMappings" : { Optional. See Field Mappings below. },
        "disabled" : Optional boolean value indicating whether hello indexer is disabled. False by default.  
    }

**Az indexelő ütemezése**

Az indexelő opcionálisan megadható ütemezés szerint. Ha az ütemezés szerint meg adva, hello indexelő ütemezés szerint rendszeresen fog futni. Ütemezés azt a következő attribútumok hello:

* `interval`: Kötelező. Egy időtartamértéket, amely megadja az időközt vagy az indexelő időszak fut. hello legkisebb megengedett időköz 5 perc; leghosszabb hello egy nap. Az XSD "daytimeduration típusú" értéknek kell formázni (korlátozott részhalmaza egy [ISO 8601 időtartama](http://www.w3.org/TR/xmlschema11-2/#dayTimeDuration) érték). hello minta: `"P[nD][T[nH][nM]]"`. Példák: `PT15M` a 15 percenként `PT2H` minden 2 órán át. 
* `startTime`: Kötelező. Hello indexelő kell elindultak, amikor az UTC datetime. 

**Az indexelő paraméterek**

Az indexelő Megadja annak viselkedését befolyásoló több paramétert. Az összes hello paraméterek opcionálisak.  

* `maxFailedItems`: hello elemek száma, amelyek a hívások meghiúsulhatnak toobe indexelése egy futó indexelő hibát. Alapértelmezett érték 0. Sikertelen elemek információt ad vissza hello [indexelő állapotának beolvasása](#GetIndexerStatus) műveletet. 
* `maxFailedItemsPerBatch`: hello elemek száma, amelyek a hívások meghiúsulhatnak toobe indexelése az egyes kötegekben egy futó indexelő hibát. Alapértelmezett érték 0.
* `base64EncodeKeys`: Adja meg, hogy dokumentum kulcsok base-64 kódolású legyen-e. Az Azure Search dokumentumkulcsban használható dokumentum kulcsban. Azonban a hello értékeket a forrásadatokban tartalmazhat érvénytelen karaktereket. Ha ilyen dokumentum kulcsok értékeinek szükséges tooindex, ez a jelző tootrue állítható be. Alapértelmezett érték a `false`.
* `batchSize`: Adja meg egy kötegben a rendelés tooimprove teljesítményét hello elemek száma, amelyek hello adatforrásból beolvasása és indexelt. hello alapértelmezett függ hello adatforrás típusa: Azure SQL és az Azure Cosmos DB 1000, és az Azure Blob Storage 10.

**A mező hozzárendelések**

Hello tooa eltérő Mezőnevekkel adatforrásnév hello cél index használható mező hozzárendelések toomap egy mező nevét. Vegyük példaként a forrástábla mezőt `_id`. Az Azure Search hello mező kell átnevezni, aláhúzásjelet, kezdve a mezőnév nem engedélyezi. Ezt megteheti hello segítségével `fieldMappings` tulajdonság hello indexelő az alábbiak szerint: 

    "fieldMappings" : [ { "sourceFieldName" : "_id", "targetFieldName" : "id" } ] 

Több mező leképezéseket adhat meg: 

    "fieldMappings" : [ 
        { "sourceFieldName" : "_id", "targetFieldName" : "id" },
        { "sourceFieldName" : "_timestamp", "targetFieldName" : "timestamp" },
     ]

A forrás- és cél mezőnevek nem különböztetik meg.

<a name="FieldMappingFunctions"></a>
***A mező leképezése***

Mező hozzárendelések is használt tootransform forrás mező értékét *funkciók leképezési*.

Csak egy ilyen funkció jelenleg támogatott: `jsonArrayToStringCollection`. Hello cél index Collection(Edm.String) mezőbe egy JSON-tömb formátumú karakterláncot tartalmazó mező értelmezi. Célja való használathoz az Azure SQL indexelő különösen fontos, mivel az SQL natív adatok gyűjteménytípus nem rendelkezik. Az alábbiak szerint használható: 

    "fieldMappings" : [ { "sourceFieldName" : "tags", "mappingFunction" : { "name" : "jsonArrayToStringCollection" } } ] 

Például ha hello mezőjének hello karakterláncot tartalmaz `["red", "white", "blue"]`, majd hello célmező típusú `Collection(Edm.String)` tölti fel a három hello értékek `"red"`, `"white"` és `"blue"`.

Vegye figyelembe, hogy hello `targetFieldName` tulajdonság választható; akkor marad, ha hello `sourceFieldName` értéket használja. 

<a name="CreateIndexerRequestExamples"></a>
**Példák a szervezet**

hello alábbi példakód létrehozza indexelőt, amely adatokat másol hello által hivatkozott hello tábla `ordersds` adatforrás toohello `orders` index ütemezés szerint 2015 jan. 1 UTC elindul, és óránként futtat. Minden indexelő meghívás sikeres lesz, ha legfeljebb 5 elem az egyes kötegekben indexelt toobe sikertelen lesz, és legfeljebb 10 elemet toobe összesen indexelése sikertelen. 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }

**Válasz**

A kérelem sikeres 201 létrehozva.

<a name="UpdateIndexer"></a>

## <a name="update-indexer"></a>Indexelő frissítése
Egy meglévő indexelő egy HTTP PUT-kérelmet használatával frissítheti. Hello indexelő tooupdate hello neve meg hello kérelem URI-azonosítója:

    PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    Content-Type: application/json
    api-key: [admin key]

Hello `api-version` szükséges. hello verziószámának `2015-02-28`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Kérés**

hello kérés törzsében szintaktikai van hello megegyezik a [létrehozása indexelő kérelmek](#CreateIndexerRequestSyntax).

**Válasz**

A kérelem sikeres: 201 létrehozva, ha egy új indexelő nem létezik, és 204 nem tartalom Ha egy meglévő indexelő frissítették.

<a name="ListIndexers"></a>

## <a name="list-indexers"></a>Lista indexelő
Hello **lista indexelők** művelet az Azure Search szolgáltatás indexelők hello listáját adja vissza. 

    GET https://[service name].search.windows.net/indexers?api-version=[api-version]
    api-key: [admin key]


Hello `api-version` szükséges. hello előzetes verziója `2015-02-28-Preview`. [Az Azure Search versioning](https://msdn.microsoft.com/library/azure/dn864560.aspx) részletek és alternatív verzióival kapcsolatos további információk.

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

A kérelem sikeres: 200 OK gombra.

Íme egy példa adott válasz törzse:

    {
      "value" : [
      {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        ... other indexer properties
      }]
    }

Vegye figyelembe, hogy le toojust hello tulajdonságok kíváncsiak vagyunk hello válasz szűrheti. Például, ha azt szeretné, hogy csak indexelőt neveinek listáját, használja a hello OData `$select` lekérdezési lehetőség:

    GET /indexers?api-version=2014-10-20-Preview&$select=name

Ebben az esetben a fenti példa hello hello válasz távolságban jelenjen meg az alábbiak szerint: 

    {
      "value" : [ { "name": "myindexer" } ]
    }

Ez az egy hasznos módszer toosave sávszélesség, ha nagy mennyiségű indexelők szerepel a keresési szolgáltatáshoz.

<a name="GetIndexer"></a>

## <a name="get-indexer"></a>Indexelt tartalom lekérése
Hello **beolvasása indexelő** művelet hello indexelő definíciójának beolvasása az Azure Search.

    GET https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` szükséges. hello előzetes verziója `2015-02-28-Preview`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkód: 200 OK visszaküldött a sikeres válasz.

a rendszer a hasonló tooexamples hello választ [indexelő hozzon létre például kérelmek](#CreateIndexerRequestExamples): 

    {
        "name" : "myindexer",
        "description" : "a cool indexer",
        "dataSourceName" : "ordersds",
        "targetIndexName" : "orders",
        "schedule" : { "interval" : "PT1H", "startTime" : "2015-01-01T00:00:00Z" },
        "parameters" : { "maxFailedItems" : 10, "maxFailedItemsPerBatch" : 5, "base64EncodeKeys": false }
    }


<a name="DeleteIndexer"></a>

## <a name="delete-indexer"></a>Indexelő törlése
Hello **indexelő törlése** művelet eltávolítja az indexelő az Azure Search szolgáltatás.

    DELETE https://[service name].search.windows.net/indexers/[indexer name]?api-version=[api-version]
    api-key: [admin key]

Az indexelő törlésekor hello indexelő végrehajtások folyamatban akkori toocompletion működik, de az nincs további végrehajtások ütemezi. Nem található egy nem létező indexelő eredményeként HTTP-állapotkód: 404 kísérletek toouse. 

Hello `api-version` szükséges. hello előzetes verziója `2015-02-28-Preview`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkód: 204 nem tartalom visszaküldött a sikeres válasz.

<a name="RunIndexer"></a>

## <a name="run-indexer"></a>Az indexelő futtatása
Továbbá toorunning rendszeresen az ütemezés szerint, az indexelőt is meghívható hello segítségével igény **indexelő futtatása** műveletet: 

    POST https://[service name].search.windows.net/indexers/[indexer name]/run?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` szükséges. hello előzetes verziója `2015-02-28-Preview`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkód: 202 elfogadott visszaküldött a sikeres válasz.

<a name="GetIndexerStatus"></a>

## <a name="get-indexer-status"></a>Az indexelő állapotának beolvasása
Hello **indexelő állapotának beolvasása** művelet lekérdezi a hello aktuális állapot és végrehajtási előzményeinek indexelőt: 

    GET https://[service name].search.windows.net/indexers/[indexer name]/status?api-version=[api-version]
    api-key: [admin key]


Hello `api-version` szükséges. hello előzetes verziója `2015-02-28-Preview`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkódja: az OK gombra a sikeres válasz 200-as.

hello adott válasz törzsének indexelő általános állapotát, hello utolsó indexelő meghívása, valamint a legutóbbi indexelő indítások hello előzményeinek adatait tartalmazza, (ha van ilyen). 

Egy minta adott válasz törzsének így néz ki: 

    {
        "status":"running",
        "lastResult": {
            "status":"success",
            "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
         },
        "executionHistory":[ {
            "status":"success",
             "errorMessage":null,
            "startTime":"2014-11-26T03:37:18.853Z",
            "endTime":"2014-11-26T03:37:19.012Z",
            "errors":[],
            "itemsProcessed":11,
            "itemsFailed":0,
            "initialTrackingState":null,
            "finalTrackingState":null
        }]
    }

**Az indexelő állapota**

Az indexelő állapotának hello a következő értékek egyike lehet:

* `running`azt jelzi, hogy az indexelő hello megfelelően fut-e. Megjegyzendő, hogy néhány hello indexelő végrehajtások továbbra is sikertelenségét, így egy jó ötlet toocheck hello `lastResult` tulajdonságot is. 
* `error`azt jelzi, hogy az indexelő hello emberi beavatkozás nélkül nem javítható hibába ütközött. Például lehet, hogy lejárt hello az adatforrás hitelesítő adatainak, vagy hello adatforrás vagy hello cél index hello séma megváltozott a legfrissebb módon. 

**Az indexelő végrehajtás eredménye**

Az indexelő végrehajtás eredménye egy egyetlen indexelő végrehajtása adatait tartalmazza. hello legújabb eredmény van illesztett, hello `lastResult` hello indexelő állapot tulajdonsága. Más legutóbbi eredményeket, ha van ilyen, vissza, hello `executionHistory` hello indexelő állapot tulajdonsága. 

Indexelő végrehajtás eredménye a következő tulajdonságai hello tartalmazza: 

* `status`: hello egy végrehajtási állapotát. Lásd: [indexelő végrehajtásának állapota](#IndexerExecutionStatus) alábbi részleteket. 
* `errorMessage`: sikertelen végrehajtása hibaüzenetet. 
* `startTime`: hello időpontja UTC a futtatását indításakor.
* `endTime`: hello ideje UTC Formátumban, amikor a végrehajtása befejeződött. Ez az érték nincs beállítva, ha hello végrehajtása még folyamatban van.
* `errors`: elemszintű hibákat, ha van ilyen tömbjét. A leírásokban a dokumentum kulcsot tartalmaz (`key` tulajdonság) és egy hibaüzenetet (`errorMessage` tulajdonság). 
* `itemsProcessed`: hello forrás adatelemek száma (például a táblázat sorait), amely megkísérelte indexelő tooindex hello a végrehajtása során. 
* `itemsFailed`: hello elemek száma, amelyek a végrehajtása során nem sikerült.  
* `initialTrackingState`: mindig `null` hello első indexelő végrehajtásra, vagy ha a hello adatok követési házirend módosítása nem engedélyezett a hello használt adatforrás. Ilyen házirend engedélyezve van, ha a későbbi végrehajtások során ezt az értéket jelzi hello első (legalacsonyabb) változáskövetési érték dolgozza fel a végrehajtása. 
* `finalTrackingState`: mindig `null` Ha hello adatok követési házirend módosítása nem engedélyezett a hello használt adatforrás. Ellenkező esetben azt jelzi, hogy hello legújabb (legmagasabb) változáskövetési a végrehajtását sikeresen feldolgozott érték. 

<a name="IndexerExecutionStatus"></a>
**Az indexelő végrehajtásának állapota**

Az indexelő végrehajtásának állapota egy egyetlen indexelő végrehajtása hello állapotát rögzíti. Hello a következő értékeket veheti fel:

* `success`azt jelzi, hogy hello indexelő végrehajtása sikeresen befejeződött.
* `inProgress`azt jelzi, hogy hello indexelő végrehajtása folyamatban van. 
* `transientFailure`azt jelzi, hogy az indexelő végrehajtása sikertelen volt. Lásd: `errorMessage` részletek tulajdonság. hello hiba előfordulhat, hogy, vagy hogy nincs szükség emberi beavatkozás toofix – például egy séma kompatibilitási hello adatforrás és hello cél index között kijavítása szükség felhasználói beavatkozásra, míg egy ideiglenes adatok forrás üzemszünet nem. Indexelő indítások / ütemezés szerint folytatódik, ha ilyen. 
* `persistentFailure`azt jelzi, hogy hello az indexelő sikertelen volt, hogy az emberi beavatkozást igényelnek. Ütemezett indexelő végrehajtások leállítása. Miután elhárította a problémát hello, alaphelyzetbe állítja az indexelő API toorestart ütemezett hello végrehajtások használja. 
* `reset`azt jelzi, hogy hello az indexelő alaphelyzetbe lett állítva egy hívás tooReset indexelő API (lásd alább) által. 

<a name="ResetIndexer"></a>

## <a name="reset-indexer"></a>Az indexelő alaphelyzetbe állítása
Hello **indexelő alaphelyzetbe állítása** művelet hello változáskövetési hello indexelő társított állapotát visszaállítja. Ez lehetővé teszi tootrigger származó-teljesen új műveletek (például, ha a adatforrásséma megváltozott), vagy toochange hello adatok módosítása szabályzat hello indexelő társított egyes adatforrások esetében.   

    POST https://[service name].search.windows.net/indexers/[indexer name]/reset?api-version=[api-version]
    api-key: [admin key]

Hello `api-version` szükséges. hello előzetes verziója `2015-02-28-Preview`. 

Hello `api-key` egy adminisztrációs kulcsot (a lekérdezési kulcs megakadályozását tooa) kell lennie. Tekintse meg a hitelesítési szakasz toohello [Search szolgáltatás REST API](https://msdn.microsoft.com/library/azure/dn798935.aspx) toolearn további kulcsok. [Hozzon létre egy keresési szolgáltatás hello portálon](search-create-service-portal.md) azt ismerteti, hogyan tooget hello szolgáltatás URL-címe és kulcstulajdonságok kérésben használt hello.

**Válasz**

Állapotkód: 204 a sikeres válasz tartalmát nem.

## <a name="mapping-between-sql-data-types-and-azure-search-data-types"></a>SQL-adattípusok és az Azure Search adattípusok közötti leképezést
<table style="font-size:12">
<tr>
<td>SQL-adattípus</td>    
<td>Cél index mezőtípusok engedélyezett</td>
<td>Megjegyzések</td>
</tr>
<tr>
<td>bit</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>int, smallint, tinyint</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>bigint</td>
<td>Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>valódi, lebegőpontos</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>kis pénz típusú értékké, pénzt<br>Decimális<br>Numerikus
</td>
<td>Edm.String</td>
<td>Az Azure Search nem támogatja a decimális típusok konvertálásakor Edm.Double, mert ez elveszítik pontosság
</td>
</tr>
<tr>
<td>CHAR, nchar, varchar, nvarchar</td>
<td>Edm.String<br/>Collection(Edm.String)</td>
<td>Lásd: [mező leképezési funkciók](#FieldMappingFunctions) ebben a dokumentumban talál részletes tootransform egy Collection(Edm.String) egy karakterlánc oszlop</td>
</tr>
<tr>
<td>smalldatetime, dátum és idő, datetime2, dátum, datetimeoffset</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>uniqueidentifer</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>földrajzi hely</td>
<td>Edm.GeographyPoint</td>
<td>Csak az srid-Azonosítónak 4326 (amely hello alapértelmezett) pontra típusú geográfiai példányban támogatottak.</td>
</tr>
<tr>
<td>ROWVERSION</td>
<td>N/A</td>
<td>Sor verziójú oszlopok hello search-index nem lehet tárolni, de a változások követése is használhatók</td>
</tr>
<tr>
<td>a timespan idő<br>binary, varbinary, kép,<br>XML-geometriai, CLR-típus</td>
<td>N/A</td>
<td>Nem támogatott</td>
</tr>
</table>

## <a name="mapping-between-json-data-types-and-azure-search-data-types"></a>JSON az adattípusokat és az Azure Search adattípusok közötti leképezést
<table style="font-size:12">
<tr>
<td>JSON-adattípus</td>    
<td>Cél index mezőtípusok engedélyezett</td>
<td>Megjegyzések</td>
</tr>
<tr>
<td>logikai érték</td>
<td>Edm.Boolean, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Egész szám</td>
<td>Edm.Int32, Edm.Int64, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Lebegőpontos szám</td>
<td>Edm.Double, Edm.String</td>
<td></td>
</tr>
<tr>
<td>Karakterlánc</td>
<td>Edm.String</td>
<td></td>
</tr>
<tr>
<td>tömbök primitív típusok, pl. ["a", "b", "c"]</td>
<td>Collection(Edm.String)</td>
<td></td>
</tr>
<tr>
<td>Karakterláncok, dátumok hasonló</td>
<td>Edm.DateTimeOffset, Edm.String</td>
<td></td>
</tr>
<tr>
<td>GeoJSON-pont objektumok</td>
<td>Edm.GeographyPoint</td>
<td>GeoJSON pontok JSON-objektumok a hello a következő formátumban: {"type": "Point", "coordinates": [hosszú, lat]} </td>
</tr>
<tr>
<td>Más JSON-objektumok</td>
<td>N/A</td>
<td>Nem támogatott. Az Azure Search jelenleg csak a primitív típusok és a karakterlánc-gyűjtemények</td>
</tr>
</table>
