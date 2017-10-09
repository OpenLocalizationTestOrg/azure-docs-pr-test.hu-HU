---
title: "Azure-Windows-alapú virtuális gépek aaaBackup |} Microsoft Docs"
description: "A Windows virtuális gépek védelme készítésével Azure Backup segítségével."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1cd3e1940a83aacd160cba3c8613b63b6f3c11d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a><span data-ttu-id="a1c31-103">Készítsen biztonsági másolatot a Windows virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a1c31-103">Back up Windows virtual machines in Azure</span></span>

<span data-ttu-id="a1c31-104">Adatai védelme érdekében érdemes rendszeres időközönként biztonság mentést végeznie.</span><span class="sxs-lookup"><span data-stu-id="a1c31-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="a1c31-105">Az Azure Backup georedundáns helyreállítás tárolók tárolt helyreállítási pontokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1c31-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="a1c31-106">Egy helyreállítási pontból állítsa vissza, ha visszaállíthatja hello az egész virtuális gép vagy csak adott fájlokat.</span><span class="sxs-lookup"><span data-stu-id="a1c31-106">When you restore from a recovery point, you can restore hello whole VM or just specific files.</span></span> <span data-ttu-id="a1c31-107">Ez a cikk azt ismerteti, hogyan toorestore egyetlen fájl tooa virtuális gép Windows Server és az IIS futnak.</span><span class="sxs-lookup"><span data-stu-id="a1c31-107">This article explains how toorestore a single file tooa VM running Windows Server and IIS.</span></span> <span data-ttu-id="a1c31-108">Ha még nem rendelkezik a virtuális gép toouse, létrehozhat egy hello segítségével [Windows gyors üzembe helyezés](quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a1c31-108">If you don't already have a VM toouse, you can create one using hello [Windows quickstart](quick-create-portal.md).</span></span> <span data-ttu-id="a1c31-109">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="a1c31-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a1c31-110">Készítsen biztonsági másolatot a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="a1c31-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="a1c31-111">Napi biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="a1c31-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="a1c31-112">A fájl visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="a1c31-112">Restore a file from a backup</span></span>




## <a name="backup-overview"></a><span data-ttu-id="a1c31-113">A biztonsági mentés áttekintése</span><span class="sxs-lookup"><span data-stu-id="a1c31-113">Backup overview</span></span>

<span data-ttu-id="a1c31-114">Hello Azure Backup szolgáltatás a biztonsági mentési feladatot indít, amikor elindítja a hello tartalék mellék tootake időpontban pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="a1c31-114">When hello Azure Backup service initiates a backup job, it triggers hello backup extension tootake a point-in-time snapshot.</span></span> <span data-ttu-id="a1c31-115">hello Azure Backup szolgáltatás által használt hello _VMSnapshot_ bővítmény.</span><span class="sxs-lookup"><span data-stu-id="a1c31-115">hello Azure Backup service uses hello _VMSnapshot_ extension.</span></span> <span data-ttu-id="a1c31-116">Ha hello virtuális gép fut, hello első virtuális gép biztonsági mentés során telepítve hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="a1c31-116">hello extension is installed during hello first VM backup if hello VM is running.</span></span> <span data-ttu-id="a1c31-117">Hello virtuális gép nem fut, ha biztonsági mentési szolgáltatás hello pillanatképet készít a hello az alapul szolgáló tárolási (mert nem alkalmazás írási műveletek hello VM leállítása közben történik).</span><span class="sxs-lookup"><span data-stu-id="a1c31-117">If hello VM is not running, hello Backup service takes a snapshot of hello underlying storage (since no application writes occur while hello VM is stopped).</span></span>

<span data-ttu-id="a1c31-118">Windows virtuális gépek pillanatkép létrehozása van folyamatban, amikor hello biztonsági mentési szolgáltatás koordinálja a kötet árnyékmásolata szolgáltatás (VSS) tooget hello hello virtuális gépek lemezeit konzisztens pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="a1c31-118">When taking a snapshot of Windows VMs, hello Backup service coordinates with hello Volume Shadow Copy Service (VSS) tooget a consistent snapshot of hello virtual machine's disks.</span></span> <span data-ttu-id="a1c31-119">Amennyiben az Azure Backup szolgáltatás hello hello pillanatfelvételt, hello adata átvitt toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a1c31-119">Once hello Azure Backup service takes hello snapshot, hello data is transferred toohello vault.</span></span> <span data-ttu-id="a1c31-120">toomaximize hatékonyságát, hello szolgáltatás azonosítja, és csak a hello korábbi biztonsági mentés óta megváltoztak adattípusnak hello blokkok átvitele.</span><span class="sxs-lookup"><span data-stu-id="a1c31-120">toomaximize efficiency, hello service identifies and transfers only hello blocks of data that have changed since hello previous backup.</span></span>

