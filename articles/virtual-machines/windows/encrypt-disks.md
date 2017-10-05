---
title: "A Windows Azure-ban lévő lemezek titkosítása |} Microsoft Docs"
description: "A fokozott biztonság Azure PowerShell használatával Windows virtuális gép virtuális lemezek titkosítása"
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
ms.openlocfilehash: 98b42b252a601af090579e3939f3c7ab91c3803b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a><span data-ttu-id="bca71-103">Virtuális lemezek a Windows virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="bca71-103">How to encrypt virtual disks on a Windows VM</span></span>
<span data-ttu-id="bca71-104">A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="bca71-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="bca71-105">Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="bca71-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="bca71-106">Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="bca71-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="bca71-107">Ez a cikk részletesen a Azure PowerShell használatával Windows virtuális gép virtuális lemezek titkosítása.</span><span class="sxs-lookup"><span data-stu-id="bca71-107">This article details how to encrypt virtual disks on a Windows VM using Azure PowerShell.</span></span> <span data-ttu-id="bca71-108">Emellett [Linux virtuális gépet az Azure CLI 2.0 titkosítása](../linux/encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="bca71-108">You can also [encrypt a Linux VM using the Azure CLI 2.0](../linux/encrypt-disks.md).</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="bca71-109">Lemeztitkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="bca71-109">Overview of disk encryption</span></span>
<span data-ttu-id="bca71-110">A Windows virtuális gépek virtuális lemezek vannak titkosítása a Bitlocker használatával.</span><span class="sxs-lookup"><span data-stu-id="bca71-110">Virtual disks on Windows VMs are encrypted at rest using Bitlocker.</span></span> <span data-ttu-id="bca71-111">Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="bca71-111">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="bca71-112">Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy a tanúsított FIPS 140-2 2. szint szabványok hardveres biztonsági modulokkal (HSM) a kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="bca71-112">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="bca71-113">Ezek a titkosítási kulcsok titkosítására és visszafejtésére a virtuális Géphez csatolt virtuális lemezek segítségével.</span><span class="sxs-lookup"><span data-stu-id="bca71-113">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="bca71-114">A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat.</span><span class="sxs-lookup"><span data-stu-id="bca71-114">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="bca71-115">Egy egyszerű Azure Active Directory szolgáltatás lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.</span><span class="sxs-lookup"><span data-stu-id="bca71-115">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="bca71-116">A virtuális gépek titkosításához a folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="bca71-116">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="bca71-117">Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="bca71-117">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="bca71-118">Állítsa be a titkosítási kulccsal, hogy a lemezek titkosítására használható.</span><span class="sxs-lookup"><span data-stu-id="bca71-118">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="bca71-119">Az Azure Key Vault beolvasni a titkosítási kulcsot, hozzon létre egy Azure Active Directory szolgáltatás egyszerű a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="bca71-119">To read the cryptographic key from the Azure Key Vault, create an Azure Active Directory service principal with the appropriate permissions.</span></span>
4. <span data-ttu-id="bca71-120">Adja ki a parancsot, a virtuális lemezek, adja meg az Azure Active Directory szolgáltatás egyszerű és megfelelő titkosítási használandó kulcs titkosításához.</span><span class="sxs-lookup"><span data-stu-id="bca71-120">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory service principal and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="bca71-121">Az Azure Active Directory szolgáltatás egyszerű kér az Azure Key Vault a szükséges titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="bca71-121">The Azure Active Directory service principal requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="bca71-122">A virtuális lemezek vannak titkosítva, a megadott titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="bca71-122">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="bca71-123">Titkosítási folyamat</span><span class="sxs-lookup"><span data-stu-id="bca71-123">Encryption process</span></span>
<span data-ttu-id="bca71-124">Adatok titkosítása a következő további összetevők támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="bca71-124">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="bca71-125">**Az Azure Key Vault** – a lemez titkosítási/visszafejtési folyamathoz használt titkosítási kulcsok és titkos biztosításához használt.</span><span class="sxs-lookup"><span data-stu-id="bca71-125">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span> 
  * <span data-ttu-id="bca71-126">Ha van ilyen, használhat egy meglévő Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="bca71-126">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="bca71-127">Nem jelölt ki a kulcstároló, a lemezek titkosító rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="bca71-127">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="bca71-128">Külön felügyeleti határokat és a kulcs látható, a dedikált kulcstároló is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="bca71-128">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="bca71-129">**Az Azure Active Directory** -kezeli a szükséges titkosítási kulcsokat és a kért műveletek hitelesítési biztonságos cseréjét.</span><span class="sxs-lookup"><span data-stu-id="bca71-129">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span> 
  * <span data-ttu-id="bca71-130">Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.</span><span class="sxs-lookup"><span data-stu-id="bca71-130">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="bca71-131">Az egyszerű szolgáltatás lehetővé teszi a biztonságos le tudja kérni, és adja ki a megfelelő titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="bca71-131">The service principal provides a secure mechanism to request and be issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="bca71-132">Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="bca71-132">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="bca71-133">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="bca71-133">Requirements and limitations</span></span>
