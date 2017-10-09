---
title: "aaaAzure Cosmos DB .NET Core API SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello .NET Core API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB .NET Core SDK verziói között."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1269cafe0ea1caaa871404d507b12632dbb3ed82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-core-sdk-release-notes-and-resources"></a>Az Azure Cosmos DB .NET Core SDK: A kibocsátási megjegyzések és erőforrások
> [!div class="op_single_selector"]
> * [.NET](documentdb-sdk-dotnet.md)
> * [.NET-módosítás adatcsatorna](documentdb-sdk-dotnet-changefeed.md)
> * [.NET Core](documentdb-sdk-dotnet-core.md)
> * [Node.js](documentdb-sdk-node.md)
> * [Java](documentdb-sdk-java.md)
> * [Python](documentdb-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST erőforrás-szolgáltató](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK letöltése**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-JÁNAK dokumentációja**</td><td>[.NET API-referenciadokumentáció](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Példák**</td><td>[.NET-Kódminták](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Első lépések**</td><td>[Ismerkedés az Azure Cosmos DB .NET Core SDK hello](documentdb-dotnetcore-get-started.md)</td></tr>

<tr><td>**Webes alkalmazás oktatóanyag**</td><td>[Webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuális támogatott keretrendszer**</td><td>[.NET szabványos 1.6-os és .NET szabványos 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

hello Azure Cosmos DB .NET Core SDK legújabb verziójával hello hello szolgáltatásparitást rendelkezik [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md).

> [!NOTE] 
> hello Azure Cosmos DB .NET Core SDK még nem kompatibilis az univerzális Windows Platform (UWP-) alkalmazásokat. Ha érdekli hello .NET Core SDK-t támogató UWP-alkalmazások, e-mailek küldése túl[askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com).

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Mint a lekérdezési eredmények tooa adott partíciótartomány kulcs-érték hatókörének egy FeedOption PartitionKeyRangeId támogatása. 
* Egy ChangeFeedOption toostart időnek az elteltével hello módosítások keres, StartTime támogatása. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Rögzített hello JsonSerializable osztály, amely a verem túlcsordulás kivételt okozhat problémát.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Egyéni JsonSerializerSettings megadása közben példányának támogatása egy [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) példány.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   .NET-szabvány 1.5 támogató hello cél keretrendszerek egyik.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Rögzített x64 érintő problémát gépek, amelyek nem támogatják a SSE4 utasítást, és SEHException throw Azure Cosmos DB lekérdezések futtatásakor.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.
*   Az egyes partíciók lekérdezési metrikák támogatása.
*   Támogatja a hello folytatási kód lekérdezések hello mérete korlátozza.
*   Sikertelen kérelmek nyomkövetése részletesebb támogatása.
*   Hello SDK végzett néhány teljesítménynövekedést.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Megtörtént egy probléma javítása, amelyik mellőzve hello PartitionKey értéket összesítő lekérdezések FeedOptions megadott.
* Megtörtént egy probléma javítása átlátszó kezelésében, partíció felügyeleti közepes repülési kereszt-partíció Order By lekérdezés végrehajtása közben.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Rögzített egyes hello aszinkron API-k használatakor az ASP.NET környezet belül holtpont okozó problémát.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Kijavítja a toomake SDK több rugalmas tooautomatic feladatátvételi bizonyos körülmények között.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Javítsa ki a WebException alkalmanként okozó hibát: hello távoli név nem sikerült feloldani.
* Hozzáadott hello támogatása típusos dokumentumok közvetlenül olvasásakor új túlterhelések tooReadDocumentAsync API hozzáadásával.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* LINQ támogatása (COUNT, MIN, MAX, SUM és átlagos) összesítési lekérdezések.
* Javítsa ki a hello ConnectionPolicy objektum eseménykezelő hello használata okozza a problémát.
* Javítsa ki a hibát, amelynek UpsertAttachmentAsync lett nem működik ETag használatakor.
* Javítsa ki a hibát, amelynek több partíciót az order by lekérdezés folytatási nem dolgozott rendezéskor karakterláncmező.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása. Lásd: [összesítési támogatási](documentdb-sql-query.md#Aggregates).
* A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

hello Azure Cosmos DB .NET Core SDK lehetővé teszi a toobuild gyors, platformfüggetlen [ASP.NET Core](https://www.asp.net/core) és [.NET Core](https://www.microsoft.com/net/core#windows) alkalmazások toorun Windows, Mac és Linux rendszeren. hello hello Azure Cosmos DB .NET Core SDK legújabb kiadása van teljesen [Xamarin](https://www.xamarin.com) kompatibilis és használt toobuild alkalmazások, amelyek monó (Linux), iOS és Android.  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

hello Azure Cosmos DB .NET Core Preview SDK lehetővé teszi a toobuild gyors, platformfüggetlen [ASP.NET Core](https://www.asp.net/core) és [.NET Core](https://www.microsoft.com/net/core#windows) alkalmazások toorun Windows, Mac és Linux rendszeren.

hello Azure Cosmos DB .NET Core Preview SDK legújabb verziójával hello hello szolgáltatásparitást rendelkezik [Azure Cosmos DB .NET SDK](documentdb-sdk-dotnet.md) és hello következőket támogatja:
* Minden [csatlakozási mód](performance-tips.md#networking): átjáró módban, a közvetlen TCP és a közvetlen HTTPs. 
* Minden [konzisztenciaszintek](consistency-levels.md): erős, munkamenet, amelyet az elavulási és Eventual.
* [A particionált gyűjtemények](partition-data.md). 
* [Több területi adatbázis fiókok és georeplikáció](distribute-data-globally.md).

Ha kérdése kapcsolódó toothis SDK, post túl[StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), vagy probléma fájlt hello [github-tárházban](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Kiadás & használatból való kivonást dátumok

| Verzió | Kiadás dátuma | Kivezetési dátum |
| --- | --- | --- |
| [1.5.0](#1.5.0) |2017. augusztus 10. |--- | 
| [1.4.1](#1.4.1) |2017. augusztus 07. |--- |
| [1.4.0](#1.4.0) |2017. augusztus 02. |--- |
| [1.3.2](#1.3.2) |2017. június 12. |--- |
| [1.3.1](#1.3.1) |2017. május 23. |--- |
| [1.3.0](#1.3.0) |2017. május 10. |--- |
| [1.2.2](#1.2.2) |2017. április 19. |--- |
| [1.2.1](#1.2.1) |2017. március 29. |--- |
| [1.2.0](#1.2.0) |2017. március 25. |--- |
| [1.1.2](#1.1.2) |2017. március 20. |--- |
| [1.1.1](#1.1.1) |2017. március 14. |--- |
| [1.1.0](#1.1.0) |2017. február 16. |--- |
| [1.0.0](#1.0.0) |2016. december 21. |--- |
| [0.1.0-Preview](#0.1.0-preview) |2016. november 15. |2016. december 31. |

## <a name="see-also"></a>Lásd még:
toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján. 

