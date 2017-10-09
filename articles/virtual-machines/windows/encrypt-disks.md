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
# <a name="how-tooencrypt-virtual-disks-on-a-windows-vm"></a><span data-ttu-id="6254e-103">Hogyan tooencrypt virtuális lemezek a Windows virtuális gép</span><span class="sxs-lookup"><span data-stu-id="6254e-103">How tooencrypt virtual disks on a Windows VM</span></span>
<span data-ttu-id="6254e-104">A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="6254e-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="6254e-105">Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="6254e-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="6254e-106">Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="6254e-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="6254e-107">Ez a következő cikket: részletek hogyan tooencrypt virtuális lemezek a Windows virtuális gép Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="6254e-107">This article details how tooencrypt virtual disks on a Windows VM using Azure PowerShell.</span></span> <span data-ttu-id="6254e-108">Emellett [Linux virtuális gépet az Azure CLI 2.0 hello titkosítása](../linux/encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="6254e-108">You can also [encrypt a Linux VM using hello Azure CLI 2.0](../linux/encrypt-disks.md).</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="6254e-109">Lemeztitkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="6254e-109">Overview of disk encryption</span></span>
<span data-ttu-id="6254e-110">A Windows virtuális gépek virtuális lemezek vannak titkosítása a Bitlocker használatával.</span><span class="sxs-lookup"><span data-stu-id="6254e-110">Virtual disks on Windows VMs are encrypted at rest using Bitlocker.</span></span> <span data-ttu-id="6254e-111">Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="6254e-111">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="6254e-112">Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy Kulcslétrehozási a hardveres biztonsági modulokkal (HSM) a hitelesített tooFIPS 140-2 2. szint szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="6254e-112">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="6254e-113">A kriptográfiai kulcsokat használt tooencrypt és virtuális lemezek csatolt tooyour VM visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="6254e-113">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="6254e-114">A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat.</span><span class="sxs-lookup"><span data-stu-id="6254e-114">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="6254e-115">Egy egyszerű Azure Active Directory szolgáltatás lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.</span><span class="sxs-lookup"><span data-stu-id="6254e-115">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="6254e-116">a virtuális gépek titkosításához hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="6254e-116">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="6254e-117">Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6254e-117">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="6254e-118">Konfigurálja a hello kriptográfiai kulcs toobe lemezek titkosítására használható.</span><span class="sxs-lookup"><span data-stu-id="6254e-118">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="6254e-119">tooread hello titkosítási kulcsot az Azure Key Vault hello hozzon létre egy Azure Active Directory szolgáltatás egyszerű hello megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="6254e-119">tooread hello cryptographic key from hello Azure Key Vault, create an Azure Active Directory service principal with hello appropriate permissions.</span></span>
4. <span data-ttu-id="6254e-120">Hello parancs tooencrypt ki a virtuális lemezek, megadva hello Azure Active Directory szolgáltatás egyszerű és a megfelelő titkosítási kulcs toobe használt.</span><span class="sxs-lookup"><span data-stu-id="6254e-120">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory service principal and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="6254e-121">hello Azure Active Directory szolgáltatás egyszerű kérelmek hello szükséges titkosítási kulcsot az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6254e-121">hello Azure Active Directory service principal requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="6254e-122">virtuális lemezek hello hello megadott titkosítási kulccsal titkosított.</span><span class="sxs-lookup"><span data-stu-id="6254e-122">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="6254e-123">Titkosítási folyamat</span><span class="sxs-lookup"><span data-stu-id="6254e-123">Encryption process</span></span>
<span data-ttu-id="6254e-124">Adatok titkosítása a következő további összetevők hello támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="6254e-124">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="6254e-125">**Az Azure Key Vault** -használt toosafeguard titkosítási kulcsok és titkos hello lemez titkosítási/visszafejtési folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="6254e-125">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span> 
  * <span data-ttu-id="6254e-126">Ha van ilyen, használhat egy meglévő Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="6254e-126">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="6254e-127">Nincs toodedicate a Key Vault tooencrypting lemezek.</span><span class="sxs-lookup"><span data-stu-id="6254e-127">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="6254e-128">tooseparate felügyeleti határokat és kulcs látható, a dedikált kulcstároló is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="6254e-128">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="6254e-129">**Az Azure Active Directory** - leírók hello szükséges titkosítási kulcsok biztonságos cseréjét, és hitelesítési a kért műveleteket.</span><span class="sxs-lookup"><span data-stu-id="6254e-129">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span> 
  * <span data-ttu-id="6254e-130">Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.</span><span class="sxs-lookup"><span data-stu-id="6254e-130">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="6254e-131">egyszerű hello szolgáltatás egy biztonságos mechanizmus toorequest és hello megfelelő titkosítási kulcsok adja ki.</span><span class="sxs-lookup"><span data-stu-id="6254e-131">hello service principal provides a secure mechanism toorequest and be issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="6254e-132">Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="6254e-132">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="6254e-133">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="6254e-133">Requirements and limitations</span></span>
