---
title: "aaaAzure Cosmos DB .NET SDK & erőforrások |} Microsoft Docs"
description: "Tudnivalók az hello .NET API és az SDK kiadási dátum, használatból való kivonást dátumok és módosítások hello Azure Cosmos DB .NET SDK verziói között."
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 8e239217-9085-49f5-b0a7-58d6e6b61949
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/11/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0ec30e0130067a9b8d4c9176cf7465bac8925bf9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-net-sdk-download-and-release-notes"></a>Az Azure DB .NET SDK Cosmos: Töltse le és kibocsátási megjegyzések
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

<tr><td>**SDK letöltése**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>

<tr><td>**API-JÁNAK dokumentációja**</td><td>[.NET API-referenciadokumentáció](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Példák**</td><td>[.NET-Kódminták](documentdb-dotnet-samples.md)</td></tr>

<tr><td>**Első lépések**</td><td>[Ismerkedés az Azure Cosmos DB .NET SDK hello](documentdb-get-started.md)</td></tr>

<tr><td>**Webes alkalmazás oktatóanyag**</td><td>[Webalkalmazás fejlesztése a Azure Cosmos DB](documentdb-dotnet-application.md)</td></tr>

<tr><td>**Aktuális támogatott keretrendszer**</td><td>[Microsoft .NET-keretrendszer 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Kibocsátási megjegyzések

### <a name="a-name11701170"></a><a name="1.17.0"/>1.17.0 

* Mint a lekérdezési eredmények tooa adott partíciótartomány kulcs-érték hatókörének egy FeedOption PartitionKeyRangeId támogatása. 
* Egy ChangeFeedOption toostart időnek az elteltével hello módosítások keres, StartTime támogatása.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Rögzített hello JsonSerializable osztály, amely a verem túlcsordulás kivételt okozhat problémát.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
*   Megtörtént egy probléma javítása újrafordítás hello alkalmazásra, amely egy nem kötelező paraméter a hello DocumentClient konstruktor JsonSerializerSettings toohello bevezetése miatt a szükséges.
* Megjelölt hello DocumentClient konstruktor elavult, amely szükséges JsonSerializerSettings ConnectionPolicy és ConsistencyLevel paraméterek alapértelmezett értékét az utolsó paraméter tooallow hello történő átadásakor JsonSerializerSettings paraméterben.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
*   Támogatása az egyéni JsonSerializerSettings megadása közben példányának [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name11411141"></a><a name="1.14.1"/>1.14.1
*   Rögzített x64 érintő problémát gépek, amelyek nem támogatják a SSE4 utasítást, és egy SEHException throw Azure Cosmos DB DocumentDB API lekérdezések futtatásakor.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*   Egy új konzisztenciaszint támogatása ConsistentPrefix nevezik.
*   Az egyes partíciók lekérdezési metrikák támogatása.
*   Támogatja a hello folytatási kód lekérdezések hello mérete korlátozza.
*   Sikertelen kérelmek nyomkövetése részletesebb támogatása.
*   Hello SDK végzett néhány teljesítménynövekedést.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* Gyakorlatilag ugyanaz, mint a 1.13.3. Néhány belső módosítja.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* Gyakorlatilag ugyanaz, mint a 1.13.2. Néhány belső módosítja.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Megtörtént egy probléma javítása, amelyik mellőzve hello PartitionKey értéket összesítő lekérdezések FeedOptions megadott.
* Megtörtént egy probléma javítása átlátszó kezelésében, partíció felügyeleti közepes repülési kereszt-partíció Order By lekérdezés végrehajtása közben.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* Rögzített egyes hello aszinkron API-k használatakor az ASP.NET környezet belül holtpont okozó problémát.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Kijavítja a toomake SDK több rugalmas tooautomatic feladatátvételi bizonyos körülmények között.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Javítsa ki a WebException alkalmanként okozó hibát: hello távoli név nem sikerült feloldani.
* Hozzáadott hello támogatása típusos dokumentumok közvetlenül olvasásakor új túlterhelések tooReadDocumentAsync API hozzáadásával.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* LINQ támogatása (COUNT, MIN, MAX, SUM és átlagos) összesítési lekérdezések.
* Javítsa ki a hello ConnectionPolicy objektum eseménykezelő hello használata okozza a problémát.
* Javítsa ki a hibát, amelynek UpsertAttachmentAsync lett nem működik ETag használatakor.
* Javítsa ki a hibát, amelynek több partíciót az order by lekérdezés folytatási nem dolgozott rendezéskor karakterláncmező.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Összesítési lekérdezéseket (COUNT, MIN, MAX, SUM és átlagos) támogatása. Lásd: [összesítési támogatási](documentdb-sql-query.md#Aggregates).
* A particionált gyűjtemények 10,100 RU/mp too2500 RU/mp a minimális átviteli szintűre csökkent.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* Egy hiba, amelynek hello kereszt-partíció lekérdezések némelyike nem került sor a hello 32 bites gazdafolyamat javítása.
* Javítsa ki a hibát, amelynek hello munkamenet tároló nem frissítése hello tokenhez tartozó átjáró módban.
* Javítsa ki a hibát, amelynek UDF hívások leképezése a lekérdezés nem működött, bizonyos esetekben.
* Ügyfél oldali teljesítmény javítását hello növelése írási és olvasási hello kapacitásának.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Javítsa ki a hibát, amelynek hello munkamenet tároló nem frissítése a sikertelen kérelmek hello tokenhez.
* A 32 bites gazdafolyamat hello SDK toowork támogatása. Vegye figyelembe, hogy több partíció lekérdezések használatakor, 64 bites állomás feldolgozása ajánlott javítja a teljesítményt.
* Javítja a teljesítményt nagy számú partíciós kulcs értékét IN kifejezésben lekérdezések érintő forgatókönyvek.
* Hello ResourceResponse a különböző erőforrás kvótájának statisztikák töltődik fel tagokkal, a PopulateQuotaInfo kérelem beállítás kiolvasni kérelmek dokumentumgyűjteményt.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* Hello 1.11.0 bevezetett CreateDocumentCollectionIfNotExistsAsync API kisebb teljesítményének javítása.
* Teljesítmény – javítás az SDK hello az egyidejű kérelmek magas fokú forgatókönyveit.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Új osztályok és módszerek tooprocess hello támogatása [adatcsatorna módosítása](change-feed.md) dokumentumok egy gyűjteményen belül.
* Kereszt-partíciólekérdezés folytatási és néhány teljesítmény fejleszthetjük tovább a kereszt-partíció lekérdezések támogatása.
* CreateDatabaseIfNotExistsAsync és CreateDocumentCollectionIfNotExistsAsync metódusok hozzáadásával.
* LINQ rendszer funkciók támogatása: IsDefined, IsNull és IsPrimitive.
* Javítsa ki az automatikus binplacing Microsoft.Azure.Documents.ServiceInterop.dll és DocumentDB.Spatial.Sql.dll szerelvények tooapplication a bin mappa project.json tooling rendelkező projektek hello Nuget-csomag használata esetén.
* Támogatja az ügyfél oldali ETW nyomkövetési adatokat a hibakeresés forgatókönyvek hasznosak lehetnek, amelyek kibocsátó.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* A particionált gyűjtemények hozzáadott közvetlen kapcsolat támogatása.
* A kötött elavulási konzisztencia szint hello teljesítménye javult.
* A hozzáadott sokszög és LineString adattípusok indexelő házirend geokerítések térbeli lekérdezéseket a gyűjtemény meg.
* LINQ támogatása StringEnumConverter, IsoDateTimeConverter és UnixDateTimeConverter predikátumok részére történő lefordításakor.
* Különböző SDK hibajavításokat tartalmaz.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Megtörtént egy probléma javítása, hogy a következő NotFoundException okozta hello: hello olvassa el a munkamenet nem érhető el hello bemeneti munkameneti jogkivonat. Ez a kivétel történt bizonyos esetekben hello olvasás-régió, egy földrajzilag elosztott fiók lekérdezésekor.
* Elérhetőségi hello hello ResourceResponse osztályt, amely lehetővé teszi, hogy a közvetlen hozzáférést toohello alapul szolgáló adatfolyamon választ a ResponseStream tulajdonságot.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Módosított hello ResourceResponse, FeedResponse, StoredProcedureResponse és MediaResponse osztályok tooimplement hello megfelelő nyilvános csatoló, így azokat a központi telepítés (TDD) alapú teszteléséhez kell mocked.
* Rögzített egyéni JsonSerializerSettings objektumot a szerializálási adatok használata esetén egy nem megfelelően formázott partíció kulcs fejléc okozó problémát.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Megtörtént egy probléma javítása, ami hosszú ideig futó lekérdezések toofail a következő hiba miatt: engedélyezési jogkivonat érvénytelen hello a jelenlegi időpontnál.
* Rögzített eltávolított problémát hello az eredeti SqlParameterCollection közötti partíció felső/szerint lekérdezések.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Támogatja a particionált gyűjtemények párhuzamos lekérdezések.
* Támogatja a particionált gyűjtemények ORDER BY és a felső lekérdezések partíció közötti.
* Rögzített hello hivatkozások tooDocumentDB.Spatial.Sql.dll és Microsoft.Azure.Documents.ServiceInterop.dll által igényelt való hivatkozáskor Azure Cosmos DB projektben egy hivatkozás toohello Azure Cosmos DB Nuget csomag hiányzik.
* Rögzített hello képességét toouse paraméterek különböző típusú LINQ a felhasználó által definiált függvények használata esetén. 
* Rögzített globálisan replikált fiókok programhiba, ahol alatt álló Upsert hívások volt irányított tooread helyek írási helyek helyett.
* A hozzáadott metódusok toohello IDocumentClient felület, amely hiányzik: 
  * UpsertAttachmentAsync metódus azon definícióváltozatát, mediaStream és paraméterekként beállítások
  * CreateAttachmentAsync metódus azon definícióváltozatát, beállítások, mint egy paraméter
  * CreateOfferQuery metódus, amely querySpec paramétert fogad.
* Lezáratlan nyilvános osztályok, hello IDocumentClient felületen elérhetővé tett tárolókra.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Több területi adatbázis fiókok hozzáadott hello támogatása.
* Próbálkozzon újra a szabályozottan halmozott kérelmek támogatása.  Felhasználói testre szabhatja az újrapróbálkozások számát hello és hello maximális várakozási idő hello ConnectionPolicy.RetryOptions tulajdonság megadásával.
* Egy új IDocumentClient felületet, amely meghatározza az összes DocumenClient tulajdonságai és metódusai hello aláírások hozzá.  Ez a változás részeként is megváltozott, IQueryable és IOrderedQueryable toomethods létrehoz a hello DocumentClient osztály kiterjesztésmetódusok.
* Konfigurációs beállítás tooset hello ServicePoint.ConnectionLimit egy adott Azure Cosmos DB-végpont Uri hozzá.  Használja a ConnectionPolicy.MaxConnectionLimit toochange hello alapértelmezett értéket, amely too50 van beállítva.
* Elavult IPartitionResolver és a megvalósítása.  IPartitionResolver támogatása elavult. Magasabb tárolási és átviteli particionált gyűjtemények használata ajánlott.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Hozzáadott egy túlterhelési tooUri alapú ExecuteStoredProcedureAsync metódus azon definícióváltozatát, RequestOptions paramétert.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Időigényessé toolive (TTL) dokumentumok támogatása.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* A .NET SDK Nuget csomagban programhiba rögzíteni csomagolás egy Azure Cloud Service megoldás részeként.

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* Megvalósított [particionált gyűjtemények](partition-data.md) és [felhasználói teljesítményszintet](performance-levels.md). 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Rögzített]**  Lekérdezése Azure Cosmos DB végpont jelez: "System.Net.Http.HttpRequestException: Hiba történt a tartalom tooa adatfolyam másolása".

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Bővített LINQ támogatja, beleértve az új operátorok lapozási, a feltételes kifejezések és összehasonlító között.
  * Operátor tooenable válasszon felső viselkedés LINQ a hálózatról
  * CompareTo operátor tooenable karakterlánc tartomány módon történő összehasonlítása
  * Feltételes (?) és a coalesce operátorok (?)
* **[Rögzített]**  ArgumentOutOfRangeException modell leképezése egyesítésekor Where-a LINQ lekérdezés. [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Rögzített]**  Ha válassza nincs hello utolsó kifejezés hello LINQ szolgáltatónál feltételezve, hogy nincs leképezés, és válasszon előállított * nem megfelelő.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Megvalósított Upsert, UpsertXXXAsync hozzáadott metódusok
* Minden olyan kérelem esetében a teljesítménnyel kapcsolatos fejlesztések
* LINQ szolgáltatónál támogatása feltételes, coalesce, és karakterláncokat CompareTo módszerei
* **[Rögzített]**  LINQ-szolgáltató--> megvalósítása tartalmazza a lista toogenerate metódusa azonos SQL hello az IEnumerable és tömb
* **[Rögzített]**  BackoffRetryUtility használja ismét egy új újrapróbálkozáskor létrehozása helyett azonos HttpRequestMessage hello
* **[Elavult]**  UriFactory.CreateCollection--> UriFactory.CreateDocumentCollection használja

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Rögzített]**  Honosítási ad ki, ha nem en kulturális környezet adatokkal például nl-NL stb. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Azonosító-alapú útválasztási hozzáadva
  * Az azonosító-alapú erőforrás hivatkozásokat hozhat létre új UriFactory segítő tooassist
  * A DocumentClient tootake az URI új túlterhelések
* A hozzáadott IsValid() és a földrajzi LINQ IsValidDetailed()
* Fokozott LINQ szolgáltatónál támogatják:
  * **Matematikai** -Abs, ARCCOS, ARCSIN, Atan Cos Exp, emelet, napló, Log10, Pow, ciklikus, bejelentkezési, EG, Sqrt, Tan, felső határának Truncate
  * **Karakterlánc** -összefűzés, tartalmazza, megadott módon végződő, IndexOf, Count, ToLower, TrimStart, cserélje le, TrimEnd, a megadott módon kezdődő, a SubString, ToUpper megfordításához
  * **A tömb** -összefűzés, tartalmazza, száma
  * **IN** operátor

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Támogatása az indexelő házirendek módosítása.
  * A DocumentClient új ReplaceDocumentCollectionAsync módszer
  * Új IndexTransformationProgress tulajdonság ResourceResponse<T> index házirend módosításai készültségi állapotának nyomon követése
  * Mostantól változtatható DocumentCollection.IndexingPolicy
* Támogatja a térbeli indexelő és lekérdezés.
  * A térbeli típusok szerializálása/deszerializálása névterét új Microsoft.Azure.Documents.Spatial, például pont és a sokszög
  * A Cosmos DB GeoJSON adataihoz az indexelés új SpatialIndex osztály
* **[Rögzített]**  Helytelen SQL-lekérdezést jön létre egy LINQ kifejezésből [#38](https://github.com/Azure/azure-documentdb-net/issues/38).

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Függőség hozzáadta a Newtonsoft.Json v5.0.7.
* Order By toosupport elvégzett módosításokat:
  
  * Támogatott a LINQ szolgáltatók OrderBy() vagy OrderByDescending()
  * Order By IndexingPolicy toosupport 
    
    **Lehetséges használhatatlanná tévő változást** 
    
    Ha a meglévő kódot, hogy rendelkezések gyűjteményeket, amelyek egyéni indexelési házirendet, a meglévő kódot kell toobe toosupport hello új IndexingPolicy osztály frissítése. Ha még nincs egyéni indexelési házirendet, majd a módosítás nem hatása a felhasználókra.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Támogatása az adatok particionálása hello új HashPartitionResolver és RangePartitionResolver osztályok és hello IPartitionResolver használatával.
* A hozzáadott DataContract szerializálása.
* A hozzáadott GUID támogatja a LINQ szolgáltatónál.
* A hozzáadott UDF LINQ támogatja.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release--retirement-dates"></a>Kiadás & használatból való kivonást dátumok
A Microsoft biztosít értesítési legalább **12 hónapon keresztül** előre kivonása az SDK-t rendelés toosmooth hello átmenet tooa vagy újabb támogatott verzióra.

Új szolgáltatásait és funkcióit és optimalizálás csak hozzáadott toohello aktuális SDK-t, így javasolt, hogy Ön mindig frissítési toohello SDK legújabb lehető leghamarabb. 

A kérelmek tooAzure hello szolgáltatás elutasítja a Cosmos DB kivont SDK használatával.

<br/>

| Verzió | Kiadás dátuma | Kivezetési dátum |
| --- | --- | --- |
| [1.17.0](#1.17.0) |2017. augusztus 10. |--- |
| [1.16.1](#1.16.1) |2017. augusztus 07. |--- |
| [1.16.0](#1.16.0) |2017. augusztus 02. |--- |
| [1.15.0](#1.15.0) |2017. június 30. |--- |
| [1.14.1](#1.14.1) |2017. május 23. |--- |
| [1.14.0](#1.14.0) |2017. május 10. |--- |
| [1.13.4](#1.13.4) |2017. Előfordulhat, hogy 09. |--- |
| [1.13.3](#1.13.3) |2017. Előfordulhat, hogy 06. |--- |
| [1.13.2](#1.13.2) |2017. április 19. |--- |
| [1.13.1](#1.13.1) |2017. március 29. |--- |
| [1.13.0](#1.13.0) |2017. március 24. |--- |
| [1.12.2](#1.12.2) |2017. március 20. |--- |
| [1.12.1](#1.12.1) |2017. március 14. |--- |
| [1.12.0](#1.12.0) |2017. február 15. |--- |
| [1.11.4](#1.11.4) |2017. február 06. |--- |
| [1.11.3](#1.11.3) |2017. január 26. |--- |
| [1.11.1](#1.11.1) |2016. december 21. |--- |
| [1.11.0](#1.11.0) |2016. december 08. |--- |
| [1.10.0](#1.10.0) |2016. Szeptembertől 27. |--- |
| [1.9.5](#1.9.5) |2016 szeptemberétől 01 |--- |
| [1.9.4](#1.9.4) |2016 augusztusától 24 |--- |
| [1.9.3](#1.9.3) |2016 augusztusától 15 |--- |
| [1.9.2](#1.9.2) |2016. július 23. |--- |
| [1.8.0](#1.8.0) |2016. június 14. |--- |
| [1.7.1](#1.7.1) |2016. május 06. |--- |
| [1.7.0](#1.7.0) |2016. április 26. |--- |
| [1.6.3](#1.6.3) |2016. április 08. |--- |
| [1.6.2](#1.6.2) |2016. március 29. |--- |
| [1.5.3](#1.5.3) |2016. február 19. |--- |
| [1.5.2](#1.5.2) |2015. december 14. |--- |
| [1.5.1](#1.5.1) |2015. november 23. |--- |
| [1.5.0](#1.5.0) |2015. október 05. |--- |
| [1.4.1](#1.4.1) |2015. augusztus 25. |--- |
| [1.4.0](#1.4.0) |2015. augusztus 13. |--- |
| [1.3.0](#1.3.0) |2015. augusztus 05. |--- |
| [1.2.0](#1.2.0) |2015. július 06. |--- |
| [1.1.0](#1.1.0) |2015. április 30. |--- |
| [1.0.0](#1.0.0) |2015. áprilisi 08 |--- |


## <a name="faq"></a>GYIK
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Lásd még:
toolearn Cosmos DB kapcsolatos további információkért lásd: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) szolgáltatás lapján. 

