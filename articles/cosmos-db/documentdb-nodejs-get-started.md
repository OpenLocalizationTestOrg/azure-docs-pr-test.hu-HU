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
# <a name="nodejs-tutorial-use-hello-documentdb-api-in-azure-cosmos-db-toocreate-a-nodejs-console-application"></a><span data-ttu-id="af3ad-104">NODE.js-oktatóanyag: az Azure Cosmos DB toocreate egy Node.js-Konzolalkalmazás a DocumentDB API használata hello</span><span class="sxs-lookup"><span data-stu-id="af3ad-104">Node.js tutorial: Use hello DocumentDB API in Azure Cosmos DB toocreate a Node.js console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="af3ad-105">.NET</span><span class="sxs-lookup"><span data-stu-id="af3ad-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="af3ad-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="af3ad-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="af3ad-107">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="af3ad-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="af3ad-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="af3ad-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="af3ad-109">Java</span><span class="sxs-lookup"><span data-stu-id="af3ad-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="af3ad-110">C++</span><span class="sxs-lookup"><span data-stu-id="af3ad-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="af3ad-111">Üdvözli a toohello Node.js-oktatóanyag hello Azure Cosmos DB Node.js SDK-t!</span><span class="sxs-lookup"><span data-stu-id="af3ad-111">Welcome toohello Node.js tutorial for hello Azure Cosmos DB Node.js SDK!</span></span> <span data-ttu-id="af3ad-112">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="af3ad-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="af3ad-113">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="af3ad-113">We'll cover:</span></span>

* <span data-ttu-id="af3ad-114">Hoz létre és csatlakoztatja tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="af3ad-114">Creating and connecting tooan Azure Cosmos DB account</span></span>
* <span data-ttu-id="af3ad-115">Az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="af3ad-115">Setting up your application</span></span>
* <span data-ttu-id="af3ad-116">Node-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-116">Creating a node database</span></span>
* <span data-ttu-id="af3ad-117">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-117">Creating a collection</span></span>
* <span data-ttu-id="af3ad-118">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-118">Creating JSON documents</span></span>
* <span data-ttu-id="af3ad-119">Hello gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="af3ad-119">Querying hello collection</span></span>
* <span data-ttu-id="af3ad-120">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="af3ad-120">Replacing a document</span></span>
* <span data-ttu-id="af3ad-121">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="af3ad-121">Deleting a document</span></span>
* <span data-ttu-id="af3ad-122">Hello node-adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="af3ad-122">Deleting hello node database</span></span>

