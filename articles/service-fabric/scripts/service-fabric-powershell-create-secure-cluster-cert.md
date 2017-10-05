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
# <a name="create-a-service-fabric-cluster"></a>A Service Fabric-fürt létrehozása

Ez a parancsfájlpélda hoz létre a Service Fabric-fürt egy X.509 tanúsítvánnyal védett öt csomópontból álló fürt.  A parancs létrehoz egy önaláírt tanúsítványt, és feltölti azt egy új kulcstartó. A tanúsítvány is másolja egy helyi könyvtárba.  Állítsa be a *-OS* paraméter a Windows vagy Linux rendszerű, a fürt csomópontjain futó verziójának kiválasztása.  A paraméterek testreszabása, igény szerint.

Szükség esetén telepítse az Azure PowerShell található utasítás használatával a [Azure PowerShell útmutató](/powershell/azure/overview) , majd futtassa `Login-AzureRmAccount` kapcsolat létrehozása az Azure-ral. 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/service-fabric/create-secure-cluster/create-secure-cluster.ps1 "a Service Fabric-fürt létrehozása")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

A parancsfájl-minta futtatása után a következő parancs segítségével távolítsa el az erőforráscsoport, a fürt és az összes kapcsolódó erőforrások.

```powershell
$groupname="mysfclustergroup"
Remove-AzureRmResourceGroup -Name $groupname -Force
```

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Új AzureRmServiceFabricCluster](/powershell/module/azurerm.servicefabric/New-AzureRmServiceFabricCluster) | Egy új Service Fabric-fürtöt hoz létre. |

## <a name="next-steps"></a>Következő lépések

Az Azure PowerShell modul további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/overview).

Azure Service Fabric további Azure Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
