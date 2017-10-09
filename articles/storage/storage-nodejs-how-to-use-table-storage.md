---
title: "Azure Table storage-ának Node.js aaaHow toouse |} Microsoft Docs"
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: storage
documentationcenter: nodejs
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.openlocfilehash: 990a71337b806d759d0277a7691712346db7b355
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-nodejs"></a>Hogyan toouse Azure Table storage Node.js-ből
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table szolgáltatás Node.js-alkalmazásokban.

hello ebben a témakörben szereplő példák azt feltételezik, hogy már rendelkezik egy Node.js-alkalmazást. További információ egy Node.js-alkalmazás az Azure toocreate jelennek meg ezek a témakörök:

* [Node.js-webalkalmazás létrehozása az Azure App Service-ben](../app-service-web/app-service-web-get-started-nodejs.md)
* [Hozza létre és telepítheti a Node.js web app tooAzure WebMatrix használatával.](../app-service-web/web-sites-nodejs-use-webmatrix.md)
* [És üzembe egy Node.js-alkalmazás tooan Azure Cloud Service](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (a Windows PowerShell használatával)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="configure-your-application-tooaccess-azure-storage"></a>Az alkalmazás tooaccess Azure Storage konfigurálása
Azure Storage toouse, kell hello Azure Storage szolgáltatás SDK a Node.js, amely kényelmi szalagtárak hello tárolószolgáltatások REST kommunikációt készletét tartalmazza.

### <a name="use-node-package-manager-npm-tooinstall-hello-package"></a>Csomópont Package Manager (NPM) tooinstall hello csomag használata
1. Használjon például egy parancssori felületet **PowerShell** (Windows) **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix), és keresse meg a toohello mappa, amelyben létrehozta az alkalmazást.
2. Típus **npm telepítése azure-tároló** hello parancssori ablakban. Hello parancs kimenete a következő példa hasonló toohello.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Manuális futtatásával hello **ls** parancs tooverify, amely egy **csomópont\_modulok** mappa hozták létre. Mappán belül található hello **azure-tároló** csomagot, amely tartalmaz hello szalagtárak tooaccess tárolási van szüksége.

### <a name="import-hello-package"></a>Hello csomag importálása
Adja hozzá a következő kód toohello felső részén hello hello **server.js** fájl az alkalmazásban:

```nodejs
var azure = require('azure-storage');
```

## <a name="set-up-an-azure-storage-connection"></a>Egy Azure Storage-kapcsolat beállítása
hello azure modul olvassák hello környezeti változók AZURE\_tárolási\_FIÓKOT és az AZURE\_tárolási\_hozzáférés\_kulcs, vagy AZURE\_tárolási\_kapcsolat \_Szükséges adatokat tooconnect tooyour Azure storage-fiók KARAKTERLÁNCOT. Ha ezek a környezeti változók nem, meg kell adnia hello fiók meghívásakor **TableService**.

