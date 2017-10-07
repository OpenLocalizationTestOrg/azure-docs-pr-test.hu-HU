---
title: "a Node.js webalkalmazás az Azure Cosmos DB aaaBuild |} Microsoft Docs"
description: "A Node.js-oktatóanyag azt ismerteti, hogyan toouse Microsoft Azure Cosmos DB toostore és a hozzáférési adatok Node.js Express-webalkalmazások Azure Websitesban tárolt."
keywords: "Alkalmazásfejlesztés, adatbázis-oktatóanyag, node.js, a node.js-oktatóanyag megismerése"
services: cosmos-db
documentationcenter: nodejs
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 9da9e63b-e76a-434e-96dd-195ce2699ef3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/14/2017
ms.author: mimig
ms.openlocfilehash: 31194dccf37eef69d2219b0d8328a88d434f79b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="10d2b-104"><a name="_Toc395783175"></a>Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával</span><span class="sxs-lookup"><span data-stu-id="10d2b-104"><a name="_Toc395783175"></a>Build a Node.js web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="10d2b-105">.NET</span><span class="sxs-lookup"><span data-stu-id="10d2b-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="10d2b-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="10d2b-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="10d2b-107">Java</span><span class="sxs-lookup"><span data-stu-id="10d2b-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="10d2b-108">Python</span><span class="sxs-lookup"><span data-stu-id="10d2b-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="10d2b-109">A Node.js-oktatóanyag bemutatja, hogyan toouse Azure Cosmos DB és hello DocumentDB API toostore és a hozzáférési adatok Node.js Express-alkalmazás az Azure Websitesban tárolt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-109">This Node.js tutorial shows you how toouse Azure Cosmos DB and hello DocumentDB API toostore and access data from a Node.js Express application hosted on Azure Websites.</span></span> <span data-ttu-id="10d2b-110">Olyan egyszerű webalapú teendőkezelő alkalmazást, todo appot fog létrehozni, amellyel feladatokat készíthet, kérhet le, és végezhet el.</span><span class="sxs-lookup"><span data-stu-id="10d2b-110">You build a simple web-based task-management application, a ToDo app, that allows creating, retrieving, and completing tasks.</span></span> <span data-ttu-id="10d2b-111">hello feladatok Azure Cosmos DB JSON-dokumentumokként tárolja.</span><span class="sxs-lookup"><span data-stu-id="10d2b-111">hello tasks are stored as JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="10d2b-112">Ez az oktatóanyag bemutatja, hogyan hello létrehozásának és telepítésének hello alkalmazás, és bemutatja, mi történik az összes részlet.</span><span class="sxs-lookup"><span data-stu-id="10d2b-112">This tutorial walks you through hello creation and deployment of hello app and explains what's happening in each snippet.</span></span>

![Képernyőfelvétel a hello jelen Node.js oktatóanyag során létrehozott My Todo List alkalmazás](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

<span data-ttu-id="10d2b-114">Nem rendelkezik idő toocomplete hello oktatóanyag, és most szeretné, hogy tooget hello teljes megoldás?</span><span class="sxs-lookup"><span data-stu-id="10d2b-114">Don't have time toocomplete hello tutorial and just want tooget hello complete solution?</span></span> <span data-ttu-id="10d2b-115">Nem probléma, hogy megkaphassa a hello teljes megoldást a [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="10d2b-115">Not a problem, you can get hello complete sample solution from [GitHub][GitHub].</span></span> <span data-ttu-id="10d2b-116">Csak olvasható hello [információs](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) fájl hogyan toorun hello app kapcsolatos utasításokat.</span><span class="sxs-lookup"><span data-stu-id="10d2b-116">Just read hello [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) file for instructions on how toorun hello app.</span></span>

## <span data-ttu-id="10d2b-117"><a name="_Toc395783176"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="10d2b-117"><a name="_Toc395783176"></a>Prerequisites</span></span>
> [!TIP]
> <span data-ttu-id="10d2b-118">Ez a Node.js-oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a Node.js és az Azure Websites használatát illetően.</span><span class="sxs-lookup"><span data-stu-id="10d2b-118">This Node.js tutorial assumes that you have some prior experience using Node.js and Azure Websites.</span></span>
> 
> 

<span data-ttu-id="10d2b-119">Ez a cikk hello utasításait követve, előtt győződjön meg, hogy rendelkezik-e hello következő:</span><span class="sxs-lookup"><span data-stu-id="10d2b-119">Before following hello instructions in this article, you should ensure that you have hello following:</span></span>

* <span data-ttu-id="10d2b-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="10d2b-120">An active Azure account.</span></span> <span data-ttu-id="10d2b-121">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="10d2b-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="10d2b-122">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="10d2b-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

   <span data-ttu-id="10d2b-123">VAGY</span><span class="sxs-lookup"><span data-stu-id="10d2b-123">OR</span></span>

   <span data-ttu-id="10d2b-124">Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md) (csak Windows).</span><span class="sxs-lookup"><span data-stu-id="10d2b-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md) (Windows only).</span></span>
