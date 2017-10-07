---
title: "hello forrás és cél VMware replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Hello lépéseket tooset forrása és célja beállítások megadása a VMware virtuális gépek tooAzure tárolás az Azure Site Recovery replikációs összesíti"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99e422e-daf7-4fa8-af3c-af2340340136
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: ef33a44bc5da17afb0442be63f576925f5b9a8b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-vmware-replication-tooazure"></a>8. lépés: Hello forrása és célja a VMware-replikáció tooAzure beállítása

Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni VMware virtuális gépek tooAzure, használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello konfigurációs kiszolgáló, regisztrálja azt hello tárolóban, és virtuális gépek felderítése.

1. Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.
2. Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.
5. Hello tárolóbeli regisztrációs kulcs letöltése. Ez szükséges az egységes telepítő futtatásakor. hello kulcs a generálásától öt napig esetén érvényes.

   ![A forrás beállítása](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a>Hello tároló hello konfigurációs kiszolgáló regisztrálása

Tegye hello következő előtt indítsa el, majd futtassa az egységes telepítő tooinstall hello konfigurációs kiszolgáló, hello folyamatkiszolgáló és fő célkiszolgáló hello.
    - Gyors áttekintő videó beolvasása

        > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video1-Source-Infrastructure-Setup/player]

    - Hello konfigurációs kiszolgálón VM, győződjön meg arról, hogy hello rendszeróra szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Meg kell felelnie. Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.
    - Futtassa a telepítőt a helyi rendszergazdájaként hello virtuális gép konfigurációs kiszolgálón.
    - Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális gép hello.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurációs kiszolgálón is telepíthető [hello parancssorból](http://aka.ms/installconfigsrv).



## <a name="connect-toovmware-servers"></a>Csatlakozás tooVMware kiszolgálók

tooallow Azure Site Recovery toodiscover futó virtuális gépek a helyszíni környezetben, kell tooconnect a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek Site Recovery szolgáltatással. Vegye figyelembe a következő hello megkezdése előtt:

- Ha hello vCenter-kiszolgáló vagy vSphere állomások tooSite helyreállítási rendszergazdai jogosultságok nélküli fiókkal hello kiszolgálón, a hello fiókot kell ezeket a jogokat engedélyezve:
    - Datacenter, Datastore, mappa, állomás, hálózati, erőforrás, a virtuális gép, vSphere elosztott kapcsoló.
    - hello vCenter-kiszolgálót kell tárolási nézetek engedélyekkel.
- VMware-kiszolgálók tooSite helyreállítási hozzáadásakor 15 percig is tarthat, vagy hosszabb ideig őket tooappear hello portálon.

### <a name="add-hello-account-for-automatic-discovery"></a>Automatikus felderítés hello fiók hozzáadása

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="set-up-a-connection"></a>Kapcsolat beállítása

Csatlakozás a tooservers az alábbiak szerint:

1. Válassza ki **+ vCenter** toostart VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.
2. A **vCenter hozzáadása**, adjon meg egy rövid nevet hello vSphere gazdagép vagy a vCenter-kiszolgálóhoz, és adja meg a hello IP-cím vagy hello kiszolgáló teljes Tartományneve.
3. Hello port hagyja 443-as, kivéve, ha a VMware Server kérések egy másik portra beállított toolisten. Válassza ki a hello fiók, amely tooconnect toohello VMware vCenter vagy vSphere ESXi-kiszolgálón. Kattintson az **OK** gombra.
4. A Site Recovery csatlakoztatja tooVMware kiszolgálókat hello segítségével megadott beállításokat, és felderíti a virtuális gépek.

> [!NOTE]
> Egy kiszolgáló vagy egy olyan fiókkal, amely nem rendelkezik rendszergazdai jogosultságokkal hello vCenter vagy a gazdagép-kiszolgálói gazdagép hozzáadása, győződjön meg arról, hogy hello fiókja rendelkezik-e ezek a jogosultságok engedélyezve: Datacenter, Datastore, mappa, Host, hálózati, erőforrás, a virtuális gép, és a vSphere elosztott kapcsoló. Emellett hello VMware vCenter kiszolgálót kell hello tárolási nézetek jogosultság engedélyezve van.


## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Hello célkörnyezet beállítása előtt győződjön meg arról, hogy az Azure storage-fiók és a virtuális hálózat beállítása.

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a kívánt toouse Azure-előfizetés hello.
2. Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

   ![cél](./media/vmware-walkthrough-source-target/gs-target.png)
4. Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, egy erőforrás-kezelő fiók vagy a hálózati beágyazott toocreate.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[9. lépés: a replikációs házirend beállítása](vmware-walkthrough-replication.md)