<span data-ttu-id="bca71-134">Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="bca71-134">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="bca71-135">Az Azure piactéren elérhető rendszerkép vagy egyéni Virtuálismerevlemez-kép új Windows virtuális gépeken futó titkosítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bca71-135">Enabling encryption on new Windows VMs from Azure Marketplace images or custom VHD image.</span></span>
* <span data-ttu-id="bca71-136">A meglévő Windows virtuális gépeken, az Azure-titkosítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bca71-136">Enabling encryption on existing Windows VMs in Azure.</span></span>
* <span data-ttu-id="bca71-137">A Windows virtuális gépeken, a tárolóhelyek szolgáltatással konfigurált titkosítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bca71-137">Enabling encryption on Windows VMs that are configured using Storage Spaces.</span></span>
* <span data-ttu-id="bca71-138">Az operációs rendszer és az adatok titkosításának letiltása meghajtók Windows virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="bca71-138">Disabling encryption on OS and data drives for Windows VMs.</span></span>
* <span data-ttu-id="bca71-139">Az azonos Azure-régió, és az előfizetés összes erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bca71-139">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="bca71-140">Standard szint virtuális gépeket, például egy, virtuális gépek D, DS, G és GS adatsorozat.</span><span class="sxs-lookup"><span data-stu-id="bca71-140">Standard tier VMs, such as A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="bca71-141">Lemeztitkosítás jelenleg nem támogatott a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="bca71-141">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="bca71-142">Az alapszintű csomag virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="bca71-142">Basic tier VMs.</span></span>
* <span data-ttu-id="bca71-143">A virtuális gépek létrehozása a klasszikus telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="bca71-143">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="bca71-144">A titkosítási kulcsok egy már titkosított virtuális gép frissítése.</span><span class="sxs-lookup"><span data-stu-id="bca71-144">Updating the cryptographic keys on an already encrypted VM.</span></span>
* <span data-ttu-id="bca71-145">Integráció a helyszíni kulcskezelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="bca71-145">Integration with on-prem Key Management Service.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="bca71-146">Az Azure Key Vault és kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="bca71-146">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="bca71-147">A kezdés előtt győződjön meg arról, hogy az Azure PowerShell-modul legújabb verziója telepítve van.</span><span class="sxs-lookup"><span data-stu-id="bca71-147">Before you start, make sure that the latest version of the Azure PowerShell module has been installed.</span></span> <span data-ttu-id="bca71-148">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="bca71-148">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="bca71-149">Egész a parancspéldákban cserélje le minden példa paraméter a saját nevét, helyét és kulcsértékek.</span><span class="sxs-lookup"><span data-stu-id="bca71-149">Throughout the command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="bca71-150">Az alábbi példák a szabályt használ *myResourceGroup*, *myKeyVault*, *myVM*stb.</span><span class="sxs-lookup"><span data-stu-id="bca71-150">The following examples use a convention of *myResourceGroup*, *myKeyVault*, *myVM*, etc.</span></span>

<span data-ttu-id="bca71-151">Az első lépés, ha az Azure Key Vault a kriptográfiai kulcsok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="bca71-151">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="bca71-152">Az Azure Key Vault kulcsok, a titkos kulcsok és a jelszót, amely engedélyezi, hogy biztonságosan végrehajtja az alkalmazások és szolgáltatások a tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="bca71-152">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="bca71-153">A virtuális lemez titkosításához hozzon létre egy Key Vault titkosítására vagy visszafejtésére a virtuális lemezek használt kriptográfiai kulcs tárolása.</span><span class="sxs-lookup"><span data-stu-id="bca71-153">For virtual disk encryption, you create a Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span> 

