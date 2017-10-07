---
title: "egy Azure internetre aaaCreate terheléselosztó - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre irányuló terheléselosztó az erőforrás-kezelőben a PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: 8257f548-7019-417f-b15f-d004a1eec826
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: e4e0e5271bc83c23fc62c0910e784c57d2b30065
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started"></a>Internetkapcsolattal rendelkező terheléselosztó létrehozása az Azure Resource Managerben a PowerShell használatával

> [!div class="op_single_selector"]
> * [Portál](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre irányuló terheléselosztó hello klasszikus telepítési modell segítségével](load-balancer-get-started-internet-classic-cli.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-by-using-azure-powershell"></a>Hello megoldás telepítése Azure PowerShell használatával

hello a következő eljárások azt ismertetik, hogyan toocreate egy internetre irányuló terheléselosztó PowerShell Azure Resource Manager használatával. Az Azure Resource Manager, az egyes erőforrások jön létre és egyenként konfigurálni, majd tegye együtt toocreate egy terheléselosztó.

Kell létrehozni, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:

* Előtér-IP-konfiguráció: a nyilvános IP-címeket (PIP) tartalmazza a bejövő hálózati forgalomhoz.
* Háttér-címkészlet: hello virtuális gépek tooreceive hálózati forgalom hello terheléselosztó hálózati adapterek (NIC) tartalmazza.
* Terheléselosztási szabályok: hello terhelés terheléselosztó tooa port hello háttér-címkészlet a nyilvános port hozzárendelését szabályokat tartalmaz.
* Bejövő NAT-szabályok: hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port hozzárendelését szabályokat tartalmaz.
* Mintavételt: állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.

A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.

## <a name="set-up-powershell-toouse-resource-manager"></a>PowerShell toouse erőforrás-kezelő beállítása

Győződjön meg arról, hogy hello legújabb éles verziót hello Azure Resource Manager modul PowerShell:

1. Jelentkezzen be tooAzure.

    ```powershell
    Login-AzureRmAccount
    ```

    Amikor a rendszer erre kéri, adja meg a hitelesítő adatait.

2. Hello előfizetések hello fiók ellenőrzése.

    ```powershell
    Get-AzureRmSubscription
    ```

3. Válassza ki, amely az Azure-előfizetések toouse.

    ```powershell
    Select-AzureRmSubscription -SubscriptionId 'GUID of subscription'
    ```

4. Hozzon létre egy erőforráscsoportot. (Hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ.)

    ```powershell
    New-AzureRmResourceGroup -Name NRP-RG -location "West US"
    ```

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Hozzon létre egy virtuális hálózatot és egy nyilvános IP-cím hello előtér-IP-címtartományhoz.

1. Hozzon létre egy alhálózatot és egy virtuális hálózatot.

    ```powershell
    $backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
    New-AzureRmvirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location 'West US' -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
    ```

2. Hozzon létre egy Azure nyilvános IP-cím erőforrás, nevű **PublicIP**, hello DNS-nevet egy előtér-IP-készlet által használt toobe **loadbalancernrp.westus.cloudapp.azure.com**. a következő parancs hello hello használ statikus foglalási típusa.

    ```powershell
    $publicIP = New-AzureRmPublicIpAddress -Name PublicIp -ResourceGroupName NRP-RG -Location 'West US' -AllocationMethod Static -DomainNameLabel loadbalancernrp
    ```

   > [!IMPORTANT]
   > hello terheléselosztót használ hello tartománycímke hello nyilvános IP-előtagjaként azonos teljes Tartománynevű. Ez eltér hello klasszikus telepítési modell, amely a terheléselosztó FQDN hello hello felhőszolgáltatás használja.
   > Ebben a példában hello FQDN-je **loadbalancernrp.westus.cloudapp.azure.com**.

## <a name="create-a-front-end-ip-pool-and-a-back-end-address-pool"></a>Előtér-IP-címkészlet és háttércímkészlet létrehozása

1. Hozzon létre egy előtér-IP-címkészletet nevű **LB-előtérbeli** hello használó **PublicIp** erőforrás.

    ```powershell
    $frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PublicIpAddress $publicIP
    ```

2. Hozzon létre egy **LB-backend** nevű háttércímkészletet.

    ```powershell
    $beaddresspool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name LB-backend
    ```

## <a name="create-nat-rules-a-load-balancer-rule-a-probe-and-a-load-balancer"></a>NAT-szabályok, terheléselosztó-szabály, mintavétel és terheléselosztó létrehozása

Ebben a példában a következő elemek hello hoz létre:

* A NAT-szabály az összes bejövő forgalmat a port 3441 tooport 3389-es tootranslate
* A NAT-szabály az összes bejövő forgalmat a port 3442 tooport 3389-es tootranslate
* A mintavétel szabály toocheck hello állapot nevű oldalon **HealthProbe.aspx**
* A load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben
* Egy terheléselosztót, amely mindezeket az objektumokat használja

Használja az alábbi lépéseket:

1. Hello NAT-szabályok létrehozása.

    ```powershell
    $inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP1 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

    $inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name RDP2 -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389
    ```

2. Hozzon létre egy állapotmintát. Nincsenek két módon tooconfigure a mintavételhez:

    HTTP-mintavétel

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -RequestPath 'HealthProbe.aspx' -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

    TCP-mintavétel

    ```powershell
    $healthProbe = New-AzureRmLoadBalancerProbeConfig -Name HealthProbe -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
    ```

3. Hozzon létre egy terheléselosztó-szabályt.

    ```powershell
    $lbrule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool  $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
    ```

4. Hozzon létre hello terheléselosztó hello korábban létrehozott objektumokat.

    ```powershell
    $NRPLB = New-AzureRmLoadBalancer -ResourceGroupName NRP-RG -Name NRP-LB -Location 'West US' -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
    ```

## <a name="create-nics"></a>Hálózati adapterek létrehozása

Hálózati adapterek létrehozása (vagy módosíthatja a meglévőket) és majd rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételt:

1. Le hello virtuális hálózati és a virtuális hálózati alhálózat, ha a hello hálózati adaptert kell a létrehozott toobe.

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG
    $backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
    ```

2. Hozzon létre egy hálózati adapter nevű **lb nic1 kell**, és társítsa azt az első NAT-szabály hello és hello első (és csak) háttér címkészletet.

    ```powershell
    $backendnic1= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic1-be -Location 'West US' -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
    ```

3. Hozzon létre egy hálózati adapter nevű **lb nic2 kell**, és társítsa azt az hello második NAT-szabály és hello első (és csak) háttér címkészletet.

    ```powershell
    $backendnic2= New-AzureRmNetworkInterface -ResourceGroupName NRP-RG -Name lb-nic2-be -Location 'West US' -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
    ```

4. Ellenőrizze a hello hálózati adaptert.

        $backendnic1

    Várt kimenet:

        Name                 : lb-nic1-be
        ResourceGroupName    : NRP-RG
        Location             : westus
        Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
        ResourceGuid         : 896cac4f-152a-40b9-b079-3e2201a5906e
        ProvisioningState    : Succeeded
        Tags                 :
        VirtualMachine       : null
        IpConfigurations     : [
                            {
                            "Name": "ipconfig1",
                            "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                            "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1",
                            "PrivateIpAddress": "10.0.2.6",
                            "PrivateIpAllocationMethod": "Static",
                            "Subnet": {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                            },
                            "ProvisioningState": "Succeeded",
                            "PrivateIpAddressVersion": "IPv4",
                            "PublicIpAddress": {
                                "Id": null
                            },
                            "LoadBalancerBackendAddressPools": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/backendAddressPools/LB-backend"
                                }
                            ],
                            "LoadBalancerInboundNatRules": [
                                {
                                "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/loadBalancers/NRPlb/inboundNatRules/RDP1"
                                }
                            ],
                            "Primary": true,
                            "ApplicationGatewayBackendAddressPools": []
                            }
                        ]
        DnsSettings          : {
                            "DnsServers": [],
                            "AppliedDnsServers": [],
                            "InternalDomainNameSuffix": "prcwibzcuvie5hnxav0yjks2cd.dx.internal.cloudapp.net"
                        }
        EnableIPForwarding   : False
        NetworkSecurityGroup : null
        Primary              :

5. Használjon hello `Add-AzureRmVMNetworkInterface` parancsmag tooassign hello hálózati adapterek toodifferent virtuális gépeket.

## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

A virtuális gép létrehozásával és egy hálózati adapter hozzárendelésével kapcsolatos útmutatásért lásd: [Azure-beli virtuális gép létrehozása a PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-hello-network-interface-toohello-load-balancer"></a>Hello hálózati illesztő toohello terheléselosztó felvétele

1. Hello terheléselosztó beolvasása az Azure-ból.

    Hello terheléselosztó erőforrást betölteni egy változóba, (Ha ezt nem tette meg, amely még). hello változó **$lb**. Azonos nevek a korábban létrehozott hello terheléselosztó erőforrást hello használata.

    ```powershell
    $lb= get-azurermloadbalancer -name NRP-LB -resourcegroupname NRP-RG
    ```

2. Hello háttér-konfiguráció tooa változó betölteni.

    ```powershell
    $backend=Get-AzureRmLoadBalancerBackendAddressPoolConfig -name LB-backend -LoadBalancer $lb
    ```

3. Hálózati illesztő már létrehozott hello betölteni egy változóba. hello változó nevének megadása **$nic**. hello a hálózati adapter neve van hello azonos hello a korábbi példában.

    ```powershell
    $nic =get-azurermnetworkinterface -name lb-nic1-be -resourcegroupname NRP-RG
    ```

4. Hello háttér-konfigurációjának módosítása hello hálózati adapterén.

    ```powershell
    $nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
    ```

5. Hello hálózati illesztő objektum mentése.

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```

    Egy adott hálózati csatoló toohello load balancer háttér-készlet hozzáadása után kezdődik, az adott terheléselosztó erőforrást hello terheléselosztási szabályok alapján hálózati forgalom fogadására.

## <a name="update-an-existing-load-balancer"></a>Meglévő terheléselosztó frissítése

1. Hello segítségével terheléselosztó a korábbi példában hello, rendelje hozzá a terhelés terheléselosztó objektum toohello változó **$slb** használatával `Get-AzureLoadBalancer`.

    ```powershell
    $slb = get-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
    ```

2. A következő példa hello hozzáadja egy bejövő NAT-szabály – hello háttér-készlet – tooan meglévő terheléselosztó előtér-készletben hello 81-es porthoz és port 8181 használatával.

    ```powershell
    $slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol TCP
    ```

3. Hello új konfiguráció mentése segítségével `Set-AzureLoadBalancer`.

    ```powershell
    $slb | Set-AzureRmLoadBalancer
    ```

## <a name="remove-a-load-balancer"></a>Terheléselosztó eltávolítása

Hello paranccsal `Remove-AzureLoadBalancer` toodelete egy korábban létrehozott terheléselosztó nevű **NRP-LB** az erőforráscsoport neve **NRP-RG**.

```powershell
Remove-AzureRmLoadBalancer -Name NRP-LB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Hello választható kapcsoló **-Force** tooavoid hello kérdés törlésre.

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