* <span data-ttu-id="10d2b-125">[Node.js][Node.js]-verzió: 0.10.29-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="10d2b-125">[Node.js][Node.js] version v0.10.29 or higher.</span></span>
* <span data-ttu-id="10d2b-126">[Express generátor](http://www.expressjs.com/starter/generator.html) (az `npm install express-generator -g` segítségével telepítheti)</span><span class="sxs-lookup"><span data-stu-id="10d2b-126">[Express generator](http://www.expressjs.com/starter/generator.html) (you can install this via `npm install express-generator -g`)</span></span>
* <span data-ttu-id="10d2b-127">[Git][Git].</span><span class="sxs-lookup"><span data-stu-id="10d2b-127">[Git][Git].</span></span>

## <span data-ttu-id="10d2b-128"><a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="10d2b-128"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="10d2b-129">Először hozzon létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="10d2b-129">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="10d2b-130">Ha már rendelkezik fiókkal, vagy használatakor hello Azure Cosmos DB emulátor ehhez az oktatóanyaghoz, ugorjon túl[2. lépés: új Node.js-alkalmazás létrehozása](#_Toc395783178).</span><span class="sxs-lookup"><span data-stu-id="10d2b-130">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create a new Node.js application](#_Toc395783178).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="10d2b-131"><a name="_Toc395783178"></a>2. lépés: Új Node.js-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="10d2b-131"><a name="_Toc395783178"></a>Step 2: Create a new Node.js application</span></span>
<span data-ttu-id="10d2b-132">Most megtanulhatja, hogyan toocreate egy alapszintű Hello World Node.js-projektet hello segítségével [Express](http://expressjs.com/) keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="10d2b-132">Now let's learn toocreate a basic Hello World Node.js project using hello [Express](http://expressjs.com/) framework.</span></span>

1. <span data-ttu-id="10d2b-133">Nyissa meg kedvenc terminálját, például a hello Node.js parancssort.</span><span class="sxs-lookup"><span data-stu-id="10d2b-133">Open your favorite terminal, such as hello Node.js command prompt.</span></span>
2. <span data-ttu-id="10d2b-134">Keresse meg, amelyben szeretné toostore hello új alkalmazás toohello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="10d2b-134">Navigate toohello directory in which you'd like toostore hello new application.</span></span>
3. <span data-ttu-id="10d2b-135">Egy új alkalmazást, hello express generátor toogenerate használja **todo**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-135">Use hello express generator toogenerate a new application called **todo**.</span></span>
   
        express todo
4. <span data-ttu-id="10d2b-136">Nyissa meg az új **todo** könyvtárat, és telepítse a függőségeket.</span><span class="sxs-lookup"><span data-stu-id="10d2b-136">Open your new **todo** directory and install dependencies.</span></span>
   
        cd todo
        npm install
5. <span data-ttu-id="10d2b-137">Futtassa az új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10d2b-137">Run your new application.</span></span>
   
        npm start
6. <span data-ttu-id="10d2b-138">Az új alkalmazás megtekintéséhez navigáljon a böngészőben túl[http://localhost: 3000](http://localhost:3000).</span><span class="sxs-lookup"><span data-stu-id="10d2b-138">You can view your new application by navigating your browser too[http://localhost:3000](http://localhost:3000).</span></span>
   
    ![Node.js megismerése – képernyőfelvétel a hello Hello World alkalmazásról egy böngészőablakban](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    <span data-ttu-id="10d2b-140">Ezt követően toostop hello alkalmazás, nyomja le a CTRL + C hello terminálablakot, és kattintson a **y** tooterminate hello kötegelt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-140">Then, toostop hello application, press CTRL+C in hello terminal window and then click **y** tooterminate hello batch job.</span></span>

## <span data-ttu-id="10d2b-141"><a name="_Toc395783179"></a>3. lépés: További modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="10d2b-141"><a name="_Toc395783179"></a>Step 3: Install additional modules</span></span>
<span data-ttu-id="10d2b-142">Hello **package.json** fájl egyike hello projekt gyökerében hello létrehozott hello fájlok.</span><span class="sxs-lookup"><span data-stu-id="10d2b-142">hello **package.json** file is one of hello files created in hello root of hello project.</span></span> <span data-ttu-id="10d2b-143">Ez a fájl tartalmazza a Node.js-alkalmazáshoz szükséges további modulok listáját.</span><span class="sxs-lookup"><span data-stu-id="10d2b-143">This file contains a list of additional modules that are required for your Node.js application.</span></span> <span data-ttu-id="10d2b-144">Később az alkalmazás tooAzure webhelyek központi telepítésekor a fájl használt toodetermine melyik modulokat kell toobe Azure toosupport az alkalmazás telepítve.</span><span class="sxs-lookup"><span data-stu-id="10d2b-144">Later, when you deploy this application tooAzure Websites, this file is used toodetermine which modules need toobe installed on Azure toosupport your application.</span></span> <span data-ttu-id="10d2b-145">Továbbra is kell tooinstall két további csomagok ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="10d2b-145">We still need tooinstall two more packages for this tutorial.</span></span>

1. <span data-ttu-id="10d2b-146">Vissza a Terminálszolgáltatások hello telepítése hello **aszinkron** modult az npm.</span><span class="sxs-lookup"><span data-stu-id="10d2b-146">Back in hello terminal, install hello **async** module via npm.</span></span>
   
        npm install async --save
2. <span data-ttu-id="10d2b-147">Telepítse a hello **documentdb** modult az npm.</span><span class="sxs-lookup"><span data-stu-id="10d2b-147">Install hello **documentdb** module via npm.</span></span> <span data-ttu-id="10d2b-148">Ez a hello modul, ahol minden hello Azure Cosmos DB magic történik.</span><span class="sxs-lookup"><span data-stu-id="10d2b-148">This is hello module where all hello Azure Cosmos DB magic happens.</span></span>
   
        npm install documentdb --save
3. <span data-ttu-id="10d2b-149">A Gyorsellenőrzés hello **package.json** hello alkalmazás fájlt meg kell jelennie hello további modulok.</span><span class="sxs-lookup"><span data-stu-id="10d2b-149">A quick check of hello **package.json** file of hello application should show hello additional modules.</span></span> <span data-ttu-id="10d2b-150">Ez a fájl utasítja az Azure melyik csomagok toodownload, és a telepítse az alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="10d2b-150">This file will tell Azure which packages toodownload and install when running your application.</span></span> <span data-ttu-id="10d2b-151">Hello az alábbi példa azt kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="10d2b-151">It should resemble hello example below.</span></span>
   
        {
          "name": "todo",
          "version": "0.0.0",
          "private": true,
          "scripts": {
            "start": "node ./bin/www"
          },
          "dependencies": {
            "async": "^2.1.4",
            "body-parser": "~1.15.2",
            "cookie-parser": "~1.4.3",
            "debug": "~2.2.0",
            "documentdb": "^1.10.0",
            "express": "~4.14.0",
            "jade": "~1.11.0",
            "morgan": "~1.7.0",
            "serve-favicon": "~2.3.0"
          }
        }
   
    <span data-ttu-id="10d2b-152">Ez értesíti a Node-ot (majd később az Azure-t) arról, hogy az alkalmazás ezektől a további moduloktól függ.</span><span class="sxs-lookup"><span data-stu-id="10d2b-152">This tells Node (and Azure later) that your application depends on these additional modules.</span></span>

## <span data-ttu-id="10d2b-153"><a name="_Toc395783180"></a>4. lépés: Hello Azure Cosmos DB szolgáltatás használata node.js-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="10d2b-153"><a name="_Toc395783180"></a>Step 4: Using hello Azure Cosmos DB service in a node application</span></span>
<span data-ttu-id="10d2b-154">Amely gondoskodik összes hello kezdeti beállítás és konfiguráció, most hozzuk get le toowhy segítünk, és néhány code Azure Cosmos DB használatával toowrite.</span><span class="sxs-lookup"><span data-stu-id="10d2b-154">That takes care of all hello initial setup and configuration, now let’s get down toowhy we’re here, and that’s toowrite some code using Azure Cosmos DB.</span></span>

### <a name="create-hello-model"></a><span data-ttu-id="10d2b-155">Hello modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="10d2b-155">Create hello model</span></span>
1. <span data-ttu-id="10d2b-156">Hello projektkönyvtárban hozzon létre egy új könyvtárat nevű **modellek** a hello hello package.json fájl könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="10d2b-156">In hello project directory, create a new directory named **models** in hello same directory as hello package.json file.</span></span>
2. <span data-ttu-id="10d2b-157">A hello **modellek** könyvtár, hozzon létre egy új fájlt **taskDao.js**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-157">In hello **models** directory, create a new file named **taskDao.js**.</span></span> <span data-ttu-id="10d2b-158">Ezt a fájlt fogja tartalmazni az alkalmazás által létrehozott hello feladatok hello modelljét.</span><span class="sxs-lookup"><span data-stu-id="10d2b-158">This file will contain hello model for hello tasks created by our application.</span></span>
3. <span data-ttu-id="10d2b-159">Az azonos hello **modellek** könyvtár, hozzon létre egy másik új fájlt **docdbUtils.js**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-159">In hello same **models** directory, create another new file named **docdbUtils.js**.</span></span> <span data-ttu-id="10d2b-160">Ez a fájl néhány hasznos, újrafelhasználható, az alkalmazás minden területén használt kódot tartalmaz majd.</span><span class="sxs-lookup"><span data-stu-id="10d2b-160">This file will contain some useful, reusable, code that we will use throughout our application.</span></span> 
4. <span data-ttu-id="10d2b-161">Másolás hello alábbi kódot túl**docdbUtils.js**</span><span class="sxs-lookup"><span data-stu-id="10d2b-161">Copy hello following code in too**docdbUtils.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
   
        var DocDBUtils = {
            getOrCreateDatabase: function (client, databaseId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id= @id',
                    parameters: [{
                        name: '@id',
                        value: databaseId
                    }]
                };
   
                client.queryDatabases(querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        if (results.length === 0) {
                            var databaseSpec = {
                                id: databaseId
                            };
   
                            client.createDatabase(databaseSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            },
   
            getOrCreateCollection: function (client, databaseLink, collectionId, callback) {
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id=@id',
                    parameters: [{
                        name: '@id',
                        value: collectionId
                    }]
                };               
   
                client.queryCollections(databaseLink, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {        
                        if (results.length === 0) {
                            var collectionSpec = {
                                id: collectionId
                            };
   
                            client.createCollection(databaseLink, collectionSpec, function (err, created) {
                                callback(null, created);
                            });
   
                        } else {
                            callback(null, results[0]);
                        }
                    }
                });
            }
        };
   
        module.exports = DocDBUtils;
   
5. <span data-ttu-id="10d2b-162">Mentse és zárja be a hello **docdbUtils.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-162">Save and close hello **docdbUtils.js** file.</span></span>
6. <span data-ttu-id="10d2b-163">Hello hello elején **taskDao.js** fájlt, adja hozzá a következő kód tooreference hello hello **DocumentDBClient** és hello **docdbUtils.js** a fenti létrehozott:</span><span class="sxs-lookup"><span data-stu-id="10d2b-163">At hello beginning of hello **taskDao.js** file, add hello following code tooreference hello **DocumentDBClient** and hello **docdbUtils.js** we created above:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. <span data-ttu-id="10d2b-164">Ezután lesz kód toodefine hozzáadása és hello feladat objektum exportálása.</span><span class="sxs-lookup"><span data-stu-id="10d2b-164">Next, you will add code toodefine and export hello Task object.</span></span> <span data-ttu-id="10d2b-165">Ez felelős a feladatobjektum elindításáért, valamint beállítja a hello adatbázis és dokumentumgyűjtemény fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="10d2b-165">This is responsible for initializing our Task object and setting up hello Database and Document Collection we will use.</span></span>
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. <span data-ttu-id="10d2b-166">Ezután adja hozzá hello kód toodefine további módszereket követően hello feladatobjektum, amelyek lehetővé teszik az Azure Cosmos DB tárolt adatok interakció.</span><span class="sxs-lookup"><span data-stu-id="10d2b-166">Next, add hello following code toodefine additional methods on hello Task object, which allow interactions with data stored in Azure Cosmos DB.</span></span>
   
        TaskDao.prototype = {
            init: function (callback) {
                var self = this;
   
                docdbUtils.getOrCreateDatabase(self.client, self.databaseId, function (err, db) {
                    if (err) {
                        callback(err);
                    } else {
                        self.database = db;
                        docdbUtils.getOrCreateCollection(self.client, self.database._self, self.collectionId, function (err, coll) {
                            if (err) {
                                callback(err);
   
                            } else {
                                self.collection = coll;
                            }
                        });
                    }
                });
            },
   
            find: function (querySpec, callback) {
                var self = this;
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results);
                    }
                });
            },
   
            addItem: function (item, callback) {
                var self = this;
   
                item.date = Date.now();
                item.completed = false;
   
                self.client.createDocument(self.collection._self, item, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, doc);
                    }
                });
            },
   
            updateItem: function (itemId, callback) {
                var self = this;
   
                self.getItem(itemId, function (err, doc) {
                    if (err) {
                        callback(err);
   
                    } else {
                        doc.completed = true;
   
                        self.client.replaceDocument(doc._self, doc, function (err, replaced) {
                            if (err) {
                                callback(err);
   
                            } else {
                                callback(null, replaced);
                            }
                        });
                    }
                });
            },
   
            getItem: function (itemId, callback) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.id = @id',
                    parameters: [{
                        name: '@id',
                        value: itemId
                    }]
                };
   
                self.client.queryDocuments(self.collection._self, querySpec).toArray(function (err, results) {
                    if (err) {
                        callback(err);
   
                    } else {
                        callback(null, results[0]);
                    }
                });
            }
        };
