---
title: "Node.js-oktatóanyag az Azure Cosmos DB-hez készült DocumentDB API-hoz | Microsoft Docs"
description: "Ez a Node.js-oktatóanyag létrehoz egy Cosmos DB-t a DocumentDB API-val."
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
ms.openlocfilehash: 6510e0270bb2efa252a2b2ad40014c5d26b74a81
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="nodejs-tutorial-use-the-documentdb-api-in-azure-cosmos-db-to-create-a-nodejs-console-application"></a><span data-ttu-id="cc2fd-104">NODE.js-oktatóanyag: Azure Cosmos DB a DocumentDB API használatával egy Node.js-Konzolalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-104">Node.js tutorial: Use the DocumentDB API in Azure Cosmos DB to create a Node.js console application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="cc2fd-105">.NET</span><span class="sxs-lookup"><span data-stu-id="cc2fd-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="cc2fd-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc2fd-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="cc2fd-107">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="cc2fd-107">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="cc2fd-108">Node.js</span><span class="sxs-lookup"><span data-stu-id="cc2fd-108">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="cc2fd-109">Java</span><span class="sxs-lookup"><span data-stu-id="cc2fd-109">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="cc2fd-110">C++</span><span class="sxs-lookup"><span data-stu-id="cc2fd-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
> 

<span data-ttu-id="cc2fd-111">Üdvözöljük az Azure Cosmos DB Node.js SDK-hoz készült Node.js-oktatóanyagban!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-111">Welcome to the Node.js tutorial for the Azure Cosmos DB Node.js SDK!</span></span> <span data-ttu-id="cc2fd-112">Az oktatóanyag lépéseinek követésével egy olyan konzolalkalmazást készít, amely Azure Cosmos DB-erőforrásokat hoz létre és kérdez le.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-112">After following this tutorial, you'll have a console application that creates and queries Azure Cosmos DB resources.</span></span>

<span data-ttu-id="cc2fd-113">Az oktatóanyag a következőket ismerteti:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-113">We'll cover:</span></span>

* <span data-ttu-id="cc2fd-114">Azure Cosmos DB-fiók létrehozása és csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-114">Creating and connecting to an Azure Cosmos DB account</span></span>
* <span data-ttu-id="cc2fd-115">Az alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-115">Setting up your application</span></span>
* <span data-ttu-id="cc2fd-116">Node-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-116">Creating a node database</span></span>
* <span data-ttu-id="cc2fd-117">Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-117">Creating a collection</span></span>
* <span data-ttu-id="cc2fd-118">JSON-dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-118">Creating JSON documents</span></span>
* <span data-ttu-id="cc2fd-119">A gyűjtemény lekérdezése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-119">Querying the collection</span></span>
* <span data-ttu-id="cc2fd-120">Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="cc2fd-120">Replacing a document</span></span>
* <span data-ttu-id="cc2fd-121">Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-121">Deleting a document</span></span>
* <span data-ttu-id="cc2fd-122">A Node-adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-122">Deleting the node database</span></span>

