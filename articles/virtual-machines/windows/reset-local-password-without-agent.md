---
title: "Azure-ügynök nélkül helyi Windows-jelszó alaphelyzetbe állítása |} Microsoft Docs"
description: "Visszaállítása egy helyi Windows felhasználói fiók jelszavát, amikor az Azure Vendég ügynök nincs telepítve vagy nem működik a virtuális gép"
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
ms.openlocfilehash: 880f5e5967298401fc2522124af3746d9906ffa8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reset-local-windows-password-for-azure-vm"></a><span data-ttu-id="59d65-103">Helyi Windows-jelszó visszaállítása az Azure virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="59d65-103">How to reset local Windows password for Azure VM</span></span>
<span data-ttu-id="59d65-104">A helyi Windows-jelszó a virtuális gépek az Azure használatával visszaállíthatja a [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) megadott Azure Vendég-ügynök telepítve van.</span><span class="sxs-lookup"><span data-stu-id="59d65-104">You can reset the local Windows password of a VM in Azure using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) provided the Azure guest agent is installed.</span></span> <span data-ttu-id="59d65-105">Ez a módszer elsődleges módja a jelszó alaphelyzetbe állítása egy Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="59d65-105">This method is the primary way to reset a password for an Azure VM.</span></span> <span data-ttu-id="59d65-106">Ha problémák lépnek fel az Azure Vendég ügynöke a vendégügynök nem válaszol, vagy sikertelen egyéni lemezkép feltöltése után telepíteni, manuálisan állítsa vissza a Windows-jelszóval.</span><span class="sxs-lookup"><span data-stu-id="59d65-106">If you encounter issues with the Azure guest agent not responding, or failing to install after uploading a custom image, you can manually reset a Windows password.</span></span> <span data-ttu-id="59d65-107">Ez a cikk részletesen a forrás operációs rendszer virtuális lemez csatolása a másik virtuális gép által a helyi fiók jelszavának visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="59d65-107">This article details how to reset a local account password by attaching the source OS virtual disk to another VM.</span></span> 

> [!WARNING]
> <span data-ttu-id="59d65-108">Csak utolsó lehetőségként használja a ezt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="59d65-108">Only use this process as a last resort.</span></span> <span data-ttu-id="59d65-109">Mindig próbálja alaphelyzetbe a jelszót a [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) első.</span><span class="sxs-lookup"><span data-stu-id="59d65-109">Always try to reset a password using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) first.</span></span>
> 
> 

## <a name="overview-of-the-process"></a><span data-ttu-id="59d65-110">A folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="59d65-110">Overview of the process</span></span>
<span data-ttu-id="59d65-111">A core lépések végrehajtásához a helyi jelszót alaphelyzetbe állítani a Windows virtuális gépek Azure-ban, amikor nem érhető el az Azure Vendég ügynöke a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="59d65-111">The core steps for performing a local password reset for a Windows VM in Azure when there is no access to the Azure guest agent is as follows:</span></span>

* <span data-ttu-id="59d65-112">A forrás virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="59d65-112">Delete the source VM.</span></span> <span data-ttu-id="59d65-113">A virtuális lemezek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="59d65-113">The virtual disks are retained.</span></span>
* <span data-ttu-id="59d65-114">A forrás virtuális gép operációsrendszer-lemez csatolása egy másik virtuális gép ugyanazon a helyen belül az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="59d65-114">Attach the source VM's OS disk to another VM on the same location within your Azure subscription.</span></span> <span data-ttu-id="59d65-115">Ez a virtuális gép a hibaelhárítás VM nevezzük.</span><span class="sxs-lookup"><span data-stu-id="59d65-115">This VM is referred to as the troubleshooting VM.</span></span>
* <span data-ttu-id="59d65-116">A hibaelhárítási VM használatával hozhat létre az egyes konfigurációs fájlokat a forrás virtuális gép operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="59d65-116">Using the troubleshooting VM, create some config files on the source VM's OS disk.</span></span>
* <span data-ttu-id="59d65-117">Válassza le a virtuális gép operációsrendszer-lemez a hibaelhárítási virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="59d65-117">Detach the VM's OS disk from the troubleshooting VM.</span></span>
* <span data-ttu-id="59d65-118">A Resource Manager-sablon használatával hozzon létre egy virtuális Gépet az eredeti virtuális lemez segítségével.</span><span class="sxs-lookup"><span data-stu-id="59d65-118">Use a Resource Manager template to create a VM, using the original virtual disk.</span></span>
* <span data-ttu-id="59d65-119">Amikor az új virtuális gép elindul, a konfigurációs fájlt hoz létre frissítése a szükséges felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="59d65-119">When the new VM boots, the config files you create update the password of the required user.</span></span>

