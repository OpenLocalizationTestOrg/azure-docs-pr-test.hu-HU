---
title: "aaaCreate SSH kulcs pár Linux virtuális gépek Azure-on |} Microsoft Docs"
description: "Nyilvános és titkos SSH-kulcspárok biztonságos létrehozása Azure-beli Linux rendszerű virtuális gépekhez."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
ms.openlocfilehash: c4c7cec77c9b48295f2a28c8179b30a4dc38a555
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>SSH nyilvános és titkos kulcspárok létrehozása Linux rendszerű virtuális gépekhez

Ez a cikk bemutatja, hogyan toogenerate az SSH protokoll 2-es RSA nyilvános és titkos kulcs fájl pár toouse Linux virtuális gépek.  Egy SSH kulcspár akkor hozhat létre virtuális gépek Azure-alapértelmezett toousing SSH-kulcsok a hitelesítéshez, így nem kell a jelszavak toolog hello.  Jelszavak kitalál is lehet, és nyissa meg a virtuális gépek toorelentless találgatásos kísérletek tooguess be a jelszót. Virtuális gépek Azure-sablonok vagy hello `azure-cli` hello üzembe helyezés, a feladás egy vagy több központi telepítési konfigurációs lépés letiltása a jelszavas bejelentkezéseket az SSH eltávolítása a nyilvános SSH-kulcs tartalmazhatnak.

## <a name="quick-commands"></a>Gyors parancsok

Futtassa a parancsokat követően a rendszerhéjakba, cseréje hello példák segítségével a saját választhat hello.

Az SSH-ra épülő nyilvános kulcsfájl alapértelmezés szerint a következő helyen jön létre: `~/.ssh/id_rsa.pub`. Amikor a rendszer kéri, a következő parancs hello segítségével, a titkos kulcsot a "jelszó" toosecure kell létrehoznia. (hello jelszót a, a jelszót használt tooencrypt a titkos kulcsot.)

```bash
ssh-keygen -t rsa -b 2048 
```

Adja hozzá az újonnan létrehozott hello kulcs túl`ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> hello fenti parancsok szinte minden terjesztéseket Linux operációs rendszeren, de nem feltétlenül működik-tárolókban, hello környezet így nagyon korlátozott módon. 

## <a name="detailed-walkthrough"></a>Részletes bemutató

Az SSH nyilvános és titkos kulcsok használata a hello legegyszerűbb módja toolog tooyour Linux-kiszolgálókon. [Nyilvános kulcsú](https://en.wikipedia.org/wiki/Public-key_cryptography) egy sokkal biztonságosabb módon toolog a tooyour Linux vagy BSD virtuális gép az Azure-ban biztosítja a jelszavak, amely lehet találgatásos módszeren alapuló sokkal könnyebben.

> [!IMPORTANT]
> A nyilvános kulcs bárkivel megosztható, azonban csak Ön (vagy a helyi biztonsági infrastruktúra) rendelkezik a titkos kulccsal.  hello titkos SSH-kulcsot rendelkeznie kell egy [rendkívül biztonságos jelszó](https://www.xkcd.com/936/) (forrás:[xkcd.com](https://xkcd.com)) toosafeguard azt.  Ez a jelszó nem csak tooaccess hello titkos SSH-kulcs és **nem** hello felhasználói fiók jelszavát.  Ha hozzáad egy jelszó tooyour SSH-kulcs, hello 128 bites AES titkosítással, titkos kulcs, az titkosítja, úgy, hogy hello titkos kulcs nélkül hello jelszó toodecrypt használhatatlan azt.  Ha egy támadó ellopott a titkos kulcsot, és hogy a kulcs nem volt meg a jelszót, a lesznek képesek toouse, hogy titkos kulcs toolog hello megfelelő nyilvános kulccsal rendelkező tooany-kiszolgálókon.  Ha egy titkos kulcsot jelszó véd, a kulcsot nem használhatja a támadó, így ez további biztonsági réteget nyújt az Azure-beli infrastruktúra számára.

Ez a cikk hoz létre SSH protokoll verziója 2 RSA nyilvános és titkos kulcs, amelyek központi erőforrás-kezelő hello használata ajánlott.  *ssh-rsa* kulcsok szükségesek hello [portal](https://portal.azure.com) klasszikus és Resource Manager üzembe helyezések esetében.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>SSH-jelszavak letiltása SSH-kulcsokkal

Az Azure legalább 2048 bites ssh-rsa formátumú nyilvános és titkos kulcsokat követel meg. a kulcsok használata toocreate hello `ssh-keygen`, amely kérdések sorát, és ezután ír egy titkos kulcsot és egy megfelelő nyilvános kulcsot. Egy Azure virtuális gép létrehozásakor a hello nyilvános kulcs túl másolódik`~/.ssh/authorized_keys`.  Az SSH-kulcsok `~/.ssh/authorized_keys` használt toochallenge hello ügyfél toomatch hello megfelelő titkos kulcsnak SSH bejelentkezés kapcsolaton vannak.  Egy Azure Linux virtuális gép létrehozásakor SSH-kulcsok használata a hitelesítéshez, Azure konfigurálása hello SSHD server toonot engedélyezése a jelszavas bejelentkezéseket, csak az SSH-kulcsok.  Ezért az SSH-kulcsok Azure Linux virtuális gépek létrehozásával is segítségével biztonságos hello virtuális gép üzembe helyezéséhez és takarítson meg hello Tipikus telepítés utáni konfigurációs lépés letiltása jelszavak hello sshd_config konfigurációs fájlban.

## <a name="using-ssh-keygen"></a>Az ssh-keygen használata

Ezzel a paranccsal létrejön egy jelszóval védett (titkosított) SSH-kulcspárral 2048 bites RSA használatával, és azt megjegyzésként tooeasily képes azonosítani.  

SSH kulcs van-e őrizni, hello alapértelmezés szerint `~/.ssh` könyvtár.  Ha nem rendelkezik egy `~/.ssh` könyvtár, hello `ssh-keygen` parancs létrehozza azt, hello a megfelelő engedélyekkel.

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

*A parancs ismertetése*

`ssh-keygen`hello használt program toocreate hello kulcsok =

`-t rsa`kulcs toocreate, amely hello RSA formátum típusú = [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048`bits hello kulcs =


