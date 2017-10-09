---
title: "aaaUse felhő inicializálás toocustomize Linux virtuális gép |} Microsoft Docs"
description: "Hogyan toouse felhő inicializálás toocustomize a Linux virtuális gép létrehozása során hello Azure CLI 2.0"
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
ms.openlocfilehash: e7297e162fc73f0da42f195bec2fcbe23b310c1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation"></a>Használja a felhő inicializálás toocustomize Linux virtuális gép létrehozása során
Ez a cikk bemutatja, hogyan toomake egy felhő-init parancsfájl tooset állomásnév hello telepített csomagok frissítése és az Azure CLI 2.0 hello felhasználói fiókok kezelése. hello felhő inicializálás parancsfájlok nevezzük, amikor egy virtuális gép (VM) hoz létre az Azure parancssori felület. Az alkalmazások részletesebb áttekintése, további konfigurációs fájlok írása, és a Key Vault kulcsok beszúrva: [ebben az oktatóanyagban](tutorial-automate-vm-deployment.md). Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](using-cloud-init-nodejs.md).

## <a name="quick-commands"></a>Gyors parancsok
Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja a hello állomásnév, frissíti az összes csomag, a sudo felhasználói tooLinux hozzáadja.

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

Hozzon létre egy erőforrás csoport toolaunch történő rendelkező virtuális gépek [az csoport létrehozása](/cli/azure/group#create). hello alábbi példakód létrehozza hello erőforráscsoportot *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) használatával a felhő inicializálás tooconfigure hello rendszerindítás során `--custom-data` paraméter.

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
Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen. [Felhő inicializálás](https://cloudinit.readthedocs.org) megegyezik a szokásos módon tooinject egy parancsfájl vagy a konfigurációs beállításokat a Linux virtuális gép akkor indítja az hello szolgáltatáshoz első alkalommal.

Azure, amelyeket többféleképpen toomake módosítások alakzatot Linux virtuális gép folyamatban van telepítve, vagy a legközelebbi.

* Parancsfájlok felhő inicializálás használatával helyezhet el.
* Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md).
* Azure-sablon alapján felhő inicializálás használatával.
* Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md).

indítás után bármikor parancsfájlok tooinject:

* SSH toorun közvetlenül parancsok
* Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md), imperatively vagy az Azure-sablon alapján
* Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.

> [!NOTE]
> Végrehajtja a VMAccess bővítmény hello a legfelső szintű azonos parancsfájl tudja módon az SSH használatával. Azonban hello Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:
| Alias | Közzétevő | Ajánlat | SKU | Verzió | felhő inicializálás |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |legújabb |nem |
| CoreOS |CoreOS |CoreOS |Stable |legújabb |igen |
| Debian |credativ |Debian |8 |legújabb |nem |
| openSUSE |SUSE |openSUSE |13.2 |legújabb |nem |
| RHEL |Redhat |RHEL |7.2 |legújabb |nem |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |legújabb |igen |

Folyamatban, a partnerek tooget felhő inicializálás része azokkal, valamint, hogy ezek biztosítanak tooAzure hello képek működik.

## <a name="add-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a>Egy felhő inicializálás parancsfájl toohello virtuális gép létrehozása az Azure CLI hello hozzáadása
toolaunch felhő inicializálás parancsfájl az Azure virtuális gép létrehozásakor adja meg a hello felhő inicializálás fájlt hello Azure parancssori felület használatával `--custom-data` váltani.

Hozzon létre egy erőforrás csoport toolaunch történő rendelkező virtuális gépek [az csoport létrehozása](/cli/azure/group#create). hello alábbi példakód létrehozza hello erőforráscsoportot *myResourceGroup*:

```azurecli
az group create --name myResourceGroup --location eastus
```

A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a>A felhő inicializálás parancsfájl tooset hello állomásnevét Linux virtuális gép létrehozása
Egyik legegyszerűbb hello és a Linux virtuális gép számára legfontosabb beállítások hello állomásnév lenne. A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.
```yaml
#cloud-config
hostname: myservername
```

Hello virtuális gép kezdeti indításkor hello, a felhő inicializálás parancsprogram beállítja hello állomásnév túl*myservername*. A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

Bejelentkezés, és ellenőrizze a hello hello állomásnevét új virtuális Gépet.

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script"></a>Felhő inicializálás parancsfájl létrehozása
A biztonság érdekében célszerű az Ubuntu virtuális gép tooupdate hello első indításakor. Használatával a felhő inicializálás tehetünk ennek, hogy a hello hajtsa végre a parancsfájl hello Linux telepítési módjától függően.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a>Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a hello Debian termékcsalád
```yaml
#cloud-config
apt_upgrade: true
```

Miután Linux betöltődött, az összes telepített hello csomagok segítségével frissített **apt get**. A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.

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
hello following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 tooremove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-tooadd-a-user-toolinux"></a>Hozzon létre egy felhő-init parancsfájl tooadd egy felhasználó tooLinux
Első feladatai hello bármely új Linux virtuális gép egyik tooadd a felhasználó saját kezűleg, illetve tooavoid használata *legfelső szintű*. SSH kulcsok akkor ajánlott a biztonság és a használhatóság és hozzáadásuk után toohello *~/.ssh/authorized_keys* a felhő inicializálás parancsfájl-fájlt.

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

Miután Linux betöltődött, senki felsorolt hello a létrehozott és a hozzáadott toohello sudo csoport sem. A Linux virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) felhő inicializálás tooconfigure használatával, a rendszerindítás során.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud_config_add_users.txt
```

Bejelentkezés és ellenőrzésére, hogy az újonnan létrehozott hello felhasználó.

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
Felhő inicializálás a Linux virtuális gép rendszerindító válik egy szabványos módon toomodify. Azure is rendelkezik Virtuálisgép-bővítmények, amelyek lehetővé teszik toomodify a LinuxVM rendszerindító vagy futtatása során. Például használható hello Azure VMAccessExtension tooreset SSH vagy felhasználói adatok hello virtuális gép futása közben. A felhő inicializálás újraindítás tooreset hello jelszót kellene.

[Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban](extensions-features.md)

[Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény](using-vmaccess-extension.md)