## <a name="detailed-steps"></a><span data-ttu-id="59d65-120">Részletes lépések</span><span class="sxs-lookup"><span data-stu-id="59d65-120">Detailed steps</span></span>
<span data-ttu-id="59d65-121">Mindig próbálja alaphelyzetbe a jelszót a [Azure-portálon vagy az Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) előtt az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="59d65-121">Always try to reset a password using the [Azure portal or Azure PowerShell](reset-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) before trying the following steps.</span></span> <span data-ttu-id="59d65-122">Győződjön meg arról, hogy a virtuális gép biztonsági másolata megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="59d65-122">Make sure you have a backup of your VM before you start.</span></span> 

1. <span data-ttu-id="59d65-123">Törölje a fertőzött virtuális Gépet az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="59d65-123">Delete the affected VM in Azure portal.</span></span> <span data-ttu-id="59d65-124">Csak a virtuális gép törlésekor törlődnek a metaadatokat, a hivatkozás a virtuális gép Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="59d65-124">Deleting the VM only deletes the metadata, the reference of the VM within Azure.</span></span> <span data-ttu-id="59d65-125">A virtuális lemezek megőrzi a virtuális gép törlésekor:</span><span class="sxs-lookup"><span data-stu-id="59d65-125">The virtual disks are retained when the VM is deleted:</span></span>
   
   * <span data-ttu-id="59d65-126">Jelölje ki azt a virtuális Gépet az Azure portálon *törlése*:</span><span class="sxs-lookup"><span data-stu-id="59d65-126">Select the VM in the Azure portal, click *Delete*:</span></span>
     
     ![Meglévő virtuális gép törlése](./media/reset-local-password-without-agent/delete_vm.png)
2. <span data-ttu-id="59d65-128">A forrás virtuális gép operációsrendszer-lemez csatolása a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="59d65-128">Attach the source VM’s OS disk to the troubleshooting VM.</span></span> <span data-ttu-id="59d65-129">A hibaelhárítási virtuális gép és a forrás virtuális gép operációsrendszer-lemez ugyanabban a régióban kell lennie (például `West US`):</span><span class="sxs-lookup"><span data-stu-id="59d65-129">The troubleshooting VM must be in the same region as the source VM's OS disk (such as `West US`):</span></span>
   
   * <span data-ttu-id="59d65-130">Válassza ki a hibaelhárítási virtuális Gépet az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="59d65-130">Select the troubleshooting VM in the Azure portal.</span></span> <span data-ttu-id="59d65-131">Kattintson a *lemezek* | *Csatolás meglévő*:</span><span class="sxs-lookup"><span data-stu-id="59d65-131">Click *Disks* | *Attach existing*:</span></span>
     
     ![Meglévő lemez csatolása](./media/reset-local-password-without-agent/disks_attach_existing.png)
     
     <span data-ttu-id="59d65-133">Válassza ki *VHD-fájl* , és válassza ki a tárfiók, amely tartalmazza a virtuális gép – forrásként:</span><span class="sxs-lookup"><span data-stu-id="59d65-133">Select *VHD File* and then select the storage account that contains your source VM:</span></span>
     
     ![Adattároló fiók kiválasztása](./media/reset-local-password-without-agent/disks_select_storageaccount.PNG)
     
     <span data-ttu-id="59d65-135">A forrás-tároló választása.</span><span class="sxs-lookup"><span data-stu-id="59d65-135">Select the source container.</span></span> <span data-ttu-id="59d65-136">A forrás tároló van általában *VHD-k*:</span><span class="sxs-lookup"><span data-stu-id="59d65-136">The source container is typically *vhds*:</span></span>
     
     ![Válassza ki a tároló](./media/reset-local-password-without-agent/disks_select_container.png)
     
     <span data-ttu-id="59d65-138">Válassza ki az operációs rendszer a csatolása.</span><span class="sxs-lookup"><span data-stu-id="59d65-138">Select the OS vhd to attach.</span></span> <span data-ttu-id="59d65-139">Kattintson a *válasszon* a folyamat befejezéséhez:</span><span class="sxs-lookup"><span data-stu-id="59d65-139">Click *Select* to complete the process:</span></span>
     
     ![Válassza ki a forrás virtuális lemez](./media/reset-local-password-without-agent/disks_select_source_vhd.png)
