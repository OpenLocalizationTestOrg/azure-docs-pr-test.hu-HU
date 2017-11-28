---
title: "Azure virtuális gépeinek aaaBack |} Microsoft Docs"
description: "Ismerje meg, és regisztrálja, készítsen biztonsági másolatot az Azure virtuális gépek tooa recovery services-tároló."
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
ms.openlocfilehash: a204a42726450a7fd89b5563a786b5070578b113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-tooa-recovery-services-vault"></a><span data-ttu-id="ce4a7-104">Az Azure virtuális gépek biztonsági mentése tooa Recovery Services tároló</span><span class="sxs-lookup"><span data-stu-id="ce4a7-104">Back up Azure virtual machines tooa Recovery Services vault</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ce4a7-105">Készítsen biztonsági másolatot a virtuális gépek tooRecovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="ce4a7-105">Back up VMs tooRecovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="ce4a7-106">Készítsen biztonsági másolatot a virtuális gépek tooBackup tároló</span><span class="sxs-lookup"><span data-stu-id="ce4a7-106">Back up VMs tooBackup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="ce4a7-107">Ez a cikk részletesen, hogyan mentése Azure virtuális gépeken (telepített Resource Manager és klasszikus telepített) tooa Recovery Services tooback tároló.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-107">This article details how tooback up Azure VMs (both Resource Manager-deployed and Classic-deployed) tooa Recovery Services vault.</span></span> <span data-ttu-id="ce4a7-108">Virtuális gépek biztonsági mentéséről hello munka nagyobb része hello előkészítése.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-108">Most of hello work for backing up VMs is hello preparation.</span></span> <span data-ttu-id="ce4a7-109">Készítsen biztonsági másolatot, vagy egy virtuális gép védelme, hajtsa végre a következő hello [Előfeltételek](backup-azure-arm-vms-prepare.md) tooprepare környezetében a virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-109">Before you can back up or protect a VM, you must complete hello [prerequisites](backup-azure-arm-vms-prepare.md) tooprepare your environment for protecting your VMs.</span></span> <span data-ttu-id="ce4a7-110">Ha az Előfeltételek hello, majd is kezdeményezhető hello biztonsági mentési művelet tootake pillanatképek a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-110">Once you have completed hello prerequisites, then you can initiate hello backup operation tootake snapshots of your VM.</span></span>


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

