---
title: "Graph API aaaAzure Cosmos DB globális terjesztési oktatóanyaga |} Microsoft Docs"
description: "Ismerje meg, hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello Graph API-val."
services: cosmos-db
keywords: "globális terjesztési, graph, gremlin"
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: denlee
ms.openlocfilehash: 1629a31e12a18079f63e07c4909862b36b5f4c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toosetup-azure-cosmos-db-global-distribution-using-hello-graph-api"></a>Hogyan toosetup Azure Cosmos DB globális terjesztési használatával hello Graph API-val

Ez a cikk megmutatjuk, hogyan toouse hello Azure portál toosetup Azure Cosmos DB globális terjesztési, és csatlakoztassa a hello Graph API-val (előzetes verzió) használatával.

Ez a cikk ismerteti a következő feladatok hello: 

> [!div class="checklist"]
> * Konfigurálja a globális terjesztési hello Azure-portál használatával
> * Globális terjesztési használatával hello konfigurálása [Graph API-k](graph-introduction.md) (előzetes verzió)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-tooa-preferred-region-using-hello-graph-api-using-hello-net-sdk"></a>Csatlakozás előnyben részesített régióba tooa hello Graph API hello .NET SDK használatával

Graph API hello fel van fedve egy bővítmény tárként fölött hello DocumentDB SDK-t.

A rendelés tootake előnyeit [globális terjesztési](distribute-data-globally.md), ügyfélalkalmazások hello rendezett használt tooperform dokumentum műveletek régiók toobe preferencia listája adhat meg. Ezt megteheti hello kapcsolat házirend beállításával. Hello Azure Cosmos DB fiók konfigurációtól függően aktuális területi rendelkezésre állását és hello preferencia lista megadott, hello legtöbb optimális végpont rendszer hello SDK tooperform írási által választott és olvasási műveletek.

Ez a beállítás lista inicializálása közben hello SDK-k használatával van megadva. hello SDK-k elfogadása "PreferredLocations" nem kötelező paraméter, amely egy Azure-régiók rendezett listáját.

* **Írási műveletek**: hello SDK automatikusan elküld minden ír toohello aktuális írási terület.
* **Beolvassa**: minden olvasási küld toohello első rendelkezésre álló terület hello PreferredLocations listában. Hello kérelem sikertelen lesz, ha hello ügyfél le hello lista toohello következő terület sikertelen, és így tovább. hello SDK-k csak kísérli meg a hello régió van megadva a PreferredLocations tooread. Így például ha hello Cosmos DB fiók három régiókban, de hello ügyfél csak határozza meg két hello nem írási régiók PreferredLocations, majd nincs olvasási szolgáltató hello írási régió, még akkor is, a feladatátvétel hello eset kívül.

hello alkalmazás hello aktuális írási végpont ellenőrizheti és olvassa el a végpont által hello SDK WriteEndpoint és ReadEndpoint, elérhető, a SDK 1.8-as verzióját és az újabb ellenőrzési két tulajdonság által választott. Hello PreferredLocations tulajdonság nincs beállítva, ha minden kérésnél hello aktuális írási régióban kell kézbesíteni.

### <a name="using-hello-sdk"></a>Hello SDK használatával

Például a .NET SDK hello, hello `ConnectionPolicy` hello paramétere `DocumentClient` konstruktor hívása tulajdonsággal rendelkezik `PreferredLocations`. Ez a tulajdonság beállítható tooa régió nevek listája. hello megjelenített neveket a [Azure-régiókat] [ regions] részeként adható meg `PreferredLocations`.

> [!NOTE]
> hello URL-címek hello végpontok hosszú élettartamú állandóként nem tekinthető. hello szolgáltatás ezek bármikor előfordulhat, hogy frissíteni. hello SDK automatikusan kezeli ezt a módosítást.
>
>

```cs
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

// connect tooAzure Cosmos DB
await docClient.OpenAsync().ConfigureAwait(false);
```

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

