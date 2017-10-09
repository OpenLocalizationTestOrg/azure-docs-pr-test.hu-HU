---
title: "a virtuális gép az Azure PowerShell hibaelhárítás Windows aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan tootroubleshoot Windows virtuális gép állít ki az Azure-ban csatlakozzon az operációs rendszer hello lemez tooa helyreállítási virtuális Gépet az Azure PowerShell"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 7a6a76f64824fe5d06dc4286cb1d87ab8bb794e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-azure-powershell"></a><span data-ttu-id="cbe39-103">A Windows virtuális gépek hello OS lemez tooa helyreállítási virtuális Gépet az Azure PowerShell csatolásával hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="cbe39-103">Troubleshoot a Windows VM by attaching hello OS disk tooa recovery VM using Azure PowerShell</span></span>
<span data-ttu-id="cbe39-104">Ha a Windows rendszerű virtuális gép (VM) az Azure-ban rendszerindító vagy a lemez hibát tapasztal, szükség lehet a tooperform hibaelhárítási hello virtuális merevlemezen magát.</span><span class="sxs-lookup"><span data-stu-id="cbe39-104">If your Windows virtual machine (VM) in Azure encounters a boot or disk error, you may need tooperform troubleshooting steps on hello virtual hard disk itself.</span></span> <span data-ttu-id="cbe39-105">Ilyenek például a sikertelen frissítés, amely megakadályozza hello méretű képes tooboot sikeresen lenne.</span><span class="sxs-lookup"><span data-stu-id="cbe39-105">A common example would be a failed application update that prevents hello VM from being able tooboot successfully.</span></span> <span data-ttu-id="cbe39-106">Ez a következő cikket: részletek hogyan toouse Azure PowerShell tooconnect a virtuális merevlemez tooanother Windows virtuális gép toofix hibáit, majd hozza létre az eredeti virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cbe39-106">This article details how toouse Azure PowerShell tooconnect your virtual hard disk tooanother Windows VM toofix any errors, then re-create your original VM.</span></span>


## <a name="recovery-process-overview"></a><span data-ttu-id="cbe39-107">Helyreállítási folyamat áttekintése</span><span class="sxs-lookup"><span data-stu-id="cbe39-107">Recovery process overview</span></span>
<span data-ttu-id="cbe39-108">hibaelhárítási folyamatának hello a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="cbe39-108">hello troubleshooting process is as follows:</span></span>

1. <span data-ttu-id="cbe39-109">Törölje a hello VM észlelt problémák, hello virtuális merevlemezek tartása.</span><span class="sxs-lookup"><span data-stu-id="cbe39-109">Delete hello VM encountering issues, keeping hello virtual hard disks.</span></span>
2. <span data-ttu-id="cbe39-110">Csatolja, és a csatlakoztatási hello virtuális merevlemez tooanother Windows virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="cbe39-110">Attach and mount hello virtual hard disk tooanother Windows VM for troubleshooting purposes.</span></span>
3. <span data-ttu-id="cbe39-111">Csatlakoztassa a VM hibaelhárítási toohello.</span><span class="sxs-lookup"><span data-stu-id="cbe39-111">Connect toohello troubleshooting VM.</span></span> <span data-ttu-id="cbe39-112">Szerkesztheti a fájlokat, vagy futtassa az olyan eszközöket toofix problémák hello eredeti virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="cbe39-112">Edit files or run any tools toofix issues on hello original virtual hard disk.</span></span>
4. <span data-ttu-id="cbe39-113">Válassza le a lemezképet, és válassza le hello virtuális merevlemezt a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="cbe39-113">Unmount and detach hello virtual hard disk from hello troubleshooting VM.</span></span>
5. <span data-ttu-id="cbe39-114">Hello eredeti virtuális merevlemez virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cbe39-114">Create a VM using hello original virtual hard disk.</span></span>