<span data-ttu-id="af3ad-123">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="af3ad-123">Don't have time?</span></span> <span data-ttu-id="af3ad-124">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="af3ad-124">Don't worry!</span></span> <span data-ttu-id="af3ad-125">a teljes megoldás hello érhető [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="af3ad-125">hello complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span> <span data-ttu-id="af3ad-126">Lásd: [hello teljes megoldás beszerzése](#GetSolution) gyors utasításokért.</span><span class="sxs-lookup"><span data-stu-id="af3ad-126">See [Get hello complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="af3ad-127">Hello Node.js-oktatóanyag befejezése után adjon használata hello szavazás gombok hello top és a lap toogive alján nekünk visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="af3ad-127">After you've completed hello Node.js tutorial, please use hello voting buttons at hello top and bottom of this page toogive us feedback.</span></span> <span data-ttu-id="af3ad-128">Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude a megjegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="af3ad-128">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="af3ad-129">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="af3ad-129">Now let's get started!</span></span>

## <a name="prerequisites-for-hello-nodejs-tutorial"></a><span data-ttu-id="af3ad-130">Hello Node.js-oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="af3ad-130">Prerequisites for hello Node.js tutorial</span></span>
<span data-ttu-id="af3ad-131">Győződjön meg arról, hogy a következő hello:</span><span class="sxs-lookup"><span data-stu-id="af3ad-131">Please make sure you have hello following:</span></span>

* <span data-ttu-id="af3ad-132">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="af3ad-132">An active Azure account.</span></span> <span data-ttu-id="af3ad-133">Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="af3ad-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
    * <span data-ttu-id="af3ad-134">Másik lehetőségként használhatja a hello [Azure Cosmos DB emulátor](local-emulator.md) ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="af3ad-134">Alternatively, you can use hello [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="af3ad-135">[Node.js](https://nodejs.org/)-verzió: 0.10.29-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="af3ad-135">[Node.js](https://nodejs.org/) version v0.10.29 or higher.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="af3ad-136">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="af3ad-137">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="af3ad-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="af3ad-138">Ha már rendelkezik toouse kívánt fiókkal, akkor kihagyhatja azokat, amelyek túl[a Node.js-alkalmazás beállítása](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="af3ad-138">If you already have an account you want toouse, you can skip ahead too[Set up your Node.js application](#SetupNode).</span></span> <span data-ttu-id="af3ad-139">Ha hello Azure Cosmos DB Emulator használata esetén kövesse hello készítésével [Azure Cosmos DB emulátor](local-emulator.md) toosetup hello emulátor, és hagyja ki azokat, amelyek túl[a Node.js-alkalmazás beállítása](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="af3ad-139">If you are using hello Azure Cosmos DB Emulator, please follow hello steps at [Azure Cosmos DB Emulator](local-emulator.md) toosetup hello emulator and skip ahead too[Set up your Node.js application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="af3ad-140"><a id="SetupNode"></a>2. lépés: A Node.js-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="af3ad-140"><a id="SetupNode"></a>Step 2: Set up your Node.js application</span></span>
1. <span data-ttu-id="af3ad-141">Nyissa meg kedvenc terminálját.</span><span class="sxs-lookup"><span data-stu-id="af3ad-141">Open your favorite terminal.</span></span>
2. <span data-ttu-id="af3ad-142">Keresse meg toosave helyének hello mappa- vagy a Node.js-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="af3ad-142">Locate hello folder or directory where you'd like toosave your Node.js application.</span></span>
3. <span data-ttu-id="af3ad-143">Hozzon létre két üres JavaScript-fájlt a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="af3ad-143">Create two empty JavaScript files with hello following commands:</span></span>
   * <span data-ttu-id="af3ad-144">Windows:</span><span class="sxs-lookup"><span data-stu-id="af3ad-144">Windows:</span></span>
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * <span data-ttu-id="af3ad-145">Linux/OS X:</span><span class="sxs-lookup"><span data-stu-id="af3ad-145">Linux/OS X:</span></span>
     * ```touch app.js```
     * ```touch config.js```
4. <span data-ttu-id="af3ad-146">Hello documentdb modult az npm telepítése.</span><span class="sxs-lookup"><span data-stu-id="af3ad-146">Install hello documentdb module via npm.</span></span> <span data-ttu-id="af3ad-147">A következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="af3ad-147">Use hello following command:</span></span>
   * ```npm install documentdb --save```

<span data-ttu-id="af3ad-148">Remek!</span><span class="sxs-lookup"><span data-stu-id="af3ad-148">Great!</span></span> <span data-ttu-id="af3ad-149">A beállítás befejeztével nekiláthat a kód írásának.</span><span class="sxs-lookup"><span data-stu-id="af3ad-149">Now that you've finished setting up, let's start writing some code.</span></span>

## <span data-ttu-id="af3ad-150"><a id="Config"></a>3. lépés: Az alkalmazás konfigurációnak megadása</span><span class="sxs-lookup"><span data-stu-id="af3ad-150"><a id="Config"></a>Step 3: Set your app's configurations</span></span>
<span data-ttu-id="af3ad-151">Nyissa meg a ```config.js``` fájlt egy tetszőleges szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="af3ad-151">Open ```config.js``` in your favorite text editor.</span></span>

<span data-ttu-id="af3ad-152">Ezután, másolás és beillesztés hello kódrészletben és tulajdonságainak beállítása ```config.endpoint``` és ```config.primaryKey``` tooyour Azure Cosmos DB-végpontjának uri és elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="af3ad-152">Then, copy and paste hello code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` tooyour Azure Cosmos DB endpoint uri and primary key.</span></span> <span data-ttu-id="af3ad-153">Mindkettő konfiguráció megtalálható hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="af3ad-153">Both these configurations can be found in hello [Azure portal](https://portal.azure.com).</span></span>

![A kijelölt node.js-oktatóanyag – képernyőfelvétel a hello Azure-portál, Azure Cosmos DB adatait, az ACTIVE központ hello megjelenítő lévő hello Azure Cosmos DB fiók panelen, és hello URI, elsődleges és másodlagos kulcsot értékek vannak kiemelve a hello hello KEYS gomb Kulcsok panelről – Node-adatbázis][keys]

    // ADD THIS PART tooYOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

<span data-ttu-id="af3ad-155">Másolja és illessze be a hello ```database id```, ```collection id```, és ```JSON documents``` tooyour ```config``` objektum az az alábbi amelyen beállíthatja a ```config.endpoint``` és ```config.authKey``` tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="af3ad-155">Copy and paste hello ```database id```, ```collection id```, and ```JSON documents``` tooyour ```config``` object below where you set your ```config.endpoint``` and ```config.authKey``` properties.</span></span> <span data-ttu-id="af3ad-156">Ha van olyan adat, milyen toostore az adatbázisban, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) hello dokumentum-definíciók hozzáadása helyett.</span><span class="sxs-lookup"><span data-stu-id="af3ad-156">If you already have data you'd like toostore in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding hello document definitions.</span></span>

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


<span data-ttu-id="af3ad-157">hello adatbázis, gyűjtemény és dokumentum-definíciók fog működni a Azure Cosmos DB ```database id```, ```collection id```, és dokumentumadataiként.</span><span class="sxs-lookup"><span data-stu-id="af3ad-157">hello database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.</span></span>

<span data-ttu-id="af3ad-158">Végül exportálja a ```config``` objektumot, hogy hivatkozhasson rá belül hello ```app.js``` fájlt.</span><span class="sxs-lookup"><span data-stu-id="af3ad-158">Finally, export your ```config``` object, so that you can reference it within hello ```app.js``` file.</span></span>

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART tooYOUR CODE
    module.exports = config;

## <span data-ttu-id="af3ad-159"><a id="Connect"></a>4. lépés: Csatlakozás tooan Azure Cosmos DB fiók</span><span class="sxs-lookup"><span data-stu-id="af3ad-159"><a id="Connect"></a> Step 4: Connect tooan Azure Cosmos DB account</span></span>
<span data-ttu-id="af3ad-160">Nyissa meg az üres ```app.js``` fájl hello szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="af3ad-160">Open your empty ```app.js``` file in hello text editor.</span></span> <span data-ttu-id="af3ad-161">Másolja és illessze be az alábbi tooimport hello hello kód ```documentdb``` modul, és az újonnan létrehozott ```config``` modul.</span><span class="sxs-lookup"><span data-stu-id="af3ad-161">Copy and paste hello code below tooimport hello ```documentdb``` module and your newly created ```config``` module.</span></span>

    // ADD THIS PART tooYOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

<span data-ttu-id="af3ad-162">Másolja és illessze be a korábban mentett hello kód toouse hello ```config.endpoint``` és ```config.primaryKey``` toocreate egy új documentclient-ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="af3ad-162">Copy and paste hello code toouse hello previously saved ```config.endpoint``` and ```config.primaryKey``` toocreate a new DocumentClient.</span></span>

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART tooYOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

<span data-ttu-id="af3ad-163">Most, hogy hello kód tooinitialize hello Azure Cosmos DB ügyfél, vessen egy pillantást a Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="af3ad-163">Now that you have hello code tooinitialize hello Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <a name="step-5-create-a-node-database"></a><span data-ttu-id="af3ad-164">5. lépés: Node-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-164">Step 5: Create a Node database</span></span>
<span data-ttu-id="af3ad-165">Másolással illessze be a hello kódot alább tooset hello HTTP-állapot nem található, hello adatbázis URL-címét és hello gyűjtemény URL-címét.</span><span class="sxs-lookup"><span data-stu-id="af3ad-165">Copy and paste hello code below tooset hello HTTP status for Not Found, hello database url, and hello collection url.</span></span> <span data-ttu-id="af3ad-166">Ezen URL-címei, hogyan hello Azure Cosmos DB ügyfél megtalálja a megfelelő hello adatbázis és gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-166">These urls are how hello Azure Cosmos DB client will find hello right database and collection.</span></span>

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART tooYOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

<span data-ttu-id="af3ad-167">A [adatbázis](documentdb-resources.md#databases) hello segítségével hozhatók létre [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) hello funkcióját **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="af3ad-167">A [database](documentdb-resources.md#databases) can be created by using hello [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="af3ad-168">Egy adatbázis a dokumentumtároló gyűjtemények között particionált logikai tárolója hello.</span><span class="sxs-lookup"><span data-stu-id="af3ad-168">A database is hello logical container of document storage partitioned across collections.</span></span>

<span data-ttu-id="af3ad-169">Másolja és illessze be a hello **getDatabase** hello hello app.js fájlban az új adatbázis létrehozása függvény ```id``` hello megadott ```config``` objektum.</span><span class="sxs-lookup"><span data-stu-id="af3ad-169">Copy and paste hello **getDatabase** function for creating your new database in hello app.js file with hello ```id``` specified in hello ```config``` object.</span></span> <span data-ttu-id="af3ad-170">hello függvény ellenőrzi, hogy ugyanaz a hello adatbázis hello ```FamilyRegistry``` azonosítója már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af3ad-170">hello function will check if hello database with hello same ```FamilyRegistry``` id does not already exist.</span></span> <span data-ttu-id="af3ad-171">Ha már létezik, a rendszer új adatbázis létrehozása helyett visszaadja a már létezőt.</span><span class="sxs-lookup"><span data-stu-id="af3ad-171">If it does exist, we'll return that database instead of creating a new one.</span></span>

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

<span data-ttu-id="af3ad-172">Másolja és illessze be az alábbi hello kód, amelyen meg hello **getDatabase** tooadd hello segítő funkció működéséhez **kilépéshez** , hogy kiírja hello kilépési üzenetet, valamint hello hívás túl**getDatabase** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-172">Copy and paste hello code below where you set hello **getDatabase** function tooadd hello helper function **exit** that will print hello exit message and hello call too**getDatabase** function.</span></span>

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

<span data-ttu-id="af3ad-173">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-173">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-174">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-174">Congratulations!</span></span> <span data-ttu-id="af3ad-175">Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="af3ad-175">You have successfully created an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="af3ad-176"><a id="CreateColl"></a>6. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-176"><a id="CreateColl"></a>Step 6: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="af3ad-177">A **CreateDocumentCollectionAsync** létrehoz egy új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="af3ad-177">**CreateDocumentCollectionAsync** will create a new collection, which has pricing implications.</span></span> <span data-ttu-id="af3ad-178">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="af3ad-178">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="af3ad-179">A [gyűjtemény](documentdb-resources.md#collections) hello segítségével hozhatók létre [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) hello funkcióját **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="af3ad-179">A [collection](documentdb-resources.md#collections) can be created by using hello [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="af3ad-180">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="af3ad-180">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="af3ad-181">Másolja és illessze be a hello **getCollection** függvény alá hello **getDatabase** működni hello app.js fájl toocreate gyűjtemény az hello ```id``` hello megadott```config```objektum.</span><span class="sxs-lookup"><span data-stu-id="af3ad-181">Copy and paste hello **getCollection** function underneath hello **getDatabase** function in hello app.js file toocreate your new collection with hello ```id``` specified in hello ```config``` object.</span></span> <span data-ttu-id="af3ad-182">Ebben az esetben toomake ellenőrizzük, hogy egy gyűjtemény hello azonos ```FamilyCollection``` azonosítója már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af3ad-182">Again, we'll check toomake sure a collection with hello same ```FamilyCollection``` id does not already exist.</span></span> <span data-ttu-id="af3ad-183">Ha már létezik, a rendszer új gyűjtemény létrehozása helyett visszaadja a már létezőt.</span><span class="sxs-lookup"><span data-stu-id="af3ad-183">If it does exist, we'll return that collection instead of creating a new one.</span></span>

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

<span data-ttu-id="af3ad-184">Másolja és illessze be a hello hívás alatt hello kód túl**getDatabase** tooexecute hello **getCollection** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-184">Copy and paste hello code below hello call too**getDatabase** tooexecute hello **getCollection** function.</span></span>

    getDatabase()

    // ADD THIS PART tooYOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="af3ad-185">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-185">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-186">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-186">Congratulations!</span></span> <span data-ttu-id="af3ad-187">Sikeresen létrehozott egy Azure Cosmos DB gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="af3ad-187">You have successfully created an Azure Cosmos DB collection.</span></span>

## <span data-ttu-id="af3ad-188"><a id="CreateDoc"></a>7. lépés: Dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="af3ad-188"><a id="CreateDoc"></a>Step 7: Create a document</span></span>
<span data-ttu-id="af3ad-189">A [dokumentum](documentdb-resources.md#documents) hello segítségével hozhatók létre [Documentclient](https://azure.github.io/azure-documentdb-node/DocumentClient.html) hello funkcióját **DocumentClient** osztály.</span><span class="sxs-lookup"><span data-stu-id="af3ad-189">A [document](documentdb-resources.md#documents) can be created by using hello [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of hello **DocumentClient** class.</span></span> <span data-ttu-id="af3ad-190">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="af3ad-190">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="af3ad-191">Most már beszúrhat egy dokumentumot az Azure Cosmos DB-be.</span><span class="sxs-lookup"><span data-stu-id="af3ad-191">You can now insert a document into Azure Cosmos DB.</span></span>

<span data-ttu-id="af3ad-192">Másolja és illessze be a hello **getFamilyDocument** függvény alá hello **getCollection** függvényt az hello hello mentett JSON-adatokat tartalmazó hello dokumentumok ```config``` objektum.</span><span class="sxs-lookup"><span data-stu-id="af3ad-192">Copy and paste hello **getFamilyDocument** function underneath hello **getCollection** function for creating hello documents containing hello JSON data saved in hello ```config``` object.</span></span> <span data-ttu-id="af3ad-193">Ebben az esetben ellenőrizzük toomake meg arról, hogy a dokumentum hello ugyanazzal az azonosítóval már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="af3ad-193">Again, we'll check toomake sure a document with hello same id does not already exist.</span></span>

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

<span data-ttu-id="af3ad-194">Másolja és illessze be a hello hívás alatt hello kód túl**getCollection** tooexecute hello **getFamilyDocument** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-194">Copy and paste hello code below hello call too**getCollection** tooexecute hello **getFamilyDocument** function.</span></span>

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="af3ad-195">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-195">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-196">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-196">Congratulations!</span></span> <span data-ttu-id="af3ad-197">Sikeresen létrehozott egy Azure Cosmos DB dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="af3ad-197">You have successfully created an Azure Cosmos DB document.</span></span>

![NODE.js-oktatóanyag – hello fiók, hello adatbázis, gyűjtemény hello és hello dokumentumok hierarchikus kapcsolatát hello ábrázoló Diagram – Node-adatbázis](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <span data-ttu-id="af3ad-199"><a id="Query"></a>8. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="af3ad-199"><a id="Query"></a>Step 8: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="af3ad-200">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="af3ad-200">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="af3ad-201">hello alábbi mintakód bemutatja egy lekérdezést, amely alapján futtathatók hello dokumentumok a gyűjteményben.</span><span class="sxs-lookup"><span data-stu-id="af3ad-201">hello following sample code shows a query that you can run against hello documents in your collection.</span></span>

<span data-ttu-id="af3ad-202">Másolja és illessze be a hello **queryCollection** függvény alá hello **getFamilyDocument** függvény hello app.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="af3ad-202">Copy and paste hello **queryCollection** function underneath hello **getFamilyDocument** function in hello app.js file.</span></span> <span data-ttu-id="af3ad-203">Azure Cosmos-adatbázis SQL-szerű lekérdezéseket támogatja az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="af3ad-203">Azure Cosmos DB supports SQL-like queries as shown below.</span></span> <span data-ttu-id="af3ad-204">Bonyolult lekérdezések felépítésével további információkért tekintse meg a hello [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo) és hello [lekérdezésekre vonatkozó dokumentációt](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="af3ad-204">For more information on building complex queries, check out hello [Query Playground](https://www.documentdb.com/sql/demo) and hello [query documentation](documentdb-sql-query.md).</span></span>

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


<span data-ttu-id="af3ad-205">hello a következő ábra szemlélteti, hogyan hello Azure Cosmos adatbázis SQL-lekérdezés szintaxisa hívást hello gyűjtemény létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af3ad-205">hello following diagram illustrates how hello Azure Cosmos DB SQL query syntax is called against hello collection you created.</span></span>

![NODE.js-oktatóanyag – és hello lekérdezés jelentését hello hatókör ábrázoló Diagram – Node-adatbázis](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

<span data-ttu-id="af3ad-207">Hello [FROM](documentdb-sql-query.md#FromClause) kulcsszó nem kötelező, hello lekérdezésben, mert Azure Cosmos DB lekérdezések már hatókörön belüli tooa egyetlen gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-207">hello [FROM](documentdb-sql-query.md#FromClause) keyword is optional in hello query because Azure Cosmos DB queries are already scoped tooa single collection.</span></span> <span data-ttu-id="af3ad-208">Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre.</span><span class="sxs-lookup"><span data-stu-id="af3ad-208">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="af3ad-209">Az Azure Cosmos DB következtethető ki, hogy családok, legfelső szintű vagy hello változó neve választja, hivatkozás hello aktuális gyűjtemény alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="af3ad-209">Azure Cosmos DB will infer that Families, root, or hello variable name you chose, reference hello current collection by default.</span></span>

<span data-ttu-id="af3ad-210">Másolja és illessze be a hello hívás alatt hello kód túl**getFamilyDocument** tooexecute hello **queryCollection** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-210">Copy and paste hello code below hello call too**getFamilyDocument** tooexecute hello **queryCollection** function.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART tooYOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="af3ad-211">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-211">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-212">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-212">Congratulations!</span></span> <span data-ttu-id="af3ad-213">Sikeresen lekérdezett Azure Cosmos DB-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="af3ad-213">You have successfully queried Azure Cosmos DB documents.</span></span>

## <span data-ttu-id="af3ad-214"><a id="ReplaceDocument"></a>9. lépés: Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="af3ad-214"><a id="ReplaceDocument"></a>Step 9: Replace a document</span></span>
<span data-ttu-id="af3ad-215">Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="af3ad-215">Azure Cosmos DB supports replacing JSON documents.</span></span>

<span data-ttu-id="af3ad-216">Másolja és illessze be a hello **replaceFamilyDocument** függvény alá hello **queryCollection** függvény hello app.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="af3ad-216">Copy and paste hello **replaceFamilyDocument** function underneath hello **queryCollection** function in hello app.js file.</span></span>

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

<span data-ttu-id="af3ad-217">Másolja és illessze be a hello hívás alatt hello kód túl**queryCollection** tooexecute hello **replaceDocument** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-217">Copy and paste hello code below hello call too**queryCollection** tooexecute hello **replaceDocument** function.</span></span> <span data-ttu-id="af3ad-218">Továbbá adja hozzá a hello kód toocall **queryCollection** újra tooverify hello a dokumentum módosítása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="af3ad-218">Also, add hello code toocall **queryCollection** again tooverify that hello document had successfully changed.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="af3ad-219">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-219">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-220">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-220">Congratulations!</span></span> <span data-ttu-id="af3ad-221">Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="af3ad-221">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="af3ad-222"><a id="DeleteDocument"></a>10. lépés: Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="af3ad-222"><a id="DeleteDocument"></a>Step 10: Delete a document</span></span>
<span data-ttu-id="af3ad-223">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.</span><span class="sxs-lookup"><span data-stu-id="af3ad-223">Azure Cosmos DB supports deleting JSON documents.</span></span>

<span data-ttu-id="af3ad-224">Másolja és illessze be a hello **deleteFamilyDocument** függvény alá hello **replaceFamilyDocument** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-224">Copy and paste hello **deleteFamilyDocument** function underneath hello **replaceFamilyDocument** function.</span></span>

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

<span data-ttu-id="af3ad-225">Másolja és illessze be a hello kódot alább hello hívás toohello második **queryCollection** tooexecute hello **deleteDocument** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-225">Copy and paste hello code below hello call toohello second **queryCollection** tooexecute hello **deleteDocument** function.</span></span>

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART tooYOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="af3ad-226">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-226">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-227">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-227">Congratulations!</span></span> <span data-ttu-id="af3ad-228">Sikeresen törölt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="af3ad-228">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="af3ad-229"><a id="DeleteDatabase"></a>11. lépés: Hello Node-adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="af3ad-229"><a id="DeleteDatabase"></a>Step 11: Delete hello Node database</span></span>
<span data-ttu-id="af3ad-230">A létrehozott adatbázis törlésével hello eltávolítja hello adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.).</span><span class="sxs-lookup"><span data-stu-id="af3ad-230">Deleting hello created database will remove hello database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="af3ad-231">Másolja és illessze be a hello **tisztítás** függvény alá hello **deleteFamilyDocument** tooremove hello adatbázis és az összes hello gyermekerőforrás működik.</span><span class="sxs-lookup"><span data-stu-id="af3ad-231">Copy and paste hello **cleanup** function underneath hello **deleteFamilyDocument** function tooremove hello database and all hello children resources.</span></span>

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

<span data-ttu-id="af3ad-232">Másolja és illessze be a hello hívás alatt hello kód túl**deleteFamilyDocument** tooexecute hello **tisztítás** függvény.</span><span class="sxs-lookup"><span data-stu-id="af3ad-232">Copy and paste hello code below hello call too**deleteFamilyDocument** tooexecute hello **cleanup** function.</span></span>

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART tooYOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <span data-ttu-id="af3ad-233"><a id="Run"></a>12. lépés: A teljes Node.js-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="af3ad-233"><a id="Run"></a>Step 12: Run your Node.js application all together!</span></span>
<span data-ttu-id="af3ad-234">Hello feladatütemezési a függvényeket meghívó teljes sorozatnak így példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="af3ad-234">Altogether, hello sequence for calling your functions should look like this:</span></span>

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

<span data-ttu-id="af3ad-235">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa:```node app.js```</span><span class="sxs-lookup"><span data-stu-id="af3ad-235">In your terminal, locate your ```app.js``` file and run hello command: ```node app.js```</span></span>

<span data-ttu-id="af3ad-236">Az első lépések alkalmazás kimenetének hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="af3ad-236">You should see hello output of your get started app.</span></span> <span data-ttu-id="af3ad-237">hello kimeneti meg kell felelnie az alábbi hello példaszöveggel.</span><span class="sxs-lookup"><span data-stu-id="af3ad-237">hello output should match hello example text below.</span></span>

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

<span data-ttu-id="af3ad-238">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="af3ad-238">Congratulations!</span></span> <span data-ttu-id="af3ad-239">Létrehozott, hello Node.js-oktatóanyag elvégzése után, és rendelkezik az első Azure Cosmos DB konzolalkalmazást!</span><span class="sxs-lookup"><span data-stu-id="af3ad-239">You've created you've completed hello Node.js tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="af3ad-240"><a id="GetSolution"></a>Hello teljes Node.js oktatóanyag megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="af3ad-240"><a id="GetSolution"></a>Get hello complete Node.js tutorial solution</span></span>
<span data-ttu-id="af3ad-241">Ha még nem idő toocomplete lépéseit az oktatóanyag hello, vagy csak szeretné, hogy toodownload hello kódot, beszerezheti a [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="af3ad-241">If you didn't have time toocomplete hello steps in this tutorial, or just want toodownload hello code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span>

<span data-ttu-id="af3ad-242">toorun hello GetStarted-megoldás, amely tartalmazza az összes hello minta ebben a cikkben, szüksége lesz a következő hello:</span><span class="sxs-lookup"><span data-stu-id="af3ad-242">toorun hello GetStarted solution that contains all hello samples in this article, you will need hello following:</span></span>

* <span data-ttu-id="af3ad-243">[Azure Cosmos DB-fiók][create-account].</span><span class="sxs-lookup"><span data-stu-id="af3ad-243">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="af3ad-244">Hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) megoldás elérhető a Githubon.</span><span class="sxs-lookup"><span data-stu-id="af3ad-244">hello [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="af3ad-245">Telepítse a hello **documentdb** modult az npm.</span><span class="sxs-lookup"><span data-stu-id="af3ad-245">Install hello **documentdb** module via npm.</span></span> <span data-ttu-id="af3ad-246">A következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="af3ad-246">Use hello following command:</span></span>

* ```npm install documentdb --save```

<span data-ttu-id="af3ad-247">Ezután hello ```config.js``` fájl, a frissítés hello config.endpoint és config.authKey értékeket a [3. lépés: állítsa be az alkalmazás konfigurációnak](#Config).</span><span class="sxs-lookup"><span data-stu-id="af3ad-247">Next, in hello ```config.js``` file, update hello config.endpoint and config.authKey values as described in [Step 3: Set your app's configurations](#Config).</span></span> 

<span data-ttu-id="af3ad-248">A terminálban keresse meg a ```app.js``` fájlt, és hello parancsot futtassa: ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="af3ad-248">Then in your terminal, locate your ```app.js``` file and run hello command: ```node app.js```.</span></span>

<span data-ttu-id="af3ad-249">Ennyi az egész! Építse ki, és máris jó úton jár!</span><span class="sxs-lookup"><span data-stu-id="af3ad-249">That's it, build it and you're on your way!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="af3ad-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af3ad-250">Next steps</span></span>
* <span data-ttu-id="af3ad-251">Összetettebb Node.js-mintát szeretne használni?</span><span class="sxs-lookup"><span data-stu-id="af3ad-251">Want a more complex Node.js sample?</span></span> <span data-ttu-id="af3ad-252">Lásd: [Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-nodejs-application.md).</span><span class="sxs-lookup"><span data-stu-id="af3ad-252">See [Build a Node.js web application using Azure Cosmos DB](documentdb-nodejs-application.md).</span></span>
* <span data-ttu-id="af3ad-253">Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="af3ad-253">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="af3ad-254">A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="af3ad-254">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="af3ad-255">További tudnivalók a programozási modellt hello hello hello Develop szakasza [Azure Cosmos DB dokumentációs oldal](https://azure.microsoft.com/documentation/services/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="af3ad-255">Learn more about hello programming model in hello Develop section of hello [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
