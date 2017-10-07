---
title: "a Linux virtuális gépek az Azure-ban a parancssori felület 2.0 kép aaaCapture |} Microsoft Docs"
description: "Rögzítsen egy rendszerképet az Azure virtuális gép toouse hello Azure CLI 2.0 használatával tömeges telepítésekhez."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9558332a86186b282775097428df462709373012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-image-of-a-virtual-machine-or-vhd"></a><span data-ttu-id="10060-103">Hogyan toocreate egy virtuális géphez vagy virtuális merevlemez képe</span><span class="sxs-lookup"><span data-stu-id="10060-103">How toocreate an image of a virtual machine or VHD</span></span>

<!-- generalize, image - extended version of hello tutorial-->

<span data-ttu-id="10060-104">toocreate több példányát az Azure, a virtuális gép (VM) toouse rögzítsen egy rendszerképet virtuális gép hello, vagy az operációs rendszer virtuális merevlemez hello.</span><span class="sxs-lookup"><span data-stu-id="10060-104">toocreate multiple copies of a virtual machine (VM) toouse in Azure, capture an image of hello VM or hello OS VHD.</span></span> <span data-ttu-id="10060-105">toocreate kép, meg kell, törölje a személyes fiók adatait, így a biztonságosabb toodeploy többször.</span><span class="sxs-lookup"><span data-stu-id="10060-105">toocreate an image, you need remove personal account information which makes it safer toodeploy multiple times.</span></span> <span data-ttu-id="10060-106">Az alábbi lépésekkel hello meg kiosztásának megszüntetése a meglévő virtuális, felszabadítása és lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="10060-106">In hello following steps you deprovision an existing VM, deallocate and create an image.</span></span> <span data-ttu-id="10060-107">A kép toocreate virtuális gépek közötti bármely erőforráscsoport használhatja az előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="10060-107">You can use this image toocreate VMs across any resource group within your subscription.</span></span>

<span data-ttu-id="10060-108">Ha szeretné, hogy a meglévő Linux virtuális gép egy másolatát toocreate biztonsági mentését vagy hibakeresés, vagy egy helyszíni virtuális gép speciális Linux virtuális merevlemez feltöltése, lásd: [feltöltése és a Linux virtuális gép létrehozása az egyéni lemezképet](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="10060-108">If you want toocreate a copy of your existing Linux VM for backup or debugging, or upload a specialized Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md).</span></span>  

<span data-ttu-id="10060-109">Is **csomagoló** toocreate az egyéni konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="10060-109">You can also use **Packer** toocreate your custom configuration.</span></span> <span data-ttu-id="10060-110">Csomagoló használatával kapcsolatos további információkért lásd: [hogyan toouse csomagoló toocreate Linux virtuális gép az Azure-ban lemezképek](build-image-with-packer.md).</span><span class="sxs-lookup"><span data-stu-id="10060-110">For more information on using Packer, see [How toouse Packer toocreate Linux virtual machine images in Azure](build-image-with-packer.md).</span></span>


## <a name="before-you-begin"></a><span data-ttu-id="10060-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="10060-111">Before you begin</span></span>
<span data-ttu-id="10060-112">Győződjön meg arról, hogy teljesülnek-e a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="10060-112">Ensure that you meet hello following prerequisites:</span></span>

