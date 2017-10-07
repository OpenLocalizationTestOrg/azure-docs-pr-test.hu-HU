---
title: "a fizikai kiszolgáló tooAzure replikáció a mobilitási szolgáltatás aaaInstall hello |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooinstall hello a mobilitási szolgáltatás ügynöke fizikai kiszolgálók replikálása tooAzure hello Azure Site Recovery szolgáltatásban."
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
ms.openlocfilehash: 48fd2c0ffe67875ed446c8167c2ae7f90d3f537c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-9-install-hello-mobility-service"></a>9. lépés: Hello mobilitási szolgáltatás telepítése


Ez a cikk ismerteti, hogyan tooinstall hello mobilitási szolgáltatás összetevőt replikálása esetén a helyszíni windowsos/Linuxos fizikai kiszolgálók tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

hello mobilitási szolgáltatás a gépen végbemenő adatírásokat, és továbbítja őket toohello folyamatkiszolgáló. Akkor kell telepíteni az egyes kiszolgálókon, amelyet az tooreplicate tooAzure.

Hello mobilitási szolgáltatást manuálisan telepítheti, vagy a leküldéses telepítéssel hello Site Recovery feldolgozni kiszolgáló, amikor a replikáció engedélyezett-e, vagy egy eszköz, például a System Center Configuration Manager segítségével. Az ügyfélleküldéses telepítés használatakor hello szolgáltatást hello kiszolgálón replikációs engedélyezésekor.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="install-manually"></a>Manuális telepítés

1. Ellenőrizze a hello [Előfeltételek](site-recovery-vmware-to-azure-install-mob-svc.md#prerequisites) manuális telepítésre.
2. Hajtsa végre a [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-by-using-the-gui) hello portál használatával manuális telepítésre.
3. Ha jobban szeret tooinstall hello parancssorból, hajtsa végre az [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-manually-at-a-command-prompt).

## <a name="install-from-hello-process-server"></a>Hello folyamatkiszolgáló telepítése

Ha azt szeretné toopush hello mobilitási szolgáltatás telepítési hello folyamat kiszolgálóról, ha engedélyezi a gépek replikálása, egy hello folyamat tooaccess hello kiszolgálógép által használt fiók szükséges. hello fiók hello ügyfélleküldéses telepítés csak szolgál.

1. Ha még nem hozott létre egy fiókot, ehhez használja ezeket az irányelveket:

    - A tartományi vagy helyi fiók használható
    - A Windows Ha nem használ egy olyan tartományi fiók kell toodisable távelérési felhasználói vezérlő hello helyi számítógépen. Ez, hello regisztrálni a toodo **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, hello DWORD bejegyzés hozzáadása **LocalAccountTokenFilterPolicy**, 1 értékű.
    - Ha a parancssori felület a Windows hello bejegyzés tooadd szeretne, írja be:

        ```
        REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.
        ```

    - Linux hello fióknak root forráskiszolgálón hello Linux kell lennie.

2. Hajtsa végre [ezeket az utasításokat](site-recovery-vmware-to-azure-install-mob-svc.md#install-mobility-service-by-push-installation-from-azure-site-recovery) Ha azt szeretné, hogy toopush hello mobilitási szolgáltatás a Windows vagy Linux rendszerű virtuális gépeken.

## <a name="other-installation-methods"></a>Egyéb telepítési módszerek

- [További tudnivalók](site-recovery-install-mobility-service-using-sccm.md) hello a mobilitási szolgáltatás a Configuration Manager telepítése
- [További tudnivalók](site-recovery-automate-mobility-service-install.md) telepítése az Azure Automation DSC Szolgáltatásban.


## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[10. lépés: replikálás engedélyezése](physical-walkthrough-enable-replication.md)
