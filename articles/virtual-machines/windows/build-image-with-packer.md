---
title: "Windows Azure Virtuálisgép-lemezképek létrehozása a csomagoló |} Microsoft Docs"
description: "Csomagoló használata Windows virtuális gépek létrehozását az Azure-ban"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 11a4a4d65be09e6c518836c25bb455a6df738dcb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-packer-to-create-windows-virtual-machine-images-in-azure"></a><span data-ttu-id="085df-103">Windows virtuális gép képek létrehozása az Azure-ban a csomagoló segítségével</span><span class="sxs-lookup"><span data-stu-id="085df-103">How to use Packer to create Windows virtual machine images in Azure</span></span>
<span data-ttu-id="085df-104">Minden virtuális gép (VM) az Azure-ban, amely meghatározza a Windows terjesztési és az operációs rendszer verziója lemezkép jön létre.</span><span class="sxs-lookup"><span data-stu-id="085df-104">Each virtual machine (VM) in Azure is created from an image that defines the Windows distribution and OS version.</span></span> <span data-ttu-id="085df-105">Lemezképek előre telepített alkalmazások és konfigurációk tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="085df-105">Images can include pre-installed applications and configurations.</span></span> <span data-ttu-id="085df-106">Az Azure piactéren sok első és a külső képek biztosít a leggyakrabban használt operációs rendszer, és alkalmazás környezetekben, vagy a saját egyéni lemezképek igényeinek igazított hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="085df-106">The Azure Marketplace provides many first and third-party images for most common OS' and application environments, or you can create your own custom images tailored to your needs.</span></span> <span data-ttu-id="085df-107">Ez a cikk részletesen a nyílt forráskódú eszköz [csomagoló](https://www.packer.io/) definiálására és egyéni lemezképeket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="085df-107">This article details how to use the open source tool [Packer](https://www.packer.io/) to define and build custom images in Azure.</span></span>


## <a name="create-azure-resource-group"></a><span data-ttu-id="085df-108">Azure erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="085df-108">Create Azure resource group</span></span>
<span data-ttu-id="085df-109">A létrehozási folyamat során a csomagoló ideiglenes Azure-erőforrások létrehozza a, a forrás Virtuálisgép-buildekről nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="085df-109">During the build process, Packer creates temporary Azure resources as it builds the source VM.</span></span> <span data-ttu-id="085df-110">A forrás virtuális gép képként rögzíti, meg kell határoznia egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="085df-110">To capture that source VM for use as an image, you must define a resource group.</span></span> <span data-ttu-id="085df-111">Ez az erőforráscsoport tárolja a csomagoló felépítési folyamat kimenetét.</span><span class="sxs-lookup"><span data-stu-id="085df-111">The output from the Packer build process is stored in this resource group.</span></span>

<span data-ttu-id="085df-112">Hozzon létre egy erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="085df-112">Create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="085df-113">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="085df-113">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"
New-AzureRmResourceGroup -Name $rgName -Location $location
```

## <a name="create-azure-credentials"></a><span data-ttu-id="085df-114">Az Azure hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="085df-114">Create Azure credentials</span></span>
<span data-ttu-id="085df-115">Csomagoló egyszerű szolgáltatás használatával végzi a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="085df-115">Packer authenticates with Azure using a service principal.</span></span> <span data-ttu-id="085df-116">Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a csomagoló használható.</span><span class="sxs-lookup"><span data-stu-id="085df-116">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Packer.</span></span> <span data-ttu-id="085df-117">Szabályozza, és adja meg az engedélyeket, hogy milyen műveletek a szolgáltatás egyszerű hajthat végre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="085df-117">You control and define the permissions as to what operations the service principal can perform in Azure.</span></span>

<span data-ttu-id="085df-118">Az egyszerű szolgáltatás létrehozása [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) és engedélyeket a szolgáltatás egyszerű létrehozása és erőforrások kezelése [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):</span><span class="sxs-lookup"><span data-stu-id="085df-118">Create a service principal with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) and assign permissions for the service principal to create and manage resources with [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment):</span></span>

```powershell
$sp = New-AzureRmADServicePrincipal -DisplayName "Azure Packer IKF" -Password "P@ssw0rd!"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="085df-119">A hitelesítéshez az Azure-ba is be kell szereznie a Azure bérlői és előfizetés-azonosító a [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):</span><span class="sxs-lookup"><span data-stu-id="085df-119">To authenticate to Azure, you also need to obtain your Azure tenant and subscription IDs with [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription):</span></span>

