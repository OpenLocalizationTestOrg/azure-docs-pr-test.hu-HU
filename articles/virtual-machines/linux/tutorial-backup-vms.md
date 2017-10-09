---
title: "Az Azure Linux virtuális gépek biztonsági mentése |} Microsoft Docs"
description: "A Linux virtuális gépek védelme készítésével Azure Backup segítségével."
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
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7c00392d5185a2f067f2ee2717529dcbde1e71f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a><span data-ttu-id="29804-103">Készítsen biztonsági másolatot a Linux virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="29804-103">Back up Linux  virtual machines in Azure</span></span>

<span data-ttu-id="29804-104">Adatai védelme érdekében érdemes rendszeres időközönként biztonság mentést végeznie.</span><span class="sxs-lookup"><span data-stu-id="29804-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="29804-105">Az Azure Backup georedundáns helyreállítás tárolók tárolt helyreállítási pontokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="29804-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="29804-106">Egy helyreállítási pontból állítsa vissza, ha visszaállíthatja hello az egész virtuális gép vagy csak adott fájlokat.</span><span class="sxs-lookup"><span data-stu-id="29804-106">When you restore from a recovery point, you can restore hello whole VM or just specific files.</span></span> <span data-ttu-id="29804-107">Ez a cikk azt ismerteti, hogyan toorestore egyetlen fájl tooa Linux virtuális gép futó nginx.</span><span class="sxs-lookup"><span data-stu-id="29804-107">This article explains how toorestore a single file tooa Linux VM running nginx.</span></span> <span data-ttu-id="29804-108">Ha még nem rendelkezik a virtuális gép toouse, létrehozhat egy hello segítségével [Linux gyors üzembe helyezés](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="29804-108">If you don't already have a VM toouse, you can create one using hello [Linux quickstart](quick-create-cli.md).</span></span> <span data-ttu-id="29804-109">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="29804-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="29804-110">Készítsen biztonsági másolatot a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="29804-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="29804-111">Napi biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="29804-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="29804-112">A fájl visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="29804-112">Restore a file from a backup</span></span>



## <a name="backup-overview"></a><span data-ttu-id="29804-113">A biztonsági mentés áttekintése</span><span class="sxs-lookup"><span data-stu-id="29804-113">Backup overview</span></span>

<span data-ttu-id="29804-114">Hello Azure biztonsági mentési szolgáltatás biztonsági kezdeményez, amikor elindítja a hello tartalék mellék tootake időpontban pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="29804-114">When hello Azure Backup service initiates a backup, it triggers hello backup extension tootake a point-in-time snapshot.</span></span> <span data-ttu-id="29804-115">hello Azure Backup szolgáltatás által használt hello _VMSnapshotLinux_ Linux bővítményt.</span><span class="sxs-lookup"><span data-stu-id="29804-115">hello Azure Backup service uses hello _VMSnapshotLinux_ extension in Linux.</span></span> <span data-ttu-id="29804-116">Ha hello virtuális gép fut, hello első virtuális gép biztonsági mentés során telepítve hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="29804-116">hello extension is installed during hello first VM backup if hello VM is running.</span></span> <span data-ttu-id="29804-117">Hello virtuális gép nem fut, ha biztonsági mentési szolgáltatás hello pillanatképet készít a hello az alapul szolgáló tárolási (mert nem alkalmazás írási műveletek hello VM leállítása közben történik).</span><span class="sxs-lookup"><span data-stu-id="29804-117">If hello VM is not running, hello Backup service takes a snapshot of hello underlying storage (since no application writes occur while hello VM is stopped).</span></span>

<span data-ttu-id="29804-118">Alapértelmezés szerint Azure biztonsági mentési időt vesz igénybe a fájlrendszer konzisztens biztonsági mentése a Linux virtuális gép, de lehet konfigurált tootake [alkalmazás konzisztens biztonsági mentését parancsfájl előtti és utáni parancsfájl keretrendszerrel](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span><span class="sxs-lookup"><span data-stu-id="29804-118">By default, Azure Backup takes a file system consistent backup for Linux VM but it can be configured tootake [application consistent backup using pre-script and post-script framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span></span> <span data-ttu-id="29804-119">Amennyiben az Azure Backup szolgáltatás hello hello pillanatfelvételt, hello adata átvitt toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="29804-119">Once hello Azure Backup service takes hello snapshot, hello data is transferred toohello vault.</span></span> <span data-ttu-id="29804-120">toomaximize hatékonyságát, hello szolgáltatás azonosítja, és csak a hello korábbi biztonsági mentés óta megváltoztak adattípusnak hello blokkok átvitele.</span><span class="sxs-lookup"><span data-stu-id="29804-120">toomaximize efficiency, hello service identifies and transfers only hello blocks of data that have changed since hello previous backup.</span></span>

<span data-ttu-id="29804-121">Hello adatátvitel befejezésekor hello pillanatkép törlődik, és egy helyreállítási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="29804-121">When hello data transfer is complete, hello snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="29804-122">Biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="29804-122">Create a backup</span></span>
<span data-ttu-id="29804-123">Hozzon létre egy egyszerű ütemezett napi biztonsági mentési tooa Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="29804-123">Create a simple scheduled daily backup tooa Recovery Services Vault.</span></span> 

1. <span data-ttu-id="29804-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29804-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="29804-125">Hello hello bal oldali menüben válasszon ki **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="29804-125">In hello menu on hello left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="29804-126">Hello listájából válassza ki a virtuális gép tooback fel.</span><span class="sxs-lookup"><span data-stu-id="29804-126">From hello list, select a VM tooback up.</span></span>
4. <span data-ttu-id="29804-127">Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="29804-127">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="29804-128">Hello **biztonságimásolat-készítés engedélyezése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="29804-128">hello **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="29804-129">A **Recovery Services-tároló**, kattintson a **hozzon létre új** , és adja meg az új tároló hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="29804-129">In **Recovery Services vault**, click **Create new** and provide hello name for hello new vault.</span></span> <span data-ttu-id="29804-130">Egy új tároló létrehozása hello ugyanazt az erőforráscsoportot és helyet hello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="29804-130">A new vault is created in hello same Resource Group and location as hello virtual machine.</span></span>
6. <span data-ttu-id="29804-131">Kattintson a **biztonsági mentési házirend**.</span><span class="sxs-lookup"><span data-stu-id="29804-131">Click **Backup policy**.</span></span> <span data-ttu-id="29804-132">Ehhez a példához hello alapértelmezett tartása, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="29804-132">For this example, keep hello defaults and click **OK**.</span></span>
7. <span data-ttu-id="29804-133">A hello **biztonságimásolat-készítés engedélyezése** panelen kattintson a **biztonsági mentés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="29804-133">On hello **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="29804-134">Ezzel létrehozza a napi biztonsági mentés a hello alapértelmezett ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="29804-134">This creates a daily backup based on hello default schedule.</span></span>
10. <span data-ttu-id="29804-135">az első helyreállítási pont, a hello toocreate **biztonsági mentés** panelen kattintson **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="29804-135">toocreate an initial recovery point, on hello **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="29804-136">A hello **biztonsági mentés most** panelen hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="29804-136">On hello **Backup Now** blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="29804-137">A hello **biztonsági mentés** a virtuális gép paneljén láthatja hello helyreállítási pontok, amelyek a teljes száma.</span><span class="sxs-lookup"><span data-stu-id="29804-137">In hello **Backup** blade for your VM, you will see hello number of recovery points that are complete.</span></span>

    ![Helyreállítási pontok](./media/tutorial-backup-vms/backup-complete.png)

<span data-ttu-id="29804-139">hello első biztonsági mentés körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="29804-139">hello first backup takes about 20 minutes.</span></span> <span data-ttu-id="29804-140">A biztonsági mentés befejezése után a folytatáshoz toohello Ez az oktatóanyag következő része.</span><span class="sxs-lookup"><span data-stu-id="29804-140">Proceed toohello next part of this tutorial after your backup is finished.</span></span>

## <a name="restore-a-file"></a><span data-ttu-id="29804-141">Fájl visszaállítása</span><span class="sxs-lookup"><span data-stu-id="29804-141">Restore a file</span></span>

<span data-ttu-id="29804-142">Ha véletlenül törli vagy módosításokat tooa fájl, a fájlok helyreállítása toorecover hello fájlt használhat a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="29804-142">If you accidentally delete or make changes tooa file, you can use File Recovery toorecover hello file from your backup vault.</span></span> <span data-ttu-id="29804-143">A fájlok helyreállítása a virtuális gép, hello futó parancsfájlt használ toomount hello helyreállítási pont helyi meghajtóként.</span><span class="sxs-lookup"><span data-stu-id="29804-143">File Recovery uses a script that runs on hello VM, toomount hello recovery point as local drive.</span></span> <span data-ttu-id="29804-144">Ezek a meghajtók marad csatlakoztatott 12 óra, hogy a fájlok másolását hello helyreállítási pont, és állítsa vissza őket toohello VM.</span><span class="sxs-lookup"><span data-stu-id="29804-144">These drives will remain mounted for 12 hours so that you can copy files from hello recovery point and restore them toohello VM.</span></span>  

<span data-ttu-id="29804-145">Ebben a példában megmutatjuk, hogyan toorecover hello alapértelmezett nginx weblap /var/www/html/index.nginx-debian.html.</span><span class="sxs-lookup"><span data-stu-id="29804-145">In this example, we show how toorecover hello default nginx web page /var/www/html/index.nginx-debian.html.</span></span> <span data-ttu-id="29804-146">Ebben a példában a virtuális gép hello nyilvános IP-címe *13.69.75.209*.</span><span class="sxs-lookup"><span data-stu-id="29804-146">hello public IP address of our VM in this example is *13.69.75.209*.</span></span> <span data-ttu-id="29804-147">Hello IP-címet a virtuális gép használatával található:</span><span class="sxs-lookup"><span data-stu-id="29804-147">You can find hello IP address of your vm using:</span></span>

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. <span data-ttu-id="29804-148">A helyi számítógépen nyisson meg egy böngészőt, és írja be hello nyilvános IP-címet a virtuális gép toosee hello alapértelmezett nginx weblap.</span><span class="sxs-lookup"><span data-stu-id="29804-148">On your local computer, open a browser and type in hello public IP address of your VM toosee hello default nginx web page.</span></span>

    ![Alapértelmezett nginx-weblap](./media/tutorial-backup-vms/nginx-working.png)

1. <span data-ttu-id="29804-150">SSH-ból a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="29804-150">SSH into your VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
2. <span data-ttu-id="29804-151">/Var/www/html/index.nginx-debian.html törlése.</span><span class="sxs-lookup"><span data-stu-id="29804-151">Delete /var/www/html/index.nginx-debian.html.</span></span>

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. <span data-ttu-id="29804-152">A helyi számítógépen, frissítheti hello böngésző szerezze meg a CTRL + F5 toosee, amely alapértelmezett nginx oldal nem.</span><span class="sxs-lookup"><span data-stu-id="29804-152">On your local computer, refresh hello browser by hitting CTRL + F5 toosee that default nginx page is gone.</span></span>

    ![Alapértelmezett nginx-weblap](./media/tutorial-backup-vms/nginx-broken.png)
    
1. <span data-ttu-id="29804-154">A helyi számítógépen jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="29804-154">On your local computer, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
6. <span data-ttu-id="29804-155">Hello hello bal oldali menüben válasszon ki **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="29804-155">In hello menu on hello left, select **Virtual machines**.</span></span> 
7. <span data-ttu-id="29804-156">Hello listáról válassza ki a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="29804-156">From hello list, select hello VM.</span></span>
8. <span data-ttu-id="29804-157">Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="29804-157">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="29804-158">Hello **biztonsági mentés** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="29804-158">hello **Backup** blade opens.</span></span> 
9. <span data-ttu-id="29804-159">Hello panel felső hello hello menüben válasszon ki **fájlhelyreállítás**.</span><span class="sxs-lookup"><span data-stu-id="29804-159">In hello menu at hello top of hello blade, select **File Recovery**.</span></span> <span data-ttu-id="29804-160">Hello **fájlhelyreállítás** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="29804-160">hello **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="29804-161">A **1. lépés: válassza ki a helyreállítási pont**, a hello legördülő listán válasszon egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="29804-161">In **Step 1: Select recovery point**, select a recovery point from hello drop-down.</span></span>
11. <span data-ttu-id="29804-162">A **2. lépés: Töltse le a parancsfájl toobrowse és a fájlok helyreállítása**, hello kattintson **végrehajtható fájl letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="29804-162">In **Step 2: Download script toobrowse and recover files**, click hello **Download Executable** button.</span></span> <span data-ttu-id="29804-163">Mentse a hello letöltött fájl tooyour helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="29804-163">Save hello downloaded file tooyour local computer.</span></span>
7. <span data-ttu-id="29804-164">Kattintson a **parancsprogram letöltése** toodownload hello parancsfájl helyileg.</span><span class="sxs-lookup"><span data-stu-id="29804-164">Click **Download script** toodownload hello script file locally.</span></span>
8. <span data-ttu-id="29804-165">Nyissa meg a Bash, írja be hello követően, hogy *Linux_myVM_05-05-2017.sh* hello megfelelő elérési út és fájlnév hello parancsfájl letöltött, *azureuser* hello a felhasználónevet használva a virtuális gép hello és *13.69.75.209* hello nyilvános IP-cím a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="29804-165">Open a Bash prompt and type hello following, replacing *Linux_myVM_05-05-2017.sh* with hello correct path and filename for hello script that you downloaded, *azureuser* with hello username for hello VM and *13.69.75.209* with hello public IP address for your VM.</span></span>
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. <span data-ttu-id="29804-166">A helyi számítógépen nyisson meg egy SSH-kapcsolat toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="29804-166">On your local computer, open an SSH connection toohello VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
    
10. <span data-ttu-id="29804-167">A virtuális gépen, vegye fel az engedélyek toohello parancsfájl végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="29804-167">On your VM, add execute permissions toohello script file.</span></span>

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. <span data-ttu-id="29804-168">A virtuális Gépet, a fájlrendszer hello parancsfájl toomount hello helyreállítási pontot futtató.</span><span class="sxs-lookup"><span data-stu-id="29804-168">On your VM, run hello script toomount hello recovery point as a filesystem.</span></span>

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. <span data-ttu-id="29804-169">hello kimenetét hello parancsfájl ad meg hello hello csatlakozási pont elérési útját.</span><span class="sxs-lookup"><span data-stu-id="29804-169">hello output from hello script gives you hello path for hello mount point.</span></span> <span data-ttu-id="29804-170">hello kimeneti hasonló toothis néz ki:</span><span class="sxs-lookup"><span data-stu-id="29804-170">hello output looks similar toothis:</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting toorecovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of hello recovery point toothis machine...
                         
    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

12. <span data-ttu-id="29804-171">Másolja a virtuális gép hello nginx alapértelmezett weblap hello csatlakoztatási pont hátsó toowhere a hello fájl törölve.</span><span class="sxs-lookup"><span data-stu-id="29804-171">On your VM, copy hello nginx default web page from hello mount point back toowhere you deleted hello file.</span></span>

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. <span data-ttu-id="29804-172">A helyi számítógépen, ahol csatlakoznak hello böngészőlapon nyílik hello VM hello nginx alapértelmezett lapot ábrázoló toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="29804-172">On your local computer, open hello browser tab where you are connected toohello IP address of hello VM showing hello nginx default page.</span></span> <span data-ttu-id="29804-173">Használja a CTRL + F5 toorefresh hello böngésző lap.</span><span class="sxs-lookup"><span data-stu-id="29804-173">Press CTRL + F5 toorefresh hello browser page.</span></span> <span data-ttu-id="29804-174">Most látnia kell, hogy hello alapértelmezett oldal ismét működik.</span><span class="sxs-lookup"><span data-stu-id="29804-174">You should now see that hello default page is working again.</span></span>

    ![Alapértelmezett nginx-weblap](./media/tutorial-backup-vms/nginx-working.png)

18. <span data-ttu-id="29804-176">A helyi számítógépen, lépjen vissza toohello böngészőlapon hello Azure-portál és a **3. lépés: hello lemez leválasztása a helyreállítás után** hello kattintson **lemez leválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="29804-176">On your local computer, go back toohello browser tab for hello Azure portal and in **Step 3: Unmount hello disks after recovery** click hello **Unmount Disks** button.</span></span> <span data-ttu-id="29804-177">Ha elfelejti toodo ezt a lépést, hello kapcsolat toohello a csatlakoztatási pont az automatikusan Bezárás 12 óra elteltével.</span><span class="sxs-lookup"><span data-stu-id="29804-177">If you forget toodo this step, hello connection toohello mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="29804-178">E 12 óra elteltével kell egy új parancsfájl toocreate egy új csatlakozásipont toodownload.</span><span class="sxs-lookup"><span data-stu-id="29804-178">After those 12 hours, you need toodownload a new script toocreate a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="29804-179">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29804-179">Next steps</span></span>

<span data-ttu-id="29804-180">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="29804-180">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="29804-181">Készítsen biztonsági másolatot a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="29804-181">Create a backup of a VM</span></span>
> * <span data-ttu-id="29804-182">Napi biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="29804-182">Schedule a daily backup</span></span>
> * <span data-ttu-id="29804-183">A fájl visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="29804-183">Restore a file from a backup</span></span>

<span data-ttu-id="29804-184">Virtuális gépek figyelésével kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="29804-184">Advance toohello next tutorial toolearn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="29804-185">Virtuális gépek figyelése</span><span class="sxs-lookup"><span data-stu-id="29804-185">Monitor virtual machines</span></span>](tutorial-monitoring.md)

