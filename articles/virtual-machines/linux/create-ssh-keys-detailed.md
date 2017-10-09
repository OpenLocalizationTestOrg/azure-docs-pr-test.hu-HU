---
title: "aaaDetailed lépéseket toocreate SSH kulcs pár Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, további lépéseket toocreate az SSH nyilvános és titkos kulcsból álló kulcspárt Linux virtuális gépek Azure-ban a különböző használatából adódó adott tanúsítványokkal együtt."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 6/28/2017
ms.author: danlep
ms.openlocfilehash: 9ac52ef4dc87e73b9c07ccc323adc955829e2014
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-walk-through-toocreate-an-ssh-key-pair-and-additional-certificates-for-a-linux-vm-in-azure"></a>Részletes lépésein végighaladva toocreate egy SSH-kulcspárral és további tanúsítványokat a Linux virtuális gépek Azure-ban
Egy SSH kulcspár akkor hozhat létre virtuális gépek Azure-alapértelmezett toousing SSH-kulcsok a hitelesítéshez, így nem kell a jelszavak toolog hello. Jelszavak kitalál is lehet, és nyissa meg a virtuális gépek toorelentless találgatásos kísérletek tooguess be a jelszót. Hello Azure parancssori felületen vagy a Resource Manager sablonnal létrehozott virtuális gépek lehetnek a nyilvános SSH-kulcs hello üzembe helyezés, a feladás egy vagy több központi telepítési konfigurációs lépés letiltása a jelszavas bejelentkezéseket az SSH eltávolítása. A cikkben részletes lépéseket és további példákat a tanúsítványok előállítása, például Linux virtuális gépekhez való használatra. Ha azt szeretné, hogy tooquickly létrehozása és egy SSH-kulcspárral használatára, lásd: [hogyan toocreate SSH-nyilvános és titkos kulcsot a Linux virtuális gépek Azure-ban párosítsa](mac-create-ssh-keys.md).

## <a name="understanding-ssh-keys"></a>Az SSH-kulcsok bemutatása

