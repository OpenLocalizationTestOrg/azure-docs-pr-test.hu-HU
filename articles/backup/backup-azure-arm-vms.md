---
title: "Azure virtuális gépek biztonsági mentése |} Microsoft Docs"
description: "Ismerje meg, regisztrálja, és a recovery services-tároló az Azure virtuális gépek mentésére."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "virtuális gép biztonsági mentése; Készítsen biztonsági másolatot a virtuális gép; biztonsági mentés és katasztrófa utáni helyreállítás; ARM virtuális gép biztonsági mentése"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40983a3de104238d09b976b5fcf2419da42c1bba
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a><span data-ttu-id="f0442-104">Azure-beli virtuális gépek biztonsági mentése Recovery Services-tárolóba</span><span class="sxs-lookup"><span data-stu-id="f0442-104">Back up Azure virtual machines to a Recovery Services vault</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0442-105">Recovery Services-tároló virtuális gépek mentésére</span><span class="sxs-lookup"><span data-stu-id="f0442-105">Back up VMs to Recovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="f0442-106">Biztonsági másolatot a virtuális gépek mentési tárolóba</span><span class="sxs-lookup"><span data-stu-id="f0442-106">Back up VMs to Backup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="f0442-107">Ez a cikk részletesen biztonsági mentése Azure virtuális gépeken (telepített Resource Manager és klasszikus telepített) a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="f0442-107">This article details how to back up Azure VMs (both Resource Manager-deployed and Classic-deployed) to a Recovery Services vault.</span></span> <span data-ttu-id="f0442-108">Virtuális gépek biztonsági mentéséről a munka nagyobb része a előkészítése.</span><span class="sxs-lookup"><span data-stu-id="f0442-108">Most of the work for backing up VMs is the preparation.</span></span> <span data-ttu-id="f0442-109">Előtt készítsen biztonsági másolatot, vagy egy virtuális gép védelmét, végre kell hajtania a [Előfeltételek](backup-azure-arm-vms-prepare.md) készítse fel a környezetet a virtuális gépek védelmére.</span><span class="sxs-lookup"><span data-stu-id="f0442-109">Before you can back up or protect a VM, you must complete the [prerequisites](backup-azure-arm-vms-prepare.md) to prepare your environment for protecting your VMs.</span></span> <span data-ttu-id="f0442-110">Miután befejezte az előfeltételeket, majd is kezdeményezhető a pillanatképek készítése a virtuális gép biztonsági mentési művelet.</span><span class="sxs-lookup"><span data-stu-id="f0442-110">Once you have completed the prerequisites, then you can initiate the backup operation to take snapshots of your VM.</span></span>


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

