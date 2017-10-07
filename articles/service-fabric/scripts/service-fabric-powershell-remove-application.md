---
title: "PowerShell parancsfájl minta - fürtök törlése alkalmazás aaaAzure |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - alkalmazás a Service Fabric-fürt eltávolítása."
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
ms.openlocfilehash: 3fe2082c2fbeffbff1622f206021d4d907197d19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Alkalmazások eltávolítása a Service Fabric-fürt

Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány törlése megszünteti az alkalmazástípus és -verzió hello fürtről és hello alkalmazáscsomag törlése hello fürt lemezképtárolóhoz.  Törlése hello alkalmazáspéldány fut az adott alkalmazáshoz kapcsolódó szolgáltatáspéldány összes hello is törli. Hello paraméterek testreszabása, igény szerint. 

Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Eltávolítja egy futó Service Fabric-alkalmazás példány hello fürtből.  |
| [ServiceFabricApplicationType regisztrációjának törlése](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Megszünteti a Service Fabric alkalmazástípus és -verzió hello fürtből. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | A Service Fabric alkalmazáscsomag eltávolítása hello lemezképtárolóhoz.|

## <a name="next-steps"></a>Következő lépések

A Service Fabric PowerShell-modul hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
