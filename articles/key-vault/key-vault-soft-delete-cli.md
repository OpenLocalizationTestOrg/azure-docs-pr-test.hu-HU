---
ms.assetid: 
title: "aaaAzure kulcsot tároló - hogyan toouse helyreállítható törlése a parancssori felület"
description: "CLI kódrészletek soft-törlés eset példái használata"
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: 672f5210ab119c244ca712f0bb80b653b50ea79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-key-vault-soft-delete-with-cli"></a><span data-ttu-id="bbafe-103">Hogyan toouse Key Vault soft-törlése a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="bbafe-103">How toouse Key Vault soft-delete with CLI</span></span>

<span data-ttu-id="bbafe-104">Az Azure Key Vault helyreállítható törlés funkció lehetővé teszi, hogy a törölt tárolók és a tároló objektumok.</span><span class="sxs-lookup"><span data-stu-id="bbafe-104">Azure Key Vault's soft delete feature allows recovery of deleted vaults and vault objects.</span></span> <span data-ttu-id="bbafe-105">Pontosabban soft-törlés címek hello a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="bbafe-105">Specifically, soft-delete addresses hello following scenarios:</span></span>

- <span data-ttu-id="bbafe-106">Helyreállítható törlésre kerültek a kulcstároló támogatása</span><span class="sxs-lookup"><span data-stu-id="bbafe-106">Support for recoverable deletion of a key vault</span></span>
- <span data-ttu-id="bbafe-107">A kulcstároló objektumok; helyreállítható törlésre támogatása a kulcsok, a titkos kulcsok, és tanúsítványokat</span><span class="sxs-lookup"><span data-stu-id="bbafe-107">Support for recoverable deletion of key vault objects; keys, secrets, and, certificates</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bbafe-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bbafe-108">Prerequisites</span></span>

