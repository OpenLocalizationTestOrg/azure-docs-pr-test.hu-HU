---
title: "Egyéni Virtuálisgép-lemezképek létrehozása az Azure parancssori felülettel |} Microsoft Docs"
description: "Útmutató – hozzon létre egy egyéni Virtuálisgép-lemezkép az Azure parancssori felület használatával."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: d32980f05ad17a76793021d0a5355d597974a4e4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-the-cli"></a><span data-ttu-id="ed615-103">Hozzon létre egy egyéni rendszerképet az Azure virtuális gép a parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="ed615-103">Create a custom image of an Azure VM using the CLI</span></span>

<span data-ttu-id="ed615-104">Egyéni lemezképek piactéren elérhető rendszerkép hasonló, de Ön hozza létre őket.</span><span class="sxs-lookup"><span data-stu-id="ed615-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="ed615-105">Egyéni lemezképek a rendszerindítási beállításokat, például alkalmazások, alkalmazás, és más operációs rendszer konfigurációjában kerüli használható.</span><span class="sxs-lookup"><span data-stu-id="ed615-105">Custom images can be used to bootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="ed615-106">Ebben az oktatóanyagban létrehoz egy Azure virtuális gép saját egyéni rendszerképét.</span><span class="sxs-lookup"><span data-stu-id="ed615-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="ed615-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="ed615-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed615-108">Kiosztásának megszüntetése és virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="ed615-108">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="ed615-109">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed615-109">Create a custom image</span></span>
> * <span data-ttu-id="ed615-110">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="ed615-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="ed615-111">Az előfizetésben a képek felsorolása</span><span class="sxs-lookup"><span data-stu-id="ed615-111">List all the images in your subscription</span></span>
> * <span data-ttu-id="ed615-112">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="ed615-112">Delete an image</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="ed615-113">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="ed615-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="ed615-114">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="ed615-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="ed615-115">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="ed615-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="ed615-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="ed615-116">Before you begin</span></span>

<span data-ttu-id="ed615-117">Az alábbi lépéseket a meglévő virtuális igénybe vehet, és kapcsolja be, amelyek segítségével hozzon létre új Virtuálisgép-példányok újra felhasználható egyéni lemezképként adatok találhatók.</span><span class="sxs-lookup"><span data-stu-id="ed615-117">The steps below detail how to take an existing VM and turn it into a re-usable custom image that you can use to create new VM instances.</span></span>

<span data-ttu-id="ed615-118">A példa az oktatóanyag elvégzéséhez rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="ed615-118">To complete the example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="ed615-119">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="ed615-119">If needed, this [script sample](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) can create one for you.</span></span> <span data-ttu-id="ed615-120">Az oktatóanyag lépéseinek működő cseréje esetén a az erőforráscsoportot és a virtuális gép nevét, amennyiben szükséges.</span><span class="sxs-lookup"><span data-stu-id="ed615-120">When working through the tutorial, replace the resource group and VM names where needed.</span></span>

## <a name="create-a-custom-image"></a><span data-ttu-id="ed615-121">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed615-121">Create a custom image</span></span>

<span data-ttu-id="ed615-122">Hozzon létre egy virtuális gép lemezképét, meg kell készíteni a virtuális gép megszüntetés, felszabadítása, és majd a forrás virtuális Gépen, mivel általánosítva.</span><span class="sxs-lookup"><span data-stu-id="ed615-122">To create an image of a virtual machine, you need to prepare the VM by deprovisioning, deallocating, and then marking the source VM as generalized.</span></span> <span data-ttu-id="ed615-123">Miután a virtuális gép előkészített, kép hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="ed615-123">Once the VM has been prepared, you can create an image.</span></span>

### <a name="deprovision-the-vm"></a><span data-ttu-id="ed615-124">A virtuális gép kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="ed615-124">Deprovision the VM</span></span> 

