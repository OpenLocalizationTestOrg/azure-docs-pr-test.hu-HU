---
title: "CLI parancsfájl minta - aaaAzure méretezése egy ACS-fürthöz |} Microsoft Docs"
description: "Az Azure CLI parancsfájl minta - skálázási egy ACS-fürthöz"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/30/2017
ms.author: nepeters
ms.openlocfilehash: 1e07518fc2ca67476d9ef64bb22d75f848a37e43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="9a0ba-104">Egy Azure Tárolószolgáltatási fürt méretezése</span><span class="sxs-lookup"><span data-stu-id="9a0ba-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="9a0ba-105">Ez a minta méretekkel és az Azure Tárolószolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9a0ba-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="9a0ba-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9a0ba-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="9a0ba-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9a0ba-107">Clean up deployment</span></span> 

<span data-ttu-id="9a0ba-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="9a0ba-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="9a0ba-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9a0ba-109">Script explanation</span></span>

<span data-ttu-id="9a0ba-110">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="9a0ba-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="9a0ba-111">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="9a0ba-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="9a0ba-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="9a0ba-112">Command</span></span> | <span data-ttu-id="9a0ba-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9a0ba-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9a0ba-114">az acs-skála</span><span class="sxs-lookup"><span data-stu-id="9a0ba-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="9a0ba-115">Az ACS-fürt méretezése.</span><span class="sxs-lookup"><span data-stu-id="9a0ba-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9a0ba-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a0ba-116">Next steps</span></span>

<span data-ttu-id="9a0ba-117">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9a0ba-117">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="9a0ba-118">További Azure-tároló szolgáltatás CLI parancsfájl minták hello található [Azure Tárolószolgáltatás-dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9a0ba-118">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>

