---
title: "a Linux virtuális gép az Azure CLI 1.0 hello aaaEncrypt lemezek |} Microsoft Docs"
description: "Hogyan Linux virtuális gép lemezeinek tooencrypt hello Azure CLI 1.0 és hello Resource Manager telepítési modell"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: 68a0394d366c3c6941e2c6db0d4263123f951946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-hello-azure-cli-10"></a>A Linux virtuális gépet az Azure CLI 1.0 hello lemezzel titkosítása
A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek aktívan titkosíthatók. Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával. Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók. Ez a cikk részletesen, hogyan Linux virtuális gép virtuális lemezein tooencrypt hello Azure CLI 1.0 és hello Resource Manager üzembe helyezési modellben.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello alapszintű hello parancsok tooencrypt virtuális lemezek, a virtuális gépen. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#overview-of-disk-encryption).

Hello kell [Azure CLI legújabb 1.0](../../xplat-cli-install.md) telepítve, és bejelentkezett használatával hello Resource Manager módra az alábbiak szerint:

```azurecli
azure config mode arm
```

Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők `myResourceGroup`, `myKeyVault`, és `myVM`.

Először engedélyezése hello Azure Key Vault szolgáltató belül az Azure-előfizetéshez, és hozzon létre egy erőforráscsoportot. hello alábbi példa létrehoz egy erőforráscsoport neve `myResourceGroup` a hello `WestUS` helye:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

