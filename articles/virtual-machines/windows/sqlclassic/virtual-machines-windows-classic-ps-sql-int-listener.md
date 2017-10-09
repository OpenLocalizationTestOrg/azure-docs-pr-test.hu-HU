---
title: "az Always On rendelkezésre állási csoportok az Azure-ban egy ILB figyelő aaaConfigure |} Microsoft Docs"
description: "Ez az oktatóanyag hello klasszikus telepítési modellel létrehozott erőforrást használ, és a belső terheléselosztót használó Azure-ban létrehoz egy Always On rendelkezésre állási csoport figyelőjének."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 2ce9b64fea491c945b58f7641e41fd39d90b078a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a>Egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-ban
> [!div class="op_single_selector"]
> * [Belső figyelő](../classic/ps-sql-int-listener.md)
> * [Külső figyelő](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a>Áttekintés

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus üzembe helyezési modellel hello használatát ismerteti. Azt javasoljuk, hogy az új telepítések esetén hello Resource Manager modellt használja.

egy Always On rendelkezésre állási csoport hello Resource Manager modellt, a figyelő tooconfigure lásd [egy terheléselosztót egy Always On rendelkezésre állási csoport konfigurálása az Azure-](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

A rendelkezésre állási csoport replikák, amelyek csak a helyszíni vagy Azure csak, vagy a helyszíni és az Azure hibrid konfigurációk átnyúló tartalmazhat. Az Azure replikák elhelyezkedhetnek hello belül azonos régióban vagy több virtuális hálózat használó különféle régiókban. hello ebben a cikkben szereplő eljárások azt feltételezik, hogy már rendelkezik [konfigurált rendelkezésre állási csoport](../classic/portal-sql-alwayson-availability-groups.md) , de még nincs konfigurálva egy figyelő.

## <a name="guidelines-and-limitations-for-internal-listeners"></a>Tudnivalók és korlátozások belső figyelők
hello egy belső terheléselosztón (ILB) egy rendelkezésre állási csoport figyelőjével, az Azure-ban használata tulajdonos toohello a következő irányelveket:

* hello rendelkezésre állási csoport figyelőjének Windows Server 2008 R2, Windows Server 2012 és Windows Server 2012 R2 esetén támogatott.
* Csak egy belső rendelkezésre állási csoport figyelőjének támogatott minden felhőalapú szolgáltatás, mert a hello figyelő toohello ILB konfigurálva, és csak egy ILB minden felhőalapú szolgáltatás. Lehetséges toocreate azonban több külső figyelők. További információkért lásd: [egy külső figyelőt a Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-ext-listener.md).

## <a name="determine-hello-accessibility-of-hello-listener"></a>Határozza meg a hello kisegítő hello-figyelő
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

Ez a cikk foglalkozik, amely egy ILB használja egy figyelő létrehozásával. Ha egy nyilvános vagy külső figyelőt, lásd: Ez a cikk ismerteti, amelyek a beállítás mentése hello verziója egy [külső figyelő](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a>Hozzon létre virtuális gép elosztott terhelésű végpontok közvetlen kiszolgálói visszatérési
Először hozzon létre egy ILB hello parancsfájl futtatásával később ebben a szakaszban.

Az egyes virtuális gépek az Azure-replikát tartalmazó elosztott terhelésű végpont létrehozásához. Ha több régióba replikákat, minden egyes replikának régió nem lehet azonos a felhőalapú szolgáltatás a hello a hello azonos Azure virtuális hálózat. Rendelkezésre állási csoport replikái, amelyek több Azure-régiók több létrehozása több virtuális hálózat beállítását igényli. Több virtuális hálózati kapcsolat konfigurálásával kapcsolatos további információkért lásd: [konfigurálja a virtuális hálózati toovirtual hálózati kapcsolat](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).

1. A hello Azure-portálon válassza a tooeach virtuális gép, amelyen a replika tooview hello részletei.

2. Kattintson a hello **végpontok** lapon az egyes virtuális gépek.

3. Győződjön meg arról, hogy hello **neve** és **nyilvános Port** hello figyelő végpont használni kívánt toouse nem már használatban van. Ebben a szakaszban hello példában hello értéke *MyEndpoint*, és hello port *1433*.

4. A helyi ügyfélen, töltse le és telepítse legújabb a hello [PowerShell modul](https://azure.microsoft.com/downloads/).

5. Indítsa el az Azure PowerShell.  
    Egy új PowerShell-munkamenetet nyit meg, a hello Azure felügyeleti modulok terhelését.

6. Futtassa az `Get-AzurePublishSettingsFile` parancsot. Ez a parancsmag tooa böngésző toodownload közzétételi beállítások fájl tooa helyi könyvtár irányítja. Rendszer felajánlhatja a bejelentkezési hitelesítő adatok Azure-előfizetése.

7. Futtassa a következő hello `Import-AzurePublishSettingsFile` hello hello elérési parancs közzététele beállításfájl letöltött:

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    Miután hello közzététele beállításfájl importálása, kezelheti az Azure-előfizetéshez hello PowerShell-munkamenetben.

8. A *ILB*, statikus IP-cím. Vizsgálja meg a hello jelenlegi virtuális hálózati konfiguráció hello a következő parancs futtatásával:

        (Get-AzureVNetConfig).XMLConfiguration
9. Megjegyzés: hello *alhálózati* hello replika hello virtuális gépeket tartalmazó hello alhálózat neve. Ez a név hello parancsfájlban hello $SubnetName paraméter szerepel.

10. Megjegyzés: hello *VirtualNetworkSite* és a kezdési hello *címelőtagja* hello replika hello virtuális gépeket tartalmazó hello alhálózat. Keresse meg az elérhető IP-cím úgy, hogy mindkét értékek toohello `Test-AzureStaticVNetIP` parancsot, és úgy hello *AvailableAddresses*. Például, ha hello virtuális hálózat neve *MyVNet* , és a alhálózati címtartományt kezdődő *172.16.0.128*, hello következő parancsot a rendelkezésre álló címek volna listában:

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. Válasszon egyet az elérhető címek hello, és felhasználhatja őket a hello $ILBStaticIP paraméter hello parancsfájl hello következő lépésben.

12. A következő PowerShell-parancsfájl tooa szövegszerkesztőben hello másolja, és állítsa be a hello változók értékeinek toosuit a környezetben. Egyes paraméterek alapértelmezett adtak ki.  

    Meglévő affinitáscsoportok használó központi telepítések nem adható hozzá egy Példánynak. ILB követelményeivel kapcsolatos további információkért lásd: [belső load balancer áttekintése](../../../load-balancer/load-balancer-internal-overview.md).

    Is ha a rendelkezésre állási csoport Azure-régiók kiterjedő, futtatnia kell hello parancsfájl egyszer minden adatközpontban hello felhőalapú szolgáltatás, és azt az adatközpontot lévő csomópontok.

        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that hello replicas use in hello virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for hello ILB in hello subnet
        $ILBName = "AGListenerLB" # customize hello ILB name or use this default value

        # Create hello ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. Hello változók beállítása után másolási hello parancsfájl hello text editor tooyour PowerShell munkamenetben toorun azt. Ha még mindig hello kérdés  **>>** , nyomja le az Enter újra toomake meg arról, hogy hello parancsfájl futásának indításakor-e.

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a>Győződjön meg arról, hogy KB2854082 telepítve van-e, ha szükséges
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a>Nyissa meg a rendelkezésre állási csoport csomópontok hello tűzfalportok
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a>Hello rendelkezésre állási csoport figyelőjének létrehozása

Hello rendelkezésre állási csoport figyelőjének létrehozása két lépésben. Először hello ügyfél hozzáférési pont fürterőforrás létrehozása, és konfigurálja a függőségek. Második konfigurálja a PowerShell hello fürt erőforrásait.

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a>Ügyfél-hozzáférési pont hello létrehozása és konfigurálása hello fürt függőségek
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a>A PowerShell hello fürterőforrások konfigurálása
1. A Példánynak korábban létrehozott ILB hello hello IP-címét kell használnia. az IP-címe a PowerShell, a következő parancsfájl használata hello tooobtain:

        # Define variables
        $ServiceName="<MyServiceName>" # hello name of hello cloud service that contains hello AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. Hello virtuális gépek egyik hello PowerShell parancsfájl másolása az operációs rendszer tooa szövegszerkesztőben, és korábban feljegyzett toohello értékek hello változók adja.

    A Windows Server 2012 vagy újabb használja a következő parancsfájl hello:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    A Windows Server 2008 R2 a következő parancsfájl hello használata:

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. Miután set hello változók, nyisson meg egy rendszergazda jogú Windows PowerShell-ablakban, Beillesztés hello parancsfájl hello szövegszerkesztőben a PowerShell-munkamenet toorun be azt. Ha még mindig hello kérdés  **>>** , nyomja le az Enter újra toomake meg arról, hogy hello parancsfájl futásának indításakor.

4. Ismételje meg az előző lépésekben az egyes virtuális gépek hello.  
    Ez a parancsfájl hello IP-cím erőforrás hello felhőszolgáltatás hello IP-címmel konfigurálja, és beállítja a más paramétereket, például hello mintavételi portot. Hello IP-cím erőforrás online állapotba kerül, ha azt válaszolhassanak a hello mintavételi portot a hello elosztott terhelésű végpont korábban létrehozott lekérdezési toohello.

## <a name="bring-hello-listener-online"></a>Kapcsolja a hálózatra hello figyelő
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a>Követő műveletet elemek
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-virtual-network"></a>Teszt hello rendelkezésre állási csoport figyelőjét (belül hello azonos virtuális hálózat)
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
