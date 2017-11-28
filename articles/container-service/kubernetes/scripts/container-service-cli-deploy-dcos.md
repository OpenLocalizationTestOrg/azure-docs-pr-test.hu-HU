---
title: "Az Azure CLI parancsfájl-mintában - ACS DC/OS-fürt létrehozása |} Microsoft Docs"
description: "Az Azure CLI parancsfájl-mintában - ACS DC/OS-fürt létrehozása"
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
ms.openlocfilehash: ff90aee308a993ae0d36288191d1496affacce2a
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="create-an-azure-container-service-dcos-cluster"></a><span data-ttu-id="62262-104">Egy Azure Tárolószolgáltatási DC/OS-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="62262-104">Create an Azure Container Service DC/OS Cluster</span></span>

<span data-ttu-id="62262-105">Ez a minta egy Vezénylőtípusú futó Azure Tárolószolgáltatás-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="62262-105">This sample creates an Azure Container Service cluster running DCOS.</span></span>

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="62262-106">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="62262-106">Sample script</span></span>

```azurecli
az group create --name myResourceGroup --location eastus

az acs create \
  --orchestrator-type dcos \
  --resource-group myResourceGroup \
  --name myDCOSCluster \
  --generate-ssh-keys
```

## <a name="clean-up-deployment"></a><span data-ttu-id="62262-107">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="62262-107">Clean up deployment</span></span> 

<span data-ttu-id="62262-108">A következő parancsot az erőforráscsoport, virtuális gép és az összes kapcsolódó erőforrások eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="62262-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="62262-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="62262-109">Script explanation</span></span>

<span data-ttu-id="62262-110">A parancsfájl a következő parancsokat a központi telepítés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="62262-110">This script uses the following commands to create the deployment.</span></span> <span data-ttu-id="62262-111">A parancs adott dokumentáció tábla mutató összes elemére.</span><span class="sxs-lookup"><span data-stu-id="62262-111">Each item in the table links to command specific documentation.</span></span>

| <span data-ttu-id="62262-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="62262-112">Command</span></span> | <span data-ttu-id="62262-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="62262-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="62262-114">az csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="62262-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="62262-115">Az összes erőforrás tároló erőforrás csoportot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="62262-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="62262-116">az acs létrehozása</span><span class="sxs-lookup"><span data-stu-id="62262-116">az acs create</span></span>](https://docs.microsoft.com/cli/azure/acs#create) | <span data-ttu-id="62262-117">Létrehozza és az ACS-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="62262-117">Creates and ACS cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="62262-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62262-118">Next steps</span></span>

<span data-ttu-id="62262-119">További információ az Azure parancssori felület: [Azure CLI dokumentáció](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="62262-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="62262-120">További Azure-tároló szolgáltatás CLI parancsfájl minták megtalálhatók a [Azure Tárolószolgáltatás-dokumentáció](../cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="62262-120">Additional Azure Container Service CLI script samples can be found in the [Azure Container Service documentation](../cli-samples.md).</span></span>