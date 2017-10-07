---
title: "az Azure Cosmos DB aaaServer ügyféloldali JavaScript programozási |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure Cosmos DB toowrite tárolt eljárások, adatbázis-eseményindítók és felhasználó által megadott funkciókat (UDF) a JavaScript. Adatbázis programing tippeket és több kapják meg."
keywords: "Adatbázis-eseményindítók, tárolt eljárás, tárolt eljárás, adatbázis program, sproc, a documentdb, azure, a Microsoft azure"
services: cosmos-db
documentationcenter: 
author: aliuy
manager: jhubbard
editor: mimig
ms.assetid: 0fba7ebd-a4fc-4253-a786-97f1354fbf17
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: andrl
ms.openlocfilehash: 5a011d1c4b0b5908d5de73607a1bc328ed1711d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Az Azure Cosmos DB kiszolgálóoldali programozása: tárolt eljárások, eseményindítók adatbázis és a felhasználó által megadott függvények
Ismerje meg, hogy Azure Cosmos DB nyelvi integrálva, a tranzakciós végrehajtását a JavaScript lehetővé teszi, hogy a fejlesztők írási **tárolt eljárások**, **eseményindítók** és **felhasználó által definiált funkciókat (UDF)** a natív módon egy [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. Ez lehetővé teszi toowrite adatbázis program alkalmazáslogikát szállított és végre közvetlenül a hello adatbázis adattárolási partíciókat. 

Ajánlott indította a következő néznek hello videó, ahol Andrew Liu egy rövid bevezető tooCosmos DB tartozó kiszolgálóoldali adatbázis programozási modellt biztosít. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

Ezt követően térjen vissza toothis cikk, ahol megtudhatja, hello válaszok toohello a következő kérdéseket:  

* Hogyan írni egy tárolt eljárás, eseményindító vagy használatával JavaScript UDF?
* Hogyan Cosmos DB sav garantálni?
* Hogyan működik a Cosmos DB tranzakciók?
* Mik azok a előre elindítja és utáni indítása, és hogyan hajtsa végre írási egy?
* Hogyan regisztrálja és futtatni egy tárolt eljárás, eseményindító vagy UDF RESTful módon HTTP-n keresztül?
* Mi Cosmos DB SDK elérhető toocreate és végrehajtani tárolt eljárások, eseményindítók és felhasználó által megadott függvények?

## <a name="introduction-toostored-procedure-and-udf-programming"></a>Bevezetés tooStored eljárás és UDF programozási
Ez a megközelítés a *"JavaScript egy T-SQL modern napot"* felszabadítja a rendszer típuseltérést és objektum-relációs leképezés technológiák hello bonyolultságára alkalmazásfejlesztők. Azt is, amelyek magas kihasználtsággal rendelkező toobuild gazdag alkalmazások lehetnek belső előnyeit számos:  

* **Eljárási logika:** egy magas szintű programozási nyelv, JavaScript biztosít a felület gazdag és ismerős tooexpress üzleti logikát. Műveletek szorosabb toohello adatok összetett sorozatát végezheti el.
* **Az atomi tranzakciók:** Cosmos DB biztosítja, hogy az adatbázis-műveletet hajtott végre egy tárolt eljárás vagy eseményindító belül atomi. Ez lehetővé teszi, hogy egy alkalmazás egyesítése egyetlen kötegben kapcsolódó műveleteket, hogy az összes sikeres legyen, vagy egyiket sem sikerült. 
* **Teljesítmény:** hello tényt, hogy JSON belsőleg csatlakoztatott toohello Javascript nyelv típusrendszernek, is hello alapvető egysége a Cosmos DB tárolási lehetővé teszi, hogy például a JSON a Lusta materialization optimalizálásokat számos hello pufferben lévő dokumentumok készlet és minősítené elérhető igény szerinti toohello kód végrehajtása. Nincsenek további teljesítménybeli előnyökben szállítási üzleti logika toohello adatbázishoz tartozó:
  
  * Kötegelés – fejlesztők csoport műveletek tartoznak, mint a beszúrások, és a tömeges küldheti el ezeket. hello hálózati forgalom késés költségeket, és hello áruház általános toocreate külön tranzakciók jelentős mértékben csökken. 
  * A fordítás előtti – Cosmos DB precompiles tárolt eljárások, eseményindítók és felhasználó által megadott funkciókat (UDF) tooavoid egyes elindításaihoz JavaScript-fordítási költség. hello hello bájt kód kialakításának hello eljárási logikai érték terhelés amortized tooa minimális érték.
  * Alkalmazás-előkészítés – sok műveleteket kell egy mellékhatása ("eseményindító"), amely potenciálisan magában foglalja a egy vagy több másodlagos tároló műveleteket. Vezérelt atomicity, ez a további performant áthelyezésekor toohello kiszolgáló. 
* **Beágyazás:** a tárolt eljárások használt toogroup üzleti logika egy helyen lehet. Ez a két előnnyel rendelkezik:
  * Hozzáadja egy hardverabsztrakciós réteg hello nyers adatait, amely lehetővé teszi, hogy a fejlesztők tooevolve adatok az alkalmazások egymástól függetlenül hello adatokból felett. Ez akkor különösen hasznos, ha hello adatok séma nélküli, toohello rideg feltételek, amelyek esetleg bővíthetőség hello alkalmazásba, ha toodeal adatokkal rendelkeznek közvetlenül toobe miatt.  
  * Ez az absztrakció lehetővé teszi, hogy a vállalatok számára az adatok biztonsága egyszerűsítése hello a hozzáférést a hello parancsfájlok.  

hello létrehozását és adatbázis-eseményindítók, tárolt eljárás és egyéni lekérdezési operátorok végrehajtásának támogatott keresztül hello [REST API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), és [ügyfél SDK-k](documentdb-sdk-dotnet.md) például .NET, Node.js és JavaScript számos platformon.

Ez az oktatóanyag használja hello [Node.js SDK-val Q tett](http://azure.github.io/azure-documentdb-node-q/) tooillustrate szintaxis és a tárolt eljárások, eseményindítók és felhasználó által megadott függvények használatát.   

## <a name="stored-procedures"></a>Tárolt eljárások
### <a name="example-write-a-simple-stored-procedure"></a>Példa: Egy egyszerű tárolt eljárás írása
Kezdjük egy egyszerű tárolt eljárás, amely a "Hello, World" választ ad vissza.

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


Tárolt eljárások gyűjteményenként regisztrálva van, és a dokumentum és a mellékletek megtalálható a gyűjteményben is működik. hello alábbi kódrészletben láthatja, hogyan tooregister hello helloWorld tárolt eljárás gyűjtemény tartozik. 

    // register hello stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Miután hello tárolt eljárás regisztrálva van, azt is végre hello gyűjtemény ellen, tartózkodik, olvassa el hello vissza hello ügyfélen. 

    // execute hello stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


hello környezeti objektumot, amely a Cosmos adatbázis-terület végrehajtható, valamint a toohello kérelem-válasz objektumokhoz tooall műveletek hozzáférést biztosít. Ebben az esetben hello objektum tooset hello választörzs a hello választ küldött vissza toohello ügyfél használtuk. További részletekért tekintse meg a toohello [Azure Cosmos DB JavaScript server SDK-dokumentáció](http://azure.github.io/azure-documentdb-js-server/).  

Ossza meg velünk bontsa ki az ebben a példában, és több adatbázis-funkciók hozzáadása toohello tárolt eljárást. Tárolt eljárások létrehozása, frissítése, olvassa el, lekérdezheti és dokumentumok és mellékletek belül hello gyűjtemény törlése.    

### <a name="example-write-a-stored-procedure-toocreate-a-document"></a>Példa: Írni egy tárolt eljárás toocreate dokumentum
hello következő kódrészletben láthatja, hogyan toouse hello környezeti objektum toointeract Cosmos DB erőforrásokkal.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


A tárolt eljárás időt vesz igénybe, mint bemeneti documentToCreate, a dokumentum toobe hello aktuális gyűjtemény létrehozott hello törzsét. Ilyen műveletek aszinkron jellegűek, és a JavaScript függvény visszahívások függenek. hello visszahívási függvény két paraméterrel, egy a hello hiba objektum rendelkezik, abban az esetben hello művelet sikertelen lesz, és egy hello a létrehozott objektum. Hello visszahívási belüli felhasználók hello kivétel kezeléséhez, vagy hibaüzenetet küldjön. Egy visszahívás nem áll rendelkezésre, és nem sikerül, akkor hello Azure Cosmos DB futásidejű hibát jelez.   

Hello a fenti példában a hello visszahívása hibát jelez, ha hello művelet sikertelen volt. Ellenkező esetben állítja a dokumentum létrehozott hello válasz toohello ügyfél hello törzsként hello hello azonosítója. Ez a tárolt eljárás bemeneti paraméterekkel végrehajtásának módját.

    // register hello stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;

            // run stored procedure toocreate a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "hello Hitchhiker’s Guide toohello Galaxy",
                author: "Douglas Adams"
            };

            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });


