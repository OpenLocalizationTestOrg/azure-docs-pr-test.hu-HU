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
# <a name="how-toouse-key-vault-soft-delete-with-powershell"></a>Hogyan toouse Key Vault soft-törlése a PowerShell használatával

Az Azure Key Vault helyreállítható törlés funkció lehetővé teszi, hogy a törölt tárolók és a tároló objektumok. Pontosabban soft-törlés címek hello a következő esetekben:

- Helyreállítható törlésre kerültek a kulcstároló támogatása
- A kulcstároló objektumok; helyreállítható törlésre támogatása a kulcsok, a titkos kulcsok, és tanúsítványokat

## <a name="prerequisites"></a>Előfeltételek

- Az Azure PowerShell 4.0.0 vagy újabb – Ha nem rendelkezik ilyen már telepítő, Azure PowerShell telepítése, és társítsa azt az Azure-előfizetése, lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/overview). 

>[!NOTE]
> A Key Vault PowerShell kimeneti formázás elavult verziójára fájlba van **előfordulhat, hogy** tölthető helyett hello megfelelő verzióját a környezetbe. Azt vannak előrejelző PowerShell toocontain hello frissített verziója szükséges javítás formázás hello kimeneti, és ekkor frissíti az ebben a témakörben. hello aktuális megoldás kell tapasztal formázási probléma van:
> - Nem is lát hello soft-törlés észlel a következő lekérdezés használata hello engedélyezve van a jelen témakörben ismertetett tulajdonság: `$vault = Get-AzureRmKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


PowerShell Key Vault adott refernece információkért lásd: [Azure Key Vault PowerShell-referencia](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).

## <a name="required-permissions"></a>Szükséges engedélyek

Key Vault műveleteket külön-külön az alábbiak szerint felügyelt keresztül szerepköralapú hozzáférés-vezérlést (RBAC) engedélyekkel:

| Művelet | Leírás | Felhasználói engedélyt |
|:--|:--|:--|
|Lista|Listák kulcstároló törlése.|Microsoft.KeyVault/deletedVaults/read|
|Helyreállítás|Visszaállítja a törölt kulcstároló.|Microsoft.KeyVault/vaults/write|
|Véglegesen töröl|Végleg törli a törölt kulcstároló és annak teljes tartalmát.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Hozzáférés-vezérlés az engedélyekkel kapcsolatos további információkért lásd: [a kulcstartót biztonságos](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Soft-Törlés engedélyezése

toobe képes toorecover törölt kulcstároló vagy egy kulcsot a tárolt objektumok tároló, először engedélyeznie kell, hogy kulcstároló soft-törlésének.

### <a name="existing-key-vault"></a>Meglévő kulcstároló

Egy meglévő kulcstároló nevű ContosoVault engedélyezze a soft-Törlés az alábbiak szerint. 

>[!NOTE]
>Jelenleg szüksége toouse Azure Resource Manager erőforrás adatkezelési toodirectly írási hello *enableSoftDelete* tulajdonság toohello Key Vault erőforrás.

```powershell
($resource = Get-AzureRmResource -ResourceId (Get-AzureRmKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzureRmResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Új kulcstartó

Történik, egy új kulcstartó soft-törlésének engedélyezése a létrehozás időpontjában hello soft-Törlés engedélyezése jelzőt hozzáadásával tooyour parancs létrehozása.

```powershell
New-AzureRmKeyVault -VaultName "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Ellenőrizze a soft-Törlés engedélyezése

tooverify, kulcstároló soft-törlés engedélyezve van, amelynek futtatása hello *beolvasása* parancsot, és keressen a "Soft törlése engedélyezve?" hello attribútum és a beállítása true vagy false.

```powershell
Get-AzureRmKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Védi a soft-törlés kulcstároló törlése

hello parancs toodelete (vagy eltávolítása) kulcstároló marad hello azonos, de a viselkedésváltozások attól függően, hogy engedélyezte a soft-törlés vagy sem.

```powershell
Remove-AzureRmKeyVault -VaultName 'ContosoVault'
```

> [!IMPORTANT]
>Hello a kulcstároló, amely nem rendelkezik a soft-törlés engedélyezve van az előző parancs futtatása, azzal végleg törli a kulcstartót és helyreállítási lehetőségeket nélkül minden tartalmát.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hogyan soft-törlés védje a kulcstárolók

Soft-Törlés engedélyezése:

- Kulcstároló törlése után van távolítva az erőforráscsoportot, és el egy fenntartott névtér, amely csak a társított hello helyre, ahol létrehozták. 
- Az objektumok a törölt kulcsot használó, például a kulcs, a titkos kulcsok és tanúsítványok, nem érhető el, és maradnak, amíg azok tartalmazó kulcstároló hello törölt állapotban van. 
- hello DNS-nevét kulcstároló törölt állapotban még fenntartva, ezért nem hozható létre egy új kulcstartó azonos nevű.  

Törölt állapotban kulcstárolót, az Ön előfizetéséhez rendelve hello a következő parancs használatával megtekintheti:

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

Hello *erőforrás-azonosító* hello kimeneti jelenti toohello eredeti erőforrás-azonosító az ehhez a tárolóhoz. Ezen a kulcstartón most már törölt állapotban van, mert nincs erőforrás létezik-e az adott erőforrás-azonosító. Hello *azonosító* mező lehet használt tooidentify hello erőforrás helyreállítása, vagy törlése. Hello *kiürítése dátum ütemezett* mező jelzi, hogy mikor hello tároló véglegesen törli (törölve), ha a törölt tároló történik semmi. hello alapértelmezett megőrzési időszak, felhasznált toocalculate hello *kiürítése dátum ütemezett*, 90 nap.

## <a name="recovering-a-key-vault"></a>Kulcstároló helyreállítása

Kulcstároló toorecover, kell toospecify hello kulcstároló neve, erőforráscsoportot és helyet. Megjegyzés: hello helyét és a törölt hello kulcstároló hello erőforráscsoport szükség van ezekre a kulcstartót a helyreállítási folyamatot.

```powershell
Undo-AzureRmKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Kulcstároló helyreállításakor hello eredménye egy új erőforrás hello kulcstároló eredeti erőforrás-azonosító. Amennyiben már létezett a hello kulcstároló hello erőforráscsoport el lett távolítva, azonos nevű új erőforráscsoport előtt hello kulcstároló állíthatók helyre kell létrehozni.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault objektumok és a soft-törlés

A kulcs, "ContosoFirstKey", a kulcstároló neve "ContosoVault" a soft-törlés engedélyezve van, itt a hogyan akkor törlődik kulcs.

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

A kulcstartót helyreállítható törlésre engedélyezve van, a törölt kulcs azt továbbra is megjelenik, például törlődik, kivéve, ha explicit módon listájához vagy törölt kulcsok beolvasása. A műveleteik zömét a kulcs törlése hello állapotban törölt kulcs listázása, helyreállítását vagy végleges törlésére, kivéve sikertelen lesz. 

Például toorequest törölt toolist kulcs a kulcstároló, használja a következő parancs hello:

```powershell
Get-AzureKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Közbenső műveletfázisa 

Egy kulcsot a kulcstároló törlése a soft-törlés engedélyezve van, a hello áttűnés toocomplete néhány másodpercet vehet igénybe. A műveletfázisa során előfordulhat, hogy megjelenik hello kulcs nem hello aktív vagy hello törölt állapotban van. Ez a parancs felsorolja az összes törölt kulcsok "ContosoVault" nevű key vaultban lévő.

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

### <a name="using-soft-delete-with-key-vault-objects"></a>Kulcstároló objektumok soft-törlés használata

Például a kulcstárolók, a törölt kulcs csak titkos kulcs vagy tanúsítvány állapotban marad törölt a too90 nap mentése kivéve végezze el a helyreállítást, vagy törli azt. 

#### <a name="keys"></a>Kulcsok

a törölt kulcs toorecover:

```powershell
Undo-AzureKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

toopermanently a kulcs törlése:

```powershell
Remove-AzureKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

>[!NOTE]
>A kulcs törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.

Hello **helyreállítása** és **kiürítése** műveletek engedélye a saját tartozó a kulcstároló hozzáférési házirendben. A felhasználó vagy szolgáltatás egyszerű toobe képes tooexecute egy **helyreállítása** vagy **kiürítése** művelet hello kulcstároló hozzáférési házirendben hello megfelelő engedéllyel az adott objektum (kulcsok vagy titkos kulcsok) kell rendelkezniük. Alapértelmezés szerint hello **kiürítése** engedély nem kerül tooa kulcstároló hozzáférési házirend esetén hello "all" helyi használt toogrant engedélyek tooa összes felhasználó. Kifejezetten biztosítania kell **kiürítése** engedéllyel. Például a következő parancs biztosít hello user@contoso.com engedély tooperform a kulcsokat számos művelet *ContosoVault* beleértve **kiürítése**.

#### <a name="set-a-key-vault-access-policy"></a>A kulcstároló hozzáférési házirend beállítása

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Ha egy meglévő kulcstároló, amely még csak soft-törlés engedélyezve van, nem lehet **helyreállítása** és **kiürítése** engedélyek.

#### <a name="secrets"></a>Titkos kulcsok

Kulcsok, például kulcstároló titkos kulcsainak üzemelteti a saját parancsokkal. A következő, már törlése, listázása, helyreállítása és titkos kulcsok kiürítése hello a parancsokat jelölik.

- SQLPassword nevű titkos kulcs törlése: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Kulcstároló törölt titkos kulcsainak listázása: 
```powershell
Get-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Helyreállítás törölt hello állapotban titkos kulcs: 
```powershell
Undo-AzureKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Törölt állapotban titkos kulcs törlése: 
```powershell
Remove-AzureKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
```

>[!NOTE]
>Titkos kulcs kiürítése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.

## <a name="purging-and-key-vaults"></a>Tárolók kiürítési és kulcs

### <a name="key-vault-objects"></a>Kulcstároló-objektumok

A kulcs törlése, titkos kulcs vagy tanúsítvány véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható. hello kulcstároló törlése hello objektumot tartalmazott azonban változatlan marad, a rendszer hello key vaultban lévő összes más objektumra. 

### <a name="key-vaults-as-containers"></a>Kulcs-tárolók tárolóként
Ha a kulcstároló véglegesen törlődnek, és annak teljes tartalmát, beleértve a kulcsokat, a titkos kulcsok és a tanúsítványok, véglegesen törlődik. toopurge kulcstároló, használja a hello `Remove-AzureRmKeyVault` hello beállítás parancsot `-InRemovedState` és hello törölt hello kulcstároló helye hello megadásával `-Location location` argumentum. Hello helyét egy törölt hello paranccsal tárolóban található `Get-AzureRmKeyVault -InRemovedState`.

```powershell
Remove-AzureRmKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

>[!NOTE]
>Kulcstároló törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.

### <a name="purge-permissions-required"></a>Törli a szükséges engedélyek
- törölt kulcstároló toopurge, úgy, hogy véglegesen eltávolítja hello tárolóhoz és annak teljes tartalmát, a hello felhasználói szükségessége, az RBAC-engedély tooperform egy *Microsoft.KeyVault/locations/deletedVaults/purge/action* műveletet. 
- toolist törölt hello kulcsot, hello tároló felhasználó kell RBAC-engedély tooperform *Microsoft.KeyVault/deletedVaults/read* engedéllyel. 
- Alapértelmezés szerint csak egy előfizetés rendszergazdája rendelkezik ezekkel a jogosultságokkal. 

### <a name="scheduled-purge"></a>Ütemezett kiürítése

Listázása a kulcstartót törölt objektumok mutat be, amikor azokat kiüríti Key Vault schedled toobe. Hello *kiürítése dátum ütemezett* mező jelzi az amikor egy kulcstartót objektum véglegesen törölve lesz, ha nem tesz. Alapértelmezés szerint a törölt kulcstároló objektum hello megőrzési idő 90 nap.

>[!NOTE]
>Egy tároló kiürítve objektum által indított annak *kiürítése dátum ütemezett* mezőben, az véglegesen törlődni fog. Nincs helyreállítható.

## <a name="other-resources"></a>Egyéb erőforrások

- Key Vault soft-törlés szolgáltatás áttekintését lásd: [Azure Key Vault soft-törlés – áttekintés](key-vault-ovw-soft-delete.md).
- Az Azure Key Vault használatának általános áttekintést, lásd: [Ismerkedés az Azure Key Vault](key-vault-get-started.md).

