---
title: "a VMware tooAzure replikáció a mobilitási szolgáltatás aaaInstall hello |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooinstall hello a mobilitási szolgáltatás ügynöke VMware tooAzure replikáció hello Azure Site Recovery szolgáltatásban."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 3189fbcd-6b5b-4ffb-b5a9-e2080c37f9d9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: d3b7bc9c4d84d13317e0b0b47adcf38e8c41d0bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-10-install-hello-mobility-service"></a>10. lépés: Hello mobilitási szolgáltatás telepítése


Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni VMware virtuális gépek tooAzure, használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

hello mobilitási szolgáltatás a gépen végbemenő adatírásokat, és továbbítja őket toohello folyamatkiszolgáló. Azt kell telepíteni minden számítógépen, amelyet az tooreplicate tooAzure.

Hello Site Recovery folyamat kiszolgálóról leküldéses telepítést használ, ha engedélyezve van a replikáció hello mobilitási szolgáltatás manuális telepítése, vagy a System Center Configuration Manager eszközzel. Ügyfélleküldéses telepítés használatakor hello szolgáltatást, a virtuális gép hello Ha engedélyezve van a replikáció.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Manuális telepítés

1. Ellenőrizze a hello [Előfeltételek](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) manuális telepítésre.
2. Hajtsa végre a [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) hello portál használatával manuális telepítésre.
3. Ha jobban szeret tooinstall hello parancssorból, hajtsa végre az [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-hello-process-server"></a>Hello folyamatkiszolgáló telepítése

Ha azt szeretné toopush hello mobilitási szolgáltatás telepítési hello folyamat kiszolgálóról, ha engedélyezi a virtuális gépek replikálása, egy hello folyamat server tooaccess hello virtuális gép által használt fiók szükséges. hello fiók hello ügyfélleküldéses telepítés csak szolgál.

1. Rendelkeznie kell [hoztak létre fiókot](vmware-walkthrough-prepare-vmware.md) , amely leküldéses telepítéséhez használható. Ezután meg hello fióknevet, amelyet a toouse az adatforrás-beállítások konfigurálásakor a Site Recovery üzembe helyezése során.
2. Hajtsa végre [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Ha azt szeretné, hogy toopush hello mobilitási szolgáltatás a Windows vagy Linux rendszerű virtuális gépeken.

## <a name="other-methods"></a>Más módszerrel

További információ [hello a mobilitási szolgáltatás a Configuration Manager telepítése](site-recovery-install-mobility-service-using-sccm.md), vagy [Azure Automation DSC](site-recovery-automate-mobility-service-install.md).

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[11. lépés: replikálás engedélyezése](vmware-walkthrough-enable-replication.md)
