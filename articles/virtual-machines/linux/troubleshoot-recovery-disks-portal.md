---
title: "a Linux virtuális gép hibaelhárítás hello Azure-portálon a aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot Linux virtuális gépekkel kapcsolatos problémákat használatával csatlakozó hello OS lemez tooa helyreállítási virtuális gép hello Azure-portálon"
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
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 794daa06d7436215af84a61ab9088524254c47df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a><span data-ttu-id="6a79e-103">Linux virtuális gép hibaelhárításáról hello OS lemez tooa helyreállítási virtuális gép csatlakoztatása az Azure portál használatával hello</span><span class="sxs-lookup"><span data-stu-id="6a79e-103">Troubleshoot a Linux VM by attaching hello OS disk tooa recovery VM using hello Azure portal</span></span>
<span data-ttu-id="6a79e-104">Ha a Linux virtuális gép (VM) rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát.</span><span class="sxs-lookup"><span data-stu-id="6a79e-104">If your Linux virtual machine (VM) encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="6a79e-105">Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab` , amely megakadályozza hello méretű képes tooboot sikeresen.</span><span class="sxs-lookup"><span data-stu-id="6a79e-105">A common example would be an invalid entry in `/etc/fstab` that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="6a79e-106">Ez a cikk részletek hogyan toouse hello Azure portál tooconnect a virtuális merevlemez lemez tooanother Linux virtuális gép toofix ki a hibákat, majd hozza létre újból az eredeti virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6a79e-106">This article details how toouse hello Azure portal tooconnect your virtual hard disk tooanother Linux VM toofix any errors, then re-create your original VM.</span></span>

## <a name="recovery-process-overview"></a><span data-ttu-id="6a79e-107">Helyreállítási folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="6a79e-107">Recovery process overview</span></span>
<span data-ttu-id="6a79e-108">hibaelhárítási folyamatának hello a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="6a79e-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="6a79e-109">Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.</span><span class="sxs-lookup"><span data-stu-id="6a79e-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="6a79e-110">Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Linux virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="6a79e-110">Attach and mount hello virtual hard disk tooanother Linux VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="6a79e-111">Csatlakoztassa a VM hibaelhárítási toohello.</span><span class="sxs-lookup"><span data-stu-id="6a79e-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="6a79e-112">Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="6a79e-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="6a79e-113">Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="6a79e-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="6a79e-114">Hello eredeti virtuális merevlemez virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6a79e-114">Create a VM using hello original virtual hard disk.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="6a79e-115">Határozza meg a rendszerindítási problémák</span><span class="sxs-lookup"><span data-stu-id="6a79e-115">Determine boot issues</span></span>
<span data-ttu-id="6a79e-116">Ellenőrizze a hello rendszerindítási diagnosztika és a virtuális gép képernyőkép toodetermine miért a virtuális gép nem tud tooboot megfelelően.</span><span class="sxs-lookup"><span data-stu-id="6a79e-116">Examine hello boot diagnostics and VM screenshot toodetermine why your VM is not able tooboot correctly.</span></span> <span data-ttu-id="6a79e-117">Ilyenek például a bejegyzés érvénytelen lenne `/etc/fstab`, vagy egy alapul szolgáló virtuális merevlemezek áthelyezése vagy törölhetők.</span><span class="sxs-lookup"><span data-stu-id="6a79e-117">A common example would be an invalid entry in `/etc/fstab`, or an underlying virtual hard disk being deleted or moved.</span></span>

<span data-ttu-id="6a79e-118">Válassza ki a virtuális gép hello portálon, és görgessen lefelé toohello **támogatási + hibaelhárítás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="6a79e-118">Select your VM in hello portal and then scroll down toohello **Support + Troubleshooting** section.</span></span> <span data-ttu-id="6a79e-119">Kattintson a **rendszerindítási diagnosztika** tooview konzol köszönőüzenetei folyamatos átviteli a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="6a79e-119">Click **Boot diagnostics** tooview hello console messages streamed from your VM.</span></span> <span data-ttu-id="6a79e-120">Felülvizsgálati hello-konzol naplói toosee, ha azt is meghatározhatja, miért hello VM kapcsolatban felmerült problémát.</span><span class="sxs-lookup"><span data-stu-id="6a79e-120">Review hello console logs toosee if you can determine why hello VM is encountering an issue.</span></span> <span data-ttu-id="6a79e-121">hello alábbi példa bemutatja a virtuális gépek elakadt a karbantartási módba manuális beavatkozásra van szükség:</span><span class="sxs-lookup"><span data-stu-id="6a79e-121">hello following example shows a VM stuck in maintenance mode that requires manual interaction:</span></span>

![Megtekintés a virtuális gép rendszerindítási diagnosztika konzol naplók](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

<span data-ttu-id="6a79e-123">Is **képernyőkép** hello felső részén hello rendszerindítási diagnosztika napló toodownload egy hello VM képernyőkép rögzítése között.</span><span class="sxs-lookup"><span data-stu-id="6a79e-123">You can also click **Screenshot** across hello top of hello boot diagnostics log toodownload a capture of hello VM screenshot.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="6a79e-124">Meglévő virtuális merevlemez részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="6a79e-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="6a79e-125">A virtuális merevlemez tooanother VM csatolhat, meg kell tooidentify hello neve hello virtuális merevlemez (VHD).</span><span class="sxs-lookup"><span data-stu-id="6a79e-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span> 

<span data-ttu-id="6a79e-126">Válassza ki az erőforráscsoport hello portálról, majd válassza ki a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="6a79e-126">Select your resource group from hello portal, then select your storage account.</span></span> <span data-ttu-id="6a79e-127">Kattintson a **Blobok**, a példában a következő hello:</span><span class="sxs-lookup"><span data-stu-id="6a79e-127">Click **Blobs**, as in hello following example:</span></span>

![Válassza ki a tárolási BLOB](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

<span data-ttu-id="6a79e-129">Általában akkor nevű tárolót **VHD-k** , amely a virtuális merevlemezeket tárolja.</span><span class="sxs-lookup"><span data-stu-id="6a79e-129">Typically you have a container named **vhds** that stores your virtual hard disks.</span></span> <span data-ttu-id="6a79e-130">Válassza ki a hello tároló tooview virtuális merevlemezek listáját.</span><span class="sxs-lookup"><span data-stu-id="6a79e-130">Select hello container tooview a list of virtual hard disks.</span></span> <span data-ttu-id="6a79e-131">Megjegyzés: hello nevet a virtuális merevlemez (hello előtag az általában a virtuális gép nevét hello):</span><span class="sxs-lookup"><span data-stu-id="6a79e-131">Note hello name of your VHD (hello prefix is usually hello name of your VM):</span></span>

![A tároló virtuális merevlemez azonosítása](./media/troubleshoot-recovery-disks-portal/storage-container.png)

<span data-ttu-id="6a79e-133">Válassza ki a meglévő virtuális merevlemez hello listából, és másolja a hello URL-CÍMÉT használja a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="6a79e-133">Select your existing virtual hard disk from hello list and copy hello URL for use in hello following steps:</span></span>

![Meglévő virtuális merevlemez URL-Címének másolása](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a><span data-ttu-id="6a79e-135">Meglévő virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="6a79e-135">Delete existing VM</span></span>
<span data-ttu-id="6a79e-136">A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa.</span><span class="sxs-lookup"><span data-stu-id="6a79e-136">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="6a79e-137">A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására.</span><span class="sxs-lookup"><span data-stu-id="6a79e-137">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="6a79e-138">hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="6a79e-138">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="6a79e-139">Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6a79e-139">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="6a79e-140">Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik.</span><span class="sxs-lookup"><span data-stu-id="6a79e-140">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="6a79e-141">hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.</span><span class="sxs-lookup"><span data-stu-id="6a79e-141">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="6a79e-142">első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="6a79e-142">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="6a79e-143">Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="6a79e-143">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="6a79e-144">Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.</span><span class="sxs-lookup"><span data-stu-id="6a79e-144">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="6a79e-145">A virtuális gép hello portálon válassza **törlése**:</span><span class="sxs-lookup"><span data-stu-id="6a79e-145">Select your VM in hello portal, then click **Delete**:</span></span>

![Virtuális gép rendszerindítási diagnosztika képernyőfelvétel rendszerindítási hiba](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

<span data-ttu-id="6a79e-147">Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6a79e-147">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="6a79e-148">hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="6a79e-148">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="6a79e-149">Meglévő virtuális merevlemez tooanother VM csatolása</span><span class="sxs-lookup"><span data-stu-id="6a79e-149">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="6a79e-150">A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="6a79e-150">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="6a79e-151">Hello VM toobe képes toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a79e-151">You attach hello existing virtual hard disk toothis troubleshooting VM toobe able toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="6a79e-152">Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például.</span><span class="sxs-lookup"><span data-stu-id="6a79e-152">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="6a79e-153">Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="6a79e-153">Choose or create another VM toouse for troubleshooting purposes.</span></span>

1. <span data-ttu-id="6a79e-154">Válassza ki az erőforráscsoport hello portálról, majd válassza ki a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="6a79e-154">Select your resource group from hello portal, then select your troubleshooting VM.</span></span> <span data-ttu-id="6a79e-155">Válassza ki **lemezek** majd **Csatolás meglévő**:</span><span class="sxs-lookup"><span data-stu-id="6a79e-155">Select **Disks** and then click **Attach existing**:</span></span>

    ![Hello portálon meglévő lemez csatolása](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. <span data-ttu-id="6a79e-157">tooselect a meglévő virtuális merevlemez kattintson **VHD-fájl**:</span><span class="sxs-lookup"><span data-stu-id="6a79e-157">tooselect your existing virtual hard disk, click **VHD File**:</span></span>

    ![Meglévő VHD keresése](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. <span data-ttu-id="6a79e-159">A tárfiók és tároló, majd kattintson a meglévő virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="6a79e-159">Select your storage account and container, then click your existing VHD.</span></span> <span data-ttu-id="6a79e-160">Kattintson a hello **válasszon** tooconfirm gomb szabadon választott:</span><span class="sxs-lookup"><span data-stu-id="6a79e-160">Click hello **Select** button tooconfirm your choice:</span></span>

    ![Meglévő VHD kiválasztása](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. <span data-ttu-id="6a79e-162">A most kijelölt virtuális merevlemez, és kattintson **OK** tooattach hello meglévő virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="6a79e-162">With your VHD now selected, click **OK** tooattach hello existing virtual hard disk:</span></span>

    ![Ellenőrizze a meglévő virtuális merevlemez csatlakoztatása](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. <span data-ttu-id="6a79e-164">Néhány másodperc múlva hello **lemezek** a virtuális gép ablaktábla listázza a meglévő virtuális merevlemez csatlakoztatva adatlemezt számára:</span><span class="sxs-lookup"><span data-stu-id="6a79e-164">After a few seconds, hello **Disks** pane for your VM lists your existing virtual hard disk connected as a data disk:</span></span>

    ![Adatlemezként csatlakoztatott meglévő virtuális merevlemez](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="6a79e-166">Hello csatolt adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="6a79e-166">Mount hello attached data disk</span></span>

> [!NOTE]
> <span data-ttu-id="6a79e-167">hello következő példák részletesen hello lépéseit egy Ubuntu virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6a79e-167">hello following examples detail hello steps required on an Ubuntu VM.</span></span> <span data-ttu-id="6a79e-168">Red Hat Enterprise Linux és SUSE, például a különböző Linux distro használata hello naplófájl helye és `mount` parancsok kissé eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="6a79e-168">If you are using a different Linux distro, such as Red Hat Enterprise Linux or SUSE, hello log file locations and `mount` commands may be a little different.</span></span> <span data-ttu-id="6a79e-169">A parancsok hello megfelelő változásokat az adott distro a tekintse meg a toohello dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="6a79e-169">Refer toohello documentation for your specific distro for hello appropriate changes in commands.</span></span>

1. <span data-ttu-id="6a79e-170">SSH tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="6a79e-170">SSH tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="6a79e-171">Ha a lemez hello első adatok csatlakoztatott lemez tooyour VM hibaelhárítási, valószínűleg csatlakoztatva van túl`/dev/sdc`.</span><span class="sxs-lookup"><span data-stu-id="6a79e-171">If this disk is hello first data disk attached tooyour troubleshooting VM, it is likely connected too`/dev/sdc`.</span></span> <span data-ttu-id="6a79e-172">Használjon `dmseg` toolist csatlakoztatott lemezekkel:</span><span class="sxs-lookup"><span data-stu-id="6a79e-172">Use `dmseg` toolist attached disks:</span></span>

    ```bash
    dmesg | grep SCSI
    ```
    <span data-ttu-id="6a79e-173">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="6a79e-173">hello output is similar toohello following example:</span></span>

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    <span data-ttu-id="6a79e-174">A fenti példa hello, hello operációsrendszer-lemez jelenleg `/dev/sda` hello ideiglenes lemez megadott összes virtuális Géphez és `/dev/sdb`.</span><span class="sxs-lookup"><span data-stu-id="6a79e-174">In hello preceding example, hello OS disk is at `/dev/sda` and hello temporary disk provided for each VM is at `/dev/sdb`.</span></span> <span data-ttu-id="6a79e-175">Ha több adatlemezek, hogy legyen `/dev/sdd`, `/dev/sde`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="6a79e-175">If you had multiple data disks, they should be at `/dev/sdd`, `/dev/sde`, and so on.</span></span>

2. <span data-ttu-id="6a79e-176">Hozzon létre egy könyvtárat toomount a meglévő virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="6a79e-176">Create a directory toomount your existing virtual hard disk.</span></span> <span data-ttu-id="6a79e-177">hello alábbi példa létrehoz egy könyvtárat nevű `troubleshootingdisk`:</span><span class="sxs-lookup"><span data-stu-id="6a79e-177">hello following example creates a directory named `troubleshootingdisk`:</span></span>

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. <span data-ttu-id="6a79e-178">Ha több partíciót a meglévő virtuális merevlemezen, csatlakoztassa a szükséges hello partíció.</span><span class="sxs-lookup"><span data-stu-id="6a79e-178">If you have multiple partitions on your existing virtual hard disk, mount hello required partition.</span></span> <span data-ttu-id="6a79e-179">hello alábbi példa csatlakoztatja, elsődleges partíció első hello `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="6a79e-179">hello following example mounts hello first primary partition at `/dev/sdc1`:</span></span>

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > <span data-ttu-id="6a79e-180">Ajánlott toomount adatlemezek virtuális gépeken Azure használatával hello univerzálisan egyedi azonosító (UUID) hello virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="6a79e-180">Best practice is toomount data disks on VMs in Azure using hello universally unique identifier (UUID) of hello virtual hard disk.</span></span> <span data-ttu-id="6a79e-181">A jelen rövid hibaelhárítási esetben csatlakoztatását a hello virtuális merevlemez segítségével hello UUID nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="6a79e-181">For this short troubleshooting scenario, mounting hello virtual hard disk using hello UUID is not necessary.</span></span> <span data-ttu-id="6a79e-182">Azonban a normál használja, a Szerkesztés `/etc/fstab` toomount virtuális merevlemezek helyett eszköznév UUID okozhat hello VM toofail tooboot.</span><span class="sxs-lookup"><span data-stu-id="6a79e-182">However, under normal use, editing `/etc/fstab` toomount virtual hard disks using device name rather than UUID may cause hello VM toofail tooboot.</span></span>


## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="6a79e-183">Az eredeti virtuális merevlemez kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="6a79e-183">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="6a79e-184">Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="6a79e-184">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="6a79e-185">Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="6a79e-185">Once you have addressed hello issues, continue with hello following steps.</span></span>

## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="6a79e-186">Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="6a79e-186">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="6a79e-187">Ha a hibák feloldása, válassza le hello meglévő virtuális merevlemezt a hibaelhárítási virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="6a79e-187">Once your errors are resolved, detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="6a79e-188">Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.</span><span class="sxs-lookup"><span data-stu-id="6a79e-188">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="6a79e-189">A hello a virtuális gép hibakeresési SSH munkamenet tooyour leválasztása hello létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="6a79e-189">From hello SSH session tooyour troubleshooting VM, unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="6a79e-190">Először kívül hello szülőkönyvtárában a csatlakoztatási pont módosítása:</span><span class="sxs-lookup"><span data-stu-id="6a79e-190">Change out of hello parent directory for your mount point first:</span></span>

    ```bash
    cd /
    ```

    <span data-ttu-id="6a79e-191">Most leválasztása hello létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="6a79e-191">Now unmount hello existing virtual hard disk.</span></span> <span data-ttu-id="6a79e-192">hello alábbi példa leválasztja hello eszközéből `/dev/sdc1`:</span><span class="sxs-lookup"><span data-stu-id="6a79e-192">hello following example unmounts hello device at `/dev/sdc1`:</span></span>

    ```bash
    sudo umount /dev/sdc1
    ```

