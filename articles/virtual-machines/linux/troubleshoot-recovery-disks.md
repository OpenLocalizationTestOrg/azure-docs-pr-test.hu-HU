---
title: "A Linux virtuális gép és az Azure CLI 2.0 hibaelhárítási használata |} Microsoft Docs"
description: "Ismerje meg, az operációs rendszer lemezének csatlakozva egy helyreállítási virtuális gépet az Azure CLI 2.0 Linux virtuális gép kapcsolatos problémák elhárítása"
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
ms.openlocfilehash: 7a28accce1bd328b2b486b588c44d91b03e42122
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-the-os-disk-to-a-recovery-vm-with-the-azure-cli-20"></a><span data-ttu-id="66d7a-103">Linux virtuális gép által a operációsrendszer-lemez csatolása a helyreállítási virtuális Gépet az Azure CLI 2.0 hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="66d7a-103">Troubleshoot a Linux VM by attaching the OS disk to a recovery VM with the Azure CLI 2.0</span></span>
<span data-ttu-id="66d7a-104">Ha a Linux virtuális gép (VM) rendszerindító vagy a lemez hibát tapasztal, szükség lehet végezze el a virtuális merevlemez hibaelhárítási lépéseket.</span><span class="sxs-lookup"><span data-stu-id="66d7a-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need to perform troubleshooting steps on the virtual hard disk itself.</span></span> <span data-ttu-id="66d7a-105">Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab` , amely megakadályozza a virtuális gép rendszerindító sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="66d7a-105">A common example would be an invalid entry in `/etc/fstab` that prevents the VM from being able to boot successfully.</span></span> <span data-ttu-id="66d7a-106">Ez a cikk részletezi az Azure CLI 2.0 másik Linux virtuális gép, javítsa ki a hibákat, majd hozza létre újból az eredeti virtuális gép csatlakozni a virtuális merevlemez használata.</span><span class="sxs-lookup"><span data-stu-id="66d7a-106">This article details how to use the Azure CLI 2.0 to connect your virtual hard disk to another Linux VM to fix any errors, then re-create your original VM.</span></span> <span data-ttu-id="66d7a-107">Az [Azure CLI 1.0-s](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="66d7a-107">You can also perform these steps with the [Azure CLI 1.0](troubleshoot-recovery-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="66d7a-108">Helyreállítási folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="66d7a-108">Recovery process overview</span></span>
<span data-ttu-id="66d7a-109">A hibaelhárítási folyamat a következő:</span><span class="sxs-lookup"><span data-stu-id="66d7a-109">The troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="66d7a-110">Törölje a virtuális gép problémák észlelése, a virtuális merevlemezek tartása.</span><span class="sxs-lookup"><span data-stu-id="66d7a-110">Delete the VM encountering issues, keeping the virtual hard disks.</span></span>
2. <span data-ttu-id="66d7a-111">Csatolja, és csatlakoztassa a virtuális merevlemez egy másik Linux VM hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="66d7a-111">Attach and mount the virtual hard disk to another Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="66d7a-112">Kapcsolódjon a hibaelhárítást végző virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="66d7a-112">Connect to the troubleshooting VM.</span></span> <span data-ttu-id="66d7a-113">Szerkesztheti a fájlokat, vagy futtassa a problémák megoldásával kapcsolatban az eredeti virtuális merevlemez olyan eszközöket.</span><span class="sxs-lookup"><span data-stu-id="66d7a-113">Edit files or run any tools to fix issues on the original virtual hard disk.</span></span>
4. <span data-ttu-id="66d7a-114">Válassza le a virtuális merevlemezt a hibaelhárító virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="66d7a-114">Unmount and detach the virtual hard disk from the troubleshooting VM.</span></span>
5. <span data-ttu-id="66d7a-115">Az eredeti virtuális merevlemez virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="66d7a-115">Create a VM using the original virtual hard disk.</span></span>

