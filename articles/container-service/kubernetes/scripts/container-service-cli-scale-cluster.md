---
title: "Az Azure CLI parancsfájl minta - skálázási egy ACS-fürthöz |} Microsoft Docs"
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
ms.openlocfilehash: 14e9f9d85bc0c1428240f15831632eafe2a0f80e
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="scale-an-azure-container-service-cluster"></a><span data-ttu-id="de6a2-104">Egy Azure Tárolószolgáltatási fürt méretezése</span><span class="sxs-lookup"><span data-stu-id="de6a2-104">Scale an Azure Container Service Cluster</span></span>

<span data-ttu-id="de6a2-105">Ez a minta méretekkel és az Azure Tárolószolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="de6a2-105">This sample scales and Azure Container Service.</span></span> 

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="de6a2-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="de6a2-106">Sample script</span></span>

```azurecli
az acs scale --resource-group myResourceGroup --name myK8SCluster --new-agent-count 5
```

## <a name="clean-up-deployment"></a><span data-ttu-id="de6a2-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="de6a2-107">Clean up deployment</span></span> 

<span data-ttu-id="de6a2-108">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="de6a2-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="de6a2-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="de6a2-109">Script explanation</span></span>

<span data-ttu-id="de6a2-110">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="de6a2-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="de6a2-111">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="de6a2-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="de6a2-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="de6a2-112">Command</span></span> | <span data-ttu-id="de6a2-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="de6a2-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="de6a2-114">az acs-skála</span><span class="sxs-lookup"><span data-stu-id="de6a2-114">az acs scale</span></span>](/cli/azure/acs#scale) | <span data-ttu-id="de6a2-115">Az ACS-fürt méretezése.</span><span class="sxs-lookup"><span data-stu-id="de6a2-115">Scale an ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="de6a2-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de6a2-116">Next steps</span></span>

<span data-ttu-id="de6a2-117">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="de6a2-117">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="de6a2-118">További Azure-tároló szolgáltatás CLI parancsfájl minták megtalálhatók a [Azure Tárolószolgáltatás-dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="de6a2-118">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>

