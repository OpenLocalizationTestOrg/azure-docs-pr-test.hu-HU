---
title: "aaaCreate egy Azure virtuálisgép-méretezési csoport |} Microsoft Docs"
description: "Létrehozhat és telepíthet a Linux vagy a Windows Azure virtuálisgép-méretezési beállítása az Azure parancssori felület, a PowerShell, a sablon vagy a Visual Studio."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 73de25c1dd2424e64655b3accfea848926e72f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a><span data-ttu-id="f1c2f-103">Létrehozhat és telepíthet egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="f1c2f-103">Create and deploy a virtual machine scale set</span></span>
<span data-ttu-id="f1c2f-104">Virtuálisgép-méretezési csoportok könnyítheti meg toodeploy, és egyetlen egységként azonos virtuális gépek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-104">Virtual machine scale sets make it easy for you toodeploy and manage identical virtual machines as a set.</span></span> <span data-ttu-id="f1c2f-105">Méretezési csoportok hyperscale alkalmazások jól méretezhető és testre szabható számítási rétegét adja meg, és a Windows platform-lemezképek, Linux platformon képek, egyéni lemezképek és bővítmények támogatják.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-105">Scale sets provide a highly scalable and customizable compute layer for hyperscale applications, and they support Windows platform images, Linux platform images, custom images, and extensions.</span></span> <span data-ttu-id="f1c2f-106">Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-méretezési csoportok](virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-106">For more information about scale sets, see [Virtual machine scale sets](virtual-machine-scale-sets-overview.md).</span></span>

<span data-ttu-id="f1c2f-107">Az oktatóanyag bemutatja, hogyan toocreate egy virtuálisgép-méretezési készlet **nélkül** használatával hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-107">This tutorial shows you how toocreate a virtual machine scale set **without** using hello Azure portal.</span></span> <span data-ttu-id="f1c2f-108">További információ a hogyan toouse hello Azure-portálon: [hogyan toocreate egy virtuálisgép-méretezési beállítása az Azure-portálon hello](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-108">For information about how toouse hello Azure portal, see [How toocreate a virtual machine scale set with hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

>[!NOTE]
><span data-ttu-id="f1c2f-109">Azure Resource Manager erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-109">For more information about Azure Resource Manager resources, see [Azure Resource Manager vs. classic deployment](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>

## <a name="sign-in-tooazure"></a><span data-ttu-id="f1c2f-110">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="f1c2f-110">Sign in tooAzure</span></span>

<span data-ttu-id="f1c2f-111">Azure CLI 2.0 vagy az Azure PowerShell toocreate terjedő skálán használata beállításához először toosign tooyour előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-111">If you're using Azure CLI 2.0 or Azure PowerShell toocreate a scale set, you first need toosign in tooyour subscription.</span></span>

<span data-ttu-id="f1c2f-112">További információ a hogyan tooinstall, állítsa be, és jelentkezzen be az Azure parancssori felület vagy a PowerShell használatával, tooAzure: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) vagy [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-112">For more information about how tooinstall, set up, and sign in tooAzure with Azure CLI or PowerShell, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) or [Get started with Azure PowerShell cmdlets](/powershell/azure/overview).</span></span>

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a><span data-ttu-id="f1c2f-113">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="f1c2f-113">Create a resource group</span></span>

<span data-ttu-id="f1c2f-114">Először toocreate olyan erőforráscsoport, hello virtuálisgép-méretezési csoport társítva van.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-114">You first need toocreate a resource group that hello virtual machine scale set is associated with.</span></span>

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a><span data-ttu-id="f1c2f-115">Az Azure parancssori felület létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1c2f-115">Create from Azure CLI</span></span>

<span data-ttu-id="f1c2f-116">Az Azure parancssori felület hozhat létre egy virtuálisgép-méretezési csatlakozhatnak beállítása.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-116">With Azure CLI, you can create a virtual machine scale set with minimal effort.</span></span> <span data-ttu-id="f1c2f-117">Ha nincs megadva alapértelmezett értékeket, az Ön megadták.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-117">If you omit default values, they are provided for you.</span></span> <span data-ttu-id="f1c2f-118">Például, ha nem adja meg a virtuális hálózati adatok, a virtuális hálózat létrejötte meg.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-118">For example, if you don't specify any virtual network information, a virtual network is created for you.</span></span> <span data-ttu-id="f1c2f-119">Ha nincs megadva a következő részek hello, akkor jön létre:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-119">If you omit hello following parts, they are created for you:</span></span> 
- <span data-ttu-id="f1c2f-120">Terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="f1c2f-120">A load balancer</span></span>
- <span data-ttu-id="f1c2f-121">Virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="f1c2f-121">A virtual network</span></span>
- <span data-ttu-id="f1c2f-122">A nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="f1c2f-122">A public IP address</span></span>

<span data-ttu-id="f1c2f-123">Ha a megjeleníteni kívánt toouse hello virtuálisgép-méretezési csoportban lévő virtuálisgép-lemezkép kiválasztása hello, néhány lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-123">When choosing hello virtual machine image that you want toouse on hello virtual machine scale set, you have a few choices:</span></span>

- <span data-ttu-id="f1c2f-124">URN</span><span class="sxs-lookup"><span data-stu-id="f1c2f-124">URN</span></span>  
<span data-ttu-id="f1c2f-125">hello erőforrás azonosítója:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-125">hello identifier of a resource:</span></span>  
<span data-ttu-id="f1c2f-126">**Win2012R2Datacenter**</span><span class="sxs-lookup"><span data-stu-id="f1c2f-126">**Win2012R2Datacenter**</span></span>

- <span data-ttu-id="f1c2f-127">URN alias</span><span class="sxs-lookup"><span data-stu-id="f1c2f-127">URN alias</span></span>  
<span data-ttu-id="f1c2f-128">hello URN felhasználóbarát nevét:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-128">hello friendly name of a URN:</span></span>  
<span data-ttu-id="f1c2f-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span><span class="sxs-lookup"><span data-stu-id="f1c2f-129">**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**</span></span>

- <span data-ttu-id="f1c2f-130">Egyéni erőforrás-azonosító</span><span class="sxs-lookup"><span data-stu-id="f1c2f-130">Custom resource id</span></span>  
<span data-ttu-id="f1c2f-131">hello elérési tooan Azure-erőforrás:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-131">hello path tooan Azure resource:</span></span>  
<span data-ttu-id="f1c2f-132">**/Subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/Providers/Microsoft.COMPUTE/Images/MyImage**</span><span class="sxs-lookup"><span data-stu-id="f1c2f-132">**/subscriptions/subscription-guid/resourceGroups/MyResourceGroup/providers/Microsoft.Compute/images/MyImage**</span></span>

- <span data-ttu-id="f1c2f-133">Webes erőforrás</span><span class="sxs-lookup"><span data-stu-id="f1c2f-133">Web resource</span></span>  
<span data-ttu-id="f1c2f-134">hello elérési tooan HTTP URI:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-134">hello path tooan HTTP URI:</span></span>  
<span data-ttu-id="f1c2f-135">**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.vhd**</span><span class="sxs-lookup"><span data-stu-id="f1c2f-135">**http://contoso.blob.core.windows.net/vhds/osdiskimage.vhd**</span></span>

>[!TIP]
><span data-ttu-id="f1c2f-136">Az elérhető rendszerkép listájának kaphat `az vm image list`.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-136">You can get a list of available images with `az vm image list`.</span></span>

<span data-ttu-id="f1c2f-137">toocreate egy virtuálisgép-méretezési csoport, meg kell adnia a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-137">toocreate a virtual machine scale set, you must specify hello following:</span></span>

- <span data-ttu-id="f1c2f-138">Erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="f1c2f-138">Resource group</span></span> 
- <span data-ttu-id="f1c2f-139">Név</span><span class="sxs-lookup"><span data-stu-id="f1c2f-139">Name</span></span>
- <span data-ttu-id="f1c2f-140">Operációsrendszer-lemezkép</span><span class="sxs-lookup"><span data-stu-id="f1c2f-140">Operating system image</span></span>
- <span data-ttu-id="f1c2f-141">Hitelesítési adatok</span><span class="sxs-lookup"><span data-stu-id="f1c2f-141">Authentication information</span></span> 
 
<span data-ttu-id="f1c2f-142">a következő példa hello létrehoz egy alapszintű virtuálisgép-méretezési (ebben a lépésben eltarthat néhány percig).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-142">hello following example creates a basic virtual machine scale set (this step might take a few minutes).</span></span>

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

<span data-ttu-id="f1c2f-143">Hello parancs befejeződése után most létrehozott beállítani a virtuálisgép-méretezési fog rendelkezni.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-143">Once hello command finishes you will now have your virtual machine scale set created.</span></span> <span data-ttu-id="f1c2f-144">Szükség lehet tooget hello IP-cím hello virtuális gép úgy, hogy tooit is elérheti.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-144">You may need tooget hello IP address of hello virtual machine so that you can connect tooit.</span></span> <span data-ttu-id="f1c2f-145">A parancs a következő hello nagy mennyiségű hello virtuális gép (beleértve a hello IP-cím) különböző adatait kérheti le.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-145">You can get a lot of different information about hello virtual machine (including hello IP address) with hello following command.</span></span> 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a><span data-ttu-id="f1c2f-146">A PowerShell létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1c2f-146">Create from PowerShell</span></span>

<span data-ttu-id="f1c2f-147">PowerShell bonyolultabb, mint az Azure parancssori felület toouse.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-147">PowerShell is more complicated toouse than Azure CLI.</span></span> <span data-ttu-id="f1c2f-148">Az Azure parancssori felület alapértelmezett beállításokat biztosít a hálózattal kapcsolatos erőforrások (például a terheléselosztók, IP-címek és virtuális hálózatok), miközben PowerShell viszont nem.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-148">While Azure CLI provides defaults for networking-related resources (such as load balancers, IP addresses, and virtual networks), PowerShell does not.</span></span> <span data-ttu-id="f1c2f-149">A PowerShell-lel lemezkép hivatkozó egy kicsit bonyolultabb túl.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-149">Referencing an image with PowerShell is a slightly more complicated too.</span></span> <span data-ttu-id="f1c2f-150">A következő parancsmagok hello kaphat a lemezképek:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-150">You can get images with hello following cmdlets:</span></span>

1. <span data-ttu-id="f1c2f-151">Get-AzureRMVMImagePublisher</span><span class="sxs-lookup"><span data-stu-id="f1c2f-151">Get-AzureRMVMImagePublisher</span></span>
2. <span data-ttu-id="f1c2f-152">Get-AzureRMVMImageOffer</span><span class="sxs-lookup"><span data-stu-id="f1c2f-152">Get-AzureRMVMImageOffer</span></span>
3. <span data-ttu-id="f1c2f-153">Get-AzureRmVMImageSku</span><span class="sxs-lookup"><span data-stu-id="f1c2f-153">Get-AzureRmVMImageSku</span></span>

<span data-ttu-id="f1c2f-154">hello parancsmagok munkahelyi átirányítható sorrendben.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-154">hello cmdlets work can be piped in sequence.</span></span> <span data-ttu-id="f1c2f-155">Hogyan összes tooget hello a lemezképeket, például **2 USA nyugati régiója** régióban, amelynek hello neve, közzétevő, **microsoft** azt.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-155">Here is an example of how tooget all images for hello **West US 2** region with a publisher that has hello name **microsoft** in it.</span></span>

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

<span data-ttu-id="f1c2f-156">hello munkafolyamat egy virtuálisgép-méretezési csoport létrehozásához a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-156">hello workflow for creating a virtual machine scale set is as follows:</span></span>

1. <span data-ttu-id="f1c2f-157">Hozzon létre egy konfigurációs objektumot, amely hello méretezési információk.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-157">Create a config object that holds information about hello scale set.</span></span>
2. <span data-ttu-id="f1c2f-158">Hivatkozás hello alap operációs rendszer lemezképét.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-158">Reference hello base OS image.</span></span>
3. <span data-ttu-id="f1c2f-159">Hello operációs rendszer beállításainak konfigurálása: hitelesítés, a virtuális gép nevének előtagja és a felhasználó/fázis.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-159">Configure hello operating system settings: authentication, VM name prefix, and user/pass.</span></span>
4. <span data-ttu-id="f1c2f-160">A hálózatkezelés konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-160">Configure networking.</span></span>
5. <span data-ttu-id="f1c2f-161">Hozzon létre hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-161">Create hello scale set.</span></span>

<span data-ttu-id="f1c2f-162">Ez a példa egy olyan számítógépen, amelyen Windows Server 2016 telepítve beállítása alapszintű két-példány méretezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-162">This example creates a basic two-instance scale set for a computer that has Windows Server 2016 installed.</span></span>

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from hello gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with hello virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create hello virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach hello virtual network toohello IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create hello scale set with hello config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a><span data-ttu-id="f1c2f-163">Egyéni virtuálisgép-lemezkép használatával</span><span class="sxs-lookup"><span data-stu-id="f1c2f-163">Using a custom virtual machine image</span></span>
<span data-ttu-id="f1c2f-164">A skála állítson be úgy a saját egyéni rendszerképét helyett egy virtuálisgép-lemezkép hivatkozó hello gyűjteményből létrehozásakor hello _Set-AzureRmVmssStorageProfile_ parancs néz ki:</span><span class="sxs-lookup"><span data-stu-id="f1c2f-164">If you are creating a scale set from your own custom image, instead of referencing a virtual machine image from hello gallery, hello _Set-AzureRmVmssStorageProfile_ command would look like this:</span></span>
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a><span data-ttu-id="f1c2f-165">Létrehozás sablonból</span><span class="sxs-lookup"><span data-stu-id="f1c2f-165">Create from a template</span></span>

<span data-ttu-id="f1c2f-166">A virtuálisgép-méretezési beállítása az Azure Resource Manager-sablon használatával telepítheti.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-166">You can deploy a virtual machine scale set by using an Azure Resource Manager template.</span></span> <span data-ttu-id="f1c2f-167">Létrehozhat saját sablont, vagy használhatja a hello [sablon tárház](https://azure.microsoft.com/resources/templates/?term=vmss).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-167">You can create your own template or use one from hello [template repository](https://azure.microsoft.com/resources/templates/?term=vmss).</span></span> <span data-ttu-id="f1c2f-168">Ezeket a sablonokat is telepíthető közvetlenül tooyour Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-168">These templates can be deployed directly tooyour Azure subscription.</span></span>

>[!NOTE]
><span data-ttu-id="f1c2f-169">toocreate JSON szövegfájl saját sablon létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-169">toocreate your own template, you create a JSON text file.</span></span> <span data-ttu-id="f1c2f-170">Általános információ toocreate és sablon testreszabása, lásd: [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-170">For general information about how toocreate and customize a template, see [Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="f1c2f-171">Egy minta sablon [a Githubon](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-171">A sample template is available [on GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set).</span></span> <span data-ttu-id="f1c2f-172">További információ a hogyan toocreate és azzal a mintát, lásd: [minimális életképes méretezési](.\virtual-machine-scale-sets-mvss-start.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-172">For more information about how toocreate and use that sample, see [Minimum viable scale set](.\virtual-machine-scale-sets-mvss-start.md).</span></span>

## <a name="create-from-visual-studio"></a><span data-ttu-id="f1c2f-173">A Visual Studio eszközből létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1c2f-173">Create from Visual Studio</span></span>

<span data-ttu-id="f1c2f-174">A Visual Studio hozzon létre egy Azure erőforráscsoport-projekt, és adja hozzá egy virtuálisgép-méretezési csoport sablon tooit.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-174">With Visual Studio, you can create an Azure resource group project and add a virtual machine scale set template tooit.</span></span> <span data-ttu-id="f1c2f-175">Dönthet úgy, hogy kívánja-e tooimport azt a Githubból vagy hello Azure Web Application Gallery.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-175">You can choose whether you want tooimport it from GitHub or hello Azure Web Application Gallery.</span></span> <span data-ttu-id="f1c2f-176">A központi telepítés PowerShell-parancsfájlt is létrejön.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-176">A deployment PowerShell script is also generated for you.</span></span> <span data-ttu-id="f1c2f-177">További információkért lásd: [hogyan toocreate egy virtuálisgép-méretezési készlet a Visual Studio](virtual-machine-scale-sets-vs-create.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-177">For more information, see [How toocreate a virtual machine scale set with Visual Studio](virtual-machine-scale-sets-vs-create.md).</span></span>

## <a name="create-from-hello-azure-portal"></a><span data-ttu-id="f1c2f-178">Az Azure-portálon hello létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1c2f-178">Create from hello Azure portal</span></span>

<span data-ttu-id="f1c2f-179">Azure-portálon hello tooquickly méretezési készlet létrehozása kényelmes megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="f1c2f-179">hello Azure portal provides a convenient way tooquickly create a scale set.</span></span> <span data-ttu-id="f1c2f-180">További információkért lásd: [hogyan toocreate egy virtuálisgép-méretezési beállítása az Azure-portálon hello](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-180">For more information, see [How toocreate a virtual machine scale set with hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1c2f-181">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1c2f-181">Next steps</span></span>

<span data-ttu-id="f1c2f-182">További információ [adatlemezek](virtual-machine-scale-sets-attached-disks.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-182">Learn more about [data disks](virtual-machine-scale-sets-attached-disks.md).</span></span>

<span data-ttu-id="f1c2f-183">Ismerje meg, hogyan túl[kezelheti az alkalmazásokat](virtual-machine-scale-sets-deploy-app.md).</span><span class="sxs-lookup"><span data-stu-id="f1c2f-183">Learn how too[manage your apps](virtual-machine-scale-sets-deploy-app.md).</span></span>
