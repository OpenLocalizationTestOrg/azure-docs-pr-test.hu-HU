---
title: "A Node.js-webalkalmazás létrehozása az Azure Cosmos DB |} Microsoft Docs"
description: "A Node.js-oktatóanyag Azure Websitesban tárolt Node.js Express-webalkalmazások használata a Microsoft Azure Cosmos DB tárolására és a hozzáférési adatok ismerteti."
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
ms.openlocfilehash: 1a98509a98bcd2a5de593eb006f905766fe72966
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <span data-ttu-id="8c18c-104"><a name="_Toc395783175"></a>Node.js-webalkalmazás létrehozása az Azure Cosmos DB használatával</span><span class="sxs-lookup"><span data-stu-id="8c18c-104"><a name="_Toc395783175"></a>Build a Node.js web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8c18c-105">.NET</span><span class="sxs-lookup"><span data-stu-id="8c18c-105">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="8c18c-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="8c18c-106">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="8c18c-107">Java</span><span class="sxs-lookup"><span data-stu-id="8c18c-107">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="8c18c-108">Python</span><span class="sxs-lookup"><span data-stu-id="8c18c-108">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="8c18c-109">A Node.js-oktatóanyag bemutatja, hogyan tárolásához Azure Cosmos DB és a DocumentDB API használata és a hozzáférési adatok az Azure Websitesban tárolt Node.js Express-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8c18c-109">This Node.js tutorial shows you how to use Azure Cosmos DB and the DocumentDB API to store and access data from a Node.js Express application hosted on Azure Websites.</span></span> <span data-ttu-id="8c18c-110">Olyan egyszerű webalapú teendőkezelő alkalmazást, todo appot fog létrehozni, amellyel feladatokat készíthet, kérhet le, és végezhet el.</span><span class="sxs-lookup"><span data-stu-id="8c18c-110">You build a simple web-based task-management application, a ToDo app, that allows creating, retrieving, and completing tasks.</span></span> <span data-ttu-id="8c18c-111">A feladatokat JSON-dokumentumok formájában tárolja az Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="8c18c-111">The tasks are stored as JSON documents in Azure Cosmos DB.</span></span> <span data-ttu-id="8c18c-112">Ez az oktatóanyag bemutatja az alkalmazás létrehozásának és üzembe helyezésének lépéseit, valamint hogy mi történik az egyes kódrészletekben.</span><span class="sxs-lookup"><span data-stu-id="8c18c-112">This tutorial walks you through the creation and deployment of the app and explains what's happening in each snippet.</span></span>

![Képernyőfelvétel a jelen Node.js oktatóanyag során készített My Todo List (Saját teendőlista) alkalmazásról](./media/documentdb-nodejs-application/cosmos-db-node-js-mytodo.png)

<span data-ttu-id="8c18c-114">Nincs ideje elvégezni az oktatóanyagot, és csak hozzá szeretne jutni a teljes megoldáshoz?</span><span class="sxs-lookup"><span data-stu-id="8c18c-114">Don't have time to complete the tutorial and just want to get the complete solution?</span></span> <span data-ttu-id="8c18c-115">Semmi gond, a teljes mintamegoldást beszerezheti a [GitHubról][GitHub].</span><span class="sxs-lookup"><span data-stu-id="8c18c-115">Not a problem, you can get the complete sample solution from [GitHub][GitHub].</span></span> <span data-ttu-id="8c18c-116">Az alkalmazás futtatásához szükséges útmutatást az [Olvass el](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) fájlban találja.</span><span class="sxs-lookup"><span data-stu-id="8c18c-116">Just read the [Readme](https://github.com/Azure-Samples/documentdb-node-todo-app/blob/master/README.md) file for instructions on how to run the app.</span></span>

## <span data-ttu-id="8c18c-117"><a name="_Toc395783176"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c18c-117"><a name="_Toc395783176"></a>Prerequisites</span></span>
> [!TIP]
> <span data-ttu-id="8c18c-118">Ez a Node.js-oktatóanyag feltételezi, hogy rendelkezik némi tapasztalattal a Node.js és az Azure Websites használatát illetően.</span><span class="sxs-lookup"><span data-stu-id="8c18c-118">This Node.js tutorial assumes that you have some prior experience using Node.js and Azure Websites.</span></span>
> 
> 

<span data-ttu-id="8c18c-119">A jelen cikkben lévő utasítások követése előtt rendelkeznie kell a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="8c18c-119">Before following the instructions in this article, you should ensure that you have the following:</span></span>

