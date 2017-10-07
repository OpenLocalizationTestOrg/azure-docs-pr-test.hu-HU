---
title: aaaPerformance tippek - Azure Cosmos DB NoSQL |} Microsoft Docs
description: "Ismerje meg az ügyfél konfigurációs beállítások tooimprove Azure Cosmos DB adatbázis teljesítménye"
keywords: "Hogyan tooimprove adatbázis-teljesítmény"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 94ff155e-f9bc-488f-8c7a-5e7037091bb9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: mimig
ms.openlocfilehash: 4f3e82ae5048e3dbc20b0fd891a2d3aa22adf3fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tips-for-azure-cosmos-db"></a>Teljesítmény tippek az Azure Cosmos DB rendszerhez
Azure Cosmos-adatbázis egy gyors és rugalmas elosztott adatbázis, amely zökkenőmentesen méretezi, garantált késéssel és átviteli sebesség. Nem toomake fő architektúra változásai vagy írási komplex kódot tooscale Cosmos DB az adatbázisokat. Felfelé és lefelé skálázás be lehető legkönnyebben egyetlen API-hívás vagy [SDK metódus hívása](set-throughput.md#set-throughput-sdk). Azonban mivel Cosmos DB keresztül érhető el a hálózati hívások nem ügyféloldali optimalizálás tooachieve csúcsteljesítmény végezheti el.

Ezért ha még kérése "Hogyan javítható az adatbázis teljesítménye?" Vegye figyelembe az alábbi beállítások hello:

## <a name="networking"></a>Hálózat
<a id="direct-connection"></a>

1. **Kapcsolat házirend: közvetlen kapcsolat mód használata**

    Hogyan csatlakozzon az ügyfél a tooCosmos DB rendelkezik fontos gyakorolt hatása a teljesítményre, különös tekintettel az megfigyelt ügyféloldali késés. Nincsenek elérhető az ügyfél kapcsolatkezelési házirendet – hello kapcsolat konfigurálása két fő konfigurációs beállítások *mód* és hello [kapcsolat *protokoll*](#connection-protocol).  hello két elérhető módok a következők:

   1. Átjáró mód (alapértelmezés)
   2. Közvetlen mód

      Átjáró mód minden SDK-platformon támogatott, és hello konfigurált alapértelmezett.  Ha az alkalmazás fut a vállalati hálózatról szigorú tűzfal korlátozások, a átjáró mód nem hello legjobb választás, mivel hello szabványos HTTPS-port és egy végpontot. hello teljesítmény kompromisszumot, azonban, hogy az átjáró mód érint egy további hálózati ugrások minden alkalommal, amikor adatait olvasása vagy írása tooCosmos DB. Emiatt a közvetlen üzemmódban miatt toofewer hálózati ugrásokat jobb teljesítményt nyújt.
<a id="use-tcp"></a>
2. **Kapcsolat házirend: hello TCP protokoll**

    Ha közvetlen módot, két módon protokoll érhető el:

   * TCP
   * HTTPS

     Cosmos DB nyújt egy egyszerű, és nyissa meg a RESTful programozási modellt HTTPS-KAPCSOLATON keresztül. Emellett kínál egy hatékony TCP protokoll, amely egyben a RESTful a kommunikációs modellben és hello .NET ügyfél SDK keresztül érhető el. Közvetlen TCP és a HTTPS egyaránt SSL használata a kezdeti hitelesítési és titkosítási forgalom. A legjobb teljesítmény érdekében használjon hello TCP protokollt, ha lehetséges.

     Ha TCP az átjáró módban, TCP 443-as Port hello Cosmos DB port, továbbá 10255 hello MongoDB API port. Továbbá toohello átjáró portok, közvetlen módban a TCP használatakor portra van szüksége tooensure hello 10000 és 20000 között nyitva, mert a Cosmos DB dinamikus TCP-portot használja. Ha ezeket a portokat nem nyitva, és megpróbál toouse TCP, hibaüzenet 503-as szolgáltatás nem érhető el.

     hello csatlakozási mód során hello konstrukció hello DocumentClient példány hello ConnectionPolicy paraméterrel van konfigurálva. Közvetlen mód használata esetén hello protokollt is hello ConnectionPolicy paraméter belül állítható be.

    ```C#
    var serviceEndpoint = new Uri("https://contoso.documents.net");
    var authKey = new "your authKey from hello Azure portal";
    DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
    new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    });
    ```

    TCP csak akkor támogatott a közvetlen mód, ha átjáró módot használ, mert hello mindig van HTTPS protokollt használja az átjáró hello toocommunicate és hello protokollértéket hello ConnectionPolicy a rendszer figyelmen kívül hagyja.

    ![Hello Azure Cosmos DB kapcsolatkezelési házirendet ábrája](./media/performance-tips/connection-policy.png)

3. **OpenAsync tooavoid indítási késleltetés hívja az első kérésre**

    Alapértelmezés szerint hello első kérelem egy nagyobb késleltetéssel járhat van, mert toofetch hello cím útválasztási táblázathoz. az indítási késleltetés a hello először igényelnie, meg kell hívni OpenAsync() egyszer az inicializálás során az alábbiak szerint tooavoid.

        await client.OpenAsync();
   <a id="same-region"></a>
4. **A teljesítmény azonos Azure régióban ügyfelek elhelyezésének engedélyezése**

    Ha lehetséges, Cosmos DB hívja alkalmazások és ugyanabban a régióban hello hely hello Cosmos DB adatbázisban. Hozzávetőleges összehasonlításhoz meghívja a 1-2 ms, de hello késés hello Nyugat és az USA hello keleti part közötti belül végrehajtani ugyanabban a régióban hello belül DB tooCosmos > 50 ms. A késés valószínűleg kérelem toorequest hello kérés hajtja végre, mivel a hello ügyfél toohello Azure datacenter határ haladnak hello útvonal függően változhat. hello legalacsonyabb lehetséges késést hello hívása biztosításával valósul alkalmazást hello belül helyezkedik el egy Azure-régióban van hello kiépített Cosmos DB végpont. Elérhető régiók listáját lásd: [Azure-régiókat](https://azure.microsoft.com/regions/#services).

    ![Hello Azure Cosmos DB kapcsolatkezelési házirendet ábrája](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
5. **Növeli a szálak/feladatok száma**

    Hívások tooAzure Cosmos DB hello hálózaton keresztül történik, mert szükség lehet a kérelmek párhuzamossági toovary hello fok, hogy az ügyfélalkalmazás hello fordít várakozó kérelmek között nagyon rövid ideig. Ha például használata. NET tartozó [feladat párhuzamos könyvtár](https://msdn.microsoft.com//library/dd460717.aspx), 100-as egység feladatok olvasása vagy írása tooCosmos DB hello sorrendben létrehozása.

## <a name="sdk-usage"></a>SDK használata
1. **Telepítse legújabb SDK hello**

    hello Cosmos DB SDK-k folyamatosan folyamatban van a továbbfejlesztett tooprovide hello legjobb teljesítmény érdekében. Lásd: hello [Cosmos DB SDK](documentdb-sdk-dotnet.md) lapok toodetermine hello legutóbbi SDK, és tekintse át a fejlesztései.
2. **Egypéldányos Cosmos DB ügyfél használata a hello élettartama**

    Vegye figyelembe, hogy minden DocumentClient példány szálbiztos, és végrehajtja a hatékony kapcsolat kezelése és a címek gyorsítótárazása, ha közvetlen módban működik. tooallow hatékony kapcsolat kezelése és a jobb teljesítmény DocumentClient által ajánlott toouse egyetlen példány futhat egy AppDomain DocumentClient hello hello élettartama.

   <a id="max-connection"></a>
3. **Növelhető a System.NET névtérbeli MaxConnections állomásonként, ha az átjáró módban**

    Cosmos DB kérelmek átjáró üzemmódban HTTPS/REST keresztül történik, és vetni toohello alapértelmezett kapcsolat felső határ az egyes állomásnév vagy IP-címét. Így hello ügyféloldali kódtár nyújthatnak több egyidejű kapcsolatok tooCosmos DB szükség lehet tooset hello MaxConnections tooa magasabb érték (100-1000). A .NET SDK 1.8.0 hello és újabb verzióiban hello alapértelmezett értéke [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) van 50 és toochange hello érték, beállíthatja a hello [Documents.Client.ConnectionPolicy.MaxConnectionLimit ](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx) tooa nagyobb értéket.   
4. **A particionált gyűjtemények párhuzamos lekérdezések hangolása**

     A DocumentDB .NET SDK verzió 1.9.0 és támogatási párhuzamos lekérdezések, amelyek lehetővé teszik egy particionált gyűjtemény párhuzamosan tooquery fent (lásd: [hello SDK-k használata](documentdb-partition-data.md#working-with-the-azure-cosmos-db-sdks) és a kapcsolódó hello [Kódminták](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) a További információ:). Párhuzamos lekérdezések a tervezett tooimprove lekérdezés-késleltetés és a teljesítményt a soros megfelelőjére keresztül történik. Párhuzamos lekérdezések két paramétert adja meg, hogy felhasználók észlelheti a toocustom méretezése rájuk vonatkozó követelményeket, a MaxDegreeOfParallelism: toocontrol hello több partíció majd lekérdezhetők, és (b) MaxBufferedItemCount: toocontrol hello száma előre lehívott eredmények.

    (a) ***hangolása MaxDegreeOfParallelism\: *** párhuzamos lekérdezés works több partíció párhuzamosan lekérdezésével. Azonban az egyes particionált összegyűjtendő adatok beolvassa Feladattervek tekintetben toohello lekérdezéssel. Igen beállítás hello MaxDegreeOfParallelism toohello partíciók száma rendelkezik hello maximális esélyét elérése érdekében a legtöbb performant lekérdezés hello, megadott összes többi rendszer feltételei továbbra is hello azonos. Ha nem ismeri a partíciók számának hello, beállíthatja a hello MaxDegreeOfParallelism tooa nagy száma, és hello rendszer hello minimális (partíciók, felhasználó által megadott bemeneti száma) úgy dönt, mivel hello MaxDegreeOfParallelism.

    Fontos, hogy a párhuzamos lekérdezések által előállított hello legjobb előnyöket, ha hello adatok egyenletesen vannak elosztva a tekintetben toohello lekérdezéssel összes partíciójára toonote. Ha hello particionált gyűjtemény particionálása oly módon, hogy az összes vagy egy lekérdezés által visszaadott hello adatok többsége néhány partíciók (egy partíciót a legrosszabb esetben) a koncentrált van, majd hello teljesítmény hello lekérdezés volna kell szűk keresztmetszetét adja ki ezeket a partíció.

    (b) ***hangolása MaxBufferedItemCount\: *** párhuzamos lekérdezések tervezett toopre-fetch eredményeket addig, amíg a hello aktuális köteg eredmények által feldolgozott hello ügyfél. hello lehívását segíti a lekérdezések teljes késést javítása. MaxBufferedItemCount hello paraméter toolimit hello előre lehívott eredmények száma. Beállítás MaxBufferedItemCount toohello várt, száma eredményének (vagy magasabb azonosítószámot) lehetővé teszi, hogy a hello lekérdezés tooreceive maximális hasznos lehívását.

    Ne feledje, hogy előre lekérdezésekor works hello azonos módon megadásától hello MaxDegreeOfParallelism, az összes partíció hello adatokat egyetlen puffert van.  
5. **Kiszolgálóoldali GC bekapcsolása**

    Bizonyos esetekben segíthet a szemétgyűjtés hello gyakoriságának csökkentését. A .NET, állítsa be [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) tootrue.
6. **Leállítási RetryAfter időközönként megvalósítása**

    Teljesítmény tesztelés során terhelés csak a kis mértékben kérelmek get halmozódni növelje meg. Ha szabályozva, hello ügyfélalkalmazás kell a kiszolgáló által megadott hello a késleltetési leállítási újrapróbálkozási időköz. Hello leállítási tiszteletben biztosítja, hogy az újrapróbálkozások közötti várakozási idő a lehető legrövidebb idő. Újrapróbálkozási házirend támogatása a rendszer része, a verzió 1.8.0 és az újabb a hello DocumentDB [.NET](documentdb-sdk-dotnet.md) és [Java](documentdb-sdk-java.md), 1.9.0 verzió vagy újabb verzió a hello [Node.js](documentdb-sdk-node.md) és [ Python](documentdb-sdk-python.md), és az összes támogatott verziója hello [.NET Core](documentdb-sdk-dotnet-core.md) SDK-k. További információkért lásd: [Exceeding fenntartott átviteli sebességének korlátai](request-units.md#RequestRateTooLarge) és [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
7. **Horizontális felskálázás az ügyfél-alkalmazások és szolgáltatások**

    Ha nagy átviteli szinten teszteli (> 50 000 RU/mp), hello ügyfélalkalmazás hello szűk toohello gép határértékét el a Processzor- vagy hálózati kihasználtsága miatt válhat. Ha eléri ezt a pontot, folytathatja a toopush hello Cosmos DB fiók további által az ügyfélalkalmazások kiterjesztése több kiszolgáló között.
8. **Gyorsítótár-dokumentum URI-azonosítók az alsó olvasási késleltetés**

    Gyorsítótár-dokumentum URI-k, amikor csak lehetséges hello olvasási teljesítménye.
   <a id="tune-page-size"></a>
9. **A jobb teljesítmény lekérdezések/olvasás hírcsatornák hello lapmérete hangolása**

    Olvasási hírcsatorna funkciók (például ReadDocumentFeedAsync) használó dokumentumokat, vagy a tömeges végrehajtása olvasási, a DocumentDB SQL-lekérdezést kiállításához, hello eredményeinek szegmentált módon ha hello eredménykészlet túl nagy. Alapértelmezés szerint eredményeinek 100 elemet vagy 1 MB adattömböket, bármelyik korlát találati első.

    tooreduce hello száma hálózati szükséges adatváltások tooretrieve vonatkozó összes eredményt, hello oldalméret x-ms-maximális elem-darabszám kérelem fejléc tooup too1000 használatával növelhető. Azokban az esetekben, ahol kell toodisplay, csak néhány eredményeket például a felhasználói felület vagy a kérelem API visszatérési értéke csak 10 egyszerre annak az eredménye, hello oldal méretét too10 tooreduce hello átviteli Olvasás, mind a lekérdezések felhasznált is csökkentheti.

    Hello oldalméret állíthatók elérhető Cosmos DB SDK-k használatával hello.  Példa:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **Növeli a szálak/feladatok száma**

    Lásd: [szálak/feladatok számának növelése](#increase-threads) hello hálózatkezelés szakaszban található.

11. **Használja a 64 bites állomás feldolgozása**

    a DocumentDB .NET SDK 1.11.4 verzió használata esetén, illetve újabb hello DocumentDB SDK-t állomás 32 bites folyamatként működik. Azonban használatakor alhálózatok közötti partíció lekérdezések, 64 bites állomás feldolgozása ajánlott javítja a teljesítményt. a következő típusú alkalmazásokat hello van 32 bites gazda hello alapértelmezett dolgozni, így ahhoz, hogy too64 bites toochange, a kövesse az alábbi lépéseket az alkalmazás hello típusától függően:

    - Végrehajtható alkalmazások esetében ezt úgy teheti eszközhitelesítést hello **előnyben részesítik a 32 bites** hello beállítása **projekt Tulajdonságok** ablakban a hello **Build** fülre.

    - VSTest alapú teszt projektek, ehhez kiválasztásával **tesztelése**->**beállítások ellenőrzése**->**X64 processzorarchitektúrát alapértelmezett**, a hello **Visual Studio Test** menüjét.

    - Helyileg telepített ASP.NET webes alkalmazásokhoz, ehhez hello ellenőrzésével **használata hello 64-bit-es verzió az IIS Express a webhelyek és projektek**a **eszközök** ->  **Beállítások**->**projektek és megoldások**->**webes projektek**.

    - ASP.NET webes alkalmazásokhoz, Azure rendszerbe, ehhez hello kiválasztásával **64 bites platformok** a hello **Alkalmazásbeállítások** a hello Azure-portálon.

## <a name="indexing-policy"></a>Indexelési házirendet
1. **Használja a Lusta indexelő adatfeldolgozást díjszabás gyorsabb maximális idő**

    Cosmos DB toospecify – hello adatgyűjtési szint – az indexelési házirendet, amely lehetővé teszi toochoose Ha azt szeretné, hogy egy gyűjtemény toobe, vagy nem automatikusan indexelt dokumentumok hello segítségével.  Ezenkívül azt is választhatnak (Consistent) szinkron és aszinkron (Lazy) index frissítések. Alapértelmezés szerint hello index frissítése szinkron módon minden insert, replace vagy dokumentum toohello gyűjtemény törlése. Szinkron módon mód lehetővé teszi, hogy hello lekérdezések toohonor hello azonos [konzisztenciaszint](consistency-levels.md) legyen, mint hello dokumentum beolvasása nélkül hello index késedelmes túl "szinkronizálásához".

    A lusta indexelési forgatókönyvek, amelyben adatot ír a felszakadásáig tekinthetők, és szeretné tooamortize hello szükséges munka tooindex tartalom egy hosszabb időtartamra vonatkozóan. A lusta indexelési is lehetővé teszi a toouse a hatékonyan kiosztott átviteli sebesség és a minimális késéssel szolgálnak írási az csúcsidőben kérelmek. Fontos toonote, azonban, hogy ha Lusta indexelő engedélyezve van, lekérdezés eredményei idővel konzisztenssé hello Cosmos DB fiókok hello konzisztenciaszint függetlenül is.

    Azt eredményezi, ezért konzisztens indexelési mód (IndexingPolicy.IndexingMode tooConsistent beállítása) hello legmagasabb kérelem egység kell fizetni írásonkénti közben (IndexingPolicy.IndexingMode tooLazy beállítása) módot, és nem indexelő indexelő Lazy azok háromszorosa (IndexingPolicy.Automatic beállítása tooFalse) nulla indexelési költség rendelkezik írási hello időpontjában.
2. **Nem használt elérési utak kizárása a gyorsabb írási az indexelő**

    Cosmos DB tartozó indexelési házirendet is lehetővé teszi-dokumentum elérési utak tooinclude, vagy zárja ki a indexelő elérési utakon található (IndexingPolicy.IncludedPaths IndexingPolicy.ExcludedPaths), ami indexelő toospecify. az elérési utak indexelési hello használata kínálhat javított teljesítménye és alacsonyabb index tárolási mely hello lekérdezési mintáinak ismert előzetesen, amennyi az indexelő költségek közvetlenül forgatókönyvek korrelált indexelt egyedi elérési utak toohello száma.  Például hello a következő kód bemutatja, hogyan tooexclude egy teljes szakaszáról hello dokumentumok (más néven egy részfája) használatával hello indexelésének "*" helyettesítő karakter.

    ```C#
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    További információkért lásd: [Azure Cosmos DB házirendek indexelő](indexing-policies.md).

## <a name="throughput"></a>Teljesítmény
<a id="measure-rus"></a>

1. **Mérését meg és hangolható alacsonyabb kérelem egység/másodperc kihasználtsága**

    Cosmos DB kínál széles választéka, beleértve a felhasználó által megadott függvények, tárolt eljárások és eseményindítók – hello dokumentumok adatbázis gyűjteményben lévő összes működési hierarchikus és relációs lekérdezések az adatbázis-művelet. e műveletek társított hello költség hello CPU IO és számára szükséges memória toocomplete hello művelet függ. Továbbléphetnek és hardver-erőforrások kezelése helyett tulajdonképpen egy kérelem egységet (RU) hello erőforrások szükséges tooperform az egyetlen intézkedésként különböző adatbázis-műveletek és a szolgáltatás egy alkalmazás kérelme.

    [Kérelem egységek](request-units.md) minden egyes megvásárolt kapacitásegység hello számú adatbázis fiókjához tartozó törlődnek. Kérelem egység fogyasztás másodpercenkénti történik. Alkalmazások, amelyek mérete meghaladja a hello kiosztott kérelem egység értékelje a fiókjuk korlátozva, amíg hello arány szint fenntartott hello fiók hello alá süllyed. Ha az alkalmazás egy magasabb szintű teljesítmény, vásárolhat további kapacitást egység.

    a lekérdezés hello összetettsége hatással van kérelem egységek művelet végrehajtásánál. predikátumok hello száma, hello predikátumok, felhasználó által megadott függvények száma, valamint hello forrás adatkészlet összes befolyásolják hello költségét hello mérete jellege lekérdezési műveletek.

    toomeasure hello terhelését, semmilyen művelet (létrehozása, frissítése vagy törlése), hello x-ms-kérelem-kell fizetni fejléc vizsgálata (vagy egyenértékű RequestCharge tulajdonság ResourceResponse hello<T> vagy FeedResponse<T> a .NET SDK hello) toomeasure Ezeket a műveleteket által felhasznált kérelemegység hello száma.

    ```C#
    // Measure hello performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure hello performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    hello vissza ezt a fejlécet, kérelmet kell fizetni az részét a létesített átviteli sebesség (azaz 2000 RUs / másodperc). Például ha hello előző lekérdezés függvény 1000 1KB-dokumentumok, hello művelet hello költségét érték 1000. Ilyen belül egy második, hello kiszolgáló eleget tegyen csak két ilyen kérelmeket előtt szabályozás későbbi kérelmeket. További információkért lásd: [egységek kérelem](request-units.md) és hello [kérelem egység Számológép](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Kezeli a sebesség korlátozása/kérelmek aránya túl nagy**

    Amikor az ügyfél tooexceed hello fenntartott átviteli egy olyan fiók, nincs teljesítmény csökkenése nélkül működhet hello kiszolgálón és átviteli sebesség fenntartott hello szint nincs használatban. hello server megelőző jelleggel véget ér hello kérelem RequestRateTooLarge (HTTP-állapotkód: 429) és a visszatérési hello x-ms-újrapróbálkozási-után-ms-fejléc jelző idő, ezredmásodpercben, felhasználói hello hello mennyiségét kell várnia, megoldódhat hello kérelem előtt.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    hello SDK-k minden implicit módon dolgozza fel ezt a választ, figyelembe vegyék hello kiszolgáló által megadott újrapróbálkozási után fejlécet, és próbálja megismételni a hello kérelem. Kivéve, ha a fiók egyszerre több ügyfél által is hozzáférnek, hello legközelebbi újrapróbálkozás sikeres lesz.

    Ha egynél több ügyfél felett hello kérelmek aránya következetesen összesítve működő, hello alapértelmezett újrapróbálkozások száma jelenleg nem elegendők set too9 hello ügyfél által belsőleg; Ebben az esetben a hello ügyfél egy DocumentClientException alkalmazással állapot kód 429-es jelű toohello jelez. beállítás hello RetryOptions hello ConnectionPolicy példányon módosíthatja hello alapértelmezett újrapróbálkozások maximális számát. Alapértelmezés szerint hello DocumentClientException állapotkóddal 429 esetén visszaadott 30 másodperc halmozódó várakozási idő után hello kérés továbbra is toooperate fent hello kérelmek aránya. Ez akkor fordul elő, még ha hello aktuális újrapróbálkozások száma nem éri el hello maximális újrapróbálkozások maximális számát, azt hello alapértelmezett 9 vagy egy felhasználó által definiált értéket.

    Amíg hello automatikus újrapróbálkozásra segít tooimprove rugalmasság és a használhatóság hello a legtöbb alkalmazást, előfordulhat, hogy térjen bármikor odds teljesítménymutatókat, különösen ha mérési késleltetés során.  hello ügyfél megfigyelt késés lefoglalását lesz, ha hello kísérlet hello server késleltetési találatok, és leállítja a hello ügyfél SDK toosilently újra. tooavoid késleltetés napra teljesítmény kísérletek során minden művelet által visszaadott mérték hello kell fizetni, és győződjön meg arról, hogy a kérelmek működnek-e alatt hello fenntartott kérelmek aránya. További információkért lásd: [egységek kérelem](request-units.md).
3. **Nagyobb átviteli sebesség eléréséhez kisebb dokumentumok kialakítása**

    hello kérelem költség (azaz Kérelemfeldolgozás költség) egy adott művelet van közvetlenül korrelált hello dokumentum toohello méretét. Műveletek nagy dokumentumok drágább, mint műveletek kis dokumentumokhoz.

## <a name="next-steps"></a>Következő lépések
Egy minta használt alkalmazás tooevaluate Cosmos DB nagy teljesítményű forgatókönyvek néhány ügyfélszámítógépekre, lásd: [teljesítmény és méretezhetőség Cosmos DB tesztelték](performance-testing.md).

Azt is ellenőrizze, a méretezés és magas szintű teljesítmény, az alkalmazás több tervezéséről toolearn [particionálás és az Azure Cosmos Adatbázisba skálázás](partition-data.md).