## <a name="classic-portal-and-x509-certs"></a>A klasszikus portál és az X.509-tanúsítványok

Ha használ hello Azure [klasszikus portál](https://manage.windowsazure.com/), X.509 Tanúsítványos igényel a hello SSH-kulcsok.  Más típusú nyilvános SSH-kulcsok nem engedélyezettek, az X.509-tanúsítványokat *kell* használnia.

egy X.509 tanúsítvány számára a meglévő titkos SSH-RSA-kulcsot a toocreate:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Klasszikus üzembe helyezés az `asm` használatával

Hello klasszikus használata modell rendszerbe állítása (Azure szolgáltatásfelügyelet CLI `asm`), egy SSH-RSA nyilvános kulccsal, vagy egy RFC4716 formázott kulcsban egy **.pem** tároló.  hello SSH-RSA nyilvános kulcs, ez a cikk használja a korábban létrehozott elemek `ssh-keygen`.

egy RFC4716 toocreate egy meglévő nyilvános SSH-kulccsal a kulcs formátuma:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Ssh-keygen – példa

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
hello keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Mentett kulcsfájlok:

`Enter file in which toosave hello key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

Ez a cikk hello kulcspár neve.  Nevű kulcspár **id_rsa** hello alapértelmezett és az egyes eszközök várt hello **id_rsa** titkos kulcsfájl nevét, egy érdemes. hello directory `~/.ssh/` hello SSH-kulcspár és hello SSH konfigurációs fájl alapértelmezett helye.  Ha nincs megadva a teljes elérési úttal `ssh-keygen` lesz az aktuális munkakönyvtárban hello hello kulcsok létrehozása, nem az alapértelmezett hello `~/.ssh`.

Hello listáját `~/.ssh` könyvtár.

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Kulcs jelszava:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`néven hivatkozik tooa használt jelszó tooencrypt hello titkos kulcs "egy hozzáférési kódot."  Az *erősen* tooadd jelszót tooyour kulcspárok ajánlott. A hozzáférési kód védelmet nyújtó hello titkos kulcs nélküli bárki, aki hello kulcsfájl használható toolog hello megfelelő nyilvános kulccsal rendelkező tooany kiszolgálónak. Hozzáadása egy hozzáférési kódot további védelmet nyújt, ha valaki képes toogain hozzáférés tooyour titkos kulcs fájlja, felkínálva idő toochange hello kulcsok használt tooauthenticate meg.

## <a name="using-ssh-agent-toostore-your-private-key-password"></a>Ssh-agent toostore a titkos kulcs jelszavának használata

Írja be a titkos kulcs tooavoid fájl jelszót minden SSH-bejelentkezéskor, használhatja a `ssh-agent` toocache a titkos kulcsfájl jelszavát. Ha egy Mac használ, az OSX Kulcskarikájához hello biztonságosan tárolja a hello titkos kulcsok jelszavát indításakor `ssh-agent`.

Győződjön meg arról, és használjon `ssh-agent` és `ssh-add` tooinform hello SSH rendszer hello kulcs fájlokkal kapcsolatban, hogy hello jelszava nem lesz szükség a toobe interaktív módon használt.

```bash
eval "$(ssh-agent -s)"
```

Adja hozzá a titkos kulcs hello túl`ssh-agent` hello paranccsal `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

hello titkos kulcs jelszava most tárolódik `ssh-agent`.

## <a name="using-ssh-copy-id-tooinstall-hello-new-key"></a>Használatával `ssh-copy-id` tooinstall hello új kulcs
Ha már létrehozott egy virtuális gép telepítése hello új SSH nyilvános kulcs tooyour Linux virtuális gép a hello a következő parancsot, hello VM felhasználónév és a hello kiszolgálócímet cseréje a saját értékeit:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>SSH konfigurációs fájl létrehozása és konfigurálása

A bevált gyakorlat toocreate, és konfigurálja az `~/.ssh/config` fájl toospeed mentése bejelentkezések és az SSH-ügyfél módot optimalizálási.

a következő példa hello a szabványos konfiguráció látható.

### <a name="create-hello-file"></a>Hello fájl létrehozása

```bash
touch ~/.ssh/config
```

### <a name="edit-hello-file-tooadd-hello-new-ssh-configuration"></a>Hello fájl tooadd hello új SSH-konfiguráció szerkesztése:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>`~/.ssh/config` példafájl:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Ez SSH konfigurációs lehetőséget biztosít, részek az egyes kiszolgáló tooenable minden toohave saját dedikált kulcspárral. alapértelmezett beállítások hello (`Host *`) vannak, amelyek nem felelnek meg bármelyik hello adott gazdagép feljebb hello konfigurációs fájlban állomásokra.

### <a name="config-file-explained"></a>A konfigurációs fájl ismertetése

`Host`= hello hívott állomás hello terminál hello neve.  `ssh fedora22`közli `SSH` toouse hello értékek hello beállítások blokkban feliratú `Host fedora22` Megjegyzés: állomás, amely logikus a használatra, és nem felel meg a kiszolgáló tényleges állomásnevét hello bármilyen olyan címke lehet.

`Hostname 102.160.203.241`= hello IP-cím vagy hello server férnek hozzá a DNS-nevét.

`User ahmet`= hello távoli felhasználói fiók toouse toohello server bejelentkezéskor.

`PubKeyAuthentication yes`= közli az SSH a kívánt toouse egy SSH-kulcs toolog.

`IdentityFile /home/ahmet/.ssh/id_id_rsa`= hello titkos SSH-kulcsot és a megfelelő nyilvános kulcs toouse hitelesítéshez.

## <a name="ssh-into-linux-without-a-password"></a>SSH-ból Linuxba jelszó nélkül

Most, hogy az SSH-kulcspárral és konfigurált SSH konfigurációs fájl, a Linux virtuális gép tooyour képes toolog biztos gyorsan és biztonságosan. hello első alkalommal jelentkezik be egy SSH-kulcs hello parancssorok tooa-kiszolgáló, az adott kulcsfájl hello jelszavát.

```bash
ssh fedora22
```

### <a name="command-explained"></a>A parancs ismertetése

Ha `ssh fedora22` végrehajtása SSH először megkeresi és betölti a beállításokat a hello `Host fedora22` letiltása, majd betölti az összes hello hello utolsó blokk beállításait fennmaradó `Host *`.

## <a name="next-steps"></a>Következő lépések

Ezután mentése toocreate Azure Linux virtuális gépeket használ hello új nyilvános SSH-kulcs.  Nyilvános SSH-kulcs hello bejelentkezésként használatával létrehozott Azure virtuális gépek, amelyek jobban biztonságos, mint a virtuális gépek hello alapértelmezett bejelentkezési módszer, jelszavak létre.  Az SSH-kulcsokkal létrehozott Azure virtuális gépek alapértelmezés szerint letiltott jelszavakkal vannak konfigurálva, ami megakadályozza a találgatásos támadásokat. Ha az SSH-kulcspárral létrehozásához további segítségre van szüksége, vagy további tanúsítványt igényel, mint például használja a klasszikus portálon hello, lásd: [részletes lépéseket toocreate SSH-kulcspár és tanúsítványok](create-ssh-keys-detailed.md).

* [Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Biztonságos Linux virtuális gép létrehozása hello Azure-portálon](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Biztonságos Linux virtuális gép létrehozása Azure CLI hello](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
