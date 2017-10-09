---
title: "a Linux virtuális gép hibaelhárítás az Azure CLI 1.0 hello aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan Linux virtuális gép problémák használatával csatlakozó hello OS lemez tooa helyreállítási virtuális gép tootroubleshoot hello Azure CLI 1.0"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 398f681d1149299d444fcfdab20737315db02855
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-cli-10"></a><span data-ttu-id="b6ff9-103">Linux virtuális gép hibaelhárítása hello OS lemez tooa helyreállítási virtuális Gépet csatolásával használatával hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b6ff9-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="b6ff9-104">Ha a Linux virtuális gép (VM) rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="b6ff9-105">Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab` , amely megakadályozza hello méretű képes tooboot sikeresen.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="b6ff9-106">Ez a cikk részletek hogyan toouse hello Azure CLI 1.0 tooconnect a virtuális merevlemez lemez tooanother Linux virtuális gép toofix ki a hibákat, majd hozza létre újból az eredeti virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-106">This article details how toouse hello Azure CLI 1.0 tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="b6ff9-107">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="b6ff9-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="b6ff9-108">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="b6ff9-109">[Az Azure CLI 1.0](#recovery-process-overview) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="b6ff9-109">[Azure CLI 1.0](#recovery-process-overview) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b6ff9-110">[Az Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="b6ff9-110">[Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="b6ff9-111">Helyreállítási folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="b6ff9-111">Recovery process overview</span></span>
<span data-ttu-id="b6ff9-112">hibaelhárítási folyamatának hello a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-112">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="b6ff9-113">Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-113">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="b6ff9-114">Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Linux virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-114">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="b6ff9-115">Csatlakoztassa a VM hibaelhárítási toohello.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-115">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="b6ff9-116">Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-116">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="b6ff9-117">Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-117">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="b6ff9-118">Hello eredeti virtuális merevlemez virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-118">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="b6ff9-119">Győződjön meg arról, hogy rendelkezik [hello Azure CLI legújabb 1.0](../../cli-install-nodejs.md) telepítve és bejelentkezett és erőforrás-kezelő módban:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-119">Make sure that you have [hello latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="b6ff9-120">Hello alábbi példák cserélje le a paraméter nevét a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-120">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="b6ff9-121">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-121">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="b6ff9-122">Határozza meg a rendszerindítási problémák</span><span class="sxs-lookup"><span data-stu-id="b6ff9-122">Determine boot issues</span></span>
<span data-ttu-id="b6ff9-123">Vizsgálja meg a hello soros kimeneti toodetermine miért a virtuális gép nem tud tooboot megfelelően.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-123">Examine hello serial output toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="b6ff9-124">Ilyenek például a bejegyzés érvénytelen `/etc/fstab`, vagy az alapul szolgáló virtuális merevlemez alatt töröltek vagy áthelyeztek hello.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-124">A common example is an invalid entry in `/etc/fstab`, or hello underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="b6ff9-125">hello alábbi példa lekérése hello soros kimeneti hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-125">hello following example gets hello serial output from hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b6ff9-126">Tekintse át a hello soros kimeneti toodetermine miért hello virtuális gép nem tud tooboot.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-126">Review hello serial output toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="b6ff9-127">Ha soros kimeneti hello nem ad meg semmilyen arra utal, hogy, szükség lehet a naplófájlokat tooreview `/var/log` után hello virtuális géphez csatlakoztatott merevlemez VM hibaelhárítási tooa.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-127">If hello serial output isn't providing any indication, you may need tooreview log files in `/var/log` once you have hello virtual hard disk connected tooa troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="b6ff9-128">Meglévő virtuális merevlemez részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="b6ff9-128">View existing virtual hard disk details</span></span>
<span data-ttu-id="b6ff9-129">A virtuális merevlemez tooanother VM csatolhat, meg kell tooidentify hello neve hello virtuális merevlemez (VHD).</span><span class="sxs-lookup"><span data-stu-id="b6ff9-129">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="b6ff9-130">hello alábbi példa lekérdezi hello nevű virtuális gép adatai `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-130">hello following example gets information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b6ff9-131">Keressen `Vhd URI` a hello hello megelőző parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-131">Look for `Vhd URI` in hello output from hello preceding command.</span></span> <span data-ttu-id="b6ff9-132">hello következő csonkolt példa kimenet látható hello `Vhd URI` hello utolsó sor:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-132">hello following truncated example output shows hello `Vhd URI` on hello last line:</span></span>

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myVM"
+ Looking up hello NIC "myNic"
+ Looking up hello public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a><span data-ttu-id="b6ff9-133">Meglévő virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="b6ff9-133">Delete existing VM</span></span>
<span data-ttu-id="b6ff9-134">A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-134">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="b6ff9-135">A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-135">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="b6ff9-136">hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="b6ff9-136">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="b6ff9-137">Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-137">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="b6ff9-138">Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-138">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="b6ff9-139">hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-139">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="b6ff9-140">első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-140">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="b6ff9-141">Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-141">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="b6ff9-142">Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-142">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="b6ff9-143">a következő példa törlések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportból `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-143">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="b6ff9-144">Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-144">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="b6ff9-145">hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-145">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="b6ff9-146">Meglévő virtuális merevlemez tooanother VM csatolása</span><span class="sxs-lookup"><span data-stu-id="b6ff9-146">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="b6ff9-147">A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-147">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="b6ff9-148">Hello VM toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-148">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="b6ff9-149">Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-149">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="b6ff9-150">Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-150">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="b6ff9-151">Hello meglévő virtuálismerevlemez-fájl csatolása, adjon meg hello URL-cím toohello lemezt előző hello beolvasott `azure vm show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-151">When you attach hello existing virtual hard disk, specify hello URL toohello disk obtained in hello preceding `azure vm show` command.</span></span> <span data-ttu-id="b6ff9-152">hello alábbi példa csatolja egy meglévő virtuális merevlemez toohello nevű virtuális gép hibakeresési `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-152">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="b6ff9-153">Hello csatolt adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="b6ff9-153">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="b6ff9-154">hello következő példák részletesen hello lépéseit egy Ubuntu virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-154">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="b6ff9-155">Red Hat Enterprise Linux és SUSE, például a különböző Linux distro használata hello naplófájl helye és `mount` parancsok kissé eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-155">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="b6ff9-156">A parancsok hello megfelelő változásokat az adott distro a tekintse meg a toohello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-156">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="b6ff9-157">SSH tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-157">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="b6ff9-158">Ha a lemez hello első adatok csatlakoztatott lemez tooyour VM hibaelhárítási, hello lemez valószínűleg csatlakozik-e meg a túl`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-158">If this disk is hello first data disk attached tooyour troubleshooting VM, hello disk is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="b6ff9-159">Használjon `dmseg` tooview csatlakoztatott lemezekkel:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-159">Use `dmseg` tooview attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="b6ff9-160">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-160">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="b6ff9-161">A fenti példa hello, hello operációsrendszer-lemez jelenleg `/dev/sda` hello ideiglenes lemez megadott összes virtuális Géphez és `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-161">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="b6ff9-162">Ha több adatlemezek, hogy legyen `/dev/sdd`, `/dev/sde`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-162">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="b6ff9-163">Hozzon létre egy könyvtárat toomount a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-163">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="b6ff9-164">hello alábbi példa létrehoz egy könyvtárat nevű `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-164">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="b6ff9-165">Ha több partíciót a meglévő virtuális merevlemezen, csatlakoztassa a szükséges hello partíció.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-165">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="b6ff9-166">hello alábbi példa csatlakoztatja, elsődleges partíció első hello `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-166">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="b6ff9-167">Ajánlott toomount adatlemezek virtuális gépeken Azure használatával hello univerzálisan egyedi azonosító (UUID) hello virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-167">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="b6ff9-168">A jelen rövid hibaelhárítási esetben csatlakoztatását a hello virtuális merevlemez segítségével hello UUID nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-168">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="b6ff9-169">Azonban a normál használja, a Szerkesztés `/etc/fstab` toomount virtuális merevlemezek helyett eszköznév UUID okozhat hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-169">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="b6ff9-170">Az eredeti virtuális merevlemez kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="b6ff9-170">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="b6ff9-171">Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-171">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="b6ff9-172">Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-172">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="b6ff9-173">Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="b6ff9-173">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="b6ff9-174">Ha a hibák fakadó problémák megoldásával válassza le, és hello meglévő virtuális merevlemez válassza le a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-174">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="b6ff9-175">Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-175">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="b6ff9-176">A hello a virtuális gép hibakeresési SSH munkamenet tooyour leválasztása hello létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-176">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="b6ff9-177">Először kívül hello szülőkönyvtárában a csatlakoztatási pont módosítása:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-177">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="b6ff9-178">Most leválasztása hello létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-178">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="b6ff9-179">hello alábbi példa leválasztja hello eszközéből `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-179">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="b6ff9-180">Most válassza le a hello hello virtuális gép a virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-180">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="b6ff9-181">Lépjen ki a virtuális gép hibakeresési hello SSH-munkamenet tooyour.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-181">Exit hello SSH session tooyour troubleshooting VM.</span></span> <span data-ttu-id="b6ff9-182">Első lista hello hello Azure parancssori felület, virtuális gép hibakeresési adatok lemezek tooyour csatolva.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-182">In hello Azure CLI, first list hello attached data disks tooyour troubleshooting VM.</span></span> <span data-ttu-id="b6ff9-183">hello alábbi példa listák hello adatlemezt csatolni toohello nevű virtuális gép `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-183">hello following example lists hello data disks attached toohello VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    <span data-ttu-id="b6ff9-184">Megjegyzés: hello `Lun` értékét a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-184">Note hello `Lun` value for your existing virtual hard disk.</span></span> <span data-ttu-id="b6ff9-185">hello alábbi példa parancs kimenetét mutatja hello meglévő virtuális lemez LUN azonosítójú 0:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-185">hello following example command output shows hello existing virtual disk attached at LUN 0:</span></span>

    ```azurecli
    info:    Executing command vm disk list
    + Looking up hello VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    <span data-ttu-id="b6ff9-186">Válassza le a virtuális gépet a megfelelő hello hello adatlemezét `Lun` érték:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-186">Detach hello data disk from your VM using hello applicable `Lun` value:</span></span>

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="b6ff9-187">Virtuális gép eredeti merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="b6ff9-187">Create VM from original hard disk</span></span>
<span data-ttu-id="b6ff9-188">toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="b6ff9-188">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="b6ff9-189">hello tényleges JSON-sablon jelenleg a következő hivatkozás hello:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-189">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="b6ff9-190">https://RAW.githubusercontent.com/Azure/Azure-quickstart-Templates/Master/201-VM-specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="b6ff9-190">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="b6ff9-191">hello sablon létrehozni meglévő virtuális hálózatban, hello virtuális merevlemez URL-címet a hello segítségével központilag telepíti egy virtuális gép korábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-191">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="b6ff9-192">hello következő példa telepíti hello sablon toohello nevű erőforráscsoport `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-192">hello following example deploys hello template toohello resource group named `myResourceGroup`:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

<span data-ttu-id="b6ff9-193">Válasz hello megadását kéri az hello sablont, például a virtuális gép nevét (`myDeployedVM` hello a következő példa), az operációs rendszer típusa (`Linux`), és a Virtuálisgép-méretet (`Standard_DS1_v2`).</span><span class="sxs-lookup"><span data-stu-id="b6ff9-193">Answer hello prompts for hello template such as VM name (`myDeployedVM` hello following example), OS type (`Linux`), and VM size (`Standard_DS1_v2`).</span></span> <span data-ttu-id="b6ff9-194">Hello `osDiskVhdUri` azonos az előzőleg használt hello VM hibaelhárítási meglévő virtuális merevlemez toohello csatlakoztatása hello.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-194">hello `osDiskVhdUri` is hello same as previously used when attaching hello existing virtual hard disk toohello troubleshooting VM.</span></span> <span data-ttu-id="b6ff9-195">Hello parancs kimenete és felszólítás például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-195">An example of hello command output and prompts is as follows:</span></span>

```azurecli
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment toocomplete
+
```


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="b6ff9-196">Engedélyezze újra a rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="b6ff9-196">Re-enable boot diagnostics</span></span>

<span data-ttu-id="b6ff9-197">Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="b6ff9-197">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="b6ff9-198">hello következő példa engedélyezi hello hello nevű virtuális gép diagnosztikai kiterjesztés `myDeployedVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b6ff9-198">hello following example enables hello diagnostic extension on hello VM named `myDeployedVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="b6ff9-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b6ff9-199">Next steps</span></span>
<span data-ttu-id="b6ff9-200">Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooan Azure virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b6ff9-200">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="b6ff9-201">A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Linux virtuális gép](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b6ff9-201">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
