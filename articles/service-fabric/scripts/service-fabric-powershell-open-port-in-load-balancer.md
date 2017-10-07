---
title: "PowerShell parancsfájl minta - megnyitása port a terheléselosztó aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - nyissa meg a hello Azure terheléselosztó a Service Fabric-alkalmazás egy portot."
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
ms.openlocfilehash: 6acb28942851dce63f89f7de362b7cf1dc7b1fee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-an-application-port-in-hello-azure-load-balancer"></a><span data-ttu-id="3c07a-103">Nyissa meg az alkalmazás port hello Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="3c07a-103">Open an application port in hello Azure load balancer</span></span>

<span data-ttu-id="3c07a-104">Egy Azure-ban futó Service Fabric-alkalmazás hello Azure terheléselosztó mögött helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="3c07a-104">A Service Fabric application running in Azure sits behind hello Azure load balancer.</span></span> <span data-ttu-id="3c07a-105">Ez a parancsfájlpélda egy portot nyit meg egy Azure terheléselosztó a, hogy a Service Fabric-alkalmazás olyan külső ügyfelek kommunikálhassanak.</span><span class="sxs-lookup"><span data-stu-id="3c07a-105">This sample script opens a port in an Azure load balancer so that a Service Fabric application can communicate with external clients.</span></span> <span data-ttu-id="3c07a-106">Hello paraméterek testreszabása, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="3c07a-106">Customize hello parameters as needed.</span></span> 

<span data-ttu-id="3c07a-107">Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3c07a-107">If needed, install hello Service Fabric PowerShell module with hello [Service Fabric SDK](../service-fabric-get-started.md).</span></span> 

## <a name="sample-script"></a><span data-ttu-id="3c07a-108">Mintaparancsfájl</span><span class="sxs-lookup"><span data-stu-id="3c07a-108">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in hello load balancer")]

## <a name="script-explanation"></a><span data-ttu-id="3c07a-109">Parancsfájl ismertetése</span><span class="sxs-lookup"><span data-stu-id="3c07a-109">Script explanation</span></span>

<span data-ttu-id="3c07a-110">A parancsfájl a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="3c07a-110">This script uses hello following commands.</span></span> <span data-ttu-id="3c07a-111">Minden egyes parancsa hello tábla hivatkozások toocommand vonatkozó dokumentációt.</span><span class="sxs-lookup"><span data-stu-id="3c07a-111">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="3c07a-112">Parancs</span><span class="sxs-lookup"><span data-stu-id="3c07a-112">Command</span></span> | <span data-ttu-id="3c07a-113">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3c07a-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3c07a-114">Get-AzureRmResource</span><span class="sxs-lookup"><span data-stu-id="3c07a-114">Get-AzureRmResource</span></span>](/powershell/module/azurerm.resources/get-azurermresource) | <span data-ttu-id="3c07a-115">Egy Azure-erőforrás lekérése.</span><span class="sxs-lookup"><span data-stu-id="3c07a-115">Gets an Azure resource.</span></span>  |
| [<span data-ttu-id="3c07a-116">Get-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="3c07a-116">Get-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancer) | <span data-ttu-id="3c07a-117">Lekérdezi a hello Azure terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="3c07a-117">Gets hello Azure load balancer.</span></span> |
| [<span data-ttu-id="3c07a-118">Adja hozzá AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="3c07a-118">Add-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | <span data-ttu-id="3c07a-119">A mintavételi konfigurációs tooa terheléselosztó hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3c07a-119">Adds a probe configuration tooa load balancer.</span></span>|
| [<span data-ttu-id="3c07a-120">Get-AzureRmLoadBalancerProbeConfig</span><span class="sxs-lookup"><span data-stu-id="3c07a-120">Get-AzureRmLoadBalancerProbeConfig</span></span>](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | <span data-ttu-id="3c07a-121">Lekérdezi a terheléselosztó mintavételi konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="3c07a-121">Gets a probe configuration for a load balancer.</span></span> |
| [<span data-ttu-id="3c07a-122">Adja hozzá AzureRmLoadBalancerRuleConfig</span><span class="sxs-lookup"><span data-stu-id="3c07a-122">Add-AzureRmLoadBalancerRuleConfig</span></span>](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | <span data-ttu-id="3c07a-123">A szabály konfigurációs tooa terheléselosztó hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3c07a-123">Adds a rule configuration tooa load balancer.</span></span> |
| [<span data-ttu-id="3c07a-124">Set-AzureRmLoadBalancer</span><span class="sxs-lookup"><span data-stu-id="3c07a-124">Set-AzureRmLoadBalancer</span></span>](/powershell/module/azurerm.network/set-azurermloadbalancer) | <span data-ttu-id="3c07a-125">Készletek hello terheléselosztó cél állapotát.</span><span class="sxs-lookup"><span data-stu-id="3c07a-125">Sets hello goal state for a load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3c07a-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c07a-126">Next steps</span></span>

<span data-ttu-id="3c07a-127">Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3c07a-127">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="3c07a-128">Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="3c07a-128">Additional Powershell samples for Azure Service Fabric can be found in hello [Azure PowerShell samples](../service-fabric-powershell-samples.md).</span></span>