<span data-ttu-id="f0442-111">További információkért tekintse meg a cikkek [az Azure virtuális gép biztonsági mentési infrastruktúrájának megtervezésével](backup-azure-vms-introduction.md) és [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="f0442-111">For more information, see the articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

## <a name="triggering-the-backup-job"></a><span data-ttu-id="f0442-112">A biztonsági mentési feladat időt.</span><span class="sxs-lookup"><span data-stu-id="f0442-112">Triggering the backup job</span></span>
<span data-ttu-id="f0442-113">A Recovery Services-tároló társított biztonsági mentési házirend határozza meg, mikor és milyen gyakran fut-e a biztonsági mentési műveletet.</span><span class="sxs-lookup"><span data-stu-id="f0442-113">The backup policy associated with the Recovery Services vault defines how often and when the backup operation runs.</span></span> <span data-ttu-id="f0442-114">Alapértelmezés szerint az első ütemezett biztonsági mentés a kezdeti biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="f0442-114">By default, the first scheduled backup is the initial backup.</span></span> <span data-ttu-id="f0442-115">A kezdeti biztonsági mentés végrehajtásáig a **Biztonsági mentési feladatok** panelen az Utolsó biztonsági mentés állapota **Figyelmeztetés (kezdeti biztonsági mentés folyamatban)** állapotú.</span><span class="sxs-lookup"><span data-stu-id="f0442-115">Until the initial backup occurs, the Last Backup Status on the **Backup Jobs** blade shows as **Warning(initial backup pending)**.</span></span>

![Biztonsági mentés függőben](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

<span data-ttu-id="f0442-117">Hacsak a kezdeti biztonsági mentés kezdete nem a nagyon közeli jövőben van, érdemes futtatni a **Biztonsági másolat készítése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f0442-117">Unless your initial backup is due to begin soon, it is recommended that you run **Back up Now**.</span></span> <span data-ttu-id="f0442-118">Az alábbi eljárást a tároló irányítópult elindul.</span><span class="sxs-lookup"><span data-stu-id="f0442-118">The following procedure starts from the vault dashboard.</span></span> <span data-ttu-id="f0442-119">Ez az eljárás szolgálja ki a kezdeti biztonsági mentési feladat futtatásához szükséges előfeltételek maradéktalanul befejezését követően.</span><span class="sxs-lookup"><span data-stu-id="f0442-119">This procedure serves for running the initial backup job after you have completed all prerequisites.</span></span> <span data-ttu-id="f0442-120">Ha már futtatta a kezdeti biztonsági mentési feladat, ez az eljárás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="f0442-120">If the initial backup job has already been run, this procedure is not available.</span></span> <span data-ttu-id="f0442-121">A társított biztonsági mentési házirend határozza meg a következő biztonsági mentési feladat.</span><span class="sxs-lookup"><span data-stu-id="f0442-121">The associated backup policy determines the next backup job.</span></span>  

<span data-ttu-id="f0442-122">A kezdeti biztonsági mentési feladat futtatása:</span><span class="sxs-lookup"><span data-stu-id="f0442-122">To run the initial backup job:</span></span>

1. <span data-ttu-id="f0442-123">A tároló irányítópultján kattintson a **Biztonsági mentési elemek** szakaszban található számra, vagy kattintson a **Biztonsági mentési elemek** csempére.</span><span class="sxs-lookup"><span data-stu-id="f0442-123">On the vault dashboard, click the number under **Backup Items**, or click the **Backup Items** tile.</span></span> <br/><span data-ttu-id="f0442-124">
  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span><span class="sxs-lookup"><span data-stu-id="f0442-124">
![Settings icon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span></span>

  <span data-ttu-id="f0442-125">Megnyílik a **Biztonsági mentési elemek** panel.</span><span class="sxs-lookup"><span data-stu-id="f0442-125">The **Backup Items** blade opens.</span></span>

  ![Elemek biztonsági mentése](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. <span data-ttu-id="f0442-127">A **Biztonsági mentési elemek** panelen válassza ki az elemet.</span><span class="sxs-lookup"><span data-stu-id="f0442-127">On the **Backup Items** blade, select the item.</span></span>

  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  <span data-ttu-id="f0442-129">Megnyílik a **Biztonsági mentési elemek** listája.</span><span class="sxs-lookup"><span data-stu-id="f0442-129">The **Backup Items** list opens.</span></span> <br/>

  ![Biztonsági mentési feladat elindul](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. <span data-ttu-id="f0442-131">A **Biztonsági mentési elemek** listában kattintson a három pontra **...** a helyi menü megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f0442-131">On the **Backup Items** list, click the ellipses **...** to open the Context menu.</span></span>

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  <span data-ttu-id="f0442-133">Megjelenik a Helyi menü.</span><span class="sxs-lookup"><span data-stu-id="f0442-133">The Context menu appears.</span></span>

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. <span data-ttu-id="f0442-135">A Helyi menüben kattintson a **Biztonsági mentés** elemre.</span><span class="sxs-lookup"><span data-stu-id="f0442-135">On the Context menu, click **Backup now**.</span></span>

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  <span data-ttu-id="f0442-137">Megnyílik a Biztonsági mentés panel.</span><span class="sxs-lookup"><span data-stu-id="f0442-137">The Backup Now blade opens.</span></span>

  ![a Biztonsági mentés panel képe](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. <span data-ttu-id="f0442-139">A Biztonsági mentés panelen kattintson a naptár ikonra, használja a naptárvezérlőt annak kiválasztására, hogy meddig kívánja megőrizni a helyreállítási pontot, majd kattintson a **Biztonsági mentés** elemre.</span><span class="sxs-lookup"><span data-stu-id="f0442-139">On the Backup Now blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>

  ![adja meg az utolsó napot, ameddig Biztonsági mentés helyreállítási pontját meg kívánja őrizni](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  <span data-ttu-id="f0442-141">Az üzembehelyezési értesítések értesítik, hogy a biztonsági mentési feladat elindult, és hogy a feladat állapotát a Biztonsági mentési feladatok oldalon figyelheti.</span><span class="sxs-lookup"><span data-stu-id="f0442-141">Deployment notifications let you know the backup job has been triggered, and that you can monitor the progress of the job on the Backup jobs page.</span></span> <span data-ttu-id="f0442-142">A virtuális gép méretétől függően a kezdeti biztonsági mentés létrehozása hosszabb időt vehet igénybe.</span><span class="sxs-lookup"><span data-stu-id="f0442-142">Depending on the size of your VM, creating the initial backup may take a while.</span></span>

6. <span data-ttu-id="f0442-143">A kezdeti biztonsági mentés állapotának megtekintéséhez vagy nyomon követéséhez a tároló irányítópultjának **Biztonsági mentési feladatok** csempéjén kattintson a **Folyamatban** elemre.</span><span class="sxs-lookup"><span data-stu-id="f0442-143">To view or track the status of the initial backup, on the vault dashboard, on the **Backup Jobs** tile click **In progress**.</span></span>

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  <span data-ttu-id="f0442-145">Megnyílik a Biztonsági mentési feladatok panel.</span><span class="sxs-lookup"><span data-stu-id="f0442-145">The Backup Jobs blade opens.</span></span>

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  <span data-ttu-id="f0442-147">A **Biztonsági mentési feladatok** panelen megtekintheti az összes feladat állapotát.</span><span class="sxs-lookup"><span data-stu-id="f0442-147">In the **Backup jobs** blade, you can see the status of all jobs.</span></span> <span data-ttu-id="f0442-148">Ellenőrizze, hogy a virtuális gép biztonsági mentése folyamatban van-e, vagy már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="f0442-148">Check if the backup job for your VM is still in progress, or if it has finished.</span></span> <span data-ttu-id="f0442-149">Amikor a biztonsági mentési feladat befejeződött, az állapota *Befejezve* lesz.</span><span class="sxs-lookup"><span data-stu-id="f0442-149">When a backup job is finished, the status is *Completed*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="f0442-150">A biztonsági mentési művelet részeként az Azure Backup szolgáltatás egy parancsot ad minden virtuális gépre a biztonsági mentési bővítménynek, hogy ürítsen ki minden írást, és készítsen egy egységes pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="f0442-150">As a part of the backup operation, the Azure Backup service issues a command to the backup extension in each VM to flush all writes and take a consistent snapshot.</span></span>
  >
  >

## <a name="troubleshooting-errors"></a><span data-ttu-id="f0442-151">Kapcsolatos hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="f0442-151">Troubleshooting errors</span></span>
<span data-ttu-id="f0442-152">Ha biztonsági során problémákat tapasztal a virtuális gép, tekintse meg a [VM hibaelhárítási cikke](backup-azure-vms-troubleshoot.md) segítségét.</span><span class="sxs-lookup"><span data-stu-id="f0442-152">If you run into issues while backing up your virtual machine, see the [VM troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0442-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0442-153">Next steps</span></span>
<span data-ttu-id="f0442-154">Most, hogy a virtuális gép védetté, tekintse meg a Virtuálisgép-felügyeleti feladatokat, és hogyan lehet visszaállítani a virtuális gépek a következő cikkeket.</span><span class="sxs-lookup"><span data-stu-id="f0442-154">Now that you have protected your VM, see the following articles to learn about VM management tasks, and how to restore VMs.</span></span>

* [<span data-ttu-id="f0442-155">A virtuális gépek kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="f0442-155">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="f0442-156">Virtuális gépek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="f0442-156">Restore virtual machines</span></span>](backup-azure-arm-restore-vms.md)