- <span data-ttu-id="bbafe-109">Lásd az Azure CLI 2.0 – Ha a telepítő nem rendelkezik a környezetnek [kezelése Key Vault CLI 2.0 használatával](key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="bbafe-109">Azure CLI 2.0 - If you don't have this setup for your environment, see [Manage Key Vault using CLI 2.0](key-vault-manage-with-cli2.md).</span></span>

<span data-ttu-id="bbafe-110">Parancssori felület Key Vault referenciával információkért lásd: [Azure CLI 2.0 Key Vault hivatkozás](https://docs.microsoft.com/cli/azure/keyvault).</span><span class="sxs-lookup"><span data-stu-id="bbafe-110">For Key Vault specific reference information for CLI, see [Azure CLI 2.0 Key Vault reference](https://docs.microsoft.com/cli/azure/keyvault).</span></span>

## <a name="required-permissions"></a><span data-ttu-id="bbafe-111">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="bbafe-111">Required permissions</span></span>

<span data-ttu-id="bbafe-112">Key Vault műveleteket külön-külön az alábbiak szerint felügyelt keresztül szerepköralapú hozzáférés-vezérlést (RBAC) engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="bbafe-112">Key Vault operations are separately managed via role-based access control (RBAC) permissions as follows:</span></span>

| <span data-ttu-id="bbafe-113">Művelet</span><span class="sxs-lookup"><span data-stu-id="bbafe-113">Operation</span></span> | <span data-ttu-id="bbafe-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="bbafe-114">Description</span></span> | <span data-ttu-id="bbafe-115">Felhasználói engedélyt</span><span class="sxs-lookup"><span data-stu-id="bbafe-115">User permission</span></span> |
|:--|:--|:--|
|<span data-ttu-id="bbafe-116">Lista</span><span class="sxs-lookup"><span data-stu-id="bbafe-116">List</span></span>|<span data-ttu-id="bbafe-117">Listák kulcstároló törlése.</span><span class="sxs-lookup"><span data-stu-id="bbafe-117">Lists deleted key vaults.</span></span>|<span data-ttu-id="bbafe-118">Microsoft.KeyVault/deletedVaults/read</span><span class="sxs-lookup"><span data-stu-id="bbafe-118">Microsoft.KeyVault/deletedVaults/read</span></span>|
|<span data-ttu-id="bbafe-119">Helyreállítás</span><span class="sxs-lookup"><span data-stu-id="bbafe-119">Recover</span></span>|<span data-ttu-id="bbafe-120">Visszaállítja a törölt kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="bbafe-120">Restores a deleted key vault.</span></span>|<span data-ttu-id="bbafe-121">Microsoft.KeyVault/vaults/write</span><span class="sxs-lookup"><span data-stu-id="bbafe-121">Microsoft.KeyVault/vaults/write</span></span>|
|<span data-ttu-id="bbafe-122">Véglegesen töröl</span><span class="sxs-lookup"><span data-stu-id="bbafe-122">Purge</span></span>|<span data-ttu-id="bbafe-123">Végleg törli a törölt kulcstároló és annak teljes tartalmát.</span><span class="sxs-lookup"><span data-stu-id="bbafe-123">Permanently removes a deleted key vault and all its contents.</span></span>|<span data-ttu-id="bbafe-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span><span class="sxs-lookup"><span data-stu-id="bbafe-124">Microsoft.KeyVault/locations/deletedVaults/purge/action</span></span>|

<span data-ttu-id="bbafe-125">Hozzáférés-vezérlés az engedélyekkel kapcsolatos további információkért lásd: [a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).</span><span class="sxs-lookup"><span data-stu-id="bbafe-125">For more information on permissions and access control, see [Secure your key vault](key-vault-secure-your-key-vault.md).</span></span>

## <a name="enabling-soft-delete"></a><span data-ttu-id="bbafe-126">Soft-Törlés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bbafe-126">Enabling soft-delete</span></span>

<span data-ttu-id="bbafe-127">toobe képes toorecover törölt kulcstároló vagy egy kulcsot a tárolt objektumok tároló, először engedélyeznie kell, hogy kulcstároló soft-törlésének.</span><span class="sxs-lookup"><span data-stu-id="bbafe-127">toobe able toorecover a deleted key vault or objects stored in a key vault, you must first enable soft-delete for that key vault.</span></span>

### <a name="existing-key-vault"></a><span data-ttu-id="bbafe-128">Meglévő kulcstároló</span><span class="sxs-lookup"><span data-stu-id="bbafe-128">Existing key vault</span></span>

<span data-ttu-id="bbafe-129">Egy meglévő kulcstároló nevű ContosoVault engedélyezze a soft-Törlés az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="bbafe-129">For an existing key vault named ContosoVault, enable soft-delete as follows.</span></span> 

>[!NOTE]
><span data-ttu-id="bbafe-130">Jelenleg szüksége toouse Azure Resource Manager erőforrás adatkezelési toodirectly írási hello *enableSoftDelete* tulajdonság toohello Key Vault erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bbafe-130">Currently you need toouse Azure Resource Manager resource manipulation toodirectly write hello *enableSoftDelete* property toohello Key Vault resource.</span></span>

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a><span data-ttu-id="bbafe-131">Új kulcstartó</span><span class="sxs-lookup"><span data-stu-id="bbafe-131">New key vault</span></span>

<span data-ttu-id="bbafe-132">Történik, egy új kulcstartó soft-törlésének engedélyezése a létrehozás időpontjában hello soft-Törlés engedélyezése jelzőt hozzáadásával tooyour parancs létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bbafe-132">Enabling soft-delete for a new key vault is done at creation time by adding hello soft-delete enable flag tooyour create command.</span></span>

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a><span data-ttu-id="bbafe-133">Ellenőrizze a soft-Törlés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bbafe-133">Verify soft-delete enablement</span></span>

<span data-ttu-id="bbafe-134">tooverify, kulcstároló soft-törlés engedélyezve van, amelynek futtatása hello *megjelenítése* parancsot, és keressen a "Soft törlése engedélyezve?" hello</span><span class="sxs-lookup"><span data-stu-id="bbafe-134">tooverify that a key vault has soft-delete enabled, run hello *show* command and look for hello 'Soft Delete Enabled?'</span></span> <span data-ttu-id="bbafe-135">attribútum és a beállítása true vagy false.</span><span class="sxs-lookup"><span data-stu-id="bbafe-135">attribute and its setting, true or false.</span></span>

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a><span data-ttu-id="bbafe-136">Védi a soft-törlés kulcstároló törlése</span><span class="sxs-lookup"><span data-stu-id="bbafe-136">Deleting a key vault protected by soft-delete</span></span>

<span data-ttu-id="bbafe-137">hello parancs toodelete (vagy eltávolítása) kulcstároló marad hello azonos, de a viselkedésváltozások attól függően, hogy engedélyezte a soft-törlés vagy sem.</span><span class="sxs-lookup"><span data-stu-id="bbafe-137">hello command toodelete (or remove) a key vault remains hello same, but its behavior changes depending on whether you have enabled soft-delete or not.</span></span>

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
><span data-ttu-id="bbafe-138">Hello a kulcstároló, amely nem rendelkezik a soft-törlés engedélyezve van az előző parancs futtatása, azzal végleg törli a kulcstartót és helyreállítási lehetőségeket nélkül minden tartalmát.</span><span class="sxs-lookup"><span data-stu-id="bbafe-138">If you run hello previous command for a key vault that does not have soft-delete enabled, you will permanently delete this key vault and all its content without any options for recovery.</span></span>

### <a name="how-soft-delete-protects-your-key-vaults"></a><span data-ttu-id="bbafe-139">Hogyan soft-törlés védje a kulcstárolók</span><span class="sxs-lookup"><span data-stu-id="bbafe-139">How soft-delete protects your key vaults</span></span>

<span data-ttu-id="bbafe-140">Soft-Törlés engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="bbafe-140">With soft-delete enabled:</span></span>

- <span data-ttu-id="bbafe-141">Kulcstároló törlése után van távolítva az erőforráscsoportot, és el egy fenntartott névtér, amely csak a társított hello helyre, ahol létrehozták.</span><span class="sxs-lookup"><span data-stu-id="bbafe-141">When a key vault is deleted, it is removed from its resource group and placed in a reserved namespace that is only associated with hello location where it was created.</span></span> 
- <span data-ttu-id="bbafe-142">Az objektumok a törölt kulcsot használó, például a kulcs, a titkos kulcsok és tanúsítványok, nem érhető el, és maradnak, amíg azok tartalmazó kulcstároló hello törölt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="bbafe-142">Objects in a deleted key vault, such as keys, secrets and, certificates, are inaccessible and remain so while their containing key vault is in hello deleted state.</span></span> 
- <span data-ttu-id="bbafe-143">hello DNS-nevét kulcstároló törölt állapotban még fenntartva, ezért nem hozható létre egy új kulcstartó azonos nevű.</span><span class="sxs-lookup"><span data-stu-id="bbafe-143">hello DNS name for a key vault in a deleted state is still reserved so, a new key vault with same name cannot be created.</span></span>  

<span data-ttu-id="bbafe-144">Törölt állapotban kulcstárolót, az Ön előfizetéséhez rendelve hello a következő parancs használatával megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="bbafe-144">You may view deleted state key vaults, associated with your subscription, using hello following command:</span></span>

```azurecli
az keyvault list-deleted
```

<span data-ttu-id="bbafe-145">Hello *erőforrás-azonosító* hello kimeneti jelenti toohello eredeti erőforrás-azonosító az ehhez a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="bbafe-145">hello *Resource ID* in hello output refers toohello original resource ID of this vault.</span></span> <span data-ttu-id="bbafe-146">Ezen a kulcstartón most már törölt állapotban van, mert nincs erőforrás létezik-e az adott erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="bbafe-146">Since this key vault is now in a deleted state, no resource exists with that resource ID.</span></span> <span data-ttu-id="bbafe-147">Hello *azonosító* mező lehet használt tooidentify hello erőforrás helyreállítása, vagy törlése.</span><span class="sxs-lookup"><span data-stu-id="bbafe-147">hello *Id* field can be used tooidentify hello resource when recovering, or purging.</span></span> <span data-ttu-id="bbafe-148">Hello *kiürítése dátum ütemezett* mező jelzi, hogy mikor hello tároló véglegesen törli (törölve), ha a törölt tároló történik semmi.</span><span class="sxs-lookup"><span data-stu-id="bbafe-148">hello *Scheduled Purge Date* field indicates when hello vault will be permanently deleted (purged) if no action is taken for this deleted vault.</span></span> <span data-ttu-id="bbafe-149">hello alapértelmezett megőrzési időszak, felhasznált toocalculate hello *kiürítése dátum ütemezett*, 90 nap.</span><span class="sxs-lookup"><span data-stu-id="bbafe-149">hello default retention period, used toocalculate hello *Scheduled Purge Date*, is 90 days.</span></span>

## <a name="recovering-a-key-vault"></a><span data-ttu-id="bbafe-150">Kulcstároló helyreállítása</span><span class="sxs-lookup"><span data-stu-id="bbafe-150">Recovering a key vault</span></span>

<span data-ttu-id="bbafe-151">Kulcstároló toorecover, kell toospecify hello kulcstároló neve, erőforráscsoportot és helyet.</span><span class="sxs-lookup"><span data-stu-id="bbafe-151">toorecover a key vault, you need toospecify hello key vault name, resource group, and location.</span></span> <span data-ttu-id="bbafe-152">Megjegyzés: hello helyét és a törölt hello kulcstároló hello erőforráscsoport szükség van ezekre a kulcstartót a helyreállítási folyamatot.</span><span class="sxs-lookup"><span data-stu-id="bbafe-152">Note hello location and hello resource group of hello deleted key vault as you need these for a key vault recovery process.</span></span>

```azurecli
az keyvault recover --location westus --name ContosoVault
```

<span data-ttu-id="bbafe-153">Kulcstároló helyreállításakor hello eredménye egy új erőforrás hello kulcstároló eredeti erőforrás-azonosító.</span><span class="sxs-lookup"><span data-stu-id="bbafe-153">When a key vault is recovered, hello result is a new resource with hello key vault's original resource ID.</span></span> <span data-ttu-id="bbafe-154">Amennyiben már létezett a hello kulcstároló hello erőforráscsoport el lett távolítva, azonos nevű új erőforráscsoport előtt hello kulcstároló állíthatók helyre kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="bbafe-154">If hello resource group where hello key vault existed has been removed, a new resource group with same name must be created before hello key vault can be recovered.</span></span>

## <a name="key-vault-objects-and-soft-delete"></a><span data-ttu-id="bbafe-155">Key Vault objektumok és a soft-törlés</span><span class="sxs-lookup"><span data-stu-id="bbafe-155">Key Vault objects and soft-delete</span></span>

<span data-ttu-id="bbafe-156">A kulcs, "ContosoFirstKey", a kulcstároló neve "ContosoVault" a soft-törlés engedélyezve van, itt a hogyan akkor törlődik kulcs.</span><span class="sxs-lookup"><span data-stu-id="bbafe-156">For a key, 'ContosoFirstKey', in a key vault named 'ContosoVault' with soft-delete enabled, here's how you would delete that key.</span></span>

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="bbafe-157">A kulcstartót helyreállítható törlésre engedélyezve van, a törölt kulcs azt továbbra is megjelenik, például törlődik, kivéve, ha explicit módon listájához vagy törölt kulcsok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bbafe-157">With your key vault enabled for soft-delete, a deleted key still appears like it's deleted except, when you explicitly list or retrieve deleted keys.</span></span> <span data-ttu-id="bbafe-158">A műveleteik zömét a kulcs törlése hello állapotban törölt kulcs listázása, helyreállítását vagy végleges törlésére, kivéve sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="bbafe-158">Most operations on a key in hello deleted state will fail except for listing a deleted key, recovering it or purging it.</span></span> 

<span data-ttu-id="bbafe-159">Például toorequest törölt toolist kulcs a kulcstároló, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bbafe-159">For example, toorequest toolist deleted keys in a key vault, use hello following command:</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a><span data-ttu-id="bbafe-160">Közbenső műveletfázisa</span><span class="sxs-lookup"><span data-stu-id="bbafe-160">Transition state</span></span> 

<span data-ttu-id="bbafe-161">Egy kulcsot a kulcstároló törlése a soft-törlés engedélyezve van, a hello áttűnés toocomplete néhány másodpercet vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="bbafe-161">When you delete a key in a key vault with soft-delete enabled, it may take a few seconds for hello transition toocomplete.</span></span> <span data-ttu-id="bbafe-162">A műveletfázisa során előfordulhat, hogy megjelenik hello kulcs nem hello aktív vagy hello törölt állapotban van.</span><span class="sxs-lookup"><span data-stu-id="bbafe-162">During this transition state, it may appear that hello key is not in hello active state or hello deleted state.</span></span> <span data-ttu-id="bbafe-163">Ez a parancs felsorolja az összes törölt kulcsok "ContosoVault" nevű key vaultban lévő.</span><span class="sxs-lookup"><span data-stu-id="bbafe-163">This command will list all deleted keys in your key vault named 'ContosoVault'.</span></span>

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a><span data-ttu-id="bbafe-164">Kulcstároló objektumok soft-törlés használata</span><span class="sxs-lookup"><span data-stu-id="bbafe-164">Using soft-delete with key vault objects</span></span>

<span data-ttu-id="bbafe-165">Például a kulcstárolók, a törölt kulcs csak titkos kulcs vagy tanúsítvány állapotban marad törölt a too90 nap mentése kivéve végezze el a helyreállítást, vagy törli azt.</span><span class="sxs-lookup"><span data-stu-id="bbafe-165">Just like key vaults, a deleted key, secret or, certificate will remain in deleted state for up too90 days unless you recover it or purge it.</span></span> 

#### <a name="keys"></a><span data-ttu-id="bbafe-166">Kulcsok</span><span class="sxs-lookup"><span data-stu-id="bbafe-166">Keys</span></span>

<span data-ttu-id="bbafe-167">a törölt kulcs toorecover:</span><span class="sxs-lookup"><span data-stu-id="bbafe-167">toorecover a deleted key:</span></span>

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

<span data-ttu-id="bbafe-168">toopermanently a kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="bbafe-168">toopermanently delete a key:</span></span>

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="bbafe-169">A kulcs törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="bbafe-169">Purging a key will permanently delete it, meaning it will not be recoverable.</span></span>

<span data-ttu-id="bbafe-170">Hello **helyreállítása** és **kiürítése** műveletek engedélye a saját tartozó a kulcstároló hozzáférési házirendben.</span><span class="sxs-lookup"><span data-stu-id="bbafe-170">hello **recover** and **purge** actions have their own permissions associated in a key vault access policy.</span></span> <span data-ttu-id="bbafe-171">A felhasználó vagy szolgáltatás egyszerű toobe képes tooexecute egy **helyreállítása** vagy **kiürítése** művelet hello kulcstároló hozzáférési házirendben hello megfelelő engedéllyel az adott objektum (kulcsok vagy titkos kulcsok) kell rendelkezniük.</span><span class="sxs-lookup"><span data-stu-id="bbafe-171">For a user or service principal toobe able tooexecute a **recover** or **purge** action they must have hello respective permission for that object (key or secret) in hello key vault access policy.</span></span> <span data-ttu-id="bbafe-172">Alapértelmezés szerint hello **kiürítése** engedély nem kerül tooa kulcstároló hozzáférési házirend esetén hello "all" helyi használt toogrant engedélyek tooa összes felhasználó.</span><span class="sxs-lookup"><span data-stu-id="bbafe-172">By default, hello **purge** permission is not added tooa key vault's access policy when hello 'all' shortcut is used toogrant all permissions tooa user.</span></span> <span data-ttu-id="bbafe-173">Kifejezetten biztosítania kell **kiürítése** engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="bbafe-173">You must explicitly grant **purge** permission.</span></span> <span data-ttu-id="bbafe-174">Például a következő parancs biztosít hello user@contoso.com engedély tooperform a kulcsokat számos művelet *ContosoVault* beleértve **kiürítése**.</span><span class="sxs-lookup"><span data-stu-id="bbafe-174">For example, hello following command grants user@contoso.com permission tooperform several operations on keys in *ContosoVault* including **purge**.</span></span>

#### <a name="set-a-key-vault-access-policy"></a><span data-ttu-id="bbafe-175">A kulcstároló hozzáférési házirend beállítása</span><span class="sxs-lookup"><span data-stu-id="bbafe-175">Set a key vault access policy</span></span>

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> <span data-ttu-id="bbafe-176">Ha egy meglévő kulcstároló, amely még csak soft-törlés engedélyezve van, nem lehet **helyreállítása** és **kiürítése** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="bbafe-176">If you have an existing key vault that has just had soft-delete enabled, you may not have **recover** and **purge** permissions.</span></span>

#### <a name="secrets"></a><span data-ttu-id="bbafe-177">Titkos kulcsok</span><span class="sxs-lookup"><span data-stu-id="bbafe-177">Secrets</span></span>

<span data-ttu-id="bbafe-178">Kulcsok, például kulcstároló titkos kulcsainak üzemelteti a saját parancsokkal.</span><span class="sxs-lookup"><span data-stu-id="bbafe-178">Like keys, secrets in a key vault are operated on with their own commands.</span></span> <span data-ttu-id="bbafe-179">A következő, már törlése, listázása, helyreállítása és titkos kulcsok kiürítése hello a parancsokat jelölik.</span><span class="sxs-lookup"><span data-stu-id="bbafe-179">Following, are hello commands for deleting, listing, recovering, and purging secrets.</span></span>

- <span data-ttu-id="bbafe-180">SQLPassword nevű titkos kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="bbafe-180">Delete a secret named SQLPassword:</span></span> 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- <span data-ttu-id="bbafe-181">Kulcstároló törölt titkos kulcsainak listázása:</span><span class="sxs-lookup"><span data-stu-id="bbafe-181">List all deleted secrets in a key vault:</span></span> 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- <span data-ttu-id="bbafe-182">Helyreállítás törölt hello állapotban titkos kulcs:</span><span class="sxs-lookup"><span data-stu-id="bbafe-182">Recover a secret in hello deleted state:</span></span> 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- <span data-ttu-id="bbafe-183">Törölt állapotban titkos kulcs törlése:</span><span class="sxs-lookup"><span data-stu-id="bbafe-183">Purge a secret in deleted state:</span></span> 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
><span data-ttu-id="bbafe-184">Titkos kulcs kiürítése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="bbafe-184">Purging a secret will permanently delete it, meaning it will not be recoverable.</span></span>

## <a name="purging-and-key-vaults"></a><span data-ttu-id="bbafe-185">Tárolók kiürítési és kulcs</span><span class="sxs-lookup"><span data-stu-id="bbafe-185">Purging and key vaults</span></span>

### <a name="key-vault-objects"></a><span data-ttu-id="bbafe-186">Kulcstároló-objektumok</span><span class="sxs-lookup"><span data-stu-id="bbafe-186">Key vault objects</span></span>

<span data-ttu-id="bbafe-187">A kulcs törlése, titkos kulcs vagy tanúsítvány véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="bbafe-187">Purging a key, secret or, certificate will permanently delete it, meaning it will not be recoverable.</span></span> <span data-ttu-id="bbafe-188">hello kulcstároló törlése hello objektumot tartalmazott azonban változatlan marad, a rendszer hello key vaultban lévő összes más objektumra.</span><span class="sxs-lookup"><span data-stu-id="bbafe-188">hello key vault that contained hello deleted object will however remain intact as will all other objects in hello key vault.</span></span> 

### <a name="key-vaults-as-containers"></a><span data-ttu-id="bbafe-189">Kulcs-tárolók tárolóként</span><span class="sxs-lookup"><span data-stu-id="bbafe-189">Key vaults as containers</span></span>
<span data-ttu-id="bbafe-190">Ha a kulcstároló véglegesen törlődnek, és annak teljes tartalmát, beleértve a kulcsokat, a titkos kulcsok és a tanúsítványok, véglegesen törlődik.</span><span class="sxs-lookup"><span data-stu-id="bbafe-190">When a key vault is purged, all of its contents, including keys, secrets, and certificates, are permanently deleted.</span></span> <span data-ttu-id="bbafe-191">toopurge kulcstároló, használja a hello `az keyvault purge` parancsot.</span><span class="sxs-lookup"><span data-stu-id="bbafe-191">toopurge a key vault, use hello `az keyvault purge` command.</span></span> <span data-ttu-id="bbafe-192">Hello helyen az előfizetés törölt kulcstárolójának hello paranccsal is megtalálhatja `az keyvault list-deleted`.</span><span class="sxs-lookup"><span data-stu-id="bbafe-192">You can find hello location your subscription's deleted key vaults using hello command `az keyvault list-deleted`.</span></span>

```azurecli
az keyvault purge --location westus --name ContosoVault
```

>[!NOTE]
><span data-ttu-id="bbafe-193">Kulcstároló törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="bbafe-193">Purging a key vault will permanently delete it, meaning it will not be recoverable.</span></span>

### <a name="purge-permissions-required"></a><span data-ttu-id="bbafe-194">Törli a szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="bbafe-194">Purge permissions required</span></span>
- <span data-ttu-id="bbafe-195">törölt kulcstároló toopurge, úgy, hogy véglegesen eltávolítja hello tárolóhoz és annak teljes tartalmát, a hello felhasználói szükségessége, az RBAC-engedély tooperform egy *Microsoft.KeyVault/locations/deletedVaults/purge/action* műveletet.</span><span class="sxs-lookup"><span data-stu-id="bbafe-195">toopurge a deleted key vault, such that hello vault and all its contents are permanently removed, hello user needs RBAC permission tooperform a *Microsoft.KeyVault/locations/deletedVaults/purge/action* operation.</span></span> 
- <span data-ttu-id="bbafe-196">toolist törölt hello kulcsot, hello tároló felhasználó kell RBAC-engedély tooperform *Microsoft.KeyVault/deletedVaults/read* engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="bbafe-196">toolist hello deleted key, hello vault a user needs RBAC permission tooperform *Microsoft.KeyVault/deletedVaults/read* permission.</span></span> 
- <span data-ttu-id="bbafe-197">Alapértelmezés szerint csak egy előfizetés rendszergazdája rendelkezik ezekkel a jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="bbafe-197">By default only a subscription administrator has these permissions.</span></span> 

### <a name="scheduled-purge"></a><span data-ttu-id="bbafe-198">Ütemezett kiürítése</span><span class="sxs-lookup"><span data-stu-id="bbafe-198">Scheduled purge</span></span>

<span data-ttu-id="bbafe-199">Listázása a kulcstartót törölt objektumok mutat be, amikor azokat kiüríti Key Vault schedled toobe.</span><span class="sxs-lookup"><span data-stu-id="bbafe-199">Listing your deleted key vault objects shows when they are schedled toobe purged by Key Vault.</span></span> <span data-ttu-id="bbafe-200">Hello *kiürítése dátum ütemezett* mező jelzi az amikor egy kulcstartót objektum véglegesen törölve lesz, ha nem tesz.</span><span class="sxs-lookup"><span data-stu-id="bbafe-200">hello *Scheduled Purge Date* field indicates when a key vault object will be permanently deleted, if no action is taken.</span></span> <span data-ttu-id="bbafe-201">Alapértelmezés szerint a törölt kulcstároló objektum hello megőrzési idő 90 nap.</span><span class="sxs-lookup"><span data-stu-id="bbafe-201">By default, hello retention period for a deleted key vault object is 90 days.</span></span>

>[!NOTE]
><span data-ttu-id="bbafe-202">Egy tároló kiürítve objektum által indított annak *kiürítése dátum ütemezett* mezőben, az véglegesen törlődni fog.</span><span class="sxs-lookup"><span data-stu-id="bbafe-202">A purged vault object, triggered by its *Scheduled Purge Date* field, is permanently deleted.</span></span> <span data-ttu-id="bbafe-203">Nincs helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="bbafe-203">It is not recoverable.</span></span>

## <a name="other-resources"></a><span data-ttu-id="bbafe-204">Egyéb erőforrások</span><span class="sxs-lookup"><span data-stu-id="bbafe-204">Other resources</span></span>

- <span data-ttu-id="bbafe-205">Key Vault soft-törlés szolgáltatás áttekintését lásd: [Azure Key Vault soft-törlés – áttekintés](key-vault-ovw-soft-delete.md).</span><span class="sxs-lookup"><span data-stu-id="bbafe-205">For an overview of Key Vault's soft-delete feature, see [Azure Key Vault soft-delete overview](key-vault-ovw-soft-delete.md).</span></span>
- <span data-ttu-id="bbafe-206">Az Azure Key Vault használatának általános áttekintést, lásd: [Ismerkedés az Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bbafe-206">For a general overview of Azure Key Vault usage, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>

