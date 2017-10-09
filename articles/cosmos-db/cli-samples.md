---
title: "aaaAzure Azure Cosmos DB CLI-példák |} Microsoft Docs"
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
ms.openlocfilehash: d6eefc3274e0b66eec4e69166bb7d4ddd58a522e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a><span data-ttu-id="74fd4-103">Az Azure Cosmos DB Azure CLI-minták</span><span class="sxs-lookup"><span data-stu-id="74fd4-103">Azure CLI samples for Azure Cosmos DB</span></span>

<span data-ttu-id="74fd4-104">hello alábbi táblázat tartalmaz hivatkozásokat toosample Azure CLI parancsfájlok Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="74fd4-104">hello following table includes links toosample Azure CLI scripts for Azure Cosmos DB.</span></span> <span data-ttu-id="74fd4-105">Az összes Azure Cosmos DB parancssori felület parancsait hivatkozás lapok érhetők el hello [Azure CLI 2.0 hivatkozás](https://docs.microsoft.com/cli/azure/cosmosdb).</span><span class="sxs-lookup"><span data-stu-id="74fd4-105">Reference pages for all Azure Cosmos DB CLI commands are available in hello [Azure CLI 2.0 Reference](https://docs.microsoft.com/cli/azure/cosmosdb).</span></span>

| |  |
|---|---|
|<span data-ttu-id="74fd4-106">**Azure Cosmos DB fiók, az adatbázis és a tárolók létrehozása**</span><span class="sxs-lookup"><span data-stu-id="74fd4-106">**Create Azure Cosmos DB account, database, and containers**</span></span>||
|[<span data-ttu-id="74fd4-107">A DocumentDB API-t, a Graph vagy a tábla API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="74fd4-107">Create a DocumentDB API, Graph, or Table API account</span></span>](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="74fd4-108">Egyetlen Azure Cosmos DB API-fiók, adatbázis és tárolót használja a DocumentDB, a diagram vagy a tábla API-k hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="74fd4-108">Creates a single Azure Cosmos DB API account, database, and container for use with hello DocumentDB, Graph, or Table APIs.</span></span> |
| [<span data-ttu-id="74fd4-109">MongoDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="74fd4-109">Create a MongoDB API account</span></span>](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="74fd4-110">Létrehoz egy egyetlen Azure Cosmos DB MongoDB API fiók, adatbázis és gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="74fd4-110">Creates a single Azure Cosmos DB MongoDB API account, database, and collection.</span></span> |
|<span data-ttu-id="74fd4-111">**Az Azure Cosmos DB méretezése**</span><span class="sxs-lookup"><span data-stu-id="74fd4-111">**Scale Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="74fd4-112">Skála tároló átviteli sebesség</span><span class="sxs-lookup"><span data-stu-id="74fd4-112">Scale container throughput</span></span>](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="74fd4-113">Módosítások hello tárolóba througput kiépítve.</span><span class="sxs-lookup"><span data-stu-id="74fd4-113">Changes hello provisioned througput on a container.</span></span>|
|[<span data-ttu-id="74fd4-114">Azure Cosmos DB adatbázisfiók több régióba replikálja, és feladatátvételi prioritások beállítása</span><span class="sxs-lookup"><span data-stu-id="74fd4-114">Replicate Azure Cosmos DB database account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="74fd4-115">Egy adott feladatátvevő prioritású több területekre globálisan replikálja a fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="74fd4-115">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="74fd4-116">**Az Azure Cosmos DB biztonságos**</span><span class="sxs-lookup"><span data-stu-id="74fd4-116">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="74fd4-117">Fiók kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="74fd4-117">Get account keys</span></span>](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="74fd4-118">Lekérdezi a hello fő írási elsődleges és másodlagos kulcsok és hello fiók az elsődleges és másodlagos csak olvasható kulcsok.</span><span class="sxs-lookup"><span data-stu-id="74fd4-118">Gets hello primary and secondary master write keys and primary and secondary read-only keys for hello account.</span></span>|
| [<span data-ttu-id="74fd4-119">MongoDB kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="74fd4-119">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="74fd4-120">Hello kapcsolati karakterlánc tooconnect lekérdezi a MongoDB-alkalmazás tooyour Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="74fd4-120">Gets hello connection string tooconnect your MongoDB app tooyour Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="74fd4-121">Fiók kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="74fd4-121">Regenerate account keys</span></span>](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="74fd4-122">Hello fő vagy a csak olvasható hello fiók kulcsának újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="74fd4-122">Regenerates hello master or read-only key for hello account.</span></span>|
|[<span data-ttu-id="74fd4-123">Hozzon létre egy tűzfal</span><span class="sxs-lookup"><span data-stu-id="74fd4-123">Create a firewall</span></span>](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| <span data-ttu-id="74fd4-124">Létrehoz egy bejövő IP hozzáférés vezérlő házirend toolimit hozzáférés toohello fiókot gépek és/vagy felhőszolgáltatások egy jóváhagyott készletből.</span><span class="sxs-lookup"><span data-stu-id="74fd4-124">Creates an inbound IP access control policy toolimit access toohello account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="74fd4-125">**Magas rendelkezésre állású, katasztrófa utáni helyreállítás, biztonsági mentése és visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="74fd4-125">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="74fd4-126">Feladatátvételi házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="74fd4-126">Configure failover policy</span></span>](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="74fd4-127">Készlet minden egyes régió, ahol a rendszer replikálja a hello fiók feladatátvételi prioritási hello.</span><span class="sxs-lookup"><span data-stu-id="74fd4-127">Sets hello failover priority of each region in which hello account is replicated.</span></span>|
|<span data-ttu-id="74fd4-128">**Csatlakozás Azure Cosmos DB erőforrások**</span><span class="sxs-lookup"><span data-stu-id="74fd4-128">**Connect Azure Cosmos DB to resources**</span></span>||
|[<span data-ttu-id="74fd4-129">Csatlakozás egy webes alkalmazás tooAzure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="74fd4-129">Connect a web app tooAzure Cosmos DB</span></span>](https://docs.microsoft.com/azure/app-service-web/scripts/app-service-cli-app-service-documentdb?toc=%2fcli%2fazure%2ftoc.json)|<span data-ttu-id="74fd4-130">Hozzon létre, és csatlakoztassa egy Azure Cosmos DB adatbázis és az Azure-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="74fd4-130">Create and connect an Azure Cosmos DB database and an Azure web app.</span></span>|
|||