<span data-ttu-id="6254e-134">Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="6254e-134">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="6254e-135">Az Azure piactéren elérhető rendszerkép vagy egyéni Virtuálismerevlemez-kép új Windows virtuális gépeken futó titkosítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6254e-135">Enabling encryption on new Windows VMs from Azure Marketplace images or custom VHD image.</span></span>
* <span data-ttu-id="6254e-136">A meglévő Windows virtuális gépeken, az Azure-titkosítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6254e-136">Enabling encryption on existing Windows VMs in Azure.</span></span>
* <span data-ttu-id="6254e-137">A Windows virtuális gépeken, a tárolóhelyek szolgáltatással konfigurált titkosítás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="6254e-137">Enabling encryption on Windows VMs that are configured using Storage Spaces.</span></span>
* <span data-ttu-id="6254e-138">Az operációs rendszer és az adatok titkosításának letiltása meghajtók Windows virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="6254e-138">Disabling encryption on OS and data drives for Windows VMs.</span></span>
* <span data-ttu-id="6254e-139">Minden erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie a hello azonos Azure-régió, és az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="6254e-139">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="6254e-140">Standard szint virtuális gépeket, például egy, virtuális gépek D, DS, G és GS adatsorozat.</span><span class="sxs-lookup"><span data-stu-id="6254e-140">Standard tier VMs, such as A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="6254e-141">Lemeztitkosítás jelenleg nem támogatott a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="6254e-141">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="6254e-142">Az alapszintű csomag virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="6254e-142">Basic tier VMs.</span></span>
* <span data-ttu-id="6254e-143">A virtuális gépek hello klasszikus telepítési modellel készült.</span><span class="sxs-lookup"><span data-stu-id="6254e-143">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="6254e-144">Egy már titkosított virtuális gép hello titkosítási kulcsok frissítése.</span><span class="sxs-lookup"><span data-stu-id="6254e-144">Updating hello cryptographic keys on an already encrypted VM.</span></span>
* <span data-ttu-id="6254e-145">Integráció a helyszíni kulcskezelő szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6254e-145">Integration with on-prem Key Management Service.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="6254e-146">Az Azure Key Vault és kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6254e-146">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="6254e-147">Mielőtt elkezdené, győződjön meg arról, hogy hello legújabb verziójának hello Azure PowerShell modul telepítve van listájában.</span><span class="sxs-lookup"><span data-stu-id="6254e-147">Before you start, make sure that hello latest version of hello Azure PowerShell module has been installed.</span></span> <span data-ttu-id="6254e-148">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6254e-148">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span> <span data-ttu-id="6254e-149">Teljes hello parancspéldákban cserélje le minden példa paraméter a saját nevét, helyét és kulcsértékek.</span><span class="sxs-lookup"><span data-stu-id="6254e-149">Throughout hello command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="6254e-150">hello alábbi példák szabályt használ a *myResourceGroup*, *myKeyVault*, *myVM*stb.</span><span class="sxs-lookup"><span data-stu-id="6254e-150">hello following examples use a convention of *myResourceGroup*, *myKeyVault*, *myVM*, etc.</span></span>

<span data-ttu-id="6254e-151">első lépés hello van egy Azure Key Vault toostore toocreate a titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="6254e-151">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="6254e-152">Az Azure Key Vault tárolhatja a kulcsokat, a titkos kulcsok, vagy a jelszavak, amelyek lehetővé teszik toosecurely végrehajtja az alkalmazások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="6254e-152">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="6254e-153">A virtuális lemez titkosításához hozzon létre egy Key Vault toostore használt tooencrypt a titkosítási kulcs, vagy a virtuális lemezek visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="6254e-153">For virtual disk encryption, you create a Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span> 

