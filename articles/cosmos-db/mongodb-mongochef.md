---
title: az Azure Cosmos DB MongoChef aaaUse |} Microsoft Docs
description: "Megtudhatja, hogyan toouse rendelkező egy Azure Cosmos DB MongoChef: API-t a MongoDB-fiók"
keywords: mongochef
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: anhoh
ms.openlocfilehash: 4b047797b231c34ccc6f2ed02416525c6228d596
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="c4cda-104">Egy Azure Cosmos DB MongoChef használni: API-t a MongoDB-fiók</span><span class="sxs-lookup"><span data-stu-id="c4cda-104">Use MongoChef with an Azure Cosmos DB: API for MongoDB account</span></span>

<span data-ttu-id="c4cda-105">tooconnect tooan Azure Cosmos DB: API-t a MongoDB-fiókot kell:</span><span class="sxs-lookup"><span data-stu-id="c4cda-105">tooconnect tooan Azure Cosmos DB: API for MongoDB account, you must:</span></span>

* <span data-ttu-id="c4cda-106">Töltse le és telepítse [MongoChef](http://3t.io/mongochef)</span><span class="sxs-lookup"><span data-stu-id="c4cda-106">Download and install [MongoChef](http://3t.io/mongochef)</span></span>
* <span data-ttu-id="c4cda-107">Az Azure Cosmos DB rendelkezik: API-t a MongoDB-fiók [kapcsolati karakterlánc](connect-mongodb-account.md) információk</span><span class="sxs-lookup"><span data-stu-id="c4cda-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="create-hello-connection-in-mongochef"></a><span data-ttu-id="c4cda-108">MongoChef hello-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="c4cda-108">Create hello connection in MongoChef</span></span>
<span data-ttu-id="c4cda-109">tooadd az Azure Cosmos DB: API a MongoDB fiók toohello MongoChef Csatlakozáskezelő, hajtsa végre az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="c4cda-109">tooadd your Azure Cosmos DB: API for MongoDB account toohello MongoChef connection manager, perform hello following steps.</span></span>

1. <span data-ttu-id="c4cda-110">Az Azure Cosmos DB beolvasása: API-t a MongoDB kapcsolatadatok hello utasításokat követve [Itt](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="c4cda-110">Retrieve your Azure Cosmos DB: API for MongoDB connection information using hello instructions [here](connect-mongodb-account.md).</span></span>

    ![Képernyőfelvétel a hello kapcsolati karakterlánc panel](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. <span data-ttu-id="c4cda-112">Kattintson a **Connect** tooopen hello Csatlakozáskezelő, majd kattintson az **új kapcsolat**</span><span class="sxs-lookup"><span data-stu-id="c4cda-112">Click **Connect** tooopen hello Connection Manager, then click **New Connection**</span></span>

    ![Képernyőfelvétel a hello MongoChef Csatlakozáskezelő](./media/mongodb-mongochef/ConnectionManager.png)
3. <span data-ttu-id="c4cda-114">A hello **új kapcsolat** ablakban a hello **Server** lapra, adja meg a GAZDAGÉP (FQDN) hello Azure Cosmos DB hello: API-t a MongoDB-fiókot és hello PORT.</span><span class="sxs-lookup"><span data-stu-id="c4cda-114">In hello **New Connection** window, on hello **Server** tab, enter hello HOST (FQDN) of hello Azure Cosmos DB: API for MongoDB account and hello PORT.</span></span>

    ![Hello MongoChef kapcsolatot kezelő kiszolgálóhoz lap](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. <span data-ttu-id="c4cda-116">A hello **új kapcsolat** ablakban a hello **hitelesítési** lapra, majd a hitelesítési mód **szabvány (MONGODB-CR vagy SCARM-SHA-1)** , és írja be a FELHASZNÁLÓNEVET hello és A JELSZÓ.</span><span class="sxs-lookup"><span data-stu-id="c4cda-116">In hello **New Connection** window, on hello **Authentication** tab, choose Authentication Mode **Standard (MONGODB-CR or SCARM-SHA-1)** and enter hello USERNAME and PASSWORD.</span></span>  <span data-ttu-id="c4cda-117">Fogadja el a hello alapértelmezett hitelesítési db (rendszergazda), vagy a saját értéket adjon meg.</span><span class="sxs-lookup"><span data-stu-id="c4cda-117">Accept hello default authentication db (admin) or provide your own value.</span></span>

    ![Képernyőfelvétel a hello MongoChef connection manager hitelesítés lap](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. <span data-ttu-id="c4cda-119">A hello **új kapcsolat** ablakban a hello **SSL** fület, ellenőrizze a hello **használja az SSL protokoll tooconnect** jelölőnégyzetet és hello **elfogadás kiszolgáló önaláírt SSL tanúsítványok** választógombot.</span><span class="sxs-lookup"><span data-stu-id="c4cda-119">In hello **New Connection** window, on hello **SSL** tab, check hello **Use SSL protocol tooconnect** check box and hello **Accept server self-signed SSL certificates** radio button.</span></span>

    ![Képernyőfelvétel a hello MongoChef connection manager SSL lap](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. <span data-ttu-id="c4cda-121">Hello kattintson **kapcsolat tesztelése** toovalidate hello kapcsolatadatok gombra, majd **OK** tooreturn toohello új kapcsolat ablakot, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-121">Click hello **Test Connection** button toovalidate hello connection information, click **OK** tooreturn toohello New Connection window, and then click **Save**.</span></span>

    ![Képernyőfelvétel a hello MongoChef teszt kapcsolat ablak](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-toocreate-a-database-collection-and-documents"></a><span data-ttu-id="c4cda-123">MongoChef toocreate adatbázis, gyűjtemény és dokumentumok</span><span class="sxs-lookup"><span data-stu-id="c4cda-123">Use MongoChef toocreate a database, collection, and documents</span></span>
<span data-ttu-id="c4cda-124">toocreate adatbázis, gyűjtemény és dokumentumok MongoChef, hajtsa végre a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="c4cda-124">toocreate a database, collection, and documents using MongoChef, perform hello following steps.</span></span>

1. <span data-ttu-id="c4cda-125">A **Csatlakozáskezelő**, jelölje ki a hello kapcsolatot, majd kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-125">In **Connection Manager**, highlight hello connection and click **Connect**.</span></span>

    ![Képernyőfelvétel a hello MongoChef Csatlakozáskezelő](./media/mongodb-mongochef/ConnectToAccount.png)
2. <span data-ttu-id="c4cda-127">Hello állomás kattintson jobb gombbal, majd válassza a **adatbázis hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-127">Right click hello host and choose **Add Database**.</span></span>  <span data-ttu-id="c4cda-128">Adja meg az adatbázis nevét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-128">Provide a database name and click **OK**.</span></span>

    ![Képernyőfelvétel a hello MongoChef hozzáadása adatbázis-beállítás](./media/mongodb-mongochef/AddDatabase1.png)
3. <span data-ttu-id="c4cda-130">Hello adatbázis kattintson jobb gombbal, majd válassza a **gyűjtemény hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-130">Right click hello database and choose **Add Collection**.</span></span>  <span data-ttu-id="c4cda-131">Adjon meg egy gyűjtemény nevét, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-131">Provide a collection name and click **Create**.</span></span>

    ![Képernyőfelvétel a hello MongoChef gyűjtemény hozzáadása lehetőséget.](./media/mongodb-mongochef/AddCollection.png)
4. <span data-ttu-id="c4cda-133">Kattintson a hello **gyűjtemény** menü elemet, majd kattintson a **hozzáadása a dokumentumhoz**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-133">Click hello **Collection** menu item, then click **Add Document**.</span></span>

    ![Képernyőfelvétel a hello MongoChef hozzáadása a dokumentumhoz menüpont](./media/mongodb-mongochef/AddDocument1.png)
5. <span data-ttu-id="c4cda-135">Illessze be hello következő hello dokumentum felvétele párbeszédpanel, és kattintson a **hozzáadása a dokumentumhoz**.</span><span class="sxs-lookup"><span data-stu-id="c4cda-135">In hello Add Document dialog, paste hello following and then click **Add Document**.</span></span>

        {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
               { "firstName": "Thomas" },
               { "firstName": "Mary Kay"}
        ],
        "children": [
           {
               "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
               "pets": [{ "givenName": "Fluffy" }]
           }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
        }
6. <span data-ttu-id="c4cda-136">Egy másik dokumentum, a tartalom a következő hello most hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="c4cda-136">Add another document, this time with hello following content.</span></span>

        {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                 "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                 "givenName": "Lisa",
                 "gender": "female",
                 "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
        }
7. <span data-ttu-id="c4cda-137">A minta lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="c4cda-137">Execute a sample query.</span></span> <span data-ttu-id="c4cda-138">Például keresse meg és hello vezetéknevét "Andersen" és a visszatérési hello szülők és az állapot mező családok.</span><span class="sxs-lookup"><span data-stu-id="c4cda-138">For example, search for families with hello last name 'Andersen' and return hello parents and state fields.</span></span>

    ![Képernyőfelvétel a Mongo Chef lekérdezés eredményei](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a><span data-ttu-id="c4cda-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4cda-140">Next steps</span></span>
* <span data-ttu-id="c4cda-141">Ismerheti meg az Azure Cosmos DB: Mongodb-protokolltámogatással API [minták](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c4cda-141">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
