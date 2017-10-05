---
title: "Az Azure CLI-parancsfájlt minták – hozzon létre egy prémium szintű Azure Redis Cache fürtözési |} Microsoft Docs"
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
ms.openlocfilehash: 87d0fe4c3eaa8f7b75343a36a069ecdac8241d74
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-premium-azure-redis-cache-with-clustering"></a><span data-ttu-id="ba5a6-103">Hozzon létre egy prémium szintű Azure Redis Cache fürtözési</span><span class="sxs-lookup"><span data-stu-id="ba5a6-103">Create a Premium Azure Redis Cache with clustering</span></span>

<span data-ttu-id="ba5a6-104">Ebben a forgatókönyvben elsajátíthatja a fürtözési engedélyezve van egy 6 GB prémium csomagban Azure Redis Cache létrehozása és a két szilánkok.</span><span class="sxs-lookup"><span data-stu-id="ba5a6-104">In this scenario, you learn how to create a 6 GB Premium tier Azure Redis Cache with clustering enabled and two shards.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="ba5a6-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ba5a6-105">Sample script</span></span>

<span data-ttu-id="ba5a6-106">[!code-azurecli[fő](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]</span><span class="sxs-lookup"><span data-stu-id="ba5a6-106">[!code-azurecli[main](../../../cli_scripts/redis-cache/create-premium-cache-cluster/create-premium-cache-cluster.sh "Azure Redis Cache")]</span></span>

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="ba5a6-107">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ba5a6-107">Script explanation</span></span>

<span data-ttu-id="ba5a6-108">A parancsfájl a következő parancsokat egy erőforráscsoport és a Premium szint redis gyorsítótár létrehozása a fürtszolgáltatás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ba5a6-108">This script uses the following commands to create a resource group and a Premium tier redis cache with clustering enable.</span></span> <span data-ttu-id="ba5a6-109">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="ba5a6-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ba5a6-110">Parancs</span><span class="sxs-lookup"><span data-stu-id="ba5a6-110">Command</span></span> | <span data-ttu-id="ba5a6-111">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ba5a6-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ba5a6-112">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba5a6-112">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ba5a6-113">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ba5a6-113">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ba5a6-114">az redis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ba5a6-114">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="ba5a6-115">Hozzon létre a Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="ba5a6-115">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="ba5a6-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ba5a6-116">Next steps</span></span>

<span data-ttu-id="ba5a6-117">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ba5a6-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ba5a6-118">További Azure Redis Cache CLI parancsfájl minták megtalálhatók a [Azure Redis Cache dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ba5a6-118">Additional Azure Redis Cache CLI script samples can be found in the [Azure Redis Cache documentation](../cli-samples.md).</span></span>