3. <span data-ttu-id="59d65-141">Csatlakoztassa a távoli asztali kapcsolattal hibaelhárítási virtuális Gépet, és győződjön meg arról, a forrás virtuális gép operációsrendszer-lemez látható:</span><span class="sxs-lookup"><span data-stu-id="59d65-141">Connect to the troubleshooting VM using Remote Desktop and ensure the source VM's OS disk is visible:</span></span>
   
   * <span data-ttu-id="59d65-142">Válassza ki a hibaelhárítási virtuális Gépet az Azure portálon, és kattintson a *Connect*.</span><span class="sxs-lookup"><span data-stu-id="59d65-142">Select the troubleshooting VM in the Azure portal and click *Connect*.</span></span>
   * <span data-ttu-id="59d65-143">Nyissa meg az RDP-fájl, amely letölti.</span><span class="sxs-lookup"><span data-stu-id="59d65-143">Open the RDP file that downloads.</span></span> <span data-ttu-id="59d65-144">Adja meg a felhasználónevet és jelszót a hibaelhárítási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="59d65-144">Enter the username and password of the troubleshooting VM.</span></span>
   * <span data-ttu-id="59d65-145">A Fájlkezelőben keresse meg a csatolt adatlemeze.</span><span class="sxs-lookup"><span data-stu-id="59d65-145">In File Explorer, look for the data disk you attached.</span></span> <span data-ttu-id="59d65-146">Ha a forrás virtuális gép virtuális merevlemez a hibaelhárítási virtuális Géphez csatlakozik, egyetlen adatlemeze, meg kell a F: meghajtó:</span><span class="sxs-lookup"><span data-stu-id="59d65-146">If the source VM’s VHD is the only data disk attached to the troubleshooting VM, it should be the F: drive:</span></span>
     
     ![Csatolt adatlemez megtekintése](./media/reset-local-password-without-agent/troubleshooting_vm_fileexplorer.png)
4. <span data-ttu-id="59d65-148">Hozzon létre `gpt.ini` a `\Windows\System32\GroupPolicy` a forrás virtuális gép meghajtón (ha létezik a gpt.ini, nevezze át gpt.ini.bak):</span><span class="sxs-lookup"><span data-stu-id="59d65-148">Create `gpt.ini` in `\Windows\System32\GroupPolicy` on the source VM’s drive (if gpt.ini exists, rename to gpt.ini.bak):</span></span>
   
   > [!WARNING]
   > <span data-ttu-id="59d65-149">Győződjön meg arról, hogy véletlenül hozzon létre a következő fájlokat a C:\Windows, az operációs rendszer meghajtó a hibaelhárítási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="59d65-149">Make sure that you do not accidentally create the following files in C:\Windows, the OS drive for the troubleshooting VM.</span></span> <span data-ttu-id="59d65-150">Hozza létre a következő fájlokat a forrás virtuális gép adatok lemezként csatolt OS meghajtóban.</span><span class="sxs-lookup"><span data-stu-id="59d65-150">Create the following files in the OS drive for your source VM that is attached as a data disk.</span></span>
   > 
   > 
   
   * <span data-ttu-id="59d65-151">Adja hozzá a következő sorokat a `gpt.ini` létrehozott fájlt:</span><span class="sxs-lookup"><span data-stu-id="59d65-151">Add the following lines into the `gpt.ini` file you created:</span></span>
     
     ```
     [General]
     gPCFunctionalityVersion=2
     gPCMachineExtensionNames=[{42B5FAAE-6536-11D2-AE5A-0000F87571E3}{40B6664F-4972-11D1-A7CA-0000F87571E3}]
     Version=1
     ```
     
     ![Gpt.ini létrehozása](./media/reset-local-password-without-agent/create_gpt_ini.png)
5. <span data-ttu-id="59d65-153">Hozzon létre `scripts.ini` a `\Windows\System32\GroupPolicy\Machine\Scripts`.</span><span class="sxs-lookup"><span data-stu-id="59d65-153">Create `scripts.ini` in `\Windows\System32\GroupPolicy\Machine\Scripts`.</span></span> <span data-ttu-id="59d65-154">Győződjön meg arról, rejtett mappákba jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="59d65-154">Make sure hidden folders are shown.</span></span> <span data-ttu-id="59d65-155">Szükség esetén hozzon létre a `Machine` vagy `Scripts` mappák.</span><span class="sxs-lookup"><span data-stu-id="59d65-155">If needed, create the `Machine` or `Scripts` folders.</span></span>
   
   * <span data-ttu-id="59d65-156">Adja hozzá a következő sorokat a `scripts.ini` létrehozott fájlt:</span><span class="sxs-lookup"><span data-stu-id="59d65-156">Add the following lines the `scripts.ini` file you created:</span></span>
     
     ```
     [Startup]
     0CmdLine=C:\Windows\System32\FixAzureVM.cmd
     0Parameters=
     ```
     
     ![Scripts.ini létrehozása](./media/reset-local-password-without-agent/create_scripts_ini.png)
