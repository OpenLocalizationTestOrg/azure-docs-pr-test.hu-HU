---
title: "Az Azure Cosmos DB kialakítási mintában: közösségi alkalmazások |} Microsoft Docs"
description: "További tudnivalók a kialakítási mintában a közösségi hálózatokkal hello tárolás rugalmasságát Azure Cosmos adatbázis és az egyéb Azure-szolgáltatások használatával."
keywords: "Közösségi alkalmazások"
services: cosmos-db
author: ealsur
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 2dbf83a7-512a-4993-bf1b-ea7d72e095d9
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2017
ms.author: mimig
ms.openlocfilehash: 47a22f2c5762d62b176921c8052e7bd75d8cf6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="going-social-with-azure-cosmos-db"></a>Az Azure Cosmos DB közösségi címen
A nagymértékben összekapcsolt társadalom élő azt jelenti, hogy a életben bármikor lesz része egy **közösségi hálózati**. Használjuk ismerősök, munkatársakat, termékcsalád vagy néha tooshare követheti a közösségi hálózatokkal tookeep nekünk kihívás a közös érdeklődési személyekkel.

Mérnökök és a fejlesztőknek, mint azt előfordulhat, hogy rendelkezik már azon, hogy hogyan lehet ezeket a hálózatokat tárolja és kapcsolódnak össze az adatok, vagy előfordulhat, hogy rendelkezik még lett kiadta toocreate vagy egy adott árkategóriájú új közösségi hálózat rendszerfejlesztők piaci yourselves. Ha ez hello nagy kérdése merül fel: ezek az adatok tárolási módjára?

Tegyük fel, hogy létrehozzuk egy új és csillogó közösségi hálózati, ahol a felhasználók cikkeket,:, képek, videók, vagy még akkor is, zene kapcsolódó adathordozó is közzé. A felhasználók bejegyzéseket fűzni és értékelések pontok biztosítják. Vonatkozó bejegyzéseket, hogy a felhasználók látják, és a hello fő webhelyének kezdőlapja a képes toointeract kell hírcsatorna lesz. Ez valóban összetett nem hangkártya (elsőre), de a hello szakét az egyszerűség, most állítja le van (azt sikerült elmélyedhet egyéni felhasználói hírcsatornák kapcsolatok érinti, de ez a cikk célja hello meghaladja).

Igen hogyan tegye tároljuk ez hol és?