<span data-ttu-id="cbe39-115">Győződjön meg arról, hogy rendelkezik [legújabb Azure PowerShell hello](/powershell/azure/overview) telepítve és tooyour előfizetés bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="cbe39-115">Make sure that you have [hello latest Azure PowerShell](/powershell/azure/overview) installed and logged in tooyour subscription:</span></span>

```powershell
Login-AzureRMAccount
```

<span data-ttu-id="cbe39-116">Hello alábbi példák cserélje le a paraméter nevét a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="cbe39-116">In hello following examples, replace parameter names with your own values.</span></span> <span data-ttu-id="cbe39-117">Példa paraméter nevek a következők `myResourceGroup`, `mystorageaccount`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="cbe39-117">Example parameter names include `myResourceGroup`, `mystorageaccount`, and `myVM`.</span></span>


## <a name="determine-boot-issues"></a><span data-ttu-id="cbe39-118">Határozza meg a rendszerindítási problémák</span><span class="sxs-lookup"><span data-stu-id="cbe39-118">Determine boot issues</span></span>
<span data-ttu-id="cbe39-119">Megtekintheti a képernyőkép a virtuális gép az Azure-ban toohelp a rendszerindítási problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="cbe39-119">You can view a screenshot of your VM in Azure toohelp troubleshoot boot issues.</span></span> <span data-ttu-id="cbe39-120">Ezen a képernyőfelvételen látható segítségével azonosíthatók, ezért a virtuális gépek tooboot sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="cbe39-120">This screenshot can help identify why a VM fails tooboot.</span></span> <span data-ttu-id="cbe39-121">hello alábbi példa lekérdezi hello képernyőfelvétel a hello nevű Windows virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="cbe39-121">hello following example gets hello screenshot from hello Windows VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

<span data-ttu-id="cbe39-122">Tekintse át a hello képernyőkép toodetermine miért hello VM tooboot sikertelen.</span><span class="sxs-lookup"><span data-stu-id="cbe39-122">Review hello screenshot toodetermine why hello VM is failing tooboot.</span></span> <span data-ttu-id="cbe39-123">Megjegyzés: a vonatkozó hibaüzeneteket vagy a megadott hibakódok.</span><span class="sxs-lookup"><span data-stu-id="cbe39-123">Note any specific error messages or error codes provided.</span></span>


## <a name="view-existing-virtual-hard-disk-details"></a><span data-ttu-id="cbe39-124">Meglévő virtuális merevlemez részleteinek megtekintése</span><span class="sxs-lookup"><span data-stu-id="cbe39-124">View existing virtual hard disk details</span></span>
<span data-ttu-id="cbe39-125">A virtuális merevlemez tooanother VM csatolhat, meg kell tooidentify hello neve hello virtuális merevlemez (VHD).</span><span class="sxs-lookup"><span data-stu-id="cbe39-125">Before you can attach your virtual hard disk tooanother VM, you need tooidentify hello name of hello virtual hard disk (VHD).</span></span>

