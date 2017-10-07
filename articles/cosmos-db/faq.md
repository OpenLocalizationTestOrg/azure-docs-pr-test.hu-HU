---
title: "Gyakori kérdések Cosmos DB aaaAzure |} Microsoft Docs"
description: "Microsoft Azure Cosmos DB, egy globálisan elosztott, több modellre adatbázis szolgáltatás kapcsolatban felmerülő kérdések válaszok toofrequently beolvasása. További tudnivalók a kapacitás, a teljesítményszintet és a méretezés."
keywords: "Az adatbázisra vonatkozó kérdések, gyakran ismételt kérdéseket, a documentdb, az azure, a Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: b68d1831-35f9-443d-a0ac-dad0c89f245b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: mimig
ms.openlocfilehash: 59e047d9acd8ac05facc96655747d7495a45317a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-faq"></a>Az Azure Cosmos DB – gyakori kérdések
## <a name="azure-cosmos-db-fundamentals"></a>Az Azure Cosmos DB – alapok
### <a name="what-is-azure-cosmos-db"></a>Mi az Azure Cosmos DB?
Azure Cosmos-adatbázis egy globálisan replikált, több modellre adatbázis biztosító szolgáltatás, amely sémamentes adatok gazdag lekérdezését nyújt konfigurálható és megbízható teljesítményt, és gyors fejlesztést tesz lehetővé. Az összes érhető el, egy felügyelt platform, amely hello power alapját, és a Microsoft Azure sokoldalúsága keresztül. 

Azure Cosmos DB hello ideális megoldás a webes, mobil-, játék- és IoT-alkalmazásokhoz, amikor a kiszámítható átviteli sebességet, magas rendelkezésre állású, alacsony késést és a sémamentesadat-modell kapcsolatos követelményeket. Séma rugalmasságát és a gazdag indexelési biztosítja, és több dokumentumos tranzakciótámogatást JavaScript-integrációval ellátott tartalmazza. 

További adatbázisra vonatkozó kérdések, válaszok és központi telepítésével és a szolgáltatás használatával kapcsolatos útmutatásért lásd: hello [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/cosmos-db/).