Vegye figyelembe, hogy ez a tárolt eljárás is lehet módosított tootake dokumentum szervezetek bemeneti tömb, és hozzon létre azokat az összes hello ugyanaz a tárolt eljárás végrehajtása több hálózati helyett kérelmek toocreate minden ezek külön-külön. Ez lehet használt tooimplement egy hatékony tömeges importáló a Cosmos DB (Ez az oktatóanyag későbbi részében bemutatása).   

leírt hello példa azt mutatja, hogyan toouse tárolt eljárások. Bemutatjuk, eseményindítók és felhasználó által megadott funkciókat (UDF) hello oktatóanyag későbbi részében.

## <a name="database-program-transactions"></a>Adatbázis-program tranzakciók
Egy tipikus adatbázisban tranzakció munka egyetlen logikai egységként végrehajtott műveletek sorozata adható meg. Minden tranzakció biztosít **ACID garanciák**. SAV egy jól ismert mozaikszó négy tulajdonságai – Atomicity, konzisztencia, elkülönítési és tartósságot jelző.  

Röviden atomicity garantálja, hogy egyetlen egységként kezelt-e a minden hello dolgozott tranzakción belül hol vagy az összes véglegesítve vagy "none". Konzisztencia lehetővé teszi, hogy hello adatok mindig a megfelelő belső állapotban tranzakciók között. Elkülönítési garantálja, hogy két tranzakciók zavarják – általában, a legtöbb kereskedelmi rendszerek adjon meg több elkülönítési szinten használható hello alkalmazás igények alapján. Tartósságot biztosítja, hogy mindig lesz jelen hello adatbázis előjegyzett változásait.   