<span data-ttu-id="cbe39-126">hello alábbi példa lekérdezi hello nevű virtuális gép adatai `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="cbe39-126">hello following example gets information for hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="cbe39-127">Keressen `Vhd URI` belül hello `StorageProfile` parancs megelőző hello szakasza hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="cbe39-127">Look for `Vhd URI` within hello `StorageProfile` section from hello output of hello preceding command.</span></span> <span data-ttu-id="cbe39-128">hello következő csonkolt példa kimenet látható hello `Vhd URI` hello kódblokk hello vége felé:</span><span class="sxs-lookup"><span data-stu-id="cbe39-128">hello following truncated example output shows hello `Vhd URI` towards hello end of hello code block:</span></span>

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a><span data-ttu-id="cbe39-129">Meglévő virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="cbe39-129">Delete existing VM</span></span>
<span data-ttu-id="cbe39-130">A virtuális merevlemezek és a virtuális gépek az Azure-erőforrások két különböző típusa.</span><span class="sxs-lookup"><span data-stu-id="cbe39-130">Virtual hard disks and VMs are two distinct resources in Azure.</span></span> <span data-ttu-id="cbe39-131">A virtuális merevlemez hello operációs rendszert illeti, alkalmazások és konfigurációk tárolására.</span><span class="sxs-lookup"><span data-stu-id="cbe39-131">A virtual hard disk is where hello operating system itself, applications, and configurations are stored.</span></span> <span data-ttu-id="cbe39-132">hello virtuális gépért csak metaadatokat, amelyek hello vagy a hely határozza meg, és hivatkozik arra az erőforrások, például egy virtuális merevlemezt vagy virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="cbe39-132">hello VM itself is just metadata that defines hello size or location, and references resources such as a virtual hard disk or virtual network interface card (NIC).</span></span> <span data-ttu-id="cbe39-133">Minden virtuális merevlemez rendelkezik-e létrehozásakor kell csatolni a címbérlet tooa virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cbe39-133">Each virtual hard disk has a lease assigned when attached tooa VM.</span></span> <span data-ttu-id="cbe39-134">Bár adatlemezt csatolni, és leválasztott hello virtuális gép futása közben is, hello operációsrendszer-lemez nem választható le, kivéve, ha a virtuális gép erőforrásához hello törlődik.</span><span class="sxs-lookup"><span data-stu-id="cbe39-134">Although data disks can be attached and detached even while hello VM is running, hello OS disk cannot be detached unless hello VM resource is deleted.</span></span> <span data-ttu-id="cbe39-135">hello bérleti tooassociate hello OS lemezt a virtuális gépek továbbra is, akkor is, ha ezt a virtuális Gépet felszabadított és leállított állapotban van.</span><span class="sxs-lookup"><span data-stu-id="cbe39-135">hello lease continues tooassociate hello OS disk with a VM even when that VM is in a stopped and deallocated state.</span></span>

<span data-ttu-id="cbe39-136">első lépés toorecover hello a virtuális gép toodelete hello VM erőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="cbe39-136">hello first step toorecover your VM is toodelete hello VM resource itself.</span></span> <span data-ttu-id="cbe39-137">Virtuális gép törlése hello hello virtuális merevlemezek elhagyja a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="cbe39-137">Deleting hello VM leaves hello virtual hard disks in your storage account.</span></span> <span data-ttu-id="cbe39-138">Hello a virtuális gép törlődik, miután hello virtuális merevlemez tooanother VM tootroubleshoot csatolja, és hárítsa el a hello hibákat.</span><span class="sxs-lookup"><span data-stu-id="cbe39-138">After hello VM is deleted, you attach hello virtual hard disk tooanother VM tootroubleshoot and resolve hello errors.</span></span>

<span data-ttu-id="cbe39-139">a következő példa törlések hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportból `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="cbe39-139">hello following example deletes hello VM named `myVM` from hello resource group named `myResourceGroup`:</span></span>

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

<span data-ttu-id="cbe39-140">Várjon, amíg hello virtuális gép törlése előtt hello virtuális merevlemez tooanother virtuális gép csatlakoztatása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="cbe39-140">Wait until hello VM has finished deleting before you attach hello virtual hard disk tooanother VM.</span></span> <span data-ttu-id="cbe39-141">hello virtuális merevlemezt, amely társítja azt hello VM hello bérlete hello virtuális merevlemez tooanother VM előtt kiadott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="cbe39-141">hello lease on hello virtual hard disk that associates it with hello VM needs toobe released before you can attach hello virtual hard disk tooanother VM.</span></span>


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a><span data-ttu-id="cbe39-142">Meglévő virtuális merevlemez tooanother VM csatolása</span><span class="sxs-lookup"><span data-stu-id="cbe39-142">Attach existing virtual hard disk tooanother VM</span></span>
<span data-ttu-id="cbe39-143">A hello ezután néhány lépést, akkor egy másik virtuális gép hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="cbe39-143">For hello next few steps, you use another VM for troubleshooting purposes.</span></span> <span data-ttu-id="cbe39-144">Hello VM toobrowse hibaelhárítási meglévő virtuális merevlemez toothis csatolja, és hello lemez tartalmának szerkesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="cbe39-144">You attach hello existing virtual hard disk toothis troubleshooting VM toobrowse and edit hello disk's content.</span></span> <span data-ttu-id="cbe39-145">Ez a folyamat toocorrect lehetővé teszi bármely konfigurációs hibák vagy a felülvizsgálati további alkalmazások vagy a rendszer naplófájljait, például.</span><span class="sxs-lookup"><span data-stu-id="cbe39-145">This process allows you toocorrect any configuration errors or review additional application or system log files, for example.</span></span> <span data-ttu-id="cbe39-146">Válasszon, vagy hozzon létre egy másik virtuális gép toouse hibaelhárítási célból.</span><span class="sxs-lookup"><span data-stu-id="cbe39-146">Choose or create another VM toouse for troubleshooting purposes.</span></span>

