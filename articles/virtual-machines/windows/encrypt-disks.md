---
title: a Windows Azure-ban aaaEncrypt lemezein |} Microsoft Docs
description: "Hogyan Windows virtuális gép virtuális lemezein tooencrypt fokozott biztonsági Azure PowerShell használatával"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/10/2017
ms.author: iainfou
ms.openlocfilehash: 77c42a67cb57a9dc5fe3159fce0be75e3a965be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-windows-vm"></a>Hogyan tooencrypt virtuális lemezek a Windows virtuális gép
A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek titkosíthatók. Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával. Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók. Ez a következő cikket: részletek hogyan tooencrypt virtuális lemezek a Windows virtuális gép Azure PowerShell használatával. Emellett [Linux virtuális gépet az Azure CLI 2.0 hello titkosítása](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Lemeztitkosítás áttekintése
A Windows virtuális gépek virtuális lemezek vannak titkosítása a Bitlocker használatával. Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására. Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy Kulcslétrehozási a hardveres biztonsági modulokkal (HSM) a hitelesített tooFIPS 140-2 2. szint szabványoknak. A kriptográfiai kulcsokat használt tooencrypt és virtuális lemezek csatolt tooyour VM visszafejtéséhez. A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat. Egy egyszerű Azure Active Directory szolgáltatás lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.

a virtuális gépek titkosításához hello folyamat a következőképpen történik:

1. Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.
2. Konfigurálja a hello kriptográfiai kulcs toobe lemezek titkosítására használható.
3. tooread hello titkosítási kulcsot az Azure Key Vault hello hozzon létre egy Azure Active Directory szolgáltatás egyszerű hello megfelelő engedélyekkel.
4. Hello parancs tooencrypt ki a virtuális lemezek, megadva hello Azure Active Directory szolgáltatás egyszerű és a megfelelő titkosítási kulcs toobe használt.
5. hello Azure Active Directory szolgáltatás egyszerű kérelmek hello szükséges titkosítási kulcsot az Azure Key Vault.
6. virtuális lemezek hello hello megadott titkosítási kulccsal titkosított.

## <a name="encryption-process"></a>Titkosítási folyamat
Adatok titkosítása a következő további összetevők hello támaszkodik:

* **Az Azure Key Vault** -használt toosafeguard titkosítási kulcsok és titkos hello lemez titkosítási/visszafejtési folyamat használja. 
  * Ha van ilyen, használhat egy meglévő Azure Key Vault. Nincs toodedicate a Key Vault tooencrypting lemezek.
  * tooseparate felügyeleti határokat és kulcs látható, a dedikált kulcstároló is létrehozhat.
* **Az Azure Active Directory** - leírók hello szükséges titkosítási kulcsok biztonságos cseréjét, és hitelesítési a kért műveleteket. 
  * Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.
  * egyszerű hello szolgáltatás egy biztonságos mechanizmus toorequest és hello megfelelő titkosítási kulcsok adja ki. Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.

## <a name="requirements-and-limitations"></a>Követelmények és korlátozások
Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:

* Az Azure piactéren elérhető rendszerkép vagy egyéni Virtuálismerevlemez-kép új Windows virtuális gépeken futó titkosítás engedélyezése.
* A meglévő Windows virtuális gépeken, az Azure-titkosítás engedélyezése.
* A Windows virtuális gépeken, a tárolóhelyek szolgáltatással konfigurált titkosítás engedélyezése.
* Az operációs rendszer és az adatok titkosításának letiltása meghajtók Windows virtuális gépek esetén.
* Minden erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie a hello azonos Azure-régió, és az előfizetés.
* Standard szint virtuális gépeket, például egy, virtuális gépek D, DS, G és GS adatsorozat.

Lemeztitkosítás jelenleg nem támogatott a következő forgatókönyvek hello:

* Az alapszintű csomag virtuális gépeket.
* A virtuális gépek hello klasszikus telepítési modellel készült.
* Egy már titkosított virtuális gép hello titkosítási kulcsok frissítése.
* Integráció a helyszíni kulcskezelő szolgáltatást.

## <a name="create-azure-key-vault-and-keys"></a>Az Azure Key Vault és kulcsok létrehozása
Mielőtt elkezdené, győződjön meg arról, hogy hello legújabb verziójának hello Azure PowerShell modul telepítve van listájában. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview). Teljes hello parancspéldákban cserélje le minden példa paraméter a saját nevét, helyét és kulcsértékek. hello alábbi példák szabályt használ a *myResourceGroup*, *myKeyVault*, *myVM*stb.

