---
title: "aaaUpdate hello Azure Linux ügynök a Githubról |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupdate Azure Linux ügynök a Linux virtuális gép Azure toohello legújabb verzió a Githubról"
services: virtual-machines-linux
documentationcenter: 
author: SuperScottz
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: f1f19300-987d-4f29-9393-9aba866f049c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: mingzhan
ms.openlocfilehash: 4ce7c56efc1e6563e6415f7687573f9fb9e7b4c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-hello-azure-linux-agent-on-a-vm"></a>Hogyan tooupdate hello Azure Linux ügynök a virtuális gép

tooupdate a [Azure Linux ügynök](https://github.com/Azure/WALinuxAgent) a Linux virtuális gép az Azure-ban, már rendelkeznie kell:

- A Linux virtuális gép az Azure-ban.
- Egy kapcsolat toothat Linux virtuális gép SSH használatával.

Mindig ellenőrizni kell a csomag hello Linux distro tárházban először. Lehetséges hello csomag rendelkezésre nem lehet hello legújabb verziójára, azonban biztosítja automatikus frissítés engedélyezése hello Linux-ügynök mindig lesz hello legújabb frissítésének letöltése. Kell hello csomag kezelők történő telepítés problémák, támogatási hello distro szállítótól kell keresni.

## <a name="updating-hello-azure-linux-agent"></a>Hello Azure Linux ügynök frissítése

## <a name="ubuntu"></a>Ubuntu

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Frissítési csomag gyorsítótár

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo apt-get install walinuxagent
```

#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Hello waagent szolgáltatás újraindítása

#### <a name="restart-agent-for-1404"></a>Indítsa újra az ügynököt a 14.04

```bash
initctl restart walinuxagent
```

#### <a name="restart-agent-for-1604--1704"></a>Indítsa újra az ügynököt a 16.04 / 17.04

```bash
systemctl restart walinuxagent.service
```

## <a name="debian"></a>Debian

### <a name="debian-7-wheezy"></a>Debian 7 "Wheezy"

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
dpkg -l | grep waagent
```

#### <a name="update-package-cache"></a>Frissítési csomag gyorsítótár

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo apt-get install waagent
```

#### <a name="enable-agent-auto-update"></a>Ügynök automatikus frissítésének engedélyezése
Debian ezen verziója nem rendelkezik egy verzió > = 2.0.16, ezért nem áll rendelkezésre az automatikus frissítés. hello kimenetét a fenti parancs hello megtudhatja, hogy naprakész állapotban-e hello csomag.

### <a name="debian-8-jessie--debian-9-stretch"></a>Debian 8 "Jessie" / "Stretch" Debian 9

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
apt list --installed | grep walinuxagent
```

#### <a name="update-package-cache"></a>Frissítési csomag gyorsítótár

```bash
sudo apt-get -qq update
```

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo apt-get install waagent
```
#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés 

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Hello waagent szolgáltatás újraindítása

```
sudo systemctl restart walinuxagent.service
```

## <a name="redhat--centos"></a>Redhat vagy CentOS

### <a name="rhelcentos-6"></a>RHEL/CentOS 6

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Elérhető frissítések ellenőrzése

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo yum install WALinuxAgent
```

#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés 

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Hello waagent szolgáltatás újraindítása

```
sudo service waagent restart
```

### <a name="rhelcentos-7"></a>RHEL/CentOS 7

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
sudo yum list WALinuxAgent
```

#### <a name="check-available-updates"></a>Elérhető frissítések ellenőrzése

```bash
sudo yum check-update WALinuxAgent
```

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo yum install WALinuxAgent  
```

#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés 

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Hello waagent szolgáltatás újraindítása

```bash
sudo systemctl restart waagent.service
```

## <a name="suse-sles"></a>SUSE SLES

### <a name="suse-sles-11-sp4"></a>SUSE SLES 11 SP4

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Elérhető frissítések ellenőrzése

hello feletti kimeneti megtudhatja, hogy hello csomag toodate be van-e.

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés 

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Hello waagent szolgáltatás újraindítása

```bash
sudo /etc/init.d/waagent restart
```

### <a name="suse-sles-12-sp2"></a>SUSE SLES 12 SP2

#### <a name="check-your-current-package-version"></a>Ellenőrizze a csomag aktuális verziója

```bash
zypper info python-azure-agent
```

#### <a name="check-available-updates"></a>Elérhető frissítések ellenőrzése

A fenti hello hello kimenete ez megtudhatja, ha hello csomag-e legfeljebb dátum.

#### <a name="install-hello-latest-package-version"></a>Hello legújabb csomag verziójának telepítése

```bash
sudo zypper install python-azure-agent
```

#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés 

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="restart-hello-waagent-service"></a>Hello waagent szolgáltatás újraindítása

```bash
sudo systemctl restart waagent.service
```

## <a name="oracle-6-and-7"></a>Oracle 6 és 7

Oracle Linux, győződjön meg arról, hogy hello `Addons` tárház engedélyezve van. Válassza a tooedit hello fájl `/etc/yum.repos.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repos.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a hello sor `enabled=0` túl`enabled=1` alatt **[ol6_addons]** vagy **[ol7_addons]** Ebben a fájlban.

Ezt követően tooinstall hello legújabb verziójának hello Azure Linux ügynök, típus:

```bash
sudo yum install WALinuxAgent
```

Ha nem találja a hello bővítmény tárház egyszerűen hozzá ezeket a sorokat a .repo fájl tooyour Oracle Linux kiadás szerint hello végén:

Oracle Linux 6 virtuális gépek:

```sh
[ol6_addons]
name=Add-Ons for Oracle Linux $releasever ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL6/addons/x86_64
gpgkey=http://public-yum.oracle.com/RPM-GPG-KEY-oracle-ol6
gpgcheck=1
enabled=1
```

Oracle Linux 7 virtuális gépek:

```sh
[ol7_addons]
name=Oracle Linux $releasever Add ons ($basearch)
baseurl=http://public-yum.oracle.com/repo/OracleLinux/OL7/addons/$basearch/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-oracle
gpgcheck=1
enabled=0
```

Írja be:

```bash
sudo yum update WALinuxAgent
```

Ez általában szüksége, de ha valamilyen okból tooinstall kell azt közvetlenül, https://github.com használata hello a következő lépéseket.


## <a name="update-hello-linux-agent-when-no-agent-package-exists-for-distribution"></a>Hello Linux ügynök frissítése, ha nincs ügynök csomag létezik a következő terjesztési

Írja be a wget (van néhány disztribúciókkal, amely nem az alapértelmezés szerint telepítésre kerülnek, például a CentOS, Oracle Linux és a Redhat 6.4 és 6.5-ös verzió) telepítése `sudo yum install wget` hello parancssorban.

### <a name="1-download-hello-latest-version"></a>1. Hello legújabb verzió letöltéséhez
Nyissa meg [hello Azure Linux ügynök a Githubon történő kiadása](https://github.com/Azure/WALinuxAgent/releases) egy weblapot, és megtudhatja, hello legújabb verziószámot. (Beírásával keresheti meg az aktuális verzió `waagent --version`.)

#### <a name="for-version-22x-or-later-type"></a>A verzió 2.2.x vagy újabb, írja be:
```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.x.zip
unzip v2.2.x.zip.zip
cd WALinuxAgent-2.2.x
```

hello következő verzióját használja 2.2.0 példa:

```bash
wget https://github.com/Azure/WALinuxAgent/archive/v2.2.14.zip
unzip v2.2.14.zip  
cd WALinuxAgent-2.2.14
```

### <a name="2-install-hello-azure-linux-agent"></a>2. Hello Azure Linux ügynök telepítése

#### <a name="for-version-22x-use"></a>A verzió 2.2.x, használja:
Előfordulhat, hogy tooinstall hello csomag `setuptools` először – lásd: [Itt](https://pypi.python.org/pypi/setuptools). Ezután futtassa:

```bash
sudo python setup.py install
```

#### <a name="ensure-auto-update-is-enabled"></a>Győződjön meg róla, engedélyezve van az automatikus frissítés

Először ellenőrizze a toosee, ha engedélyezve van:

```bash
cat /etc/waagent.conf
```

"AutoUpdate.Enabled" található. A kimenet jelenik meg, ha engedélyezve van:

```bash
# AutoUpdate.Enabled=y
AutoUpdate.Enabled=y
```

Futtatás tooenable:

```bash
sudo sed -i 's/AutoUpdate.Enabled=n/AutoUpdate.Enabled=y/g' /etc/waagent.conf
```

### <a name="3-restart-hello-waagent-service"></a>3. Hello waagent szolgáltatás újraindítása
A legtöbb Linux disztribúciókkal:

```bash
sudo service waagent restart
```

Ubuntu használja:

```bash
sudo service walinuxagent restart
```

A CoreOS használjon:

```bash
sudo systemctl restart waagent
```

### <a name="4-confirm-hello-azure-linux-agent-version"></a>4. Erősítse meg a hello Azure Linux ügynök verziója
    
```bash
waagent -version
```

CoreOS, a fenti parancs hello nem működnek.

Látni fogja, hogy hello Azure Linux-ügynök verziója lett frissítve toohello új verziója.

Hello Azure Linux ügynök kapcsolatos további információkért lásd: [Azure Linux ügynök információs](https://github.com/Azure/WALinuxAgent).