<span data-ttu-id="ed615-125">Megszüntetés használatúvá a virtuális gép-specifikus adatok eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="ed615-125">Deprovisioning generalizes the VM by removing machine-specific information.</span></span> <span data-ttu-id="ed615-126">Ez általánosítása lehetővé teszi egyetlen lemezképéről sok virtuális gép központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ed615-126">This generalization makes it possible to deploy many VMs from a single image.</span></span> <span data-ttu-id="ed615-127">Megszüntetés, során az állomás neve lesz visszaállítva *localhost.localdomain*.</span><span class="sxs-lookup"><span data-stu-id="ed615-127">During deprovisioning, the host name is reset to *localhost.localdomain*.</span></span> <span data-ttu-id="ed615-128">SSH-állomások kulcsait, a névkiszolgáló-konfigurációk, a gyökér szintű jelszó és a gyorsítótárazott DHCP-bérletek is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="ed615-128">SSH host keys, nameserver configurations, root password, and cached DHCP leases are also deleted.</span></span>

<span data-ttu-id="ed615-129">A virtuális gép kiosztásának megszüntetése, használja az Azure Virtuálisgép-ügynök (waagent).</span><span class="sxs-lookup"><span data-stu-id="ed615-129">To deprovision the VM, use the Azure VM agent (waagent).</span></span> <span data-ttu-id="ed615-130">Az Azure Virtuálisgép-ügynök telepítve van a virtuális Gépre, és kezeli az üzembe helyezési és az Azure Fabric Controller való interakció.</span><span class="sxs-lookup"><span data-stu-id="ed615-130">The Azure VM agent is installed on the VM and manages provisioning and interacting with the Azure Fabric Controller.</span></span> <span data-ttu-id="ed615-131">További információkért lásd: a [Azure Linux ügynök felhasználói útmutató](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="ed615-131">For more information, see the [Azure Linux Agent user guide](agent-user-guide.md).</span></span>

<span data-ttu-id="ed615-132">A virtuális gép SSH használatával csatlakozhat, és futtassa a parancsot a virtuális gép kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="ed615-132">Connect to your VM using SSH and run the command to deprovision the VM.</span></span> <span data-ttu-id="ed615-133">Az a `+user` argumentum, a legutóbbi kiépített felhasználói fiók és minden egyéb vonatkozó adatok is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="ed615-133">With the `+user` argument, the last provisioned user account and any associated data are also deleted.</span></span> <span data-ttu-id="ed615-134">A példa IP-cím cserélje le a virtuális gép nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ed615-134">Replace the example IP address with the public IP address of your VM.</span></span>

<span data-ttu-id="ed615-135">SSH-kapcsolatot a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="ed615-135">SSH to the VM.</span></span>
```bash
ssh azureuser@52.174.34.95
```
<span data-ttu-id="ed615-136">A virtuális gép kiosztásának megszüntetése.</span><span class="sxs-lookup"><span data-stu-id="ed615-136">Deprovision the VM.</span></span>

```bash
sudo waagent -deprovision+user -force
```
<span data-ttu-id="ed615-137">Zárja be az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="ed615-137">Close the SSH session.</span></span>

```bash
exit
```

### <a name="deallocate-and-mark-the-vm-as-generalized"></a><span data-ttu-id="ed615-138">Felszabadítani, és a virtuális gép megjelölése általánosítva</span><span class="sxs-lookup"><span data-stu-id="ed615-138">Deallocate and mark the VM as generalized</span></span>

<span data-ttu-id="ed615-139">A képfájl létrehozásához a virtuális gép felszabadítása kell.</span><span class="sxs-lookup"><span data-stu-id="ed615-139">To create an image, the VM needs to be deallocated.</span></span> <span data-ttu-id="ed615-140">A virtuális gép használatával felszabadítani [az virtuális gép felszabadítása](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="ed615-140">Deallocate the VM using [az vm deallocate](/cli//azure/vm#deallocate).</span></span> 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="ed615-141">Végezetül beállítani a virtuális gép állapotát a általánosítva [az vm generalize](/cli//azure/vm#generalize) , az Azure platformon tudja, hogy a virtuális gép már általánosítva lett.</span><span class="sxs-lookup"><span data-stu-id="ed615-141">Finally, set the state of the VM as generalized with [az vm generalize](/cli//azure/vm#generalize) so the Azure platform knows the VM has been generalized.</span></span> <span data-ttu-id="ed615-142">Kép csak létrehozhat egy általánosított virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="ed615-142">You can only create an image from a generalized VM.</span></span>
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-the-image"></a><span data-ttu-id="ed615-143">A lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed615-143">Create the image</span></span>

<span data-ttu-id="ed615-144">Most a virtuális gép lemezképét segítségével létrehozható [az lemezkép létrehozása](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="ed615-144">Now you can create an image of the VM by using [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="ed615-145">Az alábbi példakód létrehozza nevű kép *myImage* nevű VM *myVM*.</span><span class="sxs-lookup"><span data-stu-id="ed615-145">The following example creates an image named *myImage* from a VM named *myVM*.</span></span>
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-the-image"></a><span data-ttu-id="ed615-146">Virtuális gépek létrehozása lemezkép alapján</span><span class="sxs-lookup"><span data-stu-id="ed615-146">Create VMs from the image</span></span>

<span data-ttu-id="ed615-147">Most, hogy egy lemezképet, létrehozhat egy vagy több új virtuális gépek a lemezkép használatával [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="ed615-147">Now that you have an image, you can create one or more new VMs from the image using [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="ed615-148">Az alábbi példakód létrehozza a virtuális gépek nevű *myVMfromImage* nevű lemezkép alapján *myImage*.</span><span class="sxs-lookup"><span data-stu-id="ed615-148">The following example creates a VM named *myVMfromImage* from the image named *myImage*.</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a><span data-ttu-id="ed615-149">Lemezkép-kezelési</span><span class="sxs-lookup"><span data-stu-id="ed615-149">Image management</span></span> 

<span data-ttu-id="ed615-150">Az alábbiakban néhány olyan gyakori lemezkép-kezelési feladatok és, hogyan lehet elvégezni őket az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="ed615-150">Here are some examples of common image management tasks and how to complete them using the Azure CLI.</span></span>

<span data-ttu-id="ed615-151">Lista összes lemezkép nevű táblázatos formátumban.</span><span class="sxs-lookup"><span data-stu-id="ed615-151">List all images by name in a table format.</span></span>

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

<span data-ttu-id="ed615-152">Lemezkép törlése.</span><span class="sxs-lookup"><span data-stu-id="ed615-152">Delete an image.</span></span> <span data-ttu-id="ed615-153">Ebben a példában a nevű rendszerkép törlése *myOldImage* a a *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="ed615-153">This example deletes the image named *myOldImage* from the *myResourceGroup*.</span></span>

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="ed615-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ed615-154">Next steps</span></span>

<span data-ttu-id="ed615-155">Ebben az oktatóanyagban létre egyéni Virtuálisgép-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="ed615-155">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="ed615-156">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="ed615-156">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ed615-157">Kiosztásának megszüntetése és virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="ed615-157">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="ed615-158">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="ed615-158">Create a custom image</span></span>
> * <span data-ttu-id="ed615-159">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="ed615-159">Create a VM from a custom image</span></span>
> * <span data-ttu-id="ed615-160">Az előfizetésben a képek felsorolása</span><span class="sxs-lookup"><span data-stu-id="ed615-160">List all the images in your subscription</span></span>
> * <span data-ttu-id="ed615-161">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="ed615-161">Delete an image</span></span>

<span data-ttu-id="ed615-162">További információt a magas rendelkezésre állású virtuális gépek a következő oktatóanyag továbblépés.</span><span class="sxs-lookup"><span data-stu-id="ed615-162">Advance to the next tutorial to learn about highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="ed615-163">[Hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="ed615-163">[Create highly available VMs](tutorial-availability-sets.md).</span></span>

