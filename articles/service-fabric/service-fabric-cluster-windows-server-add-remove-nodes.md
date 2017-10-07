---
title: "aaaAdd, vagy távolítsa el csomópontok tooa különálló Service Fabric-fürt |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd, vagy távolítsa el az csomópontok tooan Azure Service Fabric fürt egy fizikai gép vagy a helyszíni lehet a Windows Server rendszerű virtuális gép vagy a felhőben."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: bc6b8fc0-d2af-42f8-a164-58538be38d02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/02/2017
ms.author: dekapur
ms.openlocfilehash: 1da908ad9840faa052e0b4021bc2d4ce732b02bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-or-remove-nodes-tooa-standalone-service-fabric-cluster-running-on-windows-server"></a>Hozzáadása vagy eltávolítása, csomópontok tooa különálló Service Fabric-fürt működő Windows Server
Miután [a különálló Service Fabric-fürt létrehozása a Windows Server gépeken](service-fabric-cluster-creation-for-windows-server.md), az üzleti igényeknek megfelelően változhat és előfordulhat, hogy tooadd kell, vagy távolítsa el a fürt a csomópontok tooyour. Ez a cikk ismerteti a részletes lépéseket tooachieve ez. Vegye figyelembe, hogy hozzáadása csomópont funkció nem támogatott a helyi fejlesztési fürtök.

## <a name="add-nodes-tooyour-cluster"></a>Csomópontok tooyour fürt hozzáadása
1. Prepare hello tooadd tooyour fürt hello említett hello lépéseket követve kívánt virtuális gép/gép [Prepare hello gépek toomeet hello előfeltételei fürttelepítés](service-fabric-cluster-creation-for-windows-server.md) szakasz
2. Melyik tartalék tartomány és a frissítési tartomány a virtuális gép/gép áll további folyamatos tooadd azonosítása
3. A távoli asztal (RDP) a virtuális gép vagy gépek, amelyet az tooadd toohello fürt hello
4. Másolás vagy [hello önálló csomag letöltése a Service Fabric Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) virtuális gép vagy gépek toohello és csomagolja ki a hello csomag
5. Powershell futtassa emelt szintű jogosultságokkal, és keresse meg a tömörítetlen csomag hello toohello helye
6. Futtassa a hello *AddNode.ps1* parancsfájl hello új csomópont tooadd leíró hello paraméterekkel. az alábbi példa hello VM5 nevű új csomópont hozzáadása, típussal, NodeType0 és IP-cím 182.17.34.52, UD1 és fd: / dc1/r0. Hello *ExistingClusterConnectionEndPoint* már van egy csomópont a csatlakozási végpont hello meglévő fürt, amely lehet hello IP-címe *bármely* hello fürt csomópontja.

    ```
    .\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain fd:/dc1/r0 -AcceptEULA
    ```
    Miután hello parancsfájl befejezése után történik, ha új csomópont hello bővült hello futtatásával ellenőrizheti [Get-ServiceFabricNode](/powershell/module/servicefabric/get-servicefabricnode?view=azureservicefabricps) parancsmag.

7. tooensure konzisztencia hello fürt csomópontjai között, a felhasználónak kezdeményeznie kell a konfiguráció frissítése. Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello legfrissebb konfigurációs fájlt, és adja hozzá a hello az újonnan hozzáadott csomópont túl "Csomópont" szakasz. Célszerű tooalways rendelkezik hello legújabb fürtkonfiguráció érhető el, hogy kell-e a fürt hello tooredploy hello esetben ugyanazt a konfigurációt.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
8. Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello frissítését.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    Kísérheti hello hello frissítés a Service Fabric Explorerben talál. Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-nodes-tooclusters-configured-with-windows-security-using-gmsa"></a>Csoportosan felügyelt szolgáltatásfiókot használó Windows biztonsági konfigurált csomópontok tooclusters hozzáadása
A fürtök konfigurált csoport által felügyelt szolgáltatás Account(gMSA) (https://technet.microsoft.com/library/hh831782.aspx) egy új csomópont felveheti egy konfigurációs frissítéssel:
1. Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) hello meglévő csomópontok egyikén tooget hello legfrissebb konfigurációs fájlt, és részletek megadása hello új csomópont kívánt tooadd hello "csomópont" szakaszában. Ellenőrizze, hogy új csomópont hello része a hello azonos csoport felügyelt fiók. Ezt a fiókot minden számítógépen rendszergazdának kell lennie.

    ```
        {
            "nodeName": "vm5",
            "iPAddress": "182.17.34.52",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD1"
        }
    ```
2. Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello frissítését.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>
    ```
    Kísérheti hello hello frissítés a Service Fabric Explorerben talál. Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

### <a name="add-node-types-tooyour-cluster"></a>Csomópont típusok tooyour fürt hozzáadása
Érdekében tooadd egy új típusú csomópont, módosítsa a konfigurációs tooinclude hello új csomóponttípus "NodeType tulajdonságok értéke" szakaszban a "Tulajdonságok" és egy konfigurációs megkezdéséhez használatával frissítse [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps). Hello frissítése után ez a csomóponttípus az új csomópontok tooyour fürt adhat hozzá.

## <a name="remove-nodes-from-your-cluster"></a>Csomópontok eltávolítása a fürtből
A csomópont távolíthatók el a fürt egy konfigurációs frissítése a következő módon hello:

1. Futtatás [Get-ServiceFabricClusterConfiguration](/powershell/module/servicefabric/get-servicefabricclusterconfiguration?view=azureservicefabricps) tooget hello legfrissebb konfigurációs fájlban és *eltávolítása* hello csomópont "Csomópont" szakaszában.
Adja hozzá a hello "NodesToBeRemoved" paraméter túl "beállítása" szakasz "FabricSettings" szakaszon belül. "érték" Hello csomópontnevek eltávolított toobe igénylő csomópontok vesszővel elválasztott listája legyen.

    ```
         "fabricSettings": [
            {
            "name": "Setup",
            "parameters": [
                {
                "name": "FabricDataRoot",
                "value": "C:\\ProgramData\\SF"
                },
                {
                "name": "FabricLogRoot",
                "value": "C:\\ProgramData\\SF\\Log"
                },
                {
                "name": "NodesToBeRemoved",
                "value": "vm0, vm1"
                }
            ]
            }
        ]
    ```
2. Futtatás [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps) toobegin hello frissítését.

    ```
    Start-ServiceFabricClusterConfigurationUpgrade -ClusterConfigPath <Path tooConfiguration File>

    ```
    Kísérheti hello hello frissítés a Service Fabric Explorerben talál. Alternatív megoldásként futtathatja [Get-ServiceFabricClusterUpgrade](/powershell/module/servicefabric/get-servicefabricclusterupgrade?view=azureservicefabricps)

> [!NOTE]
> Csomópontok eltávolítása több frissítés kezdeményezhet. Egyes csomópontok lesznek megjelölve `IsSeedNode=”true”` címkét, és úgy azonosítható, hello fürt lekérdezésével manifest használatával `Get-ServiceFabricClusterManifest`. Ilyen csomópontok eltávolítása is tovább tarthat a többinél mivel hello kezdőérték csomópontok kell toobe helyezi át az ilyen helyzetekben. hello fürt legalább 3 elsődleges típusú csomópontok kell fenntartani.
> 
> 

### <a name="remove-node-types-from-your-cluster"></a>Csomóponttípusok eltávolítása a fürtből
Mielőtt eltávolítaná a csomóponttípus, kérjük, ellenőrizze hivatkozzon a csomóponttípus hello csomópontok esetén. Megfelelő típusú hello a csomópont eltávolítása előtt távolítsa el ezeket a csomópontokat. Minden megfelelő csomópontokat törlődik, miután hello NodeType eltávolítása hello fürtkonfiguráció, és egy konfigurációs megkezdéséhez használatával frissítse [Start-ServiceFabricClusterConfigurationUpgrade](/powershell/module/servicefabric/start-servicefabricclusterconfigurationupgrade?view=azureservicefabricps).


### <a name="replace-primary-nodes-of-your-cluster"></a>Cserélje le a fürt elsődleges csomópontok
hello cseréje az elsődleges csomópont helyett eltávolítása és a kötegek egymás után kell elvégezni egy csomópont.


## <a name="next-steps"></a>Következő lépések
* [Önálló Windows-fürt konfigurációs beállításai](service-fabric-cluster-manifest.md)
* [Biztonságos Windows X509 használata önálló fürtben, tanúsítványok](service-fabric-windows-cluster-x509-security.md)
* [Hozzon létre egy különálló Service Fabric-fürt Windowst futtató Azure virtuális gépeken](service-fabric-cluster-creation-with-windows-azure-vms.md)