<span data-ttu-id="cbe39-147">Hello meglévő virtuálismerevlemez-fájl csatolása, adjon meg hello URL-cím toohello lemezt előző hello beolvasott `Get-AzureRmVM` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cbe39-147">When you attach hello existing virtual hard disk, specify hello URL toohello disk obtained in hello preceding `Get-AzureRmVM` command.</span></span> <span data-ttu-id="cbe39-148">hello alábbi példa csatolja egy meglévő virtuális merevlemez toohello nevű virtuális gép hibakeresési `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="cbe39-148">hello following example attaches an existing virtual hard disk toohello troubleshooting VM named `myVMRecovery` in hello resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> <span data-ttu-id="cbe39-149">Lemez hozzáadása olyan toospecify hello hello lemez méretét.</span><span class="sxs-lookup"><span data-stu-id="cbe39-149">Adding a disk requires you toospecify hello size of hello disk.</span></span> <span data-ttu-id="cbe39-150">Mivel azt a meglévő lemez csatolása, hello `-DiskSizeInGB` megadott `$null`.</span><span class="sxs-lookup"><span data-stu-id="cbe39-150">As we attach an existing disk, hello `-DiskSizeInGB` is specified as `$null`.</span></span> <span data-ttu-id="cbe39-151">Ez az érték biztosítja, hello adatlemez megfelelően csatlakozik, és hello nélkül kell toodetermine hello adatlemez igaz méretét.</span><span class="sxs-lookup"><span data-stu-id="cbe39-151">This value ensures hello data disk is correctly attached, and without hello need toodetermine hello true size of data disk.</span></span>


## <a name="mount-hello-attached-data-disk"></a><span data-ttu-id="cbe39-152">Hello csatolt adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="cbe39-152">Mount hello attached data disk</span></span>

1. <span data-ttu-id="cbe39-153">RDP tooyour hibaelhárítási hello megfelelő hitelesítő adatokat használó virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="cbe39-153">RDP tooyour troubleshooting VM using hello appropriate credentials.</span></span> <span data-ttu-id="cbe39-154">hello alábbi példa letölti hello RDP-kapcsolatfájl hello nevű virtuális gép `myVMRecovery` nevű hello erőforráscsoportban `myResourceGroup`, és letölti, túl`C:\Users\ops\Documents`"</span><span class="sxs-lookup"><span data-stu-id="cbe39-154">hello following example downloads hello RDP connection file for hello VM named `myVMRecovery` in hello resource group named `myResourceGroup`, and downloads it too`C:\Users\ops\Documents`"</span></span>

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. <span data-ttu-id="cbe39-155">hello adatlemez automatikusan észlelt és csatolva.</span><span class="sxs-lookup"><span data-stu-id="cbe39-155">hello data disk is automatically detected and attached.</span></span> <span data-ttu-id="cbe39-156">Megtekintheti a csatlakoztatott kötetek toodetermine hello meghajtóbetűjelet hello listáját az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cbe39-156">View hello list of attached volumes toodetermine hello drive letter as follows:</span></span>

    ```powershell
    Get-Disk
    ```

    <span data-ttu-id="cbe39-157">a következő egy példa a kimenetre hello jeleníti meg, a csatlakoztatott lemez virtuális merevlemez hello **2**.</span><span class="sxs-lookup"><span data-stu-id="cbe39-157">hello following example output shows hello virtual hard disk connected a disk **2**.</span></span> <span data-ttu-id="cbe39-158">(Is `Get-Volume` tooview hello meghajtóbetűjel):</span><span class="sxs-lookup"><span data-stu-id="cbe39-158">(You can also use `Get-Volume` tooview hello drive letter):</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a><span data-ttu-id="cbe39-159">Az eredeti virtuális merevlemez kapcsolatos problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="cbe39-159">Fix issues on original virtual hard disk</span></span>
