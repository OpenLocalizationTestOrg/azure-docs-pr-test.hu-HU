---
title: "egy Azure belső aaaCreate terheléselosztó - PowerShell |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó PowerShell erőforrás-kezelő használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c6c98981-df9d-4dd7-a94b-cc7d1dc99369
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 4b9657c77aa32a142de49ff7871ed6a396b22223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-using-powershell"></a>Belső terheléselosztó létrehozása a PowerShell használatával

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

hello lépések azt ismertetik, hogyan toocreate egy belső terheléselosztó Azure Resource Manager használatával a PowerShell használatával. Az Azure Resource Manager hello elemek toocreate egy belső elosztott terhelésű egyedien vannak konfigurálva és majd egyesített toocreate terheléselosztót.

Toocreate kell, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:

* Záró IP-konfiguráció első - konfigurálja a bejövő hálózati forgalom hello magánhálózati IP-cím
* Háttér címkészletet - hello hálózati adapter, amely megkapja az előtérbeli IP-készletből származó hello terheléselosztott forgalom fogja konfigurálni.
* Terheléselosztási szabályok, mert a forrás- és helyi port konfigurálása az hello belső terheléselosztót.
* Mintavétel - állapot állapotmintáihoz hello hello virtuálisgép-példányok konfigurálja.
* Bejövő NAT-szabályok – hello port szabályok toodirectly elérhetősége hello virtuálisgép-példányok egyik konfigurálható.

További információkat szerezhet a terheléselosztónak az Azure Resource Managerben használt összetevőiről [Az Azure Resource Manager által nyújtott támogatás a terheléselosztó számára](load-balancer-arm.md) című részben.

hello következő részben megtudhatja, hogyan tooconfigure között két virtuális gép egy terheléselosztó.

## <a name="setup-powershell-toouse-resource-manager"></a>A telepítő PowerShell toouse erőforrás-kezelő

Győződjön meg arról, hello legújabb éles verziója van hello Azure modul PowerShell, és a PowerShell megfelelően beállítani tooaccess az Azure-előfizetéssel rendelkezik.

### <a name="step-1"></a>1. lépés

```powershell
Login-AzureRmAccount
```

### <a name="step-2"></a>2. lépés

Hello előfizetések hello fiók ellenőrzése

```powershell
Get-AzureRmSubscription
```

Rákérdezéses tooAuthenticate fogja a hitelesítő adataival.

### <a name="step-3"></a>3. lépés

Válassza ki, amely az Azure-előfizetések toouse.

```powershell
Select-AzureRmSubscription -Subscriptionid "GUID of subscription"
```

### <a name="create-resource-group-for-load-balancer"></a>Terheléselosztó erőforráscsoportjának létrehozása

Hozzon létre egy új erőforráscsoportot (hagyja ki ezt a lépést, ha egy meglévő erőforráscsoportot használ)

```powershell
New-AzureRmResourceGroup -Name NRP-RG -location "West US"
```

Az Azure Resource Manager megköveteli, hogy minden erőforráscsoport megadjon egy helyet. Ez az erőforráscsoport erőforrások hello alapértelmezett helye szolgál. Ellenőrizze, hogy az összes parancs fogja használni a terheléselosztók toocreate hello azonos erőforráscsoportot.

Hello jelenleg a fenti példában létrehozott egy erőforráscsoport neve "NRP-RG" és "Velünk nyugati" helyen.

## <a name="create-virtual-network-and-a-private-ip-address-for-front-end-ip-pool"></a>Virtuális hálózat és magánhálózati IP-cím létrehozása az előtér-IP-címkészlethez

Létrehoz egy alhálózatot hello virtuális hálózathoz, és hozzárendeli a toovariable $backendSubnet

```powershell
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -AddressPrefix 10.0.2.0/24
```

Virtuális hálózat létrehozása:

```powershell
$vnet= New-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $backendSubnet
```

Hello virtuális hálózatot hoz létre és hello alhálózati toohello lb-alhálózat-lehet virtuális hálózati NRPVNet hozzáadja, és hozzárendeli toovariable $vnet

## <a name="create-front-end-ip-pool-and-backend-address-pool"></a>Előtér-IP-címkészlet és háttércímkészlet létrehozása

Olyan előtérbeli IP-címkészlet a bejövő hello beállítása terheléselosztó hálózati forgalom és a háttérkiszolgáló cím készlet tooreceive hello terheléselosztott forgalom betöltése.

### <a name="step-1"></a>1. lépés