6. <span data-ttu-id="59d65-158">Hozzon létre `FixAzureVM.cmd` a `\Windows\System32` a következő tartalmakat, lecserélve a `<username>` és `<newpassword>` saját értékekkel:</span><span class="sxs-lookup"><span data-stu-id="59d65-158">Create `FixAzureVM.cmd` in `\Windows\System32` with the following contents, replacing `<username>` and `<newpassword>` with your own values:</span></span>
   
    ```
    net user <username> <newpassword> /add
    net localgroup administrators <username> /add
    net localgroup "remote desktop users" <username> /add

    ```

    ![FixAzureVM.cmd létrehozása](./media/reset-local-password-without-agent/create_fixazure_cmd.png)
   
    <span data-ttu-id="59d65-160">Az új jelszó definiálásakor meg kell felelnie a virtuális géphez beállított összetettségi követelményeknek.</span><span class="sxs-lookup"><span data-stu-id="59d65-160">You must meet the configured password complexity requirements for your VM when defining the new password.</span></span>
7. <span data-ttu-id="59d65-161">Azure-portálon válassza le a lemezt a hibaelhárítási virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="59d65-161">In Azure portal, detach the disk from the troubleshooting VM:</span></span>
   
   * <span data-ttu-id="59d65-162">Jelölje ki azt a hibaelhárítási virtuális Gépet az Azure portálon *lemezek*.</span><span class="sxs-lookup"><span data-stu-id="59d65-162">Select the troubleshooting VM in the Azure portal, click *Disks*.</span></span>
   * <span data-ttu-id="59d65-163">Válassza ki az adatlemezt csatolni 2. lépésben kattintson *leválasztási*:</span><span class="sxs-lookup"><span data-stu-id="59d65-163">Select the data disk attached in step 2, click *Detach*:</span></span>
     
     ![Leválasztja a lemezt](./media/reset-local-password-without-agent/detach_disk.png)
8. <span data-ttu-id="59d65-165">Mielőtt létrehozna egy virtuális Gépet, szerezze be az URI-t a forrás operációsrendszer-lemez:</span><span class="sxs-lookup"><span data-stu-id="59d65-165">Before you create a VM, obtain the URI to your source OS disk:</span></span>
   
   * <span data-ttu-id="59d65-166">Válassza ki a tárfiókot az Azure portálon, kattintson a *Blobok*.</span><span class="sxs-lookup"><span data-stu-id="59d65-166">Select the storage account in the Azure portal, click *Blobs*.</span></span>
   * <span data-ttu-id="59d65-167">Jelölje ki a tárolót.</span><span class="sxs-lookup"><span data-stu-id="59d65-167">Select the container.</span></span> <span data-ttu-id="59d65-168">A forrás tároló van általában *VHD-k*:</span><span class="sxs-lookup"><span data-stu-id="59d65-168">The source container is typically *vhds*:</span></span>
     
     ![Válassza ki a tárolási fiók blob](./media/reset-local-password-without-agent/select_storage_details.png)
     
     <span data-ttu-id="59d65-170">Válassza ki a forrás virtuális gép operációs rendszer virtuális Merevlemezt, majd kattintson a *másolási* megjelenítő gombra a *URL-cím* nevét:</span><span class="sxs-lookup"><span data-stu-id="59d65-170">Select your source VM OS VHD and click the *Copy* button next to the *URL* name:</span></span>
     
     ![Lemezmásolás URI](./media/reset-local-password-without-agent/copy_source_vhd_uri.png)
