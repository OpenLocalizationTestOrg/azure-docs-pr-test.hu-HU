---
title: "egy Azure Cosmos DB fiókhoz aaaMongoDB kapcsolati karakterláncot |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconnect a MongoDB-alkalmazás tooan Azure Cosmos DB fiók MongoDB kapcsolati karakterlánc használatával."
keywords: "mongodb-kapcsolati karakterlánc"
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: e36f7375-9329-403b-afd1-4ab49894f75e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2017
ms.author: anhoh
ms.openlocfilehash: c0b81cb49a10e09e3f02411b91731c7f980ec47d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-a-mongodb-application-tooazure-cosmos-db"></a><span data-ttu-id="e1490-104">Csatlakozás a MongoDB-alkalmazás tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="e1490-104">Connect a MongoDB application tooAzure Cosmos DB</span></span>
<span data-ttu-id="e1490-105">Ismerje meg, hogyan tooconnect a MongoDB-alkalmazás tooan Azure Cosmos DB fiók MongoDB kapcsolati karakterlánc használatával.</span><span class="sxs-lookup"><span data-stu-id="e1490-105">Learn how tooconnect your MongoDB app tooan Azure Cosmos DB account by using a MongoDB connection string.</span></span> <span data-ttu-id="e1490-106">Ezután használhatja az Azure Cosmos DB adatbázis hello adatként a MongoDB alkalmazás tárolására.</span><span class="sxs-lookup"><span data-stu-id="e1490-106">You can then use an Azure Cosmos DB database as hello data store for your MongoDB app.</span></span> 

<span data-ttu-id="e1490-107">Ez az oktatóanyag két lehetőséget biztosít tooretrieve kapcsolati karakterlánc adatokat:</span><span class="sxs-lookup"><span data-stu-id="e1490-107">This tutorial provides two ways tooretrieve connection string information:</span></span>

