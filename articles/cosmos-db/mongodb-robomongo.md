---
title: az Azure Cosmos DB Robomongo aaaUse |} Microsoft Docs
description: "Megtudhatja, hogyan toouse rendelkező egy Azure Cosmos DB Robomongo: API-t a MongoDB-fiók"
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
ms.openlocfilehash: 3018243e904015426dc69a69b26fb53421e1fe4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-robomongo-with-an-azure-cosmos-db-api-for-mongodb-account"></a><span data-ttu-id="7f208-104">Egy Azure Cosmos DB Robomongo használni: API-t a MongoDB-fiók</span><span class="sxs-lookup"><span data-stu-id="7f208-104">Use Robomongo with an Azure Cosmos DB: API for MongoDB account</span></span>
<span data-ttu-id="7f208-105">tooconnect tooan Azure Cosmos DB: API használatával Robomongo MongoDB-fiók, meg kell:</span><span class="sxs-lookup"><span data-stu-id="7f208-105">tooconnect tooan Azure Cosmos DB: API for MongoDB account using Robomongo, you must:</span></span>

* <span data-ttu-id="7f208-106">Töltse le és telepítse [Robomongo](https://robomongo.org/)</span><span class="sxs-lookup"><span data-stu-id="7f208-106">Download and install [Robomongo](https://robomongo.org/)</span></span>
* <span data-ttu-id="7f208-107">Az Azure Cosmos DB rendelkezik: API-t a MongoDB-fiók [kapcsolati karakterlánc](connect-mongodb-account.md) információk</span><span class="sxs-lookup"><span data-stu-id="7f208-107">Have your Azure Cosmos DB: API for MongoDB account [connection string](connect-mongodb-account.md) information</span></span>

## <a name="connect-using-robomongo"></a><span data-ttu-id="7f208-108">Csatlakozás Robomongo használatával</span><span class="sxs-lookup"><span data-stu-id="7f208-108">Connect using Robomongo</span></span>
<span data-ttu-id="7f208-109">tooadd az Azure Cosmos DB: API-t a MongoDB fiók toohello Robomongo MongoDB kapcsolatok, hajtsa végre a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="7f208-109">tooadd your Azure Cosmos DB: API for MongoDB account toohello Robomongo MongoDB Connections, perform hello following steps.</span></span>

1. <span data-ttu-id="7f208-110">Az Azure Cosmos DB beolvasása: API-t a MongoDB kapcsolat fiókadatok hello utasításokat követve [Itt](connect-mongodb-account.md).</span><span class="sxs-lookup"><span data-stu-id="7f208-110">Retrieve your Azure Cosmos DB: API for MongoDB account connection information using hello instructions [here](connect-mongodb-account.md).</span></span>

    ![Képernyőfelvétel a hello kapcsolati karakterlánc panel](./media/mongodb-robomongo/connectionstringblade.png)
2. <span data-ttu-id="7f208-112">Futtatás *Robomongo.exe*</span><span class="sxs-lookup"><span data-stu-id="7f208-112">Run *Robomongo.exe*</span></span>

3. <span data-ttu-id="7f208-113">Hello kapcsolat gombra a **fájl** toomanage a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="7f208-113">Click hello connection button under **File** toomanage your connections.</span></span> <span data-ttu-id="7f208-114">Kattintson a **létrehozása** hello a **MongoDB kapcsolatok** ablakban nyílik meg hello **Biztonságoskapcsolat-beállításainak** ablak.</span><span class="sxs-lookup"><span data-stu-id="7f208-114">Then, click **Create** in hello **MongoDB Connections** window, which will open up hello **Connection Settings** window.</span></span>

4. <span data-ttu-id="7f208-115">A hello **kapcsolatbeállítások** ablakban válasszon egy nevet.</span><span class="sxs-lookup"><span data-stu-id="7f208-115">In hello **Connection Settings** window, choose a name.</span></span> <span data-ttu-id="7f208-116">Ezután megkeresi a hello **állomás** és **Port** a kapcsolati információit az 1. lépés, és írja be őket **cím** és **Port**, illetve .</span><span class="sxs-lookup"><span data-stu-id="7f208-116">Then, find hello **Host** and **Port** from your connection information in Step 1 and enter them into **Address** and **Port**, respectively.</span></span>

    ![Képernyőfelvétel a hello Robomongo kapcsolatok kezelése](./media/mongodb-robomongo/manageconnections.png)
5. <span data-ttu-id="7f208-118">A hello **hitelesítési** lapra, majd **végrehajtása hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="7f208-118">On hello **Authentication** tab, click **Perform authentication**.</span></span> <span data-ttu-id="7f208-119">Ezután írja be az adatbázis (alapértelmezett érték *Admin*), **felhasználónév** és **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7f208-119">Then, enter your Database (default is *Admin*), **User Name** and **Password**.</span></span>
<span data-ttu-id="7f208-120">Mindkét **felhasználónév** és **jelszó** a kapcsolatadatok, 1. lépésben található.</span><span class="sxs-lookup"><span data-stu-id="7f208-120">Both **User Name** and **Password** can be found in your connection information in Step 1.</span></span>

    ![Képernyőfelvétel a hello Robomongo hitelesítés lap](./media/mongodb-robomongo/authentication.png)
6. <span data-ttu-id="7f208-122">A hello **SSL** lapon jelölje **használja az SSL protokoll**, majd módosítsa a hello **hitelesítési módszer** túl**önaláírt tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="7f208-122">On hello **SSL** tab, check **Use SSL protocol**, then change hello **Authentication Method** too**Self-signed Certificate**.</span></span>

    ![Képernyőfelvétel a hello Robomongo SSL lap](./media/mongodb-robomongo/SSL.png)
7. <span data-ttu-id="7f208-124">Végezetül kattintson **teszt** képes tooconnect, majd tooverify **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7f208-124">Finally, click **Test** tooverify that you are able tooconnect, then **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f208-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f208-125">Next steps</span></span>
* <span data-ttu-id="7f208-126">Ismerheti meg az Azure Cosmos DB: Mongodb-protokolltámogatással API [minták](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="7f208-126">Explore Azure Cosmos DB: API for MongoDB [samples](mongodb-samples.md).</span></span>
