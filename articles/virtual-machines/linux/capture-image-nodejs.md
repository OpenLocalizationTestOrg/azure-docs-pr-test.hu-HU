---
title: "egy Azure Linux virtuális gép toouse sablonként aaaCapture |} Microsoft Docs"
description: "Megtudhatja, hogyan toocapture és a Linux-alapú Azure virtuális gépek (VM) hello Azure Resource Manager üzembe helyezési modellben létre kép generalize."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 877eee5c842bebe80e755c2240cdaaef4ade6ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a><span data-ttu-id="b8bd5-103">Azure-on futó Linux virtuális gép rögzítése</span><span class="sxs-lookup"><span data-stu-id="b8bd5-103">Capture a Linux virtual machine running on Azure</span></span>
<span data-ttu-id="b8bd5-104">Kövesse a cikk toogeneralize hello lépéseit, és rögzítheti az Azure Linux virtuális gép (VM) hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-104">Follow hello steps in this article toogeneralize and capture your Azure Linux virtual machine (VM) in hello Resource Manager deployment model.</span></span> <span data-ttu-id="b8bd5-105">Generalize hello VM, amikor eltávolítja a személyes fiók adatait, és készítse elő a hello VM toobe képként használni.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-105">When you generalize hello VM, you remove personal account information and prepare hello VM toobe used as an image.</span></span> <span data-ttu-id="b8bd5-106">Ekkor egy általánosított virtuális merevlemezt (VHD) hello az operációs rendszer, a mellékelt adatok lemez VHD-k a lemezkép rögzítése és egy [Resource Manager-sablon](../../azure-resource-manager/resource-group-overview.md) új virtuális gép központi telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-106">You then capture a generalized virtual hard disk (VHD) image for hello OS, VHDs for attached data disks, and a [Resource Manager template](../../azure-resource-manager/resource-group-overview.md) for new VM deployments.</span></span> <span data-ttu-id="b8bd5-107">Ez a cikk részletesen, hogyan toocapture egy virtuális gép rendszerkép hello Azure CLI 1.0 a virtuális gépek nem felügyelt lemezekkel.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-107">This article details how toocapture a VM image with hello Azure CLI 1.0 for a VM using unmanaged disks.</span></span> <span data-ttu-id="b8bd5-108">Emellett [rögzíteni a virtuális gépek Azure felügyelt lemezt az Azure CLI 2.0 hello](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-108">You can also [capture a VM using Azure Managed Disks with hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="b8bd5-109">Felügyelt lemezek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-109">Managed disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="b8bd5-110">További információ: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-110">For more information, see [Azure Managed Disks overview](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="b8bd5-111">toocreate hello lemezképpel, virtuális gépek hálózati erőforrások beállítása az egyes új virtuális gépek, és azt hello rögzített VHD lemezképek hello sablon (JavaScript Object Notation vagy JSON-NÁ, fájl) toodeploy használja.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-111">toocreate VMs using hello image, set up network resources for each new VM, and use hello template (a JavaScript Object Notation, or JSON, file) toodeploy it from hello captured VHD images.</span></span> <span data-ttu-id="b8bd5-112">Ezzel a módszerrel a virtuális gép és az aktuális szoftverkonfigurációt, hasonló toohello módon teszi lehetővé a képek a hello Azure piactér replikálhatja.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-112">In this way, you can replicate a VM with its current software configuration, similar toohello way you use images in hello Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="b8bd5-113">Ha a meglévő Linux virtuális Gépet, a speciális állapottal másolatát toocreate biztonsági mentését vagy hibakeresés, lásd: [másolatot készít egy Azure-on futó Linux virtuális gép](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-113">If you want toocreate a copy of your existing Linux VM with its specialized state for backup or debugging, see [Create a copy of a Linux virtual machine running on Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="b8bd5-114">Ha azt szeretné, hogy tooupload Linux virtuális merevlemez egy helyszíni virtuális Gépre, tekintse meg és [feltöltése és a Linux virtuális gép létrehozása az egyéni lemezképet](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-114">And if you want tooupload a Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>  

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="b8bd5-115">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="b8bd5-115">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="b8bd5-116">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-116">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="b8bd5-117">[Az Azure CLI 1.0](#before-you-begin) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="b8bd5-117">[Azure CLI 1.0](#before-you-begin) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b8bd5-118">[Az Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="b8bd5-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b8bd5-119">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b8bd5-119">Before you begin</span></span>
<span data-ttu-id="b8bd5-120">Győződjön meg arról, hogy teljesülnek-e a következő előfeltételek hello:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-120">Ensure that you meet hello following prerequisites:</span></span>

* <span data-ttu-id="b8bd5-121">**Hello Resource Manager üzembe helyezési modellel létrehozott Azure virtuális gép** -Linux virtuális gép még nem hozott létre, ha használható hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), vagy [erőforrás-kezelő sablonok](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-121">**Azure VM created in hello Resource Manager deployment model** - If you haven't created a Linux VM, you can use hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> 
  
    <span data-ttu-id="b8bd5-122">Virtuális gép szükség szerint hello konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-122">Configure hello VM as needed.</span></span> <span data-ttu-id="b8bd5-123">Például [adatok lemezek hozzáadása a](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), frissítések és alkalmazások telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-123">For example, [add data disks](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), apply updates, and install applications.</span></span> 
* <span data-ttu-id="b8bd5-124">**Az Azure CLI** -telepítés hello [Azure CLI](../../cli-install-nodejs.md) a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-124">**Azure CLI** - Install hello [Azure CLI](../../cli-install-nodejs.md) on a local computer.</span></span>

## <a name="step-1-remove-hello-azure-linux-agent"></a><span data-ttu-id="b8bd5-125">1. lépés: Hello Azure Linux ügynök eltávolítása</span><span class="sxs-lookup"><span data-stu-id="b8bd5-125">Step 1: Remove hello Azure Linux agent</span></span>
<span data-ttu-id="b8bd5-126">Először futtassa a hello **waagent** hello parancsot **deprovision** hello Linux virtuális gép paraméter.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-126">First, run hello **waagent** command with hello **deprovision** parameter on hello Linux VM.</span></span> <span data-ttu-id="b8bd5-127">Ez a parancs törli a fájlokat és adatokat toomake hello virtuális gép készen áll a normalizálása.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-127">This command deletes files and data toomake hello VM ready for generalizing.</span></span> <span data-ttu-id="b8bd5-128">További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-128">For details, see hello [Azure Linux Agent user guide](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

1. <span data-ttu-id="b8bd5-129">Csatlakozás tooyour Linux virtuális gép SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-129">Connect tooyour Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="b8bd5-130">Hello SSH ablakban írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-130">In hello SSH window, type hello following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > <span data-ttu-id="b8bd5-131">A parancs egy virtuális gépen, hogy szeretné-e képként toocapture csak fut.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-131">Only run this command on a VM that you intend toocapture as an image.</span></span> <span data-ttu-id="b8bd5-132">Hello lemezkép törlődik a bizalmas információk, vagy alkalmas terjesztési nem garantálja.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-132">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution.</span></span>
 
3. <span data-ttu-id="b8bd5-133">Típus **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-133">Type **y** toocontinue.</span></span> <span data-ttu-id="b8bd5-134">Hozzáadhat hello **-force** paraméter tooavoid a megerősítési lépés.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-134">You can add hello **-force** parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="b8bd5-135">Hello parancs után írja be a **kilépéshez**.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-135">After hello command completes, type **exit**.</span></span> <span data-ttu-id="b8bd5-136">Ez a lépés hello SSH-ügyfél bezárása után.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-136">This step closes hello SSH client.</span></span>

## <a name="step-2-capture-hello-vm"></a><span data-ttu-id="b8bd5-137">2. lépés: Hello virtuális gép rögzítése</span><span class="sxs-lookup"><span data-stu-id="b8bd5-137">Step 2: Capture hello VM</span></span>
<span data-ttu-id="b8bd5-138">Hello Azure CLI toogeneralize használja, és hello virtuális gép rögzítése.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-138">Use hello Azure CLI toogeneralize and capture hello VM.</span></span> <span data-ttu-id="b8bd5-139">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-139">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="b8bd5-140">Példa paraméter nevek a következők **myResourceGroup**, **myVnet**, és **myVM**.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-140">Example parameter names include **myResourceGroup**, **myVnet**, and **myVM**.</span></span>

1. <span data-ttu-id="b8bd5-141">A helyi számítógépről nyissa meg a hello Azure CLI és [bejelentkezési tooyour Azure-előfizetés](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-141">From your local computer, open hello Azure CLI and [login tooyour Azure subscription](../../xplat-cli-connect.md).</span></span> 
2. <span data-ttu-id="b8bd5-142">Győződjön meg arról, hogy az erőforrás-kezelő módban van.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-142">Make sure you are in Resource Manager mode.</span></span>
   
    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="b8bd5-143">Állítsa le virtuális Gépet, amely már platformelőfizetés hello a következő parancs használatával hello:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-143">Shut down hello VM that you already deprovisioned by using hello following command:</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. <span data-ttu-id="b8bd5-144">A parancs a következő hello VM hello generalize:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-144">Generalize hello VM with hello following command:</span></span>
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. <span data-ttu-id="b8bd5-145">Most futtassa a hello **azure virtuális gép rögzítése** parancs, mely rögzíti a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-145">Now run hello **azure vm capture** command, which captures hello VM.</span></span> <span data-ttu-id="b8bd5-146">A következő példa hello, hello kép VHD-k a rendszer rögzíti-tól kezdődően nevek **MyVHDNamePrefix**, és hello **-t** beállítás adja meg egy elérési utat toohello sablon **MyTemplate.json**.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-146">In hello following example, hello image VHDs are captured with names beginning with **MyVHDNamePrefix**, and hello **-t** option specifies a path toohello template **MyTemplate.json**.</span></span> 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="b8bd5-147">Alapértelmezés szerint a ugyanazt a tárfiókot, amely az eredeti virtuális gép hello használt hello hello kép VHD-fájlok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-147">hello image VHD files get created by default in hello same storage account that hello original VM used.</span></span> <span data-ttu-id="b8bd5-148">Használjon hello *ugyanazt a tárfiókot* toostore hello bármely hello lemezképből hoz létre új virtuális gépek virtuális merevlemezeket.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-148">Use hello *same storage account* toostore hello VHDs for any new VMs you create from hello image.</span></span> 

6. <span data-ttu-id="b8bd5-149">a rögzített lemezkép, egy szövegszerkesztőben megnyitott hello JSON-sablon toofind hello helye.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-149">toofind hello location of a captured image, open hello JSON template in a text editor.</span></span> <span data-ttu-id="b8bd5-150">A hello **storageProfile**, megkeresi hello **uri** a hello **kép** hello található **rendszer** tároló.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-150">In hello **storageProfile**, find hello **uri** of hello **image** located in hello **system** container.</span></span> <span data-ttu-id="b8bd5-151">Például hello URI-jának hello az operációs rendszer lemezképét hasonlít túl`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="b8bd5-151">For example, hello URI of hello OS disk image is similar too`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>

## <a name="step-3-create-a-vm-from-hello-captured-image"></a><span data-ttu-id="b8bd5-152">3. lépés: Virtuális gép létrehozása hello rögzített lemezképből</span><span class="sxs-lookup"><span data-stu-id="b8bd5-152">Step 3: Create a VM from hello captured image</span></span>
<span data-ttu-id="b8bd5-153">Most már használja egy sablon toocreate Linux virtuális gép hello kép.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-153">Now use hello image with a template toocreate a Linux VM.</span></span> <span data-ttu-id="b8bd5-154">Ezeket a lépéseket bemutatják, hogyan toouse hello Azure CLI-t, és egy új virtuális hálózat a virtuális gép toocreate hello rögzített JSON-fájl sablon hello.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-154">These steps show you how toouse hello Azure CLI and hello JSON file template you captured toocreate hello VM in a new virtual network.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="b8bd5-155">Hálózati erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="b8bd5-155">Create network resources</span></span>
<span data-ttu-id="b8bd5-156">toouse hello sablon, először a virtuális hálózat és a hálózati adapter tooset az új virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-156">toouse hello template, you first need tooset up a virtual network and NIC for your new VM.</span></span> <span data-ttu-id="b8bd5-157">Javasoljuk, hogy hozzon létre egy erőforráscsoportot ezeket az erőforrásokat, a Virtuálisgép-lemezkép tároló hello helyen.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-157">We recommend you create a resource group for these resources in hello location where your VM image is stored.</span></span> <span data-ttu-id="b8bd5-158">Futtassa a következő, az erőforrások és a megfelelő Azure-beli hely (ezek a parancsok a "centralus" jelöli) tartozó nevek és hasonló toohello parancsokat:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-158">Run commands similar toohello following, substituting names for your resources and an appropriate Azure location ("centralus" in these commands):</span></span>

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-hello-id-of-hello-nic"></a><span data-ttu-id="b8bd5-159">Első hello hello hálózati adapter azonosítója</span><span class="sxs-lookup"><span data-stu-id="b8bd5-159">Get hello Id of hello NIC</span></span>
<span data-ttu-id="b8bd5-160">toodeploy hello lemezkép rögzítése során mentett JSON hello segítségével a virtuális gép, kell hello azonosítója hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-160">toodeploy a VM from hello image by using hello JSON you saved during capture, you need hello Id of hello NIC.</span></span> <span data-ttu-id="b8bd5-161">Az beszerzése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-161">Obtain it by running hello following command:</span></span>

```azurecli
azure network nic show myResourceGroup1 myNIC
```

<span data-ttu-id="b8bd5-162">Hello **azonosító** hello a kimeneti hasonlít túl`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span><span class="sxs-lookup"><span data-stu-id="b8bd5-162">hello **Id** in hello output is similar too`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span></span>

### <a name="create-a-vm"></a><span data-ttu-id="b8bd5-163">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b8bd5-163">Create a VM</span></span>
<span data-ttu-id="b8bd5-164">Most futtassa hello következő parancsot a toocreate hello a virtuális gép rögzítése a Virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-164">Now run hello following command toocreate your VM from hello captured VM image.</span></span> <span data-ttu-id="b8bd5-165">Használjon hello **-f** paraméter toospecify hello elérési toohello sablon JSON fájl mentése.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-165">Use hello **-f** parameter toospecify hello path toohello template JSON file you saved.</span></span>

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

<span data-ttu-id="b8bd5-166">A hello parancs kimenetében rákérdezéses toosupply egy új virtuális gép nevét, hello rendszergazda felhasználónevet és jelszót, és hello hello korábban létrehozott hálózati adapter azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-166">In hello command output, you are prompted toosupply a new VM name, hello admin user name and password, and hello Id of hello NIC you created previously.</span></span>

```bash
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

<span data-ttu-id="b8bd5-167">a következő minta hello jeleníti meg, a sikeres telepítés lásd:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-167">hello following sample shows what you see for a successful deployment:</span></span>

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment toocomplete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-hello-deployment"></a><span data-ttu-id="b8bd5-168">Hello telepítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="b8bd5-168">Verify hello deployment</span></span>
<span data-ttu-id="b8bd5-169">Most SSH toohello virtuális gép tooverify hello üzembe helyezési és kezdő használatával létrehozott új virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-169">Now SSH toohello virtual machine you created tooverify hello deployment and start using hello new VM.</span></span> <span data-ttu-id="b8bd5-170">ssh, tooconnect hello IP-címének a hello hello a következő parancs futtatásával létrehozott virtuális Géphez:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-170">tooconnect via SSH, find hello IP address of hello VM you created by running hello following command:</span></span>

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

<span data-ttu-id="b8bd5-171">hello nyilvános IP-cím szerepel hello parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-171">hello public IP address is listed in hello command output.</span></span> <span data-ttu-id="b8bd5-172">Alapértelmezés szerint csatlakozás toohello Linux virtuális gép által SSH 22-es porton.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-172">By default you connect toohello Linux VM by SSH on port 22.</span></span>

## <a name="create-additional-vms"></a><span data-ttu-id="b8bd5-173">További virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b8bd5-173">Create additional VMs</span></span>
<span data-ttu-id="b8bd5-174">Használjon hello rögzített rendszerkép és sablon toodeploy szakasz megelőző hello hello lépésekkel további virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-174">Use hello captured image and template toodeploy additional VMs using hello steps in hello preceding section.</span></span> <span data-ttu-id="b8bd5-175">Egyéb beállítások toocreate virtuális gépek hello lemezképből tartalmaz gyors üzembe helyezés sablon használatával, illetve hello futtató **azure virtuális gép létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-175">Other options toocreate VMs from hello image include using a quickstart template or running hello **azure vm create** command.</span></span>

### <a name="use-hello-captured-template"></a><span data-ttu-id="b8bd5-176">Hello rögzített sablon használata</span><span class="sxs-lookup"><span data-stu-id="b8bd5-176">Use hello captured template</span></span>
<span data-ttu-id="b8bd5-177">toouse hello rögzített rendszerkép és a sablon, kövesse az alábbi lépéseket (amelynek részleteit a fenti szakaszban hello):</span><span class="sxs-lookup"><span data-stu-id="b8bd5-177">toouse hello captured image and template, follow these steps (detailed in hello preceding section):</span></span>

* <span data-ttu-id="b8bd5-178">Győződjön meg arról, hogy a Virtuálisgép-lemezkép hello ugyanazt a tárfiókot, amelyen a virtuális gép virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-178">Ensure that your VM image is in hello same storage account that hosts your VM's VHD.</span></span>
* <span data-ttu-id="b8bd5-179">Hello sablon JSON-fájlt másolja, és adjon meg egy egyedi nevet az operációsrendszer-lemez hello hello új virtuális gép virtuális merevlemez (vagy VHD-k).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-179">Copy hello template JSON file and specify a unique name for hello OS disk of hello new VM's VHD (or VHDs).</span></span> <span data-ttu-id="b8bd5-180">Például a hello **storageProfile**a **vhd**, a **uri**, adjon meg egy egyedi nevet hello **osDisk** VHD, hasonló túl`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="b8bd5-180">For example, in hello **storageProfile**, under **vhd**, in **uri**, specify a unique name for hello **osDisk** VHD, similar too`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>
* <span data-ttu-id="b8bd5-181">Hozzon létre egy hálózati adapter vagy hello ugyanabban vagy egy másik virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-181">Create a NIC in either hello same or a different virtual network.</span></span>
* <span data-ttu-id="b8bd5-182">Hello módosított sablon JSON-fájl használatával hozhat létre a központi telepítés hello erőforráscsoportban, amelyben beállítása hello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-182">Using hello modified template JSON file, create a deployment in hello resource group in which you set up hello virtual network.</span></span>

### <a name="use-a-quickstart-template"></a><span data-ttu-id="b8bd5-183">A következő gyorsindítási sablonon használata</span><span class="sxs-lookup"><span data-stu-id="b8bd5-183">Use a quickstart template</span></span>
<span data-ttu-id="b8bd5-184">Ha azt szeretné, hogy automatikusan létrehozásakor a virtuális gépek hello lemezképből hello hálózaton, egy sablon adhat meg ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-184">If you want hello network set up automatically when you create a VM from hello image, you can specify those resources in a template.</span></span> <span data-ttu-id="b8bd5-185">Lásd például: hello [101-vm-a-felhasználó-lemezkép sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) a Githubról.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-185">For example, see hello [101-vm-from-user-image template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) from GitHub.</span></span> <span data-ttu-id="b8bd5-186">Ez a sablon egy virtuális Gépet hoz létre az egyéni lemezképet és a hello szükséges virtuális hálózati, a nyilvános IP-cím és a hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-186">This template creates a VM from your custom image and hello necessary virtual network, public IP address, and NIC resources.</span></span> <span data-ttu-id="b8bd5-187">Az Azure-portálon hello hello-sablonnal útmutatást lásd: [hogyan toocreate virtuális gépek létrehozását a Resource Manager-sablon használatával egyéni lemezkép](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-187">For a walkthrough of using hello template in hello Azure portal, see [How toocreate a virtual machine from a custom image using a Resource Manager template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span></span>

### <a name="use-hello-azure-vm-create-command"></a><span data-ttu-id="b8bd5-188">Használja a hello azure virtuális gép létrehozása parancs</span><span class="sxs-lookup"><span data-stu-id="b8bd5-188">Use hello azure vm create command</span></span>
<span data-ttu-id="b8bd5-189">Általában azt a legegyszerűbb toouse a Resource Manager sablon toocreate hello lemezképből egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-189">Usually it's easiest toouse a Resource Manager template toocreate a VM from hello image.</span></span> <span data-ttu-id="b8bd5-190">Azonban létrehozhat hello VM *imperatively* hello segítségével **azure virtuális gép létrehozása** hello parancsot **-Q** (**--lemezkép-urn**) paraméter .</span><span class="sxs-lookup"><span data-stu-id="b8bd5-190">However, you can create hello VM *imperatively* by using hello **azure vm create** command with hello **-Q** (**--image-urn**) parameter.</span></span> <span data-ttu-id="b8bd5-191">Ha ezt a módszert használja, akkor is átadhatja hello **-d** (**--operációsrendszer-lemez-vhd**) paraméter toospecify hello hely az operációs rendszer hello .vhd fájl hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-191">If you use this method, you also pass hello **-d** (**--os-disk-vhd**) parameter toospecify hello location of hello OS .vhd file for hello new VM.</span></span> <span data-ttu-id="b8bd5-192">Ez a fájl hello kép VHD-fájlt tároló hello tárfiók hello a VHD-k tárolóban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-192">This file must be in hello vhds container of hello storage account where hello image VHD file is stored.</span></span> <span data-ttu-id="b8bd5-193">hello másolatok VHD hello hello parancs új virtuális gép automatikusan toohello **VHD-k** tároló.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-193">hello command copies hello VHD for hello new VM automatically toohello **vhds** container.</span></span>

<span data-ttu-id="b8bd5-194">Futtatása előtt **azure virtuális gép létrehozása** hello lemezképpel, végezze el a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="b8bd5-194">Before running **azure vm create** with hello image, complete hello following steps:</span></span>

1. <span data-ttu-id="b8bd5-195">Hozzon létre egy erőforráscsoportot, vagy egy meglévő erőforráscsoportot hello telepítéshez azonosítani.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-195">Create a resource group, or identify an existing resource group for hello deployment.</span></span>
2. <span data-ttu-id="b8bd5-196">Hozzon létre egy nyilvános IP-cím és a hálózati erőforrás hello új virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-196">Create a public IP address resource and a NIC resource for hello new VM.</span></span> <span data-ttu-id="b8bd5-197">Lépéseket toocreate egy virtuális hálózati, nyilvános IP-cím és hálózati hello CLI, használatával tekintse meg az ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-197">For steps toocreate a virtual network, public IP address, and NIC by using hello CLI, see earlier in this article.</span></span> <span data-ttu-id="b8bd5-198">(**azure virtuális gép létrehozása** is létrehozhat egy hálózati Adaptert, de a virtuális hálózati és alhálózati kell toopass további paraméterek.)</span><span class="sxs-lookup"><span data-stu-id="b8bd5-198">(**azure vm create** can also create a NIC, but you need toopass additional parameters for a virtual network and subnet.)</span></span>

<span data-ttu-id="b8bd5-199">Ezután futtassa a parancsot, kapott URI-azonosítók tooboth hello új operációs rendszer VHD-fájlt és a meglévő lemezkép hello.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-199">Then run a command that passes URIs tooboth hello new OS VHD file and hello existing image.</span></span> <span data-ttu-id="b8bd5-200">Ebben a példában a mérete Standard_A1 virtuális gép létrehozása a hello USA keleti régiójában.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-200">In this example, a size Standard_A1 VM is created in hello East US region.</span></span>

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

<span data-ttu-id="b8bd5-201">A további parancssori kapcsolók, futtassa a `azure help vm create`.</span><span class="sxs-lookup"><span data-stu-id="b8bd5-201">For additional command options, run `azure help vm create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b8bd5-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b8bd5-202">Next steps</span></span>
<span data-ttu-id="b8bd5-203">toomanage hello CLI-t, a virtuális gépek, tekintse meg a hello feladatok [központi telepítése és virtuális gépek Azure Resource Manager-sablonok segítségével kezel, és az Azure parancssori felület hello](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="b8bd5-203">toomanage your VMs with hello CLI, see hello tasks in [Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI](create-ssh-secured-vm-from-template.md).</span></span>

