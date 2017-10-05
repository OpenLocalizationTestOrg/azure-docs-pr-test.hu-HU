---
title: "Azure gyors üzembe helyezés – Windows virtuális gép létrehozása a parancssori felületen | Microsoft Docs"
description: "Gyorsan megismerheti a virtuális gépek Azure CLI-vel való létrehozásának módját."
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
ms.openlocfilehash: a7cba5b2c43704d92e36d6f808efaa9fc73fdf36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="d6b44-103">Linux virtuális gép létrehozása az Azure CLI-vel</span><span class="sxs-lookup"><span data-stu-id="d6b44-103">Create a Linux virtual machine with the Azure CLI</span></span>

<span data-ttu-id="d6b44-104">Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="d6b44-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="d6b44-105">Ez az útmutató részletesen bemutatja, hogyan lehet egy Ubuntu Servert futtató virtuális gépet üzembe helyezni az Azure CLI-vel.</span><span class="sxs-lookup"><span data-stu-id="d6b44-105">This guide details using the Azure CLI to deploy a virtual machine running Ubuntu server.</span></span> <span data-ttu-id="d6b44-106">A kiszolgáló üzembe helyezése után a rendszer létrehoz egy SSH-kapcsolatot, és telepít egy NGINX-webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="d6b44-106">Once the server is deployed, an SSH connection is created, and an NGINX webserver is installed.</span></span>

<span data-ttu-id="d6b44-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="d6b44-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="d6b44-108">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a gyorsútmutatóhoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="d6b44-108">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="d6b44-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d6b44-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="d6b44-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d6b44-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="d6b44-111">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d6b44-111">Create a resource group</span></span>

<span data-ttu-id="d6b44-112">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="d6b44-112">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d6b44-113">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d6b44-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="d6b44-114">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot az *eastus* helyen.</span><span class="sxs-lookup"><span data-stu-id="d6b44-114">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="d6b44-115">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6b44-115">Create virtual machine</span></span>

<span data-ttu-id="d6b44-116">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="d6b44-116">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="d6b44-117">Az alábbi példa egy *myVM* nevű virtuális gépet és SSH-kulcsokat hoz létre, ha azok még nem léteznek a kulcsok alapméretezett helyén.</span><span class="sxs-lookup"><span data-stu-id="d6b44-117">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="d6b44-118">Ha konkrét kulcsokat szeretné használni, használja az `--ssh-key-value` beállítást.</span><span class="sxs-lookup"><span data-stu-id="d6b44-118">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="d6b44-119">A virtuális gép létrehozása után az Azure CLI az alábbi példához hasonló információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="d6b44-119">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="d6b44-120">Jegyezze fel a `publicIpAddress` értékét.</span><span class="sxs-lookup"><span data-stu-id="d6b44-120">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="d6b44-121">Ez a cím használható a virtuális gép eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d6b44-121">This address is used to access the VM.</span></span>

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

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="d6b44-122">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="d6b44-122">Open port 80 for web traffic</span></span> 

<span data-ttu-id="d6b44-123">Alapértelmezés szerint kizárólag SSH-kapcsolatok engedélyezettek az Azure-ban üzembe helyezett, Linux rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="d6b44-123">By default only SSH connections are allowed into Linux virtual machines deployed in Azure.</span></span> <span data-ttu-id="d6b44-124">Ha ez a virtuális gép webkiszolgáló lesz, meg kell nyitnia a 80-as portot az internet irányából.</span><span class="sxs-lookup"><span data-stu-id="d6b44-124">If this VM is going to be a webserver, you need to open port 80 from the Internet.</span></span> <span data-ttu-id="d6b44-125">A kívánt port megnyitásához használja az [az vm open-port](/cli/azure/vm#open-port) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d6b44-125">Use the [az vm open-port](/cli/azure/vm#open-port) command to open the desired port.</span></span>  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="d6b44-126">Bejelentkezés a virtuális gépre SSH-val</span><span class="sxs-lookup"><span data-stu-id="d6b44-126">SSH into your VM</span></span>

<span data-ttu-id="d6b44-127">Használja az alábbi parancsot egy SSH-munkamenet létrehozásához a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="d6b44-127">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="d6b44-128">Cserélje le a *<publicIpAddress>* helyőrzőt a virtuális gépe tényleges nyilvános IP-címére.</span><span class="sxs-lookup"><span data-stu-id="d6b44-128">Make sure to replace *<publicIpAddress>* with the correct public IP address of your virtual machine.</span></span>  <span data-ttu-id="d6b44-129">A fenti példában az IP-cím a következő volt: *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="d6b44-129">In our example above our IP address was *40.68.254.142*.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a><span data-ttu-id="d6b44-130">Az NGINX telepítése</span><span class="sxs-lookup"><span data-stu-id="d6b44-130">Install NGINX</span></span>

<span data-ttu-id="d6b44-131">Az alábbi bash-parancsfájl segítségével frissítse a csomagforrásokat, és telepítse a legújabb NGINX-csomagot.</span><span class="sxs-lookup"><span data-stu-id="d6b44-131">Use the following bash script to update package sources and install the latest NGINX package.</span></span> 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-nginx-welcome-page"></a><span data-ttu-id="d6b44-132">Az NGINX kezdőlapjának megtekintése</span><span class="sxs-lookup"><span data-stu-id="d6b44-132">View the NGINX welcome page</span></span>

<span data-ttu-id="d6b44-133">Most, hogy az NGINX telepítve van, és a 80-as port meg van nyitva a virtuális gépén az internet irányából, tetszőleges böngészőt használhat az alapértelmezett NGINX-kezdőlap megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="d6b44-133">With NGINX installed and port 80 now open on your VM from the Internet - you can use a web browser of your choice to view the default NGINX welcome page.</span></span> <span data-ttu-id="d6b44-134">Ügyeljen arra, hogy az alapértelmezett oldalt a fentebb dokumentált *publicIPAddress* használatával keresse fel.</span><span class="sxs-lookup"><span data-stu-id="d6b44-134">Be sure to use the *publicIpAddress* you documented above to visit the default page.</span></span> 

![Alapértelmezett NGINX-webhely](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a><span data-ttu-id="d6b44-136">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d6b44-136">Clean up resources</span></span>

<span data-ttu-id="d6b44-137">Ha már nincs rá szükség, a [az group delete](/cli/azure/group#delete) paranccsal eltávolítható az erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d6b44-137">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="d6b44-138">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d6b44-138">Next steps</span></span>

<span data-ttu-id="d6b44-139">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="d6b44-139">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="d6b44-140">Ha bővebb információra van szüksége az Azure-beli virtuális gépekkel kapcsolatban, lépjen tovább a Linux rendszerű virtuális gépekről szóló oktatóanyagra.</span><span class="sxs-lookup"><span data-stu-id="d6b44-140">To learn more about Azure virtual machines, continue to the tutorial for Linux VMs.</span></span>


> [!div class="nextstepaction"]
> [<span data-ttu-id="d6b44-141">Azure-beli Linux rendszerű virtuális gépek – oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="d6b44-141">Azure Linux virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
