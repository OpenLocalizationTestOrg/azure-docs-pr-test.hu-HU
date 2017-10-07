---
title: "aaaUsing felhő inicializálás toocustomize Linux virtuális gép létrehozása az Azure-ban |} Microsoft Docs"
description: "Hogyan toouse felhő inicializálás toocustomize a Linux virtuális gép létrehozása során hello Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: v-livech
ms.openlocfilehash: b9f480bd04029956d0593bbef931795733cbc2f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-cloud-init-toocustomize-a-linux-vm-during-creation-with-hello-azure-cli-10"></a>Linux virtuális gép felhőalapú inicializálás toocustomize közbeni hello Azure CLI 1.0 létrehozása
Ez a cikk bemutatja, hogyan toomake egy felhő-init parancsfájl tooset állomásnév hello telepített csomagok frissítése és felhasználói fiókok kezelése.  hello felhő inicializálás parancsfájlok során a virtuális gép létrehozása az Azure CLI hello nevezzük.  hello cikk van szükség:

* egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),
* Hello [Azure CLI](../../cli-install-nodejs.md) bejelentkezett `azure login`.
* az Azure parancssori felület hello *kell* Azure Resource Manager módra `azure config mode arm`.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="quick-commands"></a>Gyors parancsok
Hozzon létre egy felhő-init.txt parancsfájlt, amely beállítja a hello állomásnév, frissíti az összes csomag, a sudo felhasználói tooLinux hozzáadja.

```sh
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
Hozzon létre egy erőforrás csoport toolaunch a virtuális gépek.

```azurecli
azure group create myResourceGroup westus
```

Linux virtuális gép létrehozása felhőben inicializálás tooconfigure azt a rendszerindítás során.

```azurecli
azure vm create \
  -g myResourceGroup \
  -n myVM \
  -l westus \
  -y Linux \
  -f myVMnic \
  -F myVNet \
  -P 10.0.0.0/22 \
  -j mySubnet \
  -k 10.0.0.0/24 \
  -Q canonical:ubuntuserver:14.04.2-LTS:latest \
  -M ~/.ssh/id_rsa.pub \
  -u myAdminUser \
  -C cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Részletes bemutató