<span data-ttu-id="a1c31-121">Hello adatátvitel befejezésekor hello pillanatkép törlődik, és egy helyreállítási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1c31-121">When hello data transfer is complete, hello snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="a1c31-122">Biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1c31-122">Create a backup</span></span>
<span data-ttu-id="a1c31-123">Hozzon létre egy egyszerű ütemezett napi biztonsági mentési tooa Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="a1c31-123">Create a simple scheduled daily backup tooa Recovery Services Vault.</span></span> 

1. <span data-ttu-id="a1c31-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="a1c31-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="a1c31-125">Hello hello bal oldali menüben válasszon ki **virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-125">In hello menu on hello left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="a1c31-126">Hello listájából válassza ki a virtuális gép tooback fel.</span><span class="sxs-lookup"><span data-stu-id="a1c31-126">From hello list, select a VM tooback up.</span></span>
4. <span data-ttu-id="a1c31-127">Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-127">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="a1c31-128">Hello **biztonságimásolat-készítés engedélyezése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a1c31-128">hello **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="a1c31-129">A **Recovery Services-tároló**, kattintson a **hozzon létre új** , és adja meg az új tároló hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a1c31-129">In **Recovery Services vault**, click **Create new** and provide hello name for hello new vault.</span></span> <span data-ttu-id="a1c31-130">Egy új tároló létrehozása hello ugyanazt az erőforráscsoportot és helyet hello virtuális gépként.</span><span class="sxs-lookup"><span data-stu-id="a1c31-130">A new vault is created in hello same Resource Group and location as hello virtual machine.</span></span>
6. <span data-ttu-id="a1c31-131">Kattintson a **biztonsági mentési házirend**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-131">Click **Backup policy**.</span></span> <span data-ttu-id="a1c31-132">Ehhez a példához hello alapértelmezett tartása, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-132">For this example, keep hello defaults and click **OK**.</span></span>
7. <span data-ttu-id="a1c31-133">A hello **biztonságimásolat-készítés engedélyezése** panelen kattintson a **biztonsági mentés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-133">On hello **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="a1c31-134">Ezzel létrehozza a napi biztonsági mentés a hello alapértelmezett ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="a1c31-134">This creates a daily backup based on hello default schedule.</span></span>
10. <span data-ttu-id="a1c31-135">az első helyreállítási pont, a hello toocreate **biztonsági mentés** panelen kattintson **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-135">toocreate an initial recovery point, on hello **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="a1c31-136">A hello **biztonsági mentés most** panelen hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-136">On hello **Backup Now** blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="a1c31-137">A hello **biztonsági mentés** a virtuális gép paneljén láthatja hello helyreállítási pontok, amelyek a teljes száma.</span><span class="sxs-lookup"><span data-stu-id="a1c31-137">In hello **Backup** blade for your VM, you will see hello number of recovery points that are complete.</span></span>

    ![Helyreállítási pontok](./media/tutorial-backup-vms/backup-complete.png)
    
<span data-ttu-id="a1c31-139">hello első biztonsági mentés körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a1c31-139">hello first backup takes about 20 minutes.</span></span> <span data-ttu-id="a1c31-140">A biztonsági mentés befejezése után a folytatáshoz toohello Ez az oktatóanyag következő része.</span><span class="sxs-lookup"><span data-stu-id="a1c31-140">Proceed toohello next part of this tutorial after your backup is finished.</span></span>

## <a name="recover-a-file"></a><span data-ttu-id="a1c31-141">A fájl helyreállításához</span><span class="sxs-lookup"><span data-stu-id="a1c31-141">Recover a file</span></span>

<span data-ttu-id="a1c31-142">Ha véletlenül törli vagy módosításokat tooa fájl, a fájlok helyreállítása toorecover hello fájlt használhat a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="a1c31-142">If you accidentally delete or make changes tooa file, you can use File Recovery toorecover hello file from your backup vault.</span></span> <span data-ttu-id="a1c31-143">A fájlok helyreállítása a virtuális gép, hello futó parancsfájlt használ toomount hello helyreállítási pont helyi meghajtóként.</span><span class="sxs-lookup"><span data-stu-id="a1c31-143">File Recovery uses a script that runs on hello VM, toomount hello recovery point as local drive.</span></span> <span data-ttu-id="a1c31-144">Ezek a meghajtók marad csatlakoztatott 12 óra, hogy a fájlok másolását hello helyreállítási pont, és állítsa vissza őket toohello VM.</span><span class="sxs-lookup"><span data-stu-id="a1c31-144">These drives will remain mounted for 12 hours so that you can copy files from hello recovery point and restore them toohello VM.</span></span>  