Az SSH nyilvános és titkos kulcsok használata a hello legegyszerűbb módja toolog tooyour Linux-kiszolgálókon. [Nyilvános kulcsú](https://en.wikipedia.org/wiki/Public-key_cryptography) egy sokkal biztonságosabb módon toolog a tooyour Linux vagy BSD virtuális gép az Azure-ban biztosítja a jelszavak, amely lehet találgatásos módszeren alapuló sokkal könnyebben.

A nyilvános kulcs bárkivel megosztható, azonban csak Ön (vagy a helyi biztonsági infrastruktúra) rendelkezik a titkos kulccsal.  hello titkos SSH-kulcsot rendelkeznie kell egy [rendkívül biztonságos jelszó](https://www.xkcd.com/936/) (forrás:[xkcd.com](https://xkcd.com)) toosafeguard azt.  Ez a jelszó nem csak tooaccess hello titkos SSH-kulcsfájl és **nem** hello felhasználói fiók jelszavát.  Ha hozzáad egy jelszó tooyour SSH-kulcs, hello 128 bites AES titkosítással, titkos kulcs, az titkosítja, úgy, hogy hello titkos kulcs nélkül hello jelszó toodecrypt használhatatlan azt.  Ha egy támadó ellopott a titkos kulcsot, és hogy a kulcs nem volt meg a jelszót, a lesznek képesek toouse, hogy titkos kulcs toolog hello megfelelő nyilvános kulccsal rendelkező tooany-kiszolgálókon.  Ha egy titkos kulcsot jelszó véd, a kulcsot nem használhatja a támadó, így ez további biztonsági réteget nyújt az Azure-beli infrastruktúra számára.

Ez a cikk az SSH protokoll verziója 2 RSA nyilvános és titkos kulcsot tartalmazó fájlt pár (is hivatkozott tooas "ssh-rsa" kulcsok), amely az Azure Resource Manager telepítésekhez javasolt hoz létre. *ssh-rsa* kulcsok szükségesek hello [portal](https://portal.azure.com) klasszikus és Resource Manager üzembe helyezések esetében.

## <a name="ssh-keys-use-and-benefits"></a>Az SSH-kulcsok használata és a használat előnyei

Az Azure-nak, legalább 2048 bites SSH protokoll 2-es RSA formátumú nyilvános és titkos kulcsok; hello nyilvános kulcsot tartalmazó fájlt rendelkezik hello `.pub` tárolóformátummal. a kulcsok használata toocreate hello `ssh-keygen`, amely kérdések sorát, és ezután ír egy titkos kulcsot és egy megfelelő nyilvános kulcsot. Egy Azure virtuális gép létrehozásakor Azure másolatok hello nyilvános kulcs toohello `~/.ssh/authorized_keys` hello VM mappájába. Az SSH-kulcsok `~/.ssh/authorized_keys` használt toochallenge hello ügyfél toomatch hello megfelelő titkos kulcsnak SSH bejelentkezés kapcsolaton vannak.  Egy Azure Linux virtuális gép létrehozásakor SSH-kulcsok használata a hitelesítéshez, Azure konfigurálása hello SSHD server toonot engedélyezése a jelszavas bejelentkezéseket, csak az SSH-kulcsok.  Ezért SSH-kulccsal rendelkező Azure Linux virtuális gépek létrehozásával segíthet biztonságos hello virtuális gép üzembe helyezéséhez és mentett hello Tipikus telepítés utáni konfigurációs lépés letiltása hello jelszavak **sshd_config** fájlt.

## <a name="using-ssh-keygen"></a>Az ssh-keygen használata

Ezzel a paranccsal létrejön egy jelszóval védett (titkosított) SSH-kulcspárral 2048 bites RSA használatával, és azt megjegyzésként tooeasily képes azonosítani.  

SSH kulcs van-e őrizni, hello alapértelmezés szerint `~/.ssh` könyvtár.  Ha nem rendelkezik egy `~/.ssh` könyvtár, hello `ssh-keygen` parancs létrehozza azt, hello a megfelelő engedélyekkel.

```bash
ssh-keygen \
    -t rsa \
    -b 2048 \
    -C "azureuser@myserver" \
    -f ~/.ssh/id_rsa \
    -N mypassword
```

*A parancs ismertetése*

`ssh-keygen`hello használt program toocreate hello kulcsok =

`-t rsa`= a kulcs toocreate, amely hello RSA formátum [wikipedia] típusú[végén parens](`https://en.wikipedia.org/wiki/RSA_(cryptosystem) `) 
 `-b 2048` bits hello kulcs =

`-C "azureuser@myserver"`= hello nyilvános kulcsot tartalmazó fájlt tooeasily hozzáfűzött toohello végét azonosíthassák megjegyzést.  Általában egy e-mailt használnak hello megjegyzésként, de használhat bármit a legjobban az infrastruktúra.

## <a name="classic-deploy-using-asm"></a>Klasszikus üzembe helyezés az `asm` használatával

Hello klasszikus üzembe helyezési modellel használata (`asm` hello CLI módban), egy SSH-RSA nyilvános kulccsal, vagy egy RFC4716 formázott kulcs pem-tárolóban.  hello SSH-RSA nyilvános kulcs, ez a cikk használja a korábban létrehozott elemek `ssh-keygen`.

egy RFC4716 toocreate egy meglévő nyilvános SSH-kulccsal a kulcs formátuma:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Ssh-keygen – példa

```bash
ssh-keygen -t rsa -b 2048 -C "azureuser@myserver"
Generating public/private rsa key pair.
Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/azureuser/.ssh/id_rsa.
Your public key has been saved in /home/azureuser/.ssh/id_rsa.pub.
hello key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 azureuser@myserver
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

`Enter file in which toosave hello key (/home/azureuser/.ssh/id_rsa): ~/.ssh/id_rsa`

Ez a cikk hello kulcspár neve.  Nevű kulcspár **id_rsa** hello alapértelmezett és az egyes eszközök várt hello **id_rsa** titkos kulcsfájl nevét, egy érdemes. hello directory `~/.ssh/` hello SSH-kulcspár és hello SSH konfigurációs fájl alapértelmezett helye.  Ha nincs megadva a teljes elérési úttal `ssh-keygen` hoz létre hello kulcsok hello aktuális munkakönyvtárban – nem hello alapértelmezett `~/.ssh`.

Hello listáját `~/.ssh` könyvtár.

```bash
ls -al ~/.ssh
-rw------- 1 azureuser staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 azureuser staff   410 Aug 25 18:04 id_rsa.pub
```

Kulcs jelszava:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen`néven hivatkozik tooa jelszó hello titkos kulcsot tartalmazó fájlt a "jelszó szükséges."  Az *erősen* ajánlott tooadd jelszó tooyour titkos kulccsal. Jelszó védelmet nyújtó hello kulcsfájlt, nélkül bárki, aki hello fájl használható toolog tooany Server hello megfelelő nyilvános kulcs. Jelszóval (jelszó) további védelmet nyújt, ha valaki képes toogain hozzáférés tooyour titkos kulcs fájlja, adott ideje toochange hello kulcsok használt tooauthenticate meg.

## <a name="using-ssh-agent-toostore-your-private-key-password"></a>Ssh-agent toostore a titkos kulcs jelszavának használata

Írja be a titkos kulcsfájl jelszavát minden SSH-bejelentkezéskor tooavoid használható `ssh-agent` toocache a titkos kulcsfájl jelszavát. Ha egy Mac használ, az OSX Kulcskarikájához hello biztonságosan tárolja a hello titkos kulcsok jelszavát indításakor `ssh-agent`.

Győződjön meg arról, ssh-agent használ, és ssh-tooinform hello SSH rendszer hozzáadása hello kulcs fájlokkal kapcsolatban, hogy hello jelszava nem lesz szükség a toobe interaktív módon használt.

```bash
eval "$(ssh-agent -s)"
```

Adja hozzá a titkos kulcs hello túl`ssh-agent` hello paranccsal `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

hello titkos kulcs jelszava most tárolódik `ssh-agent`.

## <a name="using-ssh-copy-id-toocopy-hello-key-tooan-existing-vm"></a>Használatával `ssh-copy-id` toocopy hello kulcs tooan meglévő virtuális gép
Ha már létrehozott egy virtuális gép telepítése hello új SSH nyilvános kulcs tooyour Linux virtuális gép együtt:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>SSH konfigurációs fájl létrehozása és konfigurálása

Egy ajánlott bevált gyakorlat toocreate, és konfigurálja az `~/.ssh/config` fájl toospeed mentése bejelentkezések és az SSH-ügyfél módot optimalizálási.

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

Ezután mentése toocreate Azure Linux virtuális gépeket használ hello új nyilvános SSH-kulcs.  Nyilvános SSH-kulcs hello bejelentkezésként használatával létrehozott Azure virtuális gépek, amelyek jobban biztonságos, mint a virtuális gépek hello alapértelmezett bejelentkezési módszer, jelszavak létre.  Az SSH-kulcsokkal létrehozott Azure virtuális gépek alapértelmezés szerint letiltott jelszavakkal vannak konfigurálva, ami megakadályozza a találgatásos támadásokat.

* [Biztonságos Linux virtuális gép létrehozása Azure-sablon alapján](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Biztonságos Linux virtuális gép létrehozása hello Azure-portálon](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Biztonságos Linux virtuális gép létrehozása Azure CLI hello](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