2. <span data-ttu-id="6a79e-193">Most válassza le a hello hello virtuális gép a virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="6a79e-193">Now detach hello virtual hard disk from hello VM.</span></span> <span data-ttu-id="6a79e-194">Válassza ki a virtuális gép hello portálon, és kattintson a **lemezek**.</span><span class="sxs-lookup"><span data-stu-id="6a79e-194">Select your VM in hello portal and click **Disks**.</span></span> <span data-ttu-id="6a79e-195">Válasszon a meglévő virtuális merevlemezt, majd kattintson **leválasztási**:</span><span class="sxs-lookup"><span data-stu-id="6a79e-195">Select your existing virtual hard disk and then click **Detach**:</span></span>

    ![Válassza le a meglévő virtuális merevlemez](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    <span data-ttu-id="6a79e-197">Várjon, amíg hello VM van az a folytatás előtt hello adatlemez leválasztása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="6a79e-197">Wait until hello VM has successfully detached hello data disk before continuing.</span></span>

## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="6a79e-198">Virtuális gép eredeti merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a79e-198">Create VM from original hard disk</span></span>
<span data-ttu-id="6a79e-199">toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="6a79e-199">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="6a79e-200">hello sablon létrehozni meglévő virtuális hálózatban, hello virtuális merevlemez URL-címet a hello segítségével központilag telepíti egy virtuális gép korábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="6a79e-200">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="6a79e-201">Kattintson a hello **tooAzure telepítése** gombra kattint, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6a79e-201">Click hello **Deploy tooAzure** button as follows:</span></span>

![A sablont a Githubból a virtuális gép üzembe helyezése](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

<span data-ttu-id="6a79e-203">hello sablon betöltődik a hello Azure-portál a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="6a79e-203">hello template is loaded into hello Azure portal for deployment.</span></span> <span data-ttu-id="6a79e-204">Írja be az új virtuális gép és a meglévő Azure-erőforrások hello nevét, és illessze be a hello URL-cím tooyour létező virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="6a79e-204">Enter hello names for your new VM and existing Azure resources, and paste hello URL tooyour existing virtual hard disk.</span></span> <span data-ttu-id="6a79e-205">toobegin hello központi telepítés, kattintson a **beszerzési**:</span><span class="sxs-lookup"><span data-stu-id="6a79e-205">toobegin hello deployment, click **Purchase**:</span></span>

![Telepítse a virtuális Gépet sablonból](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="6a79e-207">Engedélyezze újra a rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="6a79e-207">Re-enable boot diagnostics</span></span>
<span data-ttu-id="6a79e-208">Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="6a79e-208">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="6a79e-209">toocheck hello rendszerindítási diagnosztika állapotát, és kapcsolja be, ha szükséges, válassza ki a virtuális gép hello portálon.</span><span class="sxs-lookup"><span data-stu-id="6a79e-209">toocheck hello status of boot diagnostics and turn on if needed, select your VM in hello portal.</span></span> <span data-ttu-id="6a79e-210">A **figyelés**, kattintson a **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6a79e-210">Under **Monitoring**, click **Diagnostics settings**.</span></span> <span data-ttu-id="6a79e-211">Győződjön meg arról, hello állapota **a**, és hello pipa mellett túl**rendszerindítási diagnosztika** van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6a79e-211">Ensure hello status is **On**, and hello check mark next too**Boot diagnostics** is selected.</span></span> <span data-ttu-id="6a79e-212">Ha bármilyen módosításhoz kattintson **mentése**:</span><span class="sxs-lookup"><span data-stu-id="6a79e-212">If you make any changes, click **Save**:</span></span>

![Rendszerindítási diagnosztika beállításainak frissítése](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a><span data-ttu-id="6a79e-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a79e-214">Next steps</span></span>
<span data-ttu-id="6a79e-215">Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása SSH kapcsolatok tooan Azure virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a79e-215">If you are having issues connecting tooyour VM, see [Troubleshoot SSH connections tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="6a79e-216">A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Linux virtuális gép](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a79e-216">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Linux VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="6a79e-217">Erőforrás-kezelő használatával kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6a79e-217">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