<span data-ttu-id="a1c31-145">Ebben a példában megmutatjuk, hogyan toorecover hello képfájl IIS használt hello alapértelmezett weblapon.</span><span class="sxs-lookup"><span data-stu-id="a1c31-145">In this example, we show how toorecover hello image file that is used in hello default web page for IIS.</span></span> 

1. <span data-ttu-id="a1c31-146">Nyisson meg egy böngészőt, és csatlakozzon a hello VM tooshow hello alapértelmezett IIS lap toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="a1c31-146">Open a browser and connect toohello IP address of hello VM tooshow hello default IIS page.</span></span>

    ![Alapértelmezett IIS-weblap](./media/tutorial-backup-vms/iis-working.png)

2. <span data-ttu-id="a1c31-148">Csatlakoztassa a virtuális gép toohello.</span><span class="sxs-lookup"><span data-stu-id="a1c31-148">Connect toohello VM.</span></span>
3. <span data-ttu-id="a1c31-149">Nyissa meg a virtuális gép hello, **Fájlkezelőben** , és keresse meg a too\inetpub\wwwroot, és törölje a hello fájlt **iisstart.png**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-149">On hello VM, open **File Explorer** and navigate too\inetpub\wwwroot and delete hello file **iisstart.png**.</span></span>
4. <span data-ttu-id="a1c31-150">A helyi számítógépen frissítési hello böngésző toosee hello hello alapértelmezett IIS-lapot a lemezkép nem.</span><span class="sxs-lookup"><span data-stu-id="a1c31-150">On your local computer, refresh hello browser toosee that hello image on hello default IIS page is gone.</span></span>

    ![Alapértelmezett IIS-weblap](./media/tutorial-backup-vms/iis-broken.png)

