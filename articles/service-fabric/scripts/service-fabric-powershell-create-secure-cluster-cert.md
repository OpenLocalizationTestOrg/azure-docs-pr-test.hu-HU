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
# <a name="create-a-service-fabric-cluster"></a>A Service Fabric-fürt létrehozása

Ez a parancsfájlpélda hoz létre a Service Fabric-fürt egy X.509 tanúsítvánnyal védett öt csomópontból álló fürt.  hello parancs létrehoz egy önaláírt tanúsítványt, és feltölti azt tooa új kulcstároló. hello tanúsítvány egyben másolt tooa helyi könyvtárba.  Set hello *-OS* Windows vagy Linux hello fürtcsomóponton futó paraméter toochoose hello verziója.  Hello paraméterek testreszabása, igény szerint.

Szükség esetén telepítse az Azure PowerShell használatával hello utasítás található hello hello [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` toocreate Azure kapcsolatot. 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "Create a Service Fabric cluster")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Hello parancsfájl minta futtatása után a következő parancs hello lehet használt tooremove hello erőforráscsoport, a fürt és az összes kapcsolódó erőforrások.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Egy új Service Fabric-fürtöt hoz létre. |

## <a name="next-steps"></a>Következő lépések

Hello Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

Azure Service Fabric további Azure Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
