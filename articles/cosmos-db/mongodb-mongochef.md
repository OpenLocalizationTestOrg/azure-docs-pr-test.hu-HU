---
title: "MongoChef használata Azure Cosmos DB |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Cosmos DB MongoChef: API-t a MongoDB-fiók"
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
ms.openlocfilehash: 54c9799bd646b827f602e2ea2f9a15a4fc853f00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="680a1-104">Egy Azure Cosmos DB MongoChef használni: API-t a MongoDB-fiók</span><span class="sxs-lookup"><span data-stu-id="680a1-104">Use MongoChef with an Azure Cosmos DB: API for MongoDB account</span></span>

<span data-ttu-id="680a1-105">Egy Azure Cosmos DB való kapcsolódáshoz: API-t a MongoDB-fiókot kell:</span><span class="sxs-lookup"><span data-stu-id="680a1-105">To connect to an Azure Cosmos DB: API for MongoDB account, you must:</span></span>

* <span data-ttu-id="680a1-106">Töltse le és telepítse [MongoChef](http://3t.io/mongochef)</span><span class="sxs-lookup"><span data-stu-id="680a1-106">Download and install [MongoChef](http://3t.io/mongochef)</span></span>
* <span data-ttu-id="680a1-107">Az Azure Cosmos DB rendelkezik: API-t a MongoDB-fiók [kapcsolati karakterlánc](connect-mongodb-account.md) információk</span><span class="sxs-lookup"><span data-stu-id="680a1-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="create-the-connection-in-mongochef"></a><span data-ttu-id="680a1-108">Hozzon létre a kapcsolat MongoChef</span><span class="sxs-lookup"><span data-stu-id="680a1-108">Create the connection in MongoChef</span></span>
<span data-ttu-id="680a1-109">Az Azure Cosmos DB hozzáadása: MongoChef Csatlakozáskezelő fiók MongoDB API a következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="680a1-109">To add your Azure Cosmos DB: API for MongoDB account to the MongoChef connection manager, perform the following steps.</span></span>

1. <span data-ttu-id="680a1-110">Az Azure Cosmos DB beolvasása: API-t a MongoDB kapcsolatadatok utasításai [Itt](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="680a1-110">Retrieve your Azure Cosmos DB: API for MongoDB connection information using the instructions [here](connect-mongodb-account.md).</span></span>

    ![A kapcsolati karakterlánc panel képernyőfelvétele](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. <span data-ttu-id="680a1-112">Kattintson a **Connect** a Csatlakozáskezelő megnyitásához kattintson a **új kapcsolat**</span><span class="sxs-lookup"><span data-stu-id="680a1-112">Click **Connect** to open the Connection Manager, then click **New Connection**</span></span>

    ![Képernyőfelvétel a MongoChef Csatlakozáskezelő](./media/mongodb-mongochef/ConnectionManager.png)
3. <span data-ttu-id="680a1-114">Az a **új kapcsolat** ablakban, a a **Server** lapra, adja meg a GAZDAGÉP (FQDN) az Azure Cosmos DB: API-t a MongoDB-fiók és a PORT.</span><span class="sxs-lookup"><span data-stu-id="680a1-114">In the **New Connection** window, on the **Server** tab, enter the HOST (FQDN) of the Azure Cosmos DB: API for MongoDB account and the PORT.</span></span>

    ![A MongoChef kapcsolatot kezelő kiszolgálóhoz lap](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. <span data-ttu-id="680a1-116">Az a **új kapcsolat** ablakban, a a **hitelesítési** lapra, majd a hitelesítési mód **szabvány (MONGODB-CR vagy SCARM-SHA-1)** , és írja be a FELHASZNÁLÓNEVET és JELSZÓT.</span><span class="sxs-lookup"><span data-stu-id="680a1-116">In the **New Connection** window, on the **Authentication** tab, choose Authentication Mode **Standard (MONGODB-CR or SCARM-SHA-1)** and enter the USERNAME and PASSWORD.</span></span>  <span data-ttu-id="680a1-117">Fogadja el az alapértelmezett hitelesítési db (rendszergazda), vagy a saját értéket adjon meg.</span><span class="sxs-lookup"><span data-stu-id="680a1-117">Accept the default authentication db (admin) or provide your own value.</span></span>

    ![A MongoChef kapcsolat manager hitelesítés lap](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. <span data-ttu-id="680a1-119">Az a **új kapcsolat** ablakban, a a **SSL** lapon jelölje a **való csatlakozáshoz használjon SSL-protokoll** jelölőnégyzetet és a **kiszolgáló önaláírt SSL-tanúsítványokat elfogadó** választógombot.</span><span class="sxs-lookup"><span data-stu-id="680a1-119">In the **New Connection** window, on the **SSL** tab, check the **Use SSL protocol to connect** check box and the **Accept server self-signed SSL certificates** radio button.</span></span>

    ![A MongoChef kapcsolat manager SSL lap](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. <span data-ttu-id="680a1-121">Kattintson a **kapcsolat tesztelése** gombra kattintva ellenőrizze a kapcsolati adatokat, kattintson a **OK** az új kapcsolat ablakba, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="680a1-121">Click the **Test Connection** button to validate the connection information, click **OK** to return to the New Connection window, and then click **Save**.</span></span>

    ![A MongoChef teszt kapcsolat ablak képernyőfelvétele](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-to-create-a-database-collection-and-documents"></a><span data-ttu-id="680a1-123">Egy adatbázis, gyűjtemény és dokumentumok létrehozásához használja a MongoChef</span><span class="sxs-lookup"><span data-stu-id="680a1-123">Use MongoChef to create a database, collection, and documents</span></span>
<span data-ttu-id="680a1-124">Egy adatbázis, gyűjtemény és dokumentumok MongoChef létrehozásához hajtsa végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="680a1-124">To create a database, collection, and documents using MongoChef, perform the following steps.</span></span>

1. <span data-ttu-id="680a1-125">A **Csatlakozáskezelő**, jelölje ki a kapcsolat, majd kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="680a1-125">In **Connection Manager**, highlight the connection and click **Connect**.</span></span>

    ![Képernyőfelvétel a MongoChef Csatlakozáskezelő](./media/mongodb-mongochef/ConnectToAccount.png)
2. <span data-ttu-id="680a1-127">Kattintson jobb gombbal arra a gazdagépre, és válassza a **adatbázis hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="680a1-127">Right click the host and choose **Add Database**.</span></span>  <span data-ttu-id="680a1-128">Adja meg az adatbázis nevét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="680a1-128">Provide a database name and click **OK**.</span></span>

    ![Képernyőfelvétel a MongoChef hozzáadása adatbázis-beállítás](./media/mongodb-mongochef/AddDatabase1.png)
3. <span data-ttu-id="680a1-130">Kattintson a jobb gombbal az adatbázist, és válassza a **gyűjtemény hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="680a1-130">Right click the database and choose **Add Collection**.</span></span>  <span data-ttu-id="680a1-131">Adjon meg egy gyűjtemény nevét, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="680a1-131">Provide a collection name and click **Create**.</span></span>

    ![Képernyőfelvétel a MongoChef gyűjtemény hozzáadása lehetőséget.](./media/mongodb-mongochef/AddCollection.png)
4. <span data-ttu-id="680a1-133">Kattintson a **gyűjtemény** menü elemet, majd kattintson a **hozzáadása a dokumentumhoz**.</span><span class="sxs-lookup"><span data-stu-id="680a1-133">Click the **Collection** menu item, then click **Add Document**.</span></span>

    ![Képernyőfelvétel a MongoChef hozzáadása a dokumentumhoz menüpont](./media/mongodb-mongochef/AddDocument1.png)
5. <span data-ttu-id="680a1-135">A dokumentum felvétele párbeszédpanelen illessze be a következőt, és kattintson a **hozzáadása a dokumentumhoz**.</span><span class="sxs-lookup"><span data-stu-id="680a1-135">In the Add Document dialog, paste the following and then click **Add Document**.</span></span>

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
6. <span data-ttu-id="680a1-136">Adjon hozzá egy másik dokumentum, ezúttal az alábbi tartalommal.</span><span class="sxs-lookup"><span data-stu-id="680a1-136">Add another document, this time with the following content.</span></span>

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
7. <span data-ttu-id="680a1-137">A minta lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="680a1-137">Execute a sample query.</span></span> <span data-ttu-id="680a1-138">Például "Andersen" utolsó nevű családok kereshet, és térjen vissza a szülők és állapot mezőket.</span><span class="sxs-lookup"><span data-stu-id="680a1-138">For example, search for families with the last name 'Andersen' and return the parents and state fields.</span></span>

    ![Képernyőfelvétel a Mongo Chef lekérdezés eredményei](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a><span data-ttu-id="680a1-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="680a1-140">Next steps</span></span>
* <span data-ttu-id="680a1-141">Ismerheti meg az Azure Cosmos DB: Mongodb-protokolltámogatással API [minták](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="680a1-141">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
