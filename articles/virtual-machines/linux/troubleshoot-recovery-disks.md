---
title: "a Linux virtuális gép hibaelhárítás az Azure CLI 2.0 hello aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan Linux virtuális gép problémák használatával csatlakozó hello OS lemez tooa helyreállítási virtuális gép tootroubleshoot hello Azure CLI 2.0"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/16/2017
ms.author: iainfou
ms.openlocfilehash: 776d61b61280f46e3699157addcdb1e7dfb6818e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-with-hello-azure-cli-20"></a><span data-ttu-id="a5fe2-103">Linux virtuális gép operációs rendszer hello lemez tooa helyreállítási virtuális Gépet hello Azure CLI 2.0 csatolásával hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a5fe2-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM with hello Azure CLI 2.0</span></span>
<span data-ttu-id="a5fe2-104">Ha a Linux virtuális gép (VM) rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="a5fe2-105">Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab` , amely megakadályozza hello méretű képes tooboot sikeresen.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="a5fe2-106">Ez a cikk részletek hogyan toouse hello Azure CLI 2.0 tooconnect a virtuális merevlemez lemez tooanother Linux virtuális gép toofix ki a hibákat, majd hozza létre újból az eredeti virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-106">This article details how toouse hello Azure CLI 2.0 tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span> <span data-ttu-id="a5fe2-107">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-107">You can also perform these steps with hello [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="a5fe2-108">Helyreállítási folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="a5fe2-108">Recovery process overview</span></span>
<span data-ttu-id="a5fe2-109">hibaelhárítási folyamatának hello a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-109">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="a5fe2-110">Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-110">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="a5fe2-111">Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Linux virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-111">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="a5fe2-112">Csatlakoztassa a VM hibaelhárítási toohello.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-112">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="a5fe2-113">Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-113">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="a5fe2-114">Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-114">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="a5fe2-115">Hello eredeti virtuális merevlemez virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-115">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="a5fe2-116">tooperform ezek a hibaelhárítási lépéseket, hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-116">tooperform these troubleshooting steps, you need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="a5fe2-117">Hello alábbi példák cserélje le a paraméter nevét a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-117">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="a5fe2-118">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-118">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="a5fe2-119">Határozza meg a rendszerindítási problémák</span><span class="sxs-lookup"><span data-stu-id="a5fe2-119">Determine boot issues</span></span>
<span data-ttu-id="a5fe2-120">Vizsgálja meg a hello soros kimeneti toodetermine miért a virtuális gép nem tud tooboot megfelelően.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-120">Examine hello serial output toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="a5fe2-121">Ilyenek például a bejegyzés érvénytelen `/etc/fstab`, vagy az alapul szolgáló virtuális merevlemez alatt töröltek vagy áthelyeztek hello.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-121">A common example is an invalid entry in `/etc/fstab`, or hello underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="a5fe2-122">Hello rendszerindító naplófájlok rendelkező [az virtuális gép rendszerindítási-diagnosztika get-rendszerindítási-naplófájl](/cli/azure/vm/boot-diagnostics#get-boot-log).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-122">Get hello boot logs with [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics#get-boot-log).</span></span> <span data-ttu-id="a5fe2-123">hello alábbi példa lekérése hello soros kimeneti hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-123">hello following example gets hello serial output from hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="a5fe2-124">Tekintse át a hello soros kimeneti toodetermine miért hello virtuális gép nem tud tooboot.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-124">Review hello serial output toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="a5fe2-125">Ha soros kimeneti hello nem ad meg semmilyen arra utal, hogy, szükség lehet a naplófájlokat tooreview `/var/log` után hello virtuális géphez csatlakoztatott merevlemez VM hibaelhárítási tooa.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-125">If hello serial output isn't providing any indication, you may need tooreview log files in `/var/log` once you have hello virtual hard disk connected tooa troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="a5fe2-126">Meglévő virtuális merevlemez részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="a5fe2-126">View existing virtual hard disk details</span></span>
<span data-ttu-id="a5fe2-127">A virtuális merevlemez (VHD) tooanother VM csatolhat, meg kell tooidentify hello hello operációsrendszer-lemez URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-127">Before you can attach your virtual hard disk (VHD) tooanother VM, you need tooidentify hello URI of hello OS disk.</span></span> 

<span data-ttu-id="a5fe2-128">Tekintse meg a virtuális Gépet a [az vm megjelenítése](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-128">View information about your VM with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="a5fe2-129">Használjon hello `--query` jelző tooextract hello URI toohello OS lemezre.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-129">Use hello `--query` flag tooextract hello URI toohello OS disk.</span></span> <span data-ttu-id="a5fe2-130">hello alábbi példa lekérdezi hello nevű virtuális gép lemezeinek adatai `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-130">hello following example gets disk information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

