---
title: "aaaAzure gyors üzembe helyezés - VM PowerShell létrehozása |} Microsoft Docs"
description: "Gyorsan további toocreate egy Linux virtuális gépek a PowerShell használatával"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f05ea7fedafe4fda660dc6084ae57ebf9dced473
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-powershell"></a><span data-ttu-id="746b9-103">Linux rendszerű virtuális gép létrehozása PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="746b9-103">Create a Linux virtual machine with PowerShell</span></span>

<span data-ttu-id="746b9-104">hello Azure PowerShell modul használt toocreate és hello PowerShell parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="746b9-104">hello Azure PowerShell module is used toocreate and manage Azure resources from hello PowerShell command line or in scripts.</span></span> <span data-ttu-id="746b9-105">Ez az útmutató adatokat hello Azure PowerShell modul toodeploy Ubuntu server operációs rendszert futtató virtuális gépek használata.</span><span class="sxs-lookup"><span data-stu-id="746b9-105">This guide details using hello Azure PowerShell module toodeploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="746b9-106">Miután hello kiszolgáló van telepítve, az SSH-kapcsolat jön létre, és egy NGINX-webkiszolgálón telepítve van.</span><span class="sxs-lookup"><span data-stu-id="746b9-106">Once hello server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="746b9-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="746b9-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="746b9-108">A gyors üzembe helyezési hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="746b9-108">This quick start requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="746b9-109">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="746b9-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="746b9-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="746b9-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>

<span data-ttu-id="746b9-111">Végül, egy nyilvános SSH-kulcs hello nevű *id_rsa.pub* hello tárolt toobe kell *.ssh* könyvtárát, a Windows felhasználói profilt.</span><span class="sxs-lookup"><span data-stu-id="746b9-111">Finally, a public SSH key with hello name *id_rsa.pub* needs toobe stored in hello *.ssh* directory of your Windows user profile.</span></span> <span data-ttu-id="746b9-112">Az Azure-hoz tartozó SSH-kulcsok létrehozásával kapcsolatos részletes információkért olvassa el az [SSH-kulcsok az Azure-hoz történő létrehozását](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="746b9-112">For detailed information on creating SSH keys for Azure, see [Create SSH keys for Azure](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="746b9-113">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="746b9-113">Log in tooAzure</span></span>

<span data-ttu-id="746b9-114">Jelentkezzen be Azure előfizetés hello tooyour `Login-AzureRmAccount` parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="746b9-114">Log in tooyour Azure subscription with hello `Login-AzureRmAccount` command and follow hello on-screen directions.</span></span>

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a><span data-ttu-id="746b9-115">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="746b9-115">Create resource group</span></span>

<span data-ttu-id="746b9-116">Hozzon létre egy Azure-erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) parancsmaggal.</span><span class="sxs-lookup"><span data-stu-id="746b9-116">Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="746b9-117">Az erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="746b9-117">A resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
```

## <a name="create-networking-resources"></a><span data-ttu-id="746b9-118">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="746b9-118">Create networking resources</span></span>

<span data-ttu-id="746b9-119">Hozzon létre egy virtuális hálózatot, egy alhálózatot és egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="746b9-119">Create a virtual network, subnet, and a public IP address.</span></span> <span data-ttu-id="746b9-120">Ezeket az erőforrásokat használt tooprovide hálózati kapcsolat toohello virtuális gépet, és csatlakoztassa toohello internet.</span><span class="sxs-lookup"><span data-stu-id="746b9-120">These resources are used tooprovide network connectivity toohello virtual machine and connect it toohello internet.</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork -ResourceGroupName myResourceGroup -Location eastus `
-Name MYvNET -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$pip = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Location eastus `
-AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"
```

<span data-ttu-id="746b9-121">Hozzon létre egy hálózati biztonsági csoportot és egy hálózati biztonsági csoportszabályt.</span><span class="sxs-lookup"><span data-stu-id="746b9-121">Create a network security group and a network security group rule.</span></span> <span data-ttu-id="746b9-122">hello hálózati biztonsági csoport hello virtuális gépet a bejövő és kimenő szabályok használatával titkosítja.</span><span class="sxs-lookup"><span data-stu-id="746b9-122">hello network security group secures hello virtual machine using inbound and outbound rules.</span></span> <span data-ttu-id="746b9-123">Ebben az esetben létrejön egy bejövő szabály a 22-es porthoz, amely lehetővé teszi a bejövő SSH-kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="746b9-123">In this case, an inbound rule is created for port 22, which allows incoming SSH connections.</span></span> <span data-ttu-id="746b9-124">Szeretnénk továbbá egy bejövő forgalomra vonatkozó szabály toocreate port 80, amely lehetővé teszi a bejövő webes forgalomnak.</span><span class="sxs-lookup"><span data-stu-id="746b9-124">We also want toocreate an inbound rule for port 80, which allows incoming web traffic.</span></span>

```powershell
# Create an inbound network security group rule for port 22
$nsgRuleSSH = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleSSH  -Protocol Tcp `
-Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 22 -Access Allow

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleWWW  -Protocol Tcp `
-Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 80 -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName myResourceGroup -Location eastus `
-Name myNetworkSecurityGroup -SecurityRules $nsgRuleSSH,$nsgRuleWeb
```

<span data-ttu-id="746b9-125">Hozzon létre egy hálózati kártya [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="746b9-125">Create a network card with [New-AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) for hello virtual machine.</span></span> <span data-ttu-id="746b9-126">hello hálózati kártya hello virtuális gép tooa alhálózat, a hálózati biztonsági csoport és a nyilvános IP-cím csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="746b9-126">hello network card connects hello virtual machine tooa subnet, network security group, and public IP address.</span></span>

```powershell
# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName myResourceGroup -Location eastus `
-SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```

## <a name="create-virtual-machine"></a><span data-ttu-id="746b9-127">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="746b9-127">Create virtual machine</span></span>

<span data-ttu-id="746b9-128">Hozzon létre egy virtuálisgép-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="746b9-128">Create a virtual machine configuration.</span></span> <span data-ttu-id="746b9-129">A konfiguráció hello virtuális gépek például a virtuálisgép-lemezkép mérete és hitelesítési konfiguráció telepítésekor használt hello beállításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="746b9-129">This configuration includes hello settings that are used when deploying hello virtual machine such as a virtual machine image, size, and authentication configuration.</span></span>

```powershell
# Define a credential object
$securePassword = ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Linux -ComputerName myVM -Credential $cred -DisablePasswordAuthentication | `
Set-AzureRmVMSourceImage -PublisherName Canonical -Offer UbuntuServer -Skus 14.04.2-LTS -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Configure SSH Keys
$sshPublicKey = Get-Content "$env:USERPROFILE\.ssh\id_rsa.pub"
Add-AzureRmVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"
```

<span data-ttu-id="746b9-130">Hozzon létre virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="746b9-130">Create hello virtual machine with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span>

```powershell
New-AzureRmVM -ResourceGroupName myResourceGroup -Location eastus -VM $vmConfig
```

## <a name="connect-toovirtual-machine"></a><span data-ttu-id="746b9-131">Csatlakoztassa a gépet toovirtual</span><span class="sxs-lookup"><span data-stu-id="746b9-131">Connect toovirtual machine</span></span>

<span data-ttu-id="746b9-132">Hello központi telepítés befejezése után hozzon létre egy SSH-kapcsolat hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="746b9-132">After hello deployment has completed, create an SSH connection with hello virtual machine.</span></span>

<span data-ttu-id="746b9-133">Használjon hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) tooreturn hello nyilvános IP-cím hello virtuális gép parancsot.</span><span class="sxs-lookup"><span data-stu-id="746b9-133">Use hello [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) command tooreturn hello public IP address of hello virtual machine.</span></span>

