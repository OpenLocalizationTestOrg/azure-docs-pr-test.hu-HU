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
# <a name="secure-iis-web-server-with-ssl-certificates-on-a-windows-virtual-machine-in-azure"></a>Biztonságos Windows virtuális gépre az Azure-ban SSL-tanúsítványokat az IIS-webkiszolgáló
toosecure webkiszolgálók, a Secure Sockets később (SSL) tanúsítvány lehet tooencrypt webes forgalom használt. Ezek SSL-tanúsítványokat tárolhatja az Azure Key Vault, és a tanúsítványok tooWindows virtuális gépek (VM) biztonságos telepítéshez engedélyezése az Azure-ban. Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Egy Azure-tároló létrehozása
> * Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése
> * Hozzon létre egy virtuális Gépet, és hello IIS-webkiszolgáló telepítése
> * Hello tanúsítvány behelyezése hello VM és SSL-kötést az IIS konfigurálása

Ez az oktatóanyag hello Azure PowerShell 3,6 vagy újabb verziója szükséges. Futtatás ` Get-Module -ListAvailable AzureRM` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [telepítése Azure PowerShell modul](/powershell/azure/install-azurerm-ps).


## <a name="overview"></a>Áttekintés
Az Azure Key Vault megvédi a titkosítási kulcsok és titkos kulcsok, ilyen tanúsítványokat vagy jelszavakat. Key Vault segítségével teheti gördülékennyé hello tanúsítvány felügyeleti folyamatot, és lehetővé teszi a kulcsok ezeknek a tanúsítványoknak elérő toomaintain irányítását. Hozzon létre egy önaláírt tanúsítványt Key Vault belül, vagy töltsön fel egy meglévő, a megbízható tanúsítványt, amely már Ön a tulajdonosa.

Ahelyett, hogy az egyéni Virtuálisgép-lemezkép tanúsítványokat tartalmaz bővíthetőség a tanúsítványok behelyezése egy futó virtuális Gépet. Ez a folyamat biztosítja, hogy naprakész tanúsítványok hello telepítve vannak a webkiszolgáló üzembe helyezése során. Újítsa meg, vagy cserélje le a tanúsítványt, ha még nincs toocreate egy új egyéni Virtuálisgép-lemezkép. hello legújabb tanúsítványok automatikusan vannak olyanok, további virtuális gépek létrehozásakor. Hello teljes folyamat során hello tanúsítványok soha nem hagyja hello Azure platformon, vagy egy parancsfájl, a parancssori előzmények vagy a sablon ki vannak téve.


## <a name="create-an-azure-key-vault"></a>Egy Azure-tároló létrehozása
Tanúsítványok és a kulcstároló létrehozásához, hozzon létre egy erőforráscsoportot, a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupSecureWeb* a hello *USA keleti régiója* helye:

```powershell
$resourceGroup = "myResourceGroupSecureWeb"
$location = "East US"
New-AzureRmResourceGroup -ResourceGroupName $resourceGroup -Location $location
```

Ezután hozzon létre egy rendelkező Key Vaultból [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault). Minden Key Vault szükséges egy egyedi nevet, és minden kisbetű kell lennie. Cserélje le `<mykeyvault>` a következő példa a saját egyedi névvel Key Vault hello:

```powershell
$keyvaultName="<mykeyvault>"
New-AzureRmKeyVault -VaultName $keyvaultName `
    -ResourceGroup $resourceGroup `
    -Location $location `
    -EnabledForDeployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a>Egy tanúsítvány jön létre, és tárolja a Key Vault
Az éles környezetben való használathoz, importálnia kell a egy érvényes tanúsítvány megbízható-szolgáltató által aláírt [Import-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/import-azurekeyvaultcertificate). Ebben az oktatóanyagban hello következő példa bemutatja, hogyan hozhat létre egy önaláírt tanúsítványt [Add-AzureKeyVaultCertificate](/powershell/module/azurerm.keyvault/add-azurekeyvaultcertificate) , hogy a által használt alapértelmezett tanúsítási szabályzat a hello [ Új AzureKeyVaultCertificatePolicy](/powershell/module/azurerm.keyvault/new-azurekeyvaultcertificatepolicy). 

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


