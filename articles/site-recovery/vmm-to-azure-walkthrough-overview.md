---
title: "a VMM-ben a Hyper-V virtuális gépek aaaReplicate felhők tooAzure az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Áttekintést nyújt a VMM-felhők tooAzure hello Azure Site Recovery szolgáltatással a Hyper-V virtuális gépek replikálása"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.openlocfilehash: d6f729a49cc86ea07bebc4d7266fd7b58b3998f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-site-recovery-in-hello-azure-portal"></a>A VMM-felhők tooAzure hello Azure-portálon a Site Recovery segítségével a Hyper-V virtuális gépek replikálása
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [Klasszikus Azure portál](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klasszikus PowerShell](site-recovery-deploy-with-powershell.md)


Ez a cikk ismerteti a hello lépéseket szükséges tooreplicate áttekintését a helyszíni Hyper-V virtuális gépek (VM) kezelése a System Center Virtual Machine Manager (VMM) felhők tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

A cikk elolvasása után fűzhetnek bármely hello lap alján, vagy a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-hello-scenario-architecture"></a>1. lépés: Hello kialakítandó architektúra áttekintése

Indítsa el a központi telepítés, tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy tudomásul veszi összetevők hello toodeploy kell.

Nyissa meg túl[1. lépés: hello architektúra áttekintése](vmm-to-azure-walkthrough-architecture.md)

## <a name="step-2-review-prerequisites-and-limitations"></a>2. lépés: Tekintse át az Előfeltételek és korlátozások

Győződjön meg arról, hogy tudomásul veszi hello telepítési előfeltételek és korlátozások vonatkoznak.

**Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.
**A helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek**: Ellenőrizze, hogy a VMM-kiszolgálók és a Hyper-V-gazdagépek a szabályzatnak megfelelő és előkészített a Site Recovery üzembe helyezése.
**Virtuális gépek replikálása**: Ellenőrizze, hogy a virtuális gépek tooreplicate kívánt felel meg az Azure követelményeinek.

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](vmm-to-azure-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. lépés: Csomag kapacitás

Ha végzett teljes körű telepítésére, meg kell toofigure milyen replikációs erőforrásokat kell. Néhány az eszközök elérhető toohelp ehhez. Ha végzett a gyors tootest hello környezet kialakítása, kihagyhatja ezt a lépést.

Nyissa meg túl[3. lépés: kapacitástervezés](vmm-to-azure-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>4. lépés: A hálózat megtervezése

Meg kell toodo bizonyos hálózati tervezési tooensure hálózatleképezés hello esetben telepítésekor konfigurálható, hogy Azure virtuális gépekhez csatlakoztatott tooAzure virtuális hálózatok után lesz feladatátvételt hajt végre, és, hogy hozzá vannak rendelve megfelelő IP-címek.

Nyissa meg túl[4. lépés: hálózat tervezése](vmm-to-azure-walkthrough-network.md)


## <a name="step-5-prepare-azure-resources"></a>5. lépés: Felkészülés az Azure-erőforrások

Az Azure-fiók, a hálózatok és a tárolás beállítása. Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.

Nyissa meg túl[5. lépés: Azure előkészítése](vmm-to-azure-walkthrough-prepare-azure.md)

## <a name="step-6-prepare-vmm-and-hyper-v"></a>6. lépés: Készítse elő a VMM és a Hyper-V

Hello helyszíni VMM-kiszolgáló és a Hyper-V-gazdagépek előkészítése a Site Recovery üzembe helyezése.

Nyissa meg túl[6. lépés: a helyszíni kiszolgálók előkészítése](vmm-to-azure-walkthrough-vmm-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>7. lépés: Állítson be egy tárolóban.

Recovery Services-tároló beállítása. hello tároló konfigurációs beállításait tartalmazza, és koordinálja a replikációt.

[7. lépés: Állítson be egy tárolóban.](vmm-to-azure-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>8. lépés: A forrás- és beállítások konfigurálása

Hello forrás és cél replikációs helyek beállítása. Hello VMM server toohello tároló hozzáadása, és a Site Recovery-összetevők hello fájlok letöltéséhez. Futtassa az Azure Site Recovery Provider telepítőjét hello VMM-kiszolgálón. A telepítő telepíti a hello szolgáltató hello VMM-kiszolgálón, valamint hello-kiszolgáló regisztrálása hello tárolóban. Hello Microsoft Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen.

Nyissa meg túl[8. lépés: forrás és cél beállításainak konfigurálása](vmm-to-azure-walkthrough-source-target.md)

## <a name="step-9-configure-network-mapping"></a>9. lépés: Hálózatleképezés konfigurálása

Térkép helyszíni VMM-Virtuálisgép-hálózatok tooAzure virtuális hálózatok. A feladatátvétel után Azure virtuális gépek jönnek létre hello Azure-hálózatot, amely leképezhető toohello helyszíni Virtuálisgép-hálózat mely hello a Hyper-V adatforrás található.

Nyissa meg túl[9. lépés: hálózatleképezés konfigurálása](vmm-to-azure-walkthrough-network-mapping.md)


## <a name="step-10-set-up-a-replication-policy"></a>10. lépés: A replikációs házirend beállítása

Adja meg, hogyan a helyszíni virtuális gépek replikált tooAzure lesz.

Nyissa meg túl[10. lépés: a replikációs házirend beállítása](vmm-to-azure-walkthrough-replication.md)


## <a name="step-11-enable-replication-for-vms"></a>11. lépés: Virtuális gépek replikációjának engedélyezése

Válassza ki a kívánt tooreplicate hello virtuális gépeket. A folyamatban lévő változások replikálása követ egy virtuális Gépet a replikálási eseményindítókat hello kezdeti replikálás tooAzure engedélyezése esetén.

Nyissa meg túl[11. lépés: replikálás engedélyezése](vmm-to-azure-walkthrough-enable-replication.md)


## <a name="step-12-run-a-test-failover"></a>12. lépés: Feladatátvételi teszt futtatása

Futtassa a teszt feladatátvételi toomake meg arról, hogy minden a várt módon működik.

Nyissa meg túl[12. lépés: feladatátvételi teszt futtatása](vmm-to-azure-walkthrough-test-failover.md)