```powershell
Get-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

<span data-ttu-id="746b9-134">A telepített SSH rendszerből használt hello következő parancsot a tooconnect toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="746b9-134">From a system with SSH installed, used hello following command tooconnect toohello virtual machine.</span></span> <span data-ttu-id="746b9-135">Ha a Windows rendszeren működik [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) használt toocreate hello kapcsolat lehet.</span><span class="sxs-lookup"><span data-stu-id="746b9-135">If working on Windows, [Putty](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-ssh-from-windows?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#create-a-private-key-for-putty) can be used toocreate hello connection.</span></span> 

```bash 
ssh <Public IP Address>
```

<span data-ttu-id="746b9-136">Amikor a rendszer kéri, hello bejelentkezési felhasználói név az *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="746b9-136">When prompted, hello login user name is *azureuser*.</span></span> <span data-ttu-id="746b9-137">Ha egy hozzáférési kódot adott meg, ha SSH-kulcsok létrehozása, tooenter ebben is kell.</span><span class="sxs-lookup"><span data-stu-id="746b9-137">If a passphrase was entered when creating SSH keys, you need tooenter this as well.</span></span>


## <a name="install-nginx"></a><span data-ttu-id="746b9-138">Az NGINX telepítése</span><span class="sxs-lookup"><span data-stu-id="746b9-138">Install NGINX</span></span>

<span data-ttu-id="746b9-139">Használjon hello alábbi parancsfájl tooupdate csomag források bash, és hello legújabb NGINX csomag telepítése.</span><span class="sxs-lookup"><span data-stu-id="746b9-139">Use hello following bash script tooupdate package sources and install hello latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-ngix-welcome-page"></a><span data-ttu-id="746b9-140">Hello NGIX üdvözlőlap megtekintése</span><span class="sxs-lookup"><span data-stu-id="746b9-140">View hello NGIX welcome page</span></span>

<span data-ttu-id="746b9-141">Telepített NGINX és a virtuális gépre, az Internet - hello most nyissa meg a 80-as port az a választott tooview hello alapértelmezett NGINX üdvözlőlap webböngésző is használhatja.</span><span class="sxs-lookup"><span data-stu-id="746b9-141">With NGINX installed and port 80 now open on your VM from hello Internet - you can use a web browser of your choice tooview hello default NGINX welcome page.</span></span> <span data-ttu-id="746b9-142">Lehet, hogy toouse hello nyilvános IP-cím feletti toovisit hello alapértelmezett oldal részletes ismertetését lásd.</span><span class="sxs-lookup"><span data-stu-id="746b9-142">Be sure toouse hello public IP address you documented above toovisit hello default page.</span></span> 

![Alapértelmezett NGINX-webhely](./media/quick-create-cli/nginx.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="746b9-144">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="746b9-144">Clean up resources</span></span>

<span data-ttu-id="746b9-145">Ha már nincs szükség, használhatja a hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="746b9-145">When no longer needed, you can use hello [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command tooremove hello resource group, VM, and all related resources.</span></span>

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="746b9-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="746b9-146">Next steps</span></span>

<span data-ttu-id="746b9-147">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="746b9-147">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="746b9-148">További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Linux virtuális gépek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="746b9-148">toolearn more about Azure virtual machines, continue toohello tutorial for Linux VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="746b9-149">Azure-beli Linux rendszerű virtuális gépek – oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="746b9-149">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