<span data-ttu-id="cbe39-160">Hello meglévő virtuális merevlemezzel csatlakoztatva is képes lemezvizsgálatok elvégzésére bármely karbantartási és hibaelhárítási lépéseket, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="cbe39-160">With hello existing virtual hard disk mounted, you can now perform any maintenance and troubleshooting steps as needed.</span></span> <span data-ttu-id="cbe39-161">Egyszer foglalkoztak hello problémák, folytassa a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="cbe39-161">Once you have addressed hello issues, continue with hello following steps.</span></span>


## <a name="unmount-and-detach-original-virtual-hard-disk"></a><span data-ttu-id="cbe39-162">Válassza le a lemezképet, és válassza le az eredeti virtuális merevlemez</span><span class="sxs-lookup"><span data-stu-id="cbe39-162">Unmount and detach original virtual hard disk</span></span>
<span data-ttu-id="cbe39-163">Ha a hibák fakadó problémák megoldásával válassza le, és hello meglévő virtuális merevlemez válassza le a hibaelhárítási virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="cbe39-163">Once your errors are resolved, you unmount and detach hello existing virtual hard disk from your troubleshooting VM.</span></span> <span data-ttu-id="cbe39-164">Nem használhat a virtuális merevlemez más virtuális gép hello virtuális merevlemez toohello hibaelhárítás a virtuális gép csatlakoztatása hello bérleti kiadásáig.</span><span class="sxs-lookup"><span data-stu-id="cbe39-164">You cannot use your virtual hard disk with any other VM until hello lease attaching hello virtual hard disk toohello troubleshooting VM is released.</span></span>

