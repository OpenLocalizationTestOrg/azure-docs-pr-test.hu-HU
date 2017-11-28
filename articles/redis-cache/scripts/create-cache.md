---
title: "CLI parancsfájl minta - aaaAzure hozzon létre egy Azure Redis Cache |} Microsoft Docs"
description: "Az Azure CLI parancsfájl-mintában – az Azure Redis gyorsítótár létrehozása"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: afd7f6e0-9297-4c98-a95e-597be939cef7
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: 85b007a426fbd4752034ec8663835963d140dd75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-redis-cache"></a><span data-ttu-id="f40dc-103">Azure Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="f40dc-103">Create an Azure Redis Cache</span></span>

<span data-ttu-id="f40dc-104">Ebben a forgatókönyvben a megismerheti, hogyan toocreate az Azure Redis Cache-gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="f40dc-104">In this scenario, you learn how toocreate an Azure Redis Cache.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="f40dc-105">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="f40dc-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/redis-cache/create-cache/create-cache.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/redis-cli-script-clean-up.md)]

## <a name="script-explanation"></a><span data-ttu-id="f40dc-106">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="f40dc-106">Script explanation</span></span>

<span data-ttu-id="f40dc-107">A parancsfájl a következő parancsok toocreate hello, az erőforráscsoportot és a redis gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="f40dc-107">This script uses hello following commands toocreate a resource group and a redis cache.</span></span> <span data-ttu-id="f40dc-108">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="f40dc-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f40dc-109">Parancs</span><span class="sxs-lookup"><span data-stu-id="f40dc-109">Command</span></span> | <span data-ttu-id="f40dc-110">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f40dc-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f40dc-111">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="f40dc-111">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="f40dc-112">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f40dc-112">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="f40dc-113">az redis létrehozása</span><span class="sxs-lookup"><span data-stu-id="f40dc-113">az redis create</span></span>](https://docs.microsoft.com/cli/azure/redis#create) | <span data-ttu-id="f40dc-114">Hozzon létre a Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="f40dc-114">Create Redis Cache instance.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="f40dc-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f40dc-115">Next steps</span></span>

<span data-ttu-id="f40dc-116">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f40dc-116">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="f40dc-117">További Azure Redis Cache CLI parancsfájl minták hello található [Azure Redis Cache dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="f40dc-117">Additional Azure Redis Cache CLI script samples can be found in hello [Azure Redis Cache documentation](../cli-samples.md).</span></span>