<span data-ttu-id="cc2fd-123">Nincs elég ideje?</span><span class="sxs-lookup"><span data-stu-id="cc2fd-123">Don't have time?</span></span> <span data-ttu-id="cc2fd-124">Ne aggódjon!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-124">Don't worry!</span></span> <span data-ttu-id="cc2fd-125">A teljes megoldás elérhető a [GitHubon](https://github.com/Azure-Samples/documentdb-node-getting-started).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-125">The complete solution is available on [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span> <span data-ttu-id="cc2fd-126">Gyors útmutatásért tekintse meg [A teljes megoldás beszerzése](#GetSolution) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-126">See [Get the complete solution](#GetSolution) for quick instructions.</span></span>

<span data-ttu-id="cc2fd-127">A Node.js-oktatóanyag befejezése után a lap tetején vagy alján található szavazógomb használatával küldhet visszajelzést.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-127">After you've completed the Node.js tutorial, please use the voting buttons at the top and bottom of this page to give us feedback.</span></span> <span data-ttu-id="cc2fd-128">Ha szeretne közvetlenül kapcsolatba lépni velünk, a hozzászólásaiban tüntesse fel az e-mail-címét.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-128">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="cc2fd-129">Most pedig lássunk neki!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-129">Now let's get started!</span></span>

## <a name="prerequisites-for-the-nodejs-tutorial"></a><span data-ttu-id="cc2fd-130">A Node.js-oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="cc2fd-130">Prerequisites for the Node.js tutorial</span></span>
<span data-ttu-id="cc2fd-131">Győződjön meg róla, hogy rendelkezik az alábbiakkal:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-131">Please make sure you have the following:</span></span>

* <span data-ttu-id="cc2fd-132">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-132">An active Azure account.</span></span> <span data-ttu-id="cc2fd-133">Ha még nincs fiókja, regisztrálhat az [Azure ingyenes próbaverziójára](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-133">If you don't have one, you can sign up for a [Free Azure Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
    * <span data-ttu-id="cc2fd-134">Másik lehetőségként használhatja az [Azure Cosmos DB Emulatort](local-emulator.md) az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-134">Alternatively, you can use the [Azure Cosmos DB Emulator](local-emulator.md) for this tutorial.</span></span>
* <span data-ttu-id="cc2fd-135">[Node.js](https://nodejs.org/)-verzió: 0.10.29-s vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-135">[Node.js](https://nodejs.org/) version v0.10.29 or higher.</span></span>

## <a name="step-1-create-an-azure-cosmos-db-account"></a><span data-ttu-id="cc2fd-136">1. lépés: Azure Cosmos DB-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-136">Step 1: Create an Azure Cosmos DB account</span></span>
<span data-ttu-id="cc2fd-137">Hozzunk létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-137">Let's create an Azure Cosmos DB account.</span></span> <span data-ttu-id="cc2fd-138">Ha már rendelkezik egy használni kívánt fiókot, ugorjon előre [a Node.js-alkalmazás beállítása](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-138">If you already have an account you want to use, you can skip ahead to [Set up your Node.js application](#SetupNode).</span></span> <span data-ttu-id="cc2fd-139">Ha az Azure Cosmos DB Emulator használ, adja kövesse a [Azure Cosmos DB emulátor](local-emulator.md) kell beállítania az emulátor, és ugorjon előre [a Node.js-alkalmazás beállítása](#SetupNode).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-139">If you are using the Azure Cosmos DB Emulator, please follow the steps at [Azure Cosmos DB Emulator](local-emulator.md) to setup the emulator and skip ahead to [Set up your Node.js application](#SetupNode).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <span data-ttu-id="cc2fd-140"><a id="SetupNode"></a>2. lépés: A Node.js-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-140"><a id="SetupNode"></a>Step 2: Set up your Node.js application</span></span>
1. <span data-ttu-id="cc2fd-141">Nyissa meg kedvenc terminálját.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-141">Open your favorite terminal.</span></span>
2. <span data-ttu-id="cc2fd-142">Keresse meg azt a mappát vagy könyvtárat, ahova a Node.js-alkalmazást menteni szeretné.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-142">Locate the folder or directory where you'd like to save your Node.js application.</span></span>
3. <span data-ttu-id="cc2fd-143">Hozzon létre két üres JavaScript-fájlt az alábbi parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-143">Create two empty JavaScript files with the following commands:</span></span>
   * <span data-ttu-id="cc2fd-144">Windows:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-144">Windows:</span></span>
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```
   * <span data-ttu-id="cc2fd-145">Linux/OS X:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-145">Linux/OS X:</span></span>
     * ```touch app.js```
     * ```touch config.js```
4. <span data-ttu-id="cc2fd-146">Telepítse a DocumentDB modult az npm segítségével</span><span class="sxs-lookup"><span data-stu-id="cc2fd-146">Install the documentdb module via npm.</span></span> <span data-ttu-id="cc2fd-147">Használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-147">Use the following command:</span></span>
   * ```npm install documentdb --save```

<span data-ttu-id="cc2fd-148">Remek!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-148">Great!</span></span> <span data-ttu-id="cc2fd-149">A beállítás befejeztével nekiláthat a kód írásának.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-149">Now that you've finished setting up, let's start writing some code.</span></span>

## <span data-ttu-id="cc2fd-150"><a id="Config"></a>3. lépés: Az alkalmazás konfigurációnak megadása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-150"><a id="Config"></a>Step 3: Set your app's configurations</span></span>
<span data-ttu-id="cc2fd-151">Nyissa meg a ```config.js``` fájlt egy tetszőleges szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-151">Open ```config.js``` in your favorite text editor.</span></span>

<span data-ttu-id="cc2fd-152">Ezután, másolás és beillesztés az alábbi kódrészlet, illetve tulajdonságainak beállítása ```config.endpoint``` és ```config.primaryKey``` az Azure Cosmos DB végpont uri és elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-152">Then, copy and paste the code snippet below and set properties ```config.endpoint``` and ```config.primaryKey``` to your Azure Cosmos DB endpoint uri and primary key.</span></span> <span data-ttu-id="cc2fd-153">Mindkettő konfiguráció megtalálható a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-153">Both these configurations can be found in the [Azure portal](https://portal.azure.com).</span></span>

![NODE.js-oktatóanyag – képernyőfelvétel az Azure-portálon az Azure Cosmos DB fiókhoz, amely az ACTIVE központ megjelenítő jelölve, a Azure Cosmos DB-fiók panelen, és a kulcsok panelen - lévő URI, elsődleges és másodlagos kulcsot értékek KEYS gomb NODE-adatbázis][keys]

    // ADD THIS PART TO YOUR CODE
    var config = {}

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

<span data-ttu-id="cc2fd-155">Másolja és illessze be a ```database id```, ```collection id``` és ```JSON documents``` értékeket az alábbi ```config```-objektumba oda, ahol megadta a ```config.endpoint``` és ```config.authKey``` tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-155">Copy and paste the ```database id```, ```collection id```, and ```JSON documents``` to your ```config``` object below where you set your ```config.endpoint``` and ```config.authKey``` properties.</span></span> <span data-ttu-id="cc2fd-156">Ha van olyan adat, amelyet szeretne az adatbázisban tárolni, használhatja az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md) a dokumentumdefiníciók hozzáadása helyett.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-156">If you already have data you'd like to store in your database, you can use Azure Cosmos DB's [Data Migration tool](import-data.md) rather than adding the document definitions.</span></span>

    config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
    config.primaryKey = "~your primary key here~";

    // ADD THIS PART TO YOUR CODE
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


<span data-ttu-id="cc2fd-157">Az adatbázis, gyűjtemény és dokumentum-definíciók fog működni a Azure Cosmos DB ```database id```, ```collection id```, és dokumentumadataiként.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-157">The database, collection, and document definitions will act as your Azure Cosmos DB ```database id```, ```collection id```, and documents' data.</span></span>

<span data-ttu-id="cc2fd-158">Végül exportálja a ```config```-objektumot, hogy hivatkozhasson rá az ```app.js``` fájlban.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-158">Finally, export your ```config``` object, so that you can reference it within the ```app.js``` file.</span></span>

            },
            "isRegistered": false
        }
    };

    // ADD THIS PART TO YOUR CODE
    module.exports = config;

## <span data-ttu-id="cc2fd-159"><a id="Connect"></a> 4. lépés: Csatlakozás Azure Cosmos DB-fiókhoz</span><span class="sxs-lookup"><span data-stu-id="cc2fd-159"><a id="Connect"></a> Step 4: Connect to an Azure Cosmos DB account</span></span>
<span data-ttu-id="cc2fd-160">Nyissa meg az üres ```app.js``` fájlt a szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-160">Open your empty ```app.js``` file in the text editor.</span></span> <span data-ttu-id="cc2fd-161">Másolja és illessze be az alábbi kódot a ```documentdb```, valamint az újonnan létrehozott ```config``` modul importálásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-161">Copy and paste the code below to import the ```documentdb``` module and your newly created ```config``` module.</span></span>

    // ADD THIS PART TO YOUR CODE
    "use strict";

    var documentClient = require("documentdb").DocumentClient;
    var config = require("./config");
    var url = require('url');

<span data-ttu-id="cc2fd-162">Másolja és illessze be a kódot, ha az előzőleg mentett ```config.endpoint``` és ```config.primaryKey``` használatával szeretne létrehozni egy új DocumentClient-ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-162">Copy and paste the code to use the previously saved ```config.endpoint``` and ```config.primaryKey``` to create a new DocumentClient.</span></span>

    var config = require("./config");
    var url = require('url');

    // ADD THIS PART TO YOUR CODE
    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

<span data-ttu-id="cc2fd-163">Most, hogy az Azure Cosmos DB ügyfél kóddal, vessen egy pillantást a Azure Cosmos DB erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-163">Now that you have the code to initialize the Azure Cosmos DB client, let's take a look at working with Azure Cosmos DB resources.</span></span>

## <a name="step-5-create-a-node-database"></a><span data-ttu-id="cc2fd-164">5. lépés: Node-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-164">Step 5: Create a Node database</span></span>
<span data-ttu-id="cc2fd-165">Másolja és illessze be az alábbi kódot, hogy megadja a Not Found (Nem található) HTTP-állapotot, az adatbázis URL-címét, valamint a gyűjtemény URL-címét.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-165">Copy and paste the code below to set the HTTP status for Not Found, the database url, and the collection url.</span></span> <span data-ttu-id="cc2fd-166">Ezen URL-címei, hogyan az Azure Cosmos DB ügyfél megtalálja a megfelelő adatbázis és gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-166">These urls are how the Azure Cosmos DB client will find the right database and collection.</span></span>

    var client = new documentClient(config.endpoint, { "masterKey": config.primaryKey });

    // ADD THIS PART TO YOUR CODE
    var HttpStatusCodes = { NOTFOUND: 404 };
    var databaseUrl = `dbs/${config.database.id}`;
    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

<span data-ttu-id="cc2fd-167">Az [adatbázis](documentdb-resources.md#databases) a **DocumentClient** osztály [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) függvényének használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-167">A [database](documentdb-resources.md#databases) can be created by using the [createDatabase](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="cc2fd-168">Az adatbázis a dokumentumtároló gyűjtemények között particionált logikai tárolója.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-168">A database is the logical container of document storage partitioned across collections.</span></span>

<span data-ttu-id="cc2fd-169">Másolja és illessze be a **getDatabase** függvényt az adatbázis az app.js fájlban az ```id``` objektum ```config``` azonosítójával történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-169">Copy and paste the **getDatabase** function for creating your new database in the app.js file with the ```id``` specified in the ```config``` object.</span></span> <span data-ttu-id="cc2fd-170">A függvény ellenőrzi, hogy létezik-e már adatbázis ugyanazzal a ```FamilyRegistry```-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-170">The function will check if the database with the same ```FamilyRegistry``` id does not already exist.</span></span> <span data-ttu-id="cc2fd-171">Ha már létezik, a rendszer új adatbázis létrehozása helyett visszaadja a már létezőt.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-171">If it does exist, we'll return that database instead of creating a new one.</span></span>

    var collectionUrl = `${databaseUrl}/colls/${config.collection.id}`;

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="cc2fd-172">Másolja és illessze be az alábbi kódot oda, ahol megadta a **getDatabase** függvényt. Ezzel hozzáadhatja az **exit** (kilépés) segédfüggvényt, amely megjeleníti a kilépési üzenetet, valamint a **getDatabase** függvény hívását.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-172">Copy and paste the code below where you set the **getDatabase** function to add the helper function **exit** that will print the exit message and the call to **getDatabase** function.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
    function exit(message) {
        console.log(message);
        console.log('Press any key to exit');
        process.stdin.setRawMode(true);
        process.stdin.resume();
        process.stdin.on('data', process.exit.bind(process, 0));
    }

    getDatabase()
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="cc2fd-173">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-173">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-174">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-174">Congratulations!</span></span> <span data-ttu-id="cc2fd-175">Sikeresen létrehozott egy Azure Cosmos DB-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-175">You have successfully created an Azure Cosmos DB database.</span></span>

## <span data-ttu-id="cc2fd-176"><a id="CreateColl"></a>6. lépés: Gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-176"><a id="CreateColl"></a>Step 6: Create a collection</span></span>
> [!WARNING]
> <span data-ttu-id="cc2fd-177">A **CreateDocumentCollectionAsync** létrehoz egy új gyűjteményt, amely költségeket von maga után.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-177">**CreateDocumentCollectionAsync** will create a new collection, which has pricing implications.</span></span> <span data-ttu-id="cc2fd-178">További részletekért látogasson el az [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-178">For more details, please visit our [pricing page](https://azure.microsoft.com/pricing/details/cosmos-db/).</span></span>
> 
> 

<span data-ttu-id="cc2fd-179">[Gyűjteményt](documentdb-resources.md#collections) a **DocumentClient** osztály [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) függvényével hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-179">A [collection](documentdb-resources.md#collections) can be created by using the [createCollection](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="cc2fd-180">A gyűjtemény egy JSON-dokumentumokat és a kapcsolódó JavaScript-alkalmazáslogikát tartalmazó tároló.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-180">A collection is a container of JSON documents and associated JavaScript application logic.</span></span>

<span data-ttu-id="cc2fd-181">Másolja és illessze be a **getCollection** függvényt a **getDatabase** függvény alá a app.js fájlban az új gyűjtemény az ```config``` objektum ```id``` azonosítójával történő létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-181">Copy and paste the **getCollection** function underneath the **getDatabase** function in the app.js file to create your new collection with the ```id``` specified in the ```config``` object.</span></span> <span data-ttu-id="cc2fd-182">A rendszer ismét ellenőrzi, hogy létezik-e már gyűjtemény ugyanazzal a ```FamilyCollection```-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-182">Again, we'll check to make sure a collection with the same ```FamilyCollection``` id does not already exist.</span></span> <span data-ttu-id="cc2fd-183">Ha már létezik, a rendszer új gyűjtemény létrehozása helyett visszaadja a már létezőt.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-183">If it does exist, we'll return that collection instead of creating a new one.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="cc2fd-184">Másolja és illessze be az alábbi kódot a **getDatabase** függvény meghívásához, valamint a **getCollection** függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-184">Copy and paste the code below the call to **getDatabase** to execute the **getCollection** function.</span></span>

    getDatabase()

    // ADD THIS PART TO YOUR CODE
    .then(() => getCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="cc2fd-185">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-185">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-186">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-186">Congratulations!</span></span> <span data-ttu-id="cc2fd-187">Sikeresen létrehozott egy Azure Cosmos DB gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-187">You have successfully created an Azure Cosmos DB collection.</span></span>

## <span data-ttu-id="cc2fd-188"><a id="CreateDoc"></a>7. lépés: Dokumentum létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-188"><a id="CreateDoc"></a>Step 7: Create a document</span></span>
<span data-ttu-id="cc2fd-189">[Dokumentumot](documentdb-resources.md#documents) a **DocumentClient** osztály [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) függvényével hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-189">A [document](documentdb-resources.md#documents) can be created by using the [createDocument](https://azure.github.io/azure-documentdb-node/DocumentClient.html) function of the **DocumentClient** class.</span></span> <span data-ttu-id="cc2fd-190">A dokumentumok a felhasználó által megadott (tetszőleges) JSON-tartalmak.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-190">Documents are user defined (arbitrary) JSON content.</span></span> <span data-ttu-id="cc2fd-191">Most már beszúrhat egy dokumentumot az Azure Cosmos DB-be.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-191">You can now insert a document into Azure Cosmos DB.</span></span>

<span data-ttu-id="cc2fd-192">Másolja és illessze be a **getFamilyDocument** függvényt a **getCollection** függvény alá a ```config``` objektumban mentett JSON-adatokat tartalmazó dokumentumok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-192">Copy and paste the **getFamilyDocument** function underneath the **getCollection** function for creating the documents containing the JSON data saved in the ```config``` object.</span></span> <span data-ttu-id="cc2fd-193">A rendszer ismét ellenőrzi, hogy létezik-e már dokumentum ugyanazzal az azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-193">Again, we'll check to make sure a document with the same id does not already exist.</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="cc2fd-194">Másolja és illessze be az alábbi kódot a **getCollection** függvény meghívásához, valamint a **getFamilyDocument** függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-194">Copy and paste the code below the call to **getCollection** to execute the **getFamilyDocument** function.</span></span>

    getDatabase()
    .then(() => getCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="cc2fd-195">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-195">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-196">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-196">Congratulations!</span></span> <span data-ttu-id="cc2fd-197">Sikeresen létrehozott egy Azure Cosmos DB dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-197">You have successfully created an Azure Cosmos DB document.</span></span>

![Node.js-oktatóanyag – A fiók, az adatbázis, a gyűjtemény és a dokumentumok hierarchikus kapcsolatát ábrázoló diagram – Node-adatbázis](./media/documentdb-nodejs-get-started/node-js-tutorial-cosmos-db-account.png)

## <span data-ttu-id="cc2fd-199"><a id="Query"></a>8. lépés: Az Azure Cosmos DB-erőforrások lekérdezése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-199"><a id="Query"></a>Step 8: Query Azure Cosmos DB resources</span></span>
<span data-ttu-id="cc2fd-200">Az Azure Cosmos DB támogatja az egyes gyűjteményekben tárolt JSON-dokumentumokon végzett [részletes lekérdezéseket](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-200">Azure Cosmos DB supports [rich queries](documentdb-sql-query.md) against JSON documents stored in each collection.</span></span> <span data-ttu-id="cc2fd-201">Az alábbi mintakód egy olyan lekérdezést mutat be, amelyet a gyűjteményben található dokumentumokra vonatkozóan futtathat le.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-201">The following sample code shows a query that you can run against the documents in your collection.</span></span>

<span data-ttu-id="cc2fd-202">Másolja és illessze be a **queryCollection** függvényt a **getFamilyDocument** függvény alá az app.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-202">Copy and paste the **queryCollection** function underneath the **getFamilyDocument** function in the app.js file.</span></span> <span data-ttu-id="cc2fd-203">Azure Cosmos-adatbázis SQL-szerű lekérdezéseket támogatja az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-203">Azure Cosmos DB supports SQL-like queries as shown below.</span></span> <span data-ttu-id="cc2fd-204">A bonyolult lekérdezések felépítésével kapcsolatos további információkért tekintse meg a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) szakaszt, valamint a [lekérdezésekre vonatkozó dokumentációt](documentdb-sql-query.md).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-204">For more information on building complex queries, check out the [Query Playground](https://www.documentdb.com/sql/demo) and the [query documentation](documentdb-sql-query.md).</span></span>

                } else {
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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


<span data-ttu-id="cc2fd-205">A következő ábra bemutatja, hogyan az Azure Cosmos adatbázis SQL-lekérdezés szintaxisa hívást a gyűjtemény meg létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-205">The following diagram illustrates how the Azure Cosmos DB SQL query syntax is called against the collection you created.</span></span>

![Node.js-oktatóanyag – A lekérdezés hatókörét és jelentését ábrázoló diagram – Node-adatbázis](./media/documentdb-nodejs-get-started/node-js-tutorial-collection-documents.png)

<span data-ttu-id="cc2fd-207">A [FROM](documentdb-sql-query.md#FromClause) kulcsszó kihagyható a lekérdezésből mert Azure Cosmos adatbázis-lekérdezések hatóköre eleve egyetlen gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-207">The [FROM](documentdb-sql-query.md#FromClause) keyword is optional in the query because Azure Cosmos DB queries are already scoped to a single collection.</span></span> <span data-ttu-id="cc2fd-208">Ezért a „FROM Families f” lecserélhető a „FROM root r” vagy bármilyen tetszőleges változónévre.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-208">Therefore, "FROM Families f" can be swapped with "FROM root r", or any other variable name you choose.</span></span> <span data-ttu-id="cc2fd-209">Azure Cosmos DB fogja következtethető ki, hogy a Families, legfelső szintű vagy a változó nevét a kiválasztott, alapértelmezés szerint az aktuális gyűjteményre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-209">Azure Cosmos DB will infer that Families, root, or the variable name you chose, reference the current collection by default.</span></span>

<span data-ttu-id="cc2fd-210">Másolja és illessze be az alábbi kódot a **getFamilyDocument** függvény meghívásához, valamint a **queryCollection** függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-210">Copy and paste the code below the call to **getFamilyDocument** to execute the **queryCollection** function.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))

    // ADD THIS PART TO YOUR CODE
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="cc2fd-211">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-211">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-212">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-212">Congratulations!</span></span> <span data-ttu-id="cc2fd-213">Sikeresen lekérdezett Azure Cosmos DB-dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-213">You have successfully queried Azure Cosmos DB documents.</span></span>

## <span data-ttu-id="cc2fd-214"><a id="ReplaceDocument"></a>9. lépés: Dokumentum cseréje</span><span class="sxs-lookup"><span data-stu-id="cc2fd-214"><a id="ReplaceDocument"></a>Step 9: Replace a document</span></span>
<span data-ttu-id="cc2fd-215">Az Azure Cosmos DB támogatja a JSON-dokumentumok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-215">Azure Cosmos DB supports replacing JSON documents.</span></span>

<span data-ttu-id="cc2fd-216">Másolja és illessze be a **replaceFamilyDocument** függvényt a **queryCollection** függvény alá az app.js fájlban.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-216">Copy and paste the **replaceFamilyDocument** function underneath the **queryCollection** function in the app.js file.</span></span>

                    }
                    console.log();
                    resolve(result);
                }
            });
        });
    }

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="cc2fd-217">Másolja és illessze be az alábbi kódot a **queryCollection** függvény meghívásához, valamint a **replaceDocument** függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-217">Copy and paste the code below the call to **queryCollection** to execute the **replaceDocument** function.</span></span> <span data-ttu-id="cc2fd-218">Továbbá adja hozzá a **queryCollection** függvényt meghívó kódot annak megerősítése érdekében, hogy a dokumentum módosítása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-218">Also, add the code to call **queryCollection** again to verify that the document had successfully changed.</span></span>

    .then(() => getFamilyDocument(config.documents.Andersen))
    .then(() => getFamilyDocument(config.documents.Wakefield))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="cc2fd-219">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-219">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-220">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-220">Congratulations!</span></span> <span data-ttu-id="cc2fd-221">Sikeresen lecserélt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-221">You have successfully replaced an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="cc2fd-222"><a id="DeleteDocument"></a>10. lépés: Dokumentum törlése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-222"><a id="DeleteDocument"></a>Step 10: Delete a document</span></span>
<span data-ttu-id="cc2fd-223">Az Azure Cosmos DB támogatja a JSON-dokumentumok törlését.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-223">Azure Cosmos DB supports deleting JSON documents.</span></span>

<span data-ttu-id="cc2fd-224">Másolja és illessze be a **deleteFamilyDocument** függvényt a **replaceFamilyDocument** függvény alá.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-224">Copy and paste the **deleteFamilyDocument** function underneath the **replaceFamilyDocument** function.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
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

<span data-ttu-id="cc2fd-225">Másolja és illessze be az alábbi kódot a második **queryCollection** függvény meghívásához, valamint a **deleteDocument** függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-225">Copy and paste the code below the call to the second **queryCollection** to execute the **deleteDocument** function.</span></span>

    .then(() => queryCollection())
    .then(() => replaceFamilyDocument(config.documents.Andersen))
    .then(() => queryCollection())

    // ADD THIS PART TO YOUR CODE
    .then(() => deleteFamilyDocument(config.documents.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

<span data-ttu-id="cc2fd-226">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-226">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-227">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-227">Congratulations!</span></span> <span data-ttu-id="cc2fd-228">Sikeresen törölt egy Azure Cosmos DB-dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-228">You have successfully deleted an Azure Cosmos DB document.</span></span>

## <span data-ttu-id="cc2fd-229"><a id="DeleteDatabase"></a>11. lépés: A Node-adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-229"><a id="DeleteDatabase"></a>Step 11: Delete the Node database</span></span>
<span data-ttu-id="cc2fd-230">A létrehozott adatbázis törlésével az adatbázis és az összes gyermekerőforrás (gyűjtemények, dokumentumok stb.) is törlődik.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-230">Deleting the created database will remove the database and all children resources (collections, documents, etc.).</span></span>

<span data-ttu-id="cc2fd-231">Másolja és illessze be a **cleanup** függvényt a **deleteFamilyDocument** függvény alá az adatbázis, valamint minden gyermekerőforrásának törléséhez.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-231">Copy and paste the **cleanup** function underneath the **deleteFamilyDocument** function to remove the database and all the children resources.</span></span>

                else {
                    resolve(result);
                }
            });
        });
    };

    // ADD THIS PART TO YOUR CODE
    function cleanup() {
        console.log(`Cleaning up by deleting database ${config.database.id}`);

        return new Promise((resolve, reject) => {
            client.deleteDatabase(databaseUrl, (err) => {
                if (err) reject(err)
                else resolve(null);
            });
        });
    }

<span data-ttu-id="cc2fd-232">Másolja és illessze be az alábbi kódot a **deleteFamilyDocument** függvény meghívásához, valamint a **cleanup** függvény végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-232">Copy and paste the code below the call to **deleteFamilyDocument** to execute the **cleanup** function.</span></span>

    .then(() => deleteFamilyDocument(config.documents.Andersen))

    // ADD THIS PART TO YOUR CODE
    .then(() => cleanup())
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });

## <span data-ttu-id="cc2fd-233"><a id="Run"></a>12. lépés: A teljes Node.js-alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="cc2fd-233"><a id="Run"></a>Step 12: Run your Node.js application all together!</span></span>
<span data-ttu-id="cc2fd-234">A függvényeket meghívó teljes sorozatnak így kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-234">Altogether, the sequence for calling your functions should look like this:</span></span>

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

<span data-ttu-id="cc2fd-235">A terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```</span><span class="sxs-lookup"><span data-stu-id="cc2fd-235">In your terminal, locate your ```app.js``` file and run the command: ```node app.js```</span></span>

<span data-ttu-id="cc2fd-236">Meg kell jelennie az első lépések alkalmazás kimenetének.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-236">You should see the output of your get started app.</span></span> <span data-ttu-id="cc2fd-237">A kimenetnek meg kell egyeznie az alábbi példaszöveggel.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-237">The output should match the example text below.</span></span>

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
    Press any key to exit

<span data-ttu-id="cc2fd-238">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-238">Congratulations!</span></span> <span data-ttu-id="cc2fd-239">Ezzel befejezte a Node.js-oktatóanyagot, és létrehozta első saját Azure Cosmos DB-konzolalkalmazását.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-239">You've created you've completed the Node.js tutorial and have your first Azure Cosmos DB console application!</span></span>

## <span data-ttu-id="cc2fd-240"><a id="GetSolution"></a>A Node. js-oktatóanyagban szereplő teljes megoldás beszerzése</span><span class="sxs-lookup"><span data-stu-id="cc2fd-240"><a id="GetSolution"></a>Get the complete Node.js tutorial solution</span></span>
<span data-ttu-id="cc2fd-241">Ha nincs ideje az oktatóanyag lépéseinek végrehajtására, vagy csak szeretné letölteni a kódot, a [GitHubon](https://github.com/Azure-Samples/documentdb-node-getting-started) beszerezheti azt.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-241">If you didn't have time to complete the steps in this tutorial, or just want to download the code, you can get it from [GitHub](https://github.com/Azure-Samples/documentdb-node-getting-started).</span></span>

<span data-ttu-id="cc2fd-242">A cikkben szereplő összes mintát tartalmazó GetStarted-megoldás futtatásához az alábbiakra lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-242">To run the GetStarted solution that contains all the samples in this article, you will need the following:</span></span>

* <span data-ttu-id="cc2fd-243">[Azure Cosmos DB-fiók][create-account].</span><span class="sxs-lookup"><span data-stu-id="cc2fd-243">[Azure Cosmos DB account][create-account].</span></span>
* <span data-ttu-id="cc2fd-244">A GitHubon elérhető [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) megoldás.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-244">The [GetStarted](https://github.com/Azure-Samples/documentdb-node-getting-started) solution available on GitHub.</span></span>

<span data-ttu-id="cc2fd-245">Telepítse a **DocumentDB** modult az npm segítségével.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-245">Install the **documentdb** module via npm.</span></span> <span data-ttu-id="cc2fd-246">Használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="cc2fd-246">Use the following command:</span></span>

* ```npm install documentdb --save```

<span data-ttu-id="cc2fd-247">Ezután, a ```config.js``` fájlban frissítse a config.endpoint és config.authKey értékeket a [3. lépés: Az alkalmazás konfigurációnak megadásában](#Config) leírtak alapján.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-247">Next, in the ```config.js``` file, update the config.endpoint and config.authKey values as described in [Step 3: Set your app's configurations](#Config).</span></span> 

<span data-ttu-id="cc2fd-248">Majd a terminálban keresse meg az ```app.js``` fájlt, és futtassa az alábbi parancsot: ```node app.js```.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-248">Then in your terminal, locate your ```app.js``` file and run the command: ```node app.js```.</span></span>

<span data-ttu-id="cc2fd-249">Ennyi az egész! Építse ki, és máris jó úton jár!</span><span class="sxs-lookup"><span data-stu-id="cc2fd-249">That's it, build it and you're on your way!</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cc2fd-250">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc2fd-250">Next steps</span></span>
* <span data-ttu-id="cc2fd-251">Összetettebb Node.js-mintát szeretne használni?</span><span class="sxs-lookup"><span data-stu-id="cc2fd-251">Want a more complex Node.js sample?</span></span> <span data-ttu-id="cc2fd-252">Lásd: [Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával](documentdb-nodejs-application.md).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-252">See [Build a Node.js web application using Azure Cosmos DB](documentdb-nodejs-application.md).</span></span>
* <span data-ttu-id="cc2fd-253">Ismerje meg, hogyan [figyelhet egy Azure Cosmos DB-fiókot](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="cc2fd-253">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="cc2fd-254">Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-254">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="cc2fd-255">A programozási modellel kapcsolatos további tudnivalókat az [Azure Cosmos DB-dokumentációs oldalának](https://azure.microsoft.com/documentation/services/documentdb/) Develop (Fejlesztés) szakaszában találja.</span><span class="sxs-lookup"><span data-stu-id="cc2fd-255">Learn more about the programming model in the Develop section of the [Azure Cosmos DB documentation page](https://azure.microsoft.com/documentation/services/documentdb/).</span></span>

[create-account]: create-documentdb-dotnet.md#create-account
[keys]: media/documentdb-nodejs-get-started/node-js-tutorial-keys.png