9. <span data-ttu-id="10d2b-167">Mentse és zárja be a hello **taskDao.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-167">Save and close hello **taskDao.js** file.</span></span> 

### <a name="create-hello-controller"></a><span data-ttu-id="10d2b-168">Hello tartományvezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="10d2b-168">Create hello controller</span></span>
1. <span data-ttu-id="10d2b-169">A hello **útvonalak** a projekt könyvtárában hozzon létre egy új fájlt **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-169">In hello **routes** directory of your project, create a new file named **tasklist.js**.</span></span> 
2. <span data-ttu-id="10d2b-170">Adja hozzá a következő kód túl hello**tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-170">Add hello following code too**tasklist.js**.</span></span> <span data-ttu-id="10d2b-171">Ez betölti a hello DocumentDBClient és async modult, amely által használt **tasklist.js**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-171">This loads hello DocumentDBClient and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="10d2b-172">Ez is definiálva hello **TaskList** függvény, amelyet hello példányának **feladat** objektum korábban meghatározott:</span><span class="sxs-lookup"><span data-stu-id="10d2b-172">This also defined hello **TaskList** function, which is passed an instance of hello **Task** object we defined earlier:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. <span data-ttu-id="10d2b-173">Hozzáadja a toohello **tasklist.js** fájl túl használt hello metódusok hozzáadásával**showTasks, Addtasks**, és **completeTasks**:</span><span class="sxs-lookup"><span data-stu-id="10d2b-173">Continue adding toohello **tasklist.js** file by adding hello methods used too**showTasks, addTask**, and **completeTasks**:</span></span>
   
        TaskList.prototype = {
            showTasks: function (req, res) {
                var self = this;
   
                var querySpec = {
                    query: 'SELECT * FROM root r WHERE r.completed=@completed',
                    parameters: [{
                        name: '@completed',
                        value: false
                    }]
                };
   
                self.taskDao.find(querySpec, function (err, items) {
                    if (err) {
                        throw (err);
                    }
   
                    res.render('index', {
                        title: 'My ToDo List ',
                        tasks: items
                    });
                });
            },
   
            addTask: function (req, res) {
                var self = this;
                var item = req.body;
   
                self.taskDao.addItem(item, function (err) {
                    if (err) {
                        throw (err);
                    }
   
                    res.redirect('/');
                });
            },
   
            completeTask: function (req, res) {
                var self = this;
                var completedTasks = Object.keys(req.body);
   
                async.forEach(completedTasks, function taskIterator(completedTask, callback) {
                    self.taskDao.updateItem(completedTask, function (err) {
                        if (err) {
                            callback(err);
                        } else {
                            callback(null);
                        }
                    });
                }, function goHome(err) {
                    if (err) {
                        throw err;
                    } else {
                        res.redirect('/');
                    }
                });
            }
        };