Példa a hello hello környezeti változók beállítása [Azure-portálon](https://portal.azure.com) egy Azure-webhelyre, lásd: [Node.js web app használatával hello Azure Table szolgáltatás](../app-service-web/storage-nodejs-use-table-storage-web-site.md).

## <a name="create-a-table"></a>Tábla létrehozása
hello alábbi kód létrehoz egy **TableService** objektumra, és használja ezt a toocreate új tábla. Adja hozzá hello következő tetején hello **server.js**.

```nodejs
var tableSvc = azure.createTableService();
```

hívás túl hello**createTableIfNotExists** hoz létre egy új tábla hello megadott névvel, ha még nem létezik. hello alábbi példa táblát hoz létre egy új "mytable" nevű, ha még nem létezik:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

Hello `result.created` lesz `true` Ha új tábla létrehozása, és `false` Ha hello tábla már létezik. Hello `response` hello kérelem információkat tartalmaznak.

### <a name="filters"></a>Szűrők
Választható szűrési műveletek lehet segítségével alkalmazott toooperations **TableService**. Műveletek szűrésének lehetnek naplózási, automatikus újrapróbálkozása, stb. Szűrők hello aláírással metódus megvalósító objektumok a következők:

```nodejs
function handle (requestOptions, next)
```

Ezután a hello lehetőségek előfeldolgozása, hello metódust kell toocall "Tovább" gombra, egy visszahívási átadja a aláírását hello:

```nodejs
function (returnObject, finalCallback, next)
```

A visszahívási és hello returnObject (hello hello kérelem toohello kiszolgálótól kapott válasz) feldolgozása után hello visszahívási kell tooeither következő meghívni, ha más szűrők feldolgozása toocontinue létezik, vagy egyszerűen a finalCallback meghívása egyéb tooend hello szolgáltatás hívása.

Két szűrőket, amelyek megvalósítják az újrapróbálkozási logika érhetők el a hello Azure SDK for Node.js, **ExponentialRetryPolicyFilter** és **LinearRetryPolicyFilter**. hello következő létrehoz egy **TableService** hello használó objektum **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
egy entitás tooadd először létre kell hoznia egy objektum, amely meghatározza az entitás tulajdonságait. Összes entitás tartalmaznia kell egy **PartitionKey** és **RowKey**, amelyeket hello entitás egyedi azonosítót.

* **PartitionKey** -hello partíció hello entitás tárolt határozza meg
* **RowKey** - egyedileg azonosítja a hello entitás hello partíción belül

Mindkét **PartitionKey** és **RowKey** karakterlánc-értékekkel kell lennie. További információkért lásd: [ismertetése hello tábla szolgáltatás adatmodell](http://msdn.microsoft.com/library/azure/dd179338.aspx).

hello az alábbiakban látható egy példa egy entitás meghatározása. Vegye figyelembe, hogy **dueDate** van definiálva, egyfajta **Edm.DateTime**. Megadását hello típusa nem kötelező, és típusok fog lehet következtetni rá, ha nincs megadva.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> Szerepel továbbá egy **időbélyeg** mezőjét az egyes rekordokhoz, amely az Azure-ban van beállítva, ha entitás beszúrni vagy frissíteni.
>
>

Is használhatja a hello **entityGenerator** toocreate entitásokat. hello alábbi példakód létrehozza hello hello azonos feladat entitást **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out hello trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

egy entitás tooyour tábla tooadd átadni hello entitás objektum toohello **insertEntity** metódust.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Ha hello művelet sikeres, `result` fogja tartalmazni hello [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) a hello beszúrni a rekord és `response` hello művelet információkat tartalmaznak.

Példa egy válasz:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Alapértelmezés szerint **insertEntity** nem ad vissza beszúrni hello entitás hello részeként `response` információkat. Ha más műveleteket végez ehhez az entitáshoz tervez, vagy toocache hello adatokat kívánja, azok által visszaadott hello részeként hasznos toohave `result`. Ehhez engedélyezésével **echoContent** az alábbiak szerint:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Egy entitás frissítése
Van több módszer érhető el tooupdate meglévő entitás:

* **replaceEntity** -cserélje ki egy meglévő entitás frissítéséhez
* **mergeEntity** -meglévő entitás frissíti új tulajdonságértékek egyesíti hello meglévő entitás
* **insertOrReplaceEntity** -cserélje ki egy meglévő entitás frissíti. Ha nem entitás létezik, egy új szúrja be a
* **insertOrMergeEntity** -frissíti a meglévő entitás által hello meglévő új tulajdonságértékek egyesíti. Ha nem entitás létezik, egy új szúrja be a

hello következő példa bemutatja egy entitás használatával frissítése **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Alapértelmezés szerint egy entitás frissítése nem ellenőrzi toosee Ha hello adatok frissítése során korábban egy másik folyamat módosult. toosupport egyidejű frissítések:
>
> 1. Hello ETag frissítendő hello objektum beolvasása. A visszaadott hello részeként `response` entitás kapcsolatos műveletek és kérhető `response['.metadata'].etag`.
> 2. Entitás frissítési művelet végrehajtásakor adja hozzá a korábban beolvasni toohello új entitás hello ETag adatait. Példa:
>
>       entity2 [.metadata] .etag = currentEtag;
> 3. Hello frissítési műveletet végrehajtani. Ha hello entitás hello ETag érték, például az alkalmazás egy másik példánya lekért óta módosították egy `error` az eredmény figyelmezteti a felhasználókat arra, hogy hello kérelemben megadott hello frissítés feltétel nem teljesült.
>
>

A **replaceEntity** és **mergeEntity**, ha a frissítés alatt álló hello entitás nem létezik, akkor hello frissítési művelet sikertelen lesz. Ezért ha toostore egy entitás, függetlenül attól, hogy létezik-e, használjon **insertOrReplaceEntity** vagy **insertOrMergeEntity**.

Hello `result` sikeres frissítés műveletek hello fogja tartalmazni **Etag** hello az entitás frissítése.

## <a name="work-with-groups-of-entities"></a>Az entitások csoportok használata
Néha azt teszi logika toosubmit több művelet együtt egy kötegelt tooensure atomi hello kiszolgáló általi feldolgozás alatt. használó, hello tooaccomplish **TableBatch** toocreate egy kötegelt osztály, és aztán hello **executeBatch** metódusában **TableService** tooperform hello kötegelni műveletek.

 a következő példa hello mutatja be két entitás kötegben elküldése:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out hello trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash hello dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

A sikeres kötegelt műveleteket `result` hello kötegben lévő egyes műveletek információt tartalmaz.

### <a name="work-with-batched-operations"></a>A kötegelt műveletek használata
Műveletek hozzáadott tooa kötegelt ellenőrizni kell hello megtekintésével `operations` tulajdonság. A következő módszerek toowork műveletekkel hello is használhatja:

* **Törölje a jelet** -egy köteg az összes művelet törlése
* **getOperations** -művelet lekérdezi a hello kötegelt
* **hasOperations** -hello kötegelt műveleteket tartalmaz. Ha igaz értéket ad vissza
* **removeOperations** -eltávolítja egy műveletet
* **méret** -értéket ad vissza hello hello kötegben műveletek száma

## <a name="retrieve-an-entity-by-key"></a>Egy entitás kulcs letöltése
egy adott entitás alapján hello tooreturn **PartitionKey** és **RowKey**, használja a hello **retrieveEntity** metódus.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains hello entity
  }
});
```

Ez a művelet befejeződése után `result` hello entitás fogja tartalmazni.

## <a name="query-a-set-of-entities"></a>Az entitások készletének lekérdezése
egy tábla tooquery hello használata **TableQuery** objektum toobuild fel egy lekérdezési kifejezésben hello záradék a következő használatával:

* **Válassza ki** -hello mezők toobe hello lekérdezés által visszaadott
* **Ha** – hello ahol záradék

  * **és** - `and` where feltétel
  * **vagy** - `or` where feltétel
* **felső** -hello toofetch elemek száma

hello alábbi példa összeállít egy lekérdezést, amely visszaadható hello felső öt cikkeket a PartitionKey "hometasks".

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Mivel a **válasszon** nem használ, akkor minden mező adja vissza. egy tábla, használja a tooperform hello lekérdezése **queryEntities**. hello alábbi példában a "mytable" lekérdezés tooreturn entitásokat.

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

Ha sikeres, `result.entries` hello lekérdezésnek megfelelő entitások tömbjét tartalmazza. Ha hello lekérdezés nem tooreturn összes entitás `result.continuationToken` lesz nem -*null* és harmadik paramétere hello használható **queryEntities** tooretrieve további eredmények. Hello kezdeti lekérdezést, használjon *null* hello harmadik paraméter.

### <a name="query-a-subset-of-entity-properties"></a>Az entitástulajdonságok egy részének lekérdezése
A lekérdezéstábla tooa pár mezők kérhetnek le egy entitás.
Ez csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat. Használjon hello **válasszon** hello mezők toobe záradék és pass hello nevét adja vissza. Például hello következő lekérdezés által visszaadott csak hello **leírás** és **dueDate** mezőket.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Entitás törlése
A partíció- és sorfejlécek kulcsokkal entitás törölheti. Ebben a példában hello **task1** objektum tartalmaz hello **RowKey** és **PartitionKey** hello entitás toobe törölt értékeit. Ezután hello objektum átadása toohello **deleteEntity** metódust.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> ETag-EK esetekben érdemes elemek, tooensure, amely hello elem törlése nem módosították egy másik folyamat. Lásd: [frissíthető entitás](#update-an-entity) információ az ETag-EK használatával.
>
>

## <a name="delete-a-table"></a>Tábla törlése
a következő kód hello törölhető egy tábla a tárfiókból.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Ha biztos benne, hogy hello tábla létezik, **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Használjon tokeneket folytatása
Ha nagy mennyiségű eredmények táblák kérdez le, keresse meg folytatási jogkivonatokat. Nagy mennyiségű adat lehet a lekérdezést, hogy még lehet, hogy nem megvalósítható Ha toorecognize nem hoz létre, ha jelen a folytatási kód érhető el.

hello eredmények objektum lekérdezése entitások beállítása során a `continuationToken` tulajdonságot, ha ilyen tokent. Majd ezzel a lekérdezés toocontinue toomove hello partíció és tábla entitások közötti végrehajtása során.

Kérdez le, ha egy continuationToken paraméter is megadható hello lekérdezés objektumpéldány és hello visszahívási függvény között:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Ha hello nézze meg `continuationToken` objektum található tulajdonságok például `nextPartitionKey`, `nextRowKey` és `targetLocation`, amely minden hello eredmény keresztül használt tooiterate lehet.

Hello Azure Storage Node.js tárház a Githubon belül folytatási minta is van. Keressen `examples/samples/continuationsample.js`.

## <a name="work-with-shared-access-signatures"></a>Megosztott hozzáférési aláírásokkal működik
Közös hozzáférésű jogosultságkód (SAS) anélkül, hogy a tárfiók neve vagy a kulcsok egy biztonságos módon tooprovide részletes hozzáférés tootables. SAS olyan gyakran használt tooprovide korlátozott hozzáférés tooyour adatok, például így a mobilalkalmazások tooquery rögzíti.

A megbízható alkalmazások, például egy felhőalapú szolgáltatás hoz létre egy SAS használatával hello **generateSharedAccessSignature** a hello **TableService**, tooan biztosítja azt, és nem megbízható vagy félig megbízható alkalmazás például egy mobilalkalmazást. hello SAS egy házirendet, amely ismerteti a hello kezdő és záró dátumát, mely hello során SAS érvénytelen jön létre, valamint hello hozzáférési szint megadott toohello SAS tulajdonosával.

hello alábbi példa létrehoz egy új megosztott elérési házirendet, amely lehetővé teszi a hello SAS jogosult tooquery (r) hello tábla, és 100 perc létrehozták hello idő múlva lejár.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Vegye figyelembe, hogy hello állomásadatai csak megadott is szükség rá, amikor hello SAS jogosult megpróbálja tooaccess hello tábla.

hello ügyfélalkalmazást, akkor használja a biztonsági Társítások hello **TableServiceWithSAS** tooperform műveletek hello táblázaton. a következő példa hello toohello tábla kapcsolódik, és lekérdezést hajt végre.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains hello entities
  }
});
```

