---
title: "egy Azure internetre aaaCreate terheléselosztó IPv6 - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre terheléselosztó IPv6 PowerShell az erőforrás-kezelő használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: d4c649e3-84ad-4343-8b6a-0e89f0b9e518
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 6ebb108399b070e06dddc33b7a774481eb44d717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-with-ipv6-using-powershell-for-resource-manager"></a>Internet felé néző a terheléselosztót, IPv6, az erőforrás-kezelő használatával PowerShell létrehozásához

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Sablon](load-balancer-ipv6-internet-template.md)

Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül. hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít. Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.

## <a name="example-deployment-scenario"></a>Központi telepítési példa

hello következő diagram azt ábrázolja hello terheléselosztási megoldás üzembe helyezéséhez a cikkben.

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-ps/lb-ipv6-scenario.png)

Ebben a forgatókönyvben a következő Azure-erőforrások hello hozza létre:

* egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím
* két terheléselosztási szabályok toomap hello nyilvános virtuális IP-címek toohello titkos végpontok betöltése
* egy rendelkezésre állási csoport toothat hello két virtuális gépeket tartalmaz
* két virtuális gépek (VM)
* az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő

## <a name="deploying-hello-solution-using-hello-azure-powershell"></a>Hello Azure PowerShell használatával hello megoldás telepítése

hello lépések bemutatják, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával a PowerShell használatával. Az Azure Resource Manager, az egyes erőforrások jön létre, és egyenként konfigurálni, majd hozzáfoghat toocreate erőforrás.

a terheléselosztó toodeploy, létrehozása és beállítása a következő objektumok hello:

* Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.
* Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.
* Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.

A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.

## <a name="set-up-powershell-toouse-resource-manager"></a>PowerShell toouse erőforrás-kezelő beállítása

Győződjön meg arról, hogy hello legújabb éles verziót hello Azure Resource Manager modul PowerShell.

1. Jelentkezzen be Azure

    ```powershell
    Login-AzureRmAccount
    ```

    Amikor a rendszer erre kéri, adja meg a hitelesítő adatait.

2. Hello előfizetések hello fiók ellenőrzése

    ```powershell
    Get-AzureRmSubscription
    ```

3. Válassza ki, amely az Azure-előfizetések toouse.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Hozzon létre egy erőforráscsoportot (kihagyása a lépést, ha egy meglévő erőforráscsoportot használ)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Hozzon létre egy virtuális hálózatot és egy nyilvános IP-cím hello előtér-IP-címtartományhoz.

1. Hozzon létre egy virtuális hálózati alhálózat.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    $vnet = New-AzureRmvirtualNetwork -Name VNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Azure nyilvános IP-cím (PIP) erőforrások hello előtér-IP-címkészlet létrehozása.

    ```powershell
    $publicIPv4 = New-AzureRmPublicIpAddress -Name 'pub-ipv4' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -IpAddressVersion IPv4 -DomainNameLabel lbnrpipv4
    $publicIPv6 = New-AzureRmPublicIpAddress -Name 'pub-ipv6' -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Dynamic -IpAddressVersion IPv6 -DomainNameLabel lbnrpipv6
    ```

    > [!IMPORTANT]
    > hello terheléselosztót használja hello tartománycímke hello nyilvános IP-előtag a teljesen minősített tartománynév. Ebben a példában hello teljes tartománynevek vannak *lbnrpipv4.westus.cloudapp.azure.com* és *lbnrpipv6.westus.cloudapp.azure.com*.

## <a name="create-a-front-end-ip-configurations-and-a-back-end-address-pool"></a>Egy előtérbeli IP-konfigurációk és a háttér-címkészlet létrehozása

1. Hozzon létre hello nyilvános IP-címeket létrehozott használó előtér-címkonfigurációt.

    ```powershell
    $FEIPConfigv4 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv4" -PublicIpAddress $publicIPv4
    $FEIPConfigv6 = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontendv6" -PublicIpAddress $publicIPv6
    ```

2. Háttér-címkészletek létrehozása.

    ```powershell
    $backendpoolipv4 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv4"
    $backendpoolipv6 = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "BackendPoolIPv6"
    ```

## <a name="create-lb-rules-nat-rules-a-probe-and-a-load-balancer"></a>LB szabályok, NAT szabályok, egy mintavételt és a terheléselosztó létrehozása

Ebben a példában a következő elemek hello hoz létre:

* a NAT-szabályok tootranslate minden bejövő forgalom a 443-as porton tooport 4443
* a load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben.
* a betöltési terheléselosztó szabály tooallow RDP kapcsolat toohello futó virtuális gépek 3389-es portot.
* a mintavétel szabály toocheck hello állapot nevű oldalon *HealthProbe.aspx* vagy a szolgáltatás a 8080-as porton
* a terheléselosztó által használt ezek az objektumok

1. Hello NAT-szabályok létrehozása.

    ```powershell
    $inboundNATRule1v4 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev4" -FrontendIpConfiguration $FEIPConfigv4 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    $inboundNATRule1v6 = New-AzureRmLoadBalancerInboundNatRuleConfig -Name "NicNatRulev6" -FrontendIpConfiguration $FEIPConfigv6 -Protocol TCP -FrontendPort 443 -BackendPort 4443
    ```