<span data-ttu-id="bca71-154">Engedélyezze az Azure Key Vault-szolgáltató az Azure-előfizetése belül [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), majd hozzon létre egy erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="bca71-154">Enable the Azure Key Vault provider within your Azure subscription with [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), then create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="bca71-155">Az alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a a *USA keleti régiója* helye:</span><span class="sxs-lookup"><span data-stu-id="bca71-155">The following example creates a resource group name *myResourceGroup* in the *East US* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

<span data-ttu-id="bca71-156">Az Azure Key Vault a kriptográfiai kulcsokat és tartozó számítási erőforrásokat, például a tároló és a virtuális gépért tartalmazó ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bca71-156">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="bca71-157">Hozzon létre egy Azure Key Vault a [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) , és engedélyezze a Key Vault lemeztitkosítás való használatra.</span><span class="sxs-lookup"><span data-stu-id="bca71-157">Create an Azure Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="bca71-158">Adjon meg egy egyedi kulcstároló nevet *keyVaultName* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="bca71-158">Specify a unique Key Vault name for *keyVaultName* as follows:</span></span>

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

<span data-ttu-id="bca71-159">A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="bca71-159">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="bca71-160">A Key Vault prémium HSM használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="bca71-160">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="bca71-161">Nincs a prémium szabványos Key Vault szoftveresen védett tároló helyett kulcstároló létrehozásához további költségek.</span><span class="sxs-lookup"><span data-stu-id="bca71-161">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="bca71-162">A prémium kulcstároló létrehozásához az előző lépésben adja hozzá a *- Sku "Prémium"* paraméterek.</span><span class="sxs-lookup"><span data-stu-id="bca71-162">To create a premium Key Vault, in the preceding step add the *-Sku "Premium"* parameters.</span></span> <span data-ttu-id="bca71-163">Az alábbi példában szoftveresen védett azt a szabványos kulcstároló létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="bca71-163">The following example uses software-protected keys since we created a standard Key Vault.</span></span> 

