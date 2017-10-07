---
title: "teljesítmény és méretezés, a Hyper-V Virtuálisgép-replikációt (VMM) tooAzure az Azure Site Recovery aaaPlan |} Microsoft Docs"
description: "Ez a cikk tooplan kapacitás és a skála felhasználhatja a tooAzure, az Azure Site Recovery VMM-ben a Hyper-V virtuális gépek replikálása felhők"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 9818ada9bb21f60ac00b3894696201b06630cb2b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-3-plan-capacity-and-scaling-for-hyper-v-with-vmm-tooazure-replication"></a>3. lépés: Teljesítmény és méretezés, a Hyper-V (a VMM-mel) tooAzure replikáció tervezése

Miután már tekintse át a hello [telepítésének előfeltételei](vmm-to-azure-walkthrough-prerequisites.md), használja a cikk toofigure kimenő jelent a kapacitástervezés és skálázás, replikálása esetén a helyszíni Hyper-V virtuális gépek a System Center Virtual Machine Manager (VMM) felhők tooAzure, a [Azure Site Recovery](site-recovery-overview.md).

A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="how-do-i-start-capacity-planning"></a>Hogyan kezdhetem meg, hogy a kapacitástervezés?


A replikálási környezetet, és terv sebesség hello adatokról, és ez a cikk kiemelt hello szempontok információt gyűjteni.


## <a name="gather-information"></a>Adatainak összegyűjtése

1. Gyűjtse össze a replikálási környezetre vonatkozó adatokat, többek között a virtuális gépeket, a virtuális gépekhez tartozó lemezeket, valamint a lemezenkénti tárterületeket.
2. Azonosítsa a replikált adatok napi adatváltozás (forgalom) sebessége. Töltse le a hello [Hyper-V kapacitástervezési eszköz](https://www.microsoft.com/download/details.aspx?id=39057) tooget hello változási sebessége. Azt javasoljuk, hogy futtatja az eszközt egy hét toocapture keresztül átlagok.
 

## <a name="figure-out-capacity"></a>Mérje fel, kapacitás

Megismerte a összefog hello adatok alapján futtatja a hello [hely helyreállítási kapacitás Planner eszköz](http://aka.ms/asr-capacity-planner-excel) tooanalyze a forráskörnyezetét és a munkaterhelések, sávszélesség igényeknek és a kiszolgáló erőforrásait hello forráshely becsléséhez, és hello (virtuális gép és tárterület stb), szükséges erőforrások hello célként megadott helyen. Hello eszköz módok néhány futtathatja:

- Gyors tervezési: a hálózat- és átlagos száma virtuális gépeket, a lemezek, a tároló és a változási sebessége alapján leképezések tooget futtassa hello eszközt ebben a módban.
- Részletes tervezési: hello futtathatja ezt a módot, és adja meg a virtuális gép szinten minden munkaterhelés részleteit. Virtuális gép kompatibilitási elemzéséhez, és a hálózat- és leképezések beolvasása.

Most futtassa hello eszközt:

1. Töltse le a hello [eszköz](http://aka.ms/asr-capacity-planner-excel)
2. toorun hello gyors planner, hajtsa végre a [ezeket az utasításokat](site-recovery-capacity-planner.md#run-the-quick-planner), és jelölje be hello forgatókönyv **Hyper-V tooAzure**.
3. toorun részletes planner hello hajtsa végre az [ezeket az utasításokat](site-recovery-capacity-planner.md#run-the-detailed-planner), és jelölje be hello forgatókönyv **Hyper-V tooAzure**.

## <a name="control-network-bandwidth"></a>Hálózati sávszélesség szabályozására

Miután megismerte a számított hello sávszélesség van szüksége, a replikációhoz használt sávszélesség mennyiségét ellenőrző hello lehetőségek több lehetősége van:

* **A sávszélesség szabályozását**: tooAzure replikáló Hyper-V-forgalom halad át egy adott Hyper-V gazdagépen. Beállíthatja a gazdagép-kiszolgálón hello sávszélesség szabályozását.
* **Sávszélesség befolyásolja**: beállításkulcsok több replikációhoz használt hello sávszélességet is szabályozhatja.

### <a name="throttle-bandwidth"></a>Sávszélesség szabályozása
1. Hello Microsoft Azure Backup szolgáltatás MMC beépülő modul megnyitásához hello Hyper-V gazdagép-kiszolgálón. Alapértelmezés szerint a Microsoft Azure Backup parancsikonja áll rendelkezésre, hello asztalon vagy a C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. Kattintson a beépülő modul hello **tulajdonságainak módosítása**.
3. A hello **sávszélesség-szabályozási** lapon válassza **engedélyezi az internetes sávszélesség szabályozásának a biztonsági mentési műveleteknél**, és állítsa be a munkaórákra hello és a munkaórákon kívüli időre. Érvényes tartomány: az 512 kbit/s too102 MB / s.

    ![Sávszélesség szabályozása](./media/vmm-to-azure-walkthrough-capacity/throttle2.png)

Is használhatja a hello [Set-OBMachineSetting](https://technet.microsoft.com/library/hh770409.aspx) parancsmag tooset sávszélesség-szabályozás. Íme egy példa:

    $mon = [System.DayOfWeek]::Monday
    $tue = [System.DayOfWeek]::Tuesday
    Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth  (512*1024) -NonWorkHourBandwidth (2048*1024)

A **Set-OBMachineSetting -NoThrottle** beállítás azt jelenti, hogy nincs szükség szabályozásra.

### <a name="influence-network-bandwidth"></a>A hálózati sávszélesség szabályozása
1. Hello beállításjegyzék lépjen a túl**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Replication**.
   * tooinfluence hello sávszélesség forgalom replikációs lemezen, módosítsa a hello érték hello **UploadThreadsPerVM**, vagy hozzon létre hello kulcsot, ha még nem létezik.
   * az Azure, a feladatátvételi forgalomhoz tooinfluence hello sávszélesség módosítható hello értéke **DownloadThreadsPerVM**.
2. hello alapértelmezett értéke 4. "A szükségesnél több erőforrással" hálózatban ezek a kulcsok hello alapértelmezett értékekhez módosítani kell. hello maximális 32. Forgalom toooptimize hello érték figyelése.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[4. lépés: hálózati terv](vmm-to-azure-walkthrough-network.md).
