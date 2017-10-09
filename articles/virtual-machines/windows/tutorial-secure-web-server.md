---
title: "az Azure-ban aaaSecure IIS SSL-tanúsítványok |} Microsoft Docs"
description: "Ismerje meg, hogyan toosecure hello az IIS webkiszolgáló SSL-tanúsítványokkal meg egy Windows Azure-ban"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/14/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 7a9e0ce07be2f55095fdb5347b64faf5caa4f7e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a><span data-ttu-id="60d39-103">Biztonságos Windows virtuális gépre az Azure-ban SSL-tanúsítványokat az IIS-webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="60d39-103">Secure IIS web server with SSL certificates on a Windows virtual machine in Azure</span></span>
<span data-ttu-id="60d39-104">toosecure webkiszolgálók, a Secure Sockets később (SSL) tanúsítvány lehet tooencrypt webes forgalom használt.</span><span class="sxs-lookup"><span data-stu-id="60d39-104">toosecure web servers, a Secure Sockets Later (SSL) certificate can be used tooencrypt web traffic.</span></span> <span data-ttu-id="60d39-105">Ezek SSL-tanúsítványokat tárolhatja az Azure Key Vault, és a tanúsítványok tooWindows virtuális gépek (VM) biztonságos telepítéshez engedélyezése az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="60d39-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates tooWindows virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="60d39-106">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="60d39-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="60d39-107">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="60d39-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="60d39-108">Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése</span><span class="sxs-lookup"><span data-stu-id="60d39-108">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="60d39-109">Hozzon létre egy virtuális Gépet, és hello IIS-webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="60d39-109">Create a VM and install hello IIS web server</span></span>
> * <span data-ttu-id="60d39-110">Hello tanúsítvány behelyezése hello VM és SSL-kötést az IIS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="60d39-110">Inject hello certificate into hello VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="60d39-111">Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="60d39-111">This tutorial requires hello Azure PowerShell module version 3.6 or later.</span></span> <span data-ttu-id="60d39-112">Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="60d39-112">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="60d39-113">Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="60d39-113">If you need tooupgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>


## <a name="overview"></a><span data-ttu-id="60d39-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="60d39-114">Overview</span></span>
<span data-ttu-id="60d39-115">Az Azure Key Vault megvédi a titkosítási kulcsok és titkos kulcsok, ilyen tanúsítványokat vagy jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="60d39-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="60d39-116">Key Vault segítségével teheti gördülékennyé hello tanúsítvány felügyeleti folyamatot, és lehetővé teszi a kulcsok ezeknek a tanúsítványoknak elérő toomaintain irányítását.</span><span class="sxs-lookup"><span data-stu-id="60d39-116">Key Vault helps streamline hello certificate management process and enables you toomaintain control of keys that access those certificates.</span></span> <span data-ttu-id="60d39-117">Hozzon létre egy önaláírt tanúsítványt Key Vault belül, vagy töltsön fel egy meglévő, a megbízható tanúsítványt, amely már Ön a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="60d39-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="60d39-118">Ahelyett, hogy az egyéni Virtuálisgép-lemezkép tanúsítványokat tartalmaz bővíthetőség a tanúsítványok behelyezése egy futó virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="60d39-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="60d39-119">Ez a folyamat biztosítja, hogy naprakész tanúsítványok hello telepítve vannak a webkiszolgáló üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="60d39-119">This process ensures that hello most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="60d39-120">Újítsa meg, vagy cserélje le a tanúsítványt, ha még nincs toocreate egy új egyéni Virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="60d39-120">If you renew or replace a certificate, you don't also have toocreate a new custom VM image.</span></span> <span data-ttu-id="60d39-121">hello legújabb tanúsítványok automatikusan vannak olyanok, további virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="60d39-121">hello latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="60d39-122">Hello teljes folyamat során hello tanúsítványok soha nem hagyja hello Azure platformon, vagy egy parancsfájl, a parancssori előzmények vagy a sablon ki vannak téve.</span><span class="sxs-lookup"><span data-stu-id="60d39-122">During hello whole process, hello certificates never leave hello Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="60d39-123">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="60d39-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="60d39-124">Tanúsítványok és a kulcstároló létrehozásához, hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="60d39-124">Before you can create a Key Vault and certificates, create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="60d39-125">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupSecureWeb* a hello *USA keleti régiója* helye:</span><span class="sxs-lookup"><span data-stu-id="60d39-125">hello following example creates a resource group named *myResourceGroupSecureWeb* in hello *East US* location:</span></span>

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

