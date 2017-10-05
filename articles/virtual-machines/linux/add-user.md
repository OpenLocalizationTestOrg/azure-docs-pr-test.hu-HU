---
title: "Felhasználó hozzáadása a Linux virtuális gépek Azure-on |} Microsoft Docs"
description: "Felhasználó hozzáadása az Azure-on Linux virtuális gép."
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
ms.openlocfilehash: a95157f57c0cbd1f2a9ed68a0fe83140d7c9ec40
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-user-to-an-azure-vm"></a>Felhasználó hozzáadása az Azure virtuális gép
Az első feladatok számára bármely új Linux virtuális gép egyik hozzon létre egy új felhasználót.  Ez a cikk azt végezze el a sudo felhasználói fiók létrehozásakor a jelszó beállítása SSH nyilvános kulcsok hozzáadása, és végül használja `visudo` sudo jelszó nélkül engedélyezi.

Előfeltételek: [az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/), [SSH nyilvános és titkos kulcsok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), egy Azure-erőforráscsoportot, és az Azure parancssori felület telepítve, és át lett váltva Azure Resource Manager módban a `azure config mode arm`.

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

# Copy the SSH Public Key to the new user
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver

# Change sudoers to allow no password
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL

# Verify everything
# Verify the SSH keys & User account
bill@slackware$ ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```

## <a name="detailed-walkthrough"></a>Részletes bemutató
### <a name="introduction"></a>Bevezetés
A első és a leggyakoribb feladat, az új kiszolgáló egyik adhat hozzá felhasználói fiókot.  Legfelső szintű bejelentkezések le kell tiltani, és a legfelső szintű fiókon nem használható a Linux-kiszolgálóval, csak a sudo.  Jogosultságot ad a felhasználó legfelső szintű kiterjesztés jogosultságokkal sudo használatával azt a megfelelő módon felügyelheti, és használja a Linux.

A paranccsal `useradd` azt ad hozzá felhasználói fiókokat a Linux virtuális Gépet.  Futó `useradd` módosítja `/etc/passwd`, `/etc/shadow`, `/etc/group`, és `/etc/gshadow`.  Azt ad hozzá egy parancssori jelzőjét, hogy a `useradd` parancs futtatásával is adja hozzá az új felhasználó a megfelelő sudo csoport Linux rendszeren.  Akkor is Te `useradd` bejegyzést hoz létre a `/etc/passwd` az nem engedi meg az új felhasználói fiókhoz jelszót.  Az új felhasználó használja az egyszerű egy kezdeti jelszó létrehozzuk `passwd` parancsot.  Az utolsó lépése, hogy engedélyezi, hogy a felhasználó sudo jogosultságokkal rendelkező parancsok végrehajtása ne kelljen minden parancsot írja be a jelszót a sudo szabályok módosítása.  A titkos kulcsot, a felhasználói fiók jelenleg vélhetően adatokkal van bejelentkezve ezeken a biztonságos és sudo hozzáférést jelszó nélkül is.  

### <a name="adding-a-single-sudo-user-to-an-azure-vm"></a>Egyetlen sudo felhasználók hozzáadása az Azure virtuális gép
Jelentkezzen be az SSH-kulcsokat használó Azure virtuális Géphez.  Ha még nem telepítő SSH nyilvános hozzáférés a kulcshoz, végezze el az ebben a cikkben először [nyilvános kulcs Authentication használata az Azure-ral](http://link.to/article).  

A `useradd` parancs az alábbi feladatokat hajtja végre:

* új felhasználói fiók létrehozása
* Hozzon létre egy új felhasználói csoportot ugyanazzal a névvel
* az üres bejegyzés hozzáadása`/etc/passwd`
* az üres bejegyzés hozzáadása`/etc/gpasswd`

A `-G` parancssori jelző hozzáadja az új felhasználói fiók a megfelelő Linux csoporthoz jogosultságot ad az új felhasználói fiók legfelső szintű kiterjesztés jogosultságokkal.

#### <a name="add-the-user"></a>A felhasználó hozzáadása
```bash
# On RedHat family distros
sudo useradd -G wheel exampleUser

# On Debian family distros
sudo useradd -G sudo exampleUser
```

#### <a name="set-a-password"></a>Jelszó beállítása
A `useradd` parancs létrehozza a felhasználó, és mindkét bejegyzés hozzáadása `/etc/passwd` és `/etc/gpasswd` , de nem állítja be a jelszót.  A jelszó hozzá lett adva a bejegyzés a `passwd` parancsot.

```bash
sudo passwd exampleUser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Most már tudunk sudo jogosultsággal a kiszolgálón.

### <a name="add-your-ssh-public-key-to-the-new-user-account"></a>Az SSH nyilvános kulcs hozzáadása az új felhasználói fiók
A számítógépről, használja a `ssh-copy-id` parancsot az új jelszót.

```bash
ssh-copy-id -i ~/.ssh/id_rsa exampleuser@exampleserver
```

### <a name="using-visudo-to-allow-sudo-usage-without-a-password"></a>Visudo segítségével jelszó nélküli sudo használatának engedélyezése
Használatával `visudo` szerkesztése a `/etc/sudoers` fájlt ad hozzá néhány rétegének védelmet biztosít a fontos fájl helytelen módosítása.  Végrehajtása után `visudo`, a `/etc/sudoers` fájl zárolva van, annak érdekében, hogy más felhasználó nem módosíthatja azt aktívan szerkesztése közben.  A `/etc/sudoers` fájl is be van jelölve a hibák által `visudo` amikor megpróbál menti vagy kilép, hibás sudoers fájl nem menthető.

Már van felhasználók a megfelelő alapértelmezett csoport a sudo hozzáféréshez.  Most fogjuk ezeket a csoportokat a jelszó nélküli sudo használandó engedélyezése.

```bash
# Execute visudo as root to edit the /etc/sudoers file
visudo

# On RedHat family distros uncomment this line:
## Same thing without a password
# %wheel        ALL=(ALL)       NOPASSWD: ALL

# to this
## Same thing without a password
%wheel        ALL=(ALL)       NOPASSWD: ALL

# On Debian family distros change this line:
# Allow members of group sudo to execute any command
%sudo   ALL=(ALL:ALL) ALL

# to this
%sudo   ALL=(ALL) NOPASSWD:ALL
```

### <a name="verify-the-user-ssh-keys-and-sudo"></a>Ellenőrizze a felhasználó ssh kulcsok és a sudo
```bash
# Verify the SSH keys & User account
ssh -i ~/.ssh/id_rsa exampleuser@exampleserver

# Verify sudo access
sudo top
```
