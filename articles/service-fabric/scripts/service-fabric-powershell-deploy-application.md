---
title: "PowerShell parancsfájl minta - aaaAzure alkalmazás tooa fürt központi telepítése |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - alkalmazás tooa Service Fabric-fürt üzembe helyezése."
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
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: b417c9908c72f016e930c43ff2d13e0cc5451f46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-service-fabric-cluster"></a>Alkalmazás tooa Service Fabric-fürt üzembe helyezése

Ez a parancsfájlpélda másolja át az alkalmazás csomag tooa fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása hello fürt, és létrehoz egy alkalmazáspéldányt hello alkalmazás típusa.  Ha az alapértelmezett szolgáltatások hello célalkalmazás típusa hello alkalmazás jegyzékében meghatározott, akkor ezek a szolgáltatások jelenleg jönnek létre. Hello paraméterek testreszabása, igény szerint. 

Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application tooa cluster")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

Hello parancsfájl minta futtatása után a parancsfájl hello [alkalmazás eltávolítása](service-fabric-powershell-remove-application.md) is használt tooremove hello alkalmazáspéldány kell hello alkalmazástípus regisztrációjának törlése és hello alkalmazáscsomag törlése hello lemezképtárolóhoz.

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Másolás-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Másolja az alkalmazás csomag toohello fürt lemezképtárolóhoz.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Regisztrálja az alkalmazástípus és -verzió hello fürtön. |
|[Új ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Alkalmazást hoz létre a regisztrált alkalmazáshoz típusból. |

## <a name="next-steps"></a>Következő lépések

A Service Fabric PowerShell-modul hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