4. <span data-ttu-id="10d2b-174">Mentse és zárja be a hello **tasklist.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-174">Save and close hello **tasklist.js** file.</span></span>

### <a name="add-configjs"></a><span data-ttu-id="10d2b-175">A config.js fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="10d2b-175">Add config.js</span></span>
1. <span data-ttu-id="10d2b-176">A projektkönyvtárban hozzon létre egy új fájlt **config.js** néven.</span><span class="sxs-lookup"><span data-stu-id="10d2b-176">In your project directory create a new file named **config.js**.</span></span>
2. <span data-ttu-id="10d2b-177">Adja hozzá a hello túl a következő**config.js**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-177">Add hello following too**config.js**.</span></span> <span data-ttu-id="10d2b-178">Ez meghatározza az alkalmazáshoz szükséges konfigurációs beállításokat és értékeket.</span><span class="sxs-lookup"><span data-stu-id="10d2b-178">This defines configuration settings and values needed for our application.</span></span>
   
        var config = {}
   
        config.host = process.env.HOST || "[hello URI value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[hello PRIMARY KEY value from hello Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. <span data-ttu-id="10d2b-179">A hello **config.js** fájl, a frissítés hello értékei HOST és AUTH_KEY hello (kulcsok) panelén hello Azure Cosmos DB fiókjában található hello értékek [Microsoft Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="10d2b-179">In hello **config.js** file, update hello values of HOST and AUTH_KEY using hello values found in hello Keys blade of your Azure Cosmos DB account on hello [Microsoft Azure portal](https://portal.azure.com).</span></span>
