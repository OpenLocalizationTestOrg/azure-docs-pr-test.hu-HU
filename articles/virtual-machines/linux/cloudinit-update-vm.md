---
title: "Felhő inicializálás segítségével frissítése és a csomagok telepítése egy Linux virtuális gépre az Azure-on |} Microsoft Docs"
description: "Felhő inicializálás használata frissítése és a Linux virtuális gép létrehozása az Azure CLI 2.0 csomagok telepítése"
services: virtual-machines-linux
documentationcenter: 
author: rickstercdn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 4209bc270a6d255c8512dd6ccd5551b556da5a6b
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/04/2017
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Felhő inicializálás használatával frissítse és csomagok telepítése egy Linux virtuális gépre az Azure-ban
Ez a cikk bemutatja, hogyan használható [felhő inicializálás](https://cloudinit.readthedocs.io) egy Linux-csomagok frissítése a virtuális gép (VM) vagy virtuálisgép-méretezési beállítja (VMSS) kiépítési idő az Azure-ban. Ezen felhő inicializálás parancsfájlok futtatása az első betöltés után az erőforrásokat az Azure-ban kiépített. Felhő inicializálás működése natív Azure-ban és a Linux támogatott disztribúciókkal kapcsolatos további információkért lásd: [felhő inicializálás áttekintése](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Frissítse a virtuális gép felhőalapú inicializálás
Biztonsági okokból érdemes lehet konfigurálni a virtuális gép első indításakor a legújabb frissítések alkalmazásához. Felhő inicializálás különböző Linux disztribúciókkal keresztül működik, mert nincs szükség meg `apt` vagy `yum` a package manager számára. Helyette megadhatja `package_upgrade` és a felhő inicializálás folyamat határozza meg a distro használja a megfelelő mechanizmus segítségével. Ez a munkafolyamat lehetővé teszi, hogy az azonos felhő inicializálás parancsfájlok disztribúciókkal használja.

Tekintse meg a frissítési folyamat működés közben, hozzon létre egy fájlt az aktuális rendszerhéjban nevű *cloud_init_upgrade.txt* , majd illessze be a következő konfigurációt. Hozzon létre a fájl ebben a példában a felhő rendszerhéj nem a helyi számítógépen. A szerkesztő kívánja használata. Adja meg `sensible-editor cloud_init_upgrade.txt` hozza létre a fájlt, és elérhető szerkesztők listájának megtekintéséhez. Válassza ki a használandó #1 a **nano** szerkesztő. Győződjön meg arról, hogy az egész felhő inicializálás fájl megfelelően lett lemásolva különösen az első sor.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Ez a rendszerkép telepítés megkezdése előtt hozzon létre egy erőforráscsoportot a kell a [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot az *eastus* helyen.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Ezután hozzon létre a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create) , és adja meg a felhő inicializálás fájl `--custom-data cloud_init_upgrade.txt` az alábbiak szerint:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH-kapcsolatot a virtuális Gépet, a fenti parancs kimenetében megjelennek a nyilvános IP-címét. Adja meg a saját **publicIpAddress** az alábbiak szerint:

```bash
ssh <publicIpAddress>
```

Futtassa a csomag felügyeleti eszköz és a frissítések. Az alábbi példában `apt-get` egy Ubuntu virtuális gépen:

```bash
sudo apt-get upgrade
```

Felhő inicializálás ellenőrzi, és a frissítések telepítése a rendszerindító, kell nincsenek frissítések alkalmazásához, ahogy az a következő egy példa a kimenetre:

```bash
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

Is megtekintheti, hogy `httpd` futtatásával telepítették `yum history` , és ellenőrizze, hogy a kimeneti hivatkozási `httpd`. 

## <a name="next-steps"></a>Következő lépések
Konfigurációs módosítások további felhőalapú inicializálás példákért lásd a következő:
 
- [Egy további Linux-felhasználó hozzáadása a virtuális gépek](cloudinit-add-user.md)
- [Futtassa a Csomagkezelő első indításakor meglévő csomagok frissítése](cloudinit-update-vm.md)
- [Módosítsa a virtuális gép helyi állomásnév](cloudinit-update-vm-hostname.md) 
- [Alkalmazáscsomag telepítését, a konfigurációs fájlokat és a kulcsok beszúrása](tutorial-automate-vm-deployment.md)