A Cosmos DB JavaScript üzemeltetett hello memóriaterületén hello adatbázis. Ezért, tárolt eljárások és eseményindítók kérelmek hajtható végre hello azonos hatókör egy adatbázis-munkamenet. Ez lehetővé teszi a Cosmos DB tooguarantee sav egyetlen tárolt eljárás vagy eseményindító részét képező minden műveletnél. Vegye figyelembe a következőket hello tárolt eljárás definíciója:

    // JavaScript source code
    var exchangeItemsSproc = {
        id: "exchangeItems",
        serverScript: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            var player1Document, player2Document;

            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);

                    if (documents.length != 1) throw "Unable toofind both names";
                    player1Document = documents[0];

                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable toofind both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable tooread player details, abort ";
                });

            if (!accept) throw "Unable tooread player details, abort ";

            // swap hello two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;

                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable tooupdate player 1, abort ";

                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable tooupdate player 2, abort"
                            });

                        if (!accept2) throw "Unable tooupdate player 2, abort";
                    });

                if (!accept) throw "Unable tooupdate player 1, abort";
            }
        }
    }

    // register hello stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Ez a tárolt eljárás tranzakciók egy játék app tootrade elemek között két játékosok egyetlen művelettel belül használja. hello tárolt eljárás kísérletek tooread két dokumentumok argumentumként átadott minden megfelelő toohello player azonosítók. Ha mindkét player dokumentum található, majd hello tárolt eljárás frissítéséhez hello dokumentumok csere az elemeket. Ha bármilyen hiba történik mentén hello módja, azt, amely implicit módon a hello tranzakció megszakítása JavaScript kivételt okoz.

Ha hello gyűjtemény hello tárolt eljárás regisztrálva van-e elleni egypartíciós gyűjtemény, akkor hello tranzakció hatókörön belüli tooall hello dokumentumok hello gyűjteményen belül. Ha hello gyűjtemény particionálva van, majd a tárolt eljárások hello tranzakció hatókörében egypartíciós kulcs lesznek végrehajtva. Minden egyes tárolt eljárás végrehajtása majd tartalmaznia kell egy megfelelő toohello hatókör hello transaction kell futnia a partíciós kulcs értéke. További részletekért lásd: [Azure Cosmos DB particionálás](partition-data.md).

