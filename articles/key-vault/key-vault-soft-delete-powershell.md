---
ms.assetid: 
title: "aaaAzure kulcsot tároló - hogyan toouse soft-törlése a PowerShell használatával"
description: "PowerShell kódrészletek soft-törlés eset példái használata"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 4968b700a14f764ea1be7de2bf3697664f255f95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-powershell"></a><span data-ttu-id="e6299-103">Hogyan toouse Key Vault soft-törlése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="e6299-103">How toouse Key Vault soft-delete with PowerShell</span></span>

<span data-ttu-id="e6299-104">Az Azure Key Vault helyreállítható törlés funkció lehetővé teszi, hogy a törölt tárolók és a tároló objektumok.</span><span class="sxs-lookup"><span data-stu-id="e6299-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="e6299-105">Pontosabban soft-törlés címek hello a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="e6299-105">Specifically, soft-delete addresses hello following scenarios:</span></span>

- <span data-ttu-id="e6299-106">Helyreállítható törlésre kerültek a kulcstároló támogatása</span><span class="sxs-lookup"><span data-stu-id="e6299-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="e6299-107">A kulcstároló objektumok; helyreállítható törlésre támogatása a kulcsok, a titkos kulcsok, és tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="e6299-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6299-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e6299-108">Prerequisites</span></span>

- <span data-ttu-id="e6299-109">Az Azure PowerShell 4.0.0 vagy újabb – Ha nem rendelkezik ilyen már telepítő, Azure PowerShell telepítése, és társítsa azt az Azure-előfizetése, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e6299-109">Azure PowerShell 4.0.0 or later - If you don't have this already setup, install Azure PowerShell and associate it with your Azure subscription, see [How tooinstall and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span> 

>[!NOTE]
> <span data-ttu-id="e6299-110">A Key Vault PowerShell kimeneti formázás elavult verziójára fájlba van **előfordulhat, hogy** tölthető helyett hello megfelelő verzióját a környezetbe.</span><span class="sxs-lookup"><span data-stu-id="e6299-110">There is an outdated version of our Key Vault PowerShell output formatting file that **may** be loaded into your environment instead of hello correct version.</span></span> <span data-ttu-id="e6299-111">Azt vannak előrejelző PowerShell toocontain hello frissített verziója szükséges javítás formázás hello kimeneti, és ekkor frissíti az ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="e6299-111">We are anticipating an updated version of PowerShell toocontain hello needed correction for hello output formatting and will update this topic at that time.</span></span> <span data-ttu-id="e6299-112">hello aktuális megoldás kell tapasztal formázási probléma van:</span><span class="sxs-lookup"><span data-stu-id="e6299-112">hello current workaround, should you encounter this formatting problem, is:</span></span>
> - <span data-ttu-id="e6299-113">Nem is lát hello soft-törlés észlel a következő lekérdezés használata hello engedélyezve van a jelen témakörben ismertetett tulajdonság: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span><span class="sxs-lookup"><span data-stu-id="e6299-113">Use hello following query if you notice you're not seeing hello soft-delete enabled property described in this topic: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span></span>