Sokan előfordulhat, hogy rendelkezik tapasztalattal az SQL-adatbázisok vagy legalább fogalmát [relációs adatok modellezési](https://en.wikipedia.org/wiki/Relational_model) és lehetséges, hogy kísértésbe toostart rajzolási például ehhez hasonló:

![A relatív relációs modell ábrázoló diagram](./media/social-media-apps/social-media-apps-sql.png) 

A tökéletesen normalizált és közérthető adatszerkezet... nem méretezhető. 

Nem jelenik meg hibás, I korábban már használt SQL-adatbázisok a élettartama,-e nagy, de minden mintát, gyakorlat és szoftverek platform, például tökéletes nem minden forgatókönyvhöz.

Miért nem SQL hello legjobb választás ebben a forgatókönyvben? Vizsgáljuk meg egyetlen post hello szerkezete, az Excel, a webhelyek vagy alkalmazások tooshow, akkor van toodo tartalmazó lekérdezést... 8 tábla illesztések (!) csak tooshow egy egyetlen post, most dinamikusan betöltött és jelennek meg az üdvözlő képernyőt, és bejegyzéseket adatfolyam tapasztalhatja, ahol fogom kép.

Azt lehetett, természetesen használni humongous SQL-példány elég power toosolve több ezer lekérdezések ezek sok illesztések tooserve a tartalmat, de valóban, miért kellene azt, amikor egy egyszerűbb megoldást létezik?

## <a name="hello-nosql-road"></a>hello NoSQL közúti
Ez a cikk végigvezeti az Azure NoSQL-adatbázis a közösségi platform adatok modellezését [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) miközben használhatja más Azure Cosmos DB szolgáltatások, mint az hello költséghatékony [Gremlin Graph API-val ](../cosmos-db/graph-introduction.md). Használja a [NoSQL](https://en.wikipedia.org/wiki/NoSQL) módszert használja, JSON formátumban adatok tárolására és [denormalization](https://en.wikipedia.org/wiki/Denormalization), a korábban bonyolult post átalakíthatók egyetlen [dokumentum](https://en.wikipedia.org/wiki/Document-oriented_database):


    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["http://myfirstimage.png","http://mysecondimage.png"],
        "videos":[
            {"url":"http://myfirstvideo.mp4", "title":"hello first video"},
            {"url":"http://mysecondvideo.mp4", "title":"hello second video"}
        ],
        "audios":[
            {"url":"http://myfirstaudio.mp3", "title":"hello first audio"},
            {"url":"http://mysecondaudio.mp3", "title":"hello second audio"}
        ]
    }

És egyetlen lekérdezést, és nem csatlakozik szerezhető be. Ez sokkal több egyszerű és magától értetődő, és, budget-wise, kevesebb erőforrást tooachieve legjobb eredmény van szükség.

Az Azure Cosmos DB gondoskodik arról, hogy az összes hello tulajdonság indexelt az automatikus indexeléshez való is lehet, amely [testreszabott](indexing-policies.md). hello sémamentes megközelítés teszi lehetővé másik dokumentumok tárolására és dinamikus struktúrák, talán holnap azt szeretnénk, hogy bejegyzéseket toohave fogja kezelni a kategóriák és a hashtageket társítani a Cosmos DB listája új dokumentumok hello hello a hozzáadott rendelkező nincs további attribútumok Amerikai feladata.

A post fűzött megjegyzések csak más bejegyzéseket (Ez egyszerűbbé teszi az objektum leképezésének) szülő tulajdonsággal is kell kezelni. 

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

És minden közösségi kapcsolati is tárolni egy önálló objektumon számlálókat:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Hírcsatornák létrehozásakor csak egy adott fontossági sorrendben post azonosítók listáját tároló dokumentumok létrehozása:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Azt a létrehozási dátum szerint rendezve bejegyzéseket rendelkezhetnek a "legutóbbi" adatfolyam, levőkkel "hottest" adatfolyam küldi hello további kedveli az elmúlt 24 óra, még az egyes felhasználók például followers és érdeklődési logikán alapuló egyéni adatfolyam sikerült megvalósítása és kell listája  bejegyzéseket. Hogyan toobuild ezek listája kérdése, de hello olvasási teljesítmény akadálytalan marad. Kapni, ha e listák valamelyikébe szerezni, azt egy egyetlen lekérdezés tooCosmos DB használatával hello [OPERÁTORBAN](documentdb-sql-query.md#WhereClause) tooobtain lapok egyszerre közleményeket.

adatcsatorna-adatfolyamok hello segítségével épülhet [Azure App Services](https://azure.microsoft.com/services/app-service/) háttér-folyamatok: [Webjobs](../app-service-web/web-sites-create-web-jobs.md). Ha létrejött a feladás egy vagy több, háttérben történő feldolgozás is elindítható a használatával [Azure Storage](https://azure.microsoft.com/services/storage/) [várólisták](../storage/queues/storage-dotnet-how-to-use-queues.md) és hello segítségével indított Webjobs [Azure Webjobs SDK](../app-service-web/websites-dotnet-webjobs-sdk.md), végrehajtási hello propagálás utáni saját egyéni logika alapuló belül. 

Pontok és a post keresztül kedveli a azonos módszerrel toocreate idővel konzisztenssé környezet használatával késleltetett módon lehet feldolgozni.

Followers bonyolultabb. Cosmos DB méretkorlátja maximális dokumentum, és olvasási/írási nagy dokumentumok kedvezőtlen hatással lehet az alkalmazás hello méretezhetőséget. Ezért előfordulhat, hogy gondolja ennél a módszernél dokumentumként followers tárolására:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

Ez néhány több ezer rendelkező felhasználók esetében is működik, de ha néhány celebrity csatlakozik az egyes holtversenyekben, ezt a megközelítést vezet tooa nagy dokumentum mérete, és előfordulhat, hogy végül találati hello dokumentum mérete cap followers.

toosolve, vegyes megközelítés is használhatók. Hello felhasználói statisztikák dokumentum részeként pedig followers hello száma tárolhatja:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

És followers aktuális grafikonját hello Azure Cosmos DB tárolható [Gremlin Graph API](../cosmos-db/graph-introduction.md), toocreate [csomópontok](http://mathworld.wolfram.com/GraphVertex.html) minden felhasználó és [szélek](http://mathworld.wolfram.com/GraphEdge.html) , amelyek karbantartása hello " A-követi-B"kapcsolatokat. hello Graph API most azt nem csak egy adott felhasználó hello followers beszerzése azonban tooeven javaslat személyek közös összetettebb lekérdezések létrehozása. Ha jelenleg felvenni toohello graph hello tartalom kategóriák, amelyet, például vagy élvez, is először lép, amely tartalmazza az intelligens tartalom felderítési, szövödei tartalom arra vonatkozóan, hogy azokat azt kövesse, például vagy kikkel előfordulhat, hogy tudunk nagy közös személyek keresése.

hello felhasználói statisztikák még lehet használt toocreate kártyák hello felhasználói felületén vagy a gyors profil az előzetes verziójú funkciók a.

## <a name="hello-ladder-pattern-and-data-duplication"></a>"Létra" minta és az adatok párhuzamos hello
Észrevette, hogy a feladás egy vagy több hivatkozó hello JSON-dokumentum található, mert nincsenek a felhasználó többször. És meg szeretné rendelkezik kitalál jobb, ez azt jelenti, hogy több helyen jelen lehet-e a képviseli a felhasználót, a denormalization megadott hello információkat.

A sorrend tooallow gyorsabb lekérdezésekhez azt adat ismétlődik fel Önnek. Ezt a mellékhatást hello gond, ha néhány művelet, a felhasználói adatok változásait, igazolnia kell toofind tevékenységekre hello hogy legalább egyszer volt, és frissítse azokat az összes. Hang nagyon gyakorlati, jobb nem?

Folyamatos toosolve azt azonosításával hello kulcsattribútumoknak egy olyan felhasználó, az alkalmazás minden egyes tevékenységhez megmutatjuk, vagy folyamatban. Ha azt vizuálisan megjeleníteni a feladás egy vagy több alkalmazás, és ebben az esetben hello létrehozó nevét és kép megjelenítése, miért tárolása hello felhasználói adatok hello "hozott létre" attribútum? Minden egyes megjegyzés csak megmutatjuk hello felhasználói kép, ha valóban nem szükséges a adatait hello többi részétől. Ez a valami "létra minta" hello jelentkezem honnan szerepet.

Vegyük példaként a felhasználói adatokat:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Ezek az információk megtekintésével is gyorsan észlelünk fontos információkról, amely így létrehozása, amely nem, "Létra":

![A lépcsőzetes minta ábrája](./media/social-media-apps/social-media-apps-ladder.png)

hello legkisebb lépés neve a UserChunk, hello minimális adat, amely azonosítja a felhasználót, és az adatok párhuzamos használják. Duplikált hello az tooonly hello adatait "bemutatjuk a" hello méretének csökkentésével csökkenteni nagy frissítések hello lehetőségét.

hello középső lépés hello felhasználó neve, a teljes adatok hello Cosmos DB, a legtöbb teljesítmény-függő lekérdezésekben használt hello hozzáférés és a kritikus. Ez magában foglalja a UserChunk hello információt.

legnagyobb hello hello Extended felhasználó. Ez magában foglalja a minden hello kritikus felhasználói adatokat és más adatokat, amelyek nem tényleg szükséges toobe olvasási gyorsan vagy annak használati végleges (például hello bejelentkezési folyamat). Ezek az adatok tárolhatók kívül Cosmos DB, az Azure SQL Database vagy az Azure Storage-táblákat.

Miért ehhez a Microsoft hello felhasználói felosztása és is tárolni ezt az információt a különböző helyeken? Mivel a teljesítmény szempontjából hello nagyobb hello dokumentumok hello costlier hello lekérdezések. Dokumentumok megtartása slim hello a megfelelő információkat toodo minden a teljesítmény-függő lekérdezi a közösségi hálózat és tároló hello végleges helyzetekben, például, a teljes profil módosításokat, a bejelentkezési adatok olyan további információkat, akkor is, használatelemzési információkat és a Big data adatbányászati Adatok kezdeményezések. Valóban nem ügyelünk Ha hello adatgyűjtési adatbányászathoz lassabb, mivel az Azure SQL Database-futtat, igazolnia kell vonatkoznak azonban, hogy a felhasználók rendelkeznek-e olyan gyors és slim környezetet. A felhasználó a Cosmos DB tárolt néz ki:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"@john"
    }

És a Post alábbihoz hasonlóan fog kinézni:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

És szerkesztési merül fel, ahol az hello adattömbök hello attribútumok egyikét van hatással, esetén könnyen toofind hatással hello dokumentumok lekérdezések, amelyek indexelése toohello attribútumok használatával (válasszon * FROM visszaküldés p WHERE p.createdBy.id == "edited_user_id") és majd frissítése hello adattömböket.

## <a name="hello-search-box"></a>hello keresőmezőbe
Felhasználók hoz létre, Szerencsére, nagy mennyiségű tartalmat. Azt tudja tooprovide hello képességét toosearch legyen, és nem történhet közvetlenül a saját tartalom adatfolyamot, lehet, hogy azt ne hajtsa végre a hello creators tartalom található, és lehet, hogy csak keressük toofind 6 hónapon ezelőtt volt azt, hogy régi post.

Thankfully és az Azure Cosmos DB használjuk, mert azt is könnyen létrehozható egy keresési motor használata [Azure Search](https://azure.microsoft.com/services/search/) néhány percig, és egyetlen sor kódot beírása nélkül (eltérő természetesen hello keresést a folyamat és a felhasználói felület).

Miért van szó, ezért az easy?

Az Azure Search alkalmazza azok hívás [indexelők](https://msdn.microsoft.com/library/azure/dn946891.aspx), háttérben dolgozza fel, hogy az a adattárolók hook és automagically hozzáadása, módosítása vagy az objektumok eltávolítását hello indexek. Támogatják-e egy [Azure SQL Database indexelők](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), [Azure BLOB indexelők](../search/search-howto-indexing-azure-blob-storage.md) és thankfully, [Azure Cosmos DB indexelők](../search/search-howto-index-documentdb.md). hello átmenet Cosmos DB tooAzure keresési adatok egyszerű, mint mindkét információk tárolása JSON formátumban, már csak ellenőriznünk kell túl[hozza létre az indexet](../search/search-create-index-portal.md) , és képezze le a dokumentumok melyik attribútumok indexelt szeretnénk, és ennyi, a percek (hello az adatok méretétől függően), a tartalom lesz elérhető toobe keres, hello legjobb keresési,--szolgáltatás megoldás felhőalapú infrastruktúrában. 

Azure Search kapcsolatos további információkért látogasson el hello [Hitchhiker's Guide tooSearch](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/).

## <a name="hello-underlying-knowledge"></a>hello alapul szolgáló Tudásbázis
Után a tartalom, amely növekszik, és minden nap növekedésének tárolása, előfordulhat, hogy találtunk ragozott formáival végezni: milyen feladatokat lehet elvégezni az adatok az adatfolyam a felhasználók?

hello válasz egyszerű: toowork használatba, és ismerje meg, akkor a.

De mi azt megtudhatja? Néhány egyszerű példák [véleményeket elemzés](https://en.wikipedia.org/wiki/Sentiment_analysis), javaslatok a felhasználói beállítások alapján, vagy akár egy automatizált tartalom moderátor, amely biztosítja, hogy az összes hello a közösségi hálózat tartalmait is nyugodtan hello tartalom termékcsalád.

Most, hogy Ön akadályoznia jelent, úgy fogja valószínűleg gondolja, hogy néhány doktori a matematikai tudományos tooextract kell ezeket a mintákat és adatokat egyszerű adatbázisok és a fájlok kívül, de lenne megfelelő.

[Az Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/), hello része [Cortana Intelligence Suite](https://www.microsoft.com/en/server-cloud/cortana-analytics-suite/overview.aspx), hello van egy teljes körűen felügyelt felhőszolgáltatás, amely lehetővé teszi, hogy a munkafolyamatokat algoritmusok használatát egy egyszerű fogd és vidd felületen, a saját algoritmusok code a [R](https://en.wikipedia.org/wiki/R_\(programming_language\)) vagy bizonyos hello már létrehozott, és készen áll, mint toouse API-k: [Szövegelemzések](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [tartalom moderátor](https://www.microsoft.com/moderator) vagy [javaslatok](https://gallery.cortanaanalytics.com/MachineLearningAPI/Recommendations-2).

tooachieve bármely a Machine Learning forgatókönyvekben használhatjuk [Azure Data Lake](https://azure.microsoft.com/services/data-lake-store/) tooingest hello különböző forrásokból származó adatokat, és a [U-SQL](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) tooprocess hello információkat, és egy kimenete amely az Azure Machine Learning által feldolgozott is.

Egy másik elérhető lehetőség az toouse [Microsoft kognitív szolgáltatások](https://www.microsoft.com/cognitive-services) tooanalyze tartalom; csak nem tudja azt értelmezni őket a felhasználók jobban (keresztül elemzése, mi írják a [szöveg Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)), de azt nem sikerült is nemkívánatos vagy érett tartalom észlelése és ennek megfelelően működjön a [számítógép Látástechnológiai API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api). Kognitív szolgáltatások közé tartoznak az out-of-az-box megoldások, amelyek nem igényelnek a Machine Learning Tudásbázis toouse bármilyen típusú nagy.

## <a name="a-planet-scale-social-experience"></a>Egy bolygónk méretű közösségi élmény
A legutóbbi, de nem legalább a témakör fontos kell címezni: **méretezhetőség**. Az architektúrát, rendkívül fontos, hogy minden összetevő méretezheti önállóan, vagy mert tooprocess több adatot kell tervezésekor, vagy mert szeretnénk toohave nagyobb melyek (vagy mindkét!). Thankfully, az összetett feladat elérése egy **kulcsrakész élmény** a Cosmos DB.

Cosmos DB támogatja [dinamikus particionálást](https://azure.microsoft.com/blog/10-things-to-know-about-documentdb-partitioned-collections/) out-of-az-box alapján létrehozott partícióknak automatikusan létrehozásával egy adott **partíciókulcs** (a dokumentumok hello attribútumok egyikét megadva). Megfelelő partíciókulcs elvégzendő tervezési időben hello meghatározását, és tartsa szem előtt tartva hello [ajánlott eljárások](../cosmos-db/partition-data.md#designing-for-partitioning) érhető el; hello esetében egy közösségi felület, a particionálási stratégia egyeztetni kell hello módszert (olvasás lekérdezése hello partícióra kívánatos) írási és olvasási ("interaktív területek" elkerülheti, ha több partíciót az írási műveletek terjednek). Egyes beállítások akkor: a tartalom kategória, a felhasználó; a földrajzi régióban (nap/hónap/hét), ideiglenes kulcs alapján létrehozott partícióknak az összes valóban attól függ, hogyan fogja hello adatait, és szerepel, hogy a közösségi élményt nyújt. 

Érdemes megemlíteni érdekes pont, hogy a Cosmos DB futtatja-e a lekérdezések egy (beleértve a [összesítések](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)) a partíciók közötti transzparens módon, nem kell tooadd bármely logika az adatok növekedésével.

Idővel, végül nőhet a forgalom és a hálózatierőforrás-fogyasztás (mért [RUs](request-units.md), vagy kérjen egységek) növeli. Lesz olvasási és írási gyakrabban, ha a userbase növekszik, és akkor indul, létrehozása és további tartalmat; olvasása hello képességét **az átviteli sebesség skálázás** nélkülözhetetlen. A RUs növelése nagyon egyszerű, azt megteheti mindössze néhány kattintással hello Azure portál vagy való [keresztül hello API parancsok kiadása](https://docs.microsoft.com/rest/api/documentdb/replace-an-offer).

![Vertikális felskálázásával és a partíciókulcs meghatározása](./media/social-media-apps/social-media-apps-scaling.png)

Mi történik, ha a dolgok folyton jobb és a felhasználók egy másik régióban, ország vagy kontinensen, figyelje meg, a platform és használatba, milyen kiváló nyújtható!

Várjon..., de azt hamarosan vegye figyelembe a felhasználói élmény a platformon nincs optimalizálva; Amennyiben távol is működési régiójától, hogy hello késés Félelmetes, és értelemszerűen nem szeretné, tooquit. Ha csak a egyszerűen történt **kiterjesztése a globális reach**..., de nincs!

A cosmos DB lehetővé teszi, hogy [globálisan replikálja az adatokat](../cosmos-db/tutorial-global-distribution-documentdb.md) és transzparens módon végzett néhány kattintással, és automatikusan hello elérhető régiók a közül választhat a [Ügyfélkód](../cosmos-db/tutorial-global-distribution-documentdb.md). Ez azt is jelenti, hogy rendelkezik [feladatátvétel több régióba](regional-failover.md). 

Globálisan replikálja az adatokat, meg kell toomake meg arról, hogy az ügyfelek is igénybe vehet fel. Ha egy webes előtér- vagy egypéldányú API-k mobilügyfelek használ, telepítheti [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) és az Azure App Service az összes szükséges hello régiókban, segítségével klónozza egy [teljesítmény konfigurációs](../app-service-web/web-sites-traffic-manager.md)toosupport a kiterjesztett globális érvényességének. Amikor az ügyfelek hozzáférnek az előtérbeli vagy API-k, fogja irányított toohello legközelebbi App Service, ami viszont toohello helyi Cosmos adatbázis-replika fog csatlakozni.

![Globális érvényességének tooyour közösségi platform hozzáadása](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Összegzés
Ez a cikk egy világos hello alternatívák létrehozásának közösségi hálózatokkal teljesen Azure alacsony költségű szolgáltatásokkal és nagy eredmények biztosítása "Létra" nevű többrétegű tárolási megoldás és az adatok terjesztési ösztönözze hello használatával történő tooshed megpróbál.

![Azure-közösségi hálózati szolgáltatások közötti interakció ábrája](./media/social-media-apps/social-media-apps-azure-solution.png)

hello igazság, hogy az ilyen típusú forgatókönyvek nem ezüst listajele van, ezért létre nagyszerű szolgáltatások, amelyek lehetővé teszik a számunkra toobuild kiváló lép hello kombinációjával együttműködés hello: hello sebesség és a kiváló közösségi alkalmazás Azure Cosmos DB tooprovide szabadságának hello eszközintelligencia első osztályú keresési megoldás mögött, például Azure Search, hello rugalmasságot biztosít az Azure App Service szolgáltatások toohost nem akkor is igaz, de hatékony háttérfolyamatot nyelvtől független alkalmazások és bővíthető Azure Storage és az Azure SQL Database hello nagy mennyiségű adatok és hello analitikai hatványra emelésének Azure Machine Learning toocreate tárolása Tudásbázis és az eszközintelligencia, amely visszajelzést tooour folyamatokat, és segítsen fájlmegosztásba hello jobb tartalom toohello arra jogosult felhasználók.

## <a name="next-steps"></a>Következő lépések
toolearn Cosmos DB alkalmazási helyzetei kapcsolatos további információkért lásd: [közös Cosmos DB használati esetekben](use-cases.md).
