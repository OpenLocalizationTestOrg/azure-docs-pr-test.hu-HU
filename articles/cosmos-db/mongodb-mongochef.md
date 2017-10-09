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
# <a name="use-mongochef-with-an-azure-cosmos-db-api-for-mongodb-account"></a>Egy Azure Cosmos DB MongoChef használni: API-t a MongoDB-fiók

tooconnect tooan Azure Cosmos DB: API-t a MongoDB-fiókot kell:

* Töltse le és telepítse [MongoChef](http://3t.io/mongochef)
* Az Azure Cosmos DB rendelkezik: API-t a MongoDB-fiók [kapcsolati karakterlánc](connect-mongodb-account.md) információk

## <a name="create-hello-connection-in-mongochef"></a>MongoChef hello-kapcsolat létrehozása
tooadd az Azure Cosmos DB: API a MongoDB fiók toohello MongoChef Csatlakozáskezelő, hajtsa végre az alábbi lépésekkel hello.

1. Az Azure Cosmos DB beolvasása: API-t a MongoDB kapcsolatadatok hello utasításokat követve [Itt](connect-mongodb-account.md).

    ![Képernyőfelvétel a hello kapcsolati karakterlánc panel](./media/mongodb-mongochef/ConnectionStringBlade.png)
2. Kattintson a **Connect** tooopen hello Csatlakozáskezelő, majd kattintson az **új kapcsolat**

    ![Képernyőfelvétel a hello MongoChef Csatlakozáskezelő](./media/mongodb-mongochef/ConnectionManager.png)
3. A hello **új kapcsolat** ablakban a hello **Server** lapra, adja meg a GAZDAGÉP (FQDN) hello Azure Cosmos DB hello: API-t a MongoDB-fiókot és hello PORT.

    ![Hello MongoChef kapcsolatot kezelő kiszolgálóhoz lap](./media/mongodb-mongochef/ConnectionManagerServerTab.png)
4. A hello **új kapcsolat** ablakban a hello **hitelesítési** lapra, majd a hitelesítési mód **szabvány (MONGODB-CR vagy SCARM-SHA-1)** , és írja be a FELHASZNÁLÓNEVET hello és A JELSZÓ.  Fogadja el a hello alapértelmezett hitelesítési db (rendszergazda), vagy a saját értéket adjon meg.

    ![Képernyőfelvétel a hello MongoChef connection manager hitelesítés lap](./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png)
5. A hello **új kapcsolat** ablakban a hello **SSL** fület, ellenőrizze a hello **használja az SSL protokoll tooconnect** jelölőnégyzetet és hello **elfogadás kiszolgáló önaláírt SSL tanúsítványok** választógombot.

    ![Képernyőfelvétel a hello MongoChef connection manager SSL lap](./media/mongodb-mongochef/ConnectionManagerSSLTab.png)
6. Hello kattintson **kapcsolat tesztelése** toovalidate hello kapcsolatadatok gombra, majd **OK** tooreturn toohello új kapcsolat ablakot, és kattintson a **mentése**.

    ![Képernyőfelvétel a hello MongoChef teszt kapcsolat ablak](./media/mongodb-mongochef/TestConnectionResults.png)

## <a name="use-mongochef-toocreate-a-database-collection-and-documents"></a>MongoChef toocreate adatbázis, gyűjtemény és dokumentumok
toocreate adatbázis, gyűjtemény és dokumentumok MongoChef, hajtsa végre a következő lépéseket hello.

1. A **Csatlakozáskezelő**, jelölje ki a hello kapcsolatot, majd kattintson **Connect**.

    ![Képernyőfelvétel a hello MongoChef Csatlakozáskezelő](./media/mongodb-mongochef/ConnectToAccount.png)
2. Hello állomás kattintson jobb gombbal, majd válassza a **adatbázis hozzáadása**.  Adja meg az adatbázis nevét, és kattintson a **OK**.

    ![Képernyőfelvétel a hello MongoChef hozzáadása adatbázis-beállítás](./media/mongodb-mongochef/AddDatabase1.png)
3. Hello adatbázis kattintson jobb gombbal, majd válassza a **gyűjtemény hozzáadása**.  Adjon meg egy gyűjtemény nevét, majd kattintson a **létrehozása**.

    ![Képernyőfelvétel a hello MongoChef gyűjtemény hozzáadása lehetőséget.](./media/mongodb-mongochef/AddCollection.png)
4. Kattintson a hello **gyűjtemény** menü elemet, majd kattintson a **hozzáadása a dokumentumhoz**.

    ![Képernyőfelvétel a hello MongoChef hozzáadása a dokumentumhoz menüpont](./media/mongodb-mongochef/AddDocument1.png)
5. Illessze be hello következő hello dokumentum felvétele párbeszédpanel, és kattintson a **hozzáadása a dokumentumhoz**.

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
6. Egy másik dokumentum, a tartalom a következő hello most hozzáadása.

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
7. A minta lekérdezés végrehajtása. Például keresse meg és hello vezetéknevét "Andersen" és a visszatérési hello szülők és az állapot mező családok.

    ![Képernyőfelvétel a Mongo Chef lekérdezés eredményei](./media/mongodb-mongochef/QueryDocument1.png)

## <a name="next-steps"></a>Következő lépések
* Ismerheti meg az Azure Cosmos DB: Mongodb-protokolltámogatással API [minták](mongodb-samples.md).