<span data-ttu-id="a5fe2-131">hello URI túl hasonló**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-131">hello URI is similar too**https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span></span>

## <a name="delete-existing-vm"></a><span data-ttu-id="a5fe2-132">Meglévő virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="a5fe2-132">Delete existing VM</span></span>
<span data-ttu-id="a5fe2-133">A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-133">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="a5fe2-134">A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-134">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="a5fe2-135">hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-135">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="a5fe2-136">Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-136">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="a5fe2-137">Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-137">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="a5fe2-138">hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-138">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="a5fe2-139">első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-139">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="a5fe2-140">Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-140">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="a5fe2-141">Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-141">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="a5fe2-142">Törölje a virtuális gép hello [az virtuális gép törlése](/cli/azure/vm#delete).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-142">Delete hello VM with [az vm delete](/cli/azure/vm#delete).</span></span> <span data-ttu-id="a5fe2-143">a következő példa törlések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportból `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-143">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="a5fe2-144">Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-144">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="a5fe2-145">hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-145">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="a5fe2-146">Meglévő virtuális merevlemez tooanother VM csatolása</span><span class="sxs-lookup"><span data-stu-id="a5fe2-146">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="a5fe2-147">A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-147">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="a5fe2-148">Hello VM toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-148">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="a5fe2-149">Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-149">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="a5fe2-150">Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-150">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="a5fe2-151">Hello meglévő virtuális merevlemez csatlakoztatása [nem felügyelt az virtuálisgép-lemez csatolása](/cli/azure/vm/unmanaged-disk#attach).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-151">Attach hello existing virtual hard disk with [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk#attach).</span></span> <span data-ttu-id="a5fe2-152">Hello meglévő virtuálismerevlemez-fájl csatolása, adja meg a hello URI toohello lemez előző hello beolvasott `az vm show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-152">When you attach hello existing virtual hard disk, specify hello URI toohello disk obtained in hello preceding `az vm show` command.</span></span> <span data-ttu-id="a5fe2-153">hello alábbi példa csatolja egy meglévő virtuális merevlemez toohello nevű virtuális gép hibakeresési `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-153">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="a5fe2-154">Hello csatolt adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="a5fe2-154">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="a5fe2-155">hello következő példák részletesen hello lépéseit egy Ubuntu virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-155">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="a5fe2-156">Red Hat Enterprise Linux és SUSE, például a különböző Linux distro használata hello naplófájl helye és `mount` parancsok kissé eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-156">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="a5fe2-157">A parancsok hello megfelelő változásokat az adott distro a tekintse meg a toohello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-157">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="a5fe2-158">SSH tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-158">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="a5fe2-159">Ha a lemez hello első adatok csatlakoztatott lemez tooyour VM hibaelhárítási, hello lemez valószínűleg csatlakozik-e meg a túl`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-159">If this disk is hello first data disk attached tooyour troubleshooting VM, hello disk is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="a5fe2-160">Használjon `dmseg` tooview csatlakoztatott lemezekkel:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-160">Use `dmseg` tooview attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="a5fe2-161">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-161">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="a5fe2-162">A fenti példa hello, hello operációsrendszer-lemez jelenleg `/dev/sda` hello ideiglenes lemez megadott összes virtuális Géphez és `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-162">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="a5fe2-163">Ha több adatlemezek, hogy legyen `/dev/sdd`, `/dev/sde`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-163">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="a5fe2-164">Hozzon létre egy könyvtárat toomount a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-164">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="a5fe2-165">hello alábbi példa létrehoz egy könyvtárat nevű `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-165">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="a5fe2-166">Ha több partíciót a meglévő virtuális merevlemezen, csatlakoztassa a szükséges hello partíció.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-166">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="a5fe2-167">hello alábbi példa csatlakoztatja, elsődleges partíció első hello `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-167">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="a5fe2-168">Ajánlott toomount adatlemezek virtuális gépeken Azure használatával hello univerzálisan egyedi azonosító (UUID) hello virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-168">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="a5fe2-169">A jelen rövid hibaelhárítási esetben csatlakoztatását a hello virtuális merevlemez segítségével hello UUID nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-169">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="a5fe2-170">Azonban a normál használja, a Szerkesztés `/etc/fstab` toomount virtuális merevlemezek helyett eszköznév UUID okozhat hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-170">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="a5fe2-171">Az eredeti virtuális merevlemez kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="a5fe2-171">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="a5fe2-172">Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-172">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="a5fe2-173">Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-173">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="a5fe2-174">Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="a5fe2-174">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="a5fe2-175">Ha a hibák fakadó problémák megoldásával válassza le, és hello meglévő virtuális merevlemez válassza le a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-175">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="a5fe2-176">Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-176">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="a5fe2-177">A hello a virtuális gép hibakeresési SSH munkamenet tooyour leválasztása hello létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-177">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="a5fe2-178">Először kívül hello szülőkönyvtárában a csatlakoztatási pont módosítása:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-178">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="a5fe2-179">Most leválasztása hello létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-179">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="a5fe2-180">hello alábbi példa leválasztja hello eszközéből `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-180">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="a5fe2-181">Most válassza le a hello hello virtuális gép a virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-181">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="a5fe2-182">Lépjen ki a virtuális gép hibakeresési hello SSH-munkamenet tooyour.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-182">Exit hello SSH session tooyour troubleshooting VM.</span></span> <span data-ttu-id="a5fe2-183">Lista hello csatolt virtuális gép és hibaelhárítási adatokat lemezek tooyour [az virtuálisgép-lemez nem felügyelt lista](/cli/azure/vm/unmanaged-disk#list).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-183">List hello attached data disks tooyour troubleshooting VM with [az vm unmanaged-disk list](/cli/azure/vm/unmanaged-disk#list).</span></span> <span data-ttu-id="a5fe2-184">hello alábbi példa listák hello adatlemezt csatolni toohello nevű virtuális gép `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-184">hello following example lists hello data disks attached toohello VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    <span data-ttu-id="a5fe2-185">Vegye figyelembe a meglévő virtuális merevlemez hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-185">Note hello name for your existing virtual hard disk.</span></span> <span data-ttu-id="a5fe2-186">Például egy lemez neve hello hello URI-jának **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** van **myVHD**.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-186">For example, hello name of a disk with hello URI of **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**.</span></span> 

    <span data-ttu-id="a5fe2-187">Válassza le a virtuális gép hello adatlemezét [nem felügyelt az virtuálisgép-lemez leválasztása](/cli/azure/vm/unmanaged-disk#detach).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-187">Detach hello data disk from your VM [az vm unmanaged-disk detach](/cli/azure/vm/unmanaged-disk#detach).</span></span> <span data-ttu-id="a5fe2-188">hello alábbi példa leválasztja nevű hello lemez `myVHD` hello nevű virtuális gép a `myVMRecovery` a hello `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-188">hello following example detaches hello disk named `myVHD` from hello VM named `myVMRecovery` in hello `myResourceGroup` resource group:</span></span>

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="a5fe2-189">Virtuális gép eredeti merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5fe2-189">Create VM from original hard disk</span></span>
<span data-ttu-id="a5fe2-190">toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-190">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="a5fe2-191">hello tényleges JSON-sablon jelenleg a következő hivatkozás hello:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-191">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="a5fe2-192">https://RAW.githubusercontent.com/Azure/Azure-quickstart-Templates/Master/201-VM-specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="a5fe2-192">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="a5fe2-193">hello sablon telepíti a virtuális gépek virtuális merevlemez URI hello hello a korábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-193">hello template deploys a VM using hello VHD URI from hello earlier command.</span></span> <span data-ttu-id="a5fe2-194">A hello sablon üzembe helyezése [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-194">Deploy hello template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="a5fe2-195">Adja meg a hello URI tooyour eredeti VHD és adja meg hello az operációs rendszer típusát, a Virtuálisgép-méretet és a virtuális gép nevét az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-195">Provide hello URI tooyour original VHD and then specify hello OS type, VM size, and VM name as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="a5fe2-196">Engedélyezze újra a rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="a5fe2-196">Re-enable boot diagnostics</span></span>
<span data-ttu-id="a5fe2-197">Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="a5fe2-197">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="a5fe2-198">A rendszerindítási diagnosztika engedélyezése [az virtuális gép rendszerindítási-diagnosztika engedélyezése](/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-198">Enable boot diagnostics with [az vm boot-diagnostics enable](/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="a5fe2-199">hello következő példa engedélyezi hello hello nevű virtuális gép diagnosztikai kiterjesztés `myDeployedVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="a5fe2-199">hello following example enables hello diagnostic extension on hello VM named `myDeployedVM` in hello resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="a5fe2-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5fe2-200">Next steps</span></span>
<span data-ttu-id="a5fe2-201">Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooan Azure virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-201">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="a5fe2-202">A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Linux virtuális gép](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a5fe2-202">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
