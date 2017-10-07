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
# <a name="back-up-azure-virtual-machines-tooa-recovery-services-vault"></a>Az Azure virtuális gépek biztonsági mentése tooa Recovery Services tároló
> [!div class="op_single_selector"]
> * [Készítsen biztonsági másolatot a virtuális gépek tooRecovery Services-tároló](backup-azure-arm-vms.md)
> * [Készítsen biztonsági másolatot a virtuális gépek tooBackup tároló](backup-azure-vms.md)
>
>

Ez a cikk részletesen, hogyan mentése Azure virtuális gépeken (telepített Resource Manager és klasszikus telepített) tooa Recovery Services tooback tároló. Virtuális gépek biztonsági mentéséről hello munka nagyobb része hello előkészítése. Készítsen biztonsági másolatot, vagy egy virtuális gép védelme, hajtsa végre a következő hello [Előfeltételek](backup-azure-arm-vms-prepare.md) tooprepare környezetében a virtuális gépek védelmét. Ha az Előfeltételek hello, majd is kezdeményezhető hello biztonsági mentési művelet tootake pillanatképek a virtuális gép.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

További információkért lásd: hello cikkeket a [az Azure virtuális gép biztonsági mentési infrastruktúrájának megtervezésével](backup-azure-vms-introduction.md) és [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-hello-backup-job"></a>Eseményindító hello biztonsági mentési feladat
Recovery Services-tároló hello társított hello biztonsági mentési házirend határozza meg, milyen gyakran és mikor hello biztonsági mentést futtat. Alapértelmezés szerint a hello első ütemezett biztonsági mentés hello kezdeti biztonsági másolatot. Amíg nem történik hello kezdeti biztonsági másolatot, a hello hello utolsó biztonsági mentés állapotának **biztonsági mentési feladatok** panelt jeleníti meg, mint a **figyelmeztetés (függőben lévő kezdeti biztonsági másolatot)**.

![Biztonsági mentés függőben](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Kivéve, ha a kezdeti biztonsági másolatot esedékes toobegin hamarosan, javasoljuk, hogy **biztonsági másolat készítése**. hello következő eljárás elindul hello tároló irányítópulton. Ez az eljárás a hello kezdeti biztonsági mentési feladat fut, miután végrehajtotta a szükséges előfeltételek maradéktalanul szolgál. Ha már korábban lefutott hello kezdeti biztonsági mentési feladat, ez az eljárás nem érhető el. hello tartozó biztonsági mentési házirend meghatározza, hogy hello következő biztonsági mentési feladat.  

toorun hello kezdeti biztonsági mentési feladat:

1. Az hello tároló irányítópultján kattintson a hello szám alatt **biztonsági mentés elemek**, vagy kattintson a hello **biztonsági mentés elemek** csempe. <br/>
  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Hello **biztonsági mentés elemek** panel nyílik meg.

  ![Elemek biztonsági mentése](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. A hello **biztonsági mentés elemek** panelen, jelölje be hello elemet.

  ![Beállítások ikon](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Hello **biztonsági mentés elemek** listában megnyílik. <br/>

  ![Biztonsági mentési feladat elindul](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. A hello **biztonsági mentés elemek** listában, kattintson a hello folytatást jelző pontokra **...**  tooopen hello helyi menü.

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu.png)

  hello helyi menü megjelenik.

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Hello helyi menüben, kattintson a **biztonsági mentés most**.

  ![Helyi menü](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  hello biztonsági mentés most panel nyílik meg.

  ![hello biztonsági mentés most panelen látható](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. Hello biztonsági mentés most paneljén kattintson hello naptár ikonra, használja a hello Naptár vezérlőelem tooselect hello utolsó napja a helyreállítási pont őrzi meg, és kattintson a **biztonsági mentés**.

  ![hello utolsó nap hello biztonsági mentés most őrzi meg a helyreállítási pont beállítása](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Központi telepítés értesítések lehetővé teszik, hogy hello biztonsági mentési feladat lett elindítva, és hogy kísérheti hello hello feladat hello biztonsági mentési feladatok lapján. Attól függően, hogy a virtuális gép mérete hello hello kezdeti biztonsági másolatot készít eltarthat egy ideig.

6. hello első biztonsági hello tároló irányítópult hello tooview vagy követése hello állapotának **biztonsági mentési feladatok** csempén kattintson **folyamatban**.

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  hello biztonsági mentési feladatok panel nyílik meg.

  ![Biztonsági mentési feladatok csempe](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  A hello **biztonsági mentési feladatok** panelen láthatja, hogy az összes feladat hello állapota. Ellenőrizze, ha még folyamatban van a virtuális gép biztonsági mentési feladata hello, vagy befejeződött. Ha egy biztonsági mentési feladat befejeződött, hello állapota *befejezve*.

  > [!NOTE]
  > Hello biztonsági mentési művelet részeként hello Azure Backup szolgáltatás kibocsát egy parancs toohello tartalék mellék minden virtuális gép tooflush ír, és egységes pillanatképet készít a.
  >
  >

## <a name="troubleshooting-errors"></a>Kapcsolatos hibák elhárítása
Ha biztonsági során problémákat tapasztal a virtuális gép, tekintse meg a hello [VM hibaelhárítási cikke](backup-azure-vms-troubleshoot.md) segítségét.

## <a name="next-steps"></a>Következő lépések
Most, hogy a virtuális gép védetté, tekintse meg a következő cikkek toolearn kapcsolatos felügyeleti feladatok VM, hello és hogyan toorestore virtuális gépeket.

* [A virtuális gépek kezelése és figyelése](backup-azure-manage-vms.md)
* [Virtuális gépek visszaállítása](backup-azure-arm-restore-vms.md)