5. <span data-ttu-id="a1c31-152">A helyi számítógépen nyisson meg egy új lapra, és lépjen hello hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a1c31-152">On your local computer, open a new tab and go hello hello [Azure portal](https://portal.azure.com).</span></span>
6. <span data-ttu-id="a1c31-153">Hello hello bal oldali menüben válasszon ki **virtuális gépek** és select hello VM űrlap hello listáját.</span><span class="sxs-lookup"><span data-stu-id="a1c31-153">In hello menu on hello left, select **Virtual machines** and select hello VM form hello list.</span></span>
8. <span data-ttu-id="a1c31-154">Hello VM panelen, a hello **beállítások** kattintson **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-154">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="a1c31-155">Hello **biztonsági mentés** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a1c31-155">hello **Backup** blade opens.</span></span> 
9. <span data-ttu-id="a1c31-156">Hello panel felső hello hello menüben válasszon ki **fájlhelyreállítás**.</span><span class="sxs-lookup"><span data-stu-id="a1c31-156">In hello menu at hello top of hello blade, select **File Recovery**.</span></span> <span data-ttu-id="a1c31-157">Hello **fájlhelyreállítás** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a1c31-157">hello **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="a1c31-158">A **1. lépés: válassza ki a helyreállítási pont**, a hello legördülő listán válasszon egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="a1c31-158">In **Step 1: Select recovery point**, select a recovery point from hello drop-down.</span></span>
11. <span data-ttu-id="a1c31-159">A **2. lépés: Töltse le a parancsfájl toobrowse és a fájlok helyreállítása**, hello kattintson **végrehajtható fájl letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1c31-159">In **Step 2: Download script toobrowse and recover files**, click hello **Download Executable** button.</span></span> <span data-ttu-id="a1c31-160">Mentse a fájlt tooyour hello **letölti** mappát.</span><span class="sxs-lookup"><span data-stu-id="a1c31-160">Save hello file tooyour **Downloads** folder.</span></span>
12. <span data-ttu-id="a1c31-161">Nyissa meg a helyi számítógép **Fájlkezelőben** , és keresse meg a tooyour **letölti** mappát, másolja hello letöltött .exe-fájl.</span><span class="sxs-lookup"><span data-stu-id="a1c31-161">On your local computer, open **File Explorer** and navigate tooyour **Downloads** folder and copy hello downloaded .exe file.</span></span> <span data-ttu-id="a1c31-162">hello fájlnév fog szerepelnie, hogy a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="a1c31-162">hello filename will be prefixed by your VM name.</span></span> 
13. <span data-ttu-id="a1c31-163">A virtuális gépen (keresztül a távoli ASZTAL kapcsolaton keresztül hello) illessze be a hello .exe fájl toohello a virtuális gép asztalát.</span><span class="sxs-lookup"><span data-stu-id="a1c31-163">On your VM (over hello RDP connection) paste hello .exe file toohello Desktop of your VM.</span></span> 
14. <span data-ttu-id="a1c31-164">Keresse meg a virtuális gép asztalát toohello, és kattintson duplán arra a hello .exe.</span><span class="sxs-lookup"><span data-stu-id="a1c31-164">Navigate toohello desktop of your VM and double-click on hello .exe.</span></span> <span data-ttu-id="a1c31-165">Ez elindítani a parancssort, és csatlakoztassa hello helyreállítási pont, egy fájlmegosztást, amely érheti el.</span><span class="sxs-lookup"><span data-stu-id="a1c31-165">This will launch a command prompt and then mount hello recovery point as a file share that you can access.</span></span> <span data-ttu-id="a1c31-166">Amikor befejeződött hello megosztás létrehozása, írja be a **q** tooclose hello parancssort.</span><span class="sxs-lookup"><span data-stu-id="a1c31-166">When it is finished creating hello share, type **q** tooclose hello command prompt.</span></span>
15. <span data-ttu-id="a1c31-167">Nyissa meg a virtuális gép **Fájlkezelőben** , és keresse meg a hello fájlmegosztás használt toohello meghajtóbetűjelet.</span><span class="sxs-lookup"><span data-stu-id="a1c31-167">On your VM, open **File Explorer** and navigate toohello drive letter that was used for hello file share.</span></span>
16. <span data-ttu-id="a1c31-168">Keresse meg a too\inetpub\wwwroot, és másolja **iisstart.png** hello fájlból megosztani, és illessze be \inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="a1c31-168">Navigate too\inetpub\wwwroot and copy **iisstart.png** from hello file share and paste it into \inetpub\wwwroot.</span></span> <span data-ttu-id="a1c31-169">Például F:\inetpub\wwwroot\iisstart.png másolja és illessze be c:\inetpub\wwwroot toorecover hello fájl.</span><span class="sxs-lookup"><span data-stu-id="a1c31-169">For example, copy F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot toorecover hello file.</span></span>
17. <span data-ttu-id="a1c31-170">A helyi számítógépen, ahol csatlakoznak hello böngészőlapon nyílik hello VM hello az IIS alapértelmezett lapot ábrázoló toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="a1c31-170">On your local computer, open hello browser tab where you are connected toohello IP address of hello VM showing hello IIS default page.</span></span> <span data-ttu-id="a1c31-171">Használja a CTRL + F5 toorefresh hello böngésző lap.</span><span class="sxs-lookup"><span data-stu-id="a1c31-171">Press CTRL + F5 toorefresh hello browser page.</span></span> <span data-ttu-id="a1c31-172">Most látnia kell, hogy hello kép vissza lett állítva.</span><span class="sxs-lookup"><span data-stu-id="a1c31-172">You should now see that hello image has been restored.</span></span>
18. <span data-ttu-id="a1c31-173">A helyi számítógépen, lépjen vissza toohello böngészőlapon hello Azure-portál és a **3. lépés: hello lemez leválasztása a helyreállítás után** hello kattintson **lemez leválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1c31-173">On your local computer, go back toohello browser tab for hello Azure portal and in **Step 3: Unmount hello disks after recovery** click hello **Unmount Disks** button.</span></span> <span data-ttu-id="a1c31-174">Ha elfelejti toodo ezt a lépést, hello kapcsolat toohello a csatlakoztatási pont az automatikusan Bezárás 12 óra elteltével.</span><span class="sxs-lookup"><span data-stu-id="a1c31-174">If you forget toodo this step, hello connection toohello mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="a1c31-175">E 12 óra elteltével kell egy új parancsfájl toocreate egy új csatlakozásipont toodownload.</span><span class="sxs-lookup"><span data-stu-id="a1c31-175">After those 12 hours, you need toodownload a new script toocreate a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a1c31-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a1c31-176">Next steps</span></span>

<span data-ttu-id="a1c31-177">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="a1c31-177">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a1c31-178">Készítsen biztonsági másolatot a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="a1c31-178">Create a backup of a VM</span></span>
> * <span data-ttu-id="a1c31-179">Napi biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="a1c31-179">Schedule a daily backup</span></span>
> * <span data-ttu-id="a1c31-180">A fájl visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="a1c31-180">Restore a file from a backup</span></span>

<span data-ttu-id="a1c31-181">Virtuális gépek figyelésével kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="a1c31-181">Advance toohello next tutorial toolearn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a1c31-182">Virtuális gépek figyelése</span><span class="sxs-lookup"><span data-stu-id="a1c31-182">Monitor virtual machines</span></span>](tutorial-monitoring.md)