* <span data-ttu-id="8c18c-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="8c18c-120">An active Azure account.</span></span> <span data-ttu-id="8c18c-121">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="8c18c-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8c18c-122">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8c18c-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

   <span data-ttu-id="8c18c-123">VAGY</span><span class="sxs-lookup"><span data-stu-id="8c18c-123">OR</span></span>

   <span data-ttu-id="8c18c-124">Egy helyi telepítését teszi a [Azure Cosmos DB emulátor](local-emulator.md) (csak Windows).</span><span class="sxs-lookup"><span data-stu-id="8c18c-124">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md) (Windows only).</span></span>
* <span data-ttu-id="8c18c-125">[Node.js][Node.js]-verzió: 0.10.29-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="8c18c-125">[Node.js][Node.js] version v0.10.29 or higher.</span></span>
* <span data-ttu-id="8c18c-126">[Express generátor](http://www.expressjs.com/starter/generator.html) (az `npm install express-generator -g` segítségével telepítheti)</span><span class="sxs-lookup"><span data-stu-id="8c18c-126">[Express generator](http://www.expressjs.com/starter/generator.html) (you can install this via `npm install express-generator -g`)</span></span>
* <span data-ttu-id="8c18c-127">[Git][Git].</span><span class="sxs-lookup"><span data-stu-id="8c18c-127">[Git][Git].</span></span>

## <span data-ttu-id="8c18c-128"><a name="_Toc395637761"></a>1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c18c-128"><a name="_Toc395637761"></a>Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="8c18c-129">Először hozzon létre egy Azure Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="8c18c-129">Let's start by creating an Azure Cosmos DB account.</span></span> <span data-ttu-id="8c18c-130">Ha már rendelkezik fiókkal, vagy az oktatóanyagban az Azure Cosmos DB Emulatort használja, továbbléphet a [2. lépés: Új Node.js-alkalmazás létrehozása](#_Toc395783178) című lépésre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-130">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create a new Node.js application](#_Toc395783178).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

[!INCLUDE [cosmos-db-keys](../../includes/cosmos-db-keys.md)]

## <span data-ttu-id="8c18c-131"><a name="_Toc395783178"></a>2. lépés: Új Node.js-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c18c-131"><a name="_Toc395783178"></a>Step 2: Create a new Node.js application</span></span>
<span data-ttu-id="8c18c-132">Most megtanulhatja, hogyan hozhat létre egy alapszintű Hello World Node.js-projektet az [Express](http://expressjs.com/)-keretrendszer használatával.</span><span class="sxs-lookup"><span data-stu-id="8c18c-132">Now let's learn to create a basic Hello World Node.js project using the [Express](http://expressjs.com/) framework.</span></span>

1. <span data-ttu-id="8c18c-133">Nyissa meg kedvenc terminálját, például a Node.js parancssort.</span><span class="sxs-lookup"><span data-stu-id="8c18c-133">Open your favorite terminal, such as the Node.js command prompt.</span></span>
2. <span data-ttu-id="8c18c-134">Keresse meg azt a könyvtárat, amelyben tárolni szeretné az új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8c18c-134">Navigate to the directory in which you'd like to store the new application.</span></span>
3. <span data-ttu-id="8c18c-135">Az Express generátor használatával hozzon létre egy új alkalmazást **todo** (teendők) néven.</span><span class="sxs-lookup"><span data-stu-id="8c18c-135">Use the express generator to generate a new application called **todo**.</span></span>
   
        express todo
4. <span data-ttu-id="8c18c-136">Nyissa meg az új **todo** könyvtárat, és telepítse a függőségeket.</span><span class="sxs-lookup"><span data-stu-id="8c18c-136">Open your new **todo** directory and install dependencies.</span></span>
   
        cd todo
        npm install
5. <span data-ttu-id="8c18c-137">Futtassa az új alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8c18c-137">Run your new application.</span></span>
   
        npm start
6. <span data-ttu-id="8c18c-138">Az új alkalmazás megtekintéséhez navigáljon a böngészőben a következő címre: [http://localhost:3000](http://localhost:3000).</span><span class="sxs-lookup"><span data-stu-id="8c18c-138">You can view your new application by navigating your browser to [http://localhost:3000](http://localhost:3000).</span></span>
   
    ![A Node.js megismerése – Képernyőfelvétel a Hello World alkalmazásról egy böngészőablakban](./media/documentdb-nodejs-application/cosmos-db-node-js-express.png)

    <span data-ttu-id="8c18c-140">Ezt követően az alkalmazás leállításához nyomja le a CTRL+C billentyűkombinációt a terminálablakban, majd a kötegelt feladat leállításához kattintson az **y** elemre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-140">Then, to stop the application, press CTRL+C in the terminal window and then click **y** to terminate the batch job.</span></span>

## <span data-ttu-id="8c18c-141"><a name="_Toc395783179"></a>3. lépés: További modulok telepítése</span><span class="sxs-lookup"><span data-stu-id="8c18c-141"><a name="_Toc395783179"></a>Step 3: Install additional modules</span></span>
<span data-ttu-id="8c18c-142">A **package.json** fájl egyike azon fájloknak, amelyek a projekt gyökérmappájában létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="8c18c-142">The **package.json** file is one of the files created in the root of the project.</span></span> <span data-ttu-id="8c18c-143">Ez a fájl tartalmazza a Node.js-alkalmazáshoz szükséges további modulok listáját.</span><span class="sxs-lookup"><span data-stu-id="8c18c-143">This file contains a list of additional modules that are required for your Node.js application.</span></span> <span data-ttu-id="8c18c-144">Később amikor az alkalmazás az Azure Websitesra telepíti, a fájllal határozza meg, melyik modulokat kell az Azure támogatásához az alkalmazás telepítésére.</span><span class="sxs-lookup"><span data-stu-id="8c18c-144">Later, when you deploy this application to Azure Websites, this file is used to determine which modules need to be installed on Azure to support your application.</span></span> <span data-ttu-id="8c18c-145">A jelen oktatóanyag befejezéséhez még két csomag telepítésére van szükség.</span><span class="sxs-lookup"><span data-stu-id="8c18c-145">We still need to install two more packages for this tutorial.</span></span>

1. <span data-ttu-id="8c18c-146">A terminálban telepítse az **async** modult az npm segítségével.</span><span class="sxs-lookup"><span data-stu-id="8c18c-146">Back in the terminal, install the **async** module via npm.</span></span>
   
        npm install async --save
2. <span data-ttu-id="8c18c-147">Telepítse a **DocumentDB** modult az npm segítségével.</span><span class="sxs-lookup"><span data-stu-id="8c18c-147">Install the **documentdb** module via npm.</span></span> <span data-ttu-id="8c18c-148">Ez az, hogy a modul, ahol az összes Azure Cosmos DB magic történik.</span><span class="sxs-lookup"><span data-stu-id="8c18c-148">This is the module where all the Azure Cosmos DB magic happens.</span></span>
   
        npm install documentdb --save
3. <span data-ttu-id="8c18c-149">Ha gyorsan megtekinti a **package.json** fájl tartalmát, láthatja is a további modulokat.</span><span class="sxs-lookup"><span data-stu-id="8c18c-149">A quick check of the **package.json** file of the application should show the additional modules.</span></span> <span data-ttu-id="8c18c-150">Ez a fájl utasítja az Azure-t az alkalmazás futtatásakor szükséges csomagok letöltésére és telepítésére.</span><span class="sxs-lookup"><span data-stu-id="8c18c-150">This file will tell Azure which packages to download and install when running your application.</span></span> <span data-ttu-id="8c18c-151">Ennek az alábbi példához hasonlóan kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="8c18c-151">It should resemble the example below.</span></span>
   
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
   
    <span data-ttu-id="8c18c-152">Ez értesíti a Node-ot (majd később az Azure-t) arról, hogy az alkalmazás ezektől a további moduloktól függ.</span><span class="sxs-lookup"><span data-stu-id="8c18c-152">This tells Node (and Azure later) that your application depends on these additional modules.</span></span>

## <span data-ttu-id="8c18c-153"><a name="_Toc395783180"></a>4. lépés: Az Azure Cosmos DB szolgáltatás használata Node.js-alkalmazásokban</span><span class="sxs-lookup"><span data-stu-id="8c18c-153"><a name="_Toc395783180"></a>Step 4: Using the Azure Cosmos DB service in a node application</span></span>
<span data-ttu-id="8c18c-154">Ezzel a kezdeti beállítás és konfiguráció készen is van. Ideje elkezdeni a kódírást az Azure Cosmos DB használatával.</span><span class="sxs-lookup"><span data-stu-id="8c18c-154">That takes care of all the initial setup and configuration, now let’s get down to why we’re here, and that’s to write some code using Azure Cosmos DB.</span></span>

### <a name="create-the-model"></a><span data-ttu-id="8c18c-155">A modell létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c18c-155">Create the model</span></span>
1. <span data-ttu-id="8c18c-156">A projektkönyvtáron belül hozzon létre egy új könyvtárat **models** (modellek) néven, a package.json fájllal egy könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="8c18c-156">In the project directory, create a new directory named **models** in the same directory as the package.json file.</span></span>
2. <span data-ttu-id="8c18c-157">A **models** könyvtárban hozzon létre egy új fájlt **taskDao.js** néven.</span><span class="sxs-lookup"><span data-stu-id="8c18c-157">In the **models** directory, create a new file named **taskDao.js**.</span></span> <span data-ttu-id="8c18c-158">Ez a fájl tartalmazza majd a modellt az alkalmazás által létrehozott feladatok számára.</span><span class="sxs-lookup"><span data-stu-id="8c18c-158">This file will contain the model for the tasks created by our application.</span></span>
3. <span data-ttu-id="8c18c-159">Ugyanabban a **models** könyvtárban hozzon létre egy másik új fájlt **docdbUtils.js** néven.</span><span class="sxs-lookup"><span data-stu-id="8c18c-159">In the same **models** directory, create another new file named **docdbUtils.js**.</span></span> <span data-ttu-id="8c18c-160">Ez a fájl néhány hasznos, újrafelhasználható, az alkalmazás minden területén használt kódot tartalmaz majd.</span><span class="sxs-lookup"><span data-stu-id="8c18c-160">This file will contain some useful, reusable, code that we will use throughout our application.</span></span> 
4. <span data-ttu-id="8c18c-161">Másolja be az alábbi kódot a **docdbUtils.js** fájlba</span><span class="sxs-lookup"><span data-stu-id="8c18c-161">Copy the following code in to **docdbUtils.js**</span></span>
   
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
   
5. <span data-ttu-id="8c18c-162">Mentse és zárja be a **docdbUtils.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-162">Save and close the **docdbUtils.js** file.</span></span>
6. <span data-ttu-id="8c18c-163">A **taskDao.js** fájl elejéhez adja hozzá a következő kódot a **DocumentDBClient**-ügyfélre és a fentiekben létrehozott **docdbUtils.js** fájlra való hivatkozáshoz:</span><span class="sxs-lookup"><span data-stu-id="8c18c-163">At the beginning of the **taskDao.js** file, add the following code to reference the **DocumentDBClient** and the **docdbUtils.js** we created above:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var docdbUtils = require('./docdbUtils');
7. <span data-ttu-id="8c18c-164">Ezután adja hozzá a feladatobjektum meghatározására és exportálására használt kódot.</span><span class="sxs-lookup"><span data-stu-id="8c18c-164">Next, you will add code to define and export the Task object.</span></span> <span data-ttu-id="8c18c-165">Ez felelős a feladatobjektum elindításáért, valamint a használni kívánt adatbázis és dokumentumgyűjtemény beállításáért.</span><span class="sxs-lookup"><span data-stu-id="8c18c-165">This is responsible for initializing our Task object and setting up the Database and Document Collection we will use.</span></span>
   
        function TaskDao(documentDBClient, databaseId, collectionId) {
          this.client = documentDBClient;
          this.databaseId = databaseId;
          this.collectionId = collectionId;
   
          this.database = null;
          this.collection = null;
        }
   
        module.exports = TaskDao;
8. <span data-ttu-id="8c18c-166">Ezután adja hozzá a következő kódot a feladatobjektumokhoz további metódusok meghatározásához, amelyek lehetővé teszik majd az Azure Cosmos DB-ben tárolt adatokkal folytatott interakciót.</span><span class="sxs-lookup"><span data-stu-id="8c18c-166">Next, add the following code to define additional methods on the Task object, which allow interactions with data stored in Azure Cosmos DB.</span></span>
   
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
9. <span data-ttu-id="8c18c-167">Mentse és zárja be a **taskDao.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-167">Save and close the **taskDao.js** file.</span></span> 

### <a name="create-the-controller"></a><span data-ttu-id="8c18c-168">A vezérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c18c-168">Create the controller</span></span>
1. <span data-ttu-id="8c18c-169">A projekt **routes** könyvtárában hozzon létre egy új fájlt **tasklist.js** néven.</span><span class="sxs-lookup"><span data-stu-id="8c18c-169">In the **routes** directory of your project, create a new file named **tasklist.js**.</span></span> 
2. <span data-ttu-id="8c18c-170">Adja hozzá a következő kódot a **tasklist.js** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="8c18c-170">Add the following code to **tasklist.js**.</span></span> <span data-ttu-id="8c18c-171">Ez betölti a **tasklist.js** fájl által használt DocumentDBClient és async modult.</span><span class="sxs-lookup"><span data-stu-id="8c18c-171">This loads the DocumentDBClient and async modules, which are used by **tasklist.js**.</span></span> <span data-ttu-id="8c18c-172">Emellett a **TaskList** (Feladatlista) függvényt is meghatározta, amelyet a rendszer továbbad a **Task** (Feladat) objektum korábban meghatározott példányának:</span><span class="sxs-lookup"><span data-stu-id="8c18c-172">This also defined the **TaskList** function, which is passed an instance of the **Task** object we defined earlier:</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var async = require('async');
   
        function TaskList(taskDao) {
          this.taskDao = taskDao;
        }
   
        module.exports = TaskList;
3. <span data-ttu-id="8c18c-173">Folytassa a **tasklist.js** fájlhoz való hozzáadást a **showTasks, addTasks** és **completeTasks** által használt metódusok hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="8c18c-173">Continue adding to the **tasklist.js** file by adding the methods used to **showTasks, addTask**, and **completeTasks**:</span></span>
   
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
4. <span data-ttu-id="8c18c-174">Mentse és zárja be a **tasklist.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-174">Save and close the **tasklist.js** file.</span></span>

### <a name="add-configjs"></a><span data-ttu-id="8c18c-175">A config.js fájl hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8c18c-175">Add config.js</span></span>
1. <span data-ttu-id="8c18c-176">A projektkönyvtárban hozzon létre egy új fájlt **config.js** néven.</span><span class="sxs-lookup"><span data-stu-id="8c18c-176">In your project directory create a new file named **config.js**.</span></span>
2. <span data-ttu-id="8c18c-177">Adja hozzá a következőket a **config.js** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="8c18c-177">Add the following to **config.js**.</span></span> <span data-ttu-id="8c18c-178">Ez meghatározza az alkalmazáshoz szükséges konfigurációs beállításokat és értékeket.</span><span class="sxs-lookup"><span data-stu-id="8c18c-178">This defines configuration settings and values needed for our application.</span></span>
   
        var config = {}
   
        config.host = process.env.HOST || "[the URI value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.authKey = process.env.AUTH_KEY || "[the PRIMARY KEY value from the Azure Cosmos DB Keys blade on http://portal.azure.com]";
        config.databaseId = "ToDoList";
        config.collectionId = "Items";
   
        module.exports = config;
3. <span data-ttu-id="8c18c-179">A **config.js** fájlban frissítse a HOST és az AUTH_KEY értékeket azokkal az értékekkel, amelyeket a [Microsoft Azure Portalon](https://portal.azure.com) lévő Azure Cosmos DB-fiókjának Kulcsok panelén talál.</span><span class="sxs-lookup"><span data-stu-id="8c18c-179">In the **config.js** file, update the values of HOST and AUTH_KEY using the values found in the Keys blade of your Azure Cosmos DB account on the [Microsoft Azure portal](https://portal.azure.com).</span></span>
4. <span data-ttu-id="8c18c-180">Mentse és zárja be a **config.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-180">Save and close the **config.js** file.</span></span>

### <a name="modify-appjs"></a><span data-ttu-id="8c18c-181">Az app.js fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="8c18c-181">Modify app.js</span></span>
1. <span data-ttu-id="8c18c-182">A projekt könyvtárában nyissa meg az **app.js** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-182">In the project directory, open the **app.js** file.</span></span> <span data-ttu-id="8c18c-183">Ez a fájl korábban, az Express-webalkalmazás létrehozásakor jött létre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-183">This file was created earlier when the Express web application was created.</span></span>
2. <span data-ttu-id="8c18c-184">Adja hozzá a következő kódot az **app.js** fájl elejéhez</span><span class="sxs-lookup"><span data-stu-id="8c18c-184">Add the following code to the top of **app.js**</span></span>
   
        var DocumentDBClient = require('documentdb').DocumentClient;
        var config = require('./config');
        var TaskList = require('./routes/tasklist');
        var TaskDao = require('./models/taskDao');
3. <span data-ttu-id="8c18c-185">Ez a kód fogja meghatározni a használni kívánt konfigurációs fájlt, és kiolvasni belőle az értékeket néhány változóhoz, amelyekre hamarosan szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="8c18c-185">This code defines the config file to be used, and proceeds to read values out of this file into some variables we will use soon.</span></span>
4. <span data-ttu-id="8c18c-186">Cserélje ki az **app.js** fájl alábbi két sorát:</span><span class="sxs-lookup"><span data-stu-id="8c18c-186">Replace the following two lines in **app.js** file:</span></span>
   
        app.use('/', index);
        app.use('/users', users); 
   
      <span data-ttu-id="8c18c-187">a következő kódtöredékre:</span><span class="sxs-lookup"><span data-stu-id="8c18c-187">with the following snippet:</span></span>
   
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
5. <span data-ttu-id="8c18c-188">Ezek a sorok meghatározzák a **TaskDao** objektum egy új példányát, amely egy új (a **config.js** fájlból kiolvasott értékek felhasználásával létesített) kapcsolattal csatlakozik az Azure Cosmos DB-adatbázishoz. Továbbá ezek inicializálják a feladatobjektumot, majd társítanak űrlapműveleteket a metódusokhoz a **TaskList**-vezérlőn.</span><span class="sxs-lookup"><span data-stu-id="8c18c-188">These lines define a new instance of our **TaskDao** object, with a new connection to Azure Cosmos DB (using the values read from the **config.js**), initialize the task object and then bind form actions to methods on our **TaskList** controller.</span></span> 
6. <span data-ttu-id="8c18c-189">Végül mentse és zárja be az **app.js** fájlt. És már majdnem készen is van.</span><span class="sxs-lookup"><span data-stu-id="8c18c-189">Finally, save and close the **app.js** file, we're just about done.</span></span>

## <span data-ttu-id="8c18c-190"><a name="_Toc395783181"></a>5. lépés: Felhasználói felület létrehozása</span><span class="sxs-lookup"><span data-stu-id="8c18c-190"><a name="_Toc395783181"></a>Step 5: Build a user interface</span></span>
<span data-ttu-id="8c18c-191">Most térjünk át a felhasználói felület létrehozására, hogy a felhasználók ténylegesen használatba vehessék az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8c18c-191">Now let’s turn our attention to building the user interface so a user can actually interact with our application.</span></span> <span data-ttu-id="8c18c-192">A létrehozott Express-alkalmazás a **Jade** megjelenítési motort használja.</span><span class="sxs-lookup"><span data-stu-id="8c18c-192">The Express application we created uses **Jade** as the view engine.</span></span> <span data-ttu-id="8c18c-193">A Jade motorral kapcsolatos további információkért lásd: [http://jade-lang.com/](http://jade-lang.com/).</span><span class="sxs-lookup"><span data-stu-id="8c18c-193">For more information on Jade please refer to [http://jade-lang.com/](http://jade-lang.com/).</span></span>

1. <span data-ttu-id="8c18c-194">A rendszer a **views** (nézetek) könyvtárban található **layout.jade** fájlt használja a többi **.jade** fájl globális sablonjaként.</span><span class="sxs-lookup"><span data-stu-id="8c18c-194">The **layout.jade** file in the **views** directory is used as a global template for other **.jade** files.</span></span> <span data-ttu-id="8c18c-195">Ebben a lépésben ezt a sablont a [Twitter Bootstrap](https://github.com/twbs/bootstrap) eszközkészletre módosítja majd, amellyel könnyen tervezhet tetszetős webhelyeket.</span><span class="sxs-lookup"><span data-stu-id="8c18c-195">In this step you will modify it to use [Twitter Bootstrap](https://github.com/twbs/bootstrap), which is a toolkit that makes it easy to design a nice looking website.</span></span> 
2. <span data-ttu-id="8c18c-196">Nyissa meg a **views** (nézetek) mappában található **layout.jade** fájlt, és cserélje ki annak tartalmát a következőre:</span><span class="sxs-lookup"><span data-stu-id="8c18c-196">Open the **layout.jade** file found in the **views** folder and replace the contents with the following:</span></span>

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

    <span data-ttu-id="8c18c-197">Ez gyakorlatilag megmondja a **Jade** motornak, hogy rendereljen HTML-kódot az alkalmazás számára, és létrehoz egy **content** (tartalom) nevű **blokkot**, ahol megadhatja a tartalomoldalak elrendezését.</span><span class="sxs-lookup"><span data-stu-id="8c18c-197">This effectively tells the **Jade** engine to render some HTML for our application and creates a **block** called **content** where we can supply the layout for our content pages.</span></span>

    <span data-ttu-id="8c18c-198">Mentse és zárja be a **layout.jade** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-198">Save and close this **layout.jade** file.</span></span>

3. <span data-ttu-id="8c18c-199">Most nyissa meg az **index.jade** fájlt, az alkalmazás által használt nézetet, és cserélje ki a fájl tartalmát az alábbira:</span><span class="sxs-lookup"><span data-stu-id="8c18c-199">Now open the **index.jade** file, the view that will be used by our application, and replace the content of the file with the following:</span></span>
   
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
   

<span data-ttu-id="8c18c-200">Ez kibővíti az elrendezést, és tartalmat biztosít a **layout.jade** fájlban az imént látott **content** (tartalom) helyőrző számára.</span><span class="sxs-lookup"><span data-stu-id="8c18c-200">This extends layout, and provides content for the **content** placeholder we saw in the **layout.jade** file earlier.</span></span>
   
<span data-ttu-id="8c18c-201">Ebben az elrendezésben két HTML-űrlapot hoztunk létre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-201">In this layout we created two HTML forms.</span></span>

<span data-ttu-id="8c18c-202">Az első űrlap az adatok táblázatát, valamint egy gombot tartalmaz, amely lehetővé teszi az elemek frissítését úgy, hogy elküldi azokat a vezérlő **/completetask** metódusának.</span><span class="sxs-lookup"><span data-stu-id="8c18c-202">The first form contains a table for our data and a button that allows us to update items by posting to **/completetask** method of our controller.</span></span>
    
<span data-ttu-id="8c18c-203">A második űrlap két beviteli mezőt és egy gombot tartalmaz, amely lehetővé teszi új elemek létrehozását úgy, hogy elküldi azokat a vezérlő **/addtask** metódusának.</span><span class="sxs-lookup"><span data-stu-id="8c18c-203">The second form contains two input fields and a button that allows us to create a new item by posting to **/addtask** method of our controller.</span></span>

<span data-ttu-id="8c18c-204">Az alkalmazás működéséhez csak ennyire van szükség.</span><span class="sxs-lookup"><span data-stu-id="8c18c-204">This should be all that we need for our application to work.</span></span>

## <span data-ttu-id="8c18c-205"><a name="_Toc395783181"></a>6. lépés: Az alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="8c18c-205"><a name="_Toc395783181"></a>Step 6: Run your application locally</span></span>
1. <span data-ttu-id="8c18c-206">Ha a helyi gépén szeretné tesztelni az alkalmazást, futtassa az `npm start` parancsot a terminálon az alkalmazás elindításához, majd frissítse a [http://localhost:3000](http://localhost:3000) böngészőoldalt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-206">To test the application on your local machine, run `npm start` in the terminal to start your application, then refresh your [http://localhost:3000](http://localhost:3000) browser page.</span></span> <span data-ttu-id="8c18c-207">Az oldalnak most úgy kell kinéznie, ahogy az alábbi képen látható:</span><span class="sxs-lookup"><span data-stu-id="8c18c-207">The page should now look like the image below:</span></span>
   
    ![Képernyőfelvétel a My Todo List (Saját teendőlista) alkalmazásról egy böngészőablakban](./media/documentdb-nodejs-application/cosmos-db-node-js-localhost.png)

    > [!TIP]
    > <span data-ttu-id="8c18c-209">Ha olyan hibaüzenetet kap, amely a layout.jade fájlban vagy az index.jade fájlban lévő behúzásra vonatkozik, győződjön meg arról, hogy az első két sor mindkét fájlban balra zárt, és nem tartalmaz szóközt.</span><span class="sxs-lookup"><span data-stu-id="8c18c-209">If you receive an error about the indent in the layout.jade file or the index.jade file, ensure that the first two lines in both files is left justified, with no spaces.</span></span> <span data-ttu-id="8c18c-210">Ha szóközök kerültek az első két sor elé, távolítsa el őket, mentse mindkét fájlt, és frissítse a böngészőablakot.</span><span class="sxs-lookup"><span data-stu-id="8c18c-210">If there are spaces before the first two lines, remove them, save both files, then refresh your browser window.</span></span> 

2. <span data-ttu-id="8c18c-211">Adjon meg egy új feladatot az Item (Elem), az Item Name (Elem neve) és a Category (Kategória) mezőkben, majd kattintson az **Add Item** (Elem hozzáadása) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-211">Use the Item, Item Name and Category fields to enter a new task and then click **Add Item**.</span></span> <span data-ttu-id="8c18c-212">Ez egy új dokumentumot hoz létre az Azure Cosmos DB-ben a megadott tulajdonságokkal.</span><span class="sxs-lookup"><span data-stu-id="8c18c-212">This creates a document in Azure Cosmos DB with those properties.</span></span> 
3. <span data-ttu-id="8c18c-213">Az oldal ekkor frissül, és megjeleníti az újonnan létrehozott elemet a teendőlistában.</span><span class="sxs-lookup"><span data-stu-id="8c18c-213">The page should update to display the newly created item in the ToDo list.</span></span>
   
    ![Képernyőfelvétel az alkalmazásról és a teendőlista új eleméről](./media/documentdb-nodejs-application/cosmos-db-node-js-added-task.png)
4. <span data-ttu-id="8c18c-215">A feladatok elvégzéséhez egyszerűen jelölje be a jelölőnégyzetet a Complete (Elvégezve) oszlopban, majd kattintson az **Update tasks** (Feladatok frissítése) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-215">To complete a task, simply check the checkbox in the Complete column, and then click **Update tasks**.</span></span> <span data-ttu-id="8c18c-216">Ez frissíti a már létrehozott dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="8c18c-216">This updates the document you already created.</span></span>

5. <span data-ttu-id="8c18c-217">Az alkalmazás leállításához nyomja le a CTRL+C billentyűkombinációt a terminálablakban, majd a kötegelt feladat leállításához kattintson az **Y** elemre.</span><span class="sxs-lookup"><span data-stu-id="8c18c-217">To stop the application, press CTRL+C in the terminal window and then click **Y** to terminate the batch job.</span></span>

## <span data-ttu-id="8c18c-218"><a name="_Toc395783182"></a>7. lépés: Az alkalmazásfejlesztési projekt üzembe helyezése az Azure Websites-ban</span><span class="sxs-lookup"><span data-stu-id="8c18c-218"><a name="_Toc395783182"></a>Step 7: Deploy your application development project to Azure Websites</span></span>
1. <span data-ttu-id="8c18c-219">Ha még nem tette meg, engedélyezzen egy Git-tárházat az Azure Websites számára.</span><span class="sxs-lookup"><span data-stu-id="8c18c-219">If you haven't already, enable a git repository for your Azure Website.</span></span> <span data-ttu-id="8c18c-220">Ehhez a következő témakörben találhat útmutatót: [Local Git Deployment to Azure App Service](../app-service-web/app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="8c18c-220">You can find instructions on how to do this in the [Local Git Deployment to Azure App Service](../app-service-web/app-service-deploy-local-git.md) topic.</span></span>
2. <span data-ttu-id="8c18c-221">Adja hozzá Azure-webhelyét távoli Git-elemként.</span><span class="sxs-lookup"><span data-stu-id="8c18c-221">Add your Azure Website as a git remote.</span></span>
   
        git remote add azure https://username@your-azure-website.scm.azurewebsites.net:443/your-azure-website.git
3. <span data-ttu-id="8c18c-222">Helyezze üzembe a tárházat a távoli mappához küldéssel.</span><span class="sxs-lookup"><span data-stu-id="8c18c-222">Deploy by pushing to the remote.</span></span>
   
        git push azure master
4. <span data-ttu-id="8c18c-223">Néhány másodpercen belül git befejezi a webalkalmazás közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!</span><span class="sxs-lookup"><span data-stu-id="8c18c-223">In a few seconds, git will finish publishing your web application and launch a browser where you can see your handiwork running in Azure!</span></span>

    <span data-ttu-id="8c18c-224">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="8c18c-224">Congratulations!</span></span> <span data-ttu-id="8c18c-225">Létrehozta az első Node.js Express-webalkalmazását az Azure Cosmos DB használatával, és közzétette azt az Azure Websitesban.</span><span class="sxs-lookup"><span data-stu-id="8c18c-225">You have just built your first Node.js Express Web Application using Azure Cosmos DB and published it to Azure Websites.</span></span>

    <span data-ttu-id="8c18c-226">Az oktatóanyaghoz a teljes referenciaalkalmazás letölthető a [GitHubról][GitHub].</span><span class="sxs-lookup"><span data-stu-id="8c18c-226">If you want to download or refer to the complete reference application for this tutorial, it can be downloaded from [GitHub][GitHub].</span></span>

## <span data-ttu-id="8c18c-227"><a name="_Toc395637775"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c18c-227"><a name="_Toc395637775"></a>Next steps</span></span>

* <span data-ttu-id="8c18c-228">Méret- és teljesítménytesztelést szeretne végezni az Azure Cosmos DB használatával?</span><span class="sxs-lookup"><span data-stu-id="8c18c-228">Want to perform scale and performance testing with Azure Cosmos DB?</span></span> <span data-ttu-id="8c18c-229">Tekintse meg a következőt: [Teljesítmény- és mérettesztelés az Azure Cosmos DB használatával](performance-testing.md)</span><span class="sxs-lookup"><span data-stu-id="8c18c-229">See [Performance and Scale Testing with Azure Cosmos DB](performance-testing.md)</span></span>
* <span data-ttu-id="8c18c-230">Ismerje meg, hogyan [figyelhet egy Azure Cosmos DB-fiókot](monitor-accounts.md).</span><span class="sxs-lookup"><span data-stu-id="8c18c-230">Learn how to [monitor an Azure Cosmos DB account](monitor-accounts.md).</span></span>
* <span data-ttu-id="8c18c-231">Futtasson lekérdezéseket a minta-adatkészleteken a [Query Playground](https://www.documentdb.com/sql/demo) (Tesztlekérdezések) használatával.</span><span class="sxs-lookup"><span data-stu-id="8c18c-231">Run queries against our sample dataset in the [Query Playground](https://www.documentdb.com/sql/demo).</span></span>
* <span data-ttu-id="8c18c-232">Tekintse át az [Azure Cosmos DB-dokumentációt](https://docs.microsoft.com/azure/documentdb/).</span><span class="sxs-lookup"><span data-stu-id="8c18c-232">Explore the [Azure Cosmos DB documentation](https://docs.microsoft.com/azure/documentdb/).</span></span>

[Node.js]: http://nodejs.org/
[Git]: http://git-scm.com/
[GitHub]: https://github.com/Azure-Samples/documentdb-node-todo-app

