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
# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a><span data-ttu-id="580ff-103">Alapszintű virtuális gép létrehozása az Azure-ban Ansible</span><span class="sxs-lookup"><span data-stu-id="580ff-103">Create a basic virtual machine in Azure with Ansible</span></span>
<span data-ttu-id="580ff-104">Ansible lehetővé teszi a erőforrások tooautomate hello telepítését és konfigurálását a környezetben.</span><span class="sxs-lookup"><span data-stu-id="580ff-104">Ansible allows you tooautomate hello deployment and configuration of resources in your environment.</span></span> <span data-ttu-id="580ff-105">A virtuális gépek (VM) Ansible toomanage használja az Azure-ban, hello ugyanaz, mint bármely egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="580ff-105">You can use Ansible toomanage your virtual machines (VMs) in Azure, hello same as you would any other resource.</span></span> <span data-ttu-id="580ff-106">Ez a cikk bemutatja, hogyan toocreate Ansible rendelkező alapszintű virtuális.</span><span class="sxs-lookup"><span data-stu-id="580ff-106">This article shows you how toocreate a basic VM with Ansible.</span></span> <span data-ttu-id="580ff-107">Itt olvashat hogyan túl[hozzon létre egy teljes körű Virtuálisgép-környezetet Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="580ff-107">You can also learn how too[Create a complete VM environment with Ansible](ansible-create-complete-vm.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="580ff-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="580ff-108">Prerequisites</span></span>
<span data-ttu-id="580ff-109">toomanage Azure Ansible erőforrásokhoz, a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="580ff-109">toomanage Azure resources with Ansible, you need hello following:</span></span>

- <span data-ttu-id="580ff-110">Ansible és a gazdagép a rendszerre telepített Azure Python SDK modulok hello.</span><span class="sxs-lookup"><span data-stu-id="580ff-110">Ansible and hello Azure Python SDK modules installed on your host system.</span></span>
    - <span data-ttu-id="580ff-111">Ansible telepíthető [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), és [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span><span class="sxs-lookup"><span data-stu-id="580ff-111">Install Ansible on [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73), and [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)</span></span>
- <span data-ttu-id="580ff-112">Azure hitelesítő adatait, és a beállított Ansible toouse őket.</span><span class="sxs-lookup"><span data-stu-id="580ff-112">Azure credentials, and Ansible configured toouse them.</span></span>
    - [<span data-ttu-id="580ff-113">Az Azure hitelesítő adatok létrehozása és Ansible konfigurálása</span><span class="sxs-lookup"><span data-stu-id="580ff-113">Create Azure credentials and configure Ansible</span></span>](ansible-install-configure.md#create-azure-credentials)
- <span data-ttu-id="580ff-114">Az Azure CLI 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="580ff-114">Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="580ff-115">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="580ff-115">Run `az --version` toofind hello version.</span></span> 
    - <span data-ttu-id="580ff-116">Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="580ff-116">If you need tooupgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> <span data-ttu-id="580ff-117">Is [felhő rendszerhéj](/azure/cloud-shell/quickstart) a böngészőből.</span><span class="sxs-lookup"><span data-stu-id="580ff-117">You can also use [Cloud Shell](/azure/cloud-shell/quickstart) from your browser.</span></span>


## <a name="create-supporting-azure-resources"></a><span data-ttu-id="580ff-118">Hozzon létre az Azure-erőforrások támogatása</span><span class="sxs-lookup"><span data-stu-id="580ff-118">Create supporting Azure resources</span></span>
<span data-ttu-id="580ff-119">Ebben a példában létrehozhatunk egy runbookot, amely egy virtuális Gépet telepít egy meglévő infrastruktúra be.</span><span class="sxs-lookup"><span data-stu-id="580ff-119">In this example, we create a runbook that deploys a VM into an existing infrastructure.</span></span> <span data-ttu-id="580ff-120">Először hozza létre az erőforráscsoport [az csoport létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="580ff-120">First, create resource group with [az group create](/cli/azure/vm#create).</span></span> <span data-ttu-id="580ff-121">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="580ff-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="580ff-122">Hozzon létre egy virtuális hálózatot a virtuális gép a [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="580ff-122">Create a virtual network for your VM with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="580ff-123">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnet*:</span><span class="sxs-lookup"><span data-stu-id="580ff-123">hello following example creates a virtual network named *myVnet* and a subnet named *mySubnet*:</span></span>

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a><span data-ttu-id="580ff-124">Hozzon létre és futtasson Ansible forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="580ff-124">Create and run Ansible playbook</span></span>
<span data-ttu-id="580ff-125">Hozzon létre egy Ansible alkalmazástervezési nevű **azure_create_vm.yml** és a Beillesztés hello követő tartalmát.</span><span class="sxs-lookup"><span data-stu-id="580ff-125">Create an Ansible playbook named **azure_create_vm.yml** and paste hello following contents.</span></span> <span data-ttu-id="580ff-126">Ez a példa hoz létre egy virtuális, és konfigurálja az SSH hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="580ff-126">This example creates a single VM and configures SSH credentials.</span></span> <span data-ttu-id="580ff-127">Adja meg a saját nyilvános kulcs adatait hello *key_data* párosítsa a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="580ff-127">Enter your own public key data in hello *key_data* pair as follows:</span></span>

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

<span data-ttu-id="580ff-128">toocreate hello Ansible, futtassa a következőképpen hello alkalmazástervezési rendelkező virtuális gépet:</span><span class="sxs-lookup"><span data-stu-id="580ff-128">toocreate hello VM with Ansible, run hello playbook as follows:</span></span>

```bash
ansible-playbook azure_create_vm.yml
```

<span data-ttu-id="580ff-129">hello kimeneti a következőhöz hasonló toohello hello bemutatja a virtuális gép létrehozása sikeresen befejeződött a következő:</span><span class="sxs-lookup"><span data-stu-id="580ff-129">hello output looks similar toohello following example that shows hello VM has been successfully created:</span></span>

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a><span data-ttu-id="580ff-130">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="580ff-130">Next steps</span></span>
<span data-ttu-id="580ff-131">Ebben a példában egy virtuális Gépet hoz létre, egy meglévő erőforráscsoportot és egy virtuális hálózattal, már üzembe van helyezve.</span><span class="sxs-lookup"><span data-stu-id="580ff-131">This example creates a VM in an existing resource group and with a virtual network already deployed.</span></span> <span data-ttu-id="580ff-132">Hogyan részletesebb például toouse Ansible toocreate támogató erőforrások, például a virtuális hálózat és a hálózati biztonsági csoportszabályok, lásd: [hozzon létre egy teljes körű Virtuálisgép-környezetet Ansible](ansible-create-complete-vm.md).</span><span class="sxs-lookup"><span data-stu-id="580ff-132">For a more detailed example on how toouse Ansible toocreate supporting resources such as a virtual network and Network Security Group rules, see [Create a complete VM environment with Ansible](ansible-create-complete-vm.md).</span></span>
