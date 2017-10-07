---
title: "aaaUse Ansible toocreate egy teljes Linux virtuális Gépet az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Ansible toocreate és kezelése a Linux virtuális gép teljes körű környezetet az Azure-ban"
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
ms.openlocfilehash: 970b0427f39fc23240f9faab868196ca4f444e0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Hozzon létre egy teljes Linux virtuális gép környezetet az Azure-ban Ansible
Ansible lehetővé teszi a erőforrások tooautomate hello telepítését és konfigurálását a környezetben. A virtuális gépek (VM) Ansible toomanage használja az Azure-ban, hello ugyanaz, mint bármely egyéb erőforrásokat. Ez a cikk bemutatja, hogyan toocreate teljes körű Linux környezetet és az azt támogató Ansible erőforrásokhoz. Itt olvashat hogyan túl[hozzon létre egy egyszerű virtuális gép Ansible](ansible-create-vm.md).


## <a name="prerequisites"></a>Előfeltételek
toomanage Azure Ansible erőforrásokhoz, a következő hello szüksége:

- Ansible és a gazdagép a rendszerre telepített Azure Python SDK modulok hello.
    - Ansible telepíthető [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), és [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)
- Azure hitelesítő adatait, és a beállított Ansible toouse őket.
    - [Az Azure hitelesítő adatok létrehozása és Ansible konfigurálása](ansible-install-configure.md#create-azure-credentials)
- Az Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. 
    - Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). Is [felhő rendszerhéj](/azure/cloud-shell/quickstart) a böngészőből.


## <a name="create-virtual-network"></a>Virtuális hálózat létrehozása
hello egy Ansible forgatókönyv a következő szakasz létrehoz egy virtuális hálózatot nevű *myVnet* a hello *10.0.0.0/16* címtér:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

tooadd alhálózat, a következő szakasz hello alhálózatot hoz létre nevű *mySubnet* a hello *myVnet* virtuális hálózat:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Nyilvános IP-cím létrehozása
tooaccess erőforrások közötti hello Internet, hozzon létre, és rendelje a nyilvános IP-cím tooyour virtuális gép. hello egy Ansible forgatókönyv a következő szakasz hoz létre egy nyilvános IP-cím nevű *myPublicIP*:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Hálózati biztonsági csoport létrehozása
Hálózati biztonsági csoportok hello folyamatábrán hálózati forgalom mindkét a virtuális Gépet. hello egy Ansible forgatókönyv a következő szakasz a hálózati biztonsági csoportot hoz létre nevű *myNetworkSecurityGroup* határozza meg a szabály tooallow SSH forgalom 22-es TCP-porton és:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: TCP
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a>Hozzon létre a virtuális hálózati kártya
A virtuális hálózati kártya (NIC) a virtuális hálózat, a nyilvános IP-cím és a hálózati biztonsági csoport VM tooa csatlakozik. hello egy Ansible forgatókönyv a következő szakasz hoz létre a virtuális hálózati adapter nevű *myNIC* csatlakozó virtuális hálózati erőforrások toohello hozott létre:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
hello végső lépés toocreate egy virtuális Gépet, és létrehozott összes hello erőforrások használatára. hello egy Ansible forgatókönyv a következő szakasz hoz létre egy elnevezett VM *myVM* és rendeli hello nevű virtuális hálózati adapter *myNIC*. Adja meg a saját nyilvános kulcs adatait hello *key_data* párosítsa a következőképpen:

```yaml
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
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.3'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Teljes Ansible forgatókönyv
toobring ezekben a szakaszokban együtt, hozzon létre egy Ansible alkalmazástervezési nevű *azure_create_complete_vm.yml* és a Beillesztés hello a következő tartalmát:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
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
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

Ansible kell egy erőforrás csoport toodeploy az összes erőforrást. Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/vm#create) paranccsal. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create --name myResourceGroup --location eastus
```

toocreate hello teljes méretű környezet Ansible hello alkalmazástervezési futtassa a következőképpen:

```bash
ansible-playbook azure_create_complete_vm.yml
```

hello kimeneti a következőhöz hasonló toohello hello bemutatja a virtuális gép létrehozása sikeresen befejeződött a következő:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Következő lépések
Ez a példa többek között a virtuális hálózati erőforrások hello szükséges teljes körű Virtuálisgép-környezetet hoz létre. A több közvetlen példa toocreate egy virtuális Gépet a meglévő hálózati erőforrások alapértelmezett beállításokkal, tekintse meg [hozzon létre egy virtuális Gépet](ansible-create-vm.md).
