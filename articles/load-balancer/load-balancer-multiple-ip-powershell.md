---
title: "több IP-konfigurációk az Azure-ban a terheléselosztás aaaLoad |} Microsoft Docs"
description: "Terheléselosztás elsődleges és másodlagos IP-konfiguráció között."
services: load-balancer
documentationcenter: na
author: anavinahar
manager: narayan
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: annahar
ms.openlocfilehash: fe1cdb317350942ff759229491c2025e98dd24a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-powershell"></a>Hálózati terheléselosztást a PowerShell használatával több IP-konfigurációk

> [!div class="op_single_selector"]
> * [Portál](load-balancer-multiple-ip.md)
> * [Parancssori felület](load-balancer-multiple-ip-cli.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)

Ez a cikk ismerteti, hogyan toouse Azure Load Balancer több IP-címek egy másodlagos hálózati adapteren (NIC). Ebben a forgatókönyvben két virtuális gépeken futó Windows, az elsődleges és másodlagos hálózati tudunk Egyes hello másodlagos hálózati adapterei két IP-konfigurációk. Minden virtuális gép webhelyeket a contoso.com és fabrikam.com üzemelteti. Minden webhelyre kötött tooone az IP-konfigurációkhoz hello hello másodlagos hálózati adaptert. Azure Load Balancer tooexpose két előtérbeli IP-cím, egy a minden webhelyre, toodistribute forgalom toohello megfelelő IP-konfiguráció hello webhely használjuk. Ebben a forgatókönyvben használt hello ugyanazt a portszámot is frontends, valamint mindkét háttér címkészletet IP-címek között.

