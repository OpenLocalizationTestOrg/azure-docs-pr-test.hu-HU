---
title: "A Windows Azure virtuális gépek biztonsági mentése |} Microsoft Docs"
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
ms.openlocfilehash: 8e58a2290e5034ef393f65cbcddb86e18cf4a6ec
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a><span data-ttu-id="11321-103">Készítsen biztonsági másolatot a Windows virtuális gépek Azure-ban</span><span class="sxs-lookup"><span data-stu-id="11321-103">Back up Windows virtual machines in Azure</span></span>

<span data-ttu-id="11321-104">Adatai védelme érdekében érdemes rendszeres időközönként biztonság mentést végeznie.</span><span class="sxs-lookup"><span data-stu-id="11321-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="11321-105">Az Azure Backup georedundáns helyreállítás tárolók tárolt helyreállítási pontokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="11321-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="11321-106">Helyreállításakor a helyreállítási pont, az egész virtuális gép vagy csak adott fájlokat is helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="11321-106">When you restore from a recovery point, you can restore the whole VM or just specific files.</span></span> <span data-ttu-id="11321-107">Ez a cikk ismerteti a Windows Server és az IIS rendszert futtató virtuális gép egyetlen fájl visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="11321-107">This article explains how to restore a single file to a VM running Windows Server and IIS.</span></span> <span data-ttu-id="11321-108">Ha még nem rendelkezik a virtuális gépek használatához, hozhat létre egy a [Windows gyors üzembe helyezés](quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="11321-108">If you don't already have a VM to use, you can create one using the [Windows quickstart](quick-create-portal.md).</span></span> <span data-ttu-id="11321-109">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="11321-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="11321-110">Készítsen biztonsági másolatot a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="11321-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="11321-111">Napi biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="11321-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="11321-112">A fájl visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="11321-112">Restore a file from a backup</span></span>




## <a name="backup-overview"></a><span data-ttu-id="11321-113">A biztonsági mentés áttekintése</span><span class="sxs-lookup"><span data-stu-id="11321-113">Backup overview</span></span>

<span data-ttu-id="11321-114">Az Azure Backup szolgáltatás a biztonsági mentési feladatot indít, amikor elindítja a tartalék mellék időpontban pillanatképének elkészítéséhez.</span><span class="sxs-lookup"><span data-stu-id="11321-114">When the Azure Backup service initiates a backup job, it triggers the backup extension to take a point-in-time snapshot.</span></span> <span data-ttu-id="11321-115">Az Azure Backup szolgáltatás használ a _VMSnapshot_ bővítmény.</span><span class="sxs-lookup"><span data-stu-id="11321-115">The Azure Backup service uses the _VMSnapshot_ extension.</span></span> <span data-ttu-id="11321-116">Ha a virtuális gép fut, a virtuális gép első biztonsági mentés során telepítve a bővítmény.</span><span class="sxs-lookup"><span data-stu-id="11321-116">The extension is installed during the first VM backup if the VM is running.</span></span> <span data-ttu-id="11321-117">Ha a virtuális gép nem fut, a biztonsági mentési szolgáltatás pillanatképet készít a az alapul szolgáló tárolási (mert nem alkalmazás írások végrehajthatók, miközben a virtuális gép le van állítva).</span><span class="sxs-lookup"><span data-stu-id="11321-117">If the VM is not running, the Backup service takes a snapshot of the underlying storage (since no application writes occur while the VM is stopped).</span></span>

<span data-ttu-id="11321-118">Windows virtuális gépek pillanatkép létrehozása van folyamatban, amikor a biztonsági mentési szolgáltatás koordinálja a a kötet árnyékmásolata szolgáltatás (VSS) a virtuális gép lemezeinek konzisztens pillanatképének eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="11321-118">When taking a snapshot of Windows VMs, the Backup service coordinates with the Volume Shadow Copy Service (VSS) to get a consistent snapshot of the virtual machine's disks.</span></span> <span data-ttu-id="11321-119">Miután az Azure Backup szolgáltatás a pillanatfelvételt, az adatátvitel a tárolóba.</span><span class="sxs-lookup"><span data-stu-id="11321-119">Once the Azure Backup service takes the snapshot, the data is transferred to the vault.</span></span> <span data-ttu-id="11321-120">Hatékonyságának maximalizálása érdekében a szolgáltatás azonosítja, és csak az adatok a korábbi biztonsági mentés óta módosult blokkok átvitele.</span><span class="sxs-lookup"><span data-stu-id="11321-120">To maximize efficiency, the service identifies and transfers only the blocks of data that have changed since the previous backup.</span></span>

<span data-ttu-id="11321-121">Ha az adatok átvitele befejeződött, a rendszer eltávolítja a pillanatkép, és egy helyreállítási pontot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="11321-121">When the data transfer is complete, the snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="11321-122">Biztonsági mentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="11321-122">Create a backup</span></span>
<span data-ttu-id="11321-123">Hozzon létre egy egyszerű, ütemezett napi biztonsági mentést egy Recovery Services-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="11321-123">Create a simple scheduled daily backup to a Recovery Services Vault.</span></span> 

1. <span data-ttu-id="11321-124">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="11321-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="11321-125">A bal oldali menüben válassza a **Virtuális gépek** elemet.</span><span class="sxs-lookup"><span data-stu-id="11321-125">In the menu on the left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="11321-126">Válasszon egy virtuális gépet a listából, amelyről biztonsági mentést kíván készíteni.</span><span class="sxs-lookup"><span data-stu-id="11321-126">From the list, select a VM to back up.</span></span>
4. <span data-ttu-id="11321-127">A virtuális gép panelen a a **beállítások** kattintson **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="11321-127">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="11321-128">A **biztonságimásolat-készítés engedélyezése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="11321-128">The **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="11321-129">A **Recovery Services-tároló**, kattintson a **hozzon létre új** , és adja meg az új tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="11321-129">In **Recovery Services vault**, click **Create new** and provide the name for the new vault.</span></span> <span data-ttu-id="11321-130">Egy új tárolót a azonos erőforráscsoport és a virtuális gép jön létre.</span><span class="sxs-lookup"><span data-stu-id="11321-130">A new vault is created in the same Resource Group and location as the virtual machine.</span></span>
6. <span data-ttu-id="11321-131">Kattintson a **biztonsági mentési házirend**.</span><span class="sxs-lookup"><span data-stu-id="11321-131">Click **Backup policy**.</span></span> <span data-ttu-id="11321-132">Ehhez a példához hagyja az alapértelmezett beállításokat, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="11321-132">For this example, keep the defaults and click **OK**.</span></span>
7. <span data-ttu-id="11321-133">Az a **biztonságimásolat-készítés engedélyezése** panelen kattintson a **biztonsági mentés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="11321-133">On the **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="11321-134">Ezzel létrehoz egy alapértelmezett ütemezés szerint a napi biztonsági mentéshez.</span><span class="sxs-lookup"><span data-stu-id="11321-134">This creates a daily backup based on the default schedule.</span></span>
10. <span data-ttu-id="11321-135">Az első helyreállítási pont létrehozásához a **biztonsági mentés** panelen kattintson **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="11321-135">To create an initial recovery point, on the **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="11321-136">Az a **biztonsági mentés most** panelen kattintson a naptár ikonra, válassza ki az elmúlt nap során ez a helyreállítási pont őrzi meg, majd kattintson a havinaptár-vezérlő segítségével **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="11321-136">On the **Backup Now** blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="11321-137">Az a **biztonsági mentés** a virtuális gép paneljén láthatja, amelyek a teljes helyreállítási pontok száma.</span><span class="sxs-lookup"><span data-stu-id="11321-137">In the **Backup** blade for your VM, you will see the number of recovery points that are complete.</span></span>

    ![Helyreállítási pontok](./media/tutorial-backup-vms/backup-complete.png)
    
<span data-ttu-id="11321-139">Az első biztonsági mentés körülbelül 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="11321-139">The first backup takes about 20 minutes.</span></span> <span data-ttu-id="11321-140">A biztonsági mentés befejezése után ez az oktatóanyag következő része folytassa a műveletet.</span><span class="sxs-lookup"><span data-stu-id="11321-140">Proceed to the next part of this tutorial after your backup is finished.</span></span>

## <a name="recover-a-file"></a><span data-ttu-id="11321-141">A fájl helyreállításához</span><span class="sxs-lookup"><span data-stu-id="11321-141">Recover a file</span></span>

<span data-ttu-id="11321-142">Ha véletlenül törli vagy módosítja a fájlt, a fájlok helyreállítása használatával állítsa vissza a fájlt a biztonsági mentési tárolóból.</span><span class="sxs-lookup"><span data-stu-id="11321-142">If you accidentally delete or make changes to a file, you can use File Recovery to recover the file from your backup vault.</span></span> <span data-ttu-id="11321-143">A fájlok helyreállítása a helyreállítási pont helyi meghajtóként csatlakoztatni a virtuális gép futó parancsfájlt használ.</span><span class="sxs-lookup"><span data-stu-id="11321-143">File Recovery uses a script that runs on the VM, to mount the recovery point as local drive.</span></span> <span data-ttu-id="11321-144">Ezek a meghajtók marad csatlakoztatott 12 óra, hogy a fájlok másolását a helyreállítási pont, és állítsa vissza őket a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="11321-144">These drives will remain mounted for 12 hours so that you can copy files from the recovery point and restore them to the VM.</span></span>  

<span data-ttu-id="11321-145">Ebben a példában megmutatjuk, hogyan helyreállítása a bináris fájl, amely az IIS alapértelmezett weblap szolgál.</span><span class="sxs-lookup"><span data-stu-id="11321-145">In this example, we show how to recover the image file that is used in the default web page for IIS.</span></span> 

1. <span data-ttu-id="11321-146">Nyisson meg egy böngészőt, és kapcsolódjon az IP-cím, a virtuális gép alapértelmezett IIS lap megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="11321-146">Open a browser and connect to the IP address of the VM to show the default IIS page.</span></span>

    ![Alapértelmezett IIS-weblap](./media/tutorial-backup-vms/iis-working.png)

2. <span data-ttu-id="11321-148">Csatlakoztassa a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="11321-148">Connect to the VM.</span></span>
3. <span data-ttu-id="11321-149">Nyissa meg a virtuális gép **Fájlkezelőben** , \inetpub\wwwroot lépjen, és törölje a fájlt **iisstart.png**.</span><span class="sxs-lookup"><span data-stu-id="11321-149">On the VM, open **File Explorer** and navigate to \inetpub\wwwroot and delete the file **iisstart.png**.</span></span>
4. <span data-ttu-id="11321-150">A helyi számítógépen a böngésző meg arról, hogy a lemezképet, az alapértelmezett IIS lapon már nem frissíthetők.</span><span class="sxs-lookup"><span data-stu-id="11321-150">On your local computer, refresh the browser to see that the image on the default IIS page is gone.</span></span>

    ![Alapértelmezett IIS-weblap](./media/tutorial-backup-vms/iis-broken.png)

5. <span data-ttu-id="11321-152">A helyi számítógépen nyisson meg egy új lapot, és nyissa meg a a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="11321-152">On your local computer, open a new tab and go the the [Azure portal](https://portal.azure.com).</span></span>
6. <span data-ttu-id="11321-153">A bal oldali menüben válasszon ki **virtuális gépek** , és jelölje ki a virtuális gép űrlapot a listában.</span><span class="sxs-lookup"><span data-stu-id="11321-153">In the menu on the left, select **Virtual machines** and select the VM form the list.</span></span>
8. <span data-ttu-id="11321-154">A virtuális gép panelen a a **beállítások** kattintson **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="11321-154">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="11321-155">A **biztonsági mentés** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="11321-155">The **Backup** blade opens.</span></span> 
9. <span data-ttu-id="11321-156">A panel tetején menüben válasszon ki **fájlhelyreállítás**.</span><span class="sxs-lookup"><span data-stu-id="11321-156">In the menu at the top of the blade, select **File Recovery**.</span></span> <span data-ttu-id="11321-157">A **fájlhelyreállítás** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="11321-157">The **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="11321-158">A **1. lépés: válassza ki a helyreállítási pont**, a legördülő listán válasszon egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="11321-158">In **Step 1: Select recovery point**, select a recovery point from the drop-down.</span></span>
11. <span data-ttu-id="11321-159">A **2. lépés: Töltse le a parancsprogramot, keresse meg, majd a fájlok helyreállítása**, kattintson a **végrehajtható fájl letöltése** gombra.</span><span class="sxs-lookup"><span data-stu-id="11321-159">In **Step 2: Download script to browse and recover files**, click the **Download Executable** button.</span></span> <span data-ttu-id="11321-160">Mentse a fájlt a **letölti** mappát.</span><span class="sxs-lookup"><span data-stu-id="11321-160">Save the file to your **Downloads** folder.</span></span>
12. <span data-ttu-id="11321-161">Nyissa meg a helyi számítógép **Fájlkezelőben** , és keresse meg a **letölti** mappát, másolja a letöltött .exe fájlt.</span><span class="sxs-lookup"><span data-stu-id="11321-161">On your local computer, open **File Explorer** and navigate to your **Downloads** folder and copy the downloaded .exe file.</span></span> <span data-ttu-id="11321-162">A fájlnevet fogja szerepelnie, hogy a virtuális gép nevét.</span><span class="sxs-lookup"><span data-stu-id="11321-162">The filename will be prefixed by your VM name.</span></span> 
13. <span data-ttu-id="11321-163">A virtuális gépen (keresztül a távoli ASZTAL kapcsolaton keresztül) illessze be az .exe fájlt az asztalra, a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="11321-163">On your VM (over the RDP connection) paste the .exe file to the Desktop of your VM.</span></span> 
14. <span data-ttu-id="11321-164">Keresse meg a virtuális gép asztalán, és az .exe kattintson rá duplán.</span><span class="sxs-lookup"><span data-stu-id="11321-164">Navigate to the desktop of your VM and double-click on the .exe.</span></span> <span data-ttu-id="11321-165">Ez elindítani a parancssort, és csatlakoztassa a fájlmegosztást, amelyet el tud érni, a helyreállítási pont.</span><span class="sxs-lookup"><span data-stu-id="11321-165">This will launch a command prompt and then mount the recovery point as a file share that you can access.</span></span> <span data-ttu-id="11321-166">Miután befejezte a megosztás létrehozása, írja be a **q** kattintva zárja be a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="11321-166">When it is finished creating the share, type **q** to close the command prompt.</span></span>
15. <span data-ttu-id="11321-167">Nyissa meg a virtuális gép **Fájlkezelőben** , és keresse meg a meghajtóbetűjelet, a fájlmegosztás lett megadva.</span><span class="sxs-lookup"><span data-stu-id="11321-167">On your VM, open **File Explorer** and navigate to the drive letter that was used for the file share.</span></span>
16. <span data-ttu-id="11321-168">Navigáljon a \inetpub\wwwroot, és másolja **iisstart.png** a következő fájl megosztani, és illessze be \inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="11321-168">Navigate to \inetpub\wwwroot and copy **iisstart.png** from the file share and paste it into \inetpub\wwwroot.</span></span> <span data-ttu-id="11321-169">Például F:\inetpub\wwwroot\iisstart.png másolja és illessze be a fájl helyreállítását c:\inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="11321-169">For example, copy F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot to recover the file.</span></span>
17. <span data-ttu-id="11321-170">A helyi számítógépen nyissa meg a böngészőlapon ahol csatlakozik az IIS alapértelmezett lap megjeleníti a virtuális IP-címét.</span><span class="sxs-lookup"><span data-stu-id="11321-170">On your local computer, open the browser tab where you are connected to the IP address of the VM showing the IIS default page.</span></span> <span data-ttu-id="11321-171">Nyomja le a CTRL + F5 csak a böngésző lap frissítése.</span><span class="sxs-lookup"><span data-stu-id="11321-171">Press CTRL + F5 to refresh the browser page.</span></span> <span data-ttu-id="11321-172">Most látnia kell, hogy a kép helyre lett állítva.</span><span class="sxs-lookup"><span data-stu-id="11321-172">You should now see that the image has been restored.</span></span>
18. <span data-ttu-id="11321-173">A helyi számítógépen, térjen vissza az Azure portál és a böngészőlapon **3. lépés: a lemez leválasztása a helyreállítás után** kattintson a **lemez leválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="11321-173">On your local computer, go back to the browser tab for the Azure portal and in **Step 3: Unmount the disks after recovery** click the **Unmount Disks** button.</span></span> <span data-ttu-id="11321-174">Ha ezt a lépést, a kapcsolat a csatlakozási pont nem automatikusan Bezárás 12 óra elteltével.</span><span class="sxs-lookup"><span data-stu-id="11321-174">If you forget to do this step, the connection to the mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="11321-175">E 12 óra elteltével le kell töltenie egy új parancsfájl egy új csatlakoztatási pont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="11321-175">After those 12 hours, you need to download a new script to create a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="11321-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="11321-176">Next steps</span></span>

<span data-ttu-id="11321-177">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="11321-177">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="11321-178">Készítsen biztonsági másolatot a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="11321-178">Create a backup of a VM</span></span>
> * <span data-ttu-id="11321-179">Napi biztonsági mentés ütemezése</span><span class="sxs-lookup"><span data-stu-id="11321-179">Schedule a daily backup</span></span>
> * <span data-ttu-id="11321-180">A fájl visszaállítása biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="11321-180">Restore a file from a backup</span></span>

<span data-ttu-id="11321-181">Előzetes további virtuális gépek figyelésével kapcsolatos következő oktatóanyagot.</span><span class="sxs-lookup"><span data-stu-id="11321-181">Advance to the next tutorial to learn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="11321-182">Virtuális gépek figyelése</span><span class="sxs-lookup"><span data-stu-id="11321-182">Monitor virtual machines</span></span>](tutorial-monitoring.md)









