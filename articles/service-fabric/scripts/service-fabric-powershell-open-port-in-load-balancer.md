---
title: "Az Azure PowerShell-parancsfájl minta - megnyitása port a terheléselosztó |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - nyissa meg a Service Fabric-alkalmazás az Azure load balancer egy portot."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 2958bdef0889076249918608c04c66678fa80b97
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a><span data-ttu-id="9da2a-103">Nyissa meg az Azure load balancer egy alkalmazás port</span><span class="sxs-lookup"><span data-stu-id="9da2a-103">Open an application port in the Azure load balancer</span></span>

<span data-ttu-id="9da2a-104">Egy Azure-ban futó Service Fabric-alkalmazás az Azure terheléselosztó mögött helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="9da2a-104">A Service Fabric application running in Azure sits behind the Azure load balancer.</span></span> <span data-ttu-id="9da2a-105">Ez a parancsfájlpélda egy portot nyit meg egy Azure terheléselosztó a, hogy a Service Fabric-alkalmazás olyan külső ügyfelek kommunikálhassanak.</span><span class="sxs-lookup"><span data-stu-id="9da2a-105">This sample script opens a port in an Azure load balancer so that a Service Fabric application can communicate with external clients.</span></span> <span data-ttu-id="9da2a-106">A paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="9da2a-106">Customize the parameters as needed.</span></span> 

<span data-ttu-id="9da2a-107">Szükség esetén telepítse a Service Fabric PowerShell-modulját és a [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9da2a-107">If needed, install the Service Fabric PowerShell module with the [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="9da2a-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="9da2a-108">Sample script</span></span>

<span data-ttu-id="9da2a-109">[!code-powershell[fő](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "port megnyitása a terheléselosztó")]</span><span class="sxs-lookup"><span data-stu-id="9da2a-109">[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="9da2a-110">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="9da2a-110">Script explanation</span></span>

<span data-ttu-id="9da2a-111">A parancsfájl a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="9da2a-111">This script uses the following commands.</span></span> <span data-ttu-id="9da2a-112">Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="9da2a-112">Each command in the table links to command-specific documentation.</span></span>

| <span data-ttu-id="9da2a-113">Parancs</span><span class="sxs-lookup"><span data-stu-id="9da2a-113">Command</span></span> | <span data-ttu-id="9da2a-114">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9da2a-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="9da2a-115">Get-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="9da2a-115">Get-AzureRmResource</span></span>](/powershell/module/azurerm.resources/get-azurermresource) | <span data-ttu-id="9da2a-116">Egy Azure-erőforrás lekérése.</span><span class="sxs-lookup"><span data-stu-id="9da2a-116">Gets an Azure resource.</span></span>  |
| [<span data-ttu-id="9da2a-117">Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="9da2a-117">Get-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancer) | <span data-ttu-id="9da2a-118">Az Azure load balancer lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="9da2a-118">Gets the Azure load balancer.</span></span> |
| [<span data-ttu-id="9da2a-119">Adja hozzá AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="9da2a-119">Add-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | <span data-ttu-id="9da2a-120">A mintavétel konfiguráció hozzáadása a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="9da2a-120">Adds a probe configuration to a load balancer.</span></span>|
| [<span data-ttu-id="9da2a-121">Get-AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="9da2a-121">Get-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | <span data-ttu-id="9da2a-122">Lekérdezi a terheléselosztó mintavételi konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="9da2a-122">Gets a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="9da2a-123">Adja hozzá AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="9da2a-123">Add-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | <span data-ttu-id="9da2a-124">A szabály konfigurálását ad hozzá egy adott terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="9da2a-124">Adds a rule configuration to a load balancer.</span></span> |
| [<span data-ttu-id="9da2a-125">Set-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="9da2a-125">Set-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/set-azurermloadbalancer) | <span data-ttu-id="9da2a-126">A cél állapotának beállítása a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="9da2a-126">Sets the goal state for a load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9da2a-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9da2a-127">Next steps</span></span>

<span data-ttu-id="9da2a-128">Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9da2a-128">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="9da2a-129">Azure Service Fabric további Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="9da2a-129">Additional Powershell samples for Azure Service Fabric can be found in the [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
