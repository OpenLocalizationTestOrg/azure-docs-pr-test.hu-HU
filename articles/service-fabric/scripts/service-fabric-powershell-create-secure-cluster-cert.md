---
title: "PowerShell parancsfájl minta - aaaAzure létrehozása a Service Fabric-fürt |} Microsoft Docs"
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
ms.openlocfilehash: 12fdc201bd51688cb850cd456b1e00442b79c22d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster"></a><span data-ttu-id="b35e7-103">A Service Fabric-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b35e7-103">Create a Service Fabric cluster</span></span>

<span data-ttu-id="b35e7-104">Ez a parancsfájlpélda hoz létre a Service Fabric-fürt egy X.509 tanúsítvánnyal védett öt csomópontból álló fürt.</span><span class="sxs-lookup"><span data-stu-id="b35e7-104">This sample script creates a Service Fabric cluster a five-node cluster secured with an X.509 certificate.</span></span>  <span data-ttu-id="b35e7-105">hello parancs létrehoz egy önaláírt tanúsítványt, és feltölti azt tooa új kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="b35e7-105">hello command creates a self-signed certificate and uploads it tooa new key vault.</span></span> <span data-ttu-id="b35e7-106">hello tanúsítvány egyben másolt tooa helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="b35e7-106">hello certificate is also copied tooa local directory.</span></span>  <span data-ttu-id="b35e7-107">Set hello *-OS* Windows vagy Linux hello fürtcsomóponton futó paraméter toochoose hello verziója.</span><span class="sxs-lookup"><span data-stu-id="b35e7-107">Set hello *-OS* parameter toochoose hello version of Windows or Linux that runs on hello cluster nodes.</span></span>  <span data-ttu-id="b35e7-108">Hello paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="b35e7-108">Customize hello parameters as needed.</span></span>

<span data-ttu-id="b35e7-109">Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="b35e7-109">If needed, install hello Azure PowerShell using hello instruction found in hello [Azure PowerShell guide](/powershell/azure/overview) and then run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="b35e7-110">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="b35e7-110">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a><span data-ttu-id="b35e7-111">Az üzemelő példány eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b35e7-111">Clean up deployment</span></span> 

<span data-ttu-id="b35e7-112">Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a fürt és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b35e7-112">After hello script sample has been run, hello following command can be used tooremove hello resource group, cluster, and all related resources.</span></span>

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a><span data-ttu-id="b35e7-113">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="b35e7-113">Script explanation</span></span>

<span data-ttu-id="b35e7-114">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="b35e7-114">This script uses hello following commands.</span></span> <span data-ttu-id="b35e7-115">Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="b35e7-115">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="b35e7-116">Parancs</span><span class="sxs-lookup"><span data-stu-id="b35e7-116">Command</span></span> | <span data-ttu-id="b35e7-117">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="b35e7-117">Notes</span></span> |
|---|---|
| [<span data-ttu-id="b35e7-118">Új AzureRmServiceFabricCluster</span><span class="sxs-lookup"><span data-stu-id="b35e7-118">New-AzureRmServiceFabricCluster</span></span>](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | <span data-ttu-id="b35e7-119">Egy új Service Fabric-fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b35e7-119">Creates a new Service Fabric cluster.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="b35e7-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b35e7-120">Next steps</span></span>

<span data-ttu-id="b35e7-121">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="b35e7-121">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="b35e7-122">Azure Service Fabric további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b35e7-122">Additional Azure Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
