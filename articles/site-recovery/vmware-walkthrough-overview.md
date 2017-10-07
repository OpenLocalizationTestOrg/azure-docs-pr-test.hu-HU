---
title: "az Azure Site Recovery aaaReplicate VMware virtuális gépek tooAzure |} Microsoft Docs"
description: "Hello lépéseket áttekintést nyújt a VMware virtuális gépek tooAzure munkaterheléseinek replikálására"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 7104f67a3635b916048dcb61bca770c89f0c77fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-vmware-vms-tooazure-with-site-recovery"></a>A Site Recovery VMware virtuális gépek tooAzure replikálása

Ez a cikk áttekintést hello lépéseket szükséges tooreplicate helyszíni VMware virtuális gépek tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


![A központi telepítési folyamat](./media/vmware-walkthrough-overview/vmware-to-azure-process.png)

**1. ábra: Központi telepítési folyamat összegzése**

## <a name="step-1-review-architecture-and-prerequisites"></a>1. lépés: Tekintse át a architektúra és Előfeltételek

Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra, és győződjön meg arról, hogy toodeploy szükséges összes hello összetevők

Nyissa meg túl[1. lépés: hello architektúra áttekintése](vmware-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. lépés: Felülvizsgálati Előfeltételek

Győződjön meg arról, hogy rendelkezik hello Előfeltételek az egyes telepítési összetevők:

- **Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.
- **A helyszíni Site Recovery-összetevőkhöz**: egy a helyszíni Site Recovery összetevőt futtató gép van szüksége.
- **Helyszíni VMware Előfeltételek**: fiókok tooset kell, hogy a Site Recovery VMware-kiszolgálók és virtuális gépek hozzáférhessenek.
- **Virtuális gépek replikálása**: virtuális gépek szeretné, hogy tooreplicate kell toocomply az Azure-követelményeknek, és rendelkezik hello mobilitási szolgáltatás összetevőt.

Nyissa meg túl[2. lépés: tekintse át az Előfeltételek és korlátozások](vmware-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. lépés: Csomag kapacitás

Ha végzett teljes körű telepítésére toofigure milyen replikációs erőforrásokat kell módosítania. Néhány az eszközök elérhető toohelp ehhez. Nyissa meg tooStep 2. Ha végzett a gyors tootest hello környezet beállítása kihagyhatja ezt a lépést.

Nyissa meg túl[3. lépés: kapacitástervezés](vmware-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>4. lépés: A hálózat megtervezése

Egyes hálózati tooensure, hogy Azure virtuális gépekhez csatlakoztatott toonetworks feladatátvételt követően, és hogy, hogy rendelkezik-e hello megfelelő IP-címek tervezése kell toodo.

Nyissa meg túl[4. lépés: hálózat tervezése](vmware-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>5. lépés: Felkészülés az Azure-erőforrások

Azure-hálózatok és a tárolás beállítása az megkezdése előtt. Ehhez üzembe helyezése során, de javasoljuk, hogy ehhez megkezdése előtt.

Nyissa meg túl[5. lépés: Azure előkészítése](vmware-walkthrough-prepare-azure.md)


## <a name="step-6-prepare-vmware"></a>6. lépés: A VMware előkészítése

Fiókok, amelyek a Site Recovery segítségével tooset lesz szüksége:

- Hozzáférés VMware virtualizálási kiszolgálók tooautomatically észleli a virtuális gépek.
- Virtuális gépek tooinstall hello mobilitási szolgáltatás eléréséhez. Minden virtuális gép kívánt tooreplicate rendelkeznie kell hello mobilitási szolgáltatás ügynöke telepítve előtt is engedélyezze a replikációját.

Nyissa meg túl[6. lépés: Felkészülés VMware](vmware-walkthrough-prepare-vmware.md)

## <a name="step-7-set-up-a-vault"></a>7. lépés: Állítson be egy tárolóban.

A Recovery Services-tároló tooorchestrate mentése tooset kell, és replikáció kezelésére. Hello tároló beállításakor azt határozza meg, hogy tooreplicate, és hova tooreplicate azt.

Nyissa meg túl[7. lépés: állítson be egy tárolóban.](vmware-walkthrough-create-vault.md)

## <a name="step-8-configure-source-and-target-settings"></a>8. lépés: A forrás- és beállítások konfigurálása

Hello forrás és cél-replikációhoz használt beállítása. Adatforrás-beállítások beállítása magában foglalja az egységes telepítő tooinstall hello helyszíni Site Recovery összetevőt futtató.

Nyissa meg túl[8. lépés: állítson be hello forrása és célja](vmware-walkthrough-source-target.md)

## <a name="step-9-set-up-a-replication-policy"></a>9. lépés: A replikációs házirend beállítása

Egy házirend toospecify replikációs beállítások vonatkozóan beállított VMware virtuális gépek hello tárolóban lévő állapottal.

Nyissa meg túl[9. lépés: a replikációs házirend beállítása](vmware-walkthrough-replication.md)

## <a name="step-10-install-hello-mobility-service"></a>10. lépés: Hello mobilitási szolgáltatás telepítése

kell telepíteni a mobilitási szolgáltatás hello tooreplicate kívánt összes virtuális Géphez. Nincsenek néhány módon tooset hello szolgáltatás a lekérés és küldés telepítést.

Nyissa meg túl[10. lépés: hello mobilitási szolgáltatás telepítése](vmware-walkthrough-install-mobility.md)

## <a name="step-11-enable-replication"></a>11. lépés: A replikáció engedélyezése

Hello mobilitási szolgáltatás fut a virtuális gép után engedélyezze a replikációját. Miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.

Nyissa meg túl[11. lépés: replikálás engedélyezése](vmware-walkthrough-enable-replication.md)

## <a name="step-12-run-a-test-failover"></a>12. lépés: Feladatátvételi teszt futtatása

Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.

Nyissa meg túl[12. lépés: feladatátvételi teszt futtatása](vmware-walkthrough-test-failover.md)
