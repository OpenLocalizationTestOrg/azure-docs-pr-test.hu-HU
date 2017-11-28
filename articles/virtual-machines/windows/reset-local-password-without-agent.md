---
title: "a helyi Windows-jelszó nélkül Azure ügynök aaaReset |} Microsoft Docs"
description: "Hogyan tooreset hello-e a helyi Windows felhasználói fiók jelszavát, amikor hello Azure Vendég ügynöke nincs telepítve vagy nem működik a virtuális gép"
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: cf353dd3-89c9-47f6-a449-f874f0957013
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/07/2017
ms.author: iainfou
ms.openlocfilehash: c559c31ea142f9cf50d2c5b6182c5355fec9bac5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-local-windows-password-for-azure-vm"></a><span data-ttu-id="2e604-103">Hogyan tooreset helyi Windows-jelszó Azure virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="2e604-103">How tooreset local Windows password for Azure VM</span></span>
<span data-ttu-id="2e604-104">Visszaállíthatja a hello helyi Windows-jelszó a virtuális gépek az Azure-ban hello [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) megadott hello Azure Vendég-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="2e604-104">You can reset hello local Windows password of a VM in Azure using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) provided hello Azure guest agent is installed.</span></span> <span data-ttu-id="2e604-105">Ez a metódus hello elsődleges módon tooreset egy Azure virtuális gép jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2e604-105">This method is hello primary way tooreset a password for an Azure VM.</span></span> <span data-ttu-id="2e604-106">Ha hibát tapasztal hello Azure Vendég ügynök nem válaszol, vagy egy egyéni lemezkép feltöltése után tooinstall sikertelen, alaphelyzetbe állíthatja a manuálisan egy Windows-jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2e604-106">If you encounter issues with hello Azure guest agent not responding, or failing tooinstall after uploading a custom image, you can manually reset a Windows password.</span></span> <span data-ttu-id="2e604-107">Ez a cikk részletesen, hogyan tooreset egy helyi fiók jelszavát csatolásával hello forrás operációs rendszer virtuális lemez tooanother virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2e604-107">This article details how tooreset a local account password by attaching hello source OS virtual disk tooanother VM.</span></span> 

> [!WARNING]
> <span data-ttu-id="2e604-108">Csak utolsó lehetőségként használja a ezt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="2e604-108">Only use this process as a last resort.</span></span> <span data-ttu-id="2e604-109">Mindig próbálja tooreset hello segítségével jelszó [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) első.</span><span class="sxs-lookup"><span data-stu-id="2e604-109">Always try tooreset a password using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) first.</span></span>
> 
> 

## <a name="overview-of-hello-process"></a><span data-ttu-id="2e604-110">Hello folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="2e604-110">Overview of hello process</span></span>
<span data-ttu-id="2e604-111">egy helyi jelszót alaphelyzetbe állítani a Windows virtuális gépek Azure-ban, amikor nincs hozzáférési toohello Azure Vendég ügynök hello core lépései a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="2e604-111">hello core steps for performing a local password reset for a Windows VM in Azure when there is no access toohello Azure guest agent is as follows:</span></span>