- <span data-ttu-id="e1490-108">[hello gyors üzembe helyezési módszer](#QuickstartConnection), .NET, Node.js, MongoDB rendszerhéj, Java és Python illesztőprogramok való használatra</span><span class="sxs-lookup"><span data-stu-id="e1490-108">[hello quick start method](#QuickstartConnection), for use with .NET, Node.js, MongoDB Shell, Java, and Python drivers</span></span>
- <span data-ttu-id="e1490-109">[Egyéni kapcsolati karakterlánc metódus hello](#GetCustomConnection), más illesztőprogramok való használatra</span><span class="sxs-lookup"><span data-stu-id="e1490-109">[hello custom connection string method](#GetCustomConnection), for use with other drivers</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1490-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e1490-110">Prerequisites</span></span>

- <span data-ttu-id="e1490-111">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="e1490-111">An Azure account.</span></span> <span data-ttu-id="e1490-112">Ha az Azure-fiók nem rendelkezik, hozzon létre egy [ingyenes Azure-fiókot](https://azure.microsoft.com/free/) most.</span><span class="sxs-lookup"><span data-stu-id="e1490-112">If you don't have an Azure account, create a [free Azure account](https://azure.microsoft.com/free/) now.</span></span> 
- <span data-ttu-id="e1490-113">Azure Cosmos DB-fiók.</span><span class="sxs-lookup"><span data-stu-id="e1490-113">An Azure Cosmos DB account.</span></span> <span data-ttu-id="e1490-114">Útmutatásért lásd: [a .NET MongoDB API webalkalmazás létrehozása és hello Azure-portálon](create-mongodb-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="e1490-114">For instructions, see [Build a MongoDB API web app with .NET and hello Azure portal](create-mongodb-dotnet.md).</span></span>

## <span data-ttu-id="e1490-115"><a id="QuickstartConnection"></a>Gyors üzembe helyezési hello segítségével könnyebben nyerhet hello MongoDB kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="e1490-115"><a id="QuickstartConnection"></a>Get hello MongoDB connection string by using hello quick start</span></span>
1. <span data-ttu-id="e1490-116">Egy webböngészőben, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e1490-116">In an Internet browser, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e1490-117">A hello **Azure Cosmos DB** panelen, jelölje be hello API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="e1490-117">In hello **Azure Cosmos DB** blade, select hello API for MongoDB account.</span></span> 
3. <span data-ttu-id="e1490-118">Hello bal oldali ablaktábláján hello fiók panelén kattintson **gyors üzembe helyezési**.</span><span class="sxs-lookup"><span data-stu-id="e1490-118">In hello left pane of hello account blade, click **Quick start**.</span></span> 
4. <span data-ttu-id="e1490-119">Válassza ki a platformot (**.NET**, **Node.js**, **MongoDB rendszerhéj**, **Java**, **Python**).</span><span class="sxs-lookup"><span data-stu-id="e1490-119">Choose your platform (**.NET**, **Node.js**, **MongoDB Shell**, **Java**, **Python**).</span></span> <span data-ttu-id="e1490-120">Ha nem látja az illesztőprogram vagy eszköz szerepel, ne aggódjon--azt folyamatosan a dokumentum több kapcsolat kódrészleteket.</span><span class="sxs-lookup"><span data-stu-id="e1490-120">If you don't see your driver or tool listed, don't worry--we continuously document more connection code snippets.</span></span> <span data-ttu-id="e1490-121">Adjon fűzni alatt mi szeretné toosee.</span><span class="sxs-lookup"><span data-stu-id="e1490-121">Please comment below on what you'd like toosee.</span></span> <span data-ttu-id="e1490-122">toolearn hogyan toocraft a saját hálózati kapcsolatot, olvassa el [hello fiókhoz tartozó kapcsolati karakterlánc adatainak megszerzése](#GetCustomConnection).</span><span class="sxs-lookup"><span data-stu-id="e1490-122">toolearn how toocraft your own connection, read [Get hello account's connection string information](#GetCustomConnection).</span></span>
5. <span data-ttu-id="e1490-123">Másolással illessze be a hello kódrészletet a MongoDB-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e1490-123">Copy and paste hello code snippet into your MongoDB app.</span></span>

    ![Gyors indítás panel](./media/connect-mongodb-account/QuickStartBlade.png)

## <span data-ttu-id="e1490-125"><a id="GetCustomConnection"></a>Hello MongoDB kapcsolati karakterlánc toocustomize beolvasása</span><span class="sxs-lookup"><span data-stu-id="e1490-125"><a id="GetCustomConnection"></a> Get hello MongoDB connection string toocustomize</span></span>
1. <span data-ttu-id="e1490-126">Egy webböngészőben, jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e1490-126">In an Internet browser, sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e1490-127">A hello **Azure Cosmos DB** panelen, jelölje be hello API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="e1490-127">In hello **Azure Cosmos DB** blade, select hello API for MongoDB account.</span></span> 
3. <span data-ttu-id="e1490-128">Hello bal oldali ablaktábláján hello fiók panelén kattintson **kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="e1490-128">In hello left pane of hello account blade, click **Connection String**.</span></span> 
4. <span data-ttu-id="e1490-129">Hello **kapcsolati karakterlánc** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="e1490-129">hello **Connection String** blade opens.</span></span> <span data-ttu-id="e1490-130">Minden hello információ szükséges tooconnect toohello fiók rendelkezik illesztőprogrammal mongodb, beleértve a preconstructed kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="e1490-130">It has all hello information necessary tooconnect toohello account by using a driver for MongoDB, including a preconstructed connection string.</span></span>

    ![Kapcsolati karakterlánc panel](./media/connect-mongodb-account/ConnectionStringBlade.png)

## <a name="connection-string-requirements"></a><span data-ttu-id="e1490-132">Kapcsolati karakterlánc követelmények</span><span class="sxs-lookup"><span data-stu-id="e1490-132">Connection string requirements</span></span>
> [!Important]
> <span data-ttu-id="e1490-133">Azure Cosmos DB szigorú biztonsági követelmények és szabványok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e1490-133">Azure Cosmos DB has strict security requirements and standards.</span></span> <span data-ttu-id="e1490-134">Azure Cosmos DB fiókok szükséges hitelesítés és a biztonságos kommunikáció keresztül *SSL*.</span><span class="sxs-lookup"><span data-stu-id="e1490-134">Azure Cosmos DB accounts require authentication and secure communication via *SSL*.</span></span> 
>
>

<span data-ttu-id="e1490-135">Azure Cosmos DB hello szabványos MongoDB kapcsolati karakterlánc URI-formátum, az adott igények szerint néhány támogatja: Azure Cosmos DB fiókok hitelesítési és a biztonságos kommunikációhoz SSL-en keresztüli van szükség.</span><span class="sxs-lookup"><span data-stu-id="e1490-135">Azure Cosmos DB supports hello standard MongoDB connection string URI format, with a couple of specific requirements: Azure Cosmos DB accounts require authentication and secure communication via SSL.</span></span> <span data-ttu-id="e1490-136">Igen van hello kapcsolati karakterlánc-formátum:</span><span class="sxs-lookup"><span data-stu-id="e1490-136">So, hello connection string format is:</span></span>

    mongodb://username:password@host:port/[database]?ssl=true

<span data-ttu-id="e1490-137">Ez a karakterlánc hello értékei a következők hello érhető el **kapcsolati karakterlánc** korábban bemutatott panel:</span><span class="sxs-lookup"><span data-stu-id="e1490-137">hello values of this string are available in hello **Connection String** blade shown earlier:</span></span>

* <span data-ttu-id="e1490-138">Felhasználónév (kötelező): Azure Cosmos DB fióknevet.</span><span class="sxs-lookup"><span data-stu-id="e1490-138">Username (required): Azure Cosmos DB account name.</span></span>
* <span data-ttu-id="e1490-139">Jelszó (szükséges): Azure Cosmos DB fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="e1490-139">Password (required): Azure Cosmos DB account password.</span></span>
* <span data-ttu-id="e1490-140">Állomás (kötelező): hello Azure Cosmos DB fiók teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="e1490-140">Host (required): FQDN of hello Azure Cosmos DB account.</span></span>
* <span data-ttu-id="e1490-141">Port (kötelező): 10255.</span><span class="sxs-lookup"><span data-stu-id="e1490-141">Port (required): 10255.</span></span>
* <span data-ttu-id="e1490-142">Adatbázis (nem kötelező): hello adatbázis, amely hello kapcsolatot használ.</span><span class="sxs-lookup"><span data-stu-id="e1490-142">Database (optional): hello database that hello connection uses.</span></span> <span data-ttu-id="e1490-143">Ha nincs adatbázis, hello alapértelmezett adatbázis-e a "teszt".</span><span class="sxs-lookup"><span data-stu-id="e1490-143">If no database is provided, hello default database is "test."</span></span>
* <span data-ttu-id="e1490-144">SSL = true (kötelező)</span><span class="sxs-lookup"><span data-stu-id="e1490-144">ssl=true (required)</span></span>

<span data-ttu-id="e1490-145">Vegyük példaként hello látható hello fiók **kapcsolati karakterlánc** panelen.</span><span class="sxs-lookup"><span data-stu-id="e1490-145">For example, consider hello account shown in hello **Connection String** blade.</span></span> <span data-ttu-id="e1490-146">Egy érvényes kapcsolati karakterlánca:</span><span class="sxs-lookup"><span data-stu-id="e1490-146">A valid connection string is:</span></span>

    mongodb://contoso123:0Fc3IolnL12312asdfawejunASDF@asdfYXX2t8a97kghVcUzcDv98hawelufhawefafnoQRGwNj2nMPL1Y9qsIr9Srdw==@anhohmongo.documents.azure.com:10255/mydatabase?ssl=true

## <a name="next-steps"></a><span data-ttu-id="e1490-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e1490-147">Next steps</span></span>
* <span data-ttu-id="e1490-148">Ismerje meg, hogyan túl[MongoChef használja](mongodb-mongochef.md) egy Azure Cosmos DB API-t a MongoDB-fiók.</span><span class="sxs-lookup"><span data-stu-id="e1490-148">Learn how too[use MongoChef](mongodb-mongochef.md) with an Azure Cosmos DB API for MongoDB account.</span></span>
* <span data-ttu-id="e1490-149">Hello Azure Cosmos DB API megismerkedhet a mongodb-protokolltámogatással megtekintésével [minták](mongodb-samples.md).</span><span class="sxs-lookup"><span data-stu-id="e1490-149">Explore hello Azure Cosmos DB API for MongoDB by viewing [samples](mongodb-samples.md).</span></span>
