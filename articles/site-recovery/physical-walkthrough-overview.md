---
title: "fizikai aaaReplicate a helyszíni kiszolgálók tooAzure az Azure Site Recovery szolgáltatással |} Microsoft Docs"
description: "Hello lépéseket áttekintést nyújt a helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure az Azure Site Recovery szolgáltatás hello munkaterheléseinek replikálására."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 20122f01-929a-4675-b85b-a9b99d2618bc
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: f801b9544072d4029ec06cc1abfd4ff370e852e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-physical-servers-tooazure-with-site-recovery"></a>A Site Recovery fizikai kiszolgálók tooAzure replikálása

Ez a cikk áttekintést hello lépéseket szükséges tooreplicate helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure, hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.


## <a name="step-1-review-architecture-and-prerequisites"></a>1. lépés: Tekintse át a architektúra és Előfeltételek

Központi telepítés megkezdése előtt tekintse át a hello kialakítandó architektúra, és így megértheti, hogy minden hello összetevőket tooset hello központi telepítése szükséges.

Nyissa meg túl[1. lépés: hello architektúra áttekintése](physical-walkthrough-architecture.md)


## <a name="step-2-review-prerequisites"></a>2. lépés: Felülvizsgálati Előfeltételek

Győződjön meg arról, hogy rendelkezik hello Előfeltételek az egyes telepítési összetevők:

- **Azure-Előfeltételek**: egy Microsoft Azure-fiók, az Azure-hálózatok és a storage-fiókok van szükség.
- **A helyszíni Site Recovery-összetevőkhöz**: egy a helyszíni Site Recovery összetevőt futtató gép van szüksége.
- **A replikált gép**: kiszolgálókat szeretne tooreplicate kell toocomply a helyszíni és az Azure-követelményeknek.

Nyissa meg túl[2. lépés: tekintse át az Előfeltételek és korlátozások](physical-walkthrough-prerequisites.md)

## <a name="step-3-plan-capacity"></a>3. lépés: Csomag kapacitás

Ha végzett teljes körű telepítésére toofigure milyen replikációs erőforrásokat kell módosítania. Ha végzett a gyors tootest hello környezet kialakítása, kihagyhatja ezt a lépést.

Nyissa meg túl[3. lépés: kapacitástervezés](physical-walkthrough-capacity.md)

## <a name="step-4-plan-networking"></a>4. lépés: A hálózat megtervezése

Egyes hálózati tooensure, hogy Azure virtuális gépekhez csatlakoztatott toonetworks feladatátvételt követően, és hogy, hogy rendelkezik-e hello megfelelő IP-címek tervezése kell toodo.

Nyissa meg túl[4. lépés: hálózat tervezése](physical-walkthrough-network.md)

##  <a name="step-5-prepare-azure-resources"></a>5. lépés: Felkészülés az Azure-erőforrások

Azure-hálózatok és a tárolás beállítása az megkezdése előtt. 

Nyissa meg túl[5. lépés: Azure előkészítése](physical-walkthrough-prepare-azure.md)


## <a name="step-6-set-up-a-vault"></a>6. lépés: Állítson be egy tárolóban.

A Recovery Services-tároló tooorchestrate beállításában és -replikáció kezelésére. Hello tároló beállításakor azt határozza meg, hogy tooreplicate, és hova tooreplicate azt.

Nyissa meg túl[6. lépés: állítson be egy tárolóban.](physical-walkthrough-create-vault.md)

## <a name="step-7-configure-source-and-target-settings"></a>7. lépés: A forrás- és beállítások konfigurálása

Hello forrás beállításainak konfigurálása, és a célhely (Azure). Az adatforrás-beállítások magában foglalja az egységes telepítő tooinstall hello helyszíni Site Recovery összetevőt futtató.

Nyissa meg túl[7. lépés: állítson be hello forrása és célja](physical-walkthrough-source-target.md)

## <a name="step-8-set-up-a-replication-policy"></a>8. lépés: A replikációs házirend beállítása

Állít be a házirend toospecify hogyan fizikai kiszolgálók replikálása kell.

Nyissa meg túl[8. lépés: a replikációs házirend beállítása](physical-walkthrough-replication.md)

## <a name="step-9-install-hello-mobility-service"></a>9. lépés: Hello mobilitási szolgáltatás telepítése

hello mobilitási szolgáltatást telepíteni kell minden kiszolgálón tooreplicate szeretné. Nincsenek néhány módon tooset hello szolgáltatás, a lekérés és küldés telepítést.

Nyissa meg túl[9. lépés: hello mobilitási szolgáltatás telepítése](physical-walkthrough-install-mobility.md)

## <a name="step-10-enable-replication"></a>10. lépés: A replikáció engedélyezése

Miután hello mobilitási szolgáltatás egy kiszolgálón fut, engedélyezze a replikációját. Miután engedélyezte, hello virtuális gép kezdeti replikálása következik be.

Nyissa meg túl[10. lépés: replikálás engedélyezése](physical-walkthrough-enable-replication.md)

## <a name="step-11-run-a-test-failover"></a>11. lépés: Feladatátvételi teszt futtatása

Miután a kezdeti replikáció befejezését követően, és a változások replikálása fut, a teszt feladatátvételi toomake meg arról, hogy minden megfelelően működik-e is futtathatja.

Nyissa meg túl[11. lépés: feladatátvételi teszt futtatása](physical-walkthrough-test-failover.md)