### <a name="introduction"></a>Bevezetés
Amikor új Linux virtuális gép elindításához testreszabott vagy készen áll az Ön igényeinek kap egy szabványos Linux virtuális gép, és semmi ne legyen. [Felhő inicializálás](https://cloudinit.readthedocs.org) megegyezik a szokásos módon tooinject egy parancsfájl vagy a konfigurációs beállításokat a Linux virtuális gép akkor indítja az hello szolgáltatáshoz első alkalommal.

A Azure-ban módosulnak három különböző módon toomake Linux virtuális gép, mert folyamatban van telepítve, vagy a legközelebbi.

* Parancsfájlok felhő inicializálás használatával helyezhet el.
* Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Azure-sablon alapján felhő inicializálás használatával.
* Egy Azure-sablon használatával [CustomScriptExtention](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

indítás után bármikor parancsfájlok tooinject:

* SSH toorun közvetlenül parancsok
* Parancsfájlok hello Azure használatával szúrjon [VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), imperatively vagy az Azure-sablon alapján
* Konfigurációs felügyeleti eszközök, például Ansible, védőérték, Chef vagy Puppet.

> [!NOTE]
> : A VMAccess bővítmény hajt végre egy parancsfájl hello a legfelső szintű azonos módon tudja módon az SSH használatával.  Azonban hello Virtuálisgép-bővítmény használatával lehetővé teszi, hogy számos hasznos a forgatókönyvtől függően az Azure által kínált.
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Felhő inicializálás elérhetőségét a Azure virtuális gép gyors-lemezkép aliasok létrehozására:
| Alias | Közzétevő | Ajánlat | SKU | Verzió | felhő inicializálás |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |legújabb |nem |
| CoreOS |CoreOS |CoreOS |Stable |legújabb |igen |
| Debian |credativ |Debian |8 |legújabb |nem |
| openSUSE |SUSE |openSUSE |13.2 |legújabb |nem |
| RHEL |Redhat |RHEL |7.2 |legújabb |nem |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |legújabb |igen |

A Microsoft a partnerek tooget felhő inicializálás része azokkal, illetve működik, hogy ezek biztosítanak tooAzure hello képek.

## <a name="adding-a-cloud-init-script-toohello-vm-creation-with-hello-azure-cli"></a>A felhő inicializálás parancsfájl toohello virtuális gép létrehozása hello Azure parancssori felület Hozzáadás
toolaunch felhő inicializálás parancsfájl az Azure virtuális gép létrehozásakor adja meg a hello felhő inicializálás fájlt hello Azure parancssori felület használatával `--custom-data` váltani.

Hozzon létre egy erőforrás csoport toolaunch a virtuális gépek.

```azurecli
azure group create myResourceGroup westus
```

Linux virtuális gép létrehozása felhőben inicializálás tooconfigure azt a rendszerindítás során.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubnet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud-init.txt
```

## <a name="creating-a-cloud-init-script-tooset-hello-hostname-of-a-linux-vm"></a>A felhő inicializálás parancsfájl tooset hello állomásnevét Linux virtuális gép létrehozása
Egyik legegyszerűbb hello és a Linux virtuális gép számára legfontosabb beállítások hello állomásnév lenne. A Microsoft könnyedén állíthat be a felhő inicializálás használja ezt a parancsfájlt.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Példa felhő inicializálás parancsfájl nevű `cloud_config_hostname.txt`.
```sh
#cloud-config
hostname: myservername
```

Hello virtuális gép kezdeti indításkor hello, a felhő inicializálás parancsprogram beállítja hello állomásnév túl`myservername`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
  --custom-data cloud_config_hostname.txt
```

Bejelentkezés, és ellenőrizze a hello hello állomásnevét új virtuális Gépet.

```bash
ssh myVM
hostname
myservername
```

## <a name="creating-a-cloud-init-script-tooupdate-linux"></a>A felhő inicializálás parancsfájl tooupdate Linux létrehozása
A biztonság érdekében célszerű az Ubuntu virtuális gép tooupdate hello első indításakor.  Használatával a felhő inicializálás tehetünk ennek, hogy a hello hajtsa végre a parancsfájl hello Linux telepítési módjától függően.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-hello-debian-family"></a>Példa felhő inicializálás parancsfájl `cloud_config_apt_upgrade.txt` a hello Debian termékcsalád
```sh
#cloud-config
apt_upgrade: true
```

Miután Linux betöltődött, az összes telepített hello csomagok segítségével frissített `apt-get`.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
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

## <a name="creating-a-cloud-init-script-tooadd-a-user-toolinux"></a>A felhő inicializálás parancsfájl tooadd egy felhasználó tooLinux létrehozása
Első feladatai hello bármely új Linux virtuális gép egyik tooadd a felhasználó saját kezűleg, illetve tooavoid használata `root`. SSH kulcsok akkor ajánlott a biztonság és a használhatóság és hozzáadásuk után toohello `~/.ssh/authorized_keys` a felhő inicializálás parancsfájl-fájlt.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Példa felhő inicializálás parancsfájl `cloud_config_add_users.txt` Debian termékcsalád
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Miután Linux betöltődött, senki felsorolt hello a létrehozott és a hozzáadott toohello sudo csoport sem.

```azurecli
azure vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --location westus \
  --os-type Linux \
  --nic-name myVMnic \
  --vnet-name myVNet \
  --vnet-address-prefix 10.0.0.0/22 \
  --vnet-subnet-name mySubNet \
  --vnet-subnet-address-prefix 10.0.0.0/24 \
  --image-urn canonical:ubuntuserver:14.04.2-LTS:latest \
  --ssh-publickey-file ~/.ssh/id_rsa.pub \
  --admin-username myAdminUser \
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

[Virtuálisgép-bővítmények és szolgáltatásokkal kapcsolatban](../windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

