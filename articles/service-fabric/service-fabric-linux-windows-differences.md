---
title: "a Service Fabric aaaAzure különbségek attól függnek, Linux és a Windows között |} Microsoft Docs"
description: "Hello Azure Service Fabric előnézete a Linux és a Windows Azure Service Fabric közötti különbségeket."
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
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: 7a16a440dfc8d9006e274f46951be1562e6f10d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>A Service Fabric Linux (előzetes verziójú) és Windows (általánosan elérhető) rendszerhez készült verziója közötti különbségek

Mivel a Service Fabric Linux rendszeren még előzetes verzióban van, ezért néhány szolgáltatás csak Windows rendszeren támogatott, Linuxon nem. Végül hello szolgáltatáskészletek lesz paritásos amikor Linux Service Fabric általánosan elérhetővé válik. A jövőbeli kiadásokban a funkciók eltérései egyre csekélyebbé válnak. hello következő különbségek vannak hello legújabb elérhető kiadásai között (Ez azt jelenti, hogy verziója 5.6 Windows és Linux 5.5-ös verzió): 

* Reliable Collections (és a Reliable Stateful Services) 
* ReverseProxy 
* Önálló telepítő 
* Jegyzékfájlok XML-sémaérvényesítése 
* Konzol-átirányítás 
* hello tartalék Analysis Service (eszközök)
* Docker compose, kötet- és naplózási illesztők tárolókhoz 
* Tárolók és szolgáltatások erőforrás-szabályozása 
* DNS-szolgáltatás
* Azure Active Directory-támogatás
* Egyes PowerShell-parancsok parancssori felületi megfelelője 
* Powershell-parancsokat csak egy részét is futtathatók a Linux-fürt (a kibontott hello a következő szakaszban).

>[!NOTE]
>A konzolátirányítás nem támogatott éles fürtökben, még Windows rendszeren sem.

A fejlesztői eszközök eltérnek Windows és Linux rendszeren. Windows rendszeren a VisualStudio, a PowerShell, a VSTS és az ETW, míg Linuxon a Yeoman, az Eclipse, a Jenkins és az LTTng érhető el.

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
* Cmd
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
* [Hello Service Fabric CLI toomanage az alkalmazások használata](service-fabric-application-lifecycle-sfctl.md)
