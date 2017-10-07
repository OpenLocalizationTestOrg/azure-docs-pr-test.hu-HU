---
title: "aaaAzure CLI parancsfájl minta - ACS Windows Kubernetes fürt létrehozása |} Microsoft Docs"
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
ms.openlocfilehash: ace2f7a6dcd3ab02b61217766f4774cddbe8828b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-container-service-kubernetes-windows-cluster"></a><span data-ttu-id="ef65f-104">Hozzon létre egy Kubernetes Windows Azure Tárolószolgáltatási fürt</span><span class="sxs-lookup"><span data-stu-id="ef65f-104">Create an Azure Container Service Kubernetes Windows Cluster</span></span>

<span data-ttu-id="ef65f-105">Ez a minta futó Kubernetes a Windows-alapú tárolók egy Azure Tárolószolgáltatás-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ef65f-105">This sample creates an Azure Container Service cluster running Kubernetes for Windows based containers.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ef65f-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="ef65f-106">Sample script</span></span>

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

## <a name="clean-up-deployment"></a><span data-ttu-id="ef65f-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="ef65f-107">Clean up deployment</span></span> 

<span data-ttu-id="ef65f-108">Futtassa a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="ef65f-108">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="ef65f-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="ef65f-109">Script explanation</span></span>

<span data-ttu-id="ef65f-110">A parancsfájl a következő parancsok toocreate hello telepítési hello.</span><span class="sxs-lookup"><span data-stu-id="ef65f-110">This script uses hello following commands toocreate hello deployment.</span></span> <span data-ttu-id="ef65f-111">Minden elem hello tábla hivatkozások toocommand adott dokumentációjában.</span><span class="sxs-lookup"><span data-stu-id="ef65f-111">Each item in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="ef65f-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="ef65f-112">Command</span></span> | <span data-ttu-id="ef65f-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="ef65f-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ef65f-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef65f-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="ef65f-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ef65f-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="ef65f-116">az acs létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef65f-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="ef65f-117">Létrehozza és az ACS-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="ef65f-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ef65f-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef65f-118">Next steps</span></span>

<span data-ttu-id="ef65f-119">Az Azure CLI hello további információkért lásd: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ef65f-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ef65f-120">További Azure-tároló szolgáltatás CLI parancsfájl minták hello található [Azure Tárolószolgáltatás-dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="ef65f-120">Additional Azure Container Service CLI script samples can be found in hello [Azure Container Service documentation](../cli-samples.md).</span></span>