Hozzon létre egy Azure-tárolóban. hello alábbi példakód létrehozza a kulcstároló nevű `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

Titkosítási kulcs létrehozására a kulcstároló, majd engedélyezze a lemez titkosításához. hello alábbi példa létrehoz egy nevű kulcsot `myKey`:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

Hozzon létre egy Azure Active Directory használatával hello hitelesítési kezelésével és a Key Vault a titkosítási kulcsok cseréjét végpontot. Hello `--home-page` és `--identifier-uris` toobe tényleges irányítható cím nem szükséges. Hello legmagasabb szintű biztonság jelszavak helyett ügyfélkulcs kell használni. hello Azure parancssori felület jelenleg nem hozható létre ügyfélkulcs. Titkos ügyfélkulccsal csak az Azure-portálon hello hozható létre. hello alábbi példa létrehoz egy Azure Active Directory-végpontot nevű `myAADApp` és egy jelszót használja `myPassword`. Adja meg a saját jelszavát a következőképpen:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Megjegyzés: hello `applicationId` hello hello megelőző parancs kimenetében megjelennek. Az alkalmazás-azonosító a következő lépéseket hello olyan:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

Adjon hozzá egy meglévő virtuális gép adatok lemez tooan. hello következő példakóddal felveheti egy adatok lemez tooa nevű virtuális gép `myVM`:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

A Key Vault és hello kulcs létrehozott hello részletes adatok áttekintésére. Key Vault azonosító URI és kulcs kell hello hello utolsó lépésként URL-címet. hello alábbi példa ellenőrzi, hogy hello részleteit a Key vault nevű `myKeyVault` és nevű kulcs `myKey`:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

Titkosítása következő, a lemezek megadása egész saját paraméterek nevei:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

hello Azure parancssori felület nem biztosít részletes hibák hello titkosítási folyamat során. További hibaelhárítási információért tekintse át `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`. Hello, mert a parancs megelőző rendelkezik sok változók és sok arra utal, hogy toowhy hello folyamat sikertelen lesz, amelyekhez nem tarozik, a teljes parancs például a következőképpen nézne ki:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

Hello titkosítási állapotának ellenőrzését, végül újra tooconfirm, hogy a virtuális lemezek most lett titkosítva. hello alábbi példa hello állapotát ellenőrzi a virtuális gépek nevű `myVM` a hello `myResourceGroup` erőforráscsoport:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a>Lemeztitkosítás áttekintése
A Linux virtuális gépek virtuális lemezzel titkosított rest az [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására. Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy Kulcslétrehozási a hardveres biztonsági modulokkal (HSM) a hitelesített tooFIPS 140-2 2. szint szabványoknak. A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat. A kriptográfiai kulcsokat használt tooencrypt és virtuális lemezek csatolt tooyour VM visszafejtéséhez. Egy Azure Active Directory végpontján lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.

a virtuális gépek titkosításához hello folyamat a következőképpen történik:

1. Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.
2. Konfigurálja a hello kriptográfiai kulcs toobe lemezek titkosítására használható.
3. tooread hello titkosítási kulcsot az Azure Key Vault hello hozzon létre egy végpontot, az Azure Active Directoryval hello megfelelő engedélyekkel.
4. Hello parancs tooencrypt ki a virtuális lemezek, megadva hello Azure Active Directory végpontján és a megfelelő titkosítási kulcs toobe használt.
5. hello Azure Active Directory végpontján kér az Azure Key Vault hello szükséges titkosítási kulcs.
6. virtuális lemezek hello hello megadott titkosítási kulccsal titkosított.

## <a name="supporting-services-and-encryption-process"></a>Szolgáltatások és a titkosítási folyamat támogatása
Adatok titkosítása a következő további összetevők hello támaszkodik:

* **Az Azure Key Vault** -használt toosafeguard titkosítási kulcsok és titkos hello lemez titkosítási/visszafejtési folyamat használja.
  * Ha van ilyen, használhat egy meglévő Azure Key Vault. Nincs toodedicate a Key Vault tooencrypting lemezek.
  * tooseparate felügyeleti határokat és kulcs látható, a dedikált kulcstároló is létrehozhat.
* **Az Azure Active Directory** - leírók hello szükséges titkosítási kulcsok biztonságos cseréjét, és hitelesítési a kért műveleteket.
  * Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.
  * hello alkalmazás több egy végpont a Key Vault hello és a virtuális gép szolgáltatások toorequest, és lekérése kiadott hello megfelelő titkosítási kulcsokat. Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.

## <a name="requirements-and-limitations"></a>Követelmények és korlátozások
Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:

* a következő Linux server SKU - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) és Red Hat Enterprise Linux hello.
* Minden erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie a hello azonos Azure-régió, és az előfizetés.
* Standard A, a D, a DS-ben, a G és a GS adatsorozat virtuális gépeket.

Lemeztitkosítás jelenleg nem támogatott a következő forgatókönyvek hello:

* Az alapszintű csomag virtuális gépeket.
* A virtuális gépek hello klasszikus telepítési modellel készült.
* Az operációs rendszer lemeztitkosítás Linux virtuális gépeken letiltása.
* Titkosítási kulcsok hello az már titkosított Linux virtuális gép frissítése.

## <a name="create-hello-azure-key-vault-and-keys"></a>Hozzon létre az Azure Key Vault és kulcsok hello
Ez az útmutató toocomplete hello részében szüksége hello [Azure CLI legújabb 1.0](../../xplat-cli-install.md) telepítve, és bejelentkezett használatával hello Resource Manager módra az alábbiak szerint:

```azurecli
azure config mode arm
```

Teljes hello parancspéldákban cserélje le minden példa paraméter a saját nevét, helyét és kulcsértékek. hello alábbi példák szabályt használ a `myResourceGroup`, `myKeyVault`, `myAADApp`stb.

első lépés hello van egy Azure Key Vault toostore toocreate a titkosítási kulcsokat. Az Azure Key Vault tárolhatja a kulcsokat, a titkos kulcsok, vagy a jelszavak, amelyek lehetővé teszik toosecurely végrehajtja az alkalmazások és szolgáltatások. A virtuális lemez titkosításához használja a Key Vault toostore használt tooencrypt egy titkosítási kulcsot, illetve visszafejteni a virtuális lemezek.

Engedélyezi az Azure-előfizetése hello Azure Key Vault szolgáltatót, majd hozzon létre egy erőforráscsoportot. hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUS` helye:

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

hello Azure Key Vault tartalmazó hello kriptográfiai kulcsokat és társított számítási erőforrások, például a tárolási és hello virtuális gépért kell lennie, hello ugyanabban a régióban. hello alábbi példa létrehoz egy Azure Key Vault nevű `myKeyVault`:

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja. A Key Vault prémium HSM használata szükséges. Van egy további költség nélkül toocreating szabványos Key Vault szoftveresen védett tároló helyett a Key Vault támogatás. a prémium kulcstároló, megelőző lépés hello a hozzáadása toocreate `--sku Premium` toohello parancsot. hello alábbi példa szoftveresen védett azt a szabványos kulcstároló létrehozása óta.

Mindkét védelmi modellek hello Azure platformon kell toobe kap hozzáférést toorequest hello titkosítási kulcsok toodecrypt hello virtuális lemezek hello virtuális gép indításakor. Hozzon létre egy titkosítási kulcsot a Key Vault belül, majd a virtuális lemez titkosítás használatának engedélyezése. hello alábbi példa létrehoz egy nevű kulcsot `myKey` és majd lehetővé teszi az adatok titkosítása:

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-hello-azure-active-directory-application"></a>Hello Azure Active Directory-alkalmazás létrehozása
Ha a virtuális lemezek vannak titkosított vagy visszafejtett, használhat egy végpont toohandle hello hitelesítési és titkosítási kulcsok a Key Vault cseréjét. Ehhez a végponthoz, egy Azure Active Directory-alkalmazás, lehetővé teszi, hogy hello Azure platformon toorequest hello megfelelő titkosítási kulcsok hello virtuális gép nevében. Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.

