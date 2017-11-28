---
title: "egyéni VM képeket aaaCreate hello Azure parancssori Felülettel |} Microsoft Docs"
description: "Útmutató – hozzon létre egy egyéni Virtuálisgép-lemezkép hello Azure parancssori felület használatával."
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
ms.openlocfilehash: 217a993c0c1d48939b74108ac6c5f7a1a619416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-hello-cli"></a><span data-ttu-id="624cd-103">Hozzon létre egy Azure virtuális gép hello parancssori felület használatával egyéni képe</span><span class="sxs-lookup"><span data-stu-id="624cd-103">Create a custom image of an Azure VM using hello CLI</span></span>

<span data-ttu-id="624cd-104">Egyéni lemezképek piactéren elérhető rendszerkép hasonló, de Ön hozza létre őket.</span><span class="sxs-lookup"><span data-stu-id="624cd-104">Custom images are like marketplace images, but you create them yourself.</span></span> <span data-ttu-id="624cd-105">Egyéni lemezképek lehet például az alkalmazások, alkalmazás, és más operációs rendszer konfigurációjában kerüli használt toobootstrap konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="624cd-105">Custom images can be used toobootstrap configurations such as preloading applications, application configurations, and other OS configurations.</span></span> <span data-ttu-id="624cd-106">Ebben az oktatóanyagban létrehoz egy Azure virtuális gép saját egyéni rendszerképét.</span><span class="sxs-lookup"><span data-stu-id="624cd-106">In this tutorial, you create your own custom image of an Azure virtual machine.</span></span> <span data-ttu-id="624cd-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="624cd-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="624cd-108">Kiosztásának megszüntetése és virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="624cd-108">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="624cd-109">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="624cd-109">Create a custom image</span></span>
> * <span data-ttu-id="624cd-110">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="624cd-110">Create a VM from a custom image</span></span>
> * <span data-ttu-id="624cd-111">Az előfizetésében szereplő összes hello lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="624cd-111">List all hello images in your subscription</span></span>
> * <span data-ttu-id="624cd-112">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="624cd-112">Delete an image</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="624cd-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="624cd-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="624cd-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="624cd-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="624cd-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="624cd-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="624cd-116">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="624cd-116">Before you begin</span></span>

<span data-ttu-id="624cd-117">hello lépéseket részletesen tootake egy meglévő virtuális Gépre, és azt újra felhasználható egyéni lemezképet, hogy kapcsolja használatát toocreate új Virtuálisgép-példányok.</span><span class="sxs-lookup"><span data-stu-id="624cd-117">hello steps below detail how tootake an existing VM and turn it into a re-usable custom image that you can use toocreate new VM instances.</span></span>

<span data-ttu-id="624cd-118">Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="624cd-118">toocomplete hello example in this tutorial, you must have an existing virtual machine.</span></span> <span data-ttu-id="624cd-119">Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="624cd-119">If needed, this [script sample](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) can create one for you.</span></span> <span data-ttu-id="624cd-120">Ha feldolgozása révén hello oktatóanyag, a csere hello erőforráscsoport és a virtuális gép nevét, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="624cd-120">When working through hello tutorial, replace hello resource group and VM names where needed.</span></span>

## <a name="create-a-custom-image"></a><span data-ttu-id="624cd-121">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="624cd-121">Create a custom image</span></span>

<span data-ttu-id="624cd-122">toocreate egy virtuális gép lemezképét, meg kell tooprepare hello VM megszüntetés, felszabadítása és hello forrás virtuális gép általánosítva van, majd jelölést.</span><span class="sxs-lookup"><span data-stu-id="624cd-122">toocreate an image of a virtual machine, you need tooprepare hello VM by deprovisioning, deallocating, and then marking hello source VM as generalized.</span></span> <span data-ttu-id="624cd-123">Egyszer hello a virtuális gép előkészített, hozhat létre kép.</span><span class="sxs-lookup"><span data-stu-id="624cd-123">Once hello VM has been prepared, you can create an image.</span></span>

### <a name="deprovision-hello-vm"></a><span data-ttu-id="624cd-124">Virtuális gép hello kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="624cd-124">Deprovision hello VM</span></span> 