<span data-ttu-id="ce4a7-111">További információkért lásd: hello cikkeket a [az Azure virtuális gép biztonsági mentési infrastruktúrájának megtervezésével](backup-azure-vms-introduction.md) és [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="ce4a7-111">For more information, see hello articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

## <a name="triggering-hello-backup-job"></a><span data-ttu-id="ce4a7-112">Eseményindító hello biztonsági mentési feladat</span><span class="sxs-lookup"><span data-stu-id="ce4a7-112">Triggering hello backup job</span></span>
<span data-ttu-id="ce4a7-113">Recovery Services-tároló hello társított hello biztonsági mentési házirend határozza meg, milyen gyakran és mikor hello biztonsági mentést futtat.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-113">hello backup policy associated with hello Recovery Services vault defines how often and when hello backup operation runs.</span></span> <span data-ttu-id="ce4a7-114">Alapértelmezés szerint a hello első ütemezett biztonsági mentés hello kezdeti biztonsági másolatot.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-114">By default, hello first scheduled backup is hello initial backup.</span></span> <span data-ttu-id="ce4a7-115">Amíg nem történik hello kezdeti biztonsági másolatot, a hello hello utolsó biztonsági mentés állapotának **biztonsági mentési feladatok** panelt jeleníti meg, mint a **figyelmeztetés (függőben lévő kezdeti biztonsági másolatot)**.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-115">Until hello initial backup occurs, hello Last Backup Status on hello **Backup Jobs** blade shows as **Warning(initial backup pending)**.</span></span>

![Biztonsági mentés függőben](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

<span data-ttu-id="ce4a7-117">Kivéve, ha a kezdeti biztonsági másolatot esedékes toobegin hamarosan, javasoljuk, hogy **biztonsági másolat készítése**.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-117">Unless your initial backup is due toobegin soon, it is recommended that you run **Back up Now**.</span></span> <span data-ttu-id="ce4a7-118">hello következő eljárás elindul hello tároló irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-118">hello following procedure starts from hello vault dashboard.</span></span> <span data-ttu-id="ce4a7-119">Ez az eljárás a hello kezdeti biztonsági mentési feladat fut, miután végrehajtotta a szükséges előfeltételek maradéktalanul szolgál.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-119">This procedure serves for running hello initial backup job after you have completed all prerequisites.</span></span> <span data-ttu-id="ce4a7-120">Ha már korábban lefutott hello kezdeti biztonsági mentési feladat, ez az eljárás nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-120">If hello initial backup job has already been run, this procedure is not available.</span></span> <span data-ttu-id="ce4a7-121">hello tartozó biztonsági mentési házirend meghatározza, hogy hello következő biztonsági mentési feladat.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-121">hello associated backup policy determines hello next backup job.</span></span>  

<span data-ttu-id="ce4a7-122">toorun hello kezdeti biztonsági mentési feladat:</span><span class="sxs-lookup"><span data-stu-id="ce4a7-122">toorun hello initial backup job:</span></span>

1. <span data-ttu-id="ce4a7-123">Az hello tároló irányítópultján kattintson a hello szám alatt **biztonsági mentés elemek**, vagy kattintson a hello **biztonsági mentés elemek** csempe.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-123">On hello vault dashboard, click hello number under **Backup Items**, or click hello **Backup Items** tile.</span></span> <br/><span data-ttu-id="ce4a7-124">
  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span><span class="sxs-lookup"><span data-stu-id="ce4a7-124">
![Settings icon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span></span>

  <span data-ttu-id="ce4a7-125">Hello **biztonsági mentés elemek** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-125">hello **Backup Items** blade opens.</span></span>

  ![Elemek biztonsági mentése](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. <span data-ttu-id="ce4a7-127">A hello **biztonsági mentés elemek** panelen, jelölje be hello elemet.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-127">On hello **Backup Items** blade, select hello item.</span></span>

  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  <span data-ttu-id="ce4a7-129">Hello **biztonsági mentés elemek** listában megnyílik.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-129">hello **Backup Items** list opens.</span></span> <br/>

  ![Biztonsági mentési feladat elindul](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. <span data-ttu-id="ce4a7-131">A hello **biztonsági mentés elemek** listában, kattintson a hello folytatást jelző pontokra **...**  tooopen hello helyi menü.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-131">On hello **Backup Items** list, click hello ellipses **...** tooopen hello Context menu.</span></span>

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  <span data-ttu-id="ce4a7-133">hello helyi menü megjelenik.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-133">hello Context menu appears.</span></span>

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. <span data-ttu-id="ce4a7-135">Hello helyi menüben, kattintson a **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-135">On hello Context menu, click **Backup now**.</span></span>

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  <span data-ttu-id="ce4a7-137">hello biztonsági mentés most panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-137">hello Backup Now blade opens.</span></span>

  ![hello biztonsági mentés most panelen látható](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. <span data-ttu-id="ce4a7-139">Hello biztonsági mentés most paneljén kattintson hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-139">On hello Backup Now blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>

  ![hello utolsó nap hello biztonsági mentés most őrzi meg a helyreállítási pont beállítása](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  <span data-ttu-id="ce4a7-141">Központi telepítés értesítések lehetővé teszik, hogy hello biztonsági mentési feladat lett elindítva, és hogy kísérheti hello hello feladat hello biztonsági mentési feladatok lapján.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-141">Deployment notifications let you know hello backup job has been triggered, and that you can monitor hello progress of hello job on hello Backup jobs page.</span></span> <span data-ttu-id="ce4a7-142">Attól függően, hogy a virtuális gép mérete hello hello kezdeti biztonsági másolatot készít eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-142">Depending on hello size of your VM, creating hello initial backup may take a while.</span></span>

6. <span data-ttu-id="ce4a7-143">hello első biztonsági hello tároló irányítópult hello tooview vagy követése hello állapotának **biztonsági mentési feladatok** csempén kattintson **folyamatban**.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-143">tooview or track hello status of hello initial backup, on hello vault dashboard, on hello **Backup Jobs** tile click **In progress**.</span></span>

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  <span data-ttu-id="ce4a7-145">hello biztonsági mentési feladatok panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-145">hello Backup Jobs blade opens.</span></span>

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  <span data-ttu-id="ce4a7-147">A hello **biztonsági mentési feladatok** panelen láthatja, hogy az összes feladat hello állapota.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-147">In hello **Backup jobs** blade, you can see hello status of all jobs.</span></span> <span data-ttu-id="ce4a7-148">Ellenőrizze, ha még folyamatban van a virtuális gép biztonsági mentési feladata hello, vagy befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-148">Check if hello backup job for your VM is still in progress, or if it has finished.</span></span> <span data-ttu-id="ce4a7-149">Ha egy biztonsági mentési feladat befejeződött, hello állapota *befejezve*.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-149">When a backup job is finished, hello status is *Completed*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="ce4a7-150">Hello biztonsági mentési művelet részeként hello Azure Backup szolgáltatás kibocsát egy parancs toohello tartalék mellék minden virtuális gép tooflush ír, és egységes pillanatképet készít a.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-150">As a part of hello backup operation, hello Azure Backup service issues a command toohello backup extension in each VM tooflush all writes and take a consistent snapshot.</span></span>
  >
  >

## <a name="troubleshooting-errors"></a><span data-ttu-id="ce4a7-151">Kapcsolatos hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="ce4a7-151">Troubleshooting errors</span></span>
<span data-ttu-id="ce4a7-152">Ha biztonsági során problémákat tapasztal a virtuális gép, tekintse meg a hello [VM hibaelhárítási cikke](backup-azure-vms-troubleshoot.md) segítségét.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-152">If you run into issues while backing up your virtual machine, see hello [VM troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ce4a7-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ce4a7-153">Next steps</span></span>
<span data-ttu-id="ce4a7-154">Most, hogy a virtuális gép védetté, tekintse meg a következő cikkek toolearn kapcsolatos felügyeleti feladatok VM, hello és hogyan toorestore virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="ce4a7-154">Now that you have protected your VM, see hello following articles toolearn about VM management tasks, and how toorestore VMs.</span></span>

* [<span data-ttu-id="ce4a7-155">A virtuális gépek kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="ce4a7-155">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="ce4a7-156">Virtuális gépek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ce4a7-156">Restore virtual machines</span></span>](backup-azure-arm-restore-vms.md)
