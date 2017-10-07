---
title: "a Linux virtuális gép az Azure-ban aaaEncrypt lemezek |} Microsoft Docs"
description: "Hogyan tooencrypt virtuális lemezek, a Linux virtuális gép a fokozott biztonság használatára vonatkozó hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: d6197742bc8562630e8395588c072093fc01d614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-linux-vm"></a>Hogyan tooencrypt virtuális lemezek, a Linux virtuális gép
A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek titkosíthatók. Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával. Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók. Ez a cikk részletesen, hogyan Linux virtuális gép virtuális lemezein tooencrypt hello Azure CLI 2.0. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="quick-commands"></a>Gyors parancsok
Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello alapszintű hello parancsok tooencrypt virtuális lemezek, a virtuális gépen. Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#overview-of-disk-encryption).

Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login). Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *SajátKulcs*, és *myVM*.

Először, engedélyezése hello Azure Key Vault szolgáltató az Azure-előfizetése belül [az szolgáltató regisztrálása](/cli/azure/provider#register) és hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a hello *eastus* helye:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

Hozzon létre egy Azure Key Vault a [az keyvault létrehozása](/cli/azure/keyvault#create) , és engedélyezze a Key Vault hello lemeztitkosítás való használatra. Adjon meg egy egyedi kulcstároló nevet *keyvault_name* az alábbiak szerint:

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

A kulcstároló, a titkosítási kulcs létrehozására [az keyvault kulcs létrehozása](/cli/azure/keyvault/key#create). hello alábbi példa létrehoz egy nevű kulcsot *SajátKulcs*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

Az Azure Active Directoryval az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac). hello szolgáltatás egyszerű kezeli a hitelesítési és titkosítási kulcsok a Key Vault exchange hello. a következő példa hello beolvassa hello értékeinek hello szolgáltatás egyszerű azonosító és jelszó használható újabb parancsok:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

hello jelszó csak kimeneti hello szolgáltatás egyszerű létrehozásakor. Ha szükséges, megtekintése és rekord hello jelszó (`echo $sp_password`). A szolgáltatás elsődleges listázhatja [az sp hirdetéslista](/cli/azure/ad/sp#list) és egyszerű, az adott szolgáltatás további információkat tekinthet [az ad sp megjelenítése](/cli/azure/ad/sp#show).

A kulcstároló, az engedélyeket [az keyvault-szabály beállítása](/cli/azure/keyvault#set-policy). A következő példa hello hello résztvevő-azonosító van megadva a fenti parancs hello:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

A virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) és 5 Gb adatlemezt csatolni. Csak bizonyos piactéren elérhető rendszerkép lemeztitkosítás támogatja. hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` használatával egy **CentOS 7.2n** lemezképet:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH tooyour VM használatával hello `publicIpAddress` hello megelőző parancs kimenetét hello látható. Hozzon létre egy partíciót és filesystem, majd hello adatlemez csatlakoztatása. További információkért lásd: [tooa Linux virtuális gép toomount hello új lemez csatlakozás](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Zárja be az SSH-munkamenetet.

Az a virtuális gép titkosítása [az vm-titkosítás engedélyezése](/cli/azure/vm/encryption#enable). hello alábbi példában hello `$sp_id` és `$sp_password` változók az előző hello `ad sp create-for-rbac` parancs:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Hello lemez titkosítási folyamat toocomplete némi időt vesz igénybe. Hello folyamat hello állapotának figyelése [az vm titkosítási megjelenítése](/cli/azure/vm/encryption#show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello állapota látható szövegrészt **EncryptionInProgress**. Várja meg, amíg hello állapot hello OS lemez jelentések **VMRestartPending**, majd indítsa újra a virtuális Gépet a [az virtuális gép újraindítása](/cli/azure/vm#restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

hello lemez titkosítási folyamat akkor fejeződik be hello rendszerindítási folyamat során, így Várjon néhány percet, majd újra titkosítási hello állapotának ellenőrzése **az vm titkosítási megjelenítése**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello állapota most jelentse hello operációsrendszer-lemez, mind az adatok lemezre **titkosított**.

## <a name="overview-of-disk-encryption"></a>Lemeztitkosítás áttekintése
A Linux virtuális gépek virtuális lemezzel titkosított rest az [dm-crypt](https://wikipedia.org/wiki/Dm-crypt). Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására. Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy Kulcslétrehozási a hardveres biztonsági modulokkal (HSM) a hitelesített tooFIPS 140-2 2. szint szabványoknak. A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat. A kriptográfiai kulcsokat használt tooencrypt és virtuális lemezek csatolt tooyour VM visszafejtéséhez. Egy egyszerű Azure Active Directory szolgáltatás lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.

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

* a következő Linux server SKU - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) és Red Hat Enterprise Linux hello.
* Minden erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie a hello azonos Azure-régió, és az előfizetés.
* Standard A, a D, a DS-ben, a G és a GS adatsorozat virtuális gépeket.

Lemeztitkosítás jelenleg nem támogatott a következő forgatókönyvek hello:

* Az alapszintű csomag virtuális gépeket.
* A virtuális gépek hello klasszikus telepítési modellel készült.
* Az operációs rendszer lemeztitkosítás Linux virtuális gépeken letiltása.
* Titkosítási kulcsok hello az már titkosított Linux virtuális gép frissítése.

## <a name="create-azure-key-vault-and-keys"></a>Az Azure Key Vault és kulcsok létrehozása
Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login). Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *SajátKulcs*, és *myVM*.

első lépés hello van egy Azure Key Vault toostore toocreate a titkosítási kulcsokat. Az Azure Key Vault tárolhatja a kulcsokat, a titkos kulcsok, vagy a jelszavak, amelyek lehetővé teszik toosecurely végrehajtja az alkalmazások és szolgáltatások. A virtuális lemez titkosításához használja a Key Vault toostore használt tooencrypt egy titkosítási kulcsot, illetve visszafejteni a virtuális lemezek.

Engedélyezi az Azure-előfizetése belül hello Azure Key Vault szolgáltatót [az szolgáltató regisztrálása](/cli/azure/provider#register) és hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a hello `eastus` helye:

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

hello Azure Key Vault tartalmazó hello kriptográfiai kulcsokat és társított számítási erőforrások, például a tárolási és hello virtuális gépért kell lennie, hello ugyanabban a régióban. Hozzon létre egy Azure Key Vault a [az keyvault létrehozása](/cli/azure/keyvault#create) , és engedélyezze a Key Vault hello lemeztitkosítás való használatra. Adjon meg egy egyedi kulcstároló nevet *keyvault_name* az alábbiak szerint:

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja. A Key Vault prémium HSM használata szükséges. Van egy további költség nélkül toocreating szabványos Key Vault szoftveresen védett tároló helyett a Key Vault támogatás. a prémium kulcstároló, megelőző lépés hello a hozzáadása toocreate `--sku Premium` toohello parancsot. hello alábbi példa szoftveresen védett azt a szabványos kulcstároló létrehozása óta.

Mindkét védelmi modellek hello Azure platformon kell toobe kap hozzáférést toorequest hello titkosítási kulcsok toodecrypt hello virtuális lemezek hello virtuális gép indításakor. A kulcstároló, a titkosítási kulcs létrehozására [az keyvault kulcs létrehozása](/cli/azure/keyvault/key#create). hello alábbi példa létrehoz egy nevű kulcsot *SajátKulcs*:

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-hello-azure-active-directory-service-principal"></a>Hello Azure Active Directory szolgáltatás egyszerű létrehozása
Amikor a virtuális lemezek vannak titkosított vagy visszafejtett, megadhatja egy toohandle hello hitelesítése és a Key Vault a titkosítási kulcsok cseréjét. Ezt a fiókot, egy Azure Active Directory szolgáltatás egyszerű, lehetővé teszi, hogy hello Azure platformon toorequest hello megfelelő titkosítási kulcsok hello virtuális gép nevében. Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.

Az Azure Active Directoryval az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac). a következő példa hello beolvassa hello értékeinek hello szolgáltatás egyszerű azonosító és jelszó használható újabb parancsok:

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

hello jelszó csak akkor jelenik meg, amikor hello szolgáltatás egyszerű létrehozása. Ha szükséges, megtekintése és rekord hello jelszó (`echo $sp_password`). A szolgáltatás elsődleges listázhatja [az sp hirdetéslista](/cli/azure/ad/sp#list) és egyszerű, az adott szolgáltatás további információkat tekinthet [az ad sp megjelenítése](/cli/azure/ad/sp#show).

toosuccessfully vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs hello engedélyeinek beállítása toopermit hello Azure Active Directory szolgáltatás egyszerű tooread hello kulcsok kell lennie. A kulcstároló, az engedélyeket [az keyvault-szabály beállítása](/cli/azure/keyvault#set-policy). A következő példa hello hello résztvevő-azonosító van megadva a fenti parancs hello:

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
tooactually titkosítani az egyes virtuális lemezek, lehetővé teszi, hogy a virtuális gép létrehozása és a hozzá adatlemezt. Hozzon létre egy virtuális gép tooencrypt rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create) és 5 Gb adatlemezt csatolni. Csak bizonyos piactéren elérhető rendszerkép lemeztitkosítás támogatja. hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* használatával egy **CentOS 7.2n** lemezképet:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

SSH tooyour VM használatával hello `publicIpAddress` hello megelőző parancs kimenetét hello látható. Hozzon létre egy partíciót és filesystem, majd hello adatlemez csatlakoztatása. További információkért lásd: [tooa Linux virtuális gép toomount hello új lemez csatlakozás](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk). Zárja be az SSH-munkamenetet.


## <a name="encrypt-virtual-machine"></a>Virtuális gép titkosítása
tooencrypt hello virtuális lemezeket, akkor egyesítik összes hello az előző összetevők működését:

1. Adja meg a hello Azure Active Directory szolgáltatás egyszerű és a jelszót.
2. Adja meg a hello Key Vault toostore hello metaadatait a titkosított lemezek.
3. Adja meg a hello titkosítási kulcsok toouse hello tényleges titkosításához és visszafejtéséhez.
4. Adja meg, hogy tooencrypt hello operációsrendszer-lemez, hello adatlemezek vagy az összes.

Az a virtuális gép titkosítása [az vm-titkosítás engedélyezése](/cli/azure/vm/encryption#enable). hello alábbi példában hello `$sp_id` és `$sp_password` változók az előző hello `ad sp create-for-rbac` parancs:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

Hello lemez titkosítási folyamat toocomplete némi időt vesz igénybe. Hello folyamat hello állapotának figyelése [az vm titkosítási megjelenítése](/cli/azure/vm/encryption#show):

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello van a hasonló toohello következő csonkolt példa a kimenetre:

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

Várja meg, amíg hello állapot hello OS lemez jelentések **VMRestartPending**, majd indítsa újra a virtuális Gépet a [az virtuális gép újraindítása](/cli/azure/vm#restart):

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

hello lemez titkosítási folyamat akkor fejeződik be hello rendszerindítási folyamat során, így Várjon néhány percet, majd újra titkosítási hello állapotának ellenőrzése **az vm titkosítási megjelenítése**:

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

hello állapota most jelentse hello operációsrendszer-lemez, mind az adatok lemezre **titkosított**.


## <a name="add-additional-data-disks"></a>További adatok lemezek hozzáadása
Az adatlemezek titkosított, ha később hozzáadhat további virtuális lemezek tooyour VM és is titkosítani. Például lehetővé teszi, hogy az alábbiak szerint adja hozzá a második virtuális lemez tooyour VM:

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

Futtassa újra hello parancs tooencrypt hello virtuális lemezek az alábbiak szerint:

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a>Következő lépések
* Az Azure Key Vault, beleértve a titkosítási kulcsok és a tárolók kezelésével kapcsolatos további információért lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md).
* További információ a lemez titkosítása például egy titkosított egyéni VM tooupload tooAzure, előkészítése: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).