<span data-ttu-id="624cd-125">Megszüntetés használatúvá hello VM számítógép-specifikus adatok eltávolításával.</span><span class="sxs-lookup"><span data-stu-id="624cd-125">Deprovisioning generalizes hello VM by removing machine-specific information.</span></span> <span data-ttu-id="624cd-126">Ez általánosítása révén lehetséges toodeploy sok virtuális gép egyetlen lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="624cd-126">This generalization makes it possible toodeploy many VMs from a single image.</span></span> <span data-ttu-id="624cd-127">Megszüntetés, során hello állomásnév alaphelyzetbe áll túl*localhost.localdomain*.</span><span class="sxs-lookup"><span data-stu-id="624cd-127">During deprovisioning, hello host name is reset too*localhost.localdomain*.</span></span> <span data-ttu-id="624cd-128">SSH-állomások kulcsait, a névkiszolgáló-konfigurációk, a gyökér szintű jelszó és a gyorsítótárazott DHCP-bérletek is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="624cd-128">SSH host keys, nameserver configurations, root password, and cached DHCP leases are also deleted.</span></span>

<span data-ttu-id="624cd-129">virtuális gép, toodeprovision hello hello Azure Virtuálisgép-ügynök (waagent) használja.</span><span class="sxs-lookup"><span data-stu-id="624cd-129">toodeprovision hello VM, use hello Azure VM agent (waagent).</span></span> <span data-ttu-id="624cd-130">hello Azure Virtuálisgép-ügynök hello virtuális gép telepítve van, és kezeli az üzembe helyezési és hello Azure Fabric Controller való interakció.</span><span class="sxs-lookup"><span data-stu-id="624cd-130">hello Azure VM agent is installed on hello VM and manages provisioning and interacting with hello Azure Fabric Controller.</span></span> <span data-ttu-id="624cd-131">További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="624cd-131">For more information, see hello [Azure Linux Agent user guide](agent-user-guide.md).</span></span>

<span data-ttu-id="624cd-132">Csatlakozás a virtuális gép tooyour SSH és futtatási hello parancs toodeprovision hello VM használatával.</span><span class="sxs-lookup"><span data-stu-id="624cd-132">Connect tooyour VM using SSH and run hello command toodeprovision hello VM.</span></span> <span data-ttu-id="624cd-133">A hello `+user` argumentum, hello utolsó kiépített felhasználói fiók és minden társított adatok is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="624cd-133">With hello `+user` argument, hello last provisioned user account and any associated data are also deleted.</span></span> <span data-ttu-id="624cd-134">Hello példa IP-cím cserélje le a virtuális gép hello nyilvános IP-címe.</span><span class="sxs-lookup"><span data-stu-id="624cd-134">Replace hello example IP address with hello public IP address of your VM.</span></span>

<span data-ttu-id="624cd-135">SSH toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="624cd-135">SSH toohello VM.</span></span>
```bash
ssh azureuser@52.174.34.95
```
<span data-ttu-id="624cd-136">Deprovision hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="624cd-136">Deprovision hello VM.</span></span>

```bash
sudo waagent -deprovision+user -force
```
<span data-ttu-id="624cd-137">Zárja be a hello SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="624cd-137">Close hello SSH session.</span></span>

```bash
exit
```

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a><span data-ttu-id="624cd-138">Felszabadítani, és jelölje be a virtuális gép általánosítva, hello</span><span class="sxs-lookup"><span data-stu-id="624cd-138">Deallocate and mark hello VM as generalized</span></span>