<span data-ttu-id="66d7a-116">Hajtsa végre az alábbi lépéseket, a legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="66d7a-116">To perform these troubleshooting steps, you need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="66d7a-117">A következő példákban cserélje le a saját értékeit paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="66d7a-117">In the following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="66d7a-118">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="66d7a-118">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="66d7a-119">Határozza meg a rendszerindítási problémák</span><span class="sxs-lookup"><span data-stu-id="66d7a-119">Determine boot issues</span></span>
<span data-ttu-id="66d7a-120">Vizsgálja meg a soros kimenete annak megállapításához, miért a virtuális gép nem végezhetnek rendszerindítást megfelelően.</span><span class="sxs-lookup"><span data-stu-id="66d7a-120">Examine the serial output to determine why your VM is not able to boot correctly.</span></span> <span data-ttu-id="66d7a-121">Ilyenek például a bejegyzés érvénytelen `/etc/fstab`, vagy az alapul szolgáló virtuális merevlemezek áthelyezése vagy törölhetők.</span><span class="sxs-lookup"><span data-stu-id="66d7a-121">A common example is an invalid entry in `/etc/fstab`, or the underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="66d7a-122">A rendszerindító naplófájlok rendelkező [az virtuális gép rendszerindítási-diagnosztika get-rendszerindítási-naplófájl](/cli/azure/vm/boot-diagnostics#get-boot-log).</span><span class="sxs-lookup"><span data-stu-id="66d7a-122">Get the boot logs with [az vm boot-diagnostics get-boot-log](/cli/azure/vm/boot-diagnostics#get-boot-log).</span></span> <span data-ttu-id="66d7a-123">Az alábbi példa lekérdezi a soros kimeneti nevű virtuális gép `myVM` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-123">The following example gets the serial output from the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics get-boot-log --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="66d7a-124">Tekintse át a soros kimenetet meghatározni, miért a virtuális gép rendszerindító sikertelenek lesznek.</span><span class="sxs-lookup"><span data-stu-id="66d7a-124">Review the serial output to determine why the VM is failing to boot.</span></span> <span data-ttu-id="66d7a-125">A soros kimeneti bármely arra utal, hogy nem ad meg, ha szeretne nézze meg a naplófájlokat `/var/log` után a virtuális merevlemez, egy hibaelhárítási Virtuálisgép kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="66d7a-125">If the serial output isn't providing any indication, you may need to review log files in `/var/log` once you have the virtual hard disk connected to a troubleshooting VM.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="66d7a-126">Meglévő virtuális merevlemez részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="66d7a-126">View existing virtual hard disk details</span></span>
<span data-ttu-id="66d7a-127">A virtuális merevlemez (VHD) egy másik virtuális gép csatolhat, mielőtt kell azonosítani az URI-azonosítója az operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="66d7a-127">Before you can attach your virtual hard disk (VHD) to another VM, you need to identify the URI of the OS disk.</span></span> 

<span data-ttu-id="66d7a-128">Tekintse meg a virtuális Gépet a [az vm megjelenítése](/cli/azure/vm#show).</span><span class="sxs-lookup"><span data-stu-id="66d7a-128">View information about your VM with [az vm show](/cli/azure/vm#show).</span></span> <span data-ttu-id="66d7a-129">Használja a `--query` jelzőjét, hogy az URI-t az operációs rendszer lemezének kibontásához.</span><span class="sxs-lookup"><span data-stu-id="66d7a-129">Use the `--query` flag to extract the URI to the OS disk.</span></span> <span data-ttu-id="66d7a-130">Az alábbi példa lekérdezi a nevű virtuális gép lemezeinek adatai `myVM` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-130">The following example gets disk information for the VM named `myVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm show --resource-group myResourceGroup --name myVM \
    --query [storageProfile.osDisk.vhd.uri] --output tsv
```

<span data-ttu-id="66d7a-131">Az URI hasonlít **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span><span class="sxs-lookup"><span data-stu-id="66d7a-131">The URI is similar to **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd**.</span></span>

## <a name="delete-existing-vm"></a><span data-ttu-id="66d7a-132">Meglévő virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="66d7a-132">Delete existing VM</span></span>
<span data-ttu-id="66d7a-133">A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa.</span><span class="sxs-lookup"><span data-stu-id="66d7a-133">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="66d7a-134">A virtuális merevlemez, az operációs rendszert illeti, alkalmazások és konfigurációk tárolására.</span><span class="sxs-lookup"><span data-stu-id="66d7a-134">A virtual hard disk is where the operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="66d7a-135">A virtuális gép magát a csak metaadatokat, amelyek a méretét vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="66d7a-135">The VM itself is just metadata that defines the size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="66d7a-136">Minden virtuális merevlemez létrehozásakor kell a virtuális Géphez csatlakozik, a címbérlet rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="66d7a-136">Each virtual hard disk has a lease assigned when attached to a VM.</span></span> <span data-ttu-id="66d7a-137">Bár az adatlemezek akkor is csatlakoztathatók és leválaszthatók, amikor a virtuális gép üzemel, az operációs rendszer merevlemeze nem csatlakoztatható le, hacsak nem törli a VM-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="66d7a-137">Although data disks can be attached and detached even while the VM is running, the OS disk cannot be detached unless the VM resource is deleted.</span></span> <span data-ttu-id="66d7a-138">A bérlet továbbra is fennáll, az operációs rendszer lemezének társítandó egy virtuális Gépet, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.</span><span class="sxs-lookup"><span data-stu-id="66d7a-138">The lease continues to associate the OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="66d7a-139">Az első lépés a virtuális gép helyreállításához, hogy törli a virtuális gép erőforrásához magát.</span><span class="sxs-lookup"><span data-stu-id="66d7a-139">The first step to recover your VM is to delete the VM resource itself.</span></span> <span data-ttu-id="66d7a-140">A virtuális gép törlésével a virtuális merevlemezek a tárfiókban maradnak.</span><span class="sxs-lookup"><span data-stu-id="66d7a-140">Deleting the VM leaves the virtual hard disks in your storage account.</span></span> <span data-ttu-id="66d7a-141">A virtuális gép törlését követően a virtuális merevlemez csatlakoztatása egy másik virtuális géphez, és javítsa ki a hibákat.</span><span class="sxs-lookup"><span data-stu-id="66d7a-141">After the VM is deleted, you attach the virtual hard disk to another VM to troubleshoot and resolve the errors.</span></span>

<span data-ttu-id="66d7a-142">Törölje a virtuális Géphez a [az vm törlése](/cli/azure/vm#delete).</span><span class="sxs-lookup"><span data-stu-id="66d7a-142">Delete the VM with [az vm delete](/cli/azure/vm#delete).</span></span> <span data-ttu-id="66d7a-143">A következő példa törli a virtuális gép nevű `myVM` az erőforráscsoportból nevű `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-143">The following example deletes the VM named `myVM` from the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm delete --resource-group myResourceGroup --name myVM 
```

<span data-ttu-id="66d7a-144">Várjon, amíg a virtuális gép törlése a virtuális merevlemez egy másik virtuális géphez csatolása előtt befejeződött.</span><span class="sxs-lookup"><span data-stu-id="66d7a-144">Wait until the VM has finished deleting before you attach the virtual hard disk to another VM.</span></span> <span data-ttu-id="66d7a-145">A virtuális merevlemezen, amely a virtuális Gépet társít a címbérlet kell helyezni, előtt a virtuális merevlemez egy másik virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="66d7a-145">The lease on the virtual hard disk that associates it with the VM needs to be released before you can attach the virtual hard disk to another VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a><span data-ttu-id="66d7a-146">Meglévő virtuális merevlemez egy másik virtuális géphez csatolása</span><span class="sxs-lookup"><span data-stu-id="66d7a-146">Attach existing virtual hard disk to another VM</span></span>
<span data-ttu-id="66d7a-147">A következő néhány lépést, a másik virtuális gép a hibaelhárításhoz használja.</span><span class="sxs-lookup"><span data-stu-id="66d7a-147">For the next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="66d7a-148">A meglévő virtuális merevlemez csatlakoztatása a hibaelhárítási virtuális géppel, keresse meg és a lemez tartalmának szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="66d7a-148">You attach the existing virtual hard disk to this troubleshooting VM to browse and edit the disk's content.</span></span> <span data-ttu-id="66d7a-149">Ez a folyamat teszi javíthatja az esetleges konfigurációs hibákat, vagy tekintse át például további alkalmazás vagy a rendszer naplófájljait.</span><span class="sxs-lookup"><span data-stu-id="66d7a-149">This process allows you to correct any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="66d7a-150">Válassza ki vagy hozzon létre egy másik virtuális Gépet a hibaelhárításhoz használja.</span><span class="sxs-lookup"><span data-stu-id="66d7a-150">Choose or create another VM to use for troubleshooting purposes.</span></span>

<span data-ttu-id="66d7a-151">A meglévő virtuális merevlemez csatlakoztatása [nem felügyelt az virtuálisgép-lemez csatolása](/cli/azure/vm/unmanaged-disk#attach).</span><span class="sxs-lookup"><span data-stu-id="66d7a-151">Attach the existing virtual hard disk with [az vm unmanaged-disk attach](/cli/azure/vm/unmanaged-disk#attach).</span></span> <span data-ttu-id="66d7a-152">A meglévő virtuális merevlemez csatolása, adja meg az URI-t a lemezt az előző kapott `az vm show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="66d7a-152">When you attach the existing virtual hard disk, specify the URI to the disk obtained in the preceding `az vm show` command.</span></span> <span data-ttu-id="66d7a-153">Az alábbi példa csatolja a létező virtuális merevlemezt a hibaelhárítási nevű virtuális gép `myVMRecovery` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-153">The following example attaches an existing virtual hard disk to the troubleshooting VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm unmanaged-disk attach --resource-group myResourceGroup --vm-name myVMRecovery \
    --vhd-uri https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-the-attached-data-disk"></a><span data-ttu-id="66d7a-154">A csatolt adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="66d7a-154">Mount the attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="66d7a-155">A következő példák részletesen az Ubuntu virtuális gép indításához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="66d7a-155">The following examples detail the steps required on an Ubuntu VM.</span></span> <span data-ttu-id="66d7a-156">Red Hat Enterprise Linux és SUSE, például a különböző Linux distro használata a naplófájl helyét és `mount` parancsok kissé eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="66d7a-156">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, the log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="66d7a-157">Tekintse meg az adott distro a parancsok a megfelelő változásokat a dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="66d7a-157">Refer to the documentation for your specific distro for the appropriate changes in commands.</span></span>

1. <span data-ttu-id="66d7a-158">SSH-kapcsolatot a hibaelhárítási virtuális Gépet a megfelelő hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="66d7a-158">SSH to your troubleshooting VM using the appropriate credentials.</span></span> <span data-ttu-id="66d7a-159">Ha ezt a lemezt az első adatok lemezt, a hibaelhárítási Virtuálisgép kapcsolódik, a lemez valószínűleg csatlakozik-e `/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="66d7a-159">If this disk is the first data disk attached to your troubleshooting VM, the disk is likely connected to `/dev/sdc`.</span></span> <span data-ttu-id="66d7a-160">Használjon `dmseg` csatlakoztatott lemezek megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="66d7a-160">Use `dmseg` to view attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```

    <span data-ttu-id="66d7a-161">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="66d7a-161">The output is similar to the following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="66d7a-162">A fenti példában az operációsrendszer-lemezképet jelenleg `/dev/sda` és az egyes virtuális gépek a megadott ideiglenes lemez `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="66d7a-162">In the preceding example, the OS disk is at `/dev/sda` and the temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="66d7a-163">Ha több adatlemezek, hogy legyen `/dev/sdd`, `/dev/sde`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="66d7a-163">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="66d7a-164">Hozzon létre egy könyvtárat, a meglévő virtuális merevlemez csatlakoztatása.</span><span class="sxs-lookup"><span data-stu-id="66d7a-164">Create a directory to mount your existing virtual hard disk.</span></span> <span data-ttu-id="66d7a-165">Az alábbi példa létrehoz egy könyvtárat nevű `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-165">The following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="66d7a-166">Ha több partíciót a meglévő virtuális merevlemezen, csatlakoztassa a szükséges partíciót.</span><span class="sxs-lookup"><span data-stu-id="66d7a-166">If you have multiple partitions on your existing virtual hard disk, mount the required partition.</span></span> <span data-ttu-id="66d7a-167">Az alábbi példa csatlakoztatja, az első elsődleges partíció `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-167">The following example mounts the first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="66d7a-168">Ajánlott eljárás az adatlemezek csatlakoztatása az Azure-ban univerzálisan egyedi azonosítóját (UUID) a virtuális merevlemez virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="66d7a-168">Best practice is to mount data disks on VMs in Azure using the universally unique identifier (UUID) of the virtual hard disk.</span></span> <span data-ttu-id="66d7a-169">A jelen rövid hibaelhárítási esetben csatlakoztatni a virtuális merevlemez használatával UUID nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="66d7a-169">For this short troubleshooting scenario, mounting the virtual hard disk using the UUID is not necessary.</span></span> <span data-ttu-id="66d7a-170">Azonban a normál használja, a Szerkesztés `/etc/fstab` csatlakoztatni a virtuális merevlemezek UUID helyett eszköznév okozhat a virtuális gépek a rendszerindítás.</span><span class="sxs-lookup"><span data-stu-id="66d7a-170">However, under normal use, editing `/etc/fstab` to mount virtual hard disks using device name rather than UUID may cause the VM to fail to boot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="66d7a-171">Az eredeti virtuális merevlemez kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="66d7a-171">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="66d7a-172">A meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="66d7a-172">With the existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="66d7a-173">Miután végzett a hibák javításával, folytassa az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="66d7a-173">Once you have addressed the issues, continue with the following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="66d7a-174">Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="66d7a-174">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="66d7a-175">Ha a hibák fakadó problémák megoldásával válassza le, és a meglévő virtuális merevlemez válassza le a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="66d7a-175">Once your errors are resolved, you unmount and detach the existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="66d7a-176">A virtuális merevlemez nem használható a többi virtuális Géphez, amíg a címbérlet, a virtuális merevlemez csatolását a hibaelhárítási VM.</span><span class="sxs-lookup"><span data-stu-id="66d7a-176">You cannot use your virtual hard disk with any other VM until the lease attaching the virtual hard disk to the troubleshooting VM is released.</span></span>

1. <span data-ttu-id="66d7a-177">Az SSH-munkamenetet a hibaelhárítási virtuális gépre, a leválasztani a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="66d7a-177">From the SSH session to your troubleshooting VM, unmount the existing virtual hard disk.</span></span> <span data-ttu-id="66d7a-178">Először módosítsa a csatlakoztatási pont szülőkönyvtárában kívül:</span><span class="sxs-lookup"><span data-stu-id="66d7a-178">Change out of the parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="66d7a-179">Most leválasztani a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="66d7a-179">Now unmount the existing virtual hard disk.</span></span> <span data-ttu-id="66d7a-180">Az alábbi példa leválasztja az eszköz `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-180">The following example unmounts the device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="66d7a-181">Most válassza le a virtuális merevlemezt a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="66d7a-181">Now detach the virtual hard disk from the VM.</span></span> <span data-ttu-id="66d7a-182">Kilépés az SSH-munkamenetet a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="66d7a-182">Exit the SSH session to your troubleshooting VM.</span></span> <span data-ttu-id="66d7a-183">A mellékelt adatok a hibaelhárítási tulajdonsággal rendelkező virtuális lemezein listában [az virtuálisgép-lemez nem felügyelt lista](/cli/azure/vm/unmanaged-disk#list).</span><span class="sxs-lookup"><span data-stu-id="66d7a-183">List the attached data disks to your troubleshooting VM with [az vm unmanaged-disk list](/cli/azure/vm/unmanaged-disk#list).</span></span> <span data-ttu-id="66d7a-184">Az alábbi példa felsorolja a adatok nevű virtuális gép csatlakoztatott lemezei `myVMRecovery` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-184">The following example lists the data disks attached to the VM named `myVMRecovery` in the resource group named `myResourceGroup`:</span></span>

    ```azurecli
    azure vm unmanaged-disk list --resource-group myResourceGroup --vm-name myVMRecovery \
        --query '[].{Disk:vhd.uri}' --output table
    ```

    <span data-ttu-id="66d7a-185">Vegye figyelembe a meglévő virtuális merevlemez nevét.</span><span class="sxs-lookup"><span data-stu-id="66d7a-185">Note the name for your existing virtual hard disk.</span></span> <span data-ttu-id="66d7a-186">Például az egy lemez neve az URI-azonosítójú **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** van **myVHD**.</span><span class="sxs-lookup"><span data-stu-id="66d7a-186">For example, the name of a disk with the URI of **https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd** is **myVHD**.</span></span> 

    <span data-ttu-id="66d7a-187">Válassza le a lemezt a virtuális gép adatok [nem felügyelt az virtuálisgép-lemez leválasztása](/cli/azure/vm/unmanaged-disk#detach).</span><span class="sxs-lookup"><span data-stu-id="66d7a-187">Detach the data disk from your VM [az vm unmanaged-disk detach](/cli/azure/vm/unmanaged-disk#detach).</span></span> <span data-ttu-id="66d7a-188">Az alábbi példa leválasztja a lemezt nevű `myVHD` nevű virtuális gép `myVMRecovery` a a `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="66d7a-188">The following example detaches the disk named `myVHD` from the VM named `myVMRecovery` in the `myResourceGroup` resource group:</span></span>

    ```azurecli
    az vm unmanaged-disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --name myVHD
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="66d7a-189">Virtuális gép eredeti merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="66d7a-189">Create VM from original hard disk</span></span>
<span data-ttu-id="66d7a-190">Egy virtuális Gépet hozhat létre az eredeti virtuális merevlemez [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="66d7a-190">To create a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd).</span></span> <span data-ttu-id="66d7a-191">A tényleges JSON-sablon jelenleg a következő hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="66d7a-191">The actual JSON template is at the following link:</span></span>

- <span data-ttu-id="66d7a-192">https://RAW.githubusercontent.com/Azure/Azure-quickstart-Templates/Master/201-VM-specialized-VHD/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="66d7a-192">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json</span></span>

<span data-ttu-id="66d7a-193">A sablont egy virtuális Gépet a virtuális merevlemez URI a korábbi parancs használatával telepíti.</span><span class="sxs-lookup"><span data-stu-id="66d7a-193">The template deploys a VM using the VHD URI from the earlier command.</span></span> <span data-ttu-id="66d7a-194">A sablon telepítéséhez [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="66d7a-194">Deploy the template with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="66d7a-195">Adja meg az URI az eredeti virtuális Merevlemezt, majd adja meg az operációsrendszer-típus, a Virtuálisgép-méretet és a virtuális gép nevét az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="66d7a-195">Provide the URI to your original VHD and then specify the OS type, VM size, and VM name as follows:</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeployment \
  --parameters '{"osDiskVhdUri": {"value": "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"},
    "osType": {"value": "Linux"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myDeployedVM"}}' \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="66d7a-196">Engedélyezze újra a rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="66d7a-196">Re-enable boot diagnostics</span></span>
<span data-ttu-id="66d7a-197">Amikor a virtuális Gépet hoz létre a meglévő virtuális merevlemez, rendszerindítási diagnosztika automatikusan nem lehet engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="66d7a-197">When you create your VM from the existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="66d7a-198">A rendszerindítási diagnosztika engedélyezése [az virtuális gép rendszerindítási-diagnosztika engedélyezése](/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="66d7a-198">Enable boot diagnostics with [az vm boot-diagnostics enable](/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="66d7a-199">A következő példában engedélyezzük a diagnosztikai kiterjesztés nevű virtuális gép `myDeployedVM` az erőforráscsoport neve `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="66d7a-199">The following example enables the diagnostic extension on the VM named `myDeployedVM` in the resource group named `myResourceGroup`:</span></span>

```azurecli
az vm boot-diagnostics enable --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a><span data-ttu-id="66d7a-200">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66d7a-200">Next steps</span></span>
<span data-ttu-id="66d7a-201">Ha a virtuális Géphez való kapcsolódás problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok egy Azure virtuális gépre](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="66d7a-201">If you are having issues connecting to your VM, see [Troubleshoot SSH connections to an Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="66d7a-202">A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Linux virtuális gép](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="66d7a-202">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>