### <a name="commit-and-rollback"></a>Érvényesítés és visszaállítás
Tranzakciók mélyen és natív módon integrálva vannak Cosmos DB JavaScript programozási modellt. A JavaScript függvényen belül minden műveletet vannak automatikusan végett az egyetlen tranzakció alatt. Ha hello JavaScript befejezése nélkül kivételen hello műveletek toohello adatbázis lépnek. Gyakorlatilag hello "BEGIN TRANSACTION" és "COMMIT TRANSACTION" utasítás a relációs adatbázisok implicit Cosmos DB-ben.  

Ha bármely hello parancsfájlból propagálja kivételek, Cosmos DB a JavaScript futásidejű visszaállítja hello teljes tranzakciót. Hello korábban ismertetett módon, egy kivétel kiváltása példája hatékonyan egyenértékű tooa Cosmos DB "ROLLBACK TRANSACTION".

### <a name="data-consistency"></a>Adatkonzisztencia
Tárolt eljárások és eseményindítók mindig végre hello Azure Cosmos DB tároló hello elsődleges replikán. Ez biztosítja, hogy az olvasások belül tárolt eljárások ajánlat az erős konzisztencia. Elsődleges hello vagy bármely másodlagos replikája lekérdezések felhasználó által definiált függvények használatával hajtható végre, de toomeet biztosítható hello konzisztenciaszint kért hello megfelelő replika kiválasztásával.

## <a name="bounded-execution"></a>A kötött végrehajtása
Minden Cosmos DB műveletet kell végeznie a megadott hello kiszolgálón belüli kérelmek időtúllépési időtartama. Ennél a határértéknél tooJavaScript funkciók (tárolt eljárások, eseményindítók és felhasználó által definiált függvények) is vonatkozik. Ha egy művelet nem fejeződött be az ezt az időkorlátot, hello tranzakció vissza lesz állítva. JavaScript-funkcióként kell hello időn belül befejezni vagy egy alapú folytatási modell toobatch/Folytatás végrehajtása.  

Rendelés toosimplify fejlesztése tárolt eljárások és eseményindítók toohandle sokáig tartott, minden funkciók alatt hello gyűjtési objektumot (létrehozása, olvasása, cserélje le és törölje a dokumentumok és mellékletek) jelző logikai érték adja vissza. e, amely fogja végrehajtani a műveletet. Ha ez az érték hamis, akkor arra utal, hogy hello határidőn tooexpire kapcsolatos és hello eljárás mentése végrehajtási burkolnia kell.  Műveletek aszinkron előzetes toohello első elfogadhatatlan adattárolási művelet garantáltan toocomplete, ha hello tárolt eljárás idő alatt fejeződik be, és nem várólistára további kérelmeket.  