<span data-ttu-id="60d39-126">Ezután hozzon létre egy rendelkező Key Vaultból [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span><span class="sxs-lookup"><span data-stu-id="60d39-126">Next, create a Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault).</span></span> <span data-ttu-id="60d39-127">Minden Key Vault szükséges egy egyedi nevet, és minden kisbetű kell lennie.</span><span class="sxs-lookup"><span data-stu-id="60d39-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="60d39-128">Cserélje le `<mykeyvault>` a következő példa a saját egyedi névvel Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="60d39-128">Replace `<mykeyvault>` in hello following example with your own unique Key Vault name:</span></span>

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="60d39-129">Egy tanúsítvány jön létre, és tárolja a Key Vault</span><span class="sxs-lookup"><span data-stu-id="60d39-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="60d39-130">Az éles környezetben való használathoz, importálnia kell a egy érvényes tanúsítvány megbízható-szolgáltató által aláírt [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span><span class="sxs-lookup"><span data-stu-id="60d39-130">For production use, you should import a valid certificate signed by trusted provider with [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate).</span></span> <span data-ttu-id="60d39-131">Ebben az oktatóanyagban hello következő példa bemutatja, hogyan hozhat létre egy önaláírt tanúsítványt [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) , hogy a által használt alapértelmezett tanúsítási szabályzat a hello [ Új AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span><span class="sxs-lookup"><span data-stu-id="60d39-131">For this tutorial, hello following example shows how you can generate a self-signed certificate with [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) that uses hello default certificate policy from [New-AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy).</span></span> 

```powershell
$policy = New-AzureKeyVaultCertificatePolicy `
    -SubjectName "CN=www.contoso.com" `
    -SecretContentType "application/x-pkcs12" `
    -IssuerName Self `
    -ValidityInMonths 12

Add-AzureKeyVaultCertificate `
    -VaultName $keyvaultName `
    -Name "mycert" `
    -CertificatePolicy $policy 
```


## <a name="create-a-virtual-machine"></a><span data-ttu-id="60d39-132">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="60d39-132">Create a virtual machine</span></span>
<span data-ttu-id="60d39-133">Egy rendszergazdai jogosultságú felhasználónevet és jelszót hello tulajdonsággal rendelkező virtuális gépet set [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span><span class="sxs-lookup"><span data-stu-id="60d39-133">Set an administrator username and password for hello VM with [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):</span></span>

```powershell
$cred = Get-Credential
```

<span data-ttu-id="60d39-134">Most hozhat létre a virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="60d39-134">Now you can create hello VM with [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm).</span></span> <span data-ttu-id="60d39-135">hello alábbi példa létrehoz hello szükséges virtuális hálózati összetevők hello az operációs rendszer konfigurációs, és létrehoz egy nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="60d39-135">hello following example creates hello required virtual network components, hello OS configuration, and then creates a VM named *myVM*:</span></span>

```powershell
# Create a subnet configuration
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

# Create a virtual network
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myVnet" `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

# Create a public IP address and specify a DNS name
$publicIP = New-AzureRmPublicIpAddress `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "myPublicIP"

# Create an inbound network security group rule for port 3389
$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleRDP"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

# Create an inbound network security group rule for port 443
$nsgRuleWeb = New-AzureRmNetworkSecurityRuleConfig `
    -Name "myNetworkSecurityGroupRuleWWW"  `
    -Protocol "Tcp" `
    -Direction "Inbound" `
    -Priority 1001 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443 `
    -Access Allow

# Create a network security group
$nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name "myNetworkSecurityGroup" `
    -SecurityRules $nsgRuleRDP,$nsgRuleWeb

# Create a virtual network card and associate with public IP address and NSG
$nic = New-AzureRmNetworkInterface `
    -Name "myNic" `
    -ResourceGroupName $resourceGroup `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $publicIP.Id `
    -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration
$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS2" | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName "myVM" -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName "MicrosoftWindowsServer" `
    -Offer "WindowsServer" -Skus "2016-Datacenter" -Version "latest" | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

# Create virtual machine
New-AzureRmVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig

# Use hello Custom Script Extension tooinstall IIS
Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server -IncludeManagementTools"}'
```

<span data-ttu-id="60d39-136">Hello VM toobe létrehozott néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="60d39-136">It takes a few minutes for hello VM toobe created.</span></span> <span data-ttu-id="60d39-137">hello utolsó lépése használ hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall hello IIS-webkiszolgálón a [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span><span class="sxs-lookup"><span data-stu-id="60d39-137">hello last step uses hello Azure Custom Script Extension tooinstall hello IIS web server with [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).</span></span>


## <a name="add-a-certificate-toovm-from-key-vault"></a><span data-ttu-id="60d39-138">A Key Vault egy tanúsítvány tooVM hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60d39-138">Add a certificate tooVM from Key Vault</span></span>
<span data-ttu-id="60d39-139">tooadd hello tanúsítványt a Key Vault tooa VM, szerezze be a tanúsítvány hello azonosítója [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span><span class="sxs-lookup"><span data-stu-id="60d39-139">tooadd hello certificate from Key Vault tooa VM, obtain hello ID of your certificate with [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret).</span></span> <span data-ttu-id="60d39-140">Hello tanúsítvány toohello virtuális gép hozzáadása a [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span><span class="sxs-lookup"><span data-stu-id="60d39-140">Add hello certificate toohello VM with [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):</span></span>

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-toouse-hello-certificate"></a><span data-ttu-id="60d39-141">IIS toouse hello tanúsítvány konfigurálása</span><span class="sxs-lookup"><span data-stu-id="60d39-141">Configure IIS toouse hello certificate</span></span>
<span data-ttu-id="60d39-142">Használja az egyéni parancsprogramok futtatására szolgáló bővítmény újra hello [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooupdate hello IIS-konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="60d39-142">Use hello Custom Script Extension again with [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooupdate hello IIS configuration.</span></span> <span data-ttu-id="60d39-143">A frissítés a Key Vault tooIIS beszúrta hello tanúsítvány érvényes, és konfigurálja a hello webes kötés:</span><span class="sxs-lookup"><span data-stu-id="60d39-143">This update applies hello certificate injected from Key Vault tooIIS and configures hello web binding:</span></span>

```powershell
$PublicSettings = '{
    "fileUris":["https://raw.githubusercontent.com/iainfoulds/azure-samples/master/secure-iis.ps1"],
    "commandToExecute":"powershell -ExecutionPolicy Unrestricted -File secure-iis.ps1"
}'

Set-AzureRmVMExtension -ResourceGroupName $resourceGroup `
    -ExtensionName "IIS" `
    -VMName "myVM" `
    -Location $location `
    -Publisher "Microsoft.Compute" `
    -ExtensionType "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -SettingString $publicSettings
```


### <a name="test-hello-secure-web-app"></a><span data-ttu-id="60d39-144">Teszt hello biztonságos webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="60d39-144">Test hello secure web app</span></span>
<span data-ttu-id="60d39-145">Hello nyilvános IP-címet a virtuális gép az beszerzése [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span><span class="sxs-lookup"><span data-stu-id="60d39-145">Obtain hello public IP address of your VM with [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress).</span></span> <span data-ttu-id="60d39-146">hello alábbi példa beszerzi hello IP-címet `myPublicIP` korábban létrehozott:</span><span class="sxs-lookup"><span data-stu-id="60d39-146">hello following example obtains hello IP address for `myPublicIP` created earlier:</span></span>

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

<span data-ttu-id="60d39-147">Most nyisson meg egy webböngészőt, és írja be `https://<myPublicIP>` hello címsorába.</span><span class="sxs-lookup"><span data-stu-id="60d39-147">Now you can open a web browser and enter `https://<myPublicIP>` in hello address bar.</span></span> <span data-ttu-id="60d39-148">Válassza ki a tooaccept hello biztonsági figyelmeztetés, ha egy önaláírt tanúsítványt használt **részletek** , majd **nyissa meg a weblap toohello**:</span><span class="sxs-lookup"><span data-stu-id="60d39-148">tooaccept hello security warning if you used a self-signed certificate, select **Details** and then **Go on toohello webpage**:</span></span>

![Fogadja el a webes böngésző biztonsági figyelmeztetés](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="60d39-150">A biztonságos IIS webhely majd jelenik meg, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="60d39-150">Your secured IIS website is then displayed as in hello following example:</span></span>

![Futó biztonságos IIS webhely megtekintése](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a><span data-ttu-id="60d39-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60d39-152">Next steps</span></span>

<span data-ttu-id="60d39-153">Ebben az oktatóanyagban egy IIS webkiszolgálón az Azure Key Vault tárolt SSL-tanúsítvánnyal védett.</span><span class="sxs-lookup"><span data-stu-id="60d39-153">In this tutorial, you secured an IIS web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="60d39-154">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="60d39-154">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="60d39-155">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="60d39-155">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="60d39-156">Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése</span><span class="sxs-lookup"><span data-stu-id="60d39-156">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="60d39-157">Hozzon létre egy virtuális Gépet, és hello IIS-webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="60d39-157">Create a VM and install hello IIS web server</span></span>
> * <span data-ttu-id="60d39-158">Hello tanúsítvány behelyezése hello VM és SSL-kötést az IIS konfigurálása</span><span class="sxs-lookup"><span data-stu-id="60d39-158">Inject hello certificate into hello VM and configure IIS with an SSL binding</span></span>

<span data-ttu-id="60d39-159">Kövesse a hivatkozást toosee előzetesen elkészített virtuálisgép-parancsprogram minták.</span><span class="sxs-lookup"><span data-stu-id="60d39-159">Follow this link toosee pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="60d39-160">Windows virtuális gép parancsfájl minták</span><span class="sxs-lookup"><span data-stu-id="60d39-160">Windows virtual machine script samples</span></span>](./powershell-samples.md)