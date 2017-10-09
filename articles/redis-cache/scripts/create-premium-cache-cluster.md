---
title: "aaaAzure CLI parancsfájl minta - fürtözési hozzon létre egy prémium szintű Azure Redis Cache |} Microsoft Docs"
description: "Az Azure CLI-parancsfájlt minta - fürtözési Azure Redis Cache prémium réteg létrehozása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 07bcceae-2521-4fe3-b88f-ed833104ddd2
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: ca34d40059b282cb2abc7e3e2b8771226029744c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a><span data-ttu-id="84ef5-103">Hozzon létre egy prémium szintű Azure Redis Cache fürtözési</span><span class="sxs-lookup"><span data-stu-id="84ef5-103">Create a Premium Azure Redis Cache with clustering</span></span>

<span data-ttu-id="84ef5-104">Ebben az esetben megismerheti, hogyan toocreate egy 6 GB prémium csomagban Azure Redis Cache fürtözési engedélyezve van, és két szilánkok.</span><span class="sxs-lookup"><span data-stu-id="84ef5-104">In this scenario, you learn how toocreate a 6 GB Premium tier Azure Redis Cache with clustering enabled and two shards.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="84ef5-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="84ef5-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="84ef5-106">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="84ef5-106">Script explanation</span></span>

<span data-ttu-id="84ef5-107">A parancsfájl a következő parancsok toocreate hello az erőforráscsoportot és a Premium szint redis gyorsítótár fürtözési engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="84ef5-107">This script uses hello following commands toocreate a resource group and a Premium tier redis cache with clustering enable.</span></span> <span data-ttu-id="84ef5-108">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="84ef5-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="84ef5-109">Parancs</span><span class="sxs-lookup"><span data-stu-id="84ef5-109">Command</span></span> | <span data-ttu-id="84ef5-110">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="84ef5-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="84ef5-111">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="84ef5-111">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="84ef5-112">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="84ef5-112">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="84ef5-113">az redis létrehozása</span><span class="sxs-lookup"><span data-stu-id="84ef5-113">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="84ef5-114">Hozzon létre a Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="84ef5-114">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="84ef5-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84ef5-115">Next steps</span></span>

<span data-ttu-id="84ef5-116">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="84ef5-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="84ef5-117">További Azure Redis Cache CLI parancsfájl minták hello található [Azure Redis Cache dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="84ef5-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