JavaScript-funkcióként is kötve van a hálózatierőforrás-fogyasztás. Cosmos DB fenntart egy adatbázis-fiók üzembe hello méretének gyűjteményenként átviteli sebesség. Átviteli sebesség a CPU, a memória és a kérelemegység vagy RUs IO fogyasztás normalizált egységben van kifejezve. JavaScript-funkcióként is használhat egy nagy számú RUs rövid időn belül, és előfordulhat, hogy olvasson sebessége korlátozott hello gyűjtemény korlát elérésekor. Erőforrás intenzív tárolt eljárások is egyszerű adatbázis-műveletek karanténba zárt tooensure rendelkezésre állását.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Példa: Tömeges adatok importálása egy adatbázis-program
Alább példája toobulk-importálási dokumentumok gyűjteménybe írt tárolt eljárást. Megjegyzés: hogyan hello tárolt eljárás kezeli, amelyet végrehajtása hello logikai ellenőrzésével visszatérési érték a Documentclient, és majd használ hello között kötegek egyes elindításaihoz hello tárolt eljárás tootrack és folytatása folyamatban van a dokumentumok száma.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();

        // hello count of imported docs, also used as current doc index.
        var count = 0;

        // Validate input.
        if (!docs) throw new Error("hello array is undefined or null.");

        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }

        // Call hello create API toocreate a document.
        tryCreate(docs[count], callback);

        // Note that there are 2 exit conditions:
        // 1) hello createDocument request was not accepted. 
        //    In this case hello callback will not be called, we just call setBody and we are done.
        // 2) hello callback was called docs.length times.
        //    In this case all documents were created and we don’t need toocall tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);

            // If hello request was accepted, callback will be called.
            // Otherwise report current count back toohello client, 
            // which will call hello script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }

        // This is called when collection.createDocument is done in order tooprocess hello result.
        function callback(err, doc, options) {
            if (err) throw err;

            // One more document has been inserted, increment hello count.
            count++;

            if (count >= docsLength) {
                // If we created all documents, we are done. Just set hello response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Adatbázis-eseményindítók
### <a name="database-pre-triggers"></a>Adatbázis előtti eseményindítók
Cosmos DB biztosít, amelyek végrehajtása, vagy egy műveletet a dokumentum által indított eseményindítók. Például lehetőségeiről előtti eseményindítót hoz létre egy dokumentumot – az előzetes eseményindító fog futni, hello dokumentum létrehozása előtt. hello az alábbiakban látható egy példa előtti eseményindítók hogyan lehet a létrehozandó dokumentum használt toovalidate hello tulajdonságainak:

    var validateDocumentContentsTrigger = {
        id: "validateDocumentContents",
        serverScript: function validate() {
            var context = getContext();
            var request = context.getRequest();

            // document toobe created in hello current operation
            var documentToCreate = request.getBody();

            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }

            // update hello document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


És Node.js ügyféloldali regisztrációja kód hello eseményindító megfelelő hello:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };

            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Előtti eseményindítók nem tartozhat bemeneti paraméter. hello kérelem objektum használt toomanipulate hello társított kérelemüzenethez tartozó hello művelet lehet. Itt hello előtti eseményindító hello létrehozása egy dokumentum futtatják, és hello kérelem üzenettörzs tartalmazza hello dokumentum toobe JSON formátumú.   

Eseményindítók regisztrált, amikor a felhasználók megadhatják való futtatás hello műveletek. Ehhez az eseményindítóhoz TriggerOperation.Create, ami azt jelenti, nem engedélyezett a következő hello hozták létre.

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Adatbázis utáni eseményindítók
Utáni eseményindítók előtti eseményindítók, például egy műveletet a dokumentum tartoznak, és nem helyez el a bemeneti paramétereket. Futnak **után** hello művelet befejeződött, és hozzáférést toohello válaszüzenetet küldött toohello ügyfél rendelkezik.   

a következő példa hello utáni eseményindítók működés közben látható:

    var updateMetadataTrigger = {
        id: "updateMetadata",
        serverScript: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            // document that was created
            var createdDocument = response.getBody();

            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable tooupdate metadata, abort";

            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable toofind metadata document';

                         var metadataDocument = documents[0];

                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable tooupdate metadata, abort";
                               });
                         if(!accept) throw "Unable tooupdate metadata, abort";
                         return;                    
            }                                                                                            
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


hello eseményindító regisztrálható, ahogy az a következő minta hello.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "hello Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };

            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Ehhez az eseményindítóhoz lekérdezi a hello metaadat-dokumentum, és frissíti azt az újonnan létrehozott hello dokumentum adatait.  

Egyik dolog, ami fontos toonote hello **tranzakciós** Cosmos DB eseményindítók végrehajtását. Ez utáni eseményindító hello részeként fut ugyanabban a tranzakcióban, hello hello eredeti dokumentumhoz létrehozása. Ezért hello utáni eseményindítóval (például ha folyamatban, nem tooupdate hello metaadat-dokumentum) azt kivételt jelez, ha hello teljes tranzakció sikertelen lesz, és vissza lesz vonva. Nincs dokumentum jön létre, és kivételt adja vissza.  

## <a id="udf"></a>Felhasználó által definiált függvények
Felhasználói függvény (UDF) használt tooextend hello DocumentDB API SQL-lekérdezési nyelv szintaxis és üzleti logika megvalósításához. Ezek csak a hívható lekérdezéseken belül. Ugyanakkor nem rendelkeznek hozzáféréssel toohello környezeti objektumot, és csak számítási JavaScript használt toobe úgy van kialakítva. Ezért a felhasználó által megadott függvények másodlagos replikáin hello Cosmos DB szolgáltatás futtatható.  

hello alábbi minta létrehoz egy UDF toocalculate adó különböző bevétel zárójeleket sebességet alapján, és azt használja a lekérdezés toofind belül minden olyan személyek, akik több mint 20 000 $ fizetett adók.

    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


