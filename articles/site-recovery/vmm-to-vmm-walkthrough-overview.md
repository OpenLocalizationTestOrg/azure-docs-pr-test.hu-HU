---
title: "aaaReplicate Hyper-V virtuális gépek tooa VMM másodlagos hely az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Áttekintést nyújt a Hyper-V virtuális gépek tooa VMM másodlagos hely hello Azure-portál használatával replikálására."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 476ca82a-8f5c-4498-9dcf-e1011d60ed59
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: raynew
ms.openlocfilehash: 90e44bfc2237dfa7646fb2b7b1e533a7f6d83c50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site"></a>Hyper-V virtuális gépek VMM felhők tooa másodlagos VMM-hely replikálása

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klasszikus portál](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Ez a cikk áttekintést lépéseket szükséges tooreplicate helyszíni Hyper-V virtuális gépek (VM) kezelése a System Center Virtual Machine Manager (VMM) felhők, tooa másodlagos VMM helyre hello segítségével [Azure Site Recovery](site-recovery-overview.md)a hello Azure-portálon.

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-hello-scenario-architecture"></a>1. lépés: Hello kialakítandó architektúra áttekintése

Indítsa el a központi telepítés, tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy tudomásul veszi összetevők hello toodeploy kell.

Nyissa meg túl[1. lépés: tekintse át a hello architektúra](vmm-to-vmm-walkthrough-architecture.md).

## <a name="step-2-review-prerequisites-and-limitations"></a>2. lépés: Tekintse át az Előfeltételek és korlátozások

Győződjön meg arról, hogy tudomásul veszi hello telepítési előfeltételek és korlátozások vonatkoznak.

**Azure-Előfeltételek**: egy Microsoft Azure-előfizetés szükséges, és az Azure Recovery Services használó, tooorchestrate és -replikáció kezelésére.
**A helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek**: Ellenőrizze, hogy a VMM-kiszolgálók és a Hyper-V-gazdagépek a szabályzatnak megfelelő és előkészített a Site Recovery üzembe helyezése.

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmm-to-vmm-walkthrough-prerequisites.md).

## <a name="step-3-plan-networking"></a>3. lépés: A hálózat megtervezése

Toodo kell bizonyos hálózati tervezési tooensure állíthatja be a hálózatra való leképezés hello forgatókönyv központi telepítésekor a VMM Virtuálisgép-hálózatok között.

Nyissa meg túl[3. lépés: hálózati terv](vmm-to-vmm-walkthrough-network.md).


## <a name="step-4-prepare-vmm-and-hyper-v"></a>4. lépés: A VMM és a Hyper-V előkészítése

Hello VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése a Site Recovery üzembe helyezése.

Nyissa meg túl[4. lépés: készítse elő a helyszíni kiszolgálók](vmm-to-vmm-walkthrough-vmm-hyper-v.md).

## <a name="step-5-set-up-a-vault"></a>5. lépés: Állítson be egy tárolóban.

Recovery Services-tároló beállítása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.

[5. lépés: Állítson be egy tároló](vmm-to-vmm-walkthrough-create-vault.md).

## <a name="step-6-set-up-source-and-target-settings"></a>6. lépés: A forrás és cél beállítások megadása

Hello forrás és cél replikációs VMM helyek beállítása. Hello VMM kiszolgálók toohello tároló hozzáadása, és a Site Recovery-összetevők hello fájlok letöltéséhez. Futtassa az Azure Site Recovery Provider telepítőjét hello VMM-kiszolgálón. A telepítő telepíti a hello szolgáltató hello VMM-kiszolgálón, valamint hello-kiszolgáló regisztrálása hello tárolóban. Hello Microsoft Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen.

Nyissa meg túl[6. lépés: hello forrása és célja beállítások](vmm-to-vmm-walkthrough-source-target.md).

## <a name="step-7-configure-network-mapping"></a>7. lépés: Hálózatleképezés konfigurálása

Képezze le a VMM Virtuálisgép-hálózatok hello forrás és cél helyeket. A feladatátvétel után virtuális gépek jönnek létre hello célhálózat, hogy a maps toohello forrás Virtuálisgép-hálózat mely hello a Hyper-V virtuális gép található.

Nyissa meg túl[7. lépés: hálózatleképezés konfigurálása](vmm-to-vmm-walkthrough-network-mapping.md).


## <a name="step-8-set-up-a-replication-policy"></a>8. lépés: A replikációs házirend beállítása

Adja meg, hogyan virtuális gépeket a rendszer replikálja a VMM-helyek között.

Nyissa meg túl[8. lépés: állítson be egy replikációs házirendet](vmm-to-vmm-walkthrough-replication.md).


## <a name="step-9-enable-replication-for-vms"></a>9. lépés: Virtuális gépek replikációjának engedélyezése

Válassza ki a kívánt tooreplicate hello virtuális gépeket. A folyamatban lévő változások replikálása engedélyezése egy virtuális Gépet a replikálási eseményindítókat hello kezdeti replikálás toohello másodlagos helyet, majd.

Nyissa meg túl[9. lépés: engedélyezze a replikálást](vmm-to-vmm-walkthrough-enable-replication.md).


## <a name="step-10-run-a-test-failover"></a>10. lépés: Feladatátvételi teszt futtatása

Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

Nyissa meg túl[10. lépés: feladatátvételi teszt futtatása](vmm-to-vmm-walkthrough-test-failover.md).
