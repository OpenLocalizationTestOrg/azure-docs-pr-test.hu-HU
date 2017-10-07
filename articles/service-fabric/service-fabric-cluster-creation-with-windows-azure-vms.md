---
title: "Azure virtuális gépeken futó Windows-fürtöt aaaCreate önálló |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és kezelése az Azure Service Fabric-fürt Windows Server rendszert futtató Azure virtuális gépeken."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 7eeb40d2-fb22-4a77-80ca-f1b46b22edbc
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: ryanwi;chackdan
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: 8900204fe69887a7a0ca54b06e0d32534421bcfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-three-node-standalone-service-fabric-cluster-with-azure-virtual-machines-running-windows-server"></a>A három csomópont különálló Service Fabric-fürt létrehozása a Windows Server rendszert futtató Azure virtuális gépekkel
Ez a cikk ismerteti, hogyan toocreate a fürtben, a Windows-alapú Azure virtuális gépek (VM), használatával hello a különálló Service Fabric telepítő a Windows Server. hello forgatókönyv egy különleges esetben a rendszer [létrehozása és kezelése Windows Server rendszert futtató fürtre](service-fabric-cluster-creation-for-windows-server.md) hello virtuális gépek esetén [Windows Server rendszert futtató Azure virtuális gépek](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json), azonban nem hozza létre [egy Azure Service Fabric-fürt felhőalapú](service-fabric-cluster-creation-via-portal.md). az ebben a mintában a következő hello különbség, hogy hello különálló Service Fabric-fürt hozta létre a következő lépéseket hello teljes mértékben Ön kezeli, mivel hello Azure felhőalapú Service Fabric-fürtök által kezelt és frissíteni a Service Fabric hello erőforrás-szolgáltató.

## <a name="steps-toocreate-hello-standalone-cluster"></a>Lépéseket toocreate hello önálló fürthöz
1. Jelentkezzen be Azure-portálon toohello, és hozzon létre egy új Windows Server 2012 R2 Datacenter vagy a Windows Server 2016 Datacenter VM egy erőforráscsoportot. A cikk elolvasása hello [Windows virtuális gép létrehozása az Azure-portálon hello](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) további részleteket.
2. Vegye fel néhány további virtuális gépek toohello ugyanabban az erőforráscsoportban. Győződjön meg arról, hogy egyes virtuális gépek hello rendelkezik hello azonos rendszergazdai felhasználónév és jelszó létrehozásakor. Létrehozás után megtekintheti az összes három virtuális hello ugyanazt a virtuális hálózatot.
3. Csatlakozás a virtuális gépek hello tooeach, és kikapcsolni hello hello használata a Windows tűzfal [Kiszolgálókezelő, a helyi kiszolgáló irányítópult](https://technet.microsoft.com/library/jj134147.aspx). Ez biztosítja, hello hálózati forgalom képes kommunikálni a hello gépek között. Csatlakoztatott tooeach a számítógépen, miközben hello IP-cím beolvasása nyisson meg egy parancssort, és írja be `ipconfig`. Másik lehetőségként láthatja hello IP cím az egyes gépek hello Portal hello virtuális hálózati erőforrás hello erőforráscsoport kiválasztásával, és létre minden egyes ezeknek a gépeknek hello hálózati kapcsolatok ellenőrzése.
4. Csatlakoztassa a virtuális gépek hello tooone, és tesztelje, hogy pingelhető hello a többi két virtuális gép sikeresen.
5. Csatlakozás a virtuális gépek hello tooone és [hello különálló Service Fabric-csomag letöltése a Windows Server](http://go.microsoft.com/fwlink/?LinkId=730690) hello egy új mappájába számítógéphez, és bontsa ki a hello csomag.
6. Nyissa meg hello *ClusterConfig.Unsecure.MultiMachine.json* fájlt a Jegyzettömbben, és hello gépek hello három IP-címét az egyes csomópontok szerkesztése. Módosítsa hello felső hello fürt nevét, és mentse hello fájlt.  Hello fürtjegyzékben részleges példát alább láthatók.
   
    ```
    {
        "name": "SampleCluster",
        "clusterConfigurationVersion": "1.0.0",
        "apiVersion": "01-2017",
        "nodes": [
        {
            "nodeName": "standalonenode0",
            "iPAddress": "10.1.0.4",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc1/r0",
            "upgradeDomain": "UD0"
        },
        {
            "nodeName": "standalonenode1",
            "iPAddress": "10.1.0.5",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc2/r0",
            "upgradeDomain": "UD1"
        },
        {
            "nodeName": "standalonenode2",
            "iPAddress": "10.1.0.6",
            "nodeTypeRef": "NodeType0",
            "faultDomain": "fd:/dc3/r0",
            "upgradeDomain": "UD2"
        }
        ],
    ```
7. Ha azt tervezi, hogy a biztonságos fürt toobe, döntse el, hello biztonsági intézkedés kívánja toouse, majd hajtsa végre a hello hello készítésével kapcsolatos hivatkozás: [X509 tanúsítvány](service-fabric-windows-cluster-x509-security.md) vagy [Windows biztonsági](service-fabric-windows-cluster-windows-security.md). Ha használja a Windows biztonsági hello fürt beállítása, szüksége lesz a tooset be egy tartomány tartományvezérlői toomanage Active Directory. Vegye figyelembe, hogy egy tartomány tartományvezérlői gép használja, mint a Service Fabric csomópont nem támogatott.
8. Nyissa meg a [a PowerShell ISE ablaka](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise). Keresse meg a toohello mappát, amelyben kibontotta hello letöltött önálló telepítő csomag és hello fürt konfigurációs fájlt mentette. Futtassa a következő PowerShell parancs toodeploy hello fürt hello:
   
    ```
    .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.MultiMachine.json
    ```

hello parancsfájl hello Service Fabric-fürt távolról konfigurálja, és kell jelentse az előrehaladást, a központi telepítés áthalad.

9. Után körülbelül egy perce, ellenőrizheti, ha hello fürt működőképességét toohello csatlakoztatásával Service Fabric Explorer használatával hello gép IP-címek, például a `http://10.1.0.5:19080/Explorer/index.html`. 

## <a name="next-steps"></a>Következő lépések
* [Önálló Service Fabric-fürtök létrehozása Windows Server vagy Linux rendszerű gépeken](service-fabric-deploy-anywhere.md)
* [Hozzáadása vagy eltávolítása, csomópontok tooa különálló Service Fabric-fürt](service-fabric-cluster-windows-server-add-remove-nodes.md)
* [Önálló Windows-fürt konfigurációs beállításai](service-fabric-cluster-manifest.md)
* [Biztonságos Windows használja-e a Windows biztonsági önálló fürtben](service-fabric-windows-cluster-windows-security.md)
* [Biztonságos Windows X509 használata önálló fürtben, tanúsítványok](service-fabric-windows-cluster-x509-security.md)

