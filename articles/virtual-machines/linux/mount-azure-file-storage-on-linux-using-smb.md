---
title: "a Linux virtuális gépeken, az SMB-Azure File storage aaaMount |} Microsoft Docs"
description: "Hogyan toomount Azure File storage a Linux virtuális gépeken, az SMB protokoll segítségével hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 7b34c81e602748b78c924f44a54b876744c28f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>A Linux virtuális gépeken, az SMB-csatlakoztatási Azure fájltároló

Ez a cikk bemutatja, hogyan tooutilize hello Azure File storage szolgáltatást a Linux virtuális gépet egy SMB csatlakoztatása az Azure CLI 2.0 hello. Az Azure File storage hello szabványos SMB protokollt használó hello felhőben fájlmegosztásokat kínál. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). hello követelményei a következők:

- [egy Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)
- [SSH nyilvános- és titkoskulcs-fájlok](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Gyors parancsok

* Egy erőforráscsoportot
* Egy Azure virtuális hálózat
* Az SSH a hálózati biztonsági csoportok bejövő
* Egy alhálózaton
* Egy Azure storage-fiók
* Az Azure storage-fiók kulcsok
* Az Azure File storage-megosztás
* Linux virtuális gép

Cserélje le olyan saját beállításaival.

### <a name="create-a-directory-for-hello-local-mount"></a>Hozzon létre egy könyvtárat hello helyi csatlakoztatási

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a>Hello fájl tárolási SMB megosztás toohello csatlakoztatási pont csatlakoztatása

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a>Újraindítás után is megmaradjanak hello csatlakoztatási
toodo Igen, vegye fel a következő sor toohello hello `/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

A File storage hello felhőben használó fájlmegosztásokról hello szabványos SMB-protokollt biztosít. A File storage kiadása legújabb hello is minden os, amely támogatja az SMB 3.0 lehet csatlakoztatni fájlmegosztást. Az SMB-csatlakoztatási Linux rendszeren való használatakor kapott egyszerű biztonsági mentést tooa robusztus, állandó archiválási tárolóhely szolgáltatásiszint-szerződésben garantált által támogatott.

Helyezze át fájlokat a virtuális gép tooan SMB csatlakoztatási tárolt fájlok tárolására egy remek mód toodebug naplózza. Ennek oka azonos SMB-megosztási hello helyileg lehet csatlakoztatni tooyour Mac, Linux vagy a Windows munkaállomás. Az SMB nem hello legjobb megoldás az adatfolyamként történő Linux vagy alkalmazásnaplók valós időben, mert hello SMB protokoll nem épített toohandle ilyen nehéz naplózási feladatokat. Dedikált, egységes naplózási réteg eszközhöz hasonló eszközökkel Fluentd jobb megoldás, mint az SMB lenne a Linux és a naplózás a kimeneti alkalmazás gyűjtéséhez.

A részletes forgatókönyv létrehozhatunk hello Előfeltételek szükség toofirst hello tárolási fájlmegosztás létrehozása, és csatlakoztathatja SMB-protokollal a Linux virtuális gép.

1. Hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create) toohold hello fájlmegosztást.

    Erőforráscsoport nevű toocreate `myResourceGroup` hello "USA nyugati régiója" helyre, használja a következő példa hello:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Az Azure storage-fiók létrehozása [az storage-fiók létrehozása](/cli/azure/storage/account#create) toostore hello tényleges fájlokkal.

    a tárfiók toocreate mystorageaccount nevű hello Standard_LRS tárolási Termékváltozat, a következő példa használata hello segítségével:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. Hello tárfiókkulcsok megjelenítése.

    Amikor létrehoz egy tárfiókot, hello kulcsait jönnek létre párok, hogy a szolgáltatás megszakítás nélkül forgatható el. Amikor hello párokban szereplő toohello második kulcsot, hozzon létre egy új kulcspárt. Új tárfiókkulcsok mindig párosával győződjön meg arról, hogy mindig legyen legalább egy nem használt tárolási fiók kulcs készen tooswitch való jönnek létre.

    Hello tárfiókkulcsok a hello megtekintése [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list). hello nevű hello tárfiókkulcsainak `mystorageaccount` jelennek meg a következő példa hello:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    tooextract egy kulcs, használja a hello `--query` jelzőt. hello alábbi példa kibontja hello első kulcsát (`[0]`):

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. Hello File storage-megosztás létrehozása.

    hello File storage-megosztás az SMB-megosztási hello tartalmaz [az storage-megosztás létrehozása](/cli/azure/storage/share#create). hello kvóta mindig gigabájt (GB) van megadva. Előző hello hello kulcsok egyikével átadása `az storage account keys list` parancsot. 10 GB-os kvótával mystorageshare nevű hello a következő példa a megosztás létrehozása:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Hozzon létre egy csatlakoztatásipont-könyvtárat.

    Hozzon létre egy helyi könyvtárba hello Linux fájl rendszer toomount hello SMB-megosztás. SMB-megosztási toohello, amely a File storage semmit írt vagy hello helyi csatlakoztatási könyvtárból olvassa továbbítja. egy helyi könyvtár: / mnt/mymountdirectory, például a következő használatát hello toocreate:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. Csatlakoztatási hello SMB megosztás toohello helyi könyvtárba.

    Az alábbiak szerint adja meg a saját tárolási fiók felhasználónevét és a tárfiók kulcsa hello csatlakoztatási hitelesítő adatokat:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. SMB csatlakoztatási keresztül újraindítások hello megőrzéséhez.

    Linux virtuális gép hello újraindításakor hello csatlakoztatott SMB-megosztáson fürtköteteiről leállítás során. SMB-megosztási az rendszerindításkor tooremount hello hozzáadása egy sor toohello Linux /etc/fstab. Linux hello fstab fájl toolist hello fájlrendszerek toomount jogcímadatokat hello rendszerindítási folyamat során használ. Hozzáadását hello SMB-megosztás biztosítja, hogy hello File storage-megosztás véglegesen csatlakoztatott fájlrendszer a hello Linux virtuális gép. Hello fájl tárolási SMB megosztás tooa hozzáadása új virtuális gép esetén lehetséges felhő inicializálás.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Következő lépések

- [Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [A lemez tooa Linux virtuális gép hozzáadása](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Linux virtuális gép lemezeinek titkosítása hello Azure parancssori felület használatával](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
