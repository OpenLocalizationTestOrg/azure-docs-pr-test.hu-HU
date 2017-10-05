---
title: "Az Azure CLI parancsfájl-mintában - ACS Windows Kubernetes fürt létrehozása |} Microsoft Docs"
description: "Az Azure CLI parancsfájl-mintában - ACS Windows Kubernetes fürt létrehozása"
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
ms.openlocfilehash: 3cba915e3cf3aaaeb3faf14c2000ca94f61d28a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-container-service-kubernetes-windows-cluster"></a><span data-ttu-id="ae755-104">Hozzon létre egy Kubernetes Windows Azure Tárolószolgáltatási fürt</span><span class="sxs-lookup"><span data-stu-id="ae755-104">Create an Azure Container Service Kubernetes Windows Cluster</span></span>

<span data-ttu-id="ae755-105">Ez a minta futó Kubernetes a Windows-alapú tárolók egy Azure Tárolószolgáltatás-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ae755-105">This sample creates an Azure Container Service cluster running Kubernetes for Windows based containers.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ae755-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ae755-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type kubernetes \
  --resource-group myResourceGroup \
  --name myK8SCluster \
  --generate-ssh-keys \
  --admin-username azureuser \
  --admin-password Password12 \
  --windows
```

## <a name="clean-up-deployment"></a><span data-ttu-id="ae755-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ae755-107">Clean up deployment</span></span> 

<span data-ttu-id="ae755-108">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="ae755-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ae755-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ae755-109">Script explanation</span></span>

<span data-ttu-id="ae755-110">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ae755-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="ae755-111">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="ae755-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ae755-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="ae755-112">Command</span></span> | <span data-ttu-id="ae755-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ae755-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ae755-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae755-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ae755-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ae755-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ae755-116">az acs létrehozása</span><span class="sxs-lookup"><span data-stu-id="ae755-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="ae755-117">Létrehozza és az ACS-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ae755-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ae755-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ae755-118">Next steps</span></span>

<span data-ttu-id="ae755-119">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ae755-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ae755-120">További Azure-tároló szolgáltatás CLI parancsfájl minták megtalálhatók a [Azure Tárolószolgáltatás-dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ae755-120">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>
