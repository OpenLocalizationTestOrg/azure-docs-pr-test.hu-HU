---
ms.assetid: 
title: "Az Azure Key Vault - soft-törlés használata a PowerShell használatával"
description: "PowerShell kódrészletek soft-törlés eset példái használata"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/21/2017
ms.author: bruceper
ms.openlocfilehash: 8cf0674f7eb139e50da4a3c22a8d8376a86b0dcc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a><span data-ttu-id="f2d7f-103">Key Vault soft-törlés használata a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f2d7f-103">How to use Key Vault soft-delete with PowerShell</span></span>

<span data-ttu-id="f2d7f-104">Az Azure Key Vault helyreállítható törlés funkció lehetővé teszi, hogy a törölt tárolók és a tároló objektumok.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="f2d7f-105">Pontosabban soft-törlés címeket a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-105">Specifically, soft-delete addresses the following scenarios:</span></span>

- <span data-ttu-id="f2d7f-106">Helyreállítható törlésre kerültek a kulcstároló támogatása</span><span class="sxs-lookup"><span data-stu-id="f2d7f-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="f2d7f-107">A kulcstároló objektumok; helyreállítható törlésre támogatása a kulcsok, a titkos kulcsok, és tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="f2d7f-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f2d7f-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f2d7f-108">Prerequisites</span></span>

