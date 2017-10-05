---
title: "Használja a felhő inicializálás Linux virtuális gép testreszabása |} Microsoft Docs"
description: "Felhő inicializálás használata a Linux virtuális gép testreszabása az Azure CLI 2.0 létrehozása során"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: a7a6daad34525683579e25b9591ed28f2bf29c04
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation"></a>Felhő inicializálás segítségével testre szabhatja a Linux virtuális gép létrehozása során
Ez a cikk bemutatja, hogyan végezheti el a felhő inicializálás parancsfájl az állomásnevet, telepített csomagok frissítése, és az Azure CLI 2.0 felhasználói fiókok kezelése. A felhő inicializálás parancsfájlok nevezzük, amikor egy virtuális gép (VM) hoz létre az Azure parancssori felület. Az alkalmazások részletesebb áttekintése, további konfigurációs fájlok írása, és a Key Vault kulcsok beszúrva: [ebben az oktatóanyagban](tutorial-automate-vm-deployment.md). Az [Azure CLI 1.0-s](using-cloud-init-nodejs.md) verziójával is elvégezheti ezeket a lépéseket.

## <a name="quick-commands"></a>Gyors parancsok
Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja az állomásnevet, frissíti az összes csomagot és sudo felhasználót ad hozzá Linux.

```yaml
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```

Hozzon létre egy erőforráscsoportot, elindíthatja a virtuális gépek a [az csoport létrehozása](/cli/azure/group#create). Az alábbi példakód létrehozza a erőforráscsoportot *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja a rendszerindítás során a `--custom-data` paraméter.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Részletes bemutató
Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen. [Felhő inicializálás](https://cloudinit.readthedocs.org) szabványos módja, szúrjon be ezt a Linux virtuális Gépet egy parancsfájl vagy a konfigurációs beállítások, az első alkalommal szolgáltatáshoz indítja.

A Azure-ban többféleképpen néhány módosításokat Linux virtuális gép, mert folyamatban van telepítve, vagy a legközelebbi.

* Parancsfájlok felhő inicializálás használatával helyezhet el.
* Az Azure használatával parancsfájlok szúrjon [VMAccess bővítmény](using-vmaccess-extension.md).
* Azure-sablon alapján felhő inicializálás használatával.
* Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md).

A beillesztendő parancsfájlok indítás után bármikor:

* SSH közvetlenül a parancsok futtatásához
* Az Azure használatával parancsfájlok szúrjon [VMAccess bővítmény](using-vmaccess-extension.md), imperatively vagy az Azure-sablon alapján
* Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.

> [!NOTE]
> Legfelső szintű SSH használatával ugyanúgy lehet VMAccess bővítmény végrehajtása egy parancsfájlt. Azonban a Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:
| Alias | Közzétevő | Ajánlat | SKU | Verzió | felhő inicializálás |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |legújabb |nem |
| CoreOS |CoreOS |CoreOS |Stable |legújabb |igen |
| Debian |credativ |Debian |8 |legújabb |nem |
| openSUSE |SUSE |openSUSE |13.2 |legújabb |nem |
| RHEL |Redhat |RHEL |7.2 |legújabb |nem |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |legújabb |igen |

A Microsoft a partnerekkel együttműködve az beszerzése felhő inicializálás tartalmazza, és működik-e az Azure biztosít lemezképeket a dolgozik.

## <a name="add-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>A felhő inicializálás parancsfájl hozzáadása a virtuális gép létrehozása az Azure parancssori felülettel
Indítsa el a felhő inicializálás parancsfájl, amikor egy virtuális gép létrehozása az Azure-ban, adja meg a felhő inicializálás fájlt az Azure parancssori felület használatával `--custom-data` váltani.

Hozzon létre egy erőforráscsoportot, elindíthatja a virtuális gépek a [az csoport létrehozása](/cli/azure/group#create). Az alábbi példakód létrehozza a erőforráscsoportot *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Hozzon létre egy felhő-init parancsfájlt állítsa be a Linux virtuális gép állomásnevét
A legegyszerűbb és legfontosabb beállításait minden Linux virtuális gép egyik lenne az állomásnevet. A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.
```yaml
#cloud-config
hostname: myservername
```

A virtuális gép kezdeti indításkor, a felhő inicializálás parancsfájl értékűre állítja be az állomásnév *myservername*. A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Bejelentkezés, és ellenőrizze az új virtuális gép állomásnevét.

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a>Felhő inicializálás parancsfájl létrehozása
A biztonság érdekében célszerű az Ubuntu virtuális gép frissítése a első indításakor. Felhő inicializálás is nézzük meg, attól függően, hogy a Linux-disztribúció használ, a kövesse parancsfájl használatával.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a Debian termékcsalád
```yaml
#cloud-config
apt_upgrade: true
```

Miután Linux betöltődött, a telepített csomagok segítségével frissített **apt get**. A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_apt_upgrade.txt
```

Bejelentkezés, és ellenőrizze, hogy az összes csomag frissítése.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-to-add-a-user-to-linux"></a>A felhasználó hozzáadása Linux felhő inicializálás parancsfájl létrehozása
Az első feladatok számára bármely új Linux virtuális gép egyik felhasználó hozzáadása a szolgáltatást, vagy kerülje a *legfelső szintű*. SSH kulcs-e a biztonság és a használhatóság ajánlott eljárás, és hozzáadja őket a *~/.ssh/authorized_keys* a felhő inicializálás parancsfájl-fájlt.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Példa felhő inicializálás parancsfájl `cloud_config_add_users.txt` Debian termékcsalád
```yaml
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Miután Linux betöltődött, a felsorolt felhasználók létrehozása és a sudo csoportba felvett. A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás segítségével konfigurálja úgy a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

Bejelentkezés, és ellenőrizze az újonnan létrehozott felhasználó.

```bash
ssh myVM
cat /etc/group
```

Kimenet

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Következő lépések
Felhő inicializálás szabványos úgy lehet módosítani a Linux virtuális gép rendszerindító válik. Azure is rendelkezik a Virtuálisgép-bővítmények, amelyek lehetővé teszik a LinuxVM rendszerindító, vagy futás közben módosítani. Például használhatja az Azure VMAccessExtension SSH vagy felhasználói adatok visszaállítására, a virtuális gép futása közben. A felhő inicializálás a jelszó alaphelyzetbe állítása újraindítás kellene.

[Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban](extensions-features.md)

[Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása Azure virtuális gépeken Linux a VMAccess bővítmény használatával lemezek](using-vmaccess-extension.md)