1. <span data-ttu-id="cbe39-165">Az az RDP-munkameneten belül leválasztása hello adatlemez a helyreállítási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cbe39-165">From within your RDP session, unmount hello data disk on your recovery VM.</span></span> <span data-ttu-id="cbe39-166">Előző kell hello lemez szám hello `Get-Disk` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="cbe39-166">You need hello disk number from hello previous `Get-Disk` cmdlet.</span></span> <span data-ttu-id="cbe39-167">Ezt követően `Set-Disk` tooset hello lemez offline állapotúként:</span><span class="sxs-lookup"><span data-stu-id="cbe39-167">Then, use `Set-Disk` tooset hello disk as offline:</span></span>

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    <span data-ttu-id="cbe39-168">Győződjön meg róla, hello lemez most értéke offline használatával `Get-Disk` újra.</span><span class="sxs-lookup"><span data-stu-id="cbe39-168">Confirm hello disk is now set as offline using `Get-Disk` again.</span></span> <span data-ttu-id="cbe39-169">hello következő egy példa a kimenetre látható rendszer hello lemez most már offline módúra állítja:</span><span class="sxs-lookup"><span data-stu-id="cbe39-169">hello following example output shows hello disk is now set as offline:</span></span>

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. <span data-ttu-id="cbe39-170">Az RDP-munkamenetbe való kilépéshez.</span><span class="sxs-lookup"><span data-stu-id="cbe39-170">Exit your RDP session.</span></span> <span data-ttu-id="cbe39-171">Az Azure PowerShell-munkamenetben távolítsa el a hello virtuális merevlemezt a virtuális gép hibakeresési hello.</span><span class="sxs-lookup"><span data-stu-id="cbe39-171">From your Azure PowerShell session, remove hello virtual hard disk from hello troubleshooting VM.</span></span>

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a><span data-ttu-id="cbe39-172">Virtuális gép eredeti merevlemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbe39-172">Create VM from original hard disk</span></span>
<span data-ttu-id="cbe39-173">toocreate egy virtuális Gépet az eredeti virtuális merevlemez használata [Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="cbe39-173">toocreate a VM from your original virtual hard disk, use [this Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet).</span></span> <span data-ttu-id="cbe39-174">hello tényleges JSON-sablon jelenleg a következő hivatkozás hello:</span><span class="sxs-lookup"><span data-stu-id="cbe39-174">hello actual JSON template is at hello following link:</span></span>

- <span data-ttu-id="cbe39-175">https://RAW.githubusercontent.com/Azure/Azure-quickstart-Templates/Master/201-VM-specialized-VHD-existing-vnet/azuredeploy.JSON</span><span class="sxs-lookup"><span data-stu-id="cbe39-175">https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json</span></span>

<span data-ttu-id="cbe39-176">hello sablon létrehozni meglévő virtuális hálózatban, hello virtuális merevlemez URL-címet a hello segítségével központilag telepíti egy virtuális gép korábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="cbe39-176">hello template deploys a VM into an existing virtual network, using hello VHD URL from hello earlier command.</span></span> <span data-ttu-id="cbe39-177">hello következő példa telepíti hello sablon toohello nevű erőforráscsoport `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="cbe39-177">hello following example deploys hello template toohello resource group named `myResourceGroup`:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

<span data-ttu-id="cbe39-178">Válasz hello hello sablont, például a virtuális gép neve, típusa és Virtuálisgép-méretet megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="cbe39-178">Answer hello prompts for hello template such as VM name, OS type, and VM size.</span></span> <span data-ttu-id="cbe39-179">Hello `osDiskVhdUri` azonos az előzőleg használt hello VM hibaelhárítási meglévő virtuális merevlemez toohello csatlakoztatása hello.</span><span class="sxs-lookup"><span data-stu-id="cbe39-179">hello `osDiskVhdUri` is hello same as previously used when attaching hello existing virtual hard disk toohello troubleshooting VM.</span></span>


## <a name="re-enable-boot-diagnostics"></a><span data-ttu-id="cbe39-180">Engedélyezze újra a rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="cbe39-180">Re-enable boot diagnostics</span></span>

<span data-ttu-id="cbe39-181">Hello létező virtuális merevlemezt a virtuális Gépet hoz létre, ha rendszerindítási diagnosztika automatikusan nem lehet engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="cbe39-181">When you create your VM from hello existing virtual hard disk, boot diagnostics may not automatically be enabled.</span></span> <span data-ttu-id="cbe39-182">hello következő példa engedélyezi hello hello nevű virtuális gép diagnosztikai kiterjesztés `myVMDeployed` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="cbe39-182">hello following example enables hello diagnostic extension on hello VM named `myVMDeployed` in hello resource group named `myResourceGroup`:</span></span>

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a><span data-ttu-id="cbe39-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbe39-183">Next steps</span></span>
<span data-ttu-id="cbe39-184">Ha a Kapcsolódás a virtuális gép tooyour problémát tapasztal, tekintse meg [hibaelhárítása RDP-kapcsolatok tooan Azure virtuális gép](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbe39-184">If you are having issues connecting tooyour VM, see [Troubleshoot RDP connections tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="cbe39-185">A virtuális gépen futó alkalmazások elérésével problémákkal kapcsolatban lásd: [alkalmazás csatlakozási problémák a Windows virtuális gép](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbe39-185">For issues with accessing applications running on your VM, see [Troubleshoot application connectivity issues on a Windows VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="cbe39-186">Erőforrás-kezelő használatával kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cbe39-186">For more information about using Resource Manager, see [Azure Resource Manager overview](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
