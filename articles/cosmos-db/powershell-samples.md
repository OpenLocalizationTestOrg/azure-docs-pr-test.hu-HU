---
title: "Az Azure PowerShell-példák Azure Cosmos DB |} Microsoft Docs"
description: "Az Azure PowerShell-példák - parancsfájlok használatával segítséget Azure Cosmos DB fiókok létrehozásához és kezeléséhez."
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 7698e03c0dc8d1c6d1e926f45e903a909bfd0c93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a><span data-ttu-id="fd3b3-103">Azure Cosmos DB Azure PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="fd3b3-103">Azure PowerShell samples for Azure Cosmos DB</span></span>

<span data-ttu-id="fd3b3-104">A következő táblázat az Azure Cosmos DB Azure PowerShell-mintaparancsfájlok hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-104">The following table includes links to sample Azure PowerShell scripts for Azure Cosmos DB.</span></span>

| |  |
|---|---|
|<span data-ttu-id="fd3b3-105">**Azure Cosmos DB-fiók létrehozása**</span><span class="sxs-lookup"><span data-stu-id="fd3b3-105">**Create an Azure Cosmos DB account**</span></span>||
|[<span data-ttu-id="fd3b3-106">A DocumentDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fd3b3-106">Create a DocumentDB API account</span></span>](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="fd3b3-107">Létrehoz egy egyetlen Azure Cosmos DB fiókot használni a DocumentDB API-t.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-107">Creates a single Azure Cosmos DB account to use with the DocumentDB API.</span></span> |
|<span data-ttu-id="fd3b3-108">**Az Azure Cosmos DB méretezése**</span><span class="sxs-lookup"><span data-stu-id="fd3b3-108">**Scale Azure Cosmos DB**</span></span>||
|[<span data-ttu-id="fd3b3-109">Azure Cosmos DB fiók több régióba replikálja, és feladatátvételi prioritások beállítása</span><span class="sxs-lookup"><span data-stu-id="fd3b3-109">Replicate Azure Cosmos DB account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="fd3b3-110">Egy adott feladatátvevő prioritású több területekre globálisan replikálja a fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-110">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="fd3b3-111">**Az Azure Cosmos DB biztonságos**</span><span class="sxs-lookup"><span data-stu-id="fd3b3-111">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="fd3b3-112">Fiók kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="fd3b3-112">Get account keys</span></span>](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="fd3b3-113">Lekérdezi a fő írási elsődleges és másodlagos kulcsok és a fiók elsődleges és másodlagos csak olvasható kulcsok.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-113">Gets the primary and secondary master write keys and primary and secondary read-only keys for the account.</span></span>|
| [<span data-ttu-id="fd3b3-114">MongoDB kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="fd3b3-114">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="fd3b3-115">A kapcsolati karakterláncot a MongoDB alkalmazás csatlakoztatása az Azure Cosmos DB fiók lekérése.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-115">Gets the connection string to connect your MongoDB app to your Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="fd3b3-116">Fiók kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="fd3b3-116">Regenerate account keys</span></span>](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="fd3b3-117">A fiók a fő vagy csak olvasható kulcsának újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-117">Regenerates the master or read-only key for the account.</span></span>|
|[<span data-ttu-id="fd3b3-118">Hozzon létre egy tűzfal</span><span class="sxs-lookup"><span data-stu-id="fd3b3-118">Create a firewall</span></span>](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="fd3b3-119">Létrehoz egy bejövő IP hozzáférés-vezérlési szabályzat egy jóváhagyott gépek csoportja tartalmazza a fiókja korlátozhatják és/vagy a felhőalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-119">Creates an inbound IP access control policy to limit access to the account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="fd3b3-120">**Magas rendelkezésre állású, katasztrófa utáni helyreállítás, biztonsági mentése és visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="fd3b3-120">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="fd3b3-121">Feladatátvételi házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fd3b3-121">Configure failover policy</span></span>](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="fd3b3-122">Minden egyes régió, ahol a fiók replikálódik feladatátvételi prioritását állítja.</span><span class="sxs-lookup"><span data-stu-id="fd3b3-122">Sets the failover priority of each region in which the account is replicated.</span></span>|
|||