9. <span data-ttu-id="59d65-172">A forrás virtuális gép operációsrendszer-lemez a virtuális gép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="59d65-172">Create a VM from the source VM’s OS disk:</span></span>
   
   * <span data-ttu-id="59d65-173">Használjon [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) speciális virtuális Merevlemezt a virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="59d65-173">Use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd) to create a VM from a specialized VHD.</span></span> <span data-ttu-id="59d65-174">Kattintson a `Deploy to Azure` gombra kattintva nyissa meg az Azure-portálon a sablonalapú adatokkal feltöltve az olyan meg.</span><span class="sxs-lookup"><span data-stu-id="59d65-174">Click the `Deploy to Azure` button to open the Azure portal with the templated details populated for you.</span></span>
   * <span data-ttu-id="59d65-175">Ha meg szeretné őrizni a korábbi beállításokat a virtuális gép számára, jelölje be *Szerkesztés sablon* a meglévő virtuális hálózatot, alhálózatot, hálózati adapter vagy nyilvános IP-cím számára.</span><span class="sxs-lookup"><span data-stu-id="59d65-175">If you want to retain all the previous settings for the VM, select *Edit template* to provide your existing VNet, subnet, network adapter, or public IP.</span></span>
   * <span data-ttu-id="59d65-176">Az a `OSDISKVHDURI` paraméter beviteli mezőt, és beillesztés, az előző lépésben beszerzése a forrás VHD URI:</span><span class="sxs-lookup"><span data-stu-id="59d65-176">In the `OSDISKVHDURI` parameter text box, paste the URI of your source VHD obtain in the preceding step:</span></span>
     
     ![Virtuális gép létrehozása sablonból](./media/reset-local-password-without-agent/create_new_vm_from_template.png)
10. <span data-ttu-id="59d65-178">Miután az új virtuális gép fut, csatlakoztassa a virtuális Gépet az új jelszóval, amelyet a távoli asztali kapcsolattal a `FixAzureVM.cmd` parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="59d65-178">After the new VM is running, connect to the VM using Remote Desktop with the new password you specified in the `FixAzureVM.cmd` script.</span></span>
11. <span data-ttu-id="59d65-179">Az új virtuális géphez a távoli munkamenetet távolítsa el a környezet karbantartása a következő fájlokat:</span><span class="sxs-lookup"><span data-stu-id="59d65-179">From your remote session to the new VM, remove the following files to clean up the environment:</span></span>
    
    * <span data-ttu-id="59d65-180">A %windir%\System32</span><span class="sxs-lookup"><span data-stu-id="59d65-180">From %windir%\System32</span></span>
      * <span data-ttu-id="59d65-181">Távolítsa el a FixAzureVM.cmd</span><span class="sxs-lookup"><span data-stu-id="59d65-181">remove FixAzureVM.cmd</span></span>
    * <span data-ttu-id="59d65-182">A %windir%\System32\GroupPolicy\Machine\\</span><span class="sxs-lookup"><span data-stu-id="59d65-182">From %windir%\System32\GroupPolicy\Machine\\</span></span>
      * <span data-ttu-id="59d65-183">Távolítsa el a scripts.ini</span><span class="sxs-lookup"><span data-stu-id="59d65-183">remove scripts.ini</span></span>
    * <span data-ttu-id="59d65-184">A %windir%\System32\GroupPolicy</span><span class="sxs-lookup"><span data-stu-id="59d65-184">From %windir%\System32\GroupPolicy</span></span>
      * <span data-ttu-id="59d65-185">Távolítsa el a gpt.ini (Ha a gpt.ini létezett, és átnevezte gpt.ini.bak, nevezze át a .bak-fájl biztonsági gpt.ini)</span><span class="sxs-lookup"><span data-stu-id="59d65-185">remove gpt.ini (if gpt.ini existed before, and you renamed it to gpt.ini.bak, rename the .bak file back to gpt.ini)</span></span>

## <a name="next-steps"></a><span data-ttu-id="59d65-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="59d65-186">Next steps</span></span>
<span data-ttu-id="59d65-187">Ha továbbra is tud kapcsolódni a távoli asztal, tekintse meg a [RDP hibaelhárítási útmutató](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="59d65-187">If you still cannot connect using Remote Desktop, see the [RDP troubleshooting guide](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="59d65-188">A [részletes hibaelhárítási útmutatója RDP](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ellenőrzi, hogy bizonyos lépésekre helyett módszerek hibaelhárítás.</span><span class="sxs-lookup"><span data-stu-id="59d65-188">The [detailed RDP troubleshooting guide](detailed-troubleshoot-rdp.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) looks at troubleshooting methods rather than specific steps.</span></span> <span data-ttu-id="59d65-189">Emellett [az Azure támogatási kérést nyithat](https://azure.microsoft.com/support/options/) gyakorlati segítségért.</span><span class="sxs-lookup"><span data-stu-id="59d65-189">You can also [open an Azure support request](https://azure.microsoft.com/support/options/) for hands-on assistance.</span></span>