hello UDF ezt követően használható a következő minta hello például a lekérdezések:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);

            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>A JavaScript nyelv integrált lekérdezés API
Ezenkívül tooissuing lekérdezések DocumentDB SQL-szintaxis használatával, hello kiszolgálóoldali SDK lehetővé teszi, az SQL ismeretek nélkül Folyékonyan beszél JavaScript-illesztő segítségével tooperform optimalizált lekérdezések. API-jával tooprogrammatically build lekérdezések úgy, hogy a predikátum függvény chainable függvénynek hello JavaScript lekérdezés hívja, egy szintaxisa tisztában tooECMAScript5 tömb built-ins és lodash például népszerű JavaScript szalagtárak. Lekérdezések által hello JavaScript futásidejű toobe végre hatékonyan Azure Cosmos DB indexek elemzésének.

> [!NOTE]
> `__`(kettős-aláhúzásjel) túl alias`getContext().getCollection()`.
> <br/>
> Más szóval használhatja `__` vagy `getContext().getCollection()` tooaccess hello JavaScript lekérdezés API.
> 
> 

Támogatott funkciók a következők:

<ul>
<li>
<b>CHAIN().... érték ([visszahívási] [, beállítások])</b>
<ul>
<li>
Value() – amely kell lezárni láncolt hívás kezdődik.
</li>
</ul>
</li>
<li>
<b>szűrő (predicateFunction [, beállítások] [, visszahívási])</b>
<ul>
<li>
Adjon meg egy ad vissza igaz/hamis értékű, a rendelés toofilter kimeneti/bemeneti dokumentumok hello eredő a predikátum függvény használatával hello szűrők. Ez úgy viselkedik, hasonló tooa SQL WHERE záradékban.
</li>
</ul>
</li>
<li>
<b>térkép (transformationFunction [, beállítások] [, visszahívási])</b>
<ul>
<li>
Egy transzformációs függvény, amely minden egyes bemeneti elem tooa JavaScript-objektum vagy megadott leképezés vonatkozik. Ez úgy viselkedik, hasonló tooa SELECT záradékban az SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, beállítások] [, visszahívási])</b>
<ul>
<li>
Ez egy olyan térképet, amely egyetlen tulajdonság értékének hello összes beviteli elemet a parancsikont.
</li>
</ul>
</li>
<li>
<b>egybesimítására ([isShallow] [, beállítások] [, visszahívási])</b>
<ul>
<li>
Egyesíti, és minden tooa egyetlen tömb bemeneti eleménél tömbök simítja. A LINQ hasonló tooSelectMany ez viselkedik.
</li>
</ul>
</li>
<li>
<b>a sortBy ([predicate] [, beállítások] [, visszahívási])</b>
<ul>
<li>
Létrehoznak egy új dokumentumot hello dokumentumok, a bemeneti dokumentum-adatfolyamra hello növekvő sorrendben predikátumban megadott hello segítségével rendezésével. Ez úgy viselkedik, hasonló tooa ORDER BY záradékban az SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, beállítások] [, visszahívási])</b>
<ul>
<li>
Létrehoznak egy új dokumentumot hello dokumentumok, a bemeneti dokumentum-adatfolyamra hello csökkenő sorrendben predikátumban megadott hello segítségével rendezésével. Ez úgy viselkedik, hasonló tooa x DESC ORDER BY záradékban az SQL.
</li>
</ul>
</li>
</ul>


Amikor bekerüljön predikátum és/vagy választó funkciók, hello következő JavaScript-szerkezetek kérjen automatikusan optimalizált toorun közvetlenül a Azure Cosmos DB indexek:

* Egyszerű operátorok: = + - * / % |} ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Szövegkonstans, többek között a hello objektum szövegkonstans: {}
* var, visszatérési

hello hoz létre a következő JavaScript nem Azure Cosmos DB indexek az beszerzése optimalizált:

* Szabályozhatja a folyamat (pl. Ha, közben)
* Függvényhívások

