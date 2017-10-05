---
title: "Az Azure PowerShell-parancsfájl minta - fürtre alkalmazás központi telepítése |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - alkalmazás üzembe helyezése a Service Fabric-fürt."
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
ms.openlocfilehash: 2863823205dbd70f63948ecd4af8898220fe1ff8
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>A Service Fabric-fürt alkalmazás központi telepítése

Ez a parancsfájlpélda másolja egy alkalmazáscsomagot a fürt lemezképtárolóhoz, az alkalmazástípus regisztrálása a fürtben, és létrehoz egy alkalmazáspéldányt az alkalmazástípus.  Ha a célalkalmazás típusa, az alkalmazás jegyzékében meghatározott alapértelmezett szolgáltatások, akkor ezek a szolgáltatások jelenleg jönnek létre. A paraméterek testreszabása, igény szerint. 

Szükség esetén telepítse a Service Fabric PowerShell-modulját és a [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "fürtre alkalmazás központi telepítése")]

## <a name="clean-up-deployment"></a>Az üzemelő példány eltávolítása 

A parancsfájl-minta futtatása után, a parancsfájl [alkalmazás eltávolítása](service-fabric-powershell-remove-application.md) az alkalmazáspéldány eltávolítása az alkalmazástípus regisztrációjának törlése és az alkalmazáscsomag törlése az image store használható.

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Másolás-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Másolja egy alkalmazáscsomagot a fürt lemezképtárolóhoz.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Regisztrálja az alkalmazástípus és -verzió a fürtön. |
|[Új ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Alkalmazást hoz létre a regisztrált alkalmazáshoz típusból. |

## <a name="next-steps"></a>Következő lépések

További információ a Service Fabric PowerShell-modul: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric további Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