2. Hozzon létre egy állapotmintát. Nincsenek két módon tooconfigure a mintavételhez:

    HTTP-mintavétel

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    vagy a TCP-Hálózatfigyelővel

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name 'HealthProbe-v4v6' -Protocol Tcp -Port 8080 -IntervalInSeconds 15 -ProbeCount 2
    $RDPprobe = New-AzureRmLoadBalancerProbeConfig -Name 'RDPprobe' -Protocol Tcp -Port 3389 -IntervalInSeconds 15 -ProbeCount 2
    ```

    Ehhez a példához fogjuk toouse hello TCP-vizsgálat.

3. Hozzon létre egy terheléselosztó-szabályt.

    ```powershell
    $lbrule1v4 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv4" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $lbrule1v6 = New-AzureRmLoadBalancerRuleConfig -Name "HTTPv6" -FrontendIpConfiguration $FEIPConfigv6 -BackendAddressPool $backendpoolipv6 -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 8080
    $RDPrule = New-AzureRmLoadBalancerRuleConfig -Name "RDPrule" -FrontendIpConfiguration $FEIPConfigv4 -BackendAddressPool $backendpoolipv4 -Probe $RDPprobe -Protocol Tcp -FrontendPort 3389 -BackendPort 3389
    ```

4. Korábban létrehozott hello objektumok hello terheléselosztó létrehozása.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name 'myNrpIPv6LB' -Location 'West US' -FrontendIpConfiguration $FEIPConfigv4,$FEIPConfigv6 -InboundNatRule $inboundNATRule1v6,$inboundNATRule1v4 -BackendAddressPool $backendpoolipv4,$backendpoolipv6 -Probe $healthProbe,$RDPprobe -LoadBalancingRule $lbrule1v4,$lbrule1v6,$RDPrule
    ```

## <a name="create-nics-for-hello-back-end-vms"></a>Hálózati adapterek létrehozása a hello háttér virtuális gépek

1. Virtuális hálózati hello beolvasása és a virtuális hálózati alhálózat, ha a hálózati adapterek hello létrehozott toobe kell.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name VNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. IP-konfigurációk és a hálózati adapterek létrehozása a hello virtuális gépekhez.

    ```powershell
    $nic1IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4 -LoadBalancerInboundNatRule $inboundNATRule1v4
    $nic1IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6 -LoadBalancerInboundNatRule $inboundNATRule1v6
    $nic1 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic0' -IpConfiguration $nic1IPv4,$nic1IPv6 -ResourceGroupName NRP-RG -Location 'West US'

    $nic2IPv4 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv4IPConfig" -PrivateIpAddressVersion "IPv4" -Subnet $backendSubnet -LoadBalancerBackendAddressPool $backendpoolipv4
    $nic2IPv6 = New-AzureRmNetworkInterfaceIpConfig -Name "IPv6IPConfig" -PrivateIpAddressVersion "IPv6" -LoadBalancerBackendAddressPool $backendpoolipv6
    $nic2 = New-AzureRmNetworkInterface -Name 'myNrpIPv6Nic1' -IpConfiguration $nic2IPv4,$nic2IPv6 -ResourceGroupName NRP-RG -Location 'West US'
    ```

## <a name="create-virtual-machines-and-assign-hello-newly-created-nics"></a>Virtuális gépek létrehozása és hozzárendelése az újonnan létrehozott hálózati adapterek hello

A virtuális gépek létrehozásával kapcsolatos további információkért lásd: [létrehozása és előre konfigurálása a Windows rendszerű virtuális gép Resource Manager és az Azure PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Rendelkezésre állási csoport és a Storage-fiók létrehozása

    ```powershell
    New-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG -location 'West US'
    $availabilitySet = Get-AzureRmAvailabilitySet -Name 'myNrpIPv6AvSet' -ResourceGroupName NRP-RG
    New-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct' -Location 'West US' -SkuName "Standard_LRS"
    $CreatedStorageAccount = Get-AzureRmStorageAccount -ResourceGroupName NRP-RG -Name 'mynrpipv6stacct'
    ```

2. Minden virtuális gép létrehozása és hozzárendelése hello előző létrehozott hálózati adapter

    ```powershell
    $mySecureCredentials= Get-Credential -Message "Type hello username and password of hello local administrator account."

    $vm1 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM0' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm1 = Set-AzureRmVMOperatingSystem -VM $vm1 -Windows -ComputerName 'myNrpIPv6VM0' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm1 = Set-AzureRmVMSourceImage -VM $vm1 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm1 = Add-AzureRmVMNetworkInterface -VM $vm1 -Id $nic1.Id -Primary
    $osDisk1Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM0osdisk.vhd"
    $vm1 = Set-AzureRmVMOSDisk -VM $vm1 -Name 'myNrpIPv6VM0osdisk' -VhdUri $osDisk1Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm1

    $vm2 = New-AzureRmVMConfig -VMName 'myNrpIPv6VM1' -VMSize 'Standard_G1' -AvailabilitySetId $availabilitySet.Id
    $vm2 = Set-AzureRmVMOperatingSystem -VM $vm2 -Windows -ComputerName 'myNrpIPv6VM1' -Credential $mySecureCredentials -ProvisionVMAgent -EnableAutoUpdate
    $vm2 = Set-AzureRmVMSourceImage -VM $vm2 -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
    $vm2 = Add-AzureRmVMNetworkInterface -VM $vm2 -Id $nic2.Id -Primary
    $osDisk2Uri = $CreatedStorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/myNrpIPv6VM1osdisk.vhd"
    $vm2 = Set-AzureRmVMOSDisk -VM $vm2 -Name 'myNrpIPv6VM1osdisk' -VhdUri $osDisk2Uri -CreateOption FromImage
    New-AzureRmVM -ResourceGroupName NRP-RG -Location 'West US' -VM $vm2
    ```

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
