---
title: "egy felhasználó tooa Azure Linux virtuális gép aaaAdd |} Microsoft Docs"
description: "Adjon hozzá egy felhasználói tooa Linux virtuális gép az Azure-on."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f8aa633b-8b75-45d7-b61d-11ac112cedd5
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/18/2016
ms.author: v-livech
ms.openlocfilehash: eed050154adf0cbed2c168e7aa675bd3ded85fcd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-user-tooan-azure-vm"></a>Egy felhasználó tooan Azure virtuális gép hozzáadása
Első feladatai hello bármely új Linux virtuális gép egyik toocreate egy új felhasználót.  Ez a cikk azt végezze el a sudo felhasználói fiók, jelszó beállítása hello, létrehozása SSH nyilvános kulcsok hozzáadása, és végül használja `visudo` tooallow sudo jelszó nélkül.

Előfeltételek: [az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/), [SSH nyilvános és titkos kulcsok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), egy Azure-erőforráscsoportot és hello Azure parancssori felület telepítve, és tooAzure erőforrás-kezelő módban a kapcsolt `azure config mode arm`.

## <a name="quick-commands"></a>Gyors parancsok
```bash
# Add a new user on RedHat family distros
sudo useradd -G wheel exampleUser

# Add a new user on Debian family distros
sudo useradd -G sudo exampleUser

# Set a password
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

# Copy hello SSH Public Key toohello new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers tooallow no password
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify hello SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Részletes bemutató
### <a name="introduction"></a>Bevezetés
Az új kiszolgáló első és a leggyakoribb feladat hello egyik tooadd egy felhasználói fiókot.  Legfelső szintű bejelentkezések le kell tiltani, és hello root fiókjának magát a Linux-kiszolgálóval, csak a sudo nem használható.  Jogosultságot ad a felhasználói legfelső szintű kiterjesztés sudo hello megfelelő módon tooadminister, és használja a Linux használatával jogosultságokkal.

Hello paranccsal `useradd` azt ad hozzá felhasználói fiókokat toohello Linux virtuális gép.  Futó `useradd` módosítja `/etc/passwd`, `/etc/shadow`, `/etc/group`, és `/etc/gshadow`.  Azt ad hozzá egy parancssori jelző toohello `useradd` tooalso hello új felhasználói toohello megfelelő sudo csoport hozzáadása Linux parancs.  Akkor is Te `useradd` bejegyzést hoz létre a `/etc/passwd` az nem engedi hello új felhasználói fiókhoz jelszót.  Egy kezdeti hello hello egyszerű használatával új felhasználói jelszavát létrehozzuk `passwd` parancsot.  hello utolsó lépése toomodify hello sudo szabályok tooallow sudo jogosultságokkal rendelkező adott felhasználói tooexecute parancsok tooenter minden parancs jelszó nélkül.  Hello titkos kulcsot, a felhasználói fiók jelenleg vélhetően adatokkal van bejelentkezve ezeken a biztonságos és tooallow sudo hozzáférés jelszó nélkül is.  

### <a name="adding-a-single-sudo-user-tooan-azure-vm"></a>A sudo egyetlen felhasználó tooan Azure virtuális gép hozzáadása
Jelentkezzen be toohello SSH-kulcsokat használó Azure virtuális gép.  Ha még nem telepítő SSH nyilvános hozzáférés a kulcshoz, végezze el az ebben a cikkben először [nyilvános kulcs Authentication használata az Azure-ral](http://link.to/article).  

Hello `useradd` parancs végrehajtása után a következő feladatok hello:

* új felhasználói fiók létrehozása
* Hozzon létre egy új felhasználói csoportot hello ugyanazzal a névvel
* üres bejegyzés túl hozzáadása`/etc/passwd`
* üres bejegyzés túl hozzáadása`/etc/gpasswd`

Hello `-G` parancssori jelző hello új felhasználói fiók toohello megfelelő Linux csoport hozzáadása jogosultságot ad az új felhasználói fiók hello legfelső szintű kiterjesztés jogosultságokkal.

#### <a name="add-hello-user"></a>Hello felhasználó hozzáadása
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Jelszó beállítása
Hello `useradd` parancs hello felhasználó hoz létre, és hozzáad egy bejegyzés tooboth `/etc/passwd` és `/etc/gpasswd` , de valójában a hello jelszó nincs beállítva.  hello jelszó kerül hello segítségével toohello bejegyzés `passwd` parancsot.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Most már tudunk sudo jogosultsággal hello kiszolgálón.

### <a name="add-your-ssh-public-key-toohello-new-user-account"></a>Az SSH nyilvános kulcs toohello új felhasználói fiók hozzáadása
A számítógépről, használja a hello `ssh-copy-id` hello új jelszó parancsot.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-tooallow-sudo-usage-without-a-password"></a>Használatával visudo tooallow sudo használati jelszó nélkül
Használatával `visudo` tooedit hello `/etc/sudoers` fájlt ad hozzá néhány rétegének védelmet biztosít a fontos fájl helytelen módosítása.  Végrehajtása után `visudo`, hello `/etc/sudoers` fájl zárolt tooensure más felhasználó nem módosíthatja azt aktívan szerkesztése közben.  Hello `/etc/sudoers` fájl is be van jelölve a hibák által `visudo` toosave kísérletet, vagy amikor a lépni a hibás sudoers fájl nem menthető.

Már van felhasználó hello megfelelő alapértelmezett csoport sudo hozzáférés.  Most fogjuk tooenable ezen csoportok toouse sudo jelszó nélküli.

```bash
# Execute visudo as root tooedit hello /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# toothis
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo tooexecute any command
%sudo   ALL=(ALL:ALL) ALL

# toothis
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-hello-user-ssh-keys-and-sudo"></a>Győződjön meg arról hello felhasználói ssh kulcsok és a sudo
```bash
# Verify hello SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