További információkért lásd: a [kiszolgálóoldali JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-hello-javascript-query-api"></a>Példa: Írási hello JavaScript API-jával lekérdezés tárolt eljárás
a következő példakód hello példája hogyan hello JavaScript lekérdezés API hello környezetben tárolt eljárás használható. hello tárolt eljárás szúr be egy dokumentum, egy bemeneti paraméter által megadott, és frissíti a metaadat-dokumentum, hello segítségével `__.filter()` metódus minSize, a maxSize és a totalSize hello bemeneti dokumentum size tulajdonság alapján.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent tooour callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check hello doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get hello meta document. We keep it in hello same collection. it's hello only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed toofind hello metadata document.");

            // hello metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all hello rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace hello metadata document in hello store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read hello meta again 
              //       and update again because due tooSnapshot isolation we will read same exact version (we are in same transaction).
              //       We have tootake care of that on hello client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-toojavascript-cheat-sheet"></a>SQL tooJavascript Adatlap
hello alábbi táblázat mutatja be különböző SQL-lekérdezések és hello megfelelő JavaScript lekérdezések.

Az SQL-lekérdezések, a dokumentum tulajdonság kulcsok, (pl. `doc.id`)-és nagybetűk.

|SQL| JavaScript lekérdezés API|Az alábbi leírása|
|---|---|---|
|VÁLASSZA KI *<br>A dokumentumok| __.Map(Function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc;<br>});|1|
|Jelölje be docs.id, mint docs.message adatköltségek, docs.actions <br>A dokumentumok|__.Map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;{visszaadása<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;azonosító: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;üzenet: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;actions:doc.Actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|VÁLASSZA KI *<br>A dokumentumok<br>HOL docs.id="X998_Y998"|__.Filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc.id === "X998_Y998";<br>});|3|
|VÁLASSZA KI *<br>A dokumentumok<br>HOL ARRAY_CONTAINS (dokumentumok. Címkék, 123)|__.Filter(Function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a x.Tags & & x.Tags.indexOf(123) > -1;<br>});|4|
|Jelölje be docs.id, docs.message adatköltségek,<br>A dokumentumok<br>HOL docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc.id === "X998_Y998";<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.Map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{visszaadása<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;azonosító: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;üzenet: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|5|
|A SELECT VALUE címke<br>A dokumentumok<br>CSATLAKOZTASSA a docs címke. Címkék<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a dokumentumot. Címkék & & Array.isArray (doc. Címkék);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;térjen vissza a doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|6|

hello következő leírások ismertetik a fenti hello tábla minden egyes lekérdezés.
1. Az összes dokumentumok (a folytatási kód paginated) eredményez.
2. Projektek hello azonosítója, az üzenet (aliasnevet toomsg) és a művelet az összes dokumentumot.
3. A dokumentumok hello predikátum lekérdezések: azonosító = "X998_Y998".
4. A lekérdezések, amelyek a Tags tulajdonság és címkék dokumentumok 123 hello értéket tartalmazó tömb.
5. Lekérdezések dokumentumok predikátumával, azonosító = "X998_Y998", és ezután projektek hello azonosítója és (aliasnevet toomsg) üzenet.
6. Szűrők tömbtulajdonság, címkék, amelyeknek dokumentumok és hello eredményül kapott dokumentumok rendezése hello _ts időbélyeg-rendszer tulajdonság, majd projektek + simítja hello címkék tömb.


## <a name="runtime-support"></a>Futásidejű támogatása
[A DocumentDB a JavaScript kiszolgáló oldalán API](http://azure.github.io/azure-documentdb-js-server/) támogatást nyújt az hello hello többsége alapvető technikai JavaScript nyelv szolgáltatások által szabványosított [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Biztonság
JavaScript tárolt eljárások és eseményindítók elkülönített, hogy egy parancsfájl hello hatásait nem szivárognak más toohello hello pillanatkép-tranzakció elkülönítés hello adatbázis szintjén áthaladás nélkül. hello futtatási környezetekben a készletbe vonást, de tisztítani hello környezet után minden egyes futtatásához. Ezért ezek garantáltan toobe biztonságos az oldal nem várt hatások egymástól.

### <a name="pre-compilation"></a>A fordítás előtti
Tárolt eljárások, eseményindítók és felhasználó által megadott függvények, hello minden parancsfájl foglalja találhatók az implicit módon lefordított toohello bájt kód formátum rendelés tooavoid fordítási költséggel jár. Ez biztosítja a tárolt eljárások indítások gyors, és egy kis erőforrásigényét.

## <a name="client-sdk-support"></a>Ügyfél SDK-támogatás
A hozzáadása toohello DocumentDB API a [Node.js](documentdb-sdk-node.md) ügyfél, Azure Cosmos DB rendelkezik [.NET](documentdb-sdk-dotnet.md), [.NET Core](documentdb-sdk-dotnet-core.md), [Java](documentdb-sdk-java.md), [ JavaScript](http://azure.github.io/azure-documentdb-js/), és [Python SDK-k](documentdb-sdk-python.md) hello DocumentDB API számára. Tárolt eljárások, eseményindítók és felhasználó által megadott függvények hozhatók létre, és végre bármely, valamint a SDK használatával. a következő példa azt mutatja meg hogyan hello toocreate, majd hajtsa végre a tárolt eljárás hello .NET ügyfél használatával. Vegye figyelembe, hogyan hello .NET típusok átadott hello JSON-ként tárolt eljárást, és olvasási vissza.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    

                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }

                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };

    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;

    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "sproc"), document, 1920);