## <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása
Egy rendszergazdai jogosultságú felhasználónevet és jelszót hello tulajdonsággal rendelkező virtuális gépet set [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```powershell
$cred = Get-Credential
```

Most hozhat létre a virtuális gép hello [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). hello alábbi példa létrehoz hello szükséges virtuális hálózati összetevők hello az operációs rendszer konfigurációs, és létrehoz egy nevű virtuális gép *myVM*:

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

Hello VM toobe létrehozott néhány percet vesz igénybe. hello utolsó lépése használ hello Azure egyéni parancsprogramok futtatására szolgáló bővítmény tooinstall hello IIS-webkiszolgálón a [Set-AzureRmVmExtension](/powershell/module/azurerm.compute/set-azurermvmextension).


## <a name="add-a-certificate-toovm-from-key-vault"></a>A Key Vault egy tanúsítvány tooVM hozzáadása
tooadd hello tanúsítványt a Key Vault tooa VM, szerezze be a tanúsítvány hello azonosítója [Get-AzureKeyVaultSecret](/powershell/module/azurerm.keyvault/get-azurekeyvaultsecret). Hello tanúsítvány toohello virtuális gép hozzáadása a [Add-AzureRmVMSecret](/powershell/module/azurerm.compute/add-azurermvmsecret):

```powershell
$certURL=(Get-AzureKeyVaultSecret -VaultName $keyvaultName -Name "mycert").id

$vm=Get-AzureRMVM -ResourceGroupName $resourceGroup -Name "myVM"
$vaultId=(Get-AzureRmKeyVault -ResourceGroupName $resourceGroup -VaultName $keyVaultName).ResourceId
$vm = Add-AzureRmVMSecret -VM $vm -SourceVaultId $vaultId -CertificateStore "My" -CertificateUrl $certURL

Update-AzureRmVM -ResourceGroupName $resourceGroup -VM $vm
```


## <a name="configure-iis-toouse-hello-certificate"></a>IIS toouse hello tanúsítvány konfigurálása
Használja az egyéni parancsprogramok futtatására szolgáló bővítmény újra hello [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension) tooupdate hello IIS-konfigurációt. A frissítés a Key Vault tooIIS beszúrta hello tanúsítvány érvényes, és konfigurálja a hello webes kötés:

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


### <a name="test-hello-secure-web-app"></a>Teszt hello biztonságos webes alkalmazás
Hello nyilvános IP-címet a virtuális gép az beszerzése [Get-AzureRmPublicIPAddress](/powershell/resourcemanager/azurerm.network/get-azurermpublicipaddress). hello alábbi példa beszerzi hello IP-címet `myPublicIP` korábban létrehozott:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName $resourceGroup -Name "myPublicIP" | select "IpAddress"
```

Most nyisson meg egy webböngészőt, és írja be `https://<myPublicIP>` hello címsorába. Válassza ki a tooaccept hello biztonsági figyelmeztetés, ha egy önaláírt tanúsítványt használt **részletek** , majd **nyissa meg a weblap toohello**:

![Fogadja el a webes böngésző biztonsági figyelmeztetés](./media/tutorial-secure-web-server/browser-warning.png)

A biztonságos IIS webhely majd jelenik meg, mint például a következő hello:

![Futó biztonságos IIS webhely megtekintése](./media/tutorial-secure-web-server/secured-iis.png)


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban egy IIS webkiszolgálón az Azure Key Vault tárolt SSL-tanúsítvánnyal védett. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Egy Azure-tároló létrehozása
> * Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése
> * Hozzon létre egy virtuális Gépet, és hello IIS-webkiszolgáló telepítése
> * Hello tanúsítvány behelyezése hello VM és SSL-kötést az IIS konfigurálása

Kövesse a hivatkozást toosee előzetesen elkészített virtuálisgép-parancsprogram minták.

> [!div class="nextstepaction"]
> [Windows virtuális gép parancsfájl minták](./powershell-samples.md)