<span data-ttu-id="6254e-154">Engedélyezi az Azure-előfizetése belül hello Azure Key Vault szolgáltatót [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), majd hozzon létre egy erőforráscsoportot a [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="6254e-154">Enable hello Azure Key Vault provider within your Azure subscription with [Register-AzureRmResourceProvider](/powershell/module/azurerm.resources/register-azurermresourceprovider), then create a resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span></span> <span data-ttu-id="6254e-155">hello alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a hello *USA keleti régiója* helye:</span><span class="sxs-lookup"><span data-stu-id="6254e-155">hello following example creates a resource group name *myResourceGroup* in hello *East US* location:</span></span>

```powershell
$rgName = "myResourceGroup"
$location = "East US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

<span data-ttu-id="6254e-156">hello Azure Key Vault tartalmazó hello kriptográfiai kulcsokat és társított számítási erőforrások, például a tárolási és hello virtuális gépért kell lennie, hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="6254e-156">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="6254e-157">Hozzon létre egy Azure Key Vault a [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) , és engedélyezze a Key Vault hello lemeztitkosítás való használatra.</span><span class="sxs-lookup"><span data-stu-id="6254e-157">Create an Azure Key Vault with [New-AzureRmKeyVault](/powershell/module/azurerm.keyvault/new-azurermkeyvault) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="6254e-158">Adjon meg egy egyedi kulcstároló nevet *keyVaultName* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6254e-158">Specify a unique Key Vault name for *keyVaultName* as follows:</span></span>

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location `
    -ResourceGroupName $rgName `
    -VaultName $keyVaultName `
    -EnabledForDiskEncryption
```

<span data-ttu-id="6254e-159">A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="6254e-159">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="6254e-160">A Key Vault prémium HSM használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="6254e-160">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="6254e-161">Van egy további költség nélkül toocreating szabványos Key Vault szoftveresen védett tároló helyett a Key Vault támogatás.</span><span class="sxs-lookup"><span data-stu-id="6254e-161">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="6254e-162">a prémium kulcstároló, megelőző lépés hello a toocreate hozzáadása hello *- Sku "Prémium"* paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6254e-162">toocreate a premium Key Vault, in hello preceding step add hello *-Sku "Premium"* parameters.</span></span> <span data-ttu-id="6254e-163">hello alábbi példa szoftveresen védett azt a szabványos kulcstároló létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="6254e-163">hello following example uses software-protected keys since we created a standard Key Vault.</span></span> 

<span data-ttu-id="6254e-164">Mindkét védelmi modellek hello Azure platformon kell toobe kap hozzáférést toorequest hello titkosítási kulcsok toodecrypt hello virtuális lemezek hello virtuális gép indításakor.</span><span class="sxs-lookup"><span data-stu-id="6254e-164">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="6254e-165">A kulcstároló, a titkosítási kulcs létrehozására [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span><span class="sxs-lookup"><span data-stu-id="6254e-165">Create a cryptographic key in your Key Vault with [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey).</span></span> <span data-ttu-id="6254e-166">hello alábbi példa létrehoz egy nevű kulcsot *SajátKulcs*:</span><span class="sxs-lookup"><span data-stu-id="6254e-166">hello following example creates a key named *myKey*:</span></span>

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName `
    -Name "myKey" `
    -Destination "Software"
```


## <a name="create-hello-azure-active-directory-service-principal"></a><span data-ttu-id="6254e-167">Hello Azure Active Directory szolgáltatás egyszerű létrehozása</span><span class="sxs-lookup"><span data-stu-id="6254e-167">Create hello Azure Active Directory service principal</span></span>
<span data-ttu-id="6254e-168">Amikor a virtuális lemezek vannak titkosított vagy visszafejtett, megadhatja egy toohandle hello hitelesítése és a Key Vault a titkosítási kulcsok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="6254e-168">When virtual disks are encrypted or decrypted, you specify an account toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="6254e-169">Ezt a fiókot, egy Azure Active Directory szolgáltatás egyszerű, lehetővé teszi, hogy hello Azure platformon toorequest hello megfelelő titkosítási kulcsok hello virtuális gép nevében.</span><span class="sxs-lookup"><span data-stu-id="6254e-169">This account, an Azure Active Directory service principal, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="6254e-170">Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.</span><span class="sxs-lookup"><span data-stu-id="6254e-170">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="6254e-171">Egy egyszerű szolgáltatás létrehozása az Azure Active Directoryban [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span><span class="sxs-lookup"><span data-stu-id="6254e-171">Create a service principal in Azure Active Directory with [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal).</span></span> <span data-ttu-id="6254e-172">a biztonságos jelszó toospecify hello kövesse [jelszóházirendek és -korlátozások az Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span><span class="sxs-lookup"><span data-stu-id="6254e-172">toospecify a secure password, follow hello [Password policies and restrictions in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):</span></span>

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName `
    -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" `
    -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

<span data-ttu-id="6254e-173">toosuccessfully vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs hello engedélyeinek beállítása toopermit hello Azure Active Directory szolgáltatás egyszerű tooread hello kulcsok kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6254e-173">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory service principal tooread hello keys.</span></span> <span data-ttu-id="6254e-174">A kulcstároló, az engedélyeket [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span><span class="sxs-lookup"><span data-stu-id="6254e-174">Set permissions on your Key Vault with [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy):</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName `
    -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "WrapKey" `
    -PermissionsToSecrets "Set"
```


## <a name="create-virtual-machine"></a><span data-ttu-id="6254e-175">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6254e-175">Create virtual machine</span></span>
<span data-ttu-id="6254e-176">tootest hello titkosítási folyamat, hozzon létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="6254e-176">tootest hello encryption process, let's create a VM.</span></span> <span data-ttu-id="6254e-177">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* használatával egy *Windows Server 2016 Datacenter* lemezképet:</span><span class="sxs-lookup"><span data-stu-id="6254e-177">hello following example creates a VM named *myVM* using a *Windows Server 2016 Datacenter* image:</span></span>

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


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="6254e-178">Virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="6254e-178">Encrypt virtual machine</span></span>
<span data-ttu-id="6254e-179">tooencrypt hello virtuális lemezeket, akkor egyesítik összes hello az előző összetevők működését:</span><span class="sxs-lookup"><span data-stu-id="6254e-179">tooencrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="6254e-180">Adja meg a hello Azure Active Directory szolgáltatás egyszerű és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="6254e-180">Specify hello Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="6254e-181">Adja meg a hello Key Vault toostore hello metaadatait a titkosított lemezek.</span><span class="sxs-lookup"><span data-stu-id="6254e-181">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="6254e-182">Adja meg a hello titkosítási kulcsok toouse hello tényleges titkosításához és visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="6254e-182">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="6254e-183">Adja meg, hogy tooencrypt hello operációsrendszer-lemez, hello adatlemezek vagy az összes.</span><span class="sxs-lookup"><span data-stu-id="6254e-183">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="6254e-184">Az a virtuális gép titkosítása [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) hello Azure Key Vault kulcs és az Azure Active Directory szolgáltatás egyszerű hitelesítő adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="6254e-184">Encrypt your VM with [Set-AzureRmVMDiskEncryptionExtension](/powershell/module/azurerm.compute/set-azurermvmdiskencryptionextension) using hello Azure Key Vault key and Azure Active Directory service principal credentials.</span></span> <span data-ttu-id="6254e-185">hello alábbi példa minden hello kulcs adatainak beolvasása és titkosítja a hello nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="6254e-185">hello following example retrieves all hello key information then encrypts hello VM named *myVM*:</span></span>

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

<span data-ttu-id="6254e-186">Fogadja el a hello Rákérdezés toocontinue hello VM titkosítással.</span><span class="sxs-lookup"><span data-stu-id="6254e-186">Accept hello prompt toocontinue with hello VM encryption.</span></span> <span data-ttu-id="6254e-187">virtuális gép hello hello folyamat során újraindul.</span><span class="sxs-lookup"><span data-stu-id="6254e-187">hello VM restarts during hello process.</span></span> <span data-ttu-id="6254e-188">Miután hello titkosítási folyamat befejeződött, és hello virtuális gép újraindítása, tekintse át a titkosítási állapot hello [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span><span class="sxs-lookup"><span data-stu-id="6254e-188">Once hello encryption process completes and hello VM has rebooted, review hello encryption status with [Get-AzureRmVmDiskEncryptionStatus](/powershell/module/azurerm.compute/get-azurermvmdiskencryptionstatus):</span></span>

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

<span data-ttu-id="6254e-189">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6254e-189">hello output is similar toohello following example:</span></span>

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a><span data-ttu-id="6254e-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6254e-190">Next steps</span></span>
* <span data-ttu-id="6254e-191">További információ az Azure Key Vault kezeléséhez: [Key Vault beállítása a virtuális gépek](key-vault-setup.md).</span><span class="sxs-lookup"><span data-stu-id="6254e-191">For more information about managing Azure Key Vault, see [Set up Key Vault for virtual machines](key-vault-setup.md).</span></span>
* <span data-ttu-id="6254e-192">További információ a lemez titkosítása például egy titkosított egyéni VM tooupload tooAzure, előkészítése: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="6254e-192">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