Hozzon létre egy hello alhálózati 10.0.2.0/24 hello bejövő hálózati forgalom végpont használandó hello magánhálózati IP-cím 10.0.2.5 használ előtérbeli IP-címkészletet.

```powershell
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name LB-Frontend -PrivateIpAddress 10.0.2.5 -SubnetId $vnet.subnets[0].Id
```

### <a name="step-2"></a>2. lépés

Állítsa be egy háttér címkészletet tooreceive bejövő forgalom használt előtérbeli IP-készletből:

```powershell
$beaddresspool= New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
```

## <a name="create-lb-rules-nat-rules-probe-and-load-balancer"></a>LB-szabályok, NAT-szabályok, mintavétel és terheléselosztó létrehozása

Miután létrehozta a hello előtérbeli IP-készlet és hello háttércímkészletre, toohello terheléselosztó erőforrást fog tartozni toocreate hello szabályok lesz szüksége:

### <a name="step-1"></a>1. lépés

```powershell
$inboundNATRule1= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389

$inboundNATRule2= New-AzureRmLoadBalancerInboundNatRuleConfig -Name "RDP2" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3442 -BackendPort 3389

$healthProbe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -RequestPath "HealthProbe.aspx" -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name "HTTP" -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Probe $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

hello példa a fenti hoz létre a következő elemek hello:

* NAT-szabály, amely minden bejövő forgalom tooport 3441 tooport 3389-es kerül.
* második NAT-szabályt hozhat létre, amely minden bejövő forgalom tooport 3442 tooport 3389-es kerül.
* olyan terheléselosztó szabályhoz, amely betölti a nyilvános port 80 toolocal 80-as portján hello háttér címkészletet minden bejövő forgalom elosztása.
* a mintavétel szabályt, amely ellenőrzi a elérési út "HealthProbe.aspx" hello állapotinformációit

### <a name="step-2"></a>2. lépés

Minden objektumok (NAT-szabályok, Load balancer szabályok, mintavételi konfigurációk) együtt hozzáadása hello terheléselosztó létrehozása:

```powershell
$NRPLB = New-AzureRmLoadBalancer -ResourceGroupName "NRP-RG" -Name "NRP-LB" -Location "West US" -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule1,$inboundNatRule2 -LoadBalancingRule $lbrule -BackendAddressPool $beAddressPool -Probe $healthProbe
```

## <a name="create-network-interfaces"></a>Hálózati adapterek létrehozása

Miután létrehozta a belső terheléselosztó hello, meg kell adnia mely hálózati csatolók hello bejövő hálózati forgalmat, NAT-szabályok és a mintavételi fog kapni. hello hálózati illesztő ebben az esetben külön-külön van konfigurálva, és rendelhető tooa virtuális gépek később.

### <a name="step-1"></a>1. lépés

Hello erőforrást beolvasni a virtuális hálózati és alhálózati toocreate hálózati adapterek:

```powershell
$vnet = Get-AzureRmVirtualNetwork -Name NRPVNet -ResourceGroupName NRP-RG

$backendSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name LB-Subnet-BE -VirtualNetwork $vnet
```

Ebben a lépésben létrehoz egy hálózati adapter, amely toohello load balancer háttérkészlethez tartozik, és hello első NAT-szabályhoz társítani RDP a hálózati adapter:

```powershell
$backendnic1= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic1-be -Location "West US" -PrivateIpAddress 10.0.2.6 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[0]
```

### <a name="step-2"></a>2. lépés

Hozzon létre egy második hálózati adaptert LB-Nic2-BE néven:

Ebben a lépésben létrehoz egy második hálózati adapter, toohello hozzárendelése vissza az azonos terheléselosztóhoz készlet végén, és RDP társítása hello második NAT-szabályának létrehozása:

```powershell
$backendnic2= New-AzureRmNetworkInterface -ResourceGroupName "NRP-RG" -Name lb-nic2-be -Location "West US" -PrivateIpAddress 10.0.2.7 -Subnet $backendSubnet -LoadBalancerBackendAddressPool $nrplb.BackendAddressPools[0] -LoadBalancerInboundNatRule $nrplb.InboundNatRules[1]
```

hello végeredménynek hello következő jelennek meg:

    $backendnic1

Várt kimenet:

    Name                 : lb-nic1-be
    ResourceGroupName    : NRP-RG
    Location             : westus
    Id                   : /subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
    Etag                 : W/"d448256a-e1df-413a-9103-a137e07276d1"
    ProvisioningState    : Succeeded
    Tags                 :
    VirtualMachine       : null
    IpConfigurations     : [
                         {
                           "PrivateIpAddress": "10.0.2.6",
                           "PrivateIpAllocationMethod": "Static",
                           "Subnet": {
                             "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/virtualNetworks/NRPVNet/subnets/LB-Subnet-BE"
                           },
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
                           "ProvisioningState": "Succeeded",
                           "Name": "ipconfig1",
                           "Etag": "W/\"d448256a-e1df-413a-9103-a137e07276d1\"",
                           "Id": "/subscriptions/f50504a2-1865-4541-823a-b32842e3e0ee/resourceGroups/NRP-RG/providers/Microsoft.Network/networkInterfaces/lb-nic1-be/ipConfigurations/ipconfig1"
                         }
                       ]
    DnsSettings          : {
                         "DnsServers": [],
                         "AppliedDnsServers": []
                       }
    AppliedDnsSettings   :
    NetworkSecurityGroup : null
    Primary              : False



### <a name="step-3"></a>3. lépés

Hello parancs Add-AzureRmVMNetworkInterface tooassign hello NIC tooa virtuális gép használja.

Hello lépésenkénti utasításokat toocreate egy virtuális gépet, és rendelje hozzá a tooa hálózati adapter található a következő hello dokumentáció: [egy Azure virtuális gép létrehozása PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json).

## <a name="add-hello-network-interface"></a>Hello hálózati adapter hozzáadása

Ha már létrehozott virtuális gépek, az alábbi lépésekkel hello hello hálózati adapter is hozzáadhat:

### <a name="step-1"></a>1. lépés

Hello terheléselosztó erőforrást betölteni egy változóba, (Ha ezt nem tette meg, amely még). hello használt változó hívott $lb és a fentiekben létrehozott hello terheléselosztó erőforrást azonos nevek használata hello.

```powershell
$lb = Get-AzureRmLoadBalancer –name NRP-LB -resourcegroupname NRP-RG
```

### <a name="step-2"></a>2. lépés

Hello háttér konfigurációs tooa változó betölteni.

```powershell
$backend = Get-AzureRmLoadBalancerBackendAddressPoolConfig -name backendpool1 -LoadBalancer $lb
```

### <a name="step-3"></a>3. lépés

Hálózati illesztő már létrehozott hello betölteni egy változóba. hello változónév használt $ hálózati adaptert. hello a hálózati adapter neve használt hello ugyanaz a fenti példa hello számára.

```powershell
$nic = Get-AzureRmNetworkInterface –name lb-nic1-be -resourcegroupname NRP-RG
```

### <a name="step-4"></a>4. lépés

Hello háttér konfigurációjának módosítása hello hálózati adapterén.

```powershell
$nic.IpConfigurations[0].LoadBalancerBackendAddressPools=$backend
```

### <a name="step-5"></a>5. lépés

Hello hálózati illesztő objektum mentése.

```powershell
Set-AzureRmNetworkInterface -NetworkInterface $nic
```

Egy adott hálózati csatoló toohello terheléselosztó háttérkészletének hozzáadása után kezdődik hello terheléselosztási szabályok az adott terheléselosztó erőforrást alapján hálózati forgalom fogadására.

## <a name="update-an-existing-load-balancer"></a>Meglévő terheléselosztó frissítése

### <a name="step-1"></a>1. lépés
A fenti példa hello hello terheléselosztót használ, rendeljen load balancer objektum toovariable használja a Get-AzureRmLoadBalancer $slb

```powershell
$slb = Get-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

### <a name="step-2"></a>2. lépés

A következő példa hello egy új bejövő forgalmat kezelő NAT-szabály hello előtér a 81-es port használatával adhat, és a port 8181 hello vissza az elosztott terhelésű meglévő készlet tooan végén

```powershell
$slb | Add-AzureRmLoadBalancerInboundNatRuleConfig -Name NewRule -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -FrontendPort 81  -BackendPort 8181 -Protocol Tcp
```

### <a name="step-3"></a>3. lépés

Használja a Set-AzureLoadBalancer hello új konfiguráció mentése

```powershell
$slb | Set-AzureRmLoadBalancer
```

## <a name="remove-a-load-balancer"></a>Terheléselosztó eltávolítása

Hello parancs Remove-AzureRmLoadBalancer toodelete "NRP-LB" erőforráscsoportban "NRP-RG" nevű nevű korábban létrehozott terheléselosztó használata

```powershell
Remove-AzureRmLoadBalancer -Name NRPLB -ResourceGroupName NRP-RG
```

> [!NOTE]
> Használhatja a hello választható kapcsoló - Force tooavoid hello kérdés törlésre.

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