4. <span data-ttu-id="10d2b-180">Mentse és zárja be a hello **config.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-180">Save and close hello **config.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="10d2b-181">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="10d2b-181">Modify app.js</span></span>
1. <span data-ttu-id="10d2b-182">Hello projekt könyvtárában nyissa meg hello **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-182">In hello project directory, open hello **app.js** file.</span></span> <span data-ttu-id="10d2b-183">Ez a fájl korábban létrejött hello Express-webalkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="10d2b-183">This file was created earlier when hello Express web application was created.</span></span>
2. <span data-ttu-id="10d2b-184">Adja hozzá a következő kód toohello felső részén hello **app.js**</span><span class="sxs-lookup"><span data-stu-id="10d2b-184">Add hello following code toohello top of **app.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. <span data-ttu-id="10d2b-185">Ez a kód hello konfigurációs fájl toobe használt határozza meg, és tooread értékeket abból néhány változó, amelyeket hamarosan használni fog a eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="10d2b-185">This code defines hello config file toobe used, and proceeds tooread values out of this file into some variables we will use soon.</span></span>
4. <span data-ttu-id="10d2b-186">Cserélje le az alábbi két hello **app.js** fájlt:</span><span class="sxs-lookup"><span data-stu-id="10d2b-186">Replace hello following two lines in **app.js** file:</span></span>
   
        app.use('/', index);
        app.use('/users', users); 
   
      <span data-ttu-id="10d2b-187">a következő kódrészletet hello:</span><span class="sxs-lookup"><span data-stu-id="10d2b-187">with hello following snippet:</span></span>
   
        var docDbClient = new DocumentDBClient(config.host, {
            masterKey: config.authKey
        });
        var taskDao = new TaskDao(docDbClient, config.databaseId, config.collectionId);
        var taskList = new TaskList(taskDao);
        taskDao.init();
   
        app.get('/', taskList.showTasks.bind(taskList));
        app.post('/addtask', taskList.addTask.bind(taskList));
        app.post('/completetask', taskList.completeTask.bind(taskList));
        app.set('view engine', 'jade');