<span data-ttu-id="624cd-139">toocreate kép, hello virtuális gép felszabadítása toobe kell.</span><span class="sxs-lookup"><span data-stu-id="624cd-139">toocreate an image, hello VM needs toobe deallocated.</span></span> <span data-ttu-id="624cd-140">Hello VM használatával felszabadítani [az virtuális gép felszabadítása](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="624cd-140">Deallocate hello VM using [az vm deallocate](/cli//azure/vm#deallocate).</span></span> 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="624cd-141">Végezetül beállítani hello VM állapotának hello rendelkező általánosított [az vm generalize](/cli//azure/vm#generalize) , hello Azure platformon tudja hello VM már általánosítva lett.</span><span class="sxs-lookup"><span data-stu-id="624cd-141">Finally, set hello state of hello VM as generalized with [az vm generalize](/cli//azure/vm#generalize) so hello Azure platform knows hello VM has been generalized.</span></span> <span data-ttu-id="624cd-142">Kép csak létrehozhat egy általánosított virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="624cd-142">You can only create an image from a generalized VM.</span></span>
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-hello-image"></a><span data-ttu-id="624cd-143">Hello lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="624cd-143">Create hello image</span></span>

<span data-ttu-id="624cd-144">Most használatával hozhat létre virtuális gép hello képe [az lemezkép létrehozása](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="624cd-144">Now you can create an image of hello VM by using [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="624cd-145">hello alábbi példakód létrehozza nevű kép *myImage* nevű VM *myVM*.</span><span class="sxs-lookup"><span data-stu-id="624cd-145">hello following example creates an image named *myImage* from a VM named *myVM*.</span></span>
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-hello-image"></a><span data-ttu-id="624cd-146">Hozzon létre a virtuális gépek hello lemezképből</span><span class="sxs-lookup"><span data-stu-id="624cd-146">Create VMs from hello image</span></span>

<span data-ttu-id="624cd-147">Most, hogy egy lemezképet, létrehozhat egy vagy több új virtuális gépek hello lemezkép használatával [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="624cd-147">Now that you have an image, you can create one or more new VMs from hello image using [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="624cd-148">hello alábbi példakód létrehozza a virtuális gépek nevű *myVMfromImage* nevű hello lemezképből *myImage*.</span><span class="sxs-lookup"><span data-stu-id="624cd-148">hello following example creates a VM named *myVMfromImage* from hello image named *myImage*.</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a><span data-ttu-id="624cd-149">Lemezkép-kezelési</span><span class="sxs-lookup"><span data-stu-id="624cd-149">Image management</span></span> 

<span data-ttu-id="624cd-150">Az alábbiakban néhány olyan gyakori lemezkép-kezelési feladatok és hogyan toocomplete őket hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="624cd-150">Here are some examples of common image management tasks and how toocomplete them using hello Azure CLI.</span></span>

<span data-ttu-id="624cd-151">Lista összes lemezkép nevű táblázatos formátumban.</span><span class="sxs-lookup"><span data-stu-id="624cd-151">List all images by name in a table format.</span></span>

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

<span data-ttu-id="624cd-152">Lemezkép törlése.</span><span class="sxs-lookup"><span data-stu-id="624cd-152">Delete an image.</span></span> <span data-ttu-id="624cd-153">Ez a példa törlések hello nevű kép *myOldImage* a hello *myResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="624cd-153">This example deletes hello image named *myOldImage* from hello *myResourceGroup*.</span></span>

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="624cd-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="624cd-154">Next steps</span></span>

<span data-ttu-id="624cd-155">Ebben az oktatóanyagban létre egyéni Virtuálisgép-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="624cd-155">In this tutorial, you created a custom VM image.</span></span> <span data-ttu-id="624cd-156">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="624cd-156">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="624cd-157">Kiosztásának megszüntetése és virtuális gépek generalize</span><span class="sxs-lookup"><span data-stu-id="624cd-157">Deprovision and generalize VMs</span></span>
> * <span data-ttu-id="624cd-158">Egyéni lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="624cd-158">Create a custom image</span></span>
> * <span data-ttu-id="624cd-159">Virtuális gép létrehozása egy egyéni lemezképből</span><span class="sxs-lookup"><span data-stu-id="624cd-159">Create a VM from a custom image</span></span>
> * <span data-ttu-id="624cd-160">Az előfizetésében szereplő összes hello lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="624cd-160">List all hello images in your subscription</span></span>
> * <span data-ttu-id="624cd-161">Lemezkép törlése</span><span class="sxs-lookup"><span data-stu-id="624cd-161">Delete an image</span></span>

<span data-ttu-id="624cd-162">Előzetes toohello oktatóanyag következő toolearn kapcsolatos magas rendelkezésre állású virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="624cd-162">Advance toohello next tutorial toolearn about highly available virtual machines.</span></span>

> [!div class="nextstepaction"]
> <span data-ttu-id="624cd-163">[Hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="624cd-163">[Create highly available VMs](tutorial-availability-sets.md).</span></span>

