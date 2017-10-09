---
title: "Virtuálisgép-méretezési aaaCreating beállítja a PowerShell-parancsmagok használatával |} Microsoft Docs"
description: "Első lépések, létrehozását és kezelését az első Azure virtuális gép méretezési csoportok Azure PowerShell-parancsmagok használatával"
services: virtual-machines-windows
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 430d9d64-1f35-48f0-a4fd-9b69910ffa59
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: danielsollondon
ms.openlocfilehash: 7979be367d04c904b60d78849c1b751a52cc8caf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a>PowerShell-parancsmagok használatával virtuálisgép-méretezési csoportok létrehozása
Ez a cikk végigvezeti egy példa hogyan toocreate egy virtuálisgép-méretezési csoportban (VMSS). Kapcsolódó hálózati és tárolási méretezési meg három csomópontok létrehozott.

## <a name="first-steps"></a>Első lépések
Győződjön meg arról, hello legújabb Azure PowerShell-modul telepítve van, toomake meg arról, hogy a PowerShell parancsmagjaival hello toomaintain szükséges, és hozza létre a méretezési készlet.
Nyissa meg a parancssori eszközök toohello [Itt](http://aka.ms/webpi-azps) a hello a legújabb elérhető Azure-modulok.

toofind VMSS kapcsolódó parancsmagok, hello keresési karakterlánc \*VMSS\*. Például _gcm *vmss*_

## <a name="creating-a-vmss"></a>Egy VMSS létrehozása
#### <a name="create-resource-group"></a>Create resource group (Erőforráscsoport létrehozása)
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a>Hálózatkezelés létrehozása (virtuális Hálózatot / alhálózatot)
#### <a name="subnet-specification"></a>Alhálózati meghatározása
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a>Virtuális hálózat meghatározása
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume hello new subnet is hello only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-tooallow-external-access"></a>Nyilvános IP-erőforrás tooAllow külső hozzáférés létrehozása
Ez lesz a kötött toohello terheléselosztóhoz.

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a>Terheléselosztó létrehozása
```
$frontendName = 'fe' + $rgname
$backendAddressPoolName = 'bepool' + $rgname
$probeName = 'vmssprobe' + $rgname
$inboundNatPoolName = 'innatpool' + $rgname
$lbruleName = 'lbrule' + $rgname
$lbName = 'vmsslb' + $rgname

# Bind Public IP tooLoad Balancer
$frontend = New-AzureRmLoadBalancerFrontendIpConfig -Name $frontendName -PublicIpAddress $pubip
```

#### <a name="configure-load-balancer"></a>Terheléselosztó konfigurálása
Háttér cím készlet Config létrehozása, ez osztják meg hello hello virtuális gépek hálózati adapterek hello méretezési csoportban lévő.

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

Set terhelés kiegyensúlyozott mintavételi portot, az alkalmazás megfelelő hello-beállítások módosítása.

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

Hozzon létre egy bejövő NAT-készlete közvetlen útválasztásos kapcsolatot (nem elosztott terhelésű) toohello virtuális gépek méretezési hello beállítása hello terheléselosztó keresztül. Ez toodemonstrate RDP Funkciót használnak, és nincs szükség az alkalmazásban.

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

Hozzon létre hello elosztott terhelésű szabály betöltése ebben a példában látható betöltése terheléselosztási port 80 kérelem, az előző lépéseket hello-beállítások használatával.

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

Terheléselosztó létrehozása konfigurációjával kapcsolatban.

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

Betöltési LB beállítások ellenőrizze, elosztott terhelésű port configs, vegye figyelembe, nem jelenik meg, amíg hello méretezési csoportban lévő virtuális gépek hello bejövő forgalmat kezelő NAT-szabályok jönnek létre.

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-hello-scale-set"></a>Konfigurálja és hello méretezési létrehozása
Vegye figyelembe, hogy a infrastruktúra példa bemutatja, hogyan mentése tooset terjesztése és skálázási webes forgalom hello méretezési, de a virtuális gépek képek itt megadott hello között nincs bármely telepített webes szolgáltatás.

```
# specify scale set Name
$vmssName = 'vmss' + $rgname;

## specify VMSS specific details
$adminUsername = 'azadmin';
$adminPassword = "Password1234!";

$PublisherName = 'MicrosoftWindowsServer'
$Offer         = 'WindowsServer'
$Sku          = '2012-R2-Datacenter'
$Version       = 'latest'
$vmNamePrefix = 'winvmss'

###add an extension
$extname = 'BGInfo';
$publisher = 'Microsoft.Compute';
$exttype = 'BGInfo';
$extver = '2.1';
```

Terheléselosztó hálózati adapter tooLoad és alhálózati kötése

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

Hozzon létre a skála konfigurációs beállítása

```
# Specify number of nodes
$numberofnodes = 3

$vmss = New-AzureRmVmssConfig -Location $loc -SkuCapacity $numberofnodes -SkuName 'Standard_A2' -UpgradePolicyMode 'automatic' `
    | Add-AzureRmVmssNetworkInterfaceConfiguration -Name $subnetName -Primary $true -IPConfiguration $ipCfg `
    | Set-AzureRmVmssOSProfile -ComputerNamePrefix $vmNamePrefix -AdminUsername $adminUsername -AdminPassword $adminPassword `
    | Set-AzureRmVmssStorageProfile -OsDiskCreateOption 'FromImage' -OsDiskCaching 'None' `
    -ImageReferenceOffer $Offer -ImageReferenceSku $Sku -ImageReferenceVersion $Version `
    -ImageReferencePublisher $PublisherName `
    | Add-AzureRmVmssExtension -Name $extname -Publisher $publisher -Type $exttype -TypeHandlerVersion $extver -AutoUpgradeMinorVersion $true
```

Méretezési készlet konfiguráció

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

Most létrehozott hello méretezési készlet. Csatlakozás toohello tesztelheti az egyes virtuális gép RDP ebben a példában:

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