<span data-ttu-id="bca71-164">Mindkét védelmi modellek esetében az Azure platformon kell hozzáférést kérni a titkosítási kulcsokat a virtuális lemezek visszafejtése a virtuális gép indításakor.</span><span class="sxs-lookup"><span data-stu-id="bca71-164">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="bca71-165">A kulcstároló, a titkosítási kulcs létrehozására [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span><span class="sxs-lookup"><span data-stu-id="bca71-165">Create a cryptographic key in your Key Vault with [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span></span> <span data-ttu-id="bca71-166">Az alábbi példakód létrehozza nevű kulcs *SajátKulcs*:</span><span class="sxs-lookup"><span data-stu-id="bca71-166">The following example creates a key named *myKey*:</span></span>

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a><span data-ttu-id="bca71-167">Az Azure Active Directory szolgáltatás egyszerű létrehozása</span><span class="sxs-lookup"><span data-stu-id="bca71-167">Create the Azure Active Directory service principal</span></span>
<span data-ttu-id="bca71-168">Ha a virtuális lemezek vannak titkosított vagy visszafejtett, meg kell adnia egy fiókot a hitelesítési és titkosítási kulcsok a Key Vault cseréjét.</span><span class="sxs-lookup"><span data-stu-id="bca71-168">When virtual disks are encrypted or decrypted, you specify an account to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="bca71-169">Ezt a fiókot, egy Azure Active Directory szolgáltatás egyszerű, lehetővé teszi, hogy az Azure platformon, kérje a megfelelő titkosítási kulcsokat a virtuális gép nevében.</span><span class="sxs-lookup"><span data-stu-id="bca71-169">This account, an Azure Active Directory service principal, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="bca71-170">Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.</span><span class="sxs-lookup"><span data-stu-id="bca71-170">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="bca71-171">Egy egyszerű szolgáltatás létrehozása az Azure Active Directoryban [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span><span class="sxs-lookup"><span data-stu-id="bca71-171">Create a service principal in Azure Active Directory with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span></span> <span data-ttu-id="bca71-172">Biztonságos jelszó megadásához kövesse a [jelszóházirendek és -korlátozások az Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span><span class="sxs-lookup"><span data-stu-id="bca71-172">To specify a secure password, follow the [Password policies and restrictions in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span></span>

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

<span data-ttu-id="bca71-173">Sikeresen vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs engedélyei lehetővé teszik az Azure Active Directory szolgáltatás egyszerű az a kulcsok beolvasása a értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="bca71-173">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory service principal to read the keys.</span></span> <span data-ttu-id="bca71-174">A kulcstároló, az engedélyeket [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span><span class="sxs-lookup"><span data-stu-id="bca71-174">Set permissions on your Key Vault with [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a><span data-ttu-id="bca71-175">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bca71-175">Create virtual machine</span></span>
<span data-ttu-id="bca71-176">A titkosítási folyamat teszteléséhez hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="bca71-176">To test the encryption process, let's create a VM.</span></span> <span data-ttu-id="bca71-177">Az alábbi példakód létrehozza a virtuális gépek nevű *myVM* használatával egy *Windows Server 2016 Datacenter* lemezképet:</span><span class="sxs-lookup"><span data-stu-id="bca71-177">The following example creates a VM named *myVM* using a *Windows Server 2016 Datacenter* image:</span></span>

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


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="bca71-178">Virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="bca71-178">Encrypt virtual machine</span></span>
<span data-ttu-id="bca71-179">A virtuális lemez titkosításához, kapcsolása együtt minden az előző összetevők működését:</span><span class="sxs-lookup"><span data-stu-id="bca71-179">To encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="bca71-180">Adja meg az Azure Active Directory szolgáltatás egyszerű és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="bca71-180">Specify the Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="bca71-181">Adja meg a Key Vault a titkosított lemezek metaadatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="bca71-181">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="bca71-182">Adja meg a titkosítási kulcsok használandó tényleges titkosításához és visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="bca71-182">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="bca71-183">Adja meg, hogy az operációsrendszer-lemezképet, az adatlemezek vagy az összes titkosításához.</span><span class="sxs-lookup"><span data-stu-id="bca71-183">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="bca71-184">Az a virtuális gép titkosítása [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) használata az Azure Key Vault-kulcsot és az Azure Active Directory szolgáltatás egyszerű hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="bca71-184">Encrypt your VM with [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) using the Azure Key Vault key and Azure Active Directory service principal credentials.</span></span> <span data-ttu-id="bca71-185">Az alábbi példa lekéri a kulccsal kapcsolatos adatokat, majd titkosítja a virtuális gép nevű *myVM*:</span><span class="sxs-lookup"><span data-stu-id="bca71-185">The following example retrieves all the key information then encrypts the VM named *myVM*:</span></span>

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

<span data-ttu-id="bca71-186">Fogadja el a rendszer a virtuális gép titkosítási folytatásához.</span><span class="sxs-lookup"><span data-stu-id="bca71-186">Accept the prompt to continue with the VM encryption.</span></span> <span data-ttu-id="bca71-187">A folyamat során a virtuális gép újraindul.</span><span class="sxs-lookup"><span data-stu-id="bca71-187">The VM restarts during the process.</span></span> <span data-ttu-id="bca71-188">Miután a titkosítási folyamat befejeződött, és a virtuális gép újraindítása, tekintse át a titkosítás állapotát az [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span><span class="sxs-lookup"><span data-stu-id="bca71-188">Once the encryption process completes and the VM has rebooted, review the encryption status with [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

<span data-ttu-id="bca71-189">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="bca71-189">The output is similar to the following example:</span></span>

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a><span data-ttu-id="bca71-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bca71-190">Next steps</span></span>
* <span data-ttu-id="bca71-191">További információ az Azure Key Vault kezeléséhez: [Key Vault beállítása a virtuális gépek](key-vault-setup.md).</span><span class="sxs-lookup"><span data-stu-id="bca71-191">For more information about managing Azure Key Vault, see [Set up Key Vault for virtual machines](key-vault-setup.md).</span></span>
* <span data-ttu-id="bca71-192">Lemeztitkosítás, például egy titkosított egyéni virtuális Gépre az Azure-ba, a feltöltendő előkészítése kapcsolatos további információk: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="bca71-192">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