![Terheléselosztó forgatókönyv kép](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a>Több IP-konfigurációk lépéseket tooload egyenlege

Kövesse az alábbi cikkben leírt tooachieve hello forgatókönyv hello lépéseket:

1. Az Azure PowerShell telepítése. Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.
2. Hozzon létre egy erőforráscsoportot, a következő beállítások hello használata:

    ```powershell
    $location = "westcentralus".
    $myResourceGroup = "contosofabrikam"
    ```

    További információkért lásd a 2. lépés: [hozzon létre egy erőforráscsoportot](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

3. [Rendelkezésre állási csoport létrehozása](../virtual-machines/windows/tutorial-availability-sets.md?toc=%2fazure%2fload-balancer%2ftoc.json) toocontain a virtuális gépek. Ebben az esetben használja a következő parancs hello:

    ```powershell
    New-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset" -Location "West Central US"
    ```

4. Kövesse az utasításokat lépéseket 3-5 a [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) tooprepare hello létrehozását a virtuális gép egyetlen hálózati és a következő cikket: 6.1-es lépés végrehajtása, és használja a hello következő helyett 6.2. lépés:

    ```powershell
    $availset = Get-AzureRmAvailabilitySet -ResourceGroupName "contosofabrikam" -Name "myAvailset"
    New-AzureRmVMConfig -VMName "VM1" -VMSize "Standard_DS1_v2" -AvailabilitySetId $availset.Id
    ```

    Fejezze be [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json) lépések 6.3 6.8 keresztül.

5. Adjon hozzá egy második IP konfigurációs tooeach a virtuális gépek hello. Hello utasításait követve [több IP-cím hozzárendelése toovirtual gépek](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md#add) cikk. A következő konfigurációs beállítások hello használata:

    ```powershell
    $NicName = "VM1-NIC2"
    $RgName = "contosofabrikam"
    $NicLocation = "West Central US"
    $IPConfigName4 = "VM1-ipconfig2"
    $Subnet1 = Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $myVnet
    ```

    Ez az oktatóanyag célja hello tooassociate hello másodlagos IP-konfigurációk nyilvános IP-címek nem szükséges. Szerkesztés hello parancs tooremove hello nyilvános IP társítás része.

6. Újra 4 – 6. Ez a cikk lépéseinek elvégzését vm2 virtuális gépnek. Lehet, hogy tooreplace hello virtuális gép neve tooVM2, ennek során. Vegye figyelembe, hogy nem kell egy virtuális hálózati toocreate hello a második virtuális gép. Lehet, vagy nem hozható létre egy új alhálózatot a használati eset alapján.

7. Hozzon létre két nyilvános IP-címet, és tárolhatja őket hello megfelelő változók látható módon:

    ```powershell
    $publicIP1 = New-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel contoso
    $publicIP2 = New-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam -Location 'West Central US' -AllocationMethod Dynamic -DomainNameLabel fabrikam

    $publicIP1 = Get-AzureRmPublicIpAddress -Name PublicIp1 -ResourceGroupName contosofabrikam
    $publicIP2 = Get-AzureRmPublicIpAddress -Name PublicIp2 -ResourceGroupName contosofabrikam
    ```

8. Hozzon létre két előtérbeli IP-konfigurációkat:

    ```powershell
    $frontendIP1 = New-AzureRmLoadBalancerFrontendIpConfig -Name contosofe -PublicIpAddress $publicIP1
    $frontendIP2 = New-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2
    ```

9. A háttér címkészletet, egy mintavételt és a terheléselosztási szabályok létrehozása:

    ```powershell
    $beaddresspool1 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name contosopool
    $beaddresspool2 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool

    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HTTP -RequestPath 'index.html' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

    $lbrule1 = New-AzureRmLoadBalancerRuleConfig -Name HTTPc -FrontendIpConfiguration $frontendIP1 -BackendAddressPool $beaddresspool1 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    $lbrule2 = New-AzureRmLoadBalancerRuleConfig -Name HTTPf -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthprobe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

10. Ha már létrehozott ezeket az erőforrásokat, hozza létre a terheléselosztó:

    ```powershell
    $mylb = New-AzureRmLoadBalancer -ResourceGroupName contosofabrikam -Name mylb -Location 'West Central US' -FrontendIpConfiguration $frontendIP1 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

11. Hello második háttér cím címkészletet és az előtérbeli IP konfigurációs az újonnan létrehozott tooyour terheléselosztó felvétele:

    ```powershell
    $mylb = Get-AzureRmLoadBalancer -Name "mylb" -ResourceGroupName $myResourceGroup | Add-AzureRmLoadBalancerBackendAddressPoolConfig -Name fabrikampool | Set-AzureRmLoadBalancer

    $mylb | Add-AzureRmLoadBalancerFrontendIpConfig -Name fabrikamfe -PublicIpAddress $publicIP2 | Set-AzureRmLoadBalancer
    
    Add-AzureRmLoadBalancerRuleConfig -Name HTTP -LoadBalancer $mylb -FrontendIpConfiguration $frontendIP2 -BackendAddressPool $beaddresspool2 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80 | Set-AzureRmLoadBalancer
    ```

12. az alábbi parancsok hello beolvasása hello hálózati adapterek, és adja meg, mindkét IP-konfigurációk az egyes másodlagos hálózati adapter toohello háttércímkészletet hello a terheléselosztó:

    ```powershell
    $nic1 = Get-AzureRmNetworkInterface -Name "VM1-NIC2" -ResourceGroupName "MyResourcegroup";
    $nic2 = Get-AzureRmNetworkInterface -Name "VM2-NIC2" -ResourceGroupName "MyResourcegroup";

    $nic1.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic1.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);
    $nic2.IpConfigurations[0].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[0]);
    $nic2.IpConfigurations[1].LoadBalancerBackendAddressPools.Add($mylb.BackendAddressPools[1]);

    $mylb = $mylb | Set-AzureRmLoadBalancer

    $nic1 | Set-AzureRmNetworkInterface
    $nic2 | Set-AzureRmNetworkInterface
    ```

13. Végül konfigurálnia kell DNS erőforrás rekordok toopoint toohello megfelelő előtérbeli IP-címe hello terheléselosztóhoz. A tartományok Azure DNS-ben is tartalmazhat. Az Azure DNS-sel terheléselosztással kapcsolatos további információkért lásd: [Azure DNS használata más Azure-szolgáltatásokkal](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Következő lépések
- További tudnivalók az Azure-ban hogyan toocombine terheléselosztás szolgáltatások [terheléselosztás szolgáltatások használata az Azure-ban](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Megtudhatja, hogyan naplók különböző típusait használják az Azure toomanage és hibaelhárítása a terheléselosztó [analytics keresse meg a Azure terheléselosztó](../load-balancer/load-balancer-monitor-log.md).
