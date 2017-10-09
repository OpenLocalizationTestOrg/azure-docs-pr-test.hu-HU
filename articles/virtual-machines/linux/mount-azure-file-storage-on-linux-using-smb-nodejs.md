---
title: "Azure File storage a Linux virtuális gépek az Azure CLI 1.0-s SMB használatával aaaMount |} Microsoft Docs"
description: "Hogyan toomount Azure File storage a Linux virtuális gépeken SMB használatával"
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
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 14a4224228cadb0ae2f05e8e5c8022ee84f138a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a>Csatlakoztatási Azure File storage a Linux virtuális gépek az Azure CLI 1.0-s SMB használatával

Ez a cikk bemutatja, hogyan toomount Azure File storage a Linux virtuális gép használatával hello Server Message Block (SMB) protokoll. A File storage fájlmegosztások hello felhőben hello szabványos SMB protokollon keresztül biztosít. hello követelményei a következők:

* Egy [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/)
* [Secure Shell (SSH) nyilvános és titkos kulcs fájlok](mac-create-ssh-keys.md)

## <a name="cli-versions-toouse"></a>Parancssori felület verziók toouse
Hello feladat a következő parancssori felület (CLI) verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-a következő generációs CLI hello erőforrás felügyeleti telepítési modell


## <a name="quick-commands"></a>Gyors parancsok
tooaccomplish hello feladat gyorsan, kövesse az ebben a szakaszban hello lépéseket. Részletes információkat és a környezetben, kezdődjenek hello ["Részletes forgatókönyv"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) szakasz.

### <a name="prerequisites"></a>Előfeltételek
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
Adja hozzá a következő sor túl hello`/etc/fstab`:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

A File storage hello felhőben használó fájlmegosztásokról hello szabványos SMB-protokollt biztosít. A File storage kiadása legújabb hello is minden os, amely támogatja az SMB 3.0 lehet csatlakoztatni fájlmegosztást. Az SMB-csatlakoztatási Linux rendszeren való használatakor kapott egyszerű biztonsági mentést tooa robusztus, állandó archiválási tárolóhely szolgáltatásiszint-szerződésben garantált által támogatott.

Helyezze át fájlokat a virtuális gép tooan SMB csatlakoztatási tárolt fájlok tárolására egy remek mód toodebug naplózza. Ennek oka azonos SMB-megosztási hello helyileg lehet csatlakoztatni tooyour Mac, Linux vagy a Windows munkaállomás. Az SMB nem hello legjobb megoldás az adatfolyamként történő Linux vagy alkalmazásnaplók valós időben, mert hello SMB protokoll nem épített toohandle ilyen nehéz naplózási feladatokat. Dedikált, egységes naplózási réteg eszközhöz hasonló eszközökkel Fluentd jobb megoldás, mint az SMB lenne a Linux és a naplózás a kimeneti alkalmazás gyűjtéséhez.

A részletes forgatókönyv létrehozhatunk hello Előfeltételek szükség toofirst hello tárolási fájlmegosztás létrehozása, és csatlakoztathatja SMB-protokollal a Linux virtuális gép.

1. Egy Azure storage-fiók létrehozása a következő kód hello használatával:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. Hello tárfiókkulcsok megjelenítése.

    Amikor létrehoz egy tárfiókot, hello kulcsait jönnek létre párok, hogy a szolgáltatás megszakítás nélkül forgatható el. Amikor hello párokban szereplő toohello második kulcsot, hozzon létre egy új kulcspárt. Új tárfiókkulcsok mindig párosával győződjön meg arról, hogy mindig legyen legalább egy nem használt tárolási kulcs készen tooswitch való jönnek létre. tooshow hello tárfiókok kulcsait, a következő kód hello használata:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. Hello File storage-megosztás létrehozása.

    hello File storage-megosztás hello SMB-megosztás tartalmazza. hello kvóta mindig gigabájt (GB) van megadva. toocreate hello File storage-megosztást, a következő kód hello használata:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Hozzon létre hello csatlakoztatásipont-könyvtárat.

    Hozzon létre egy helyi könyvtárat hello Linux fájl rendszer toomount hello SMB-megosztás. SMB-megosztási toohello, amely a File storage semmit írt vagy hello helyi csatlakoztatási könyvtárból olvassa továbbítja. toocreate hello directory, a következő kód használható hello:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Csatlakoztatás SMB-megosztás a következő kód hello segítségével hello:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. SMB csatlakoztatási keresztül újraindítások hello megőrzéséhez.

    Linux virtuális gép hello újraindításakor hello csatlakoztatott SMB-megosztáson fürtköteteiről leállítás során. tooremount hello SMB-megosztás a rendszerindítás, hozzá kell adni egy sor toohello Linux /etc/fstab. Linux hello fstab fájl toolist hello fájlrendszerek toomount jogcímadatokat hello rendszerindítási folyamat során használ. Hozzáadását hello SMB-megosztás biztosítja, hogy hello File storage-megosztás véglegesen csatlakoztatott fájlrendszer a hello Linux virtuális gép. Hello fájl tárolási SMB megosztás tooa hozzáadása új virtuális gép esetén lehetséges felhő inicializálás.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Következő lépések

- [Használatával a felhő inicializálás toocustomize Linux virtuális gép létrehozása során](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [A lemez tooa Linux virtuális gép hozzáadása](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Linux virtuális gép lemezeinek titkosítása hello Azure parancssori felület használatával](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
