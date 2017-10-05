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
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Nyissa meg az Azure load balancer egy alkalmazás port

Egy Azure-ban futó Service Fabric-alkalmazás az Azure terheléselosztó mögött helyezkedik el. Ez a parancsfájlpélda egy portot nyit meg egy Azure terheléselosztó a, hogy a Service Fabric-alkalmazás olyan külső ügyfelek kommunikálhassanak. A paraméterek testreszabása, igény szerint. 

Szükség esetén telepítse a Service Fabric PowerShell-modulját és a [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "port megnyitása a terheléselosztó")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs-specifikus dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource) | Egy Azure-erőforrás lekérése.  |
| [Get-AzureRmLoadBalancer](/powershell/module/azurerm.network/get-azurermloadbalancer) | Az Azure load balancer lekérdezi. |
| [Adja hozzá AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/add-azurermloadbalancerprobeconfig) | A mintavétel konfiguráció hozzáadása a terheléselosztóhoz.|
| [Get-AzureRmLoadBalancerProbeConfig](/powershell/module/azurerm.network/get-azurermloadbalancerprobeconfig) | Lekérdezi a terheléselosztó mintavételi konfigurációját. |
| [Adja hozzá AzureRmLoadBalancerRuleConfig](/powershell/module/azurerm.network/add-azurermloadbalancerruleconfig) | A szabály konfigurálását ad hozzá egy adott terheléselosztóhoz. |
| [Set-AzureRmLoadBalancer](/powershell/module/azurerm.network/set-azurermloadbalancer) | A cél állapotának beállítása a terheléselosztóhoz. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

Azure Service Fabric további Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
