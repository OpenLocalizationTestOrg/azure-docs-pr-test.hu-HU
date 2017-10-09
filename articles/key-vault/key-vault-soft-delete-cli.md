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
# <a name="how-toouse-key-vault-soft-delete-with-cli"></a>Hogyan toouse Key Vault soft-törlése a parancssori felület

Az Azure Key Vault helyreállítható törlés funkció lehetővé teszi, hogy a törölt tárolók és a tároló objektumok. Pontosabban soft-törlés címek hello a következő esetekben:

- Helyreállítható törlésre kerültek a kulcstároló támogatása
- A kulcstároló objektumok; helyreállítható törlésre támogatása a kulcsok, a titkos kulcsok, és tanúsítványokat

## <a name="prerequisites"></a>Előfeltételek

- Lásd az Azure CLI 2.0 – Ha a telepítő nem rendelkezik a környezetnek [kezelése Key Vault CLI 2.0 használatával](key-vault-manage-with-cli2.md).

Parancssori felület Key Vault referenciával információkért lásd: [Azure CLI 2.0 Key Vault hivatkozás](https://docs.microsoft.com/cli/azure/keyvault).

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

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a>Új kulcstartó

Történik, egy új kulcstartó soft-törlésének engedélyezése a létrehozás időpontjában hello soft-Törlés engedélyezése jelzőt hozzáadásával tooyour parancs létrehozása.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Ellenőrizze a soft-Törlés engedélyezése

tooverify, kulcstároló soft-törlés engedélyezve van, amelynek futtatása hello *megjelenítése* parancsot, és keressen a "Soft törlése engedélyezve?" hello attribútum és a beállítása true vagy false.

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-key-vault-protected-by-soft-delete"></a>Védi a soft-törlés kulcstároló törlése

hello parancs toodelete (vagy eltávolítása) kulcstároló marad hello azonos, de a viselkedésváltozások attól függően, hogy engedélyezte a soft-törlés vagy sem.

```azurecli
az keyvault delete --name ContosoVault
```

> [!IMPORTANT]
>Hello a kulcstároló, amely nem rendelkezik a soft-törlés engedélyezve van az előző parancs futtatása, azzal végleg törli a kulcstartót és helyreállítási lehetőségeket nélkül minden tartalmát.

### <a name="how-soft-delete-protects-your-key-vaults"></a>Hogyan soft-törlés védje a kulcstárolók

Soft-Törlés engedélyezése:

- Kulcstároló törlése után van távolítva az erőforráscsoportot, és el egy fenntartott névtér, amely csak a társított hello helyre, ahol létrehozták. 
- Az objektumok a törölt kulcsot használó, például a kulcs, a titkos kulcsok és tanúsítványok, nem érhető el, és maradnak, amíg azok tartalmazó kulcstároló hello törölt állapotban van. 
- hello DNS-nevét kulcstároló törölt állapotban még fenntartva, ezért nem hozható létre egy új kulcstartó azonos nevű.  

Törölt állapotban kulcstárolót, az Ön előfizetéséhez rendelve hello a következő parancs használatával megtekintheti:

```azurecli
az keyvault list-deleted
```

Hello *erőforrás-azonosító* hello kimeneti jelenti toohello eredeti erőforrás-azonosító az ehhez a tárolóhoz. Ezen a kulcstartón most már törölt állapotban van, mert nincs erőforrás létezik-e az adott erőforrás-azonosító. Hello *azonosító* mező lehet használt tooidentify hello erőforrás helyreállítása, vagy törlése. Hello *kiürítése dátum ütemezett* mező jelzi, hogy mikor hello tároló véglegesen törli (törölve), ha a törölt tároló történik semmi. hello alapértelmezett megőrzési időszak, felhasznált toocalculate hello *kiürítése dátum ütemezett*, 90 nap.

## <a name="recovering-a-key-vault"></a>Kulcstároló helyreállítása

Kulcstároló toorecover, kell toospecify hello kulcstároló neve, erőforráscsoportot és helyet. Megjegyzés: hello helyét és a törölt hello kulcstároló hello erőforráscsoport szükség van ezekre a kulcstartót a helyreállítási folyamatot.

```azurecli
az keyvault recover --location westus --name ContosoVault
```

Kulcstároló helyreállításakor hello eredménye egy új erőforrás hello kulcstároló eredeti erőforrás-azonosító. Amennyiben már létezett a hello kulcstároló hello erőforráscsoport el lett távolítva, azonos nevű új erőforráscsoport előtt hello kulcstároló állíthatók helyre kell létrehozni.

## <a name="key-vault-objects-and-soft-delete"></a>Key Vault objektumok és a soft-törlés

A kulcs, "ContosoFirstKey", a kulcstároló neve "ContosoVault" a soft-törlés engedélyezve van, itt a hogyan akkor törlődik kulcs.

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

A kulcstartót helyreállítható törlésre engedélyezve van, a törölt kulcs azt továbbra is megjelenik, például törlődik, kivéve, ha explicit módon listájához vagy törölt kulcsok beolvasása. A műveleteik zömét a kulcs törlése hello állapotban törölt kulcs listázása, helyreállítását vagy végleges törlésére, kivéve sikertelen lesz. 

Például toorequest törölt toolist kulcs a kulcstároló, használja a következő parancs hello:

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Közbenső műveletfázisa 

Egy kulcsot a kulcstároló törlése a soft-törlés engedélyezve van, a hello áttűnés toocomplete néhány másodpercet vehet igénybe. A műveletfázisa során előfordulhat, hogy megjelenik hello kulcs nem hello aktív vagy hello törölt állapotban van. Ez a parancs felsorolja az összes törölt kulcsok "ContosoVault" nevű key vaultban lévő.

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="using-soft-delete-with-key-vault-objects"></a>Kulcstároló objektumok soft-törlés használata

Például a kulcstárolók, a törölt kulcs csak titkos kulcs vagy tanúsítvány állapotban marad törölt a too90 nap mentése kivéve végezze el a helyreállítást, vagy törli azt. 

#### <a name="keys"></a>Kulcsok

a törölt kulcs toorecover:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

toopermanently a kulcs törlése:

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

>[!NOTE]
>A kulcs törlése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.

Hello **helyreállítása** és **kiürítése** műveletek engedélye a saját tartozó a kulcstároló hozzáférési házirendben. A felhasználó vagy szolgáltatás egyszerű toobe képes tooexecute egy **helyreállítása** vagy **kiürítése** művelet hello kulcstároló hozzáférési házirendben hello megfelelő engedéllyel az adott objektum (kulcsok vagy titkos kulcsok) kell rendelkezniük. Alapértelmezés szerint hello **kiürítése** engedély nem kerül tooa kulcstároló hozzáférési házirend esetén hello "all" helyi használt toogrant engedélyek tooa összes felhasználó. Kifejezetten biztosítania kell **kiürítése** engedéllyel. Például a következő parancs biztosít hello user@contoso.com engedély tooperform a kulcsokat számos művelet *ContosoVault* beleértve **kiürítése**.

#### <a name="set-a-key-vault-access-policy"></a>A kulcstároló hozzáférési házirend beállítása

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Ha egy meglévő kulcstároló, amely még csak soft-törlés engedélyezve van, nem lehet **helyreállítása** és **kiürítése** engedélyek.

#### <a name="secrets"></a>Titkos kulcsok

Kulcsok, például kulcstároló titkos kulcsainak üzemelteti a saját parancsokkal. A következő, már törlése, listázása, helyreállítása és titkos kulcsok kiürítése hello a parancsokat jelölik.

- SQLPassword nevű titkos kulcs törlése: 
```azurecli
az keyvault secret delete --vault-name ContosoVault -name SQLPassword
```

- Kulcstároló törölt titkos kulcsainak listázása: 
```azurecli
az keyvault secret list-deleted --vault-name ContosoVault
```

- Helyreállítás törölt hello állapotban titkos kulcs: 
```azurecli
az keyvault secret recover --name SQLPassword --vault-name ContosoVault
```

- Törölt állapotban titkos kulcs törlése: 
```azurecli
az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
```

>[!NOTE]
>Titkos kulcs kiürítése véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható.

## <a name="purging-and-key-vaults"></a>Tárolók kiürítési és kulcs

### <a name="key-vault-objects"></a>Kulcstároló-objektumok

A kulcs törlése, titkos kulcs vagy tanúsítvány véglegesen törli azt, ami azt jelenti, akkor nem fog helyreállítható. hello kulcstároló törlése hello objektumot tartalmazott azonban változatlan marad, a rendszer hello key vaultban lévő összes más objektumra. 

### <a name="key-vaults-as-containers"></a>Kulcs-tárolók tárolóként
Ha a kulcstároló véglegesen törlődnek, és annak teljes tartalmát, beleértve a kulcsokat, a titkos kulcsok és a tanúsítványok, véglegesen törlődik. toopurge kulcstároló, használja a hello `az keyvault purge` parancsot. Hello helyen az előfizetés törölt kulcstárolójának hello paranccsal is megtalálhatja `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
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

