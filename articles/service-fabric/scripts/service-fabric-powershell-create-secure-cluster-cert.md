---
title: "Az Azure PowerShell-parancsfájl minták – a Service Fabric-fürt létrehozása |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minták – a Service Fabric-fürt létrehozása."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0f9c8bc5-3789-4eb3-8deb-ae6e2200795a
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 7cbeb3da695af3815ba660f9cc2e3388abb6f87d
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="create-a-service-fabric-cluster"></a><span data-ttu-id="a8f8a-103">A Service Fabric-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a8f8a-103">Create a Service Fabric cluster</span></span>

<span data-ttu-id="a8f8a-104">Ez a parancsfájlpélda hoz létre a Service Fabric-fürt egy X.509 tanúsítvánnyal védett öt csomópontból álló fürt.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-104">This sample script creates a Service Fabric cluster a five-node cluster secured with an X.509 certificate.</span></span>  <span data-ttu-id="a8f8a-105">A parancs létrehoz egy önaláírt tanúsítványt, és feltölti azt egy új kulcstartó.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-105">The command creates a self-signed certificate and uploads it to a new key vault.</span></span> <span data-ttu-id="a8f8a-106">A tanúsítvány is másolja egy helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-106">The certificate is also copied to a local directory.</span></span>  <span data-ttu-id="a8f8a-107">Állítsa be a *-OS* paraméter a Windows vagy Linux rendszerű, a fürt csomópontjain futó verziójának kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-107">Set the *-OS* parameter to choose the version of Windows or Linux that runs on the cluster nodes.</span></span>  <span data-ttu-id="a8f8a-108">A paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-108">Customize the parameters as needed.</span></span>

<span data-ttu-id="a8f8a-109">Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-109">If needed, install the Azure PowerShell using the instruction found in the [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` to create a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="a8f8a-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="a8f8a-110">Sample script</span></span>

<span data-ttu-id="a8f8a-111">[!code-powershell[fő](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "a Service Fabric-fürt létrehozása")]</span><span class="sxs-lookup"><span data-stu-id="a8f8a-111">[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="a8f8a-112">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="a8f8a-112">Clean up deployment</span></span> 

<span data-ttu-id="a8f8a-113">A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoport, a fürt és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-113">After the script sample has been run, the following command can be used to remove the resource group, cluster, and all related resources.</span></span>

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="a8f8a-114">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="a8f8a-114">Script explanation</span></span>

<span data-ttu-id="a8f8a-115">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-115">This script uses the following commands.</span></span> <span data-ttu-id="a8f8a-116">Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-116">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="a8f8a-117">Parancs</span><span class="sxs-lookup"><span data-stu-id="a8f8a-117">Command</span></span> | <span data-ttu-id="a8f8a-118">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="a8f8a-118">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a8f8a-119">Új AzureRmServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="a8f8a-119">New-AzureRmServiceFabricCluster</span></span>](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | <span data-ttu-id="a8f8a-120">Egy új Service Fabric-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a8f8a-120">Creates a new Service Fabric cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a8f8a-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8f8a-121">Next steps</span></span>

<span data-ttu-id="a8f8a-122">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a8f8a-122">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="a8f8a-123">Azure Service Fabric további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a8f8a-123">Additional Azure Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