```powershell
$sub = Get-AzureRmSubscription
$sub.TenantId
$sub.SubscriptionId
```

<span data-ttu-id="085df-120">A két azonosító használja a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="085df-120">You use these two IDs in the next step.</span></span>


## <a name="define-packer-template"></a><span data-ttu-id="085df-121">Adja meg a csomagoló sablon</span><span class="sxs-lookup"><span data-stu-id="085df-121">Define Packer template</span></span>
<span data-ttu-id="085df-122">Lemezképeket, létrehozhat egy sablon JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="085df-122">To build images, you create a template as a JSON file.</span></span> <span data-ttu-id="085df-123">A sablon megadása a létrehozói és a tényleges felépítési folyamat végrehajtott provisioners.</span><span class="sxs-lookup"><span data-stu-id="085df-123">In the template, you define builders and provisioners that carry out the actual build process.</span></span> <span data-ttu-id="085df-124">Csomagoló rendelkezik egy [Azure webhelykiépítőt](https://www.packer.io/docs/builders/azure.html) , amely lehetővé teszi, hogy adható meg az Azure-erőforrások, például a szolgáltatás egyszerű létrehozott hitelesítő adatok az előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="085df-124">Packer has a [provisioner for Azure](https://www.packer.io/docs/builders/azure.html) that allows you to define Azure resources, such as the service principal credentials created in the preceding step.</span></span>

<span data-ttu-id="085df-125">Hozzon létre egy fájlt *windows.json* , majd illessze be a következő tartalmat.</span><span class="sxs-lookup"><span data-stu-id="085df-125">Create a file named *windows.json* and paste the following content.</span></span> <span data-ttu-id="085df-126">Adja meg a saját értékeket a következő:</span><span class="sxs-lookup"><span data-stu-id="085df-126">Enter your own values for the following:</span></span>

| <span data-ttu-id="085df-127">Paraméter</span><span class="sxs-lookup"><span data-stu-id="085df-127">Parameter</span></span>                           | <span data-ttu-id="085df-128">Beszerzési helyét</span><span class="sxs-lookup"><span data-stu-id="085df-128">Where to obtain</span></span> |
|-------------------------------------|----------------------------------------------------|
| <span data-ttu-id="085df-129">*client_id*</span><span class="sxs-lookup"><span data-stu-id="085df-129">*client_id*</span></span>                         | <span data-ttu-id="085df-130">Nézet szolgáltatás résztvevő-azonosító az`$sp.applicationId`</span><span class="sxs-lookup"><span data-stu-id="085df-130">View service principal ID with `$sp.applicationId`</span></span> |
| <span data-ttu-id="085df-131">*client_secret*</span><span class="sxs-lookup"><span data-stu-id="085df-131">*client_secret*</span></span>                     | <span data-ttu-id="085df-132">A megadott jelszó`$securePassword`</span><span class="sxs-lookup"><span data-stu-id="085df-132">Password you specified in `$securePassword`</span></span> |
| <span data-ttu-id="085df-133">*tenant_id*</span><span class="sxs-lookup"><span data-stu-id="085df-133">*tenant_id*</span></span>                         | <span data-ttu-id="085df-134">A kimeneti `$sub.TenantId` parancs</span><span class="sxs-lookup"><span data-stu-id="085df-134">Output from `$sub.TenantId` command</span></span> |
| <span data-ttu-id="085df-135">*ELŐFIZETÉS_AZONOSÍTÓJA*</span><span class="sxs-lookup"><span data-stu-id="085df-135">*subscription_id*</span></span>                   | <span data-ttu-id="085df-136">A kimeneti `$sub.SubscriptionId` parancs</span><span class="sxs-lookup"><span data-stu-id="085df-136">Output from `$sub.SubscriptionId` command</span></span> |
| <span data-ttu-id="085df-137">*object_id*</span><span class="sxs-lookup"><span data-stu-id="085df-137">*object_id*</span></span>                         | <span data-ttu-id="085df-138">A nézet szolgáltatás egyszerű objektum azonosítója:`$sp.Id`</span><span class="sxs-lookup"><span data-stu-id="085df-138">View service principal object ID with `$sp.Id`</span></span> |
| <span data-ttu-id="085df-139">*managed_image_resource_group_name*</span><span class="sxs-lookup"><span data-stu-id="085df-139">*managed_image_resource_group_name*</span></span> | <span data-ttu-id="085df-140">Az első lépésben létrehozott erőforráscsoport nevét</span><span class="sxs-lookup"><span data-stu-id="085df-140">Name of resource group you created in the first step</span></span> |
| <span data-ttu-id="085df-141">*managed_image_name*</span><span class="sxs-lookup"><span data-stu-id="085df-141">*managed_image_name*</span></span>                | <span data-ttu-id="085df-142">A felügyelt lemezképe létrehozott nevét</span><span class="sxs-lookup"><span data-stu-id="085df-142">Name for the managed disk image that is created</span></span> |

```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "0831b578-8ab6-40b9-a581-9a880a94aab1",
    "client_secret": "P@ssw0rd!",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",
    "object_id": "a7dfb070-0d5b-47ac-b9a5-cf214fff0ae2",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Windows",
    "image_publisher": "MicrosoftWindowsServer",
    "image_offer": "WindowsServer",
    "image_sku": "2016-Datacenter",

    "communicator": "winrm",
    "winrm_use_ssl": "true",
    "winrm_insecure": "true",
    "winrm_timeout": "3m",
    "winrm_username": "packer",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "type": "powershell",
    "inline": [
      "Add-WindowsFeature Web-Server",
      "if( Test-Path $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml ){ rm $Env:SystemRoot\\windows\\system32\\Sysprep\\unattend.xml -Force}",
      "& $Env:SystemRoot\\System32\\Sysprep\\Sysprep.exe /oobe /generalize /shutdown /quiet"
    ]
  }]
}
```

<span data-ttu-id="085df-143">Ez a sablon összeállít egy Windows Server 2016 virtuális Gépet, telepíti az IIS szolgáltatást, majd a virtuális Gépet a Sysprep használatúvá.</span><span class="sxs-lookup"><span data-stu-id="085df-143">This template builds a Windows Server 2016 VM, installs IIS, then generalizes the VM with Sysprep.</span></span>


## <a name="build-packer-image"></a><span data-ttu-id="085df-144">Csomagoló lemezkép</span><span class="sxs-lookup"><span data-stu-id="085df-144">Build Packer image</span></span>
<span data-ttu-id="085df-145">Ha még nincs telepítve a helyi számítógépre csomagoló [csomagoló telepítési utasításokat](https://www.packer.io/docs/install/index.html).</span><span class="sxs-lookup"><span data-stu-id="085df-145">If you don't already have Packer installed on your local machine, [follow the Packer installation instructions](https://www.packer.io/docs/install/index.html).</span></span>

<span data-ttu-id="085df-146">Megadásával hozhat létre a lemezképet a csomagoló sablonfájl az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="085df-146">Build the image by specifying your Packer template file as follows:</span></span>

```bash
./packer build windows.json
```

<span data-ttu-id="085df-147">A kimenet a fenti parancsok például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="085df-147">An example of the output from the preceding commands is as follows:</span></span>

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> task : Image deployment
==> azure-arm:  ->> dept : Engineering
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the certificate’s URL ...
==> azure-arm:  -> Key Vault Name        : ‘pkrkvpq0mthtbtt’
==> azure-arm:  -> Key Vault Secret Name : ‘packerKeyVaultSecret’
==> azure-arm:  -> Certificate URL       : ‘https://pkrkvpq0mthtbtt.vault.azure.net/secrets/packerKeyVaultSecret/8c7bd823e4fa44e1abb747636128adbb'
==> azure-arm: Setting the certificate’s URL ...
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> DeploymentName    : ‘pkrdppq0mthtbtt’
==> azure-arm: Getting the VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.55.35’
==> azure-arm: Waiting for WinRM to become available...
==> azure-arm: Connected to WinRM!
==> azure-arm: Provisioning with Powershell...
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-powershell-provisioner902510110
    azure-arm: #< CLIXML
    azure-arm:
    azure-arm: Success Restart Needed Exit Code      Feature Result
    azure-arm: ------- -------------- ---------      --------------
    azure-arm: True    No             Success        {Common HTTP Features, Default Document, D...
    azure-arm: <Objs Version=“1.1.0.1” xmlns=“http://schemas.microsoft.com/powershell/2004/04"><Obj S=“progress” RefId=“0"><TN RefId=“0”><T>System.Management.Automation.PSCustomObject</T><T>System.Object</T></TN><MS><I64 N=“SourceId”>1</I64><PR N=“Record”><AV>Preparing modules for first use.</AV><AI>0</AI><Nil /><PI>-1</PI><PC>-1</PC><T>Completed</T><SR>-1</SR><SD> </SD></PR></MS></Obj></Objs>
==> azure-arm: Querying the machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-pq0mthtbtt/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> ComputeName       : ‘pkrvmpq0mthtbtt’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm:  -> Compute Name              : ‘pkrvmpq0mthtbtt’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-pq0mthtbtt’
==> azure-arm: Deleting the temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. The artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```

<span data-ttu-id="085df-148">A virtuális gép létrehozása, a provisioners futtatására, és a központi telepítés tisztítása csomagoló néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="085df-148">It takes a few minutes for Packer to build the VM, run the provisioners, and clean up the deployment.</span></span>


## <a name="create-vm-from-azure-image"></a><span data-ttu-id="085df-149">Kép: Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="085df-149">Create VM from Azure Image</span></span>
<span data-ttu-id="085df-150">Állítsa a Rendszergazda felhasználónévvel és jelszóval rendelkező virtuális gépek [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential).</span><span class="sxs-lookup"><span data-stu-id="085df-150">Set an administrator username and password for the VMs with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential).</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="085df-151">Mostantól létrehozhat egy virtuális Gépet a lemezkép [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="085df-151">You can now create a VM from your Image with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="085df-152">Az alábbi példakód létrehozza a virtuális gépek nevű *myVM* a *myPackerImage*.</span><span class="sxs-lookup"><span data-stu-id="085df-152">The following example creates a VM named *myVM* from *myPackerImage*.</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod "Static" `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 80
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleWWW  `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 80 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Define the image created by Packer
$image = Get-AzureRMImage -ImageName myPackerImage -ResourceGroupName $rgName

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName myVM -VMSize Standard_DS2 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -Id $image.Id | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```

<span data-ttu-id="085df-153">A virtuális gép létrehozásához néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="085df-153">It takes a few minutes to create the VM.</span></span>


## <a name="test-vm-and-iis"></a><span data-ttu-id="085df-154">Virtuális gép és az IIS tesztelése</span><span class="sxs-lookup"><span data-stu-id="085df-154">Test VM and IIS</span></span>
<span data-ttu-id="085df-155">A nyilvános IP-címet a virtuális gép az beszerzése [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="085df-155">Obtain the public IP address of your VM with [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="085df-156">Az alábbi példa beolvassa az IP-címek *myPublicIP* korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="085df-156">The following example obtains the IP address for *myPublicIP* created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress `
    -ResourceGroupName $rgName `
    -Name "myPublicIP" | select "IpAddress"
```

<span data-ttu-id="085df-157">Beírhatja a nyilvános IP-címet a webböngésző.</span><span class="sxs-lookup"><span data-stu-id="085df-157">You can then enter the public IP address in to a web browser.</span></span>

![Alapértelmezett IIS-webhely](./media/build-image-with-packer/iis.png) 


## <a name="next-steps"></a><span data-ttu-id="085df-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="085df-159">Next steps</span></span>
<span data-ttu-id="085df-160">Ebben a példában a csomagoló már telepített IIS szolgáltatást egy Virtuálisgép-lemezkép létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="085df-160">In this example, you used Packer to create a VM image with IIS already installed.</span></span> <span data-ttu-id="085df-161">A Virtuálisgép-lemezkép mellett a meglévő központi telepítési munkafolyamatai, például segítségével telepítse az alkalmazást az Team Services, Ansible, Chef vagy Puppet lemezkép alapján létrehozott virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="085df-161">You can use this VM image alongside existing deployment workflows, such as to deploy your app to VMs created from the Image with Team Services, Ansible, Chef, or Puppet.</span></span>

<span data-ttu-id="085df-162">További példa csomagoló sablonokat más Windows disztribúciókkal, lásd: [a GitHub-tárház](https://github.com/hashicorp/packer/tree/master/examples/azure).</span><span class="sxs-lookup"><span data-stu-id="085df-162">For additional example Packer templates for other Windows distros, see [this GitHub repo](https://github.com/hashicorp/packer/tree/master/examples/azure).</span></span>