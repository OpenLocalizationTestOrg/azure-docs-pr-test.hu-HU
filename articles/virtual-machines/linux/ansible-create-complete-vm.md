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
# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a><span data-ttu-id="b7c7d-103">Hozzon létre egy teljes Linux virtuális gép környezetet az Azure-ban Ansible</span><span class="sxs-lookup"><span data-stu-id="b7c7d-103">Create a complete Linux virtual machine environment in Azure with Ansible</span></span>
<span data-ttu-id="b7c7d-104">Ansible lehetővé teszi a erőforrások tooautomate hello telepítését és konfigurálását a környezetben.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-104">Ansible allows you tooautomate hello deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="b7c7d-105">A virtuális gépek (VM) Ansible toomanage használja az Azure-ban, hello ugyanaz, mint bármely egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-105">You can use Ansible toomanage your virtual machines (VMs) in Azure, hello same as you would any other resource.</span></span> <span data-ttu-id="b7c7d-106">Ez a cikk bemutatja, hogyan toocreate teljes körű Linux környezetet és az azt támogató Ansible erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-106">This article shows you how toocreate a complete Linux environment and supporting resources with Ansible.</span></span> <span data-ttu-id="b7c7d-107">Itt olvashat hogyan túl[hozzon létre egy egyszerű virtuális gép Ansible](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="b7c7d-107">You can also learn how too[Create a basic VM with Ansible](ansible-create-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="b7c7d-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b7c7d-108">Prerequisites</span></span>
<span data-ttu-id="b7c7d-109">toomanage Azure Ansible erőforrásokhoz, a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-109">toomanage Azure resources with Ansible, you need hello following:</span></span>

