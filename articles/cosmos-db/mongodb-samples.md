---
title: "MongoDB API-k segítségével az Azure Cosmos DB alkalmazás létrehozása |} Microsoft Docs"
description: "Ez az oktatóanyag az Azure Cosmos DB API-k használatával mongodb egy online adatbázist hoz létre."
keywords: "mongodb-példák"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: fb38bc53-3561-487d-9e03-20f232319a87
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: anhoh
ms.openlocfilehash: 433d2e585c884a10e7e923a0b27c179a95410d01
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="build-an-azure-cosmos-db-api-for-mongodb-app-using-nodejs"></a><span data-ttu-id="77bc4-104">Egy Azure Cosmos DB létrehozása: API-t a MongoDB-alkalmazásokhoz Node.js használatával</span><span class="sxs-lookup"><span data-stu-id="77bc4-104">Build an Azure Cosmos DB: API for MongoDB app using Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="77bc4-105">.NET</span><span class="sxs-lookup"><span data-stu-id="77bc4-105">.NET</span></span>](documentdb-get-started.md)
> * [<span data-ttu-id="77bc4-106">.NET Core</span><span class="sxs-lookup"><span data-stu-id="77bc4-106">.NET Core</span></span>](documentdb-dotnetcore-get-started.md)
> * [<span data-ttu-id="77bc4-107">Java</span><span class="sxs-lookup"><span data-stu-id="77bc4-107">Java</span></span>](documentdb-java-get-started.md)
> * [<span data-ttu-id="77bc4-108">Node.js MongoDB-hez</span><span class="sxs-lookup"><span data-stu-id="77bc4-108">Node.js for MongoDB</span></span>](mongodb-samples.md)
> * [<span data-ttu-id="77bc4-109">Node.js</span><span class="sxs-lookup"><span data-stu-id="77bc4-109">Node.js</span></span>](documentdb-nodejs-get-started.md)
> * [<span data-ttu-id="77bc4-110">C++</span><span class="sxs-lookup"><span data-stu-id="77bc4-110">C++</span></span>](documentdb-cpp-get-started.md)
>  
>

<span data-ttu-id="77bc4-111">Ez a példa bemutatja, hogyan hozhat létre egy Azure Cosmos DB: API-t a MongoDB-Konzolalkalmazás Node.js segítségével.</span><span class="sxs-lookup"><span data-stu-id="77bc4-111">This example shows you how to build an Azure Cosmos DB: API for MongoDB console app using Node.js.</span></span>

<span data-ttu-id="77bc4-112">Ebben a példában használatához az alábbiak szükségesek:</span><span class="sxs-lookup"><span data-stu-id="77bc4-112">To use this example, you must:</span></span>

* <span data-ttu-id="77bc4-113">[Hozzon létre](create-mongodb-dotnet.md#create-account) egy Azure Cosmos DB: API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="77bc4-113">[Create](create-mongodb-dotnet.md#create-account) an Azure Cosmos DB: API for MongoDB account.</span></span>
* <span data-ttu-id="77bc4-114">A MongoDB beolvasása [kapcsolati karakterlánc](connect-mongodb-account.md) információkat.</span><span class="sxs-lookup"><span data-stu-id="77bc4-114">Retrieve your MongoDB [connection string](connect-mongodb-account.md) information.</span></span>

## <a name="create-the-app"></a><span data-ttu-id="77bc4-115">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="77bc4-115">Create the app</span></span>

1. <span data-ttu-id="77bc4-116">Hozzon létre egy *app.js* fájl és másolja és illessze be az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="77bc4-116">Create a *app.js* file and copy & paste the code below.</span></span>

    ```nodejs
    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var ObjectId = require('mongodb').ObjectID;
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';

    var insertDocument = function(db, callback) {
    db.collection('families').insertOne( {
            "id": "AndersenFamily",
            "lastName": "Andersen",
            "parents": [
                { "firstName": "Thomas" },
                { "firstName": "Mary Kay" }
            ],
            "children": [
                { "firstName": "John", "gender": "male", "grade": 7 }
            ],
            "pets": [
                { "givenName": "Fluffy" }
            ],
            "address": { "country": "USA", "state": "WA", "city": "Seattle" }
        }, function(err, result) {
        assert.equal(err, null);
        console.log("Inserted a document into the families collection.");
        callback();
    });
    };
    
    var findFamilies = function(db, callback) {
    var cursor =db.collection('families').find( );
    cursor.each(function(err, doc) {
        assert.equal(err, null);
        if (doc != null) {
            console.dir(doc);
        } else {
            callback();
        }
    });
    };
    
    var updateFamilies = function(db, callback) {
    db.collection('families').updateOne(
        { "lastName" : "Andersen" },
        {
            $set: { "pets": [
                { "givenName": "Fluffy" },
                { "givenName": "Rocky"}
            ] },
            $currentDate: { "lastModified": true }
        }, function(err, results) {
        console.log(results);
        callback();
    });
    };
    
    var removeFamilies = function(db, callback) {
    db.collection('families').deleteMany(
        { "lastName": "Andersen" },
        function(err, results) {
            console.log(results);
            callback();
        }
    );
    };
    
    MongoClient.connect(url, function(err, db) {
    assert.equal(null, err);
    insertDocument(db, function() {
        findFamilies(db, function() {
        updateFamilies(db, function() {
            removeFamilies(db, function() {
                db.close();
            });
        });
        });
    });
    });
    ```

2. <span data-ttu-id="77bc4-117">A következő változók módosítása a *app.js* fiókbeállításokban / fájl (keresése a [kapcsolati karakterlánc](connect-mongodb-account.md)):</span><span class="sxs-lookup"><span data-stu-id="77bc4-117">Modify the following variables in the *app.js* file per your account settings (Learn how to find your [connection string](connect-mongodb-account.md)):</span></span>
   
    ```nodejs
    var url = 'mongodb://<endpoint>:<password>@<endpoint>.documents.azure.com:10255/?ssl=true';
    ```
     
3. <span data-ttu-id="77bc4-118">Nyissa meg kedvenc terminálját, és futtassa **npm mongodb--telepítése Mentés**, majd futtassa az alkalmazás a **csomópont app.js**</span><span class="sxs-lookup"><span data-stu-id="77bc4-118">Open your favorite terminal, run **npm install mongodb --save**, then run your app with **node app.js**</span></span>

## <a name="next-steps"></a><span data-ttu-id="77bc4-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="77bc4-119">Next steps</span></span>
* <span data-ttu-id="77bc4-120">Megtudhatja, hogyan [MongoChef használja](mongodb-mongochef.md) együtt az Azure Cosmos DB: API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="77bc4-120">Learn how to [use MongoChef](mongodb-mongochef.md) with your Azure Cosmos DB: API for MongoDB account.</span></span>
