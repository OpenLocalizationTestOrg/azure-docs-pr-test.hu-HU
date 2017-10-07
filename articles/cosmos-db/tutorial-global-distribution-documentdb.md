---
title: "aaaAzure Cosmos DB globális terjesztési oktatóprogram a DocumentDB az API-hoz |} Microsoft Docs"
description: "Ismerje meg, hogyan hello DocumentDB API toosetup Azure Cosmos DB globális terjesztési használatával."
services: cosmos-db
keywords: "globális terjesztési, a documentdb"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: a1d5f01faa62407fbbc9c078ef4a9589a1a29219
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-documentdb-api"></a>Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello DocumentDB API

Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello DocumentDB API használatával.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési használatával hello konfigurálása [DocumentDB API-k](documentdb-introduction.md)

<a id="portal"></a>
[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-documentdb-api"></a>Csatlakozás előnyben részesített régióba tooa hello DocumentDB API

A rendelés tootake előnyeit [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások hello rendezett használt tooperform dokumentum műveletek régiók toobe preferencia listája adhat meg. Ezt megteheti hello kapcsolat házirend beállításával. Hello Azure Cosmos DB fiók konfigurációja alapján, aktuális területi rendelkezésre állási és hello preferencia lista megadva, a legtöbb optimális végpont választja ki a DocumentDB SDK tooperform írási és olvasási műveletek hello hello.

Ez a beállítás lista inicializálása közben hello DocumentDB SDK-k használatával van megadva. hello SDK-k elfogadása "PreferredLocations" nem kötelező paraméter, amely egy Azure-régiók rendezett listáját.

hello SDK automatikusan elküld minden írások toohello aktuális írási terület.

Az összes olvasási küld toohello első rendelkezésre álló terület hello PreferredLocations listában. Hello kérelem sikertelen lesz, ha hello ügyfél le hello lista toohello következő terület sikertelen, és így tovább.

hello SDK-k csak kísérli meg a hello régió van megadva a PreferredLocations tooread. Így például ha hello adatbázisfiók három régiókban, de hello ügyfél csak határozza meg két hello nem írási régiók PreferredLocations, majd nincs olvasási szolgáltató hello írási régió, még akkor is, a feladatátvétel hello eset kívül.

hello alkalmazás hello aktuális írási végpont ellenőrizheti és olvassa el a végpont által hello SDK WriteEndpoint és ReadEndpoint, elérhető, a SDK 1.8-as verzióját és az újabb ellenőrzési két tulajdonság által választott.

Hello PreferredLocations tulajdonság nincs beállítva, ha minden kérésnél hello aktuális írási régióban kell kézbesíteni.

## <a name="net-sdk"></a>.NET SDK
hello SDK kód módosítások nélkül használható. Ebben az esetben hello SDK automatikusan számáról mindkét olvasási és az toohello aktuális írási területen írja.

1,8 és hello .NET SDK újabb verziójában hello ConnectionPolicy paramétere hello DocumentClient konstruktor hívása Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations tulajdonsággal rendelkezik. Ez a tulajdonság nem gyűjtemény típusú `<string>` és tartalmaznia kell a régió neveinek listáját. hello karakterlánc-értékek vannak formázva a hello hello régió neve oszloponként [Azure-régiókat] [ regions] lap, szóközök nélkül előtt vagy után hello először, és az utolsó karakter rendre.

hello aktuális írási és olvasási végpontok találhatók DocumentClient.WriteEndpoint és DocumentClient.ReadEndpoint kulcsattribútumokkal.

> [!NOTE]
> hello URL-címek hello végpontok hosszú élettartamú állandóként nem tekinthető. hello szolgáltatás ezek bármikor előfordulhat, hogy frissíteni. hello SDK automatikusan kezeli ezt a módosítást.
>
>

```csharp
// Getting endpoints from application settings or other configuration location
Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
string accountKey = Properties.Settings.Default.GlobalDatabaseKey;
  
ConnectionPolicy connectionPolicy = new ConnectionPolicy();

//Setting read region selection preference
connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

// initialize connection
DocumentClient docClient = new DocumentClient(
    accountEndPoint,
    accountKey,
    connectionPolicy);

// connect tooDocDB
await docClient.OpenAsync().ConfigureAwait(false);
```

## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS és a Python SDK-k
hello SDK kód módosítások nélkül használható. Ebben az esetben SDK automatikusan átirányítja a hello is beolvassa és toohello aktuális írási terület.

Verzió 1,8, és később a minden SDK hello ConnectionPolicy paraméterében hello DocumentClient konstruktor DocumentClient.ConnectionPolicy.PreferredLocations nevű új tulajdonsággal. A paraméter karakterláncok tömbje, amely régió neveinek listáját. a hello hello régió neve oszloponként formázott hello nevek [Azure-régiókat] [ regions] lap. Használhatja az előre megadott hello állandók hello AzureDocuments.Regions kényelmi objektumban

hello aktuális írási és olvasási végpontok találhatók DocumentClient.getWriteEndpoint és DocumentClient.getReadEndpoint kulcsattribútumokkal.

> [!NOTE]
> hello URL-címek hello végpontok hosszú élettartamú állandóként nem tekinthető. hello szolgáltatás ezek bármikor előfordulhat, hogy frissíteni. hello SDK automatikusan kezeli ezt a módosítást.
>
>

Az alábbiakban egy példa van NodeJS/Javascript. Python és a Java követi hello ugyanilyen mintájú.

```java
// Creating a ConnectionPolicy object
var connectionPolicy = new DocumentBase.ConnectionPolicy();

// Setting read region selection preference, in hello following order -
// 1 - West US
// 2 - East US
// 3 - North Europe
connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];

// initialize hello connection
var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);
```

## <a name="rest"></a>REST
Az adatbázisfiók elérhetővé tett több régióba, ha az ügyfelek lekérhetnek által egy GET kérelem végrehajtása a következő URI hello elérhetőségét.

    https://{databaseaccount}.documents.azure.com/

hello szolgáltatás régiók és a megfelelő Azure Cosmos DB végpont URI-azonosítók hello replikáinak listáját adja vissza. hello aktuális írási terület hello válasz jelzik. hello ügyfél ezután kijelölhet hello megfelelő végpont a minden további REST API-kérelmek az alábbiak szerint.

Példa egy válasz

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


* Minden PUT, POST és DELETE kérelmek be kell lépnie jelzett toohello írási URI
* Minden lekérdezi és egyéb olvasási kérések (például a lekérdezések) előfordulhat, hogy nyissa meg az hello ügyfél választott tooany végpont

Az írási kérések csak tooread régiók sikertelen lesz, és HTTP-hibakódot 403 ("tiltott").

Ha hello írási régió után módosítja a hello ügyfél kezdeti felderítés fázisban későbbi ír előző toohello írási régió sikertelen lesz, és HTTP-hibakódot ("tiltott") 403. hello ügyfél kell majd le régiók listáját hello újra tooget hello frissített írási régióban.

Ez azt, hogy ez az oktatóanyag befejezése. Megismerheti, hogyan toomanage hello olvasásával globálisan replikált fiókja konzisztencia [Azure Cosmos DB-ben konzisztenciaszintek](consistency-levels.md). És hogyan globális adatbázis-replikációval kapcsolatos további információk az Azure Cosmos Adatbázisba működik, a következő témakörben: [adatok globálisan Azure Cosmos DB terjesztése](distribute-data-globally.md).

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban hello következő régebben már kötöttek:

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési hello DocumentDB API-k használatával konfigurálása

Most már folytathatja a következő útmutató toolearn toohello hogyan helyileg toodevelop hello Azure Cosmos DB helyi emulátor.

> [!div class="nextstepaction"]
> [Hello emulátorral helyileg fejlesztése](local-emulator.md)

[regions]: https://azure.microsoft.com/regions/

