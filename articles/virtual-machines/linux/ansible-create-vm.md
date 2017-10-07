---
title: "aaaUse Ansible toocreate egy alapvető Linux virtuális Gépet az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Ansible toocreate és az Azure-ban alapvető Linux virtuális gépek kezelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2017
ms.author: iainfou
ms.openlocfilehash: ffe278b3f846924ff9c4d026120565146f951152
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Alapszintű virtuális gép létrehozása az Azure-ban Ansible
Ansible lehetővé teszi a erőforrások tooautomate hello telepítését és konfigurálását a környezetben. A virtuális gépek (VM) Ansible toomanage használja az Azure-ban, hello ugyanaz, mint bármely egyéb erőforrásokat. Ez a cikk bemutatja, hogyan toocreate Ansible rendelkező alapszintű virtuális. Itt olvashat hogyan túl[hozzon létre egy teljes körű Virtuálisgép-környezetet Ansible](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Előfeltételek
toomanage Azure Ansible erőforrásokhoz, a következő hello szüksége:

- Ansible és a gazdagép a rendszerre telepített Azure Python SDK modulok hello.
    - Ansible telepíthető [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), és [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)
- Azure hitelesítő adatait, és a beállított Ansible toouse őket.
    - [Az Azure hitelesítő adatok létrehozása és Ansible konfigurálása](ansible-install-configure.md#create-azure-credentials)
- Az Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. 
    - Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). Is [felhő rendszerhéj](/azure/cloud-shell/quickstart) a böngészőből.


## <a name="create-supporting-azure-resources"></a>Hozzon létre az Azure-erőforrások támogatása
Ebben a példában létrehozhatunk egy runbookot, amely egy virtuális Gépet telepít egy meglévő infrastruktúra be. Először hozza létre az erőforráscsoport [az csoport létrehozása](/cli/azure/vm#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

Hozzon létre egy virtuális hálózatot a virtuális gép a [az hálózati vnet létrehozása](/cli/azure/network/vnet#create). hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnet*:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Hozzon létre és futtasson Ansible forgatókönyv
Hozzon létre egy Ansible alkalmazástervezési nevű **azure_create_vm.yml** és a Beillesztés hello követő tartalmát. Ez a példa hoz létre egy virtuális, és konfigurálja az SSH hitelesítő adatokat. Adja meg a saját nyilvános kulcs adatait hello *key_data* párosítsa a következőképpen:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

toocreate hello Ansible, futtassa a következőképpen hello alkalmazástervezési rendelkező virtuális gépet:

```bash
ansible-playbook azure_create_vm.yml
```

hello kimeneti a következőhöz hasonló toohello hello bemutatja a virtuális gép létrehozása sikeresen befejeződött a következő:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Következő lépések
Ebben a példában egy virtuális Gépet hoz létre, egy meglévő erőforráscsoportot és egy virtuális hálózattal, már üzembe van helyezve. Hogyan részletesebb például toouse Ansible toocreate támogató erőforrások, például a virtuális hálózat és a hálózati biztonsági csoportszabályok, lásd: [hozzon létre egy teljes körű Virtuálisgép-környezetet Ansible](ansible-create-complete-vm.md).
