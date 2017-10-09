---
title: "a Linux és Azure File storage aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toomount egy Azure-fájl megosztása Linux SMB protokollon keresztül."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
ms.openlocfilehash: eeaa24b7f9e646724c5d86ae1e80dfdadaff34fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-storage-with-linux"></a>Azure File storage használata Linux
[Az Azure File storage](../storage-dotnet-how-to-use-files.md) a Microsoft easy toouse felhő fájlrendszer. A Linux terjesztéseket hello segítségével lehet csatlakoztatni az Azure fájlmegosztások [cifs-utils csomag](https://wiki.samba.org/index.php/LinuxCIFS_utils) a hello [Samba projekt](https://www.samba.org/). Ez a cikk bemutatja két módon toomount egy Azure fájlmegosztás: az igény a hello `mount` parancs és a rendszerindítási bejegyzés létrehozásával `/etc/fstab`.

> [!NOTE]  
> A sorrend toomount egy Azure fájlmegosztás hello kívül helyezkedik el, például a helyszínen vagy egy másik Azure régióban, az operációs rendszer hello Azure-régiót támogatnia kell hello titkosítás funkciót az SMB 3.0. Az SMB 3.0 Linux titkosítási szolgáltatás 4.11 kernel jelent meg. Ez a funkció lehetővé teszi, hogy az Azure fájlmegosztás regisztrációját a helyi vagy egy másik Azure-régiót csatlakoztatását. A közzététel hello időpontjában ez a funkció backported tooUbuntu 16.04 és újabb verziók le lett.


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-hello-cifs-utils-package"></a>Egy Azure fájlmegosztás Linux és hello cifs-utils csomaggal csatlakoztatására Prerequisities
* **Válasszon egy Linux-disztribúció, amelyeken telepítve hello cifs-utils csomag**: a Microsoft azt javasolja, hogy a Linux terjesztéseket hello kép: Azure-katalógus következő hello:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**hello cifs-utils telepítve van-e**: hello cifs-utils hello a package manager segítségével az Ön által választott hello Linux terjesztési kell telepíteni. 

    A **Ubuntu** és **Debian-alapú** azokat a terjesztéseket, használja a hello `apt-get` Csomagkezelő:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    A **RHEL** és **CentOS**, használja a hello `yum` Csomagkezelő:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    A **openSUSE**, használja a hello `zypper` Csomagkezelő:

    ```
    sudo zypper install samba*
    ```

    A más azokat a terjesztéseket, használja a megfelelő Csomagkezelő hello vagy [fordítási forrás](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Adja meg a könyvtár vagy fájl engedélyeit hello hello csatlakoztatott megosztást**: hello a következő példa használjuk 0777, toogive olvasási, írási, és végrehajtási engedélyek tooall felhasználók. Más lecserélheti [chmod engedélyek](https://en.wikipedia.org/wiki/Chmod) igény szerint. 

* **A tárfiók neve**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello hello tárfiókja nevére.

* **Tárfiók kulcsa**: toomount egy Azure fájlmegosztás, akkor lesz szüksége hello elsődleges (vagy másodlagos) storage-kulcs. Az SAS-kulcsokkal való csatlakoztatás jelenleg nem támogatott.

* **Győződjön meg arról, 445-ös port meg nyitva**: SMB - 445-ös TCP-porton keresztül kommunikál ellenőrizze toosee, ha a tűzfal nem blokkolja-e a TCP portokat a 445-ös, az ügyfélszámítógépen.

## <a name="mount-hello-azure-file-share-on-demand-with-mount"></a>Azure fájlmegosztás az igény a hello csatlakoztatása`mount`
1. **[A Linux terjesztéshez hello cifs-utils telepítéséhez](#install-cifs-utils)**.

2. **Hozzon létre egy mappát hello csatlakoztatási pont**: ehhez bárhol hello fájlrendszerben.

    ```
    mkdir mymountpoint
    ```

3. **Használjon hello csatlakoztatási parancs toomount hello Azure fájlmegosztás**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, és `<storage-account-key>` hello megfelelő információkkal.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> Ha végzett használatával hello Azure fájlmegosztás, használhatja a `sudo umount ./mymountpoint` toounmount hello megosztást.

## <a name="create-a-persistent-mount-point-for-hello-azure-file-share-with-etcfstab"></a>Egy állandó csatlakoztatási pontot hello Azure fájlmegosztás létrehozása`/etc/fstab`
1. **[A Linux terjesztéshez hello cifs-utils telepítéséhez](#install-cifs-utils)**.

2. **Hozzon létre egy mappát hello csatlakoztatási pont**: ehhez bárhol hello fájlrendszerben, de toonote hello abszolút mappa elérési útja hello van szüksége. a következő példa hello egy legfelső szintű mappát hoz létre.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Használjon hello következő parancsot a sor túl a következő tooappend hello`/etc/fstab`**: Ne feledje tooreplace `<storage-account-name>`, `<share-name>`, és `<storage-account-key>` hello megfelelő információkkal.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Használhat `sudo mount -a` toomount hello Azure fájlmegosztás módosítása után `/etc/fstab` újraindítása helyett.

## <a name="feedback"></a>Visszajelzés
Linux-felhasználók, azt szeretnénk, ha az Ön toohear!

hello Azure File storage Linux-felhasználók csoport visszajelzést biztosítanak a fórumot az Ön tooshare értékelje ki és elfogadják a File storage Linux rendszeren. E-mailek [Azure File storage Linux-felhasználók](mailto:azurefileslinuxusers@microsoft.com) toojoin hello felhasználói csoportnak.

## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozások további információkat tartalmaznak az Azure File Storage-ról.
* [Referencia a fájlszolgáltatás REST API-jához](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Hogyan toouse AzCopy Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Az Azure storage hello Azure parancssori felület használatával](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)
* [Gyakori kérdések](../storage-files-faq.md)
* [hibaelhárítással](storage-troubleshoot-linux-file-connection-problems.md)