Nem hoz létre a teljes Azure Active Directory-alkalmazást, mert hello `--home-page` és `--identifier-uris` hello a következő példa a paraméterek nem kell toobe tényleges irányítható cím. hello alábbi példa is meghatározza, hogy hello Azure-portálon belül előállítása kulcsokat helyett jelszóalapú titkos kulcs. Most kulcs létrehozásakor nem végezhető el az Azure parancssori felület hello.

Az Azure Active Directory-alkalmazás létrehozása. hello alábbi példa létrehoz egy alkalmazást `myAADApp` és egy jelszót használja `myPassword`. Adja meg a saját jelszavát a következőképpen:

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

Jegyezze fel a hello `applicationId` , visszaadott hello kimenet hello megelőző parancsot. Az alkalmazás azonosító olyan egyes hello hátralévő lépéseket. Ezután hozzon létre egy egyszerű szolgáltatásnév (SPN), így a hello alkalmazás nem érhető el a környezetben. toosuccessfully vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs hello engedélyeinek beállítása toopermit hello Azure Active Directory application tooread hello kulcsok kell lennie.

Hello egyszerű szolgáltatásnév létrehozása, és hello megfelelő engedélyek beállítása az alábbiak szerint:

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a>Adjon hozzá egy virtuális lemezt, és tekintse át a titkosítás állapotát
tooactually titkosítani az egyes virtuális lemezek, lehetővé teszi, hogy a virtuális gép meglévő lemez tooan hozzáadása. Adja hozzá a következőképpen meglévő virtuális gép 5Gb adat lemez tooan:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

virtuális lemezek hello jelenleg nincs titkosítva. Tekintse át a virtuális gép hello aktuális titkosítás állapotát az alábbiak szerint:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a>Virtuális lemezek titkosítása
toonow titkosítása hello virtuális lemezek, akkor egyesítik összes hello az előző összetevők működését:

1. Adja meg a hello Azure Active Directory-alkalmazás és a jelszót.
2. Adja meg a hello Key Vault toostore hello metaadatait a titkosított lemezek.
3. Adja meg a hello titkosítási kulcsok toouse hello tényleges titkosításához és visszafejtéséhez.
4. Adja meg, hogy tooencrypt hello operációsrendszer-lemez, hello adatlemezek vagy az összes.

Lehetővé teszi, hogy az Azure Key Vault és hello létrehozott kulccsal, hello tároló Kulcsazonosító, URI, és majd kulcs URL-címet a végső lépés hello hello részletes adatok áttekintésére:

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

A virtuális lemezek használatával hello hello kimenete titkosítása `azure keyvault show` és `azure keyvault key show` parancsok az alábbiak szerint:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

Hello előző parancs esetében van sok változók, hello következő példája hello teljes parancs referenciaként:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

hello Azure parancssori felület nem biztosít részletes hibák hello titkosítási folyamat során. További hibaelhárítási információért tekintse át a `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` a virtuális gép titkosít hello.

Végezetül lehetővé teszi, hogy tekintse át a hello titkosítási állapot újra, hogy a virtuális lemezek most már titkosítva tooconfirm:

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a>További adatok lemezek hozzáadása
Az adatlemezek titkosított, ha később hozzáadhat további virtuális lemezek tooyour VM és is titkosítani. Hello futtatásakor `azure vm enable-disk-encryption` parancs, a növelést hello feladatütemezési verzió hello segítségével `--sequence-version` paraméter. A feladatütemezési verzió paraméter lehetővé teszi a tooperform történő műveletek hello azonos virtuális gép.

Például lehetővé teszi, hogy az alábbiak szerint adja hozzá a második virtuális lemez tooyour VM:

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

Futtassa újra a hello parancs tooencrypt hello virtuális lemezek, ezúttal hello hozzáadása `--sequence-version` paraméter, és növekvő hello értéket az első futtassa a következőképpen:

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a>Következő lépések
* Az Azure Key Vault, beleértve a titkosítási kulcsok és a tárolók kezelésével kapcsolatos további információért lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md).
* További információ a lemez titkosítása például egy titkosított egyéni VM tooupload tooAzure, előkészítése: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
