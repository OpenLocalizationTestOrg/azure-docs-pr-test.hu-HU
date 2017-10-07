---
title: "hello DocumentDB API for Azure Cosmos DB aaaNode.js oktatóanyaga |} Microsoft Docs"
description: "A Node.js-oktatóanyag, amely egy Cosmos DB hello DocumentDB API hoz létre."
keywords: "node.js-oktatóanyag, node-adatbázis"
services: cosmos-db
documentationcenter: node.js
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: 14d52110-1dce-4ac0-9dd9-f936afccd550
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: node
ms.topic: article
ms.date: 08/14/2017
ms.author: anhoh
ms.openlocfilehash: fce244c6a5f321608e82ca51a2c987e84b98bffa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="nodejs-tutorial-use-hello-documentdb-api-in-azure-cosmos-db-toocreate-a-nodejs-console-application"></a>NODE.js-oktatóanyag: az Azure Cosmos DB toocreate egy Node.js-Konzolalkalmazás a DocumentDB API használata hello
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js MongoDB-hez](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Üdvözli a toohello Node.js-oktatóanyag hello Azure Cosmos DB Node.js SDK-t! Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.

Az oktatóanyag a következőket ismerteti:

* Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók
* Az alkalmazás beállítása
* Node-adatbázis létrehozása
* Gyűjtemény létrehozása
* JSON-dokumentumok létrehozása
* Hello gyűjtemény lekérdezése
* Dokumentum cseréje
* Dokumentum törlése
* Hello node-adatbázis törlése

Nincs elég ideje? Ne aggódjon! a teljes megoldás hello érhető [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started). Lásd: [hello teljes megoldás beszerzése](#GetSolution) gyors utasításokért.

Hello Node.js-oktatóanyag befejezése után adjon használata hello szavazás gombok hello top és a lap toogive alján nekünk visszajelzést. Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude a megjegyzéseit.

Most pedig lássunk neki!

## <a name="prerequisites-for-hello-nodejs-tutorial"></a>Hello Node.js-oktatóanyag előfeltételei
Győződjön meg arról, hogy a következő hello:

* Aktív Azure-fiók. Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).
    * Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.
* [Node.js](https://nodejs.org/)-verzió: 0.10.29-s vagy újabb.

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. lépés: Azure Cosmos DB-fiók létrehozása
Hozzunk létre egy Azure Cosmos DB-fiókot. Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a Node.js-alkalmazás beállítása](#SetupNode). Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Node.js-alkalmazás beállítása](#SetupNode).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>2. lépés: A Node.js-alkalmazás beállítása
1. Nyissa meg kedvenc terminálját.
2. Keresse meg toosave helyének hello mappa- vagy a Node.js-alkalmazás.
3. Hozzon létre két üres JavaScript-fájlt a következő parancsok hello:
   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```
4. Hello documentdb modult az npm telepítése. A következő parancs hello használata:
   * ```npm install documentdb --save```

Remek! A beállítás befejeztével nekiláthat a kód írásának.

## <a id="Config"></a>3. lépés: Az alkalmazás konfigurációnak megadása
Nyissa meg a ```config.js``` fájlt egy tetszőleges szövegszerkesztőben.

Ezután, másolás és beillesztés hello kódrészletben és tulajdonságainak beállítása ```config.endpoint``` és ```config.primaryKey``` tooyour Azure Cosmos DB-végpontjának uri és elsődleges kulcs. Mindkettő konfiguráció megtalálható hello [Azure-portálon](https://portal.azure.com).

![A kijelölt node.js-oktatóanyag – képernyőfelvétel a hello Azure-portál, Azure Cosmos DB adatait, az ACTIVE központ hello megjelenítő lévő hello Azure Cosmos DB fiók panelen, és hello URI, elsődleges és másodlagos kulcsot értékek vannak kiemelve a hello hello KEYS gomb Kulcsok panelről – Node-adatbázis][keys]

    // ADD THIS PART tooYOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

Másolja és illessze be a hello ```database id```, ```collection id```, és ```JSON documents``` tooyour ```config``` objektum az az alábbi amelyen beállíthatja a ```config.endpoint``` és ```config.authKey``` tulajdonságok. Ha van olyan adat, milyen toostore az adatbázisban, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) hello dokumentum-definíciók hozzáadása helyett.

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART tooYOUR CODE
    config.database = {
        "id": "FamilyDB"
    };

    config.collection = {
        "id": "FamilyColl"
    };

    config.documents = {
        "Andersen": {
            "id": "Anderson.1",
            "lastName": "Andersen",
            "parents": [{
                "firstName": "Thomas"
            }, {
                    "firstName": "Mary Kay"
                }],
            "children": [{
                "firstName": "Henriette Thaulow",
                "gender": "female",
                "grade": 5,
                "pets": [{
                    "givenName": "Fluffy"
                }]
            }],
            "address": {
                "state": "WA",
                "county": "King",
                "city": "Seattle"
            }
        },
        "Wakefield": {
            "id": "Wakefield.7",
            "parents": [{
                "familyName": "Wakefield",
                "firstName": "Robin"
            }, {
                    "familyName": "Miller",
                    "firstName": "Ben"
                }],
            "children": [{
                "familyName": "Merriam",
                "firstName": "Jesse",
                "gender": "female",
                "grade": 8,
                "pets": [{
                    "givenName": "Goofy"
                }, {
                        "givenName": "Shadow"
                    }]
            }, {
                    "familyName": "Miller",
                    "firstName": "Lisa",
                    "gender": "female",
                    "grade": 1
                }],
            "address": {
                "state": "NY",
                "county": "Manhattan",
                "city": "NY"
            },
            "isRegistered": false
        }
    };


hello adatbázis, gyűjtemény és dokumentum-definíciók fog működni a Azure Cosmos DB ```database id```, ```collection id```, és dokumentumadataiként.

Végül exportálja a ```config``` objektumot, hogy hivatkozhasson rá belül hello ```app.js``` fájlt.

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART tooYOUR CODE
    module.exports = config;

## <a id="Connect"></a>4. lépés: Csatlakozás tooan Azure Cosmos DB fiók
Nyissa meg az üres ```app.js``` fájl hello szövegszerkesztőben. Másolja és illessze be az alábbi tooimport hello hello kód ```documentdb``` modul, és az újonnan létrehozott ```config``` modul.

    // ADD THIS PART tooYOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

Másolja és illessze be a korábban mentett hello kód toouse hello ```config.endpoint``` és ```config.primaryKey``` toocreate egy új documentclient-ügyfelet.

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART tooYOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

Most, hogy hello kód tooinitialize hello Azure Cosmos DB ügyfél, vessen egy pillantást a Azure Cosmos DB erőforrásokat.

## <a name="step-5-create-a-node-database"></a>5. lépés: Node-adatbázis létrehozása
Másolással illessze be a hello kódot alább tooset hello HTTP-állapot nem található, hello adatbázis URL-címét és hello gyűjtemény URL-címét. Ezen URL-címei, hogyan hello Azure Cosmos DB ügyfél megtalálja a megfelelő hello adatbázis és gyűjtemény.

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART tooYOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

A [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) hello funkcióját **DocumentClient** osztály. Egy adatbázis a dokumentumtároló gyűjtemények között particionált logikai tárolója hello.

Másolja és illessze be a hello **getDatabase** hello hello app.js fájlban az új adatbázis létrehozása függvény ```id``` hello megadott ```config``` objektum. hello függvény ellenőrzi, hogy ugyanaz a hello adatbázis hello ```FamilyRegistry``` azonosítója már nem létezik. Ha már létezik, a rendszer új adatbázis létrehozása helyett visszaadja a már létezőt.

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART tooYOUR CODE
    function getDatabase() {
        console.log(`Getting database:\n${config.database.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDatabase(databaseUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDatabase(config.database, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Másolja és illessze be az alábbi hello kód, amelyen meg hello **getDatabase** tooadd hello segítő funkció működéséhez **kilépéshez** , hogy kiírja hello kilépési üzenetet, valamint hello hívás túl**getDatabase** függvény.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key tooexit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Gratulálunk! Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.

## <a id="CreateColl"></a>6. lépés: Gyűjtemény létrehozása
> [!WARNING]
> A **CreateDocumentCollectionAsync** létrehoz egy új gyűjteményt, amely költségeket von maga után. További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) hello funkcióját **DocumentClient** osztály. A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.

Másolja és illessze be a hello **getCollection** függvény alá hello **getDatabase** működni hello app.js fájl toocreate gyűjtemény az hello ```id``` hello megadott```config```objektum. Ebben az esetben toomake ellenőrizzük, hogy egy gyűjtemény hello azonos ```FamilyCollection``` azonosítója már nem létezik. Ha már létezik, a rendszer új gyűjtemény létrehozása helyett visszaadja a már létezőt.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function getCollection() {
        console.log(`Getting collection:\n${config.collection.id}\n`);

        return new Promise((resolve, reject) => {
            client.readCollection(collectionUrl, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createCollection(databaseUrl, config.collection, { offerThroughput: 400 }, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    }

Másolja és illessze be a hello hívás alatt hello kód túl**getDatabase** tooexecute hello **getCollection** függvény.

    getDatabase()

    // ADD THIS PART tooYOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Gratulálunk! Sikeresen létrehozott egy Azure Cosmos DB gyűjteményt.

## <a id="CreateDoc"></a>7. lépés: Dokumentum létrehozása
A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://azure.github.io/azure-documentdb-node/DocumentClient.html) hello funkcióját **DocumentClient** osztály. A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak. Most már beszúrhat egy dokumentumot az Azure Cosmos DB-be.

Másolja és illessze be a hello **getFamilyDocument** függvény alá hello **getCollection** függvényt az hello hello mentett JSON-adatokat tartalmazó hello dokumentumok ```config``` objektum. Ebben az esetben ellenőrizzük toomake meg arról, hogy a dokumentum hello ugyanazzal az azonosítóval már nem létezik.

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function getFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Getting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.readDocument(documentUrl, { partitionKey: document.district }, (err, result) => {
                if (err) {
                    if (err.code == HttpStatusCodes.NOTFOUND) {
                        client.createDocument(collectionUrl, document, (err, created) => {
                            if (err) reject(err)
                            else resolve(created);
                        });
                    } else {
                        reject(err);
                    }
                } else {
                    resolve(result);
                }
            });
        });
    };

Másolja és illessze be a hello hívás alatt hello kód túl**getCollection** tooexecute hello **getFamilyDocument** függvény.

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Gratulálunk! Sikeresen létrehozott egy Azure Cosmos DB dokumentumot.

![NODE.js-oktatóanyag – hello fiók, hello adatbázis, gyűjtemény hello és hello dokumentumok hierarchikus kapcsolatát hello ábrázoló Diagram – Node-adatbázis](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <a id="Query"></a>8. lépés: Az Azure Cosmos DB-erőforrások lekérdezése
Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md). hello alábbi mintakód bemutatja egy lekérdezést, amely alapján futtathatók hello dokumentumok a gyűjteményben.

Másolja és illessze be a hello **queryCollection** függvény alá hello **getFamilyDocument** függvény hello app.js fájlban. Azure Cosmos-adatbázis SQL-szerű lekérdezéseket támogatja az alább látható módon. Bonyolult lekérdezések felépítésével további információkért tekintse meg a hello [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo) és hello [lekérdezésekre vonatkozó dokumentációt](documentdb-sql-query.md).

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function queryCollection() {
        console.log(`Querying collection through index:\n${config.collection.id}`);

        return new Promise((resolve, reject) => {
            client.queryDocuments(
                collectionUrl,
                'SELECT VALUE r.children FROM root r WHERE r.lastName = "Andersen"'
            ).toArray((err, results) => {
                if (err) reject(err)
                else {
                    for (var queryResult of results) {
                        let resultString = JSON.stringify(queryResult);
                        console.log(`\tQuery returned ${resultString}`);
                    }
                    console.log();
                    resolve(results);
                }
            });
        });
    };


hello a következő ábra szemlélteti, hogyan hello Azure Cosmos adatbázis SQL-lekérdezés szintaxisa hívást hello gyűjtemény létrehozása.

![NODE.js-oktatóanyag – és hello lekérdezés jelentését hello hatókör ábrázoló Diagram – Node-adatbázis](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

Hello [FROM](documentdb-sql-query.md#FromClause) kulcsszó nem kötelező, hello lekérdezésben, mert Azure Cosmos DB lekérdezések már hatókörön belüli tooa egyetlen gyűjtemény. Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre. Az Azure Cosmos DB következtethető ki, hogy családok, legfelső szintű vagy hello változó neve választja, hivatkozás hello aktuális gyűjtemény alapértelmezés szerint.

Másolja és illessze be a hello hívás alatt hello kód túl**getFamilyDocument** tooexecute hello **queryCollection** függvény.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART tooYOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Gratulálunk! Sikeresen lekérdezett Azure Cosmos DB-dokumentumokat.

## <a id="ReplaceDocument"></a>9. lépés: Dokumentum cseréje
Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.

Másolja és illessze be a hello **replaceFamilyDocument** függvény alá hello **queryCollection** függvény hello app.js fájlban.

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART tooYOUR CODE
    function replaceFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Replacing document:\n${document.id}\n`);
        document.children[0].grade = 6;

        return new Promise((resolve, reject) => {
            client.replaceDocument(documentUrl, document, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Másolja és illessze be a hello hívás alatt hello kód túl**queryCollection** tooexecute hello **replaceDocument** függvény. Továbbá adja hozzá a hello kód toocall **queryCollection** újra tooverify hello a dokumentum módosítása sikeres volt.

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Gratulálunk! Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.

## <a id="DeleteDocument"></a>10. lépés: Dokumentum törlése
Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.

Másolja és illessze be a hello **deleteFamilyDocument** függvény alá hello **replaceFamilyDocument** függvény.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function deleteFamilyDocument(document) {
        let documentUrl = `${collectionUrl}/docs/${document.id}`;
        console.log(`Deleting document:\n${document.id}\n`);

        return new Promise((resolve, reject) => {
            client.deleteDocument(documentUrl, (err, result) => {
                if (err) reject(err);
                else {
                    resolve(result);
                }
            });
        });
    };

Másolja és illessze be a hello kódot alább hello hívás toohello második **queryCollection** tooexecute hello **deleteDocument** függvény.

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Gratulálunk! Sikeresen törölt egy Azure Cosmos DB-dokumentumot.

## <a id="DeleteDatabase"></a>11. lépés: Hello Node-adatbázis törlése
A létrehozott adatbázis törlésével hello eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).

Másolja és illessze be a hello **tisztítás** függvény alá hello **deleteFamilyDocument** tooremove hello adatbázis és az összes hello gyermekerőforrás működik.

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART tooYOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

Másolja és illessze be a hello hívás alatt hello kód túl**deleteFamilyDocument** tooexecute hello **tisztítás** függvény.

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART tooYOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <a id="Run"></a>12. lépés: A teljes Node.js-alkalmazás futtatása
Hello feladatütemezési a függvényeket meghívó teljes sorozatnak így példához hasonló:

    getDatabase()
    .then(() => getCollection())
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```

Az első lépések alkalmazás kimenetének hello kell megjelennie. hello kimeneti meg kell felelnie az alábbi hello példaszöveggel.

    Getting database:
    FamilyDB

    Getting collection:
    FamilyColl

    Getting document:
    Anderson.1

    Getting document:
    Wakefield.7

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing document:
    Anderson.1

    Querying collection through index:
    FamilyColl
        Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleting document:
    Anderson.1

    Cleaning up by deleting database FamilyDB
    Completed successfully
    Press any key tooexit

Gratulálunk! Létrehozott, hello Node.js-oktatóanyag elvégzése után, és rendelkezik az első Azure Cosmos DB konzolalkalmazást!

## <a id="GetSolution"></a>Hello teljes Node.js oktatóanyag megoldás beszerzése
Ha még nem idő toocomplete lépéseit az oktatóanyag hello, vagy csak szeretné, hogy toodownload hello kódot, beszerezheti a [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).

toorun hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben, szüksége lesz a következő hello:

* [Azure Cosmos DB-fiók][create-account].
* Hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) megoldás elérhető a Githubon.

Telepítse a hello **documentdb** modult az npm. A következő parancs hello használata:

* ```npm install documentdb --save```

Ezután hello ```config.js``` fájl, a frissítés hello config.endpoint és config.authKey értékeket a [3. lépés: állítsa be az alkalmazás konfigurációnak](#Config). 

A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa: ```node app.js```.

Ennyi az egész! Építse ki, és máris jó úton jár! 

## <a name="next-steps"></a>Következő lépések
* Összetettebb Node.js-mintát szeretne használni? Lásd: [Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-nodejs-application.md).
* Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).
* A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).
* További tudnivalók a programozási modellt hello hello hello Develop szakasza [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/documentdb/).

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
