---
title: "Az Azure PowerShell-parancsfájl minta - frissítés a Service Fabric-alkalmazás |} Microsoft Docs"
description: "Az Azure PowerShell-parancsfájl minta - frissítés a Service Fabric-alkalmazás."
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
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.openlocfilehash: 454849f82ddb23ddb9d71459f86e3cf5a1589254
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upgrade-a-service-fabric-application"></a>A Service Fabric-alkalmazás frissítése

Ez a parancsfájlpélda futó Service Fabric-alkalmazás példány 1.3.0 verzióra frissíti. A parancsfájl másolja az új alkalmazáscsomagot a fürt lemezképtárolóhoz, az alkalmazástípus regisztrálása, a figyelt frissítés elindul, és folyamatosan ellenőrzi a frissítési állapot, amíg nem fejeződik a frissítés, vagy visszaállítja a. A paraméterek testreszabása, igény szerint. 

Szükség esetén telepítse a Service Fabric PowerShell-modulját és a [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[fő](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "alkalmazás frissítése")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsokat. Minden egyes parancsa a tábla-parancs adott dokumentációjára mutató hivatkozásokat.

| Parancs | Megjegyzések |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Lekérdezi a Service Fabric-fürt összes alkalmazást vagy egy adott alkalmazáshoz.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | A Service Fabric-alkalmazás frissítés állapotát olvassa be. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | A Service Fabric a Service Fabric-fürt regisztrált típusok beolvasása. |
| [ServiceFabricApplicationType regisztrációjának törlése](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | A Service Fabric alkalmazástípus regisztrációjának törlése.  |
| [Másolás-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | A Service Fabric alkalmazáscsomagok másolja az image store.  |
| [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Regisztrálja a Service Fabric-alkalmazás típusa. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | A Service Fabric-alkalmazás a megadott alkalmazás típusa verzióra frissíti. |


## <a name="next-steps"></a>Következő lépések

További információ a Service Fabric PowerShell-modul: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric további Powershell-példák találhatók a [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
