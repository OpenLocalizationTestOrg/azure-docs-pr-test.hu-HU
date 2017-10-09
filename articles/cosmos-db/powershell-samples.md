---
title: "PowerShell-példák Azure Cosmos DB aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-példák - parancsfájlok toohelp létrehozásakor és kezelésekor Azure Cosmos DB fiókok."
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
ms.openlocfilehash: 8815e775f720226e98cc8dcf7743e481c213272b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a><span data-ttu-id="caea6-103">Azure Cosmos DB Azure PowerShell-példák</span><span class="sxs-lookup"><span data-stu-id="caea6-103">Azure PowerShell samples for Azure Cosmos DB</span></span>

<span data-ttu-id="caea6-104">hello alábbi táblázat tartalmaz hivatkozásokat toosample Azure PowerShell-parancsfájlok Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="caea6-104">hello following table includes links toosample Azure PowerShell scripts for Azure Cosmos DB.</span></span>

| |  |
|---|---|
|<span data-ttu-id="caea6-105">**Azure Cosmos DB-fiók létrehozása**</span><span class="sxs-lookup"><span data-stu-id="caea6-105">**Create an Azure Cosmos DB account**</span></span>||
|[<span data-ttu-id="caea6-106">A DocumentDB API-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="caea6-106">Create a DocumentDB API account</span></span>](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="caea6-107">Egy Azure Cosmos DB egyetlen fiók toouse hello DocumentDB API hoz létre.</span><span class="sxs-lookup"><span data-stu-id="caea6-107">Creates a single Azure Cosmos DB account toouse with hello DocumentDB API.</span></span> |
|<span data-ttu-id="caea6-108">**Az Azure Cosmos DB méretezése**</span><span class="sxs-lookup"><span data-stu-id="caea6-108">**Scale Azure Cosmos DB**</span></span>||
|[<span data-ttu-id="caea6-109">Azure Cosmos DB fiók több régióba replikálja, és feladatátvételi prioritások beállítása</span><span class="sxs-lookup"><span data-stu-id="caea6-109">Replicate Azure Cosmos DB account in multiple regions and configure failover priorities</span></span>](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="caea6-110">Egy adott feladatátvevő prioritású több területekre globálisan replikálja a fiók adatait.</span><span class="sxs-lookup"><span data-stu-id="caea6-110">Globally replicates account data into multiple regions with a specified failover priority.</span></span>|
|<span data-ttu-id="caea6-111">**Az Azure Cosmos DB biztonságos**</span><span class="sxs-lookup"><span data-stu-id="caea6-111">**Secure Azure Cosmos DB**</span></span>||
| [<span data-ttu-id="caea6-112">Fiók kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="caea6-112">Get account keys</span></span>](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="caea6-113">Lekérdezi a hello fő írási elsődleges és másodlagos kulcsok és hello fiók az elsődleges és másodlagos csak olvasható kulcsok.</span><span class="sxs-lookup"><span data-stu-id="caea6-113">Gets hello primary and secondary master write keys and primary and secondary read-only keys for hello account.</span></span>|
| [<span data-ttu-id="caea6-114">MongoDB kapcsolati karakterlánc beolvasása</span><span class="sxs-lookup"><span data-stu-id="caea6-114">Get MongoDB connection string</span></span>](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | <span data-ttu-id="caea6-115">Hello kapcsolati karakterlánc tooconnect lekérdezi a MongoDB-alkalmazás tooyour Azure Cosmos DB fiók.</span><span class="sxs-lookup"><span data-stu-id="caea6-115">Gets hello connection string tooconnect your MongoDB app tooyour Azure Cosmos DB account.</span></span>|
|[<span data-ttu-id="caea6-116">Fiók kulcsok újragenerálása</span><span class="sxs-lookup"><span data-stu-id="caea6-116">Regenerate account keys</span></span>](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="caea6-117">Hello fő vagy a csak olvasható hello fiók kulcsának újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="caea6-117">Regenerates hello master or read-only key for hello account.</span></span>|
|[<span data-ttu-id="caea6-118">Hozzon létre egy tűzfal</span><span class="sxs-lookup"><span data-stu-id="caea6-118">Create a firewall</span></span>](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| <span data-ttu-id="caea6-119">Létrehoz egy bejövő IP hozzáférés vezérlő házirend toolimit hozzáférés toohello fiókot gépek és/vagy felhőszolgáltatások egy jóváhagyott készletből.</span><span class="sxs-lookup"><span data-stu-id="caea6-119">Creates an inbound IP access control policy toolimit access toohello account from an approved set of machines and/or cloud services.</span></span>|
|<span data-ttu-id="caea6-120">**Magas rendelkezésre állású, katasztrófa utáni helyreállítás, biztonsági mentése és visszaállítása**</span><span class="sxs-lookup"><span data-stu-id="caea6-120">**High availability, disaster recovery, backup and restore**</span></span>||
|[<span data-ttu-id="caea6-121">Feladatátvételi házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="caea6-121">Configure failover policy</span></span>](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|<span data-ttu-id="caea6-122">Készlet minden egyes régió, ahol a rendszer replikálja a hello fiók feladatátvételi prioritási hello.</span><span class="sxs-lookup"><span data-stu-id="caea6-122">Sets hello failover priority of each region in which hello account is replicated.</span></span>|
|||