* <span data-ttu-id="10060-113">Egy hello Resource Manager üzembe helyezési modellel felügyelt lemezekkel létrehozott Azure virtuális Gépre van szüksége.</span><span class="sxs-lookup"><span data-stu-id="10060-113">You need an Azure VM created in hello Resource Manager deployment model using managed disks.</span></span> <span data-ttu-id="10060-114">Ha még nem hozott létre egy Linux virtuális Gépet, használhatja a hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), vagy [Resource Manager-sablonok](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="10060-114">If you haven't created a Linux VM, you can use hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> <span data-ttu-id="10060-115">Virtuális gép szükség szerint hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="10060-115">Configure hello VM as needed.</span></span> <span data-ttu-id="10060-116">Például [adatok lemezek hozzáadása a](add-disk.md), frissítések és alkalmazások telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="10060-116">For example, [add data disks](add-disk.md), apply updates, and install applications.</span></span> 

* <span data-ttu-id="10060-117">Szükség toohave hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve és tooan Azure-fiókot használó rögzítheti [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="10060-117">You also need toohave hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and be logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="10060-118">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="10060-118">Quick commands</span></span>

<span data-ttu-id="10060-119">Egy egyszerűsített verziója, tesztelési, ez a témakör értékelése és megismerése az virtuális gépeket az Azure-ban, olvassa el [hozzon létre egy Azure virtuális gép hello parancssori felület használatával egyéni lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="10060-119">For a simplified version of this topic, for testing, evaluating or learning about VMs in Azure, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md).</span></span>


## <a name="step-1-deprovision-hello-vm"></a><span data-ttu-id="10060-120">1. lépés: Virtuális gép hello kiosztásának megszüntetése</span><span class="sxs-lookup"><span data-stu-id="10060-120">Step 1: Deprovision hello VM</span></span>
<span data-ttu-id="10060-121">Ön kiosztásának megszüntetése hello virtuális gépet a hello Azure Virtuálisgép-ügynök, toodelete gép meghatározott fájlokat és adatokat.</span><span class="sxs-lookup"><span data-stu-id="10060-121">You deprovision hello VM, using hello Azure VM agent, toodelete machine specific files and data.</span></span> <span data-ttu-id="10060-122">Használjon hello `waagent` hello parancsot *-deprovision + felhasználói* paraméter a Linux virtuális gép – forrásként.</span><span class="sxs-lookup"><span data-stu-id="10060-122">Use hello `waagent` command with hello *-deprovision+user* parameter on your source Linux VM.</span></span> <span data-ttu-id="10060-123">További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](../windows/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="10060-123">For more information, see hello [Azure Linux Agent user guide](../windows/agent-user-guide.md).</span></span>

1. <span data-ttu-id="10060-124">Csatlakozás tooyour Linux virtuális gép SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="10060-124">Connect tooyour Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="10060-125">Hello SSH ablakban írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="10060-125">In hello SSH window, type hello following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > <span data-ttu-id="10060-126">A parancs egy virtuális gépen, hogy szeretné-e képként toocapture csak fut.</span><span class="sxs-lookup"><span data-stu-id="10060-126">Only run this command on a VM that you intend toocapture as an image.</span></span> <span data-ttu-id="10060-127">Hello lemezkép törlődik a bizalmas információk, vagy alkalmas terjesztési nem garantálja.</span><span class="sxs-lookup"><span data-stu-id="10060-127">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution.</span></span> <span data-ttu-id="10060-128">Hello *+ felhasználói* paraméter is eltávolítja a hello utolsó kiépített felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="10060-128">hello *+user* parameter also removes hello last provisioned user account.</span></span> <span data-ttu-id="10060-129">Ha azt szeretné, hogy a virtuális gép hello tookeep fiók hitelesítő adatait, használja *-deprovision* tooleave hello felhasználói fiók helyen.</span><span class="sxs-lookup"><span data-stu-id="10060-129">If you want tookeep account credentials in hello VM, just use *-deprovision* tooleave hello user account in place.</span></span>
 
3. <span data-ttu-id="10060-130">Típus **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="10060-130">Type **y** toocontinue.</span></span> <span data-ttu-id="10060-131">Hozzáadhat hello **-force** paraméter tooavoid a megerősítési lépés.</span><span class="sxs-lookup"><span data-stu-id="10060-131">You can add hello **-force** parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="10060-132">Hello parancs után írja be a **kilépéshez**.</span><span class="sxs-lookup"><span data-stu-id="10060-132">After hello command completes, type **exit**.</span></span> <span data-ttu-id="10060-133">Ez a lépés hello SSH-ügyfél bezárása után.</span><span class="sxs-lookup"><span data-stu-id="10060-133">This step closes hello SSH client.</span></span>

## <a name="step-2-create-vm-image"></a><span data-ttu-id="10060-134">2. lépés: A Virtuálisgép-lemezkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="10060-134">Step 2: Create VM image</span></span>
<span data-ttu-id="10060-135">Általános hello Azure CLI 2.0 toomark hello virtuális gép használja, és hello lemezképének.</span><span class="sxs-lookup"><span data-stu-id="10060-135">Use hello Azure CLI 2.0 toomark hello VM as generalized and capture hello image.</span></span> <span data-ttu-id="10060-136">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="10060-136">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="10060-137">Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="10060-137">Example parameter names include *myResourceGroup*, *myVnet*, and *myVM*.</span></span>

1. <span data-ttu-id="10060-138">Felszabadítani a virtuális Gépet, amely az Ön platformelőfizetés hello [az virtuális gép felszabadítása](/cli//azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="10060-138">Deallocate hello VM that you deprovisioned with [az vm deallocate](/cli//azure/vm#deallocate).</span></span> <span data-ttu-id="10060-139">hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="10060-139">hello following example deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. <span data-ttu-id="10060-140">Megjelölés hello módon rendelkező általánosított virtuális gép [az vm generalize](/cli//azure/vm#generalize).</span><span class="sxs-lookup"><span data-stu-id="10060-140">Mark hello VM as generalized with [az vm generalize](/cli//azure/vm#generalize).</span></span> <span data-ttu-id="10060-141">Példa jelek hello hello VM nevű következő hello *myVM* nevű hello erőforráscsoportban *myResourceGroup* , általánosítva van:</span><span class="sxs-lookup"><span data-stu-id="10060-141">hello following example marks hello hello VM named *myVM* in hello resource group named *myResourceGroup* as generalized:</span></span>
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. <span data-ttu-id="10060-142">Most létrehoz egy képet hello VM erőforrás [az lemezkép létrehozása](/cli//azure/image#create).</span><span class="sxs-lookup"><span data-stu-id="10060-142">Now create an image of hello VM resource with [az image create](/cli//azure/image#create).</span></span> <span data-ttu-id="10060-143">hello alábbi példakód létrehozza nevű kép *myImage* nevű hello erőforráscsoportban *myResourceGroup* nevű VM erőforrás hello segítségével *myVM*:</span><span class="sxs-lookup"><span data-stu-id="10060-143">hello following example creates an image named *myImage* in hello resource group named *myResourceGroup* using hello VM resource named *myVM*:</span></span>
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > <span data-ttu-id="10060-144">hello lemezkép létrehozása hello azonos erőforráscsoporthoz tartozik, mint a forrás virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="10060-144">hello image is created in hello same resource group as your source VM.</span></span> <span data-ttu-id="10060-145">Bármely erőforráscsoport virtuális gépeket hozhat létre a lemezképből az előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="10060-145">You can create VMs in any resource group within your subscription from this image.</span></span> <span data-ttu-id="10060-146">Felügyeleti szempontból Kezdésként toocreate egy adott erőforráscsoportban található a Virtuálisgép-erőforrások és a képeket.</span><span class="sxs-lookup"><span data-stu-id="10060-146">From a management perspective, you may wish toocreate a specific resource group for your VM resources and images.</span></span>

## <a name="step-3-create-a-vm-from-hello-captured-image"></a><span data-ttu-id="10060-147">3. lépés: Virtuális gép létrehozása hello rögzített lemezképből</span><span class="sxs-lookup"><span data-stu-id="10060-147">Step 3: Create a VM from hello captured image</span></span>
<span data-ttu-id="10060-148">Hello lemezkép segítségével létrehozott virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="10060-148">Create a VM using hello image you created with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="10060-149">hello alábbi példakód létrehozza a virtuális gépek nevű *myVMDeployed* nevű hello lemezképből *myImage*:</span><span class="sxs-lookup"><span data-stu-id="10060-149">hello following example creates a VM named *myVMDeployed* from hello image named *myImage*:</span></span>

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-hello-vm-in-another-resource-group"></a><span data-ttu-id="10060-150">Egy másik erőforráscsoportban található hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="10060-150">Creating hello VM in another resource group</span></span> 

<span data-ttu-id="10060-151">Bármely erőforráscsoport lemezkép virtuális gépeket hozhat létre az előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="10060-151">You can create VMs from an image in any resource group within your subscription.</span></span> <span data-ttu-id="10060-152">egy virtuális Gépet egy másik erőforráscsoportban található, mint hello lemezkép toocreate adja meg a hello teljes erőforrás azonosítója tooyour lemezképet.</span><span class="sxs-lookup"><span data-stu-id="10060-152">toocreate a VM in a different resource group than hello image, specify hello full resource ID tooyour image.</span></span> <span data-ttu-id="10060-153">Használjon [az képlistában](/cli/azure/image#list) tooview lemezképek listáját.</span><span class="sxs-lookup"><span data-stu-id="10060-153">Use [az image list](/cli/azure/image#list) tooview a list of images.</span></span> <span data-ttu-id="10060-154">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="10060-154">hello output is similar toohello following example:</span></span>

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

<span data-ttu-id="10060-155">hello alábbi példában [az virtuális gép létrehozása](/cli/azure/vm#create) toocreate egy virtuális Gépet egy másik erőforráscsoportban található, mint hello forrás lemezkép hello képerőforrás-azonosító megadásával:</span><span class="sxs-lookup"><span data-stu-id="10060-155">hello following example uses [az vm create](/cli/azure/vm#create) toocreate a VM in a different resource group than hello source image by specifying hello image resource ID:</span></span>

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-hello-deployment"></a><span data-ttu-id="10060-156">4. lépés: Hello telepítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="10060-156">Step 4: Verify hello deployment</span></span>

<span data-ttu-id="10060-157">Most SSH toohello virtuális gép tooverify hello üzembe helyezési és kezdő használatával létrehozott új virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="10060-157">Now SSH toohello virtual machine you created tooverify hello deployment and start using hello new VM.</span></span> <span data-ttu-id="10060-158">ssh, tooconnect található hello IP-cím vagy FQDN-jét a virtuális Gépet a [az vm megjelenítése](/cli/azure/vm#show):</span><span class="sxs-lookup"><span data-stu-id="10060-158">tooconnect via SSH, find hello IP address or FQDN of your VM with [az vm show](/cli/azure/vm#show):</span></span>

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a><span data-ttu-id="10060-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10060-159">Next steps</span></span>
<span data-ttu-id="10060-160">A forrás Virtuálisgép-lemezkép létrehozhat több virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="10060-160">You can create multiple VMs from your source VM image.</span></span> <span data-ttu-id="10060-161">Ha toomake módosítások tooyour kép lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="10060-161">If you need toomake changes tooyour image:</span></span> 

- <span data-ttu-id="10060-162">Hozzon létre egy virtuális Gépet a lemezképből.</span><span class="sxs-lookup"><span data-stu-id="10060-162">Create a VM from your image.</span></span>
- <span data-ttu-id="10060-163">Győződjön meg a szükséges frissítések vagy a konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="10060-163">Make any updates or configuration changes.</span></span>
- <span data-ttu-id="10060-164">Hajtsa végre a hello lépések újra toodeprovision, felszabadítani, generalize és lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="10060-164">Follow hello steps again toodeprovision, deallocate, generalize, and create an image.</span></span>
- <span data-ttu-id="10060-165">Az új kép használata a jövőben a központi telepítésekre.</span><span class="sxs-lookup"><span data-stu-id="10060-165">Use this new image for future deployments.</span></span> <span data-ttu-id="10060-166">Ha szükséges, hello eredeti lemezkép törlése.</span><span class="sxs-lookup"><span data-stu-id="10060-166">If desired, delete hello original image.</span></span>

<span data-ttu-id="10060-167">A virtuális gépek hello CLI kezeléséről további információkért lásd: [Azure CLI 2.0](/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="10060-167">For more information on managing your VMs with hello CLI, see [Azure CLI 2.0](/cli/azure/overview).</span></span>