Hello SAS generálása óta lekérdezés hozzáférést, csak ha kísérlet történt tooinsert, frissítése vagy törlése entitások, akkor kell hibát jelez.

### <a name="access-control-lists"></a>Hozzáférés-vezérlési listák
A hozzáférés-vezérlési lista (ACL) tooset hello hozzáférési házirendek tartozó SAS korlátozására is használhatja. Ez akkor hasznos, ha akarja tooallow több ügyfelek tooaccess hello táblában, de adjon meg másik hozzáférési házirendeket az egyes ügyfelekhez.

Hozzáférés-vezérlési Listában hozzáférési házirendeket, tömbje segítségével minden házirendhez társított azonosítójú van megvalósítva. a következő példa hello két házirend, egy "felhasználó1", és egy "felhasználó2" határozza meg:

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

a következő példa lekérdezi hello hello aktuális hozzáférés-vezérlési listája hello **hometasks** tábla, és hozzáadja az új házirendeket hello segítségével **setTableAcl**. Ez a megközelítés lehetővé teszi, hogy:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Egyszer hello hozzáférés-vezérlési lista van beállítva, majd létrehozhat egy SAS-házirendje hello azonosítója alapján. a következő példa hello hoz létre egy új SAS-kód "felhasználó2":

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Következő lépések
További információkért tekintse meg a következő erőforrások hello.

* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* [Az Azure Storage szolgáltatás SDK csomópont](https://github.com/Azure/azure-storage-node) GitHub tárházából.
* [Node.js fejlesztői központ](/develop/nodejs/)
* [Létrehozhat és telepíthet egy Node.js-alkalmazás tooan Azure-webhelyen](../app-service-web/app-service-web-get-started-nodejs.md)