Ez a példa bemutatja, hogyan toouse hello [DocumentDB .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) toocreate előtti eseményindítót, és hozzon létre egy dokumentum hello eseményindító engedélyezve van. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };

    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


És hello a következő példa bemutatja, hogyan toocreate egy felhasználói függvény (UDF), és ezért a [DocumentDB API SQL-lekérdezés](documentdb-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };

    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API
Minden Azure Cosmos DB műveletet RESTful módon végezheti el. Tárolt eljárások, eseményindítók és felhasználó által definiált függvények regisztrálhatók egy gyűjteményt a HTTP POST használatával. hello az alábbiakban látható egy példa bemutatja, hogyan tooregister tárolt eljárást:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


hello tárolt eljárás regisztrálva van-e a POST kérelmet hello URI végrehajtásával adatbázisok/testdb/colls/testColl/sprocs hello törzs tartalmazó a tárolt eljárás toocreate hello. Eseményindítók és felhasználó által megadott függvények hasonlóan a FELADÁS egy vagy több/eseményindítók és /udfs rendre kiállításával regisztrálható.
Ez tárolt eljárást is, majd a POST kérelmet az erőforrás-hivatkozás kiállításával hajtható végre:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of hello Patriarch"}, "Price", 200 ]


Itt hello bemeneti toohello tárolt eljárás lett átadva hello kérés törzsében. Vegye figyelembe, hogy hello bemeneti, egy JSON-tömb, a bemeneti paraméterek átadása. hello tárolt eljárás vesz hello első bemenet, amely egy adott válasz törzsének-dokumentumként. hello válasz érkező a következőképpen történik:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of hello Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


Ellentétben a tárolt eljárások, eseményindítók közvetlenül nem hajtható végre. Ehelyett végrehajtás egy dokumentumot egy művelet részeként. A kérelem HTTP-fejlécek használatával hello eseményindítók toorun azt adhatja meg. hello kérelem toocreate dokumentum látható.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “hello Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Itt hello előtti eseményindító toobe hello kérelem hibaüzenettel hello x-ms-documentdb-pre-trigger-include fejlécben megadott. Ennek megfelelően a utáni eseményindítókat hello x-ms-documentdb-post-trigger-include fejléc szerepelnek. Vegye figyelembe, hogy mindkét előtti és utáni eseményindítók adható meg egy adott kérés esetében.

## <a name="sample-code"></a>Mintakód
További példákat kiszolgálóoldali található (beleértve a [tömeges törlési](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), és [frissítése](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) a a [GitHub-tárházban](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Tooshare szeretné, hogy a Soft tárolt eljárás? Kérjük küldjön egy lekérést! 

## <a name="next-steps"></a>Következő lépések
Miután egy vagy több tárolt eljárások, eseményindítók és felhasználó által definiált függvények létrehozott, betöltve helyezheti, és megtekintheti hello adatkezelő Azure portálra.

Azt is tapasztalhatja hello következő hivatkozások és erőforrások az az elérési út toolearn Azure Cosmos dB kiszolgálóoldali programozása kapcsolatos további hasznos információkat:

* [Az Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md)
* [A DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Biztonságos és hordozható adatbázis bővíthetőség](http://dl.acm.org/citation.cfm?id=276339) 
* [Szolgáltatás készült adatbázis-architektúra](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [A Microsoft SQL server üzemeltetési hello .NET-futtatókörnyezet](http://dl.acm.org/citation.cfm?id=1007669)