- <span data-ttu-id="b7c7d-110">Ansible és a gazdagép a rendszerre telepített Azure Python SDK modulok hello.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-110">Ansible and hello Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="b7c7d-111">Ansible telepíthető [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), és [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="b7c7d-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="b7c7d-112">Azure hitelesítő adatait, és a beállított Ansible toouse őket.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-112">Azure credentials, and Ansible configured toouse them.</span></span>
    - [<span data-ttu-id="b7c7d-113">Az Azure hitelesítő adatok létrehozása és Ansible konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7c7d-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="b7c7d-114">Az Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b7c7d-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-115">Run `az --version` toofind hello version.</span></span> 
    - <span data-ttu-id="b7c7d-116">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b7c7d-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="b7c7d-117">Is [felhő rendszerhéj](/azure/cloud-shell/quickstart) a böngészőből.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-virtual-network"></a><span data-ttu-id="b7c7d-118">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7c7d-118">Create virtual network</span></span>
<span data-ttu-id="b7c7d-119">hello egy Ansible forgatókönyv a következő szakasz létrehoz egy virtuális hálózatot nevű *myVnet* a hello *10.0.0.0/16* címtér:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-119">hello following section in an Ansible playbook creates a virtual network named *myVnet* in hello *10.0.0.0/16* address space:</span></span>

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

<span data-ttu-id="b7c7d-120">tooadd alhálózat, a következő szakasz hello alhálózatot hoz létre nevű *mySubnet* a hello *myVnet* virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-120">tooadd a subnet, hello following section creates a subnet named *mySubnet* in hello *myVnet* virtual network:</span></span>

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a><span data-ttu-id="b7c7d-121">Nyilvános IP-cím létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7c7d-121">Create public IP address</span></span>
<span data-ttu-id="b7c7d-122">tooaccess erőforrások közötti hello Internet, hozzon létre, és rendelje a nyilvános IP-cím tooyour virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-122">tooaccess resources across hello Internet, create and assign a public IP address tooyour VM.</span></span> <span data-ttu-id="b7c7d-123">hello egy Ansible forgatókönyv a következő szakasz hoz létre egy nyilvános IP-cím nevű *myPublicIP*:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-123">hello following section in an Ansible playbook creates a public IP address named *myPublicIP*:</span></span>

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a><span data-ttu-id="b7c7d-124">Hálózati biztonsági csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7c7d-124">Create Network Security Group</span></span>
<span data-ttu-id="b7c7d-125">Hálózati biztonsági csoportok hello folyamatábrán hálózati forgalom mindkét a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-125">Network Security Groups control hello flow of network traffic in and out of your VM.</span></span> <span data-ttu-id="b7c7d-126">hello egy Ansible forgatókönyv a következő szakasz a hálózati biztonsági csoportot hoz létre nevű *myNetworkSecurityGroup* határozza meg a szabály tooallow SSH forgalom 22-es TCP-porton és:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-126">hello following section in an Ansible playbook creates a network security group named *myNetworkSecurityGroup* and defines a rule tooallow SSH traffic on TCP port 22:</span></span>

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


## <a name="create-virtual-network-interface-card"></a><span data-ttu-id="b7c7d-127">Hozzon létre a virtuális hálózati kártya</span><span class="sxs-lookup"><span data-stu-id="b7c7d-127">Create virtual network interface card</span></span>
<span data-ttu-id="b7c7d-128">A virtuális hálózati kártya (NIC) a virtuális hálózat, a nyilvános IP-cím és a hálózati biztonsági csoport VM tooa csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-128">A virtual network interface card (NIC) connects your VM tooa given virtual network, public IP address, and network security group.</span></span> <span data-ttu-id="b7c7d-129">hello egy Ansible forgatókönyv a következő szakasz hoz létre a virtuális hálózati adapter nevű *myNIC* csatlakozó virtuális hálózati erőforrások toohello hozott létre:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-129">hello following section in an Ansible playbook creates a virtual NIC named *myNIC* connected toohello virtual networking resources you have created:</span></span>

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


## <a name="create-virtual-machine"></a><span data-ttu-id="b7c7d-130">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b7c7d-130">Create virtual machine</span></span>
<span data-ttu-id="b7c7d-131">hello végső lépés toocreate egy virtuális Gépet, és létrehozott összes hello erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-131">hello final step is toocreate a VM and use all hello resources created.</span></span> <span data-ttu-id="b7c7d-132">hello egy Ansible forgatókönyv a következő szakasz hoz létre egy elnevezett VM *myVM* és rendeli hello nevű virtuális hálózati adapter *myNIC*.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-132">hello following section in an Ansible playbook creates a VM named *myVM* and attaches hello virtual NIC named *myNIC*.</span></span> <span data-ttu-id="b7c7d-133">Adja meg a saját nyilvános kulcs adatait hello *key_data* párosítsa a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-133">Enter your own public key data in hello *key_data* pair as follows:</span></span>

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

## <a name="complete-ansible-playbook"></a><span data-ttu-id="b7c7d-134">Teljes Ansible forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="b7c7d-134">Complete Ansible playbook</span></span>
<span data-ttu-id="b7c7d-135">toobring ezekben a szakaszokban együtt, hozzon létre egy Ansible alkalmazástervezési nevű *azure_create_complete_vm.yml* és a Beillesztés hello a következő tartalmát:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-135">toobring all these sections together, create an Ansible playbook named *azure_create_complete_vm.yml* and paste hello following contents:</span></span>

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

<span data-ttu-id="b7c7d-136">Ansible kell egy erőforrás csoport toodeploy az összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-136">Ansible needs a resource group toodeploy all your resources into.</span></span> <span data-ttu-id="b7c7d-137">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-137">Create a resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="b7c7d-138">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-138">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="b7c7d-139">toocreate hello teljes méretű környezet Ansible hello alkalmazástervezési futtassa a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-139">toocreate hello complete VM environment with Ansible, run hello playbook as follows:</span></span>

```bash
ansible-playbook azure_create_complete_vm.yml
```

<span data-ttu-id="b7c7d-140">hello kimeneti a következőhöz hasonló toohello hello bemutatja a virtuális gép létrehozása sikeresen befejeződött a következő:</span><span class="sxs-lookup"><span data-stu-id="b7c7d-140">hello output looks similar toohello following example that shows hello VM has been successfully created:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="b7c7d-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7c7d-141">Next steps</span></span>
<span data-ttu-id="b7c7d-142">Ez a példa többek között a virtuális hálózati erőforrások hello szükséges teljes körű Virtuálisgép-környezetet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b7c7d-142">This example creates a complete VM environment including hello required virtual networking resources.</span></span> <span data-ttu-id="b7c7d-143">A több közvetlen példa toocreate egy virtuális Gépet a meglévő hálózati erőforrások alapértelmezett beállításokkal, tekintse meg [hozzon létre egy virtuális Gépet](ansible-create-vm.md).</span><span class="sxs-lookup"><span data-stu-id="b7c7d-143">For a more direct example toocreate a VM into existing network resources with default options, see [Create a VM](ansible-create-vm.md).</span></span>