* <span data-ttu-id="2e604-112">Hello forrás virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="2e604-112">Delete hello source VM.</span></span> <span data-ttu-id="2e604-113">hello virtuális lemezek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2e604-113">hello virtual disks are retained.</span></span>
* <span data-ttu-id="2e604-114">Hello forrás virtuális gép operációs rendszer lemez tooanother VM hello csatolása ugyanazon a helyen belül az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="2e604-114">Attach hello source VM's OS disk tooanother VM on hello same location within your Azure subscription.</span></span> <span data-ttu-id="2e604-115">Ez a virtuális gép virtuális gép hibakeresési hivatkozott tooas hello.</span><span class="sxs-lookup"><span data-stu-id="2e604-115">This VM is referred tooas hello troubleshooting VM.</span></span>
* <span data-ttu-id="2e604-116">Néhány konfigurációs fájlt, a hello forrás virtuális gép operációsrendszer-lemez VM hibaelhárítási hello segítségével hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="2e604-116">Using hello troubleshooting VM, create some config files on hello source VM's OS disk.</span></span>
* <span data-ttu-id="2e604-117">Hello virtuális gép operációs rendszer lemezt leválasztani a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="2e604-117">Detach hello VM's OS disk from hello troubleshooting VM.</span></span>
* <span data-ttu-id="2e604-118">A Resource Manager sablon toocreate egy virtuális gépet a hello eredeti virtuális lemezt használja.</span><span class="sxs-lookup"><span data-stu-id="2e604-118">Use a Resource Manager template toocreate a VM, using hello original virtual disk.</span></span>
* <span data-ttu-id="2e604-119">Amikor hello új virtuális gép rendszerindításnál hello olyan konfigurációs fájlt hoz létre a frissítés hello szükséges hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2e604-119">When hello new VM boots, hello config files you create update hello password of hello required user.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="2e604-120">Részletes lépések</span><span class="sxs-lookup"><span data-stu-id="2e604-120">Detailed steps</span></span>
<span data-ttu-id="2e604-121">Mindig próbálja tooreset hello segítségével jelszó [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) hello lépések előtt.</span><span class="sxs-lookup"><span data-stu-id="2e604-121">Always try tooreset a password using hello [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before trying hello following steps.</span></span> <span data-ttu-id="2e604-122">Győződjön meg arról, hogy a virtuális gép biztonsági másolata megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="2e604-122">Make sure you have a backup of your VM before you start.</span></span> 

1. <span data-ttu-id="2e604-123">Törölje az érintett virtuális gép hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2e604-123">Delete hello affected VM in Azure portal.</span></span> <span data-ttu-id="2e604-124">Virtuális gép törlése hello csak törli hello metaadatok, az Azure virtuális gép hello hello hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="2e604-124">Deleting hello VM only deletes hello metadata, hello reference of hello VM within Azure.</span></span> <span data-ttu-id="2e604-125">virtuális lemezek hello megmaradnak hello virtuális gép törlésekor:</span><span class="sxs-lookup"><span data-stu-id="2e604-125">hello virtual disks are retained when hello VM is deleted:</span></span>
   
   * <span data-ttu-id="2e604-126">Kattintson a hello Azure-portálon válassza hello VM *törlése*:</span><span class="sxs-lookup"><span data-stu-id="2e604-126">Select hello VM in hello Azure portal, click *Delete*:</span></span>
     
     ![Meglévő virtuális gép törlése](./media/reset-local-password-without-agent/delete_vm.png)
2. <span data-ttu-id="2e604-128">Hello forrás virtuális gép operációs rendszer lemez toohello VM hibaelhárítási csatolni.</span><span class="sxs-lookup"><span data-stu-id="2e604-128">Attach hello source VM’s OS disk toohello troubleshooting VM.</span></span> <span data-ttu-id="2e604-129">hibaelhárítás a virtuális gép hello hello kell lennie és hello forrás virtuális gép operációsrendszer-lemez ugyanabban a régióban (például `West US`):</span><span class="sxs-lookup"><span data-stu-id="2e604-129">hello troubleshooting VM must be in hello same region as hello source VM's OS disk (such as `West US`):</span></span>
   
   * <span data-ttu-id="2e604-130">Válassza ki a virtuális gép hibaelhárítás hello Azure-portálon a hello.</span><span class="sxs-lookup"><span data-stu-id="2e604-130">Select hello troubleshooting VM in hello Azure portal.</span></span> <span data-ttu-id="2e604-131">Kattintson a *lemezek* | *Csatolás meglévő*:</span><span class="sxs-lookup"><span data-stu-id="2e604-131">Click *Disks* | *Attach existing*:</span></span>
     
     ![Meglévő lemez csatolása](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     <span data-ttu-id="2e604-133">Válassza ki *VHD-fájl* , és válassza a hello storage-fiók, amely tartalmazza a virtuális gép – forrásként:</span><span class="sxs-lookup"><span data-stu-id="2e604-133">Select *VHD File* and then select hello storage account that contains your source VM:</span></span>
     
     ![Adattároló fiók kiválasztása](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     <span data-ttu-id="2e604-135">Hello forrás-tároló választása.</span><span class="sxs-lookup"><span data-stu-id="2e604-135">Select hello source container.</span></span> <span data-ttu-id="2e604-136">hello forrás tároló van általában *VHD-k*:</span><span class="sxs-lookup"><span data-stu-id="2e604-136">hello source container is typically *vhds*:</span></span>
     
     ![Válassza ki a tároló](./media/reset-local-password-without-agent/disks_select_container.png)
     
     <span data-ttu-id="2e604-138">Válassza ki a hello az operációs rendszer virtuális merevlemez tooattach.</span><span class="sxs-lookup"><span data-stu-id="2e604-138">Select hello OS vhd tooattach.</span></span> <span data-ttu-id="2e604-139">Kattintson a *válasszon* toocomplete hello folyamat:</span><span class="sxs-lookup"><span data-stu-id="2e604-139">Click *Select* toocomplete hello process:</span></span>
     
     ![Válassza ki a forrás virtuális lemez](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. <span data-ttu-id="2e604-141">Csatlakozzon a virtuális gép használata a távoli asztal hibaelhárítási toohello, és győződjön meg arról, hello forrás virtuális gép operációsrendszer-lemez látható:</span><span class="sxs-lookup"><span data-stu-id="2e604-141">Connect toohello troubleshooting VM using Remote Desktop and ensure hello source VM's OS disk is visible:</span></span>
   
   * <span data-ttu-id="2e604-142">Válassza ki a virtuális gép hibaelhárítás hello Azure-portálon a hello, és kattintson a *Connect*.</span><span class="sxs-lookup"><span data-stu-id="2e604-142">Select hello troubleshooting VM in hello Azure portal and click *Connect*.</span></span>
   * <span data-ttu-id="2e604-143">Nyissa meg a hello RDP-fájlt, amely letölti.</span><span class="sxs-lookup"><span data-stu-id="2e604-143">Open hello RDP file that downloads.</span></span> <span data-ttu-id="2e604-144">Hello felhasználóneve és jelszava megadása a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="2e604-144">Enter hello username and password of hello troubleshooting VM.</span></span>
   * <span data-ttu-id="2e604-145">A Fájlkezelőben keresse meg a csatolt hello adatlemez.</span><span class="sxs-lookup"><span data-stu-id="2e604-145">In File Explorer, look for hello data disk you attached.</span></span> <span data-ttu-id="2e604-146">Ha hello a virtuális gép virtuális merevlemez forrás hello VM hibakeresési adatok kizárólag csatlakoztatott lemez toohello, meg kell hello F: meghajtó:</span><span class="sxs-lookup"><span data-stu-id="2e604-146">If hello source VM’s VHD is hello only data disk attached toohello troubleshooting VM, it should be hello F: drive:</span></span>
     
     ![Csatolt adatlemez megtekintése](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. <span data-ttu-id="2e604-148">Hozzon létre `gpt.ini` a `\Windows\System32\GroupPolicy` hello forrás Virtuálisgép-meghajtón (ha létezik a gpt.ini, nevezze át toogpt.ini.bak):</span><span class="sxs-lookup"><span data-stu-id="2e604-148">Create `gpt.ini` in `\Windows\System32\GroupPolicy` on hello source VM’s drive (if gpt.ini exists, rename toogpt.ini.bak):</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="2e604-149">Győződjön meg arról, hogy véletlenül nem hello következő C:\Windows a fájlok létrehozása, az operációs rendszer meghajtóját VM hibaelhárítási hello hello.</span><span class="sxs-lookup"><span data-stu-id="2e604-149">Make sure that you do not accidentally create hello following files in C:\Windows, hello OS drive for hello troubleshooting VM.</span></span> <span data-ttu-id="2e604-150">Hozzon létre a következő fájlok hello operációsrendszer-meghajtón a forrás virtuális gép adatok lemezként csatolt hello.</span><span class="sxs-lookup"><span data-stu-id="2e604-150">Create hello following files in hello OS drive for your source VM that is attached as a data disk.</span></span>
   > 
   > 
   
   * <span data-ttu-id="2e604-151">Adja hozzá az alábbi be hello hello `gpt.ini` létrehozott fájlt:</span><span class="sxs-lookup"><span data-stu-id="2e604-151">Add hello following lines into hello `gpt.ini` file you created:</span></span>
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Gpt.ini létrehozása](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. <span data-ttu-id="2e604-153">Hozzon létre `scripts.ini` a `\Windows\System32\GroupPolicy\Machine\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="2e604-153">Create `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`.</span></span> <span data-ttu-id="2e604-154">Győződjön meg arról, rejtett mappákba jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="2e604-154">Make sure hidden folders are shown.</span></span> <span data-ttu-id="2e604-155">Szükség esetén hozzon létre hello `Machine` vagy `Scripts` mappák.</span><span class="sxs-lookup"><span data-stu-id="2e604-155">If needed, create hello `Machine` or `Scripts` folders.</span></span>
   
   * <span data-ttu-id="2e604-156">Adja hozzá a következő sorokat hello hello `scripts.ini` létrehozott fájlt:</span><span class="sxs-lookup"><span data-stu-id="2e604-156">Add hello following lines hello `scripts.ini` file you created:</span></span>
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini létrehozása](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. <span data-ttu-id="2e604-158">Hozzon létre `FixAzureVM.cmd` a `\Windows\System32` hello tartalma a következő, hogy a `<username>` és `<newpassword>` saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="2e604-158">Create `FixAzureVM.cmd` in `\Windows\System32` with hello following contents, replacing `<username>` and `<newpassword>` with your own values:</span></span>
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![FixAzureVM.cmd létrehozása](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    <span data-ttu-id="2e604-160">Új jelszó hello definiálásakor meg kell felelnie konfigurált hello összetettségi követelményeknek a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="2e604-160">You must meet hello configured password complexity requirements for your VM when defining hello new password.</span></span>
7. <span data-ttu-id="2e604-161">Azure-portálon hello lemezt leválasztani a virtuális gép hibakeresési hello:</span><span class="sxs-lookup"><span data-stu-id="2e604-161">In Azure portal, detach hello disk from hello troubleshooting VM:</span></span>
   
   * <span data-ttu-id="2e604-162">Jelölje ki azt a virtuális gép hibaelhárítás hello Azure-portálon a hello *lemezek*.</span><span class="sxs-lookup"><span data-stu-id="2e604-162">Select hello troubleshooting VM in hello Azure portal, click *Disks*.</span></span>
   * <span data-ttu-id="2e604-163">SELECT hello adatlemezt csatolni a 2. lépésben, kattintson a *leválasztási*:</span><span class="sxs-lookup"><span data-stu-id="2e604-163">Select hello data disk attached in step 2, click *Detach*:</span></span>
     
     ![Leválasztja a lemezt](./media/reset-local-password-without-agent/detach_disk.png)
8. <span data-ttu-id="2e604-165">Mielőtt létrehozna egy virtuális Gépet, szerezzen be hello URI tooyour forrás operációsrendszer-lemez:</span><span class="sxs-lookup"><span data-stu-id="2e604-165">Before you create a VM, obtain hello URI tooyour source OS disk:</span></span>
   
   * <span data-ttu-id="2e604-166">Jelölje be hello storage-fiókot hello Azure-portálon kattintson *Blobok*.</span><span class="sxs-lookup"><span data-stu-id="2e604-166">Select hello storage account in hello Azure portal, click *Blobs*.</span></span>
   * <span data-ttu-id="2e604-167">Hello-tároló választása.</span><span class="sxs-lookup"><span data-stu-id="2e604-167">Select hello container.</span></span> <span data-ttu-id="2e604-168">hello forrás tároló van általában *VHD-k*:</span><span class="sxs-lookup"><span data-stu-id="2e604-168">hello source container is typically *vhds*:</span></span>
     
     ![Válassza ki a tárolási fiók blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     <span data-ttu-id="2e604-170">Válassza ki a forrás virtuális gép operációs rendszer virtuális Merevlemezt, majd kattintson a hello *másolási* gomb következő toohello *URL-cím* nevét:</span><span class="sxs-lookup"><span data-stu-id="2e604-170">Select your source VM OS VHD and click hello *Copy* button next toohello *URL* name:</span></span>
     
     ![Lemezmásolás URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. <span data-ttu-id="2e604-172">A virtuális gépek hello forrás virtuális gép operációsrendszer-lemez létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2e604-172">Create a VM from hello source VM’s OS disk:</span></span>
   
   * <span data-ttu-id="2e604-173">Használjon [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate speciális virtuális Merevlemezt a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2e604-173">Use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) toocreate a VM from a specialized VHD.</span></span> <span data-ttu-id="2e604-174">Kattintson a hello `Deploy tooAzure` gomb tooopen hello Azure-portálon kijelöli a hello sablonalapú adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2e604-174">Click hello `Deploy tooAzure` button tooopen hello Azure portal with hello templated details populated for you.</span></span>
   * <span data-ttu-id="2e604-175">Ha tooretain összes hello előző beállítása hello virtuális gép, jelölje be *Szerkesztés sablon* tooprovide a meglévő virtuális hálózatot, alhálózatot, hálózati adapter vagy nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="2e604-175">If you want tooretain all hello previous settings for hello VM, select *Edit template* tooprovide your existing VNet, subnet, network adapter, or public IP.</span></span>
   * <span data-ttu-id="2e604-176">A hello `OSDISKVHDURI` paraméter szövegmezőben, Beillesztés hello lépést megelőző hello az beszerzése a forrás VHD URI:</span><span class="sxs-lookup"><span data-stu-id="2e604-176">In hello `OSDISKVHDURI` parameter text box, paste hello URI of your source VHD obtain in hello preceding step:</span></span>
     
     ![Virtuális gép létrehozása sablonból](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. <span data-ttu-id="2e604-178">Után hello új virtuális gép fut, csatlakozzon a virtuális gép használata a távoli asztal hello hello megadott új jelszó toohello `FixAzureVM.cmd` parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="2e604-178">After hello new VM is running, connect toohello VM using Remote Desktop with hello new password you specified in hello `FixAzureVM.cmd` script.</span></span>
11. <span data-ttu-id="2e604-179">A távoli munkamenet toohello az új virtuális Gépet, a következő eltávolítása hello fájlok tooclean hello környezetet:</span><span class="sxs-lookup"><span data-stu-id="2e604-179">From your remote session toohello new VM, remove hello following files tooclean up hello environment:</span></span>
    
    * <span data-ttu-id="2e604-180">A %windir%\System32</span><span class="sxs-lookup"><span data-stu-id="2e604-180">From %windir%\System32</span></span>
      * <span data-ttu-id="2e604-181">Távolítsa el a FixAzureVM.cmd</span><span class="sxs-lookup"><span data-stu-id="2e604-181">remove FixAzureVM.cmd</span></span>
    * <span data-ttu-id="2e604-182">A %windir%\System32\GroupPolicy\Machine\\</span><span class="sxs-lookup"><span data-stu-id="2e604-182">From %windir%\System32\GroupPolicy\Machine\\</span></span>
      * <span data-ttu-id="2e604-183">Távolítsa el a scripts.ini</span><span class="sxs-lookup"><span data-stu-id="2e604-183">remove scripts.ini</span></span>
    * <span data-ttu-id="2e604-184">A %windir%\System32\GroupPolicy</span><span class="sxs-lookup"><span data-stu-id="2e604-184">From %windir%\System32\GroupPolicy</span></span>
      * <span data-ttu-id="2e604-185">Távolítsa el a gpt.ini (Ha a gpt.ini létezett, és hogy átnevezte toogpt.ini.bak, nevezze át hello .bak fájl hátsó toogpt.ini)</span><span class="sxs-lookup"><span data-stu-id="2e604-185">remove gpt.ini (if gpt.ini existed before, and you renamed it toogpt.ini.bak, rename hello .bak file back toogpt.ini)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2e604-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e604-186">Next steps</span></span>
<span data-ttu-id="2e604-187">Ha továbbra is tud kapcsolódni a távoli asztal használatával, lásd: hello [RDP hibaelhárítási útmutató](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="2e604-187">If you still cannot connect using Remote Desktop, see hello [RDP troubleshooting guide](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="2e604-188">Hello [részletes hibaelhárítási útmutatója RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ellenőrzi, hogy bizonyos lépésekre helyett módszerek hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="2e604-188">hello [detailed RDP troubleshooting guide](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) looks at troubleshooting methods rather than specific steps.</span></span> <span data-ttu-id="2e604-189">Emellett [az Azure támogatási kérést nyithat](https://azure.microsoft.com/support/options/) gyakorlati segítségért.</span><span class="sxs-lookup"><span data-stu-id="2e604-189">You can also [open an Azure support request](https://azure.microsoft.com/support/options/) for hands-on assistance.</span></span>

