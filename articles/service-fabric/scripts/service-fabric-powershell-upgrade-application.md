---
title: "PowerShell parancsfájl minta - aaaAzure frissítése a Service Fabric-alkalmazás |} Microsoft Docs"
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
ms.openlocfilehash: 4f4777607bd6b35a76029e09ddb441006565d4cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-a-service-fabric-application"></a>A Service Fabric-alkalmazás frissítése

Ez a parancsfájlpélda egy futó Service Fabric application példány tooversion 1.3.0 frissíti. hello parancsfájl hello új alkalmazás csomag toohello fürt lemezképtárolóhoz másolja, hello alkalmazástípus regisztrálása, a figyelt frissítés kezdődik, és folyamatosan ellenőrzi az hello frissítési állapot, amíg hello frissítése befejeződik, vagy visszaállítja a. Hello paraméterek testreszabása, igény szerint. 

Szükség esetén – hello Service Fabric PowerShell-modul telepítése a hello [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Mintaparancsfájl

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Parancsfájl ismertetése

A parancsfájl a következő parancsok hello. Minden egyes parancsa hello tábla hivatkozások toocommand adott dokumentációját.

| Parancs | Megjegyzések |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Lekérdezi a hello Service Fabric-fürt összes hello alkalmazást vagy egy adott alkalmazáshoz.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | A Service Fabric-alkalmazás frissítés hello állapotának beolvasása. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Lekérdezi a Service Fabric típusok hello hello Service Fabric-fürt regisztrált. |
| [ServiceFabricApplicationType regisztrációjának törlése](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | A Service Fabric alkalmazástípus regisztrációjának törlése.  |
| [Másolás-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Másolja a Service Fabric application toohello lemezképet tárolja.  |
| [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Regisztrálja a Service Fabric-alkalmazás típusa. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | A Service Fabric toohello megadott alkalmazás alkalmazástípus verziója frissíti. |


## <a name="next-steps"></a>Következő lépések

A Service Fabric PowerShell-modul hello további információkért lásd: [Azure PowerShell dokumentációs](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric további Powershell-példák találhatók hello [Azure PowerShell-példák](../service-fabric-powershell-samples.md).