- <span data-ttu-id="f2d7f-109">Az Azure PowerShell 4.0.0 vagy újabb – Ha nem rendelkezik ilyen már telepítő, Azure PowerShell telepítése, és társítsa azt az Azure-előfizetése, lásd: [telepítése és konfigurálása az Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f2d7f-109">Azure PowerShell 4.0.0 or later - If you don't have this already setup, install Azure PowerShell and associate it with your Azure subscription, see [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span> 

>[!NOTE]
> <span data-ttu-id="f2d7f-110">A Key Vault PowerShell kimeneti formázás elavult verziójára fájlba van **előfordulhat, hogy** kell betölteni a környezet helyett a helyes verzióra.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-110">There is an outdated version of our Key Vault PowerShell output formatting file that **may** be loaded into your environment instead of the correct version.</span></span> <span data-ttu-id="f2d7f-111">Azt vannak előrejelző magában foglalja a kimeneti formázásához szükséges javítása PowerShell frissített verziója, és ekkor frissíti az ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-111">We are anticipating an updated version of PowerShell to contain the needed correction for the output formatting and will update this topic at that time.</span></span> <span data-ttu-id="f2d7f-112">A jelenlegi megoldást kell tapasztal formázási probléma van:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-112">The current workaround, should you encounter this formatting problem, is:</span></span>
> - <span data-ttu-id="f2d7f-113">A következő lekérdezés nem is lát a soft-törlés észlel a jelen témakörben ismertetett tulajdonság engedélyezve: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-113">Use the following query if you notice you're not seeing the soft-delete enabled property described in this topic: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.</span></span>


<span data-ttu-id="f2d7f-114">PowerShell Key Vault adott refernece információkért lásd: [Azure Key Vault PowerShell-referencia](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="f2d7f-114">For Key Vault specific refernece information for PowerShell, see [Azure Key Vault PowerShell reference](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="f2d7f-115">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="f2d7f-115">Required permissions</span></span>

<span data-ttu-id="f2d7f-116">Key Vault műveleteket külön-külön az alábbiak szerint felügyelt keresztül szerepköralapú hozzáférés-vezérlést (RBAC) engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-116">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="f2d7f-117">Művelet</span><span class="sxs-lookup"><span data-stu-id="f2d7f-117">Operation</span></span> | <span data-ttu-id="f2d7f-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="f2d7f-118">Description</span></span> | <span data-ttu-id="f2d7f-119">Felhasználói engedélyt</span><span class="sxs-lookup"><span data-stu-id="f2d7f-119">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="f2d7f-120">Lista</span><span class="sxs-lookup"><span data-stu-id="f2d7f-120">List</span></span>|<span data-ttu-id="f2d7f-121">Listák kulcstároló törlése.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-121">Lists deleted key vaults.</span></span>|<span data-ttu-id="f2d7f-122">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="f2d7f-122">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="f2d7f-123">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="f2d7f-123">Recover</span></span>|<span data-ttu-id="f2d7f-124">Visszaállítja a törölt kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-124">Restores a deleted key vault.</span></span>|<span data-ttu-id="f2d7f-125">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="f2d7f-125">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="f2d7f-126">Véglegesen töröl</span><span class="sxs-lookup"><span data-stu-id="f2d7f-126">Purge</span></span>|<span data-ttu-id="f2d7f-127">Végleg törli a törölt kulcstároló és annak teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-127">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="f2d7f-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="f2d7f-128">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="f2d7f-129">Hozzáférés-vezérlés az engedélyekkel kapcsolatos további információkért lásd: [a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="f2d7f-129">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="f2d7f-130">Soft-Törlés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f2d7f-130">Enabling soft-delete</span></span>

<span data-ttu-id="f2d7f-131">Helyre szeretné állítani a törölt kulcstároló vagy a key vaultban tárolt objektumok, először engedélyeznie kell, hogy kulcstároló soft-törlésének.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-131">To be able to recover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="f2d7f-132">Meglévő kulcstároló</span><span class="sxs-lookup"><span data-stu-id="f2d7f-132">Existing key vault</span></span>

<span data-ttu-id="f2d7f-133">Egy meglévő kulcstároló nevű ContosoVault engedélyezze a soft-Törlés az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-133">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="f2d7f-134">Jelenleg kell használnia az Azure Resource Manager erőforrás-kezelést közvetlenül írni a *enableSoftDelete* a Key Vault erőforrás-tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-134">Currently you need to use Azure Resource Manager resource manipulation to directly write the *enableSoftDelete* property to the Key Vault resource.</span></span>

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a><span data-ttu-id="f2d7f-135">Új kulcstartó</span><span class="sxs-lookup"><span data-stu-id="f2d7f-135">New key vault</span></span>

<span data-ttu-id="f2d7f-136">Egy új kulcstartó soft-törlésének engedélyezése a létrehozás időpontjában hozzáadásával történik a soft-Törlés engedélyezése jelzőt, amely a create parancs.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-136">Enabling soft-delete for a new key vault is done at creation time by adding the soft-delete enable flag to your create command.</span></span>

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="f2d7f-137">Ellenőrizze a soft-Törlés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f2d7f-137">Verify soft-delete enablement</span></span>

<span data-ttu-id="f2d7f-138">Győződjön meg arról, hogy kulcstároló soft-törlés engedélyezve van-e, futtassa a *beolvasása* parancsot, és keresse meg a "Soft törlése engedélyezve?"</span><span class="sxs-lookup"><span data-stu-id="f2d7f-138">To verify that a key vault has soft-delete enabled, run the *get* command and look for the 'Soft Delete Enabled?'</span></span> <span data-ttu-id="f2d7f-139">attribútum és a beállítása true vagy false.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-139">attribute and its setting, true or false.</span></span>

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="f2d7f-140">Védi a soft-törlés kulcstároló törlése</span><span class="sxs-lookup"><span data-stu-id="f2d7f-140">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="f2d7f-141">A parancs kulcstároló törlése (vagy eltávolításához) változatlan marad, de attól függően, hogy engedélyezte a soft-törlés vagy nem módosítja annak viselkedését.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-141">The command to delete (or remove) a key vault remains the same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
><span data-ttu-id="f2d7f-142">A kulcstároló, amely nem rendelkezik a soft-törlés engedélyezve van az előző parancs futtatása, ha véglegesen törli a kulcstartót és helyreállítási lehetőségeket nélkül minden tartalmát.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-142">If you run the previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="f2d7f-143">Hogyan soft-törlés védje a kulcstárolók</span><span class="sxs-lookup"><span data-stu-id="f2d7f-143">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="f2d7f-144">Soft-Törlés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-144">With soft-delete enabled:</span></span>

- <span data-ttu-id="f2d7f-145">Kulcstároló törlése után van távolítva az erőforráscsoportot, és el egy fenntartott névtér, amely csak a hely, ahol létrehozták társított.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-145">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with the location where it was created.</span></span> 
- <span data-ttu-id="f2d7f-146">Az objektumok a törölt kulcsot használó, például a kulcs, a titkos kulcsok és tanúsítványok, nem érhető el, és maradnak, amíg azok tartalmazó kulcstároló a törölt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-146">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in the deleted state.</span></span> 
- <span data-ttu-id="f2d7f-147">A DNS-nevét kulcstároló törölt állapotban továbbra is fenntartva, ezért nem hozható létre egy új kulcstartó azonos nevű.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-147">The DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="f2d7f-148">Törölt állapotban kulcstárolót, az Ön előfizetéséhez rendelve az alábbi parancs segítségével megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-148">You may view deleted state key vaults, associated with your subscription, using the following command:</span></span>

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

<span data-ttu-id="f2d7f-149">A *erőforrás-azonosító* a kimenetben az eredeti erőforrás-azonosító az ehhez a tárolóhoz hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-149">The *Resource ID* in the output refers to the original resource ID of this vault.</span></span> <span data-ttu-id="f2d7f-150">Ezen a kulcstartón most már törölt állapotban van, mert nincs erőforrás létezik-e az adott erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-150">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="f2d7f-151">A *azonosító* mező helyreállítása, vagy végleges törlése az erőforrás azonosítására használható.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-151">The *Id* field can be used to identify the resource when recovering, or purging.</span></span> <span data-ttu-id="f2d7f-152">A *kiürítése dátum ütemezett* mező azt jelzi, ha a tároló véglegesen törli (törölve), ha a törölt tároló történik semmi.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-152">The *Scheduled Purge Date* field indicates when the vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="f2d7f-153">Az alapértelmezett megőrzési időtartamot, kiszámításához használt a *kiürítése dátum ütemezett*, 90 nap.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-153">The default retention period, used to calculate the *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="f2d7f-154">Kulcstároló helyreállítása</span><span class="sxs-lookup"><span data-stu-id="f2d7f-154">Recovering a key vault</span></span>

<span data-ttu-id="f2d7f-155">Kulcstároló helyreállításához, meg kell adnia a kulcstároló neve, erőforráscsoportot és helyet.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-155">To recover a key vault, you need to specify the key vault name, resource group, and location.</span></span> <span data-ttu-id="f2d7f-156">Megjegyzés: a hely és az erőforráscsoport a törölt kulcstároló szükség van ezekre a kulcstartót a helyreállítási folyamatot.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-156">Note the location and the resource group of the deleted key vault as you need these for a key vault recovery process.</span></span>

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

<span data-ttu-id="f2d7f-157">Kulcstároló helyreállításakor eredménye egy új erőforrás a key vault eredeti erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-157">When a key vault is recovered, the result is a new resource with the key vault's original resource ID.</span></span> <span data-ttu-id="f2d7f-158">Az erőforráscsoport, ha már létező a key vault el lett távolítva, azonos nevű új erőforráscsoport előtt a key vault állíthatók helyre kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-158">If the resource group where the key vault existed has been removed, a new resource group with same name must be created before the key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="f2d7f-159">Key Vault objektumok és a soft-törlés</span><span class="sxs-lookup"><span data-stu-id="f2d7f-159">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="f2d7f-160">A kulcs, "ContosoFirstKey", a kulcstároló neve "ContosoVault" a soft-törlés engedélyezve van, itt a hogyan akkor törlődik kulcs.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-160">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="f2d7f-161">A kulcstartót helyreállítható törlésre engedélyezve van, a törölt kulcs azt továbbra is megjelenik, például törlődik, kivéve, ha explicit módon listájához vagy törölt kulcsok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-161">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="f2d7f-162">A műveleteik zömét a törölt állapotban egy kulcs egy törölt kulcs listázása, helyreállítását vagy végleges törlésére, kivéve sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-162">Most operations on a key in the deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="f2d7f-163">Például a kulcstároló törlése listában kulcsok kérelmezéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-163">For example, to request to list deleted keys in a key vault, use the following command:</span></span>

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a><span data-ttu-id="f2d7f-164">Közbenső műveletfázisa</span><span class="sxs-lookup"><span data-stu-id="f2d7f-164">Transition state</span></span> 

<span data-ttu-id="f2d7f-165">Egy kulcsot a kulcstároló törlése a soft-törlés engedélyezve van, az átmenet befejezéséhez néhány másodpercet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-165">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for the transition to complete.</span></span> <span data-ttu-id="f2d7f-166">A közbenső műveletfázisa során tűnhet, hogy a kulcs nem az aktív vagy törölt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-166">During this transition state, it may appear that the key is not in the active state or the deleted state.</span></span> <span data-ttu-id="f2d7f-167">Ez a parancs felsorolja az összes törölt kulcsok "ContosoVault" nevű key vaultban lévő.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-167">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

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

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="f2d7f-168">Kulcstároló objektumok soft-törlés használata</span><span class="sxs-lookup"><span data-stu-id="f2d7f-168">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="f2d7f-169">Csak a például a kulcstárolók, a törölt kulcs titkos kulcs vagy tanúsítvány állapotban marad törölt 90 napig kivéve végezze el a helyreállítást, vagy törli azt.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-169">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up to 90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="f2d7f-170">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="f2d7f-170">Keys</span></span>

<span data-ttu-id="f2d7f-171">A törölt kulcs helyreállítása:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-171">To recover a deleted key:</span></span>

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

<span data-ttu-id="f2d7f-172">Végleg törölni kívánja a kulcs:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-172">To permanently delete a key:</span></span>

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
><span data-ttu-id="f2d7f-173">A kulcs törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-173">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="f2d7f-174">A **helyreállítása** és **kiürítése** műveletek engedélye a saját tartozó a kulcstároló hozzáférési házirendben.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-174">The **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="f2d7f-175">Egy felhasználó vagy a szolgáltatás egyszerű tudják futtatni egy **helyreállítása** vagy **kiürítése** művelet a kulcstároló hozzáférési házirendben a megfelelő engedéllyel az adott objektum (kulcsok vagy titkos kulcsok) kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-175">For a user or service principal to be able to execute a **recover** or **purge** action they must have the respective permission for that object (key or secret) in the key vault access policy.</span></span> <span data-ttu-id="f2d7f-176">Alapértelmezés szerint a **kiürítése** engedély nem kerül a kulcstároló hozzáférési házirend, az "all" helyi használatakor a felhasználó az összes engedélyt.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-176">By default, the **purge** permission is not added to a key vault's access policy when the 'all' shortcut is used to grant all permissions to a user.</span></span> <span data-ttu-id="f2d7f-177">Kifejezetten biztosítania kell **kiürítése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-177">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="f2d7f-178">Például a következő parancsot a biztosít user@contoso.com engedéllyel a kulcsokat számos műveletek végrehajtásához *ContosoVault* beleértve **kiürítése**.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-178">For example, the following command grants user@contoso.com permission to perform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="f2d7f-179">A kulcstároló hozzáférési házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="f2d7f-179">Set a key vault access policy</span></span>

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> <span data-ttu-id="f2d7f-180">Ha egy meglévő kulcstároló, amely még csak soft-törlés engedélyezve van, nem lehet **helyreállítása** és **kiürítése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-180">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="f2d7f-181">Titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="f2d7f-181">Secrets</span></span>

<span data-ttu-id="f2d7f-182">Kulcsok, például kulcstároló titkos kulcsainak üzemelteti a saját parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-182">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="f2d7f-183">A következő, törlése, listázása, helyreállítása és titkos kulcsok kiürítése parancsok vannak.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-183">Following, are the commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="f2d7f-184">SQLPassword nevű titkos kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-184">Delete a secret named SQLPassword:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- <span data-ttu-id="f2d7f-185">Kulcstároló törölt titkos kulcsainak listázása:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-185">List all deleted secrets in a key vault:</span></span> 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- <span data-ttu-id="f2d7f-186">Helyreállítás törölt állapotban titkos kulcs:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-186">Recover a secret in the deleted state:</span></span> 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- <span data-ttu-id="f2d7f-187">Törölt állapotban titkos kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="f2d7f-187">Purge a secret in deleted state:</span></span> 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
><span data-ttu-id="f2d7f-188">Titkos kulcs kiürítése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-188">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="f2d7f-189">Tárolók kiürítési és kulcs</span><span class="sxs-lookup"><span data-stu-id="f2d7f-189">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="f2d7f-190">Kulcstároló-objektumok</span><span class="sxs-lookup"><span data-stu-id="f2d7f-190">Key vault objects</span></span>

<span data-ttu-id="f2d7f-191">A kulcs törlése, titkos kulcs vagy tanúsítvány véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-191">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="f2d7f-192">A kulcstároló, a törölt objektum találhatók azonban változatlan marad, a rendszer a key vaultban lévő összes más objektumra.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-192">The key vault that contained the deleted object will however remain intact as will all other objects in the key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="f2d7f-193">Kulcs-tárolók tárolóként</span><span class="sxs-lookup"><span data-stu-id="f2d7f-193">Key vaults as containers</span></span>
<span data-ttu-id="f2d7f-194">Ha a kulcstároló véglegesen törlődnek, és annak teljes tartalmát, beleértve a kulcsokat, a titkos kulcsok és a tanúsítványok, véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-194">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="f2d7f-195">Kulcstároló törlése, használja a `Remove-AzureRmKeyVault` beállítás parancsot `-InRemovedState` és a törölt kulcstároló helye megadásával a `-Location location` argumentum.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-195">To purge a key vault, use the `Remove-AzureRmKeyVault` command with the option `-InRemovedState` and by specifying the location of the deleted key vault with the `-Location location` argument.</span></span> <span data-ttu-id="f2d7f-196">A paranccsal egy törölt tároló helyét található `Get-AzureRmKeyVault -InRemovedState`.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-196">You can find the location of a deleted vault using the command `Get-AzureRmKeyVault -InRemovedState`.</span></span>

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
><span data-ttu-id="f2d7f-197">Kulcstároló törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-197">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="f2d7f-198">Törli a szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="f2d7f-198">Purge permissions required</span></span>
- <span data-ttu-id="f2d7f-199">Törli a törölt kulcstároló, úgy, hogy véglegesen törli a a tárolóhoz és annak teljes tartalmát, a felhasználónak a végrehajtásához az RBAC-engedély van szüksége egy *Microsoft.KeyVault/locations/deletedVaults/purge/action* műveletet.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-199">To purge a deleted key vault, such that the vault and all its contents are permanently removed, the user needs RBAC permission to perform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="f2d7f-200">A törölt kulcs listázásához, a tárolót a felhasználó számára szükséges végrehajtásához az RBAC-engedély *Microsoft.KeyVault/deletedVaults/read* engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-200">To list the deleted key, the vault a user needs RBAC permission to perform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="f2d7f-201">Alapértelmezés szerint csak egy előfizetés rendszergazdája rendelkezik ezekkel a jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-201">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="f2d7f-202">Ütemezett kiürítése</span><span class="sxs-lookup"><span data-stu-id="f2d7f-202">Scheduled purge</span></span>

<span data-ttu-id="f2d7f-203">A törölt kulcstároló objektumok listázása látható, ha azok schedled Key Vault kell kiüríti.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-203">Listing your deleted key vault objects shows when they are schedled to be purged by Key Vault.</span></span> <span data-ttu-id="f2d7f-204">A *kiürítése dátum ütemezett* mező jelzi az amikor egy kulcstartót objektum véglegesen törölve lesz, ha nem tesz.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-204">The *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="f2d7f-205">Alapértelmezés szerint a törölt kulcstároló objektum megőrzési időtartama 90 nap.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-205">By default, the retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="f2d7f-206">Egy tároló kiürítve objektum által indított annak *kiürítése dátum ütemezett* mezőben, az véglegesen törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-206">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="f2d7f-207">Nincs helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="f2d7f-207">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="f2d7f-208">Egyéb erőforrások</span><span class="sxs-lookup"><span data-stu-id="f2d7f-208">Other resources</span></span>

- <span data-ttu-id="f2d7f-209">Key Vault soft-törlés szolgáltatás áttekintését lásd: [Azure Key Vault soft-törlés – áttekintés](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="f2d7f-209">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="f2d7f-210">Az Azure Key Vault használatának általános áttekintést, lásd: [Ismerkedés az Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="f2d7f-210">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

