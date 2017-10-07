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
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Létrehozhat és telepíthet egy virtuálisgép-méretezési csoport
Virtuálisgép-méretezési csoportok könnyítheti meg toodeploy, és egyetlen egységként azonos virtuális gépek kezeléséhez. Méretezési csoportok hyperscale alkalmazások jól méretezhető és testre szabható számítási rétegét adja meg, és a Windows platform-lemezképek, Linux platformon képek, egyéni lemezképek és bővítmények támogatják. Méretezési csoportok kapcsolatos további információkért lásd: [virtuálisgép-méretezési csoportok](virtual-machine-scale-sets-overview.md).

Az oktatóanyag bemutatja, hogyan toocreate egy virtuálisgép-méretezési készlet **nélkül** használatával hello Azure-portálon. További információ a hogyan toouse hello Azure-portálon: [hogyan toocreate egy virtuálisgép-méretezési beállítása az Azure-portálon hello](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Azure Resource Manager erőforrásokra vonatkozó további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure

Azure CLI 2.0 vagy az Azure PowerShell toocreate terjedő skálán használata beállításához először toosign tooyour előfizetésben.

További információ a hogyan tooinstall, állítsa be, és jelentkezzen be az Azure parancssori felület vagy a PowerShell használatával, tooAzure: [Ismerkedés az Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) vagy [Ismerkedés az Azure PowerShell-parancsmagok](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Először toocreate olyan erőforráscsoport, hello virtuálisgép-méretezési csoport társítva van.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Az Azure parancssori felület létrehozása

Az Azure parancssori felület hozhat létre egy virtuálisgép-méretezési csatlakozhatnak beállítása. Ha nincs megadva alapértelmezett értékeket, az Ön megadták. Például, ha nem adja meg a virtuális hálózati adatok, a virtuális hálózat létrejötte meg. Ha nincs megadva a következő részek hello, akkor jön létre: 
- Terheléselosztó
- Virtuális hálózat
- A nyilvános IP-cím

Ha a megjeleníteni kívánt toouse hello virtuálisgép-méretezési csoportban lévő virtuálisgép-lemezkép kiválasztása hello, néhány lehetősége van:

- URN  
hello erőforrás azonosítója:  
**Win2012R2Datacenter**

- URN alias  
hello URN felhasználóbarát nevét:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Egyéni erőforrás-azonosító  
hello elérési tooan Azure-erőforrás:  
**/Subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/Providers/Microsoft.COMPUTE/Images/MyImage**

- Webes erőforrás  
hello elérési tooan HTTP URI:  
**http://contoso.BLOB.Core.Windows.NET/vhds/osdiskimage.vhd**

>[!TIP]
>Az elérhető rendszerkép listájának kaphat `az vm image list`.

toocreate egy virtuálisgép-méretezési csoport, meg kell adnia a következő hello:

- Erőforráscsoport 
- Név
- Operációsrendszer-lemezkép
- Hitelesítési adatok 
 
a következő példa hello létrehoz egy alapszintű virtuálisgép-méretezési (ebben a lépésben eltarthat néhány percig).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Hello parancs befejeződése után most létrehozott beállítani a virtuálisgép-méretezési fog rendelkezni. Szükség lehet tooget hello IP-cím hello virtuális gép úgy, hogy tooit is elérheti. A parancs a következő hello nagy mennyiségű hello virtuális gép (beleértve a hello IP-cím) különböző adatait kérheti le. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>A PowerShell létrehozása

PowerShell bonyolultabb, mint az Azure parancssori felület toouse. Az Azure parancssori felület alapértelmezett beállításokat biztosít a hálózattal kapcsolatos erőforrások (például a terheléselosztók, IP-címek és virtuális hálózatok), miközben PowerShell viszont nem. A PowerShell-lel lemezkép hivatkozó egy kicsit bonyolultabb túl. A következő parancsmagok hello kaphat a lemezképek:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

hello parancsmagok munkahelyi átirányítható sorrendben. Hogyan összes tooget hello a lemezképeket, például **2 USA nyugati régiója** régióban, amelynek hello neve, közzétevő, **microsoft** azt.

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

hello munkafolyamat egy virtuálisgép-méretezési csoport létrehozásához a következőképpen történik:

1. Hozzon létre egy konfigurációs objektumot, amely hello méretezési információk.
2. Hivatkozás hello alap operációs rendszer lemezképét.
3. Hello operációs rendszer beállításainak konfigurálása: hitelesítés, a virtuális gép nevének előtagja és a felhasználó/fázis.
4. A hálózatkezelés konfigurálását.
5. Hozzon létre hello méretezési készlet.

Ez a példa egy olyan számítógépen, amelyen Windows Server 2016 telepítve beállítása alapszintű két-példány méretezési hoz létre.

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

### <a name="using-a-custom-virtual-machine-image"></a>Egyéni virtuálisgép-lemezkép használatával
A skála állítson be úgy a saját egyéni rendszerképét helyett egy virtuálisgép-lemezkép hivatkozó hello gyűjteményből létrehozásakor hello _Set-AzureRmVmssStorageProfile_ parancs néz ki:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Létrehozás sablonból

A virtuálisgép-méretezési beállítása az Azure Resource Manager-sablon használatával telepítheti. Létrehozhat saját sablont, vagy használhatja a hello [sablon tárház](https://azure.microsoft.com/resources/templates/?term=vmss). Ezeket a sablonokat is telepíthető közvetlenül tooyour Azure-előfizetés.

>[!NOTE]
>toocreate JSON szövegfájl saját sablon létrehozása. Általános információ toocreate és sablon testreszabása, lásd: [Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-authoring-templates.md).

Egy minta sablon [a Githubon](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). További információ a hogyan toocreate és azzal a mintát, lásd: [minimális életképes méretezési](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>A Visual Studio eszközből létrehozása

A Visual Studio hozzon létre egy Azure erőforráscsoport-projekt, és adja hozzá egy virtuálisgép-méretezési csoport sablon tooit. Dönthet úgy, hogy kívánja-e tooimport azt a Githubból vagy hello Azure Web Application Gallery. A központi telepítés PowerShell-parancsfájlt is létrejön. További információkért lásd: [hogyan toocreate egy virtuálisgép-méretezési készlet a Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-hello-azure-portal"></a>Az Azure-portálon hello létrehozása

Azure-portálon hello tooquickly méretezési készlet létrehozása kényelmes megoldást kínál. További információkért lásd: [hogyan toocreate egy virtuálisgép-méretezési beállítása az Azure-portálon hello](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Következő lépések

További információ [adatlemezek](virtual-machine-scale-sets-attached-disks.md).

Ismerje meg, hogyan túl[kezelheti az alkalmazásokat](virtual-machine-scale-sets-deploy-app.md).