5. <span data-ttu-id="10d2b-188">Ezek a sorok meghatározzák egy új példányt a **TaskDao** objektum, egy új kapcsolat tooAzure Cosmos DB (hello hello értékekkel olvasni **config.js**) hello feladat objektum inicializálása., majd társítanak a toomethods a **TaskList** vezérlő.</span><span class="sxs-lookup"><span data-stu-id="10d2b-188">These lines define a new instance of our **TaskDao** object, with a new connection tooAzure Cosmos DB (using hello values read from hello **config.js**), initialize hello task object and then bind form actions toomethods on our **TaskList** controller.</span></span> 
6. <span data-ttu-id="10d2b-189">Végül mentse és zárja be a hello **app.js** fájlt, hogy szinte végzett.</span><span class="sxs-lookup"><span data-stu-id="10d2b-189">Finally, save and close hello **app.js** file, we're just about done.</span></span>

## <span data-ttu-id="10d2b-190"><a name="_Toc395783181"></a>5. lépés: Felhasználói felület létrehozása</span><span class="sxs-lookup"><span data-stu-id="10d2b-190"><a name="_Toc395783181"></a>Step 5: Build a user interface</span></span>
<span data-ttu-id="10d2b-191">Most adjuk a figyelmet toobuilding hello felhasználói felületét, így a felhasználók ténylegesen használatba vehessék az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10d2b-191">Now let’s turn our attention toobuilding hello user interface so a user can actually interact with our application.</span></span> <span data-ttu-id="10d2b-192">hello használ létrehozott Express-alkalmazás **Jade** , hello megjelenítési motort.</span><span class="sxs-lookup"><span data-stu-id="10d2b-192">hello Express application we created uses **Jade** as hello view engine.</span></span> <span data-ttu-id="10d2b-193">További információ a Jade tekintse meg túl[http://jade-lang.com/](http://jade-lang.com/).</span><span class="sxs-lookup"><span data-stu-id="10d2b-193">For more information on Jade please refer too[http://jade-lang.com/](http://jade-lang.com/).</span></span>

