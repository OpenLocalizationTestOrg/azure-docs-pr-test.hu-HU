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
# <a name="creating-virtual-machine-scale-sets-using-powershell-cmdlets"></a><span data-ttu-id="ed9cd-103">PowerShell-parancsmagok használatával virtuálisgép-méretezési csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-103">Creating Virtual Machine Scale Sets using PowerShell cmdlets</span></span>
<span data-ttu-id="ed9cd-104">Ez a cikk végigvezeti egy példa hogyan toocreate egy virtuálisgép-méretezési csoportban (VMSS).</span><span class="sxs-lookup"><span data-stu-id="ed9cd-104">This article walks through an example of how toocreate a Virtual Machine scale set (VMSS).</span></span> <span data-ttu-id="ed9cd-105">Kapcsolódó hálózati és tárolási méretezési meg három csomópontok létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-105">It creates a scale set of three nodes, with associated Networking and Storage.</span></span>

## <a name="first-steps"></a><span data-ttu-id="ed9cd-106">Első lépések</span><span class="sxs-lookup"><span data-stu-id="ed9cd-106">First Steps</span></span>
<span data-ttu-id="ed9cd-107">Győződjön meg arról, hello legújabb Azure PowerShell-modul telepítve van, toomake meg arról, hogy a PowerShell parancsmagjaival hello toomaintain szükséges, és hozza létre a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-107">Ensure you have hello latest Azure PowerShell module installed, toomake sure you have hello PowerShell commandlets needed toomaintain, and create scale sets.</span></span>
<span data-ttu-id="ed9cd-108">Nyissa meg a parancssori eszközök toohello [Itt](http://aka.ms/webpi-azps) a hello a legújabb elérhető Azure-modulok.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-108">Go toohello command line tools [here](http://aka.ms/webpi-azps) for hello latest available Azure Modules.</span></span>

<span data-ttu-id="ed9cd-109">toofind VMSS kapcsolódó parancsmagok, hello keresési karakterlánc \*VMSS\*.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-109">toofind VMSS related commandlets, use hello search string \*VMSS\*.</span></span> <span data-ttu-id="ed9cd-110">Például _gcm *vmss*_</span><span class="sxs-lookup"><span data-stu-id="ed9cd-110">For example, _gcm *vmss*_</span></span>

## <a name="creating-a-vmss"></a><span data-ttu-id="ed9cd-111">Egy VMSS létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-111">Creating a VMSS</span></span>
#### <a name="create-resource-group"></a><span data-ttu-id="ed9cd-112">Create resource group (Erőforráscsoport létrehozása)</span><span class="sxs-lookup"><span data-stu-id="ed9cd-112">Create Resource Group</span></span>
```
$loc = 'westus';
$rgname = 'mynewrgwu';
  New-AzureRmResourceGroup -Name $rgname -Location $loc -Force;
```

### <a name="create-networking-vnet--subnet"></a><span data-ttu-id="ed9cd-113">Hálózatkezelés létrehozása (virtuális Hálózatot / alhálózatot)</span><span class="sxs-lookup"><span data-stu-id="ed9cd-113">Create Networking (VNET / Subnet)</span></span>
#### <a name="subnet-specification"></a><span data-ttu-id="ed9cd-114">Alhálózati meghatározása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-114">Subnet Specification</span></span>
```
$subnetName = 'websubnet'
  $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix "10.0.0.0/24";
```

#### <a name="vnet-specification"></a><span data-ttu-id="ed9cd-115">Virtuális hálózat meghatározása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-115">VNET Specification</span></span>
```
$vnet = New-AzureRmVirtualNetwork -Force -Name ('vnet' + $rgname) -ResourceGroupName $rgname -Location $loc -AddressPrefix "10.0.0.0/16" -Subnet $subnet;
$vnet = Get-AzureRmVirtualNetwork -Name ('vnet' + $rgname) -ResourceGroupName $rgname;

# In this case assume hello new subnet is hello only one
$subnetId = $vnet.Subnets[0].Id;
```

#### <a name="create-public-ip-resource-tooallow-external-access"></a><span data-ttu-id="ed9cd-116">Nyilvános IP-erőforrás tooAllow külső hozzáférés létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-116">Create Public IP Resource tooAllow External Access</span></span>
<span data-ttu-id="ed9cd-117">Ez lesz a kötött toohello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-117">This will be bound toohello Load Balancer.</span></span>

```
$pubip = New-AzureRmPublicIpAddress -Force -Name ('pubip' + $rgname) -ResourceGroupName $rgname -Location $loc -AllocationMethod Dynamic -DomainNameLabel ('pubip' + $rgname);
$pubip = Get-AzureRmPublicIpAddress -Name ('pubip' + $rgname) -ResourceGroupName $rgname;
```

#### <a name="create-load-balancer"></a><span data-ttu-id="ed9cd-118">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-118">Create Load Balancer</span></span>
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

#### <a name="configure-load-balancer"></a><span data-ttu-id="ed9cd-119">Terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-119">Configure Load Balancer</span></span>
<span data-ttu-id="ed9cd-120">Háttér cím készlet Config létrehozása, ez osztják meg hello hello virtuális gépek hálózati adapterek hello méretezési csoportban lévő.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-120">Create Backend Address Pool Config, this will be shared by hello NICs of hello VMs in hello scale set.</span></span>

```
$backendAddressPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name $backendAddressPoolName
```

<span data-ttu-id="ed9cd-121">Set terhelés kiegyensúlyozott mintavételi portot, az alkalmazás megfelelő hello-beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-121">Set Load Balanced Probe Port, change hello settings as appropriate for your application.</span></span>

```
$probe = New-AzureRmLoadBalancerProbeConfig -Name $probeName -RequestPath healthcheck.aspx -Protocol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2
```

<span data-ttu-id="ed9cd-122">Hozzon létre egy bejövő NAT-készlete közvetlen útválasztásos kapcsolatot (nem elosztott terhelésű) toohello virtuális gépek méretezési hello beállítása hello terheléselosztó keresztül.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-122">Create an inbound NAT pool for direct routed connectivity (not load balanced) toohello VMs in hello scale set via hello Load Balancer.</span></span> <span data-ttu-id="ed9cd-123">Ez toodemonstrate RDP Funkciót használnak, és nincs szükség az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-123">This is toodemonstrate using RDP and may not be required in your application.</span></span>

```
$frontendpoolrangestart = 3360
$frontendpoolrangeend = 3370
$backendvmport = 3389
$inboundNatPool = New-AzureRmLoadBalancerInboundNatPoolConfig -Name $inboundNatPoolName -FrontendIPConfigurationId `
$frontend.Id -Protocol Tcp -FrontendPortRangeStart $frontendpoolrangestart -FrontendPortRangeEnd $frontendpoolrangeend -BackendPort $backendvmport;
```

<span data-ttu-id="ed9cd-124">Hozzon létre hello elosztott terhelésű szabály betöltése ebben a példában látható betöltése terheléselosztási port 80 kérelem, az előző lépéseket hello-beállítások használatával.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-124">Create hello Load Balanced Rule, this example shows load balancing port 80 requests, using hello settings from previous steps.</span></span>

```
$protocol = 'Tcp'
$feLBPort = 80
$beLBPort = 80

$lbrule = New-AzureRmLoadBalancerRuleConfig -Name $lbruleName `
-FrontendIPConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -Protocol $protocol -FrontendPort $feLBPort -BackendPort $beLBPort `
-IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -Verbose;
```

<span data-ttu-id="ed9cd-125">Terheléselosztó létrehozása konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-125">Create Load Balancer with configuration.</span></span>

```
$actualLb = New-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname -Location $loc `
-FrontendIpConfiguration $frontend -BackendAddressPool $backendAddressPool `
-Probe $probe -LoadBalancingRule $lbrule -InboundNatPool $inboundNatPool -Verbose;
```

<span data-ttu-id="ed9cd-126">Betöltési LB beállítások ellenőrizze, elosztott terhelésű port configs, vegye figyelembe, nem jelenik meg, amíg hello méretezési csoportban lévő virtuális gépek hello bejövő forgalmat kezelő NAT-szabályok jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-126">Check  LB settings, check load balanced port configs, note, you will not see Inbound NAT rules until hello VMs in hello scale set are created.</span></span>

```
$expectedLb = Get-AzureRmLoadBalancer -Name $lbName -ResourceGroupName $rgname
```

##### <a name="configure-and-create-hello-scale-set"></a><span data-ttu-id="ed9cd-127">Konfigurálja és hello méretezési létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-127">Configure and Create hello scale set</span></span>
<span data-ttu-id="ed9cd-128">Vegye figyelembe, hogy a infrastruktúra példa bemutatja, hogyan mentése tooset terjesztése és skálázási webes forgalom hello méretezési, de a virtuális gépek képek itt megadott hello között nincs bármely telepített webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-128">Note, this infrastructure example shows how tooset up distribute and scale web traffic across hello scale set, but hello VMs Images specified here do not have any web services installed.</span></span>

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

<span data-ttu-id="ed9cd-129">Terheléselosztó hálózati adapter tooLoad és alhálózati kötése</span><span class="sxs-lookup"><span data-stu-id="ed9cd-129">Bind NIC tooLoad Balancer and Subnet</span></span>

```
$ipCfg = New-AzureRmVmssIPConfig -Name 'nic' `
-LoadBalancerInboundNatPoolsId $actualLb.InboundNatPools[0].Id `
-LoadBalancerBackendAddressPoolsId $actualLb.BackendAddressPools[0].Id `
-SubnetId $subnetId;
```

<span data-ttu-id="ed9cd-130">Hozzon létre a skála konfigurációs beállítása</span><span class="sxs-lookup"><span data-stu-id="ed9cd-130">Create scale set Config</span></span>

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

<span data-ttu-id="ed9cd-131">Méretezési készlet konfiguráció</span><span class="sxs-lookup"><span data-stu-id="ed9cd-131">Build scale set configuration</span></span>

```
New-AzureRmVmss -ResourceGroupName $rgname -Name $vmssName -VirtualMachineScaleSet $vmss -Verbose;
```

<span data-ttu-id="ed9cd-132">Most létrehozott hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="ed9cd-132">Now you have created hello scale set.</span></span> <span data-ttu-id="ed9cd-133">Csatlakozás toohello tesztelheti az egyes virtuális gép RDP ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="ed9cd-133">You can test connecting toohello individual VM using RDP in this example:</span></span>

```
VM0 : pubipmynewrgwu.westus.cloudapp.azure.com:3360
VM1 : pubipmynewrgwu.westus.cloudapp.azure.com:3361
VM2 : pubipmynewrgwu.westus.cloudapp.azure.com:3362
```
