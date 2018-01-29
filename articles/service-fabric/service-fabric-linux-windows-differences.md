---
title: "Különbségek az Azure Service Fabric Linux- és Windows-verziója között | Microsoft Docs"
description: "Az Azure Service Fabric Linux- és Windows-verziója közötti különbségek."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/19/2017
ms.author: subramar
ms.openlocfilehash: 25976ba919454e26f1dd7965de5db7c4f80b9355
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="differences-between-service-fabric-on-linux-and-windows"></a>A Service Fabric Linux és Windows rendszerhez készült verziója közötti különbségek

Néhány szolgáltatás csak Windows rendszeren támogatott, Linuxon nem. Idővel ugyanazok a szolgáltatások lesznek elérhetőek, és minden újabb kiadással egyre csökkennek a szolgáltatásbeli különbségek. A legújabb elérhető kiadások (Windows rendszeren 6.0, Linuxon 6.0) között az alábbi eltérések állnak fenn: 

* Minden programozási modell előzetes verzióként érhető el (Java/C# Reliable Actors, Reliable Stateless Services és Reliable Stateful Services)
* Az Envoy (ReverseProxy) előzetes verzióként érhető el Linuxon
* Önálló Linuxos telepítő még nem érhető el Linux rendszeren.
* Konzolátirányítás (nem támogatott az éles fürtökben sem Linux, sem Windows rendszeren)
* A Fault Analysis Service (FAS) Linuxon
* DNS szolgáltatás a Service Fabric-szolgáltatásokhoz (a DNS szolgáltatás a tárolók esetében támogatott a Linuxon)
* Egyes PowerShell-parancsok parancssori felületi megfelelője (lásd az alábbi listát; a parancsok nagy része csak önálló fürtökre vonatkozik)

A fejlesztői eszközök eltérnek Windows és Linux rendszeren. Windows rendszeren a Visual Studio, a PowerShell, a VSTS és az ETW, míg Linuxon a Yeoman, az Eclipse, a Jenkins és az LTTng érhető el.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>PowerShell-parancsmagok, amelyek nem működnek Linux rendszerű Service Fabric-fürtökön

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Következő lépések
* [A fejlesztőkörnyezet előkészítése Linuxon](service-fabric-get-started-linux.md)
* [A fejlesztőkörnyezet előkészítése OSX-en](service-fabric-get-started-mac.md)
* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren Yeoman használatával](service-fabric-create-your-first-linux-application-with-java.md)
* [Az első Service Fabric Java-alkalmazás létrehozása és üzembe helyezése Linux rendszeren az Eclipse Service Fabric beépülő modul használatával](service-fabric-get-started-eclipse.md)
* [Az első CSharp-alkalmazás létrehozása Linuxon](service-fabric-create-your-first-linux-application-with-csharp.md)
* [A Service Fabric parancssori felület használata az alkalmazások kezeléséhez](service-fabric-application-lifecycle-sfctl.md)
