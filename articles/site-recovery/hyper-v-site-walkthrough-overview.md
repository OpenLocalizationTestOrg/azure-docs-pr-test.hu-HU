---
title: "az Azure Site Recovery aaaReplicate Hyper-V virtuális gépek tooAzure |} Microsoft Docs"
description: "Ismerteti, hogyan tooorchestrate replikációjának, feladatátvételének és helyreállításának helyszíni Hyper-V virtuális gépek tooAzure"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: efddd986-bc13-4a1d-932d-5484cdc7ad8d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: ab9cd14149ef32a416428d0f4327aa18b042e9c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-without-vmm-tooazure"></a>Hyper-V virtuális gépek (VMM nélkül) tooAzure replikálása 

> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [Klasszikus Azure portál](site-recovery-hyper-v-site-to-azure-classic.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
>
>

Ez a cikk áttekintést hello szükséges lépések tooreplicate helyszíni Hyper-V virtuális gépek tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) a hello Azure-portálon. A központi telepítés a Hyper-V virtuális gépek felügyelete nem a System Center Virtual Machine Manager (VMM).


A cikk elolvasása után bármely fűzhetnek hello lap alján, vagy technikai kérdéseket hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="step-1-review-architecture-and-prerequisites"></a>1. lépés: Tekintse át a architektúra és Előfeltételek

Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy toodeploy szükséges összes hello összetevők

Nyissa meg túl[1. lépés: hello architektúra áttekintése](hyper-v-site-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. lépés: Felülvizsgálati Előfeltételek

Győződjön meg arról, hogy rendelkezik hello Előfeltételek az egyes telepítési összetevők:

- **Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.
- **Helyszíni Hyper-V előfeltételei**: Győződjön meg arról, hogy a Hyper-V-gazdagépek van előkészítve a Site Recovery üzembe helyezése.
- **Virtuális gépek replikálása**: kívánt tooreplicate kell toocomply Azure-követelményekkel rendelkező virtuális gépeket.

Nyissa meg túl[2. lépés: Ellenőrizze az Előfeltételek és korlátozások](hyper-v-site-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. lépés: Csomag kapacitás

Ha végzett teljes körű telepítésére toofigure milyen replikációs erőforrásokat kell módosítania. Néhány az eszközök elérhető toohelp ehhez. Nyissa meg tooStep 2. Ha végzett a gyors tootest hello környezet beállítása kihagyhatja ezt a lépést.

Nyissa meg túl[3. lépés: kapacitástervezés](hyper-v-site-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>4. lépés: A hálózat megtervezése

Egyes hálózati tooensure, hogy Azure virtuális gépekhez csatlakoztatott toonetworks feladatátvételt követően, és hogy, hogy rendelkezik-e hello megfelelő IP-címek tervezése kell toodo.

Nyissa meg túl[4. lépés: hálózat tervezése](hyper-v-site-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>5. lépés: Felkészülés az Azure-erőforrások

Azure-hálózatok és a tárolás beállítása az megkezdése előtt. Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.

Nyissa meg túl[5. lépés: Azure előkészítése](hyper-v-site-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-hyper-v"></a>6. lépés: Felkészülés a Hyper-V

Győződjön meg arról, hogy a Hyper-V kiszolgálók megfelelnek-e a Site Recovery üzembe helyezés feltételeit.

Nyissa meg túl[6. lépés: Felkészülés a Hyper-V](hyper-v-site-walkthrough-prepare-hyper-v.md)

## <a name="step-7-set-up-a-vault"></a>7. lépés: Állítson be egy tárolóban.

A Recovery Services-tároló tooorchestrate mentése tooset kell, és replikáció kezelésére. Hello tároló beállításakor azt határozza meg, hogy tooreplicate, és hova tooreplicate azt.

Nyissa meg túl[7. lépés: a tároló létrehozása](hyper-v-site-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>8. lépés: A forrás- és beállítások konfigurálása

Hello forrás és cél-replikációhoz használt beállítása. Adatforrás-beállítások beállítása magában foglalja hozzáadása Hyper-V gazdagépek tooa Hyper-V hely, hello Site Recovery Provider és Recovery Services Agent ügynök telepítése minden Hyper-V gazdagépen, és a Recovery Services-tároló hello hello hely regisztrálása.

Nyissa meg túl[8. lépés: állítson be hello forrása és célja](hyper-v-site-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>9. lépés: A replikációs házirend beállítása

Egy házirend toospecify replikációs beállítások vonatkozóan beállított Hyper-V virtuális gépek hello tárolóban lévő állapottal.

Nyissa meg túl[9. lépés: a replikációs házirend beállítása](hyper-v-site-walkthrough-replication.md)


## <a name="step-10-enable-replication"></a>10. lépés: A replikáció engedélyezése

Miután egy replikációs házirendet helyen, miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.

Nyissa meg túl[10. lépés: replikálás engedélyezése](hyper-v-site-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>11. lépés: Feladatátvételi teszt futtatása

Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.

Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](hyper-v-site-walkthrough-test-failover.md)
