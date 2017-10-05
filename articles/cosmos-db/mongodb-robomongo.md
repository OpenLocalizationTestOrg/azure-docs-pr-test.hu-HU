---
title: "Robomongo használata Azure Cosmos DB |} Microsoft Docs"
description: "Ismerje meg, hogyan használható az Azure Cosmos DB Robomongo: API-t a MongoDB-fiók"
keywords: robomongo
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
ms.openlocfilehash: 8983594776a1bbe413a6d7cf2cd518f0e327648a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="c3bf4-104">Egy Azure Cosmos DB Robomongo használni: API-t a MongoDB-fiók</span><span class="sxs-lookup"><span data-stu-id="c3bf4-104">Use Robomongo with an Azure Cosmos DB: API for MongoDB account</span></span>
<span data-ttu-id="c3bf4-105">Egy Azure Cosmos DB való kapcsolódáshoz: API használatával Robomongo MongoDB-fiók, meg kell:</span><span class="sxs-lookup"><span data-stu-id="c3bf4-105">To connect to an Azure Cosmos DB: API for MongoDB account using Robomongo, you must:</span></span>

* <span data-ttu-id="c3bf4-106">Töltse le és telepítse [Robomongo](https://robomongo.org/)</span><span class="sxs-lookup"><span data-stu-id="c3bf4-106">Download and install [Robomongo](https://robomongo.org/)</span></span>
* <span data-ttu-id="c3bf4-107">Az Azure Cosmos DB rendelkezik: API-t a MongoDB-fiók [kapcsolati karakterlánc](connect-mongodb-account.md) információk</span><span class="sxs-lookup"><span data-stu-id="c3bf4-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="connect-using-robomongo"></a><span data-ttu-id="c3bf4-108">Csatlakozás Robomongo használatával</span><span class="sxs-lookup"><span data-stu-id="c3bf4-108">Connect using Robomongo</span></span>
<span data-ttu-id="c3bf4-109">Az Azure Cosmos DB hozzáadása: a Robomongo MongoDB kapcsolatok fiók MongoDB API a következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-109">To add your Azure Cosmos DB: API for MongoDB account to the Robomongo MongoDB Connections, perform the following steps.</span></span>

1. <span data-ttu-id="c3bf4-110">Az Azure Cosmos DB beolvasása: API-t a MongoDB fiók kapcsolat adatait az utasításokat követve [Itt](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="c3bf4-110">Retrieve your Azure Cosmos DB: API for MongoDB account connection information using the instructions [here](connect-mongodb-account.md).</span></span>

    ![A kapcsolati karakterlánc panel képernyőfelvétele](./media/mongodb-robomongo/connectionstringblade.png)
2. <span data-ttu-id="c3bf4-112">Futtatás *Robomongo.exe*</span><span class="sxs-lookup"><span data-stu-id="c3bf4-112">Run *Robomongo.exe*</span></span>

3. <span data-ttu-id="c3bf4-113">A kapcsolat gombra a **fájl** a kapcsolatok kezelésére.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-113">Click the connection button under **File** to manage your connections.</span></span> <span data-ttu-id="c3bf4-114">Kattintson a **létrehozása** a a **MongoDB kapcsolatok** ablakban nyílik meg, a **kapcsolatbeállítások** ablak.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-114">Then, click **Create** in the **MongoDB Connections** window, which will open up the **Connection Settings** window.</span></span>

4. <span data-ttu-id="c3bf4-115">Az a **kapcsolatbeállítások** ablakban válasszon egy nevet.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-115">In the **Connection Settings** window, choose a name.</span></span> <span data-ttu-id="c3bf4-116">Majd keresse meg a **állomás** és **Port** a kapcsolati információit az 1. lépés, és írja be őket **cím** és **Port**, illetve.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-116">Then, find the **Host** and **Port** from your connection information in Step 1 and enter them into **Address** and **Port**, respectively.</span></span>

    ![Képernyőfelvétel a Robomongo kapcsolatok kezelése](./media/mongodb-robomongo/manageconnections.png)
5. <span data-ttu-id="c3bf4-118">Az a **hitelesítési** lapra, majd **végrehajtása hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-118">On the **Authentication** tab, click **Perform authentication**.</span></span> <span data-ttu-id="c3bf4-119">Ezután írja be az adatbázis (alapértelmezett érték *Admin*), **felhasználónév** és **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-119">Then, enter your Database (default is *Admin*), **User Name** and **Password**.</span></span>
<span data-ttu-id="c3bf4-120">Mindkét **felhasználónév** és **jelszó** a kapcsolatadatok, 1. lépésben található.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-120">Both **User Name** and **Password** can be found in your connection information in Step 1.</span></span>

    ![A Robomongo hitelesítés lap](./media/mongodb-robomongo/authentication.png)
6. <span data-ttu-id="c3bf4-122">Az a **SSL** lapon jelölje **használja az SSL protokoll**, majd módosítsa a **hitelesítési módszer** való **önaláírt tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-122">On the **SSL** tab, check **Use SSL protocol**, then change the **Authentication Method** to **Self-signed Certificate**.</span></span>

    ![A Robomongo SSL lap](./media/mongodb-robomongo/SSL.png)
7. <span data-ttu-id="c3bf4-124">Végezetül kattintson **teszt** ellenőrizze, hogy kapcsolódni tud, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c3bf4-124">Finally, click **Test** to verify that you are able to connect, then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c3bf4-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3bf4-125">Next steps</span></span>
* <span data-ttu-id="c3bf4-126">Ismerheti meg az Azure Cosmos DB: Mongodb-protokolltámogatással API [minták](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="c3bf4-126">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