### <a name="what-happened-toodocumentdb"></a>Milyen happened tooDocumentDB?
hello DocumentDB API az egyik Azure Cosmos DB az API-k és az adatok modell hello támogatott. Emellett Azure Cosmos DB támogatja, a Graph API-val (előzetes verzió), a tábla API (előzetes verzió) és a MongoDB API. További információkért lásd: [kérdések a DocumentDB-ügyfelek](#moving-to-cosmos-db).

### <a name="how-do-i-get-toomy-documentdb-account-in-hello-azure-portal"></a>Hogyan szerezhetek toomy DocumentDB-fiókot az Azure-portálon hello?
Hello Azure-portálon kattintson hello Azure Cosmos DB ikonra hello bal oldali ablaktáblán. Ha egy DocumentDB-fiókot, mielőtt, most már rendelkezik egy Azure Cosmos DB fiókhoz, amely nincs változás tooyour számlázási.

### <a name="what-are-hello-typical-use-cases-for-azure-cosmos-db"></a>Mik az Azure Cosmos DB hello jellemző használati esetei?
Az Azure Cosmos DB jó választás az új webes, mobil-, játék- és IoT-alkalmazásokhoz, ahol automatikus méretezési, kiszámítható teljesítményt, gyors ezredmásodperces válaszidők sorrendjét, és képes tooquery hello sémamentes adatok azért fontos. Az Azure Cosmos DB adatmodelljeinek toorapid fejlesztési és az azt támogató hello folyamatos ismétlését az alkalmazás. Felhasználó által létrehozott tartalmakat és adatokat kezelő alkalmazások [Azure Cosmos DB gyakori alkalmazási esetei](use-cases.md). 

### <a name="how-does-azure-cosmos-db-offer-predictable-performance"></a>Hogyan Azure Cosmos DB kiszámítható teljesítményt nyújtanak?
A [kérelem egység](request-units.md) (RU) az Adatbázisba az Azure Cosmos átviteli hello mérték. Egy 1-RU átviteli hello egy 1 KB-os dokumentum GET átviteli toohello felel meg. Az Azure Cosmos Adatbázisba, beleértve az olvasások, írások, SQL-lekérdezések és tárolt eljárás végrehajtások minden művelet értéke determinisztikus RU hello szükséges átviteli toocomplete hello művelet alapul. CPU, IO, és a memória és a az alkalmazás átviteli sebességére gyakorolt ezek mindegyike továbbléphetnek helyett kérelemegységet kell figyelembe RU egyetlen mértéket.

Minden Azure Cosmos DB tárolóhoz kiosztott átviteli sebesség szempontjából másodpercenkénti RUs foglalhat. Az alkalmazások valamilyen skálát elvégez egy teljesítménytesztet egyes kérelmeket toomeasure az RU értékekre, és jogosultságok kiosztása a kérelemegység tároló toohandle hello összesen összes kérelem összes kérelemegységének. Is növelheti vagy csökkentheti a tároló átviteli, hello kell rendelkeznie az alkalmazás igényeinek változásával. Kapcsolatos további információt, és segítséget meghatározása a tárolóban van szüksége, lásd [átviteli igények becslése](request-units.md#estimating-throughput-needs) hello próbálja [átviteli Számológép](https://www.documentdb.com/capacityplanner). hello kifejezés *tároló* toorefers tooa DocumentDB API gyűjtemény, a Graph API graph, a MongoDB API gyűjtemény és a tábla API tábla itt hivatkozik. 

### <a name="how-does-azure-cosmos-db-support-various-data-models-such-as-keyvalue-columnar-document-and-graph"></a>Hogyan támogatja az Azure Cosmos DB különböző adatmodellekben, például a kulcs/érték, oszlopos, a dokumentum és a graph?

Kulcs/érték (tábla), oszlopos, dokumentum és modellek összes miatt hello ARS (atom, rekordok és sorozatok) natív módon támogatott kialakítása, hogy Azure Cosmos DB Diagramadatok épül. Atom, a rekordok és a feladatütemezések lehet könnyen csatlakoztatott és a tervezett toovarious adatmodellekben. API-k hello modellek egy része számára elérhető jobb most (DocumentDB MongoDB, Table és Graph API-k) és mások adott tooadditional adatmodellekben rendelkezésre áll a jövőben hello.

Azure Cosmos-adatbázis egy séma független indexelési motor képes automatikusan indexelő azt ingests anélkül, hogy semmilyen sémát, illetve másodlagos indexek hello fejlesztőtől származó összes hello adatokat tartalmaz. hello motor logikai index elrendezések (fordított, oszlopos, fa), amely hello tárolási elrendezés hello index és a lekérdezés feldolgozása alrendszerek absztrakcióhoz készlete támaszkodik. Cosmos DB is bővíthető módon az átviteli protokollokat és API-k olyan készlete hello képességét toosupport rendelkezik, és kéréseivé átalakítani azokat hatékonyan toohello alapvető adatok modell (1) és hello logikai index elrendezés (2) így több adatmodellekben natív módon támogató egyedi módon képes .

### <a name="is-azure-cosmos-db-hipaa-compliant"></a>Azure Cosmos DB HIPAA megfelel?
Igen, az Azure Cosmos DB HIPAA-szabványnak megfelelő. HIPAA létesít követelményei hello használatához közzététel, és egyenként beazonosítható egészségügyi adatok védelmét. További információkért lásd: hello [Microsoft Trust Center](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA).

### <a name="what-are-hello-storage-limits-of-azure-cosmos-db"></a>Hello tárolási korlátai Azure Cosmos-adatbázis
Nincs adat egy tárolót tárolhat Azure Cosmos DB összeg összesen korlátot toohello.

### <a name="what-are-hello-throughput-limits-of-azure-cosmos-db"></a>Mik azok a Azure Cosmos DB hello átviteli sebességének korlátai?
Nincs egy tároló által támogatott az Azure Cosmos Adatbázisba átviteli összeg összesen korlát toohello. kulcs ötlet hello van toodistribute a munkaterhelés nagyjából egyenlően megfelelően nagy számú partíciós kulcsok között.

### <a name="how-much-does-azure-cosmos-db-cost"></a>Milyen mértékű nem költségű Azure Cosmos DB?
További információkért tekintse meg a toohello [Azure Cosmos DB díjszabása](https://azure.microsoft.com/pricing/details/cosmos-db/) lap. Kiépített tárolók, hello ennyi ideig hello száma hello tárolók online, és hello kiosztott átviteli sebesség minden egyes tároló határozza meg az Azure Cosmos DB hálózathasználati költségeket. hello kifejezés *tárolók* toohello DocumentDB API gyűjtemény, a Graph API graph, a MongoDB API gyűjtemény és a tábla API táblák itt hivatkozik. 

### <a name="is-a-free-account-available"></a>Van egy ingyenes fiókot?
Ha új tooAzure, akkor regisztrálhatnak az egy [ingyenes Azure-fiókot](https://azure.microsoft.com/free/), ami 30 nap és és a jóváírás tootry összes hello Azure-szolgáltatásokhoz. Ha a Visual Studio-előfizetéssel rendelkezik, akkor jogosultak is a [ingyenes Azure-krediteket](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) toouse bármely Azure-szolgáltatáshoz. 

Is használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) toodevelop és tesztelési az alkalmazás helyi szabadítson, Azure-előfizetés létrehozása nélkül. Ha elégedett az alkalmazás a hello Azure Cosmos DB emulátor alakulását, toousing hello felhőben Azure Cosmos DB fiók válthat.

### <a name="how-can-i-get-additional-help-with-azure-cosmos-db"></a>Hogyan kaphatok további segítséget a Azure Cosmos DB?
Ha segítségre van szüksége, érheti el a toous [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb) vagy hello [MSDN fórum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB), vagy hello Azure Cosmos DB mérnöki csapathoz beszélgetne csevegni ütemezése mail túl elküldésével[ askcosmosdb@microsoft.com](mailto:askcosmosdb@microsoft.com). 

## <a name="set-up-azure-cosmos-db"></a>Azure Cosmos DB beállítása
### <a name="how-do-i-sign-up-for-azure-cosmos-db"></a>Do I regisztrálás módja az Azure Cosmos DB?
Hello Azure portál Azure Cosmos DB érhető el. Először regisztráljon az Azure-előfizetésre. Most jelentkezett, miután adhat hozzá a DocumentDB API, a Graph API (előzetes verzió), a tábla API (előzetes verzió), vagy a MongoDB API fiók tooyour Azure-előfizetés.

### <a name="what-is-a-master-key"></a>Mi a főkulcs?
A főkulcs egy biztonsági jogkivonat tooaccess van egy olyan fiók összes erőforrást. Hello kulccsal rendelkező egyének olvasási és írási hozzáférés tooall erőforrások hello adatbázisfiók rendelkezik. Legyen körültekintő főkulcsok terjesztésekor. hello elsődleges és másodlagos főkulcsok érhetők el a hello **kulcsok** panelen található hello [Azure-portálon][azure-portal]. Kulcsokkal kapcsolatos további információkért lásd: [megtekintése, másolása és újragenerálása hívóbetűk](manage-account.md#keys).

### <a name="what-are-hello-regions-that-preferredlocations-can-be-set-to"></a>Mik azok a PreferredLocations megadható hello régióban? 
hello PreferredLocations érték beállítható az hello Azure tooany régiókban, ahol Cosmos DB érhető el. Elérhető régiók listáját lásd: [Azure-régiók](https://azure.microsoft.com/regions/).

### <a name="is-there-anything-i-should-be-aware-of-when-distributing-data-across-hello-world-via-hello-azure-datacenters"></a>Van-e, hogy ügyelnie, ha osztja el az adatok között hello world hello Azure adatközpontjaiban keresztül? 
Azure Cosmos DB megtalálható keresztül minden Azure-régió, mint a megadott hello [Azure-régiók](https://azure.microsoft.com/regions/) lap. Mivel hello alapvető szolgáltatásához, minden új datacenter rendelkezik egy Azure Cosmos DB jelenlétét. 

Ha úgy állítja be a régió, ne feledje, hogy Azure Cosmos DB tiszteletben tartja a állami és kormányzati felhők. Ez azt jelenti, hogy egy szuverén régióban létrehoz egy fiókot, ha nem replikálja szuverén régió kívül. Hasonlóképpen nem engedélyezhető a replikálás be más szuverén helyeken kívül fiókból. 

## <a name="develop-against-hello-documentdb-api"></a>Fejlesztése a DocumentDB API hello ellen

### <a name="how-do-i-start-developing-against-hello-documentdb-api"></a>Hogyan kezdhetem meg, hogy fejlesztését a DocumentDB API hello?
Microsoft DocumentDB API érhető el hello [Azure-portálon][azure-portal]. Először regisztrálnia kell az Azure-előfizetéssel. Regisztrálás után egy Azure-előfizetéséhez, a DocumentDB API tároló tooyour Azure-előfizetés is hozzáadhat. Egy Azure Cosmos DB fiók hozzáadására vonatkozó utasításokért lásd: [Azure Cosmos-adatbázis adatbázis-fiók létrehozása](create-documentdb-dotnet.md#create-account). Ha egy DocumentDB-fiókot az elmúlt hello, most már rendelkezik egy Azure Cosmos DB fiókot. 

[SDK-k](documentdb-sdk-dotnet.md) a .NET, Python, Node.js, JavaScript és Java esetében érhetők el. A fejlesztők is a hello [RESTful HTTP API-k](/rest/api/documentdb/) toointeract a különböző platformok és nyelvek Azure Cosmos DB erőforrásokkal.

### <a name="can-i-access-some-ready-made-samples-tooget-a-head-start"></a>Tudok hozzáférni bizonyos előre elkészített minták tooget egy head kezdő?
Minták a DocumentDB API hello [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md), és [Python](documentdb-python-samples.md) SDK a Githubon érhetők el.


### <a name="does-hello-documentdb-api-database-support-schema-free-data"></a>Nem hello DocumentDB API adatbázis támogatási sémamentes adatokat?
Hello DocumentDB API Igen, lehetővé teszi az alkalmazások toostore tetszőleges JSON-dokumentumok sémadefiníciók vagy mutatók nélkül. Adatok azonnal lekérdezhetők hello Azure Cosmos adatbázis SQL-lekérdezési felületén keresztül.  

### <a name="does-hello-documentdb-api-support-acid-transactions"></a>Támogatja a DocumentDB API hello ACID-tranzakciókat?
Igen, a DocumentDB API hello támogatja a JavaScript-ben tárolt eljárásokként és eseményindítókként kifejezett dokumentumok közötti tranzakciókat. -Tranzakciók le vannak tooa a egyes gyűjteményeken belül egyetlen partícióra hatókörű végrehajtásuk pedig az ACID szemantikákkal "mindent vagy semmit,", a többi párhuzamosan végrehajtott kódtól vagy felhasználói kérelmektől elkülönítve. Ha a kivételek JavaScript alkalmazáskód kiszolgálóoldali végrehajtásának hello jelentkeznek, hello teljes tranzakció vissza lesz állítva. Tranzakciókkal kapcsolatos további információkért lásd: [adatbázis-program tranzakciók](programming.md#database-program-transactions).

### <a name="what-is-a-collection"></a>Mi a gyűjtemény?
A gyűjtemény olyan dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát. A gyűjtemény egy számlázható entitás, akkor, ha hello [költség](performance-levels.md) hello átviteli sebesség határozza meg, és a tárhelyet használja. Gyűjtemények is kiterjedhet, egy vagy több partíció vagy a kiszolgálók és méretezhető toohandle gyakorlatilag korlátlan mennyiségű tárterület vagy átviteli sebesség.

Gyűjtemények egyaránt hello Azure Cosmos DB számlázási egységei. Egyes gyűjtemények számlázása óránként történik, a hello kiosztott átviteli sebesség alapján, és tárolóhely használt. További információkért lásd: [Azure Cosmos DB árképzési](https://azure.microsoft.com/pricing/details/cosmos-db/). 

### <a name="how-do-i-create-a-database"></a>Hogyan hozható létre adatbázis?
Adatbázisok hello segítségével létrehozható [Azure-portálon](https://portal.azure.com)leírtak szerint [egy gyűjtemény hozzáadása](create-documentdb-dotnet.md#create-collection), hello egyik [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md), vagy hello [REST API-k](/rest/api/documentdb/). 

### <a name="how-do-i-set-up-users-and-permissions"></a>Hogyan állíthatom be a felhasználókat és engedélyeket?
Hello egyikének használatával hozhat létre felhasználókat és engedélyeket [Cosmos DB API SDK-k](documentdb-sdk-dotnet.md) vagy hello [REST API-k](/rest/api/documentdb/).  

### <a name="does-hello-documentdb-api-support-sql"></a>Támogatja a DocumentDB API hello SQL?
hello SQL lekérdező nyelve az SQL által támogatott hello lekérdezési funkcionalitás továbbfejlesztett alkészlete. hello Azure Cosmos DB SQL lekérdező nyelve gazdag hierarchikus és relációs operátorokat és bővíthetőséget JavaScript-alapú, felhasználó által megadott funkciókat (UDF) biztosít. A JSON-szintaxis lehetővé teszi a JSON-dokumentumok fákként való címkézett csomópontok, amelyek hello Azure Cosmos DB automatikus indexelési technikái és a hello SQL-lekérdezési dialektusa Azure Cosmos-adatbázis által használt modellezési. SQL-szintaxis használatával kapcsolatos információkért lásd: hello [QueryDocumentDB] [ query] cikk.

### <a name="does-hello-documentdb-api-support-sql-aggregation-functions"></a>Hello DocumentDB API támogatási SQL összesítő függvények nem?
hello DocumentDB API támogatja a kis késleltetésű összesítési keresztül összesítő függvények bármilyen léptékben `COUNT`, `MIN`, `MAX`, `AVG`, és `SUM` hello SQL-szintaxis használatával. További információkért lásd: [Aggregátumfüggvények](documentdb-sql-query.md#Aggregates).

### <a name="how-does-hello-documentdb-api-provide-concurrency"></a>Hogyan biztosítja a DocumentDB API hello párhuzamossági?
hello DocumentDB API támogatja az egyidejű hozzáférések optimista vezérlését (OCC) HTTP entitáscímkék vagy ETag-EK keresztül. Minden DocumentDB API erőforrás rendelkezik egy ETag-gel, és hello ETag hello kiszolgálón van beállítva, minden alkalommal, amikor frissül egy dokumentumot. hello ETag fejlécet és hello jelenlegi értéke összes válaszüzenetek szerepelnek. ETag-EK használható hello If-Match fejléc tooallow hello server toodecide, hogy egy erőforrás kell frissíteni. If-Match érték hello hello ETag érték toobe veti össze. Van-e hello ETag érték hello kiszolgálói ETag érték megegyezik, hello erőforrás frissítése. Ha már nem aktuális hello ETag, hello kiszolgáló elutasítja a hello műveletet egy "HTTP 412 sikertelen előfeltétel" válaszkódot. hello ügyfél majd újból lekéri hello erőforrás tooacquire hello jelenlegi ETag érték hello erőforrás. Ezenkívül ETag-EK használható hello If-None-Match fejléc toodetermine e erőforrás újból beolvasni van szükség.

.NET-keretrendszerből használja hello a hozzáférések optimista toouse [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx) osztály. .NET mintát, lásd: [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) hello DocumentManagement mintát a Githubon található.

### <a name="how-do-i-perform-transactions-in-hello-documentdb-api"></a>Hogyan végezhetők tranzakciók a DocumentDB API hello?
hello DocumentDB API támogatja a nyelvintegrált tranzakciókat JavaScript-tárolt eljárások és eseményindítók keresztül. A parancsfájlban az összes művelet pillanatkép-elkülönítés lesz végrehajtva. Ha egy egypartíciós gyűjtemény, hello végrehajtása hatókörön belüli toohello gyűjtemény. Hello gyűjtemény particionálva van, ha hello végrehajtása-e a hello hatókörön belüli toodocuments hello gyűjteményen belül azonos partíciókulcs érték. Hello dokumentumban verziók (ETag-EK) pillanatkép hello tranzakció hello elején hozott és véglegesített csak akkor, ha hello parancsfájl sikeres. Hello JavaScript hibát jelez, ha hello tranzakció vissza lesz állítva. További információkért lásd: [Azure Cosmos DB kiszolgálóoldali JavaScript programozás](programming.md).

### <a name="how-can-i-bulk-insert-documents-into-cosmos-db"></a>Hogyan képes vagyok tömeges beszúrási dokumentumok Cosmos DB be?
Akkor is tömeges beszúrási dokumentumokat az Azure Cosmos DB két módon:

* hello adatáttelepítési eszköz, a [adatbázis áttelepítési eszköz az Azure Cosmos DB](import-data.md).
* Tárolt eljárások, a [Azure Cosmos DB kiszolgálóoldali JavaScript programozás](programming.md).

### <a name="does-hello-documentdb-api-support-resource-link-caching"></a>Hello DocumentDB API támogatja az erőforrás-hivatkozások gyorsítótárazását?
Igen, mert Azure Cosmos DB egy RESTful szolgáltatás, erőforrás-hivatkozások nem módosíthatók és ezáltal gyorsítótárazhatók. A DocumentDB API ügyfelek adjon meg egy "If-None-Match" fejlécet bármilyen erőforrás-szerű dokumentum vagy gyűjtemény olvasása és módosítsa a helyi példányok, miután hello kiszolgáló verziója megváltozott.

### <a name="is-a-local-instance-of-documentdb-api-available"></a>Egy helyi példányát a DocumentDB API érhető el?
Igen. Hello [Azure Cosmos DB emulátor](local-emulator.md) egy valósághű emulációját hello Cosmos DB szolgáltatást biztosít. Azonos tooAzure Cosmos DB, például létrehozása és a JSON-dokumentumok lekérdezését kiépítés funkciókat támogatja, és gyűjtemények skálázás, és végrehajtása tárolt eljárásokként és eseményindítókként. Fejlesztése és tesztelése alkalmazások hello Azure Cosmos DB Emulator használatával, és azok globális léptékű tooAzure azáltal, hogy az Azure Cosmos DB toohello csatlakozási végpont megváltoztatásához egyetlen konfigurációja.

## <a name="develop-against-hello-api-for-mongodb"></a>Mongodb-protokolltámogatással elleni hello API fejlesztése
### <a name="what-is-hello-azure-cosmos-db-api-for-mongodb"></a>Mi az Azure Cosmos DB API mongodb hello?
hello Azure Cosmos DB API mongodb, amely lehetővé teszi az alkalmazások tooeasily és transzparens módon hello natív Azure Cosmos-adatbázis adatbázis-kezelő használatával kommunikálnak meglévő, a Közösség által támogatott Apache MongoDB API-k és illesztőprogramok kompatibilitási réteg. A fejlesztők mostantól a meglévő MongoDB eszköz láncok és képességek toobuild alkalmazások Azure Cosmos DB előnyeit. A fejlesztők előnyt hello egyedi képességeit Azure Cosmos DB, többek között automatikus indexeléshez, a biztonsági mentési karbantartási, pénzügyi biztonsági szolgáltatásszint-szerződések (SLA), és így tovább.

### <a name="how-do-i-connect-toomy-api-for-mongodb-database"></a>Csatlakozás a MongoDB adatbázis API toomy?
hello leggyorsabb módon tooconnect toohello Azure Cosmos DB API a MongoDB toohead keresztül toohello [Azure-portálon](https://portal.azure.com). Nyissa meg tooyour fiók és hello bal oldali navigációs menüjében kattintson **gyors üzembe helyezés**. Gyors üzembe helyezési hello legjobb módja tooget kód kódtöredékek tooconnect tooyour adatbázis. 

Azure Cosmos-adatbázis kényszeríti a szigorú biztonsági követelményeket és előírásokat. Az Azure Cosmos DB fiókok hitelesítési és SSL-en keresztüli biztonságos kommunikációhoz szükséges, ezért meg arról, hogy toouse TLSv1.2.

További információkért lásd: [tooyour API-t a MongoDB adatbázis csatlakozás](connect-mongodb-account.md).

### <a name="are-there-additional-error-codes-for-an-api-for-mongodb-database"></a>Vannak további hibakódokat egy API-t a MongoDB-adatbázist?
Továbbá toohello gyakori MongoDB hibakódokat hello MongoDB API rendelkezik saját adott hibakódok:


| Hiba               | Kód  | Leírás  | Megoldás  |
|---------------------|-------|--------------|-----------|
| TooManyRequests     | 16500 | hello kérelem egységek száma felhasznált hello gyűjtemény hello kiosztott kérelem-egység arány meghaladta, és már elindította. | Vegye figyelembe a skálázás hello átviteli sebességgel hello Azure-portálon hello gyűjteményt, vagy újra próbálkozna. |
| ExceededMemoryLimit | 16501 | A több-bérlős szolgáltatásként hello művelet túllépte a hello ügyfél memória allokációs arányt. | Csökkentse a hello művelet keresztül szigorúbb lekérdezési kritériumok hello hatókörén, vagy forduljon a támogatási szolgálathoz a hello [Azure-portálon](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). <br><br>Példa: * &nbsp; &nbsp; &nbsp; &nbsp;db.getCollection('users').aggregate ([<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{$match: {név: "ADAM"}}, <br> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;{$sort: {kor: -1}}<br>&nbsp;&nbsp;&nbsp;&nbsp;])*) |

## <a name="develop-with-hello-table-api-preview"></a>Fejlesztést hello tábla API (előzetes verzió)

### <a name="terms"></a>Fogalmak 
hello Azure Cosmos DB: tábla API (előzetes verzió) toohello prémium ajánlat Azure Cosmos DB hivatkozik a Build 2017 bejelentette tábla támogatásához. 

hello szabványos tábla SDK hello meglévő Azure Storage tábla SDK. 

### <a name="how-can-i-use-hello-new-table-api-preview-offering"></a>Hogyan használhatók az új ajánlat hello a tábla API (előzetes verzió)? 
hello Azure Cosmos DB tábla API érhető el hello [Azure-portálon][azure-portal]. Először regisztrálnia kell az Azure-előfizetéssel. Most jelentkezett, miután egy Azure Cosmos DB tábla API fiók tooyour Azure-előfizetés hozzáadása, és adja hozzá táblák tooyour fiókot. 

Hello preview időszak amikor [SDK-k](../cosmos-db/table-sdk-dotnet.md) vannak érhető el a .NET, elkezdheti hello elvégzésével [tábla API](../cosmos-db/create-table-dotnet.md) gyors üzembe helyezési cikk.

### <a name="do-i-need-a-new-sdk-toouse-hello-table-api-preview"></a>Kell egy új SDK toouse hello tábla API (előzetes verzió)? 
Igen, hello [Windows Azure Storage prémium Table (előzetes verzió) SDK](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable) NuGet érhető el. További információ áll rendelkezésre a hello [Azure Cosmos DB táblában .NET API: Töltse le és a kibocsátási megjegyzésekben](https://github.com/Microsoft/azure-docs-pr/cosmos-db/table-sdk-dotnet.md) lap. 

### <a name="how-do-i-provide-feedback-about-hello-sdk-or-bugs"></a>Hogyan nyújtanak hello SDK vagy hibák visszajelzést?
A következő módokon hello valamelyikében visszajelzése megoszthatja:

* [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api)
* [MSDN-fórum](https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=AzureDocumentDB)
* [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-cosmosdb)

### <a name="what-is-hello-connection-string-that-i-need-toouse-tooconnect-toohello-table-api-preview"></a>Mi az, hogy le kell toouse tooconnect toohello tábla API (előzetes verzió) hello kapcsolati karakterláncot?
hello kapcsolati karakterlánca:
```
DefaultEndpointsProtocol=https;AccountName=<AccountNamefromCosmos DB;AccountKey=<FromKeysPaneofCosmosDB>;TableEndpoint=https://<AccountNameFromDocumentDB>.documents.azure.com
```
Hello kapcsolati karakterlánc kaphat oldaláról hello kulcsok hello Azure-portálon. 

### <a name="how-do-i-override-hello-config-settings-for-hello-request-options-in-hello-new-table-api-preview"></a>Hogyan felülbírálja a hello konfigurációs beállításainak hello lehetőségek hello új tábla API (előzetes verzió)?
Konfigurációs beállításaival kapcsolatos további információkért lásd: [Azure Cosmos DB képességek](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Hello beállításokat hozzáadással tooapp.config hello appSettings szakaszának hello ügyfélalkalmazás módosíthatja.

    <appSettings>
        <add key="TableConsistencyLevel" value="Eventual|Strong|Session|BoundedStaleness|ConsistentPrefix"/>
        <add key="TableThroughput" value="<PositiveIntegerValue"/>
        <add key="TableIndexingPolicy" value="<jsonindexdefn>"/>
        <add key="TableUseGatewayMode" value="True|False"/>
        <add key="TablePreferredLocations" value="Location1|Location2|Location3|Location4>"/>....
    </appSettings>


### <a name="are-there-any-changes-for-customers-who-are-using-hello-existing-standard-table-sdk"></a>Vannak-e módosítások hello meglévő szabványos tábla SDK használó ügyfelek számára?
nincs. Nincsenek meglévő vagy új hello meglévő szabványos tábla SDK használó ügyfelek nem módosult. 

### <a name="how-do-i-view-table-data-that-is-stored-in-azure-cosmos-db-for-use-with-hello-table-api-review"></a>Hogyan tekinthetem meg az Azure Cosmos DB hello tábla API (áttekintése) való használatra tárolt tábla adatai? 
Hello Azure portál toobrowse hello adatokat használhatja. Hello (előzetes verzió) tábla API-kódban vagy szerepel a következő választ hello hello eszközök is használható. 

### <a name="which-tools-work-with-hello-table-api-preview"></a>Mely eszközök használható hello tábla API (előzetes verzió)? 
Használhatja az Azure-kezelővel (0.8.9) hello régebbi verzióját.

Hello rugalmasságot tootake hello formátumban megadott kapcsolati karakterlánc korábban is támogatja az eszközök hello új tábla API (előzetes verzió). Egy tábla eszközök listája a hello [Azure Storage ügyféleszközök elől](../storage/common/storage-explorers.md) lap. 

### <a name="do-powershell-or-azure-cli-work-with-hello-new-table-api-preview"></a>PowerShell vagy Azure CLI működnek hello új tábla API-t (előzetes verzió)?
Tervezzük tooadd támogatja a PowerShell és az Azure parancssori felület tábla API (előzetes verzió). 

### <a name="is-hello-concurrency-on-operations-controlled"></a>Az hello feldolgozási műveletek ellenőrzött?
Igen, egyidejű hozzáférések optimista hello ETag mechanizmus hello segítségével valósul meg. 

### <a name="is-hello-odata-query-model-supported-for-entities"></a>Hello OData-lekérdezési modellel támogatja az entitások? 
Igen, a hello tábla API (előzetes verzió) támogatja az OData-lekérdezés és a LINQ lekérdezés. 

### <a name="can-i-connect-toohello-standard-azure-table-and-hello-new-premium-table-api-preview-side-by-side-in-hello-same-application"></a>Csatlakoztathatok toohello standard Azure-tábla és hello új premium tábla API (előzetes verzió) egymás mellett a hello ugyanahhoz az alkalmazáshoz? 
Igen, csatlakozhat, hozzon létre két külön példányának hello CloudTableClient, minden egyes mutató tooits URI saját keresztül hello kapcsolati karakterláncot.

### <a name="how-do-i-migrate-an-existing-azure-table-storage-application-toothis-new-offering"></a>Hogyan át egy meglévő Azure Table storage alkalmazás toothis az új ajánlat?
hello új tábla API ajánlat a meglévő tábla tárolási adatai, lépjen kapcsolatba tootake előnyeit [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com). 

### <a name="what-is-hello-roadmap-for-this-service-and-when-will-you-offer-other-standard-table-api-functionality"></a>Hello terv ezt a szolgáltatást, és mikor fog kínált más szabványos tábla API-funkciók?
Tervezzük tooadd támogatja az SAS jogkivonatokat, ServiceContext, statisztikák, ügyfél ügyféloldali titkosítás, elemzés és egyéb szolgáltatások, a Folytatás GA felé Akkor is küldjön visszajelzést a [Uservoice](https://feedback.azure.com/forums/599062-azure-cosmos-db-table-api). 

### <a name="how-is-expansion-of-hello-storage-size-done-for-this-service-if-for-example-i-start-with-n-gb-of-data-and-my-data-will-grow-too1-tb-over-time"></a>Hogyan van elvégezni ezt a szolgáltatást, ha például indításakor a hello tárméret bővítése * n * GB adatok és az adatok too1 TB tájékoztatás idővel bővülni fog? 
Az Azure Cosmos DB tervezett tooprovide hello segítségével a horizontális skálázás korlátlan tárterület. hello szolgáltatás figyelheti és hatékonyan növelése tárhelyét. 

### <a name="how-do-i-monitor-hello-table-api-preview-offering"></a>Hogyan figyelhetek hello tábla API (előzetes verzió) ajánlat?
Hello tábla API (előzetes verzió) is használhat **metrikák** ablaktábla toomonitor kérelmek és tárhely kihasználtsága. 

### <a name="how-do-i-calculate-hello-throughput-i-require"></a>Hogyan kiszámítja a szükséges I hello átviteli?
Hello kapacitás négyzetgyökének toocalculate hello hello műveletekhez szükséges TableThroughput is használhatja. További információkért lásd: [becsült kérelem egységek és az adatok tárolási](https://www.documentdb.com/capacityplanner). Általában határoz meg a JSON-ként entitás, és adja meg az operatív hello számok. 

### <a name="can-i-use-hello-new-table-api-preview-sdk-locally-with-hello-emulator"></a>Használhatok hello új tábla API (előzetes verzió) helyileg hello emulátorral SDK?
Igen, az emulátorral hello helyi használatakor tábla API (előzetes verzió) hello hello új SDK. új emulátor toodownload, nyissa meg túl[használata hello Azure Cosmos DB emulátor helyi fejlesztési és tesztelési](local-emulator.md). hello StorageConnectionString érték az App.config fájlban toobe van szüksége:

```
DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=C2y6yDjf5/R+ob0N8A7Cgv30VRDJIWEHLM+4QDU5DE2nQ9nDuVTqobD4b8mGGyPMbIZnqyMsEcaGQy67XIw/Jw==;TableEndpoint=https://localhost:8081`. 
```

### <a name="can-my-existing-application-work-with-hello-table-api-preview"></a>A meglévő alkalmazás dolgozhat hello tábla API (előzetes verzió)? 
hello hello felületének új tábla API (előzetes verzió) található hello Azure standard tábla között hello SDK létrehozása, törlése, frissítése és lekérdezési műveletek hello .NET API-t a meglévő kompatibilis. Ellenőrizze, hogy a sorkulcs, mert hello tábla API (előzetes verzió) partíciós kulcs és a sorkulcs van szükség. Is tervezzük tooadd felé szolgáltatás ajánlat GA Folytatás további SDK támogatása.

### <a name="do-i-need-toomigrate-my-existing-azure-table-based-applications-toohello-new-sdk-if-i-do-not-want-toouse-hello-table-api-preview-features"></a>Van toomigrate a meglévő Azure table-alapú alkalmazások toohello új SDK, ha nem szeretnék toouse hello tábla API (előzetes verzió) szolgáltatások?
Nem hozhat létre és használja a meglévő szabványos tábla eszközök bármilyen megszakítás nélkül. Azonban ha nem hajtja végre hello új tábla API (előzetes verzió), nem kihasználhatja a hello automatikus index, a hello további konzisztencia lehetőséget, vagy a globális terjesztési. 

### <a name="how-do-i-add-replication-of-hello-data-in-hello-premium-table-api-preview-across-multiple-regions-of-azure"></a>Hogyan hozzá hello adatok replikálása a hello prémium tábla API (előzetes verzió) Azure különféle régiókban?
Használhatja a hello Azure Cosmos DB portal [globális replikációs beállítások](tutorial-global-distribution-documentdb.md#portal) tooadd régiók megfelelő alkalmazás. toodevelop globálisan elosztott alkalmazást, akkor is fel kell hello PreferredLocation set toohello helyi körzet olvasási kis késleltetésű biztosítani az alkalmazásba. 

### <a name="how-do-i-change-hello-primary-write-region-for-hello-account-in-hello-premium-table-api-preview"></a>Hogyan változtathatom hello elsődleges írási régió hello fiók hello prémium tábla API (előzetes verzió)?
Hello Azure Cosmos DB globális replikációs portál ablaktábla tooadd egy területet használ, és majd feladatátvételt toohello kötelező régiót. Útmutatásért lásd: [fejlesztés az Azure Cosmos DB fiókok több területi](regional-failover.md). 

### <a name="how-do-i-configure-my-preferred-read-regions-for-low-latency-when-i-distribute-my-data"></a>Konfigurálása a saját elsődleges olvasási régiók az alacsony késleltetés amikor I terjesztése adataimat? 
toohelp olvasni hello helyi helyén, billentyűvel hello PreferredLocation hello app.config fájlban. A meglévő alkalmazások hello tábla API (előzetes verzió) hibát jelez, ha LocationMode be van állítva. Eltávolítani ezt a kódot, mert a hello prémium tábla API (előzetes verzió) szerzi be ezt az információt hello app.config fájlból. További információkért lásd: [Azure Cosmos DB képességek](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="how-should-i-think-about-consistency-levels-in-hello-table-api-preview"></a>Hogyan kell a tábla API (előzetes verzió) hello konzisztenciaszintek fontolnunk? 
Azure Cosmos DB biztosít jól indokolt kompromisszumot konzisztencia, a rendelkezésre állás és a késleltetés között. Azure Cosmos-adatbázis kínál öt konzisztenciaszintek tooTable (előzetes verzió) API-fejlesztők számára, hogy válassza ki a hello jobb konzisztencia modell hello tábla szinten egyedi kéréseket hello adatok lekérdezése közben. Amikor egy ügyfél csatlakozik, a konzisztenciaszint határozhatja meg. Hello szint hello app.config beállítás hello hello TableConsistencyLevel kulcs értékét a keresztül módosítható. 

hello tábla API (előzetes verzió) a kis késleltetésű olvassa be a "saját írások"olvasása hello alapértelmezett munkamenet-konzisztencia biztosítja. További információkért lásd: [konzisztenciaszintek](consistency-levels.md). 

Alapértelmezés szerint a Azure Table storage régión belül az erős konzisztencia és a másodlagos helyeken hello Eventual konzisztencia biztosítja. 

### <a name="does-azure-cosmos-db-offer-more-consistency-levels-than-standard-tables"></a>Azure Cosmos DB nyújtja a több, mint a hagyományos táblázatokban konzisztenciaszintek?
Igen, hogyan toobenefit hello az elosztott jellege Azure Cosmos DB kapcsolatos információkért lásd: [konzisztenciaszintek](consistency-levels.md). Mert garanciák hello konzisztenciaszintek-okat, használhatja őket az vetett bizalmat. További információkért lásd: [Azure Cosmos DB képességek](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities).

### <a name="when-global-distribution-is-enabled-how-long-does-it-take-tooreplicate-hello-data"></a>Ha engedélyezve van a globális terjesztési, mennyi ideig tart tooreplicate hello adatokat?
A Microsoft hello adatai tartósan régióban hello helyi véglegesíthetők, és leküldéses hello tooother adatterületre azonnal ezredmásodperc kell. A replikáció csak függ hello üzenetváltási időt (RTT) hello Datacenter. toolearn hello globális terjesztési képességét Azure Cosmos DB, kapcsolatos további információkért lásd: [Azure Cosmos DB: Azure globálisan elosztott adatbázis szolgáltatás](distribute-data-globally.md).

### <a name="can-hello-read-request-consistency-level-be-changed"></a>Hello olvasási kérelem konzisztenciaszint módosítható?
Az Azure Cosmos DB amelyen beállíthatja hello konzisztenciaszint hello tároló szintjén (hello tábla). Hello SDK használatával hello szint TableConsistencyLevel kulcs hello app.config fájlban hello érték megadásával módosítható. hello lehetséges értékek a következők: erős, kötött elavulási, munkamenet, egységes előtagot, valamint Eventual. További információkért lásd: [konzisztenciaszintek hangolható adatokat az Adatbázisba az Azure Cosmos](consistency-levels.md). hello kulcs lényege, hogy nem állítható be hello kérelem konzisztencia szint, több mint hello tábla hello beállítása. Például akkor nem be hello konzisztencia szintje Eventual hello táblázat és hello kérelem konzisztenciaszint erős. 

### <a name="how-does-hello-premium-table-api-preview-account-handle-failover-if-a-region-goes-down"></a>Hogyan nem hello prémium tábla API (előzetes verzió) fiókot kezelik feladatátvételi Ha leáll egy régiót? 
hello prémium szintű Azure Cosmos DB hello globálisan elosztott platformról alapszik tábla API (előzetes verzió). tooensure, hogy az alkalmazás működését datacenter állásidő, legalább egy további régió hello Azure Cosmos DB portálon hello fiók engedélyezése [fejlesztés az Azure Cosmos DB fiókok több területi](regional-failover.md). Hello portál használatával beállíthatja a hello prioritás hello régió [fejlesztés az Azure Cosmos DB fiókok több területi](regional-failover.md). 

Tetszőleges számú régiókban hello fiók, és szabályozhatja, ahol azt a feladatátvétel tooby biztosító feladatátvevő prioritást adhat hozzá. Természetesen toouse hello adatbázis, szüksége van egy alkalmazás tooprovide túl. Ha így tesz, az ügyfelek nem fog tapasztalni állásidő. a rendszer automatikusan hello ügyfél SDK homing. Ez azt jelenti, hogy nem működik, és automatikusan áthelyezze a feladatokat toohello új régió hello régió azonosítására képes.

### <a name="is-hello-premium-table-api-preview-enabled-for-backups"></a>Hello prémium tábla API (előzetes verzió) engedélyezve van a biztonsági mentésekhez?
Igen, hello prémium tábla API (előzetes verzió) alapszik platformról hello Azure Cosmos-adatbázis a biztonsági mentésekhez. Biztonsági mentések a automatikusan történik. További információkért lásd: [Online biztonsági mentés és helyreállítás Azure Cosmos DB](online-backup-and-restore.md).

 
### <a name="does-hello-table-api-preview-index-all-attributes-of-an-entity-by-default"></a>Tábla API (előzetes verzió) hello index entitás összes attribútumának alapértelmezés szerint?
Igen, az entitás összes attribútum indexelt alapértelmezés szerint. További információkért lásd: [Azure Cosmos DB: Indexelő házirendek](indexing-policies.md). 

### <a name="does-this-mean-i-do-not-have-toocreate-multiple-indexes-toosatisfy-hello-queries"></a>Ezt az átlagos does nincs toocreate több indexek toosatisfy hello lekérdezést? 
Igen, Azure Cosmos DB biztosít az összes attribútum nélkül bármely sémadefiníciót automatikus indexeléshez. Ezt az automatizálást szabadít fel a fejlesztők toofocus hello alkalmazás helyett index létrehozását és kezelését. További információkért lásd: [Azure Cosmos DB: Indexelő házirendek](indexing-policies.md).

### <a name="can-i-change-hello-indexing-policy"></a>Módosíthatja a hello indexelési házirendet?
Igen, módosíthatja hello indexelési házirendet hello Indexdefiníció megadásával. További információkért lásd: [Azure Cosmos DB képességek](../cosmos-db/tutorial-develop-table-dotnet.md#azure-cosmos-db-capabilities). Tooproperly kell kódolni és escape-hello-beállítások. 

Karakterlánc json formátumban hello app.config fájlban:
```
{
  "indexingMode": "consistent",
  "automatic": true,
  "includedPaths": [
    {
      "path": "/somepath",
      "indexes": [
        {
          "kind": "Range",
          "dataType": "Number",
          "precision": -1
        },
        {
          "kind": "Range",
          "dataType": "String",
          "precision": -1
        } 
      ]
    }
  ],
  "excludedPaths": 
[
 {
      "path": "/anotherpath"
 }
]
}
```

### <a name="azure-cosmos-db-as-a-platform-seems-toohave-lot-of-capabilities-such-as-sorting-aggregates-hierarchy-and-other-functionality-will-you-be-adding-these-capabilities-toohello-table-api"></a>Azure Cosmos DB platformként úgy tűnik, hogy toohave nagy mennyiségű képességeit, például a rendezés, összesítések, hierarchiát és egyéb funkciókat. Ön hozzáadja a képességek toohello tábla API? 
A képen hello tábla API biztosít hello azonos Azure Table storage funkciókat lekérdezése. Azure Cosmos-adatbázis is támogatja a rendezést, összesítések, a földrajzi lekérdezést, a hierarchia és a számos különféle beépített funkciók. A tábla API hello jövőbeli szolgáltatásfrissítés további funkciók lesz elérhető. További információkért lásd: [Azure Cosmos DB DocumentDB API SQL-lekérdezések](../documentdb/documentdb-sql-query.md).
 
### <a name="when-should-i-change-tablethroughput-for-hello-table-api-preview"></a>Mikor kell módosítani a tábla API (előzetes verzió) hello TableThroughput?
TableThroughput kell módosítani, ha érvényes hello a következő feltételek valamelyike:
* Helyreállítást hajt végre egy kinyerési, átalakítási és betöltési (ETL) az adatok, vagy a kívánt tooupload nagy mennyiségű adat rövid időn belül. 
* További átviteli sebesség: hello háttér hello tárolóból van szüksége. Például láthatja, hogy több mint hello kiosztott átviteli sebesség esetén hello használt átviteli sebesség, és akkor első szabályozott. További információkért lásd: [Set átviteli az Azure Cosmos DB tárolókat](set-throughput.md).

### <a name="can-i-scale-up-or-scale-down-hello-throughput-of-my-table-api-preview-table"></a>Növelheti vagy csökkentheti a tábla API (előzetes verzió) tábla hello átviteli? 
Igen, az hello Azure Cosmos DB portal méretezési ablaktábla tooscale hello átviteli sebességet. További információkért lásd: [Set átviteli](set-throughput.md).

### <a name="is-a-default-tablethroughput-set-for-newly-provisioned-tables"></a>Az egy alapértelmezésben TableThroughput újonnan kiosztott táblák?
Igen, ha nem bírál felül hello TableThroughput app.config keresztül, és nem használható egy előre létrehozott tároló Azure Cosmos DB, hello szolgáltatás létrehoz egy táblát 400 átviteli sebességgel.
 
### <a name="is-there-any-change-of-pricing-for-existing-customers-of-hello-standard-table-api"></a>Van-e bármilyen módosítás a meglévő ügyfeleknek hello szabványos tábla API árképzési?
nincs. Nincs meglévő szabványos tábla API-ügyfeleknek az ár nem változik. 

### <a name="how-is-hello-price-calculated-for-hello-table-api-preview"></a>Hogyan hello ár számítása hello tábla API (előzetes verzió)? 
hello ára TableThroughput lefoglalt hello függ. 

### <a name="how-do-i-handle-any-throttling-on-hello-tables-in-table-api-preview-offering"></a>Hogyan kezelik, a sávszélesség-szabályozás hello táblákon tábla API (előzetes verzió) nyújtó? 
Hello kérelmek aránya meghaladja a hello mögöttes tároló hello hello kiosztott átviteli sebesség kapacitását, ha hiba lép fel, és újra hello SDK megpróbálja hello hívás hello újrapróbálkozási házirendje alkalmazásával.

### <a name="why-do-i-need-toochoose-a-throughput-apart-from-partitionkey-and-rowkey-tootake-advantage-of-hello-premium-table-api-preview-offering-of-azure-cosmos-db"></a>Miért kell toochoose átviteli sebességgel leszámítva hello prémium tábla API (előzetes verzió) elérhető Azure Cosmos DB PartitionKey és RowKey tootake előnyeit?
Azure Cosmos-adatbázis a egy alapértelmezett átviteli sebesség a tároló beállítja, ha nem ad meg egy hello app.config fájlban. 

Azure Cosmos DB garanciákat nyújt a teljesítmény és a késés, a felső határai, a műveletet. Ez a garancia hello motor kényszerítheti a cégirányítási műveletek hello bérlői esetén lehetséges. TableThroughput beállítása biztosítja, hogy garantált átviteli sebesség és a késleltetés, mert hello platform fenntartja a kapacitás, és biztosítja, hogy az operatív sikeres hello kap. 

Hello átviteli sebesség meghatározása segítségével rugalmasan módosítsa úgy a hello szezonalitás értékének az alkalmazás toobenefit programot, hello átviteli igényeinek, és a költségek csökkentése.

### <a name="azure-storage-sdk-has-been-very-inexpensive-for-me-because-i-pay-only-toostore-hello-data-and-i-rarely-query-hello-new-azure-cosmos-db-offering-seems-toobe-charging-me-even-though-i-have-not-performed-a-single-transaction-or-stored-anything-can-you-please-explain"></a>Az Azure Storage szolgáltatás SDK a számomra, nagyon alacsony költségű volt, mert az csak a toostore hello adatokat kell fizetni, és ritkán lekérdezése. hello az új Azure Cosmos DB ajánlat díjszabási me, annak ellenére, hogy I nem egyetlen tranzakció végre vagy bármi tárolt toobe tűnik. Adjon melyek?

Azure Cosmos DB tervezett toobe olyan globálisan elosztott, SLA-alapú rendszer, amely a rendelkezésre állási, a késés és a teljesítményt garantálja. Amikor lefoglalni Azure Cosmos DB átviteli sebességet, válik biztossá, eltérően hello sebességét, a más rendszerekkel. Azure Cosmos-adatbázis, amely az ügyfelek kért, például a másodlagos indexek és globális terjesztési további képességeket biztosít. Hello preview időszakban nyújtunk átviteli optimalizált modell, és végül tervezzük tooprovide egy tárolási optimalizált modell toomeet ügyfelei igényeit. 

### <a name="i-never-get-a-quota-full-notification-indicating-that-a-partition-is-full-when-i-ingest-data-into-table-storage-with-hello-table-api-preview-i-do-get-this-message-is-this-offering-limiting-me-and-forcing-me-toochange-my-existing-application"></a>Soha nem jelenik meg a "teljes" kvótaértesítéshez (arról, hogy a partíció teljes) Ha I betöltik az adatokat a Table storage-be. A hello tábla API (előzetes verzió) Ez az üzenet jelenik meg. Ez kínál me korlátozása, és a me toochange a meglévő alkalmazás?

Azure Cosmos-adatbázis egy SLA-alapú rendszeren, amely korlátlan méretezési biztosít garanciák késés, teljesítményt, rendelkezésre állási és konzisztencia. prémium szintű teljesítmény, garantált tooensure ellenőrizze, hogy az adatok mérete és az index kezelhető és méretezhető. hello 10 GB-os korlátot hello az entitások vagy elemek száma a partíciós kulcs, hogy nyújtunk remek keresési és lekérdezéseivel kapcsolatos teljesítményt tooensure. amely az alkalmazás méretezi is jól az Azure Storage tooensure, azt javasoljuk, hogy Ön *nem* hozza létre a gyakran használt adatok partíciót egy partíció összes információ tárolása és lekérdezéssel. 

### <a name="so-partitionkey-and-rowkey-are-still-required-with-hello-new-table-api-preview"></a>Így továbbra is szükséges a PartitionKey és RowKey hello új tábla API (előzetes verzió)? 
Igen. Mivel hello felületének hello tábla API (előzetes verzió), a Table storage SDK hello hasonló toothat, a hello partíciókulcs egy hatékony módját toodistribute hello adatokat biztosít. hello sor fontos, hogy egyedi. hello sor kulcs igények toobe jelen, és nem lehet null értékű, és a szabványos SDK hello. hello RowKey hossza 255 bájt és hello PartitionKey hossza 100 bájtra (hamarosan toobe nőtt too1 KB). 

### <a name="what-are-hello-error-messages-for-hello-table-api-preview"></a>Mik azok a hello hibaüzenetekben hello tábla API (előzetes verzió)?
Mivel ez az előnézet hello szabványos tábla kompatibilis, hello hibák többségét toohello hibák hello szabványos táblából felelteti meg. 

### <a name="why-do-i-get-throttled-when-i-try-toocreate-lot-of-tables-one-after-another-in-hello-table-api-preview"></a>Miért tegye I get halmozódni jelenik meg, hogy toocreate számos táblázathoz egymás után hello tábla API (előzetes verzió)?
Azure Cosmos-adatbázis az SLA-alapú rendszerek késés, teljesítményt, rendelkezésre állását és konzisztencia biztosítja biztosító. Mivel a kiépített rendszer, az fenntartja erőforrások tooguarantee ezeket a követelményeket. táblák létrehozása gyors aránya hello észlelt és szabályozva. Azt javasoljuk, hogy tekintse meg a táblák létrehozásakor hello sebessége és alacsonyabb, mint 5 percenként tooless. Ne feledje, hogy hello tábla API (előzetes verzió) kiépített rendszer. hello jelenleg építi ki, toopay megkezdődik az. 

## <a name="develop-against-hello-graph-api-preview"></a>Graph API-val (előzetes verzió) hello elleni fejlesztése
### <a name="how-can-i-apply-hello-functionality-of-graph-api-preview-tooazure-cosmos-db"></a>Hogyan alkalmazhatja a Graph API-val (előzetes verzió) tooAzure Cosmos DB hello működését?
Egy bővítmény könyvtár tooapply hello funkciót a Graph API (előzetes verzió) használhatja. Ezt a szalagtárat a Microsoft Azure diagramjait nevezik, és a NuGet érhető el. 

### <a name="it-looks-like-you-support-hello-gremlin-graph-traversal-language-do-you-plan-tooadd-more-forms-of-query"></a>Úgy tűnik, Ön által támogatott hello Gremlin graph átjárás nyelv. Tervezi tooadd további formáit lekérdezést?
Igen, tervezzük tooadd más mechanizmusok jövőbeli hello lekérdezést. 

### <a name="how-can-i-use-hello-new-graph-api-preview-offering"></a>Hogyan használhatók az új ajánlat hello a Graph API-val (előzetes verzió)? 
tooget elindítva, teljes hello [Graph API](../cosmos-db/create-graph-dotnet.md) gyors üzembe helyezési cikk.

<a id="moving-to-cosmos-db"></a>
## <a name="questions-from-documentdb-customers"></a>Kérdések a DocumentDB-ügyfelek
### <a name="why-are-you-moving-tooazure-cosmos-db"></a>Miért meg áthelyezi tooAzure Cosmos DB? 

Azure Cosmos DB hello következő nagy termékek globálisan elosztott, a méretezési felhő adatbázisokban. A DocumentDB ügyfélként, most már rendelkezik hozzáférési toohello áttörést jelentő rendszer és az Azure Cosmos DB által kínált lehetőségeket.

Az Azure Cosmos DB "Projekt Firenzében" 2010-es tooaddress hello problémás pontok a Microsoft vállalaton belül nagyméretű alkalmazások fejlesztése során a fejlesztők által tapasztalt indítja el. hello kihívásai globálisan elosztott alkalmazások létrehozása nincsenek egyedi tooMicrosoft, így azt elérhetik a technológia hello első generációs 2015 tooAzure fejlesztők Azure DocumentDB hello formájában. 

Azóta előre hozzáadott új funkciók és bevezetett jelentős új képességekkel rendelkezik. Az Azure Cosmos DB hello eredménye. Ebben a kiadásban részeként ügyfelek a DocumentDB-vel adataikat, automatikusan és zökkenőmentesen vásárlás Azure Cosmos DB. Ezek a képességek hello területek hello core adatbázismotor, valamint globális terjesztési, rugalmas méretezhetőség és iparágvezető, átfogó SLA-k szerepelnek. Pontosabban azt lett hatékonyabb hello Azure Cosmos DB database engine tooefficiently térkép összes népszerű adatmodellekben típus rendszerek és API-k toohello alapul szolgáló adatmodellt Azure Cosmos-adatbázis. 

hello aktuális fejlesztői irányuló megnyilvánulása a munka hello új támogatása [Gremlin](../cosmos-db/graph-introduction.md) és [Table storage API-kkal](../cosmos-db/table-introduction.md). És ez csak hello elején. Tervezzük tooadd egyéb népszerű API-k és az újabb adatmodellekben adott idő alatt, további fejlett teljesítmény- és tárolási globális méretekben. 

Fontos toopoint ki, hogy a DocumentDB hello [SQL dialektus](../documentdb/documentdb-sql-query.md) mindig az, hogy csak egy hello sok API-k, hogy hello alapul szolgáló Azure Cosmos DB képes támogatni. A fejlesztők számára, például az Azure Cosmos Database egy teljes körűen felügyelt szolgáltatást alkalmazó, hello csak illesztőfelület toohello szolgáltatás le hello API-k által hello szolgáltatást. Semmi valóban módosítja a meglévő DocumentDB-ügyfelek. Az Azure Cosmos Adatbázisba pontosan azonos SQL API-t, hogy a DocumentDB kínál hello beolvasása. És most (és a jövőbeli hello) érheti el más korábban nem elérhető képességek 

A folyamatos munka egy másik megnyilvánulása kiterjesztett hello alaprendszer a globális és a rugalmas méretezhetőség érdekében az átviteli sebesség és tárterület. Továbbfejlesztettük több eligazodást toohello globális terjesztési alrendszer. Hello sok ilyen fejlesztői irányuló szolgáltatások egyik hello konzisztens előtag konzisztencia modell, amely lehetővé teszi egy teljes öt jól meghatározott konzisztencia modellek. Sok érdekesebb képességek, valamint azt ad ki. 

### <a name="what-do-i-need-toodo-tooensure-that-my-documentdb-resources-continue-toorun-on-azure-cosmos-db"></a>Mire van szükség, hogy a DocumentDB-erőforrások Azure Cosmos DB Folytatás toorun toodo tooensure?

Meg kell toomake nem végez módosítást minden. A DocumentDB-erőforrások Azure Cosmos DB erőforrásokat, és hiba történt a hello szolgáltatás megszakítás nélküli fenntartása, ha ez történt.

### <a name="what-changes-do-i-need-toomake-for-my-app-toowork-with-azure-cosmos-db"></a>Módosítások mire toomake van szükségem az Azure Cosmos DB my app toowork?

Nincsenek nem módosítások toomake. Osztályok, a névterek és a NuGet csomag neve nem módosultak. Mivel mindig, azt javasoljuk, hogy ne az SDK-k mentése toodate tootake előnyeit hello legújabb szolgáltatásait és fejlesztéseit. 

### <a name="whats-changed-in-hello-azure-portal"></a>Az Azure-portálon hello változások?

A DocumentDB már nem jelenik meg a hello portálon az Azure-szolgáltatások. Helyette van egy új Azure Cosmos DB ikonra, ahogy az a következő kép hello. A gyűjtemények érhetők el, előtt voltak, és továbbra is méretezheti átviteli, a módosítás konzisztenciaszintek és a figyelő SLA-k. hello képességeit adatok kezelővel (előzetes verzió) javítása. Mostantól megtekintése és szerkeszthetik a dokumentumokat, hozzon létre és lekérdezéseinek futtatásához, és hello kép a következő ábrán egy oldal, a tárolt eljárások, eseményindítók és UDF működik: 

![hello Azure Cosmos DB Gyűjtemények panel](./media/faq/cosmos-db-data-explorer.png)

### <a name="are-there-changes-toopricing"></a>Vannak-e módosítások toopricing?

Nem fut az alkalmazás az Azure-Cosmos adatbázis hello költségét hello azonos előtt.

### <a name="are-there-changes-toohello-slas"></a>Vannak-e módosítások toohello SLA-kat?

Nem, hello rendelkezésre állási SLA-k, a konzisztencia, a késés és az átvitel nem változik, és hello portál továbbra is megjelennek. További információkért lásd: [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/).
   
![Tennivalók app mintaadatokkal](./media/faq/azure-cosmosdb-portal-metrics-slas.png)


[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md