első lépés hello van egy Azure Key Vault toostore toocreate a titkosítási kulcsokat. Az Azure Key Vault tárolhatja a kulcsokat, a titkos kulcsok, vagy a jelszavak, amelyek lehetővé teszik toosecurely végrehajtja az alkalmazások és szolgáltatások. A virtuális lemez titkosításához hozzon létre egy Key Vault toostore használt tooencrypt a titkosítási kulcs, vagy a virtuális lemezek visszafejtéséhez. 

Engedélyezi az Azure-előfizetése belül hello Azure Key Vault szolgáltatót [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), majd hozzon létre egy erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). hello alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a hello *USA keleti régiója* helye:

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

hello Azure Key Vault tartalmazó hello kriptográfiai kulcsokat és társított számítási erőforrások, például a tárolási és hello virtuális gépért kell lennie, hello ugyanabban a régióban. Hozzon létre egy Azure Key Vault a [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) , és engedélyezze a Key Vault hello lemeztitkosítás való használatra. Adjon meg egy egyedi kulcstároló nevet *keyVaultName* az alábbiak szerint:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja. A Key Vault prémium HSM használata szükséges. Van egy további költség nélkül toocreating szabványos Key Vault szoftveresen védett tároló helyett a Key Vault támogatás. a prémium kulcstároló, megelőző lépés hello a toocreate hozzáadása hello *- Sku "Prémium"* paraméterek. hello alábbi példa szoftveresen védett azt a szabványos kulcstároló létrehozása óta. 

Mindkét védelmi modellek hello Azure platformon kell toobe kap hozzáférést toorequest hello titkosítási kulcsok toodecrypt hello virtuális lemezek hello virtuális gép indításakor. A kulcstároló, a titkosítási kulcs létrehozására [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey). hello alábbi példa létrehoz egy nevű kulcsot *SajátKulcs*:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-hello-azure-active-directory-service-principal"></a>Hello Azure Active Directory szolgáltatás egyszerű létrehozása
Amikor a virtuális lemezek vannak titkosított vagy visszafejtett, megadhatja egy toohandle hello hitelesítése és a Key Vault a titkosítási kulcsok cseréjét. Ezt a fiókot, egy Azure Active Directory szolgáltatás egyszerű, lehetővé teszi, hogy hello Azure platformon toorequest hello megfelelő titkosítási kulcsok hello virtuális gép nevében. Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.

Egy egyszerű szolgáltatás létrehozása az Azure Active Directoryban [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal). a biztonságos jelszó toospecify hello kövesse [jelszóházirendek és -korlátozások az Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

toosuccessfully vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs hello engedélyeinek beállítása toopermit hello Azure Active Directory szolgáltatás egyszerű tooread hello kulcsok kell lennie. A kulcstároló, az engedélyeket [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
tootest hello titkosítási folyamat, hozzon létre egy virtuális Gépet. hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* használatával egy *Windows Server 2016 Datacenter* lemezképet:

```powershell
$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName `
    -Location $location `
    -Name myVnet `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName `
    -Location $location `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4 `
    -Name "mypublicdns$(Get-Random)"

$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName `
    -Location $location `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface -Name myNic `
    -ResourceGroupName $rgName `
    -Location $location `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$cred = Get-Credential

$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```


## <a name="encrypt-virtual-machine"></a>Virtuális gép titkosítása
tooencrypt hello virtuális lemezeket, akkor egyesítik összes hello az előző összetevők működését:

1. Adja meg a hello Azure Active Directory szolgáltatás egyszerű és a jelszót.
2. Adja meg a hello Key Vault toostore hello metaadatait a titkosított lemezek.
3. Adja meg a hello titkosítási kulcsok toouse hello tényleges titkosításához és visszafejtéséhez.
4. Adja meg, hogy tooencrypt hello operációsrendszer-lemez, hello adatlemezek vagy az összes.

Az a virtuális gép titkosítása [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) hello Azure Key Vault kulcs és az Azure Active Directory szolgáltatás egyszerű hitelesítő adatok használatával. hello alábbi példa minden hello kulcs adatainak beolvasása és titkosítja a hello nevű virtuális gép *myVM*:

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName `
    -VMName $vmName `
    -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword `
    -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId `
    -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Fogadja el a hello Rákérdezés toocontinue hello VM titkosítással. virtuális gép hello hello folyamat során újraindul. Miután hello titkosítási folyamat befejeződött, és hello virtuális gép újraindítása, tekintse át a titkosítási állapot hello [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

hello hasonló toohello a következő példa a kimenetre:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Következő lépések
* További információ az Azure Key Vault kezeléséhez: [Key Vault beállítása a virtuális gépek](key-vault-setup.md).
* További információ a lemez titkosítása például egy titkosított egyéni VM tooupload tooAzure, előkészítése: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