1. <span data-ttu-id="10d2b-194">Hello **Views** hello fájlban **nézetek** directory globális sablonként szolgál az egyéb **.jade** fájlokat.</span><span class="sxs-lookup"><span data-stu-id="10d2b-194">hello **layout.jade** file in hello **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="10d2b-195">Ebben a lépésben lesz a módosítás toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), amelyen egy eszközkészlet, így könnyen toodesign egy töltött tetszetős webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="10d2b-195">In this step you will modify it toouse [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy toodesign a nice looking website.</span></span> 
2. <span data-ttu-id="10d2b-196">Nyissa meg hello **Views** fájl található a hello **nézetek** hello következőre mappa és a név felülírandó hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="10d2b-196">Open hello **layout.jade** file found in hello **views** folder and replace hello contents with hello following:</span></span>

    ```
    doctype html
    html
      head
        title= title
        link(rel='stylesheet', href='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css')
        link(rel='stylesheet', href='/stylesheets/style.css')
      body
        nav.navbar.navbar-inverse.navbar-fixed-top
          div.navbar-header
            a.navbar-brand(href='#') My Tasks
        block content
        script(src='//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js')
        script(src='//ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js')
    ```

    <span data-ttu-id="10d2b-197">Ez gyakorlatilag megmondja hello **Jade** motor toorender az alkalmazás HTML-kódot, és létrehoz egy **blokk** nevű **tartalom** ahol megadhatja hello elrendezés a tartalomhoz lapok.</span><span class="sxs-lookup"><span data-stu-id="10d2b-197">This effectively tells hello **Jade** engine toorender some HTML for our application and creates a **block** called **content** where we can supply hello layout for our content pages.</span></span>

    <span data-ttu-id="10d2b-198">Mentse és zárja be a **layout.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-198">Save and close this **layout.jade** file.</span></span>

3. <span data-ttu-id="10d2b-199">Most nyissa meg a hello **index.jade** fájlt, az alkalmazás által használható, és cserélje ki hello fájl tartalma hello hello következőre hello megtekintése:</span><span class="sxs-lookup"><span data-stu-id="10d2b-199">Now open hello **index.jade** file, hello view that will be used by our application, and replace hello content of hello file with hello following:</span></span>
   
        extends layout
        block content
           h1 #{title}
           br
        
           form(action="/completetask", method="post")
             table.table.table-striped.table-bordered
               tr
                 td Name
                 td Category
                 td Date
                 td Complete
               if (typeof tasks === "undefined")
                 tr
                   td
               else
                 each task in tasks
                   tr
                     td #{task.name}
                     td #{task.category}
                     - var date  = new Date(task.date);
                     - var day   = date.getDate();
                     - var month = date.getMonth() + 1;
                     - var year  = date.getFullYear();
                     td #{month + "/" + day + "/" + year}
                     td
                       input(type="checkbox", name="#{task.id}", value="#{!task.completed}", checked=task.completed)
             button.btn.btn-primary(type="submit") Update tasks
           hr
           form.well(action="/addtask", method="post")
             .form-group
               label(for="name") Item Name:
               input.form-control(name="name", type="textbox")
             .form-group
               label(for="category") Item Category:
               input.form-control(name="category", type="textbox")
             br
             button.btn(type="submit") Add item
   

<span data-ttu-id="10d2b-200">Ez kibővíti az elrendezést, és tartalmat biztosít hello **tartalom** imént látott hello helyőrző **Views** korábbi fájlt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-200">This extends layout, and provides content for hello **content** placeholder we saw in hello **layout.jade** file earlier.</span></span>
   
<span data-ttu-id="10d2b-201">Ebben az elrendezésben két HTML-űrlapot hoztunk létre.</span><span class="sxs-lookup"><span data-stu-id="10d2b-201">In this layout we created two HTML forms.</span></span>

<span data-ttu-id="10d2b-202">hello első űrlap az adatok és a gomb használatával kérdezhetjük túl elhelyezésével tooupdate elemek táblát tartalmaz**/completetask** metódusának.</span><span class="sxs-lookup"><span data-stu-id="10d2b-202">hello first form contains a table for our data and a button that allows us tooupdate items by posting too**/completetask** method of our controller.</span></span>
    
<span data-ttu-id="10d2b-203">hello második űrlap két beviteli mezőt és egy gombot, amely lehetővé teszi új elem toocreate elhelyezésével túl tartalmaz**/addtask** metódusának.</span><span class="sxs-lookup"><span data-stu-id="10d2b-203">hello second form contains two input fields and a button that allows us toocreate a new item by posting too**/addtask** method of our controller.</span></span>

<span data-ttu-id="10d2b-204">Csak az alkalmazás toowork szükséges.</span><span class="sxs-lookup"><span data-stu-id="10d2b-204">This should be all that we need for our application toowork.</span></span>

## <span data-ttu-id="10d2b-205"><a name="_Toc395783181"></a>6. lépés: Az alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="10d2b-205"><a name="_Toc395783181"></a>Step 6: Run your application locally</span></span>
1. <span data-ttu-id="10d2b-206">tootest hello alkalmazás a helyi számítógépen futni `npm start` a hello terminál toostart az alkalmazást, majd frissítse a [http://localhost: 3000](http://localhost:3000) webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="10d2b-206">tootest hello application on your local machine, run `npm start` in hello terminal toostart your application, then refresh your [http://localhost:3000](http://localhost:3000) browser page.</span></span> <span data-ttu-id="10d2b-207">hello lap most hello az alábbi képen hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="10d2b-207">hello page should now look like hello image below:</span></span>
   
    ![Képernyőfelvétel a hello My ToDo List alkalmazásról egy böngészőablakban](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > <span data-ttu-id="10d2b-209">Ha hello Views vagy hello index.jade fájl hello francia kapcsolatos hibaüzenetet kap, akkor győződjön meg arról, hogy mindkét fájl első két sora hello bal oldali indokolt, szóközök nélkül.</span><span class="sxs-lookup"><span data-stu-id="10d2b-209">If you receive an error about hello indent in hello layout.jade file or hello index.jade file, ensure that hello first two lines in both files is left justified, with no spaces.</span></span> <span data-ttu-id="10d2b-210">Ha hello első két sor elé, távolítsa el őket, mentse a fájlt, majd frissítse a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-210">If there are spaces before hello first two lines, remove them, save both files, then refresh your browser window.</span></span> 

2. <span data-ttu-id="10d2b-211">Hello elemet, az elem neve és a kategória mező tooenter egy új feladatot használja, és kattintson a **elem hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-211">Use hello Item, Item Name and Category fields tooenter a new task and then click **Add Item**.</span></span> <span data-ttu-id="10d2b-212">Ez egy új dokumentumot hoz létre az Azure Cosmos DB-ben a megadott tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="10d2b-212">This creates a document in Azure Cosmos DB with those properties.</span></span> 
3. <span data-ttu-id="10d2b-213">hello oldal ekkor frissül, az újonnan létrehozott elemet a teendőlistában hello toodisplay hello.</span><span class="sxs-lookup"><span data-stu-id="10d2b-213">hello page should update toodisplay hello newly created item in hello ToDo list.</span></span>
   
    ![Képernyőfelvétel a hello alkalmazás hello teendőlista új eleméről](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. <span data-ttu-id="10d2b-215">toocomplete egy feladatot, egyszerűen jelölje be a hello teljes oszlopban hello jelölőnégyzetet, és kattintson a **feladatok frissítése**.</span><span class="sxs-lookup"><span data-stu-id="10d2b-215">toocomplete a task, simply check hello checkbox in hello Complete column, and then click **Update tasks**.</span></span> <span data-ttu-id="10d2b-216">Ezzel frissíti a már létrehozott hello dokumentum.</span><span class="sxs-lookup"><span data-stu-id="10d2b-216">This updates hello document you already created.</span></span>

5. <span data-ttu-id="10d2b-217">toostop hello alkalmazás, nyomja le a CTRL + C hello terminálablakot, és kattintson a **Y** tooterminate hello kötegelt.</span><span class="sxs-lookup"><span data-stu-id="10d2b-217">toostop hello application, press CTRL+C in hello terminal window and then click **Y** tooterminate hello batch job.</span></span>

## <span data-ttu-id="10d2b-218"><a name="_Toc395783182"></a>7. lépés: Az alkalmazás fejlesztési projekt tooAzure webhelyek központi telepítése</span><span class="sxs-lookup"><span data-stu-id="10d2b-218"><a name="_Toc395783182"></a>Step 7: Deploy your application development project tooAzure Websites</span></span>
1. <span data-ttu-id="10d2b-219">Ha még nem tette meg, engedélyezzen egy Git-tárházat az Azure Websites számára.</span><span class="sxs-lookup"><span data-stu-id="10d2b-219">If you haven't already, enable a git repository for your Azure Website.</span></span> <span data-ttu-id="10d2b-220">Hogyan találhat útmutatót toodo ezt a hello [helyi Git-telepítésének tooAzure App Service](../app-service-web/app-service-deploy-local-git.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="10d2b-220">You can find instructions on how toodo this in hello [Local Git Deployment tooAzure App Service](../app-service-web/app-service-deploy-local-git.md) topic.</span></span>
2. <span data-ttu-id="10d2b-221">Adja hozzá Azure-webhelyét távoli Git-elemként.</span><span class="sxs-lookup"><span data-stu-id="10d2b-221">Add your Azure Website as a git remote.</span></span>
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. <span data-ttu-id="10d2b-222">Telepítsen toohello távoli küldésével.</span><span class="sxs-lookup"><span data-stu-id="10d2b-222">Deploy by pushing toohello remote.</span></span>
   
        git push azure master
4. <span data-ttu-id="10d2b-223">Néhány másodpercen belül git befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!</span><span class="sxs-lookup"><span data-stu-id="10d2b-223">In a few seconds, git will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>

    <span data-ttu-id="10d2b-224">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="10d2b-224">Congratulations!</span></span> <span data-ttu-id="10d2b-225">Ebben az esetben az első Node.js Express-webalkalmazását Azure Cosmos DB használatával építve, és közzétette azt tooAzure webhelyek.</span><span class="sxs-lookup"><span data-stu-id="10d2b-225">You have just built your first Node.js Express Web Application using Azure Cosmos DB and published it tooAzure Websites.</span></span>

    <span data-ttu-id="10d2b-226">Ha szeretné, hogy toodownload, vagy tekintse meg a teljes referenciaalkalmazás toohello ehhez az oktatóanyaghoz, le is tölthetők: [GitHub][GitHub].</span><span class="sxs-lookup"><span data-stu-id="10d2b-226">If you want toodownload or refer toohello complete reference application for this tutorial, it can be downloaded from [GitHub][GitHub].</span></span>

## <span data-ttu-id="10d2b-227"><a name="_Toc395637775"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10d2b-227"><a name="_Toc395637775"></a>Next steps</span></span>

* <span data-ttu-id="10d2b-228">Szeretné, hogy tooperform méretezés és teljesítmény Azure Cosmos DB tesztelték?</span><span class="sxs-lookup"><span data-stu-id="10d2b-228">Want tooperform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="10d2b-229">Tekintse meg a következőt: [Teljesítmény- és mérettesztelés az Azure Cosmos DB használatával](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="10d2b-229">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="10d2b-230">Ismerje meg, hogyan túl[figyelése Azure Cosmos DB fiók](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="10d2b-230">Learn how too[monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="10d2b-231">A minta-adatkészleteken hello lekérdezéseinek futtatásához [Tesztlekérdezéseket](https://www.documentdb.com/sql/demo).</span><span class="sxs-lookup"><span data-stu-id="10d2b-231">Run queries against our sample dataset in hello [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="10d2b-232">Fedezze fel hello [Azure Cosmos DB dokumentáció](https://docs.microsoft.com/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="10d2b-232">Explore hello [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/documentdb/).</span></span>

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

