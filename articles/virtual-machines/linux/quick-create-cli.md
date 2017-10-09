---
title: "gyors üzembe helyezés – aaaAzure hozzon létre virtuális gép CLI |} Microsoft Docs"
description: "Ismerje meg gyorsan, toocreate virtuális gépek hello Azure parancssori felület."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0d9c686e2f4c7339b29b8c43cd1dd9ee56d7dc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="bdd2a-103">Hozzon létre egy Linux virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="bdd2a-103">Create a Linux virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="bdd2a-104">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="bdd2a-105">Ez az útmutató adatokat hello Azure CLI toodeploy Ubuntu server operációs rendszert futtató virtuális gépek használata.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-105">This guide details using hello Azure CLI toodeploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="bdd2a-106">Miután hello kiszolgáló van telepítve, az SSH-kapcsolat jön létre, és egy NGINX-webkiszolgálón telepítve van.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-106">Once hello server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="bdd2a-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="bdd2a-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="bdd2a-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="bdd2a-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bdd2a-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="bdd2a-111">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="bdd2a-111">Create a resource group</span></span>

<span data-ttu-id="bdd2a-112">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-112">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="bdd2a-113">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="bdd2a-114">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-114">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="bdd2a-115">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bdd2a-115">Create virtual machine</span></span>

<span data-ttu-id="bdd2a-116">Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-116">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="bdd2a-117">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* és SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-117">hello following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="bdd2a-118">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-118">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="bdd2a-119">Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-119">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="bdd2a-120">Jegyezze fel a hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-120">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="bdd2a-121">Ez a cím használt tooaccess hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-121">This address is used tooaccess hello VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="bdd2a-122">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="bdd2a-122">Open port 80 for web traffic</span></span> 

<span data-ttu-id="bdd2a-123">Alapértelmezés szerint kizárólag SSH-kapcsolatok engedélyezettek az Azure-ban üzembe helyezett, Linux rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-123">By default only SSH connections are allowed into Linux virtual machines deployed in Azure.</span></span> <span data-ttu-id="bdd2a-124">Ha a virtuális gép lesz egy webkiszolgáló toobe, kell tooopen hello Internet a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-124">If this VM is going toobe a webserver, you need tooopen port 80 from hello Internet.</span></span> <span data-ttu-id="bdd2a-125">Használjon hello [az vm-port megnyitása](/cli/azure/vm#open-port) parancs tooopen hello port szükséges.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-125">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="bdd2a-126">Bejelentkezés a virtuális gépre SSH-val</span><span class="sxs-lookup"><span data-stu-id="bdd2a-126">SSH into your VM</span></span>

<span data-ttu-id="bdd2a-127">Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-127">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="bdd2a-128">Győződjön meg arról, hogy tooreplace  *<publicIpAddress>*  hello megfelelő nyilvános IP-címet a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-128">Make sure tooreplace *<publicIpAddress>* with hello correct public IP address of your virtual machine.</span></span>  <span data-ttu-id="bdd2a-129">A fenti példában az IP-cím a következő volt: *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-129">In our example above our IP address was *40.68.254.142*.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a><span data-ttu-id="bdd2a-130">Az NGINX telepítése</span><span class="sxs-lookup"><span data-stu-id="bdd2a-130">Install NGINX</span></span>

<span data-ttu-id="bdd2a-131">Használjon hello alábbi parancsfájl tooupdate csomag források bash, és hello legújabb NGINX csomag telepítése.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-131">Use hello following bash script tooupdate package sources and install hello latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-nginx-welcome-page"></a><span data-ttu-id="bdd2a-132">Hello NGINX üdvözlőlap megtekintése</span><span class="sxs-lookup"><span data-stu-id="bdd2a-132">View hello NGINX welcome page</span></span>

<span data-ttu-id="bdd2a-133">Telepített NGINX és a virtuális gépre, az Internet - hello most nyissa meg a 80-as port az a választott tooview hello alapértelmezett NGINX üdvözlőlap webböngésző is használhatja.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-133">With NGINX installed and port 80 now open on your VM from hello Internet - you can use a web browser of your choice tooview hello default NGINX welcome page.</span></span> <span data-ttu-id="bdd2a-134">Lehet, hogy toouse hello *publicIpAddress* toovisit hello alapértelmezett oldal fent részletes ismertetését lásd.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-134">Be sure toouse hello *publicIpAddress* you documented above toovisit hello default page.</span></span> 

![Alapértelmezett NGINX-webhely](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a><span data-ttu-id="bdd2a-136">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="bdd2a-136">Clean up resources</span></span>

<span data-ttu-id="bdd2a-137">Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-137">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="bdd2a-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bdd2a-138">Next steps</span></span>

<span data-ttu-id="bdd2a-139">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-139">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="bdd2a-140">További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Linux virtuális gépek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="bdd2a-140">toolearn more about Azure virtual machines, continue toohello tutorial for Linux VMs.</span></span>


> [!div class="nextstepaction"]
> [<span data-ttu-id="bdd2a-141">Azure-beli Linux rendszerű virtuális gépek – oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="bdd2a-141">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