<span data-ttu-id="e6299-114">PowerShell Key Vault adott refernece információkért lásd: [Azure Key Vault PowerShell-referencia](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="e6299-114">For Key Vault specific refernece information for PowerShell, see [Azure Key Vault PowerShell reference](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="e6299-115">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="e6299-115">Required permissions</span></span>

<span data-ttu-id="e6299-116">Key Vault műveleteket külön-külön az alábbiak szerint felügyelt keresztül szerepköralapú hozzáférés-vezérlést (RBAC) engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="e6299-116">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="e6299-117">Művelet</span><span class="sxs-lookup"><span data-stu-id="e6299-117">Operation</span></span> | <span data-ttu-id="e6299-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="e6299-118">Description</span></span> | <span data-ttu-id="e6299-119">Felhasználói engedélyt</span><span class="sxs-lookup"><span data-stu-id="e6299-119">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="e6299-120">Lista</span><span class="sxs-lookup"><span data-stu-id="e6299-120">List</span></span>|<span data-ttu-id="e6299-121">Listák kulcstároló törlése.</span><span class="sxs-lookup"><span data-stu-id="e6299-121">Lists deleted key vaults.</span></span>|<span data-ttu-id="e6299-122">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="e6299-122">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="e6299-123">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="e6299-123">Recover</span></span>|<span data-ttu-id="e6299-124">Visszaállítja a törölt kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="e6299-124">Restores a deleted key vault.</span></span>|<span data-ttu-id="e6299-125">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="e6299-125">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="e6299-126">Véglegesen töröl</span><span class="sxs-lookup"><span data-stu-id="e6299-126">Purge</span></span>|<span data-ttu-id="e6299-127">Végleg törli a törölt kulcstároló és annak teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e6299-127">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="e6299-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="e6299-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="e6299-129">Hozzáférés-vezérlés az engedélyekkel kapcsolatos további információkért lásd: [a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="e6299-129">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="e6299-130">Soft-Törlés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e6299-130">Enabling soft-delete</span></span>

<span data-ttu-id="e6299-131">toobe képes toorecover törölt kulcstároló vagy egy kulcsot a tárolt objektumok tároló, először engedélyeznie kell, hogy kulcstároló soft-törlésének.</span><span class="sxs-lookup"><span data-stu-id="e6299-131">toobe able toorecover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="e6299-132">Meglévő kulcstároló</span><span class="sxs-lookup"><span data-stu-id="e6299-132">Existing key vault</span></span>

<span data-ttu-id="e6299-133">Egy meglévő kulcstároló nevű ContosoVault engedélyezze a soft-Törlés az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="e6299-133">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="e6299-134">Jelenleg szüksége toouse Azure Resource Manager erőforrás adatkezelési toodirectly írási hello *enableSoftDelete* tulajdonság toohello Key Vault erőforrás.</span><span class="sxs-lookup"><span data-stu-id="e6299-134">Currently you need toouse Azure Resource Manager resource manipulation toodirectly write hello *enableSoftDelete* property toohello Key Vault resource.</span></span>

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a><span data-ttu-id="e6299-135">Új kulcstartó</span><span class="sxs-lookup"><span data-stu-id="e6299-135">New key vault</span></span>

<span data-ttu-id="e6299-136">Történik, egy új kulcstartó soft-törlésének engedélyezése a létrehozás időpontjában hello soft-Törlés engedélyezése jelzőt hozzáadásával tooyour parancs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e6299-136">Enabling soft-delete for a new key vault is done at creation time by adding hello soft-delete enable flag tooyour create command.</span></span>

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="e6299-137">Ellenőrizze a soft-Törlés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e6299-137">Verify soft-delete enablement</span></span>

<span data-ttu-id="e6299-138">tooverify, kulcstároló soft-törlés engedélyezve van, amelynek futtatása hello *beolvasása* parancsot, és keressen a "Soft törlése engedélyezve?" hello</span><span class="sxs-lookup"><span data-stu-id="e6299-138">tooverify that a key vault has soft-delete enabled, run hello *get* command and look for hello 'Soft Delete Enabled?'</span></span> <span data-ttu-id="e6299-139">attribútum és a beállítása true vagy false.</span><span class="sxs-lookup"><span data-stu-id="e6299-139">attribute and its setting, true or false.</span></span>

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="e6299-140">Védi a soft-törlés kulcstároló törlése</span><span class="sxs-lookup"><span data-stu-id="e6299-140">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="e6299-141">hello parancs toodelete (vagy eltávolítása) kulcstároló marad hello azonos, de a viselkedésváltozások attól függően, hogy engedélyezte a soft-törlés vagy sem.</span><span class="sxs-lookup"><span data-stu-id="e6299-141">hello command toodelete (or remove) a key vault remains hello same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
><span data-ttu-id="e6299-142">Hello a kulcstároló, amely nem rendelkezik a soft-törlés engedélyezve van az előző parancs futtatása, azzal végleg törli a kulcstartót és helyreállítási lehetőségeket nélkül minden tartalmát.</span><span class="sxs-lookup"><span data-stu-id="e6299-142">If you run hello previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="e6299-143">Hogyan soft-törlés védje a kulcstárolók</span><span class="sxs-lookup"><span data-stu-id="e6299-143">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="e6299-144">Soft-Törlés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="e6299-144">With soft-delete enabled:</span></span>

- <span data-ttu-id="e6299-145">Kulcstároló törlése után van távolítva az erőforráscsoportot, és el egy fenntartott névtér, amely csak a társított hello helyre, ahol létrehozták.</span><span class="sxs-lookup"><span data-stu-id="e6299-145">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with hello location where it was created.</span></span> 
- <span data-ttu-id="e6299-146">Az objektumok a törölt kulcsot használó, például a kulcs, a titkos kulcsok és tanúsítványok, nem érhető el, és maradnak, amíg azok tartalmazó kulcstároló hello törölt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="e6299-146">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in hello deleted state.</span></span> 
- <span data-ttu-id="e6299-147">hello DNS-nevét kulcstároló törölt állapotban még fenntartva, ezért nem hozható létre egy új kulcstartó azonos nevű.</span><span class="sxs-lookup"><span data-stu-id="e6299-147">hello DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="e6299-148">Törölt állapotban kulcstárolót, az Ön előfizetéséhez rendelve hello a következő parancs használatával megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="e6299-148">You may view deleted state key vaults, associated with your subscription, using hello following command:</span></span>

```powershell
PS C:\> Get-AzureRmKeyVault -InRemovedStateVault 

Name           : ContosoVault
Location             : westus
Id                   : /subscriptions/xxx/providers/Microsoft.KeyVault/locations/westus/deletedVaults/ContosoVault
Resource ID          : /subscriptions/xxx/resourceGroups/ContosoVault/providers/Microsoft.KeyVault/vaults/ContosoVault
Deletion Date        : 5/9/2017 12:14:14 AM
Scheduled Purge Date : 8/7/2017 12:14:14 AM
Tags                 :
```

<span data-ttu-id="e6299-149">Hello *erőforrás-azonosító* hello kimeneti jelenti toohello eredeti erőforrás-azonosító az ehhez a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="e6299-149">hello *Resource ID* in hello output refers toohello original resource ID of this vault.</span></span> <span data-ttu-id="e6299-150">Ezen a kulcstartón most már törölt állapotban van, mert nincs erőforrás létezik-e az adott erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="e6299-150">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="e6299-151">Hello *azonosító* mező lehet használt tooidentify hello erőforrás helyreállítása, vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="e6299-151">hello *Id* field can be used tooidentify hello resource when recovering, or purging.</span></span> <span data-ttu-id="e6299-152">Hello *kiürítése dátum ütemezett* mező jelzi, hogy mikor hello tároló véglegesen törli (törölve), ha a törölt tároló történik semmi.</span><span class="sxs-lookup"><span data-stu-id="e6299-152">hello *Scheduled Purge Date* field indicates when hello vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="e6299-153">hello alapértelmezett megőrzési időszak, felhasznált toocalculate hello *kiürítése dátum ütemezett*, 90 nap.</span><span class="sxs-lookup"><span data-stu-id="e6299-153">hello default retention period, used toocalculate hello *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="e6299-154">Kulcstároló helyreállítása</span><span class="sxs-lookup"><span data-stu-id="e6299-154">Recovering a key vault</span></span>

<span data-ttu-id="e6299-155">Kulcstároló toorecover, kell toospecify hello kulcstároló neve, erőforráscsoportot és helyet.</span><span class="sxs-lookup"><span data-stu-id="e6299-155">toorecover a key vault, you need toospecify hello key vault name, resource group, and location.</span></span> <span data-ttu-id="e6299-156">Megjegyzés: hello helyét és a törölt hello kulcstároló hello erőforráscsoport szükség van ezekre a kulcstartót a helyreállítási folyamatot.</span><span class="sxs-lookup"><span data-stu-id="e6299-156">Note hello location and hello resource group of hello deleted key vault as you need these for a key vault recovery process.</span></span>

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

<span data-ttu-id="e6299-157">Kulcstároló helyreállításakor hello eredménye egy új erőforrás hello kulcstároló eredeti erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="e6299-157">When a key vault is recovered, hello result is a new resource with hello key vault's original resource ID.</span></span> <span data-ttu-id="e6299-158">Amennyiben már létezett a hello kulcstároló hello erőforráscsoport el lett távolítva, azonos nevű új erőforráscsoport előtt hello kulcstároló állíthatók helyre kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e6299-158">If hello resource group where hello key vault existed has been removed, a new resource group with same name must be created before hello key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="e6299-159">Key Vault objektumok és a soft-törlés</span><span class="sxs-lookup"><span data-stu-id="e6299-159">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="e6299-160">A kulcs, "ContosoFirstKey", a kulcstároló neve "ContosoVault" a soft-törlés engedélyezve van, itt a hogyan akkor törlődik kulcs.</span><span class="sxs-lookup"><span data-stu-id="e6299-160">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="e6299-161">A kulcstartót helyreállítható törlésre engedélyezve van, a törölt kulcs azt továbbra is megjelenik, például törlődik, kivéve, ha explicit módon listájához vagy törölt kulcsok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="e6299-161">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="e6299-162">A műveleteik zömét a kulcs törlése hello állapotban törölt kulcs listázása, helyreállítását vagy végleges törlésére, kivéve sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="e6299-162">Most operations on a key in hello deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="e6299-163">Például toorequest törölt toolist kulcs a kulcstároló, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="e6299-163">For example, toorequest toolist deleted keys in a key vault, use hello following command:</span></span>

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a><span data-ttu-id="e6299-164">Közbenső műveletfázisa</span><span class="sxs-lookup"><span data-stu-id="e6299-164">Transition state</span></span> 

<span data-ttu-id="e6299-165">Egy kulcsot a kulcstároló törlése a soft-törlés engedélyezve van, a hello áttűnés toocomplete néhány másodpercet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="e6299-165">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for hello transition toocomplete.</span></span> <span data-ttu-id="e6299-166">A műveletfázisa során előfordulhat, hogy megjelenik hello kulcs nem hello aktív vagy hello törölt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="e6299-166">During this transition state, it may appear that hello key is not in hello active state or hello deleted state.</span></span> <span data-ttu-id="e6299-167">Ez a parancs felsorolja az összes törölt kulcsok "ContosoVault" nevű key vaultban lévő.</span><span class="sxs-lookup"><span data-stu-id="e6299-167">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```powershell
  Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
  Vault Name           : ContosoVault
  Name                 : ContosoFirstKey
  Id                   : https://ContosoVault.vault.azure.net:443/keys/ContosoFirstKey
  Deleted Date         : 2/14/2017 8:20:52 PM
  Scheduled Purge Date : 5/15/2017 8:20:52 PM
  Enabled              : True
  Expires              :
  Not Before           :
  Created              : 2/14/2017 8:16:07 PM
  Updated              : 2/14/2017 8:16:07 PM
  Tags                 :
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="e6299-168">Kulcstároló objektumok soft-törlés használata</span><span class="sxs-lookup"><span data-stu-id="e6299-168">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="e6299-169">Például a kulcstárolók, a törölt kulcs csak titkos kulcs vagy tanúsítvány állapotban marad törölt a too90 nap mentése kivéve végezze el a helyreállítást, vagy törli azt.</span><span class="sxs-lookup"><span data-stu-id="e6299-169">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up too90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="e6299-170">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="e6299-170">Keys</span></span>

<span data-ttu-id="e6299-171">a törölt kulcs toorecover:</span><span class="sxs-lookup"><span data-stu-id="e6299-171">toorecover a deleted key:</span></span>

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="e6299-172">toopermanently a kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="e6299-172">toopermanently delete a key:</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
><span data-ttu-id="e6299-173">A kulcs törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="e6299-173">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="e6299-174">Hello **helyreállítása** és **kiürítése** műveletek engedélye a saját tartozó a kulcstároló hozzáférési házirendben.</span><span class="sxs-lookup"><span data-stu-id="e6299-174">hello **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="e6299-175">A felhasználó vagy szolgáltatás egyszerű toobe képes tooexecute egy **helyreállítása** vagy **kiürítése** művelet hello kulcstároló hozzáférési házirendben hello megfelelő engedéllyel az adott objektum (kulcsok vagy titkos kulcsok) kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="e6299-175">For a user or service principal toobe able tooexecute a **recover** or **purge** action they must have hello respective permission for that object (key or secret) in hello key vault access policy.</span></span> <span data-ttu-id="e6299-176">Alapértelmezés szerint hello **kiürítése** engedély nem kerül tooa kulcstároló hozzáférési házirend esetén hello "all" helyi használt toogrant engedélyek tooa összes felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e6299-176">By default, hello **purge** permission is not added tooa key vault's access policy when hello 'all' shortcut is used toogrant all permissions tooa user.</span></span> <span data-ttu-id="e6299-177">Kifejezetten biztosítania kell **kiürítése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="e6299-177">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="e6299-178">Például a következő parancs biztosít hello user@contoso.com engedély tooperform a kulcsokat számos művelet *ContosoVault* beleértve **kiürítése**.</span><span class="sxs-lookup"><span data-stu-id="e6299-178">For example, hello following command grants user@contoso.com permission tooperform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="e6299-179">A kulcstároló hozzáférési házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="e6299-179">Set a key vault access policy</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> <span data-ttu-id="e6299-180">Ha egy meglévő kulcstároló, amely még csak soft-törlés engedélyezve van, nem lehet **helyreállítása** és **kiürítése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="e6299-180">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="e6299-181">Titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="e6299-181">Secrets</span></span>

<span data-ttu-id="e6299-182">Kulcsok, például kulcstároló titkos kulcsainak üzemelteti a saját parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="e6299-182">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="e6299-183">A következő, már törlése, listázása, helyreállítása és titkos kulcsok kiürítése hello a parancsokat jelölik.</span><span class="sxs-lookup"><span data-stu-id="e6299-183">Following, are hello commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="e6299-184">SQLPassword nevű titkos kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="e6299-184">Delete a secret named SQLPassword:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- <span data-ttu-id="e6299-185">Kulcstároló törölt titkos kulcsainak listázása:</span><span class="sxs-lookup"><span data-stu-id="e6299-185">List all deleted secrets in a key vault:</span></span> 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- <span data-ttu-id="e6299-186">Helyreállítás törölt hello állapotban titkos kulcs:</span><span class="sxs-lookup"><span data-stu-id="e6299-186">Recover a secret in hello deleted state:</span></span> 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- <span data-ttu-id="e6299-187">Törölt állapotban titkos kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="e6299-187">Purge a secret in deleted state:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
><span data-ttu-id="e6299-188">Titkos kulcs kiürítése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="e6299-188">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="e6299-189">Tárolók kiürítési és kulcs</span><span class="sxs-lookup"><span data-stu-id="e6299-189">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="e6299-190">Kulcstároló-objektumok</span><span class="sxs-lookup"><span data-stu-id="e6299-190">Key vault objects</span></span>

<span data-ttu-id="e6299-191">A kulcs törlése, titkos kulcs vagy tanúsítvány véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="e6299-191">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="e6299-192">hello kulcstároló törlése hello objektumot tartalmazott azonban változatlan marad, a rendszer hello key vaultban lévő összes más objektumra.</span><span class="sxs-lookup"><span data-stu-id="e6299-192">hello key vault that contained hello deleted object will however remain intact as will all other objects in hello key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="e6299-193">Kulcs-tárolók tárolóként</span><span class="sxs-lookup"><span data-stu-id="e6299-193">Key vaults as containers</span></span>
<span data-ttu-id="e6299-194">Ha a kulcstároló véglegesen törlődnek, és annak teljes tartalmát, beleértve a kulcsokat, a titkos kulcsok és a tanúsítványok, véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="e6299-194">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="e6299-195">toopurge kulcstároló, használja a hello `Remove-AzureRmKeyVault` hello beállítás parancsot `-InRemovedState` és hello törölt hello kulcstároló helye hello megadásával `-Location location` argumentum.</span><span class="sxs-lookup"><span data-stu-id="e6299-195">toopurge a key vault, use hello `Remove-AzureRmKeyVault` command with hello option `-InRemovedState` and by specifying hello location of hello deleted key vault with hello `-Location location` argument.</span></span> <span data-ttu-id="e6299-196">Hello helyét egy törölt hello paranccsal tárolóban található `Get-AzureRmKeyVault -InRemovedState`.</span><span class="sxs-lookup"><span data-stu-id="e6299-196">You can find hello location of a deleted vault using hello command `Get-AzureRmKeyVault -InRemovedState`.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
><span data-ttu-id="e6299-197">Kulcstároló törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="e6299-197">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="e6299-198">Törli a szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="e6299-198">Purge permissions required</span></span>
- <span data-ttu-id="e6299-199">törölt kulcstároló toopurge, úgy, hogy véglegesen eltávolítja hello tárolóhoz és annak teljes tartalmát, a hello felhasználói szükségessége, az RBAC-engedély tooperform egy *Microsoft.KeyVault/locations/deletedVaults/purge/action* műveletet.</span><span class="sxs-lookup"><span data-stu-id="e6299-199">toopurge a deleted key vault, such that hello vault and all its contents are permanently removed, hello user needs RBAC permission tooperform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="e6299-200">toolist törölt hello kulcsot, hello tároló felhasználó kell RBAC-engedély tooperform *Microsoft.KeyVault/deletedVaults/read* engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="e6299-200">toolist hello deleted key, hello vault a user needs RBAC permission tooperform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="e6299-201">Alapértelmezés szerint csak egy előfizetés rendszergazdája rendelkezik ezekkel a jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="e6299-201">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="e6299-202">Ütemezett kiürítése</span><span class="sxs-lookup"><span data-stu-id="e6299-202">Scheduled purge</span></span>

<span data-ttu-id="e6299-203">Listázása a kulcstartót törölt objektumok mutat be, amikor azokat kiüríti Key Vault schedled toobe.</span><span class="sxs-lookup"><span data-stu-id="e6299-203">Listing your deleted key vault objects shows when they are schedled toobe purged by Key Vault.</span></span> <span data-ttu-id="e6299-204">Hello *kiürítése dátum ütemezett* mező jelzi az amikor egy kulcstartót objektum véglegesen törölve lesz, ha nem tesz.</span><span class="sxs-lookup"><span data-stu-id="e6299-204">hello *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="e6299-205">Alapértelmezés szerint a törölt kulcstároló objektum hello megőrzési idő 90 nap.</span><span class="sxs-lookup"><span data-stu-id="e6299-205">By default, hello retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="e6299-206">Egy tároló kiürítve objektum által indított annak *kiürítése dátum ütemezett* mezőben, az véglegesen törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="e6299-206">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="e6299-207">Nincs helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="e6299-207">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="e6299-208">Egyéb erőforrások</span><span class="sxs-lookup"><span data-stu-id="e6299-208">Other resources</span></span>

- <span data-ttu-id="e6299-209">Key Vault soft-törlés szolgáltatás áttekintését lásd: [Azure Key Vault soft-törlés – áttekintés](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="e6299-209">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="e6299-210">Az Azure Key Vault használatának általános áttekintést, lásd: [Ismerkedés az Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e6299-210">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

