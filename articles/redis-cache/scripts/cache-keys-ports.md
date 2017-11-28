---
title: "parancssori felület parancsfájl minta - Get hello állomásnév, portokat és az Azure Redis Cache kulcsok aaaAzure |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - Get hello állomásnév, portokat és a kulcsok Azure Redis Cache példányt"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: e6e794558087d6568438c439e2bf99fc46eeb8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-hostname-ports-and-keys-for-azure-redis-cache"></a><span data-ttu-id="5b325-103">Azure Redis Cache hello állomásnév, portokat és a kulcsok beszerzése</span><span class="sxs-lookup"><span data-stu-id="5b325-103">Get hello hostname, ports, and keys for Azure Redis Cache</span></span>

<span data-ttu-id="5b325-104">Ebben a forgatókönyvben a megismerheti, hogyan használja az tooretrieve hello állomásnév, portokat és a kulcsok tooconnect tooan Azure Redis Cache példány.</span><span class="sxs-lookup"><span data-stu-id="5b325-104">In this scenario, you learn how tooretrieve hello hostname, ports, and keys used tooconnect tooan Azure Redis Cache instance.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="5b325-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="5b325-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a><span data-ttu-id="5b325-106">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="5b325-106">Script explanation</span></span>

<span data-ttu-id="5b325-107">A parancsfájl a következő parancsok tooretrieve hello állomásnév, a kulcsok és a portok az Azure Redis Cache példány hello.</span><span class="sxs-lookup"><span data-stu-id="5b325-107">This script uses hello following commands tooretrieve hello hostname, keys, and ports of an Azure Redis Cache instance.</span></span> <span data-ttu-id="5b325-108">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5b325-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="5b325-109">Parancs</span><span class="sxs-lookup"><span data-stu-id="5b325-109">Command</span></span> | <span data-ttu-id="5b325-110">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5b325-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5b325-111">az redis megjelenítése</span><span class="sxs-lookup"><span data-stu-id="5b325-111">az redis show</span></span>](https://docs.microsoft.com/cli/azure/redis#show) | <span data-ttu-id="5b325-112">Beolvasni az Azure Redis Cache példány részleteit.</span><span class="sxs-lookup"><span data-stu-id="5b325-112">Retrieve details of an Azure Redis Cache instance.</span></span> |
| [<span data-ttu-id="5b325-113">az-redis-listázása</span><span class="sxs-lookup"><span data-stu-id="5b325-113">az redis list-keys</span></span>](https://docs.microsoft.com/cli/azure/redis#list-keys) | <span data-ttu-id="5b325-114">Azure Redis Cache példány elérési kulcsainak beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5b325-114">Retrieve access keys for an Azure Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="5b325-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b325-115">Next steps</span></span>

<span data-ttu-id="5b325-116">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5b325-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5b325-117">További Azure Redis Cache CLI parancsfájl minták hello található [Azure Redis Cache dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5b325-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
