---
title: "Az Azure CLI-példák Azure Cosmos DB |} Microsoft Docs"
description: "Az Azure CLI minták – Azure Cosmos DB fiókok, adatbázisok, tárolók, régiók és tűzfalak létrehozása és kezelése."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 06/07/2017
ms.author: mimig
ms.openlocfilehash: 709d2ccce0f4b9827a8076f683c7e0f74cbdd4ea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a><span data-ttu-id="1b173-103">Az Azure Cosmos DB Azure CLI-minták</span><span class="sxs-lookup"><span data-stu-id="1b173-103">Azure CLI samples for Azure Cosmos DB</span></span>

<span data-ttu-id="1b173-104">A következő táblázat az Azure Cosmos DB Azure CLI mintaparancsfájlok hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="1b173-104">The following table includes links to sample Azure CLI scripts for Azure Cosmos DB.</span></span> <span data-ttu-id="1b173-105">Az összes Azure Cosmos DB parancssori felület parancsait hivatkozás lapok érhetők el a [Azure CLI 2.0 hivatkozás](https://docs.microsoft.com/cli/azure/cosmosdb).</span><span class="sxs-lookup"><span data-stu-id="1b173-105">Reference pages for all Azure Cosmos DB CLI commands are available in the [Azure CLI 2.0 Reference](https://docs.microsoft.com/cli/azure/cosmosdb).</span></span>

| |  |
|---|---|
|<span data-ttu-id="1b173-106">**Azure Cosmos DB fiók, az adatbázis és a tárolók létrehozása**</span><span class="sxs-lookup"><span data-stu-id="1b173-106">**Create Azure Cosmos DB account, database, and containers**</span></span>||
|[<span data-ttu-id="1b173-107">A DocumentDB API-t, a Graph vagy a tábla API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b173-107">Create a DocumentDB API, Graph, or Table API account</span></span>](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="1b173-108">A DocumentDB, a diagram vagy a tábla API-k egyetlen Azure Cosmos DB API-fiók, adatbázis és használatra tároló hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1b173-108">Creates a single Azure Cosmos DB API account, database, and container for use with the DocumentDB, Graph, or Table APIs.</span></span> |
| [<span data-ttu-id="1b173-109">MongoDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="1b173-109">Create a MongoDB API account</span></span>](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="1b173-110">Létrehoz egy egyetlen Azure Cosmos DB MongoDB API fiók, adatbázis és gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="1b173-110">Creates a single Azure Cosmos DB MongoDB API account, database, and collection.</span></span> |
|<span data-ttu-id="1b173-111">**Az Azure Cosmos DB méretezése**</span><span class="sxs-lookup"><span data-stu-id="1b173-111">**Scale Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="1b173-112">Skála tároló átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="1b173-112">Scale container throughput</span></span>](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="1b173-113">Módosítja a kiosztott througput tárolóba.</span><span class="sxs-lookup"><span data-stu-id="1b173-113">Changes the provisioned througput on a container.</span></span>|
|[<span data-ttu-id="1b173-114">Azure Cosmos DB adatbázisfiók több régióba replikálja, és feladatátvételi prioritások beállítása</span><span class="sxs-lookup"><span data-stu-id="1b173-114">Replicate Azure Cosmos DB database account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="1b173-115">Egy adott feladatátvevő prioritású több területekre globálisan replikálja a fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="1b173-115">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="1b173-116">**Az Azure Cosmos DB biztonságos**</span><span class="sxs-lookup"><span data-stu-id="1b173-116">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="1b173-117">Fiók kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="1b173-117">Get account keys</span></span>](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="1b173-118">Lekérdezi a fő írási elsődleges és másodlagos kulcsok és a fiók elsődleges és másodlagos csak olvasható kulcsok.</span><span class="sxs-lookup"><span data-stu-id="1b173-118">Gets the primary and secondary master write keys and primary and secondary read-only keys for the account.</span></span>|
| [<span data-ttu-id="1b173-119">MongoDB kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="1b173-119">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="1b173-120">A kapcsolati karakterláncot a MongoDB alkalmazás csatlakoztatása az Azure Cosmos DB fiók lekérése.</span><span class="sxs-lookup"><span data-stu-id="1b173-120">Gets the connection string to connect your MongoDB app to your Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="1b173-121">Fiók kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="1b173-121">Regenerate account keys</span></span>](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="1b173-122">A fiók a fő vagy csak olvasható kulcsának újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="1b173-122">Regenerates the master or read-only key for the account.</span></span>|
|[<span data-ttu-id="1b173-123">Hozzon létre egy tűzfal</span><span class="sxs-lookup"><span data-stu-id="1b173-123">Create a firewall</span></span>](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="1b173-124">Létrehoz egy bejövő IP hozzáférés-vezérlési szabályzat egy jóváhagyott gépek csoportja tartalmazza a fiókja korlátozhatják és/vagy a felhőalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="1b173-124">Creates an inbound IP access control policy to limit access to the account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="1b173-125">**Magas rendelkezésre állású, katasztrófa utáni helyreállítás, biztonsági mentése és visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="1b173-125">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="1b173-126">Feladatátvételi házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1b173-126">Configure failover policy</span></span>](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="1b173-127">Minden egyes régió, ahol a fiók replikálódik feladatátvételi prioritását állítja.</span><span class="sxs-lookup"><span data-stu-id="1b173-127">Sets the failover priority of each region in which the account is replicated.</span></span>|
|<span data-ttu-id="1b173-128">**Csatlakozás Azure Cosmos DB erőforrások**</span><span class="sxs-lookup"><span data-stu-id="1b173-128">**Connect Azure Cosmos DB to resources**</span></span>||
|[<span data-ttu-id="1b173-129">Webes alkalmazás csatlakoztatása az Azure Cosmos-Adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="1b173-129">Connect a web app to Azure Cosmos DB</span></span>](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="1b173-130">Hozzon létre, és csatlakoztassa egy Azure Cosmos DB adatbázis és az Azure-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1b173-130">Create and connect an Azure Cosmos DB database and an Azure web app.</span></span>|
|||
