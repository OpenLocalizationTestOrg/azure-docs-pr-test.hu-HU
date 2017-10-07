---
title: az Azure-on aaaIntroduction tooFreeBSD |} Microsoft Docs
description: "További információ az Azure-on freebsd rendszerű virtuális gépek használatáról"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: eab7aeda7f7ef893740b39c0250aacc29d6fd71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toofreebsd-on-azure"></a>Bevezetés tooFreeBSD az Azure-on
Ez a témakör az Azure-ban freebsd rendszerű virtuális gépet futtató áttekintését.

## <a name="overview"></a>Áttekintés
A Microsoft Azure freebsd rendszerű egy speciális operációs rendszerhez használt toopower modern kiszolgálók, asztali számítógépek és beágyazott platformok.

A Microsoft Corporation van elérhetővé képeket freebsd rendszerű az Azure a hello [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) előre konfigurálva van. Jelenleg hello következő freebsd rendszerű verziók képként által felajánlott Microsoft:

- Freebsd rendszerű 10.3-kiadás
- Freebsd rendszerű 11.0-kiadás

hello ügynök hello freebsd rendszerű virtuális gép közötti kommunikáció felelős és hello Azure-hálót műveletek, például a kiépítés hello virtuális gép első használat (felhasználónév, jelszó vagy SSH-kulcsot, állomásnév, stb.) és a szelektív Virtuálisgép-bővítmények lehetővé teszi funkcióiról.

Mint freebsd rendszerű jövőbeli verzióiban hello stratégia aktuális toostay, és elérhetővé hello legfrissebb verziókban azok közzététele hello freebsd rendszerű kiadás mérnöki csapat követően rövid időn belül.

## <a name="deploying-a-freebsd-virtual-machine"></a>Freebsd rendszerű virtuális gép telepítése
Egy egyszerű folyamatot hello Azure Piactérről származó hello Azure-portálon a lemezkép használatával freebsd rendszerű virtuális gép telepítése:

- [Az Azure piactér hello freebsd rendszerű 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [Az Azure piactér hello freebsd rendszerű 11.0](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a>Az Azure CLI 2.0 keresztül freebsd rendszerű virtuális gép létrehozása az freebsd rendszerű
Először meg kell tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) , ha a következő parancsot egy freebsd rendszerű gépen.

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

Ha a bash nincs telepítve a freebsd rendszerű számítógépre, futtassa a következő parancs hello telepítés előtt. 

```
    sudo pkg install bash
```

Ha a python nincs telepítve a freebsd rendszerű számítógépre, futtassa a következő parancsok hello telepítés előtt. 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

Hello a telepítés során a rendszer felkéri `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`. Válasz `y` , és írja be `/etc/rc.conf` , `a path tooan rc file tooupdate`, előfordulhat, hogy megfelel a hello probléma `ERROR: [Errno 13] Permission denied`. tooresolve a probléma, akkor adja meg az hello írási jobb toocurrent felhasználót hello fájl `etc/rc.conf`.

Most már Azure-e jelentkezni, és a freebsd rendszerű virtuális gép létrehozása. Az alábbiakban van egy példa toocreate 11.0 freebsd rendszerű virtuális gép. Azt is megteheti hello paraméter `--public-ip-address-dns-name` egy újonnan létrehozott nyilvános IP-cím globálisan egyedi DNS-név. 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

Ezután bejelentkezhet tooyour freebsd rendszerű virtuális gép hello IP-címet, amelyet a központi telepítési fent hello kimenetét a keresztül. 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a>Freebsd rendszerű Virtuálisgép-bővítmények
Az alábbiakban freebsd rendszerű támogatott Virtuálisgép-bővítmények.

### <a name="vmaccess"></a>Vmaccess bővítmény
Hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) bővítmény is:

* Hello jelszó-átállítási hello eredeti sudo felhasználó.
* Hozzon létre egy új felhasználót sudo hello jelszót adott meg.
* Állítsa be a hello nyilvános állomáskulcsa megadott hello kulccsal.
* Alaphelyzetbe állítja a hello nyilvános állomáskulcsa során Virtuálisgép-létrehozásnál Ha hello állomáskulcsa nem áll rendelkezésre.
* Nyissa meg a hello SSH-port (22-es), és visszaállítását hello sshd_config reset_ssh tootrue van beállítva.
* Távolítsa el a meglévő felhasználói hello.
* Ellenőrizze a lemezek.
* Javítsa ki az felvett lemezekről.

### <a name="customscript"></a>CustomScript
Hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) bővítmény is:

* Ha a megadott, le hello egyéni parancsfájlok Azure Storage vagy a külső nyilvános tárhelyen (például GitHub).
* Hello belépési pont parancsprogrammal.
* Támogatja a soron belüli parancsokat.
* Windows-stílusú sortörés a rendszerhéj- és Python parancsfájlok automatikusan konvertálni.
* Távolítsa el azt AJ rendszerhéjat, és a Python parancsfájlok automatikusan.
* CommandToExecute bizalmas adatok védelme érdekében.

> [!NOTE]
> Freebsd rendszerű virtuális gép csak támogatja CustomScript mostanra 1.x.  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a>Hitelesítési: felhasználóneveket, jelszavakat és SSH-kulcsok
Freebsd rendszerű virtuális gép létrehozásakor hello Azure portál segítségével meg kell adnia egy felhasználónevet, jelszót vagy nyilvános SSH-kulcs.
Felhasználónevek Azure freebsd rendszerű virtuális gép üzembe helyezéséhez nem felelhet meg rendszerfiókok nevét (UID < 100) már szerepel a hello virtuális gép ("Gyökér", például).
Jelenleg csak hello RSA SSH-kulcsának használata támogatott. A többsoros SSH-kulcs a következővel kell kezdődnie `---- BEGIN SSH2 PUBLIC KEY ----` és végződhet `---- END SSH2 PUBLIC KEY ----`.

## <a name="obtaining-superuser-privileges"></a>Felügyelő jogosultságokat megszerzésére
hello Azure virtuálisgép-példány telepítése során megadott felhasználói fiók egy rendszerjogosultságú fiókot. hello csomagot a sudo telepítette a hello közzétett freebsd rendszerű kép.
Miután jelentkezett be a felhasználói fiókon keresztül, futtathatja parancsok legfelső szintű hello parancsszintaxis segítségével.

```
    $ sudo <COMMAND>
```

Opcionálisan beszerezhet egy legfelső szintű rendszerhéj használatával `sudo -s`.

## <a name="known-issues"></a>Ismert problémák
Hello [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) 2.2.2 rendelkezik [ismert probléma] verziója (https://github.com/Azure/WALinuxAgent/pull/517), ami hello kiépítése sikertelen freebsd rendszerű virtuális gép az Azure-on. hello javítás által rögzített [Azure virtuális gép Vendégügynökének](https://github.com/Azure/WALinuxAgent/) 2.2.3 verziója és a későbbi kibocsátásokban megtörténik. 

## <a name="next-steps"></a>Következő lépések
* Nyissa meg túl[Azure piactér](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate freebsd rendszerű virtuális gép.
* Ha a toobring saját freebsd rendszerű tooAzure, olvassa el túl[létrehozása és feltöltése egy freebsd rendszerű virtuális merevlemez tooAzure](linux/classic/freebsd-create-upload-vhd.md).
