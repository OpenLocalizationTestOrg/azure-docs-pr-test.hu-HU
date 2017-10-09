---
title: "hello forrás és cél a fizikai kiszolgáló replikációs tooAzure az Azure Site Recovery aaaSet |} Microsoft Docs"
description: "Összefoglalja a hello lépéseket tooset hello Azure Site Recovery szolgáltatás a fizikai kiszolgálók tooAzure tárhely replikálás forrás- és beállításainak mentése"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 2e3d03bc-4e53-4590-8a01-f702d3cc9500
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e75ef23174d77509bf54380eba8f251a7337a45f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-7-set-up-hello-source-and-target-for-physical-server-replication-tooazure"></a>7. lépés: Hello forrása és célja a fizikai kiszolgáló replikációs tooAzure beállítása

Ez a cikk ismerteti, hogyan tooconfigure forrás és cél beállításai replikálása esetén a helyszíni fizikai kiszolgálók tooAzure használatával hello [Azure Site Recovery](site-recovery-overview.md) szolgáltatással hello Azure-portálon.

Ez a cikk vagy a hello hello alsó megjegyzések és kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

Hello konfigurációs kiszolgáló, és regisztrálja azt hello tárolóban gépek észlelése.

1. Kattintson a **helyreállítási hely** > **1. lépés: infrastruktúra előkészítése** > **forrás**.
2. Ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló**.
3. A **kiszolgáló hozzáadása**, ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.
5. Hello tárolóbeli regisztrációs kulcs letöltése. Ez szükséges az egységes telepítő futtatásakor. hello kulcs a generálásától öt napig esetén érvényes.

   ![A forrás beállítása](./media/vmware-walkthrough-source-target/set-source2.png)


## <a name="register-hello-configuration-server-in-hello-vault"></a>Hello tároló hello konfigurációs kiszolgáló regisztrálása

Tegye hello következő előtt indítsa el, majd futtassa az egységes telepítő tooinstall hello konfigurációs kiszolgáló, hello folyamatkiszolgáló és fő célkiszolgáló hello. videó hello VMware virtuális gép tooAzure replikációs hello összetevőket beállításának a módját ismerteti. Azonban hello ugyanazt a folyamatot nem érvényes a fizikai kiszolgáló tooAzure replikációs.

- Hello konfigurációs kiszolgálón VM, győződjön meg arról, hogy hello rendszeróra szinkronizálva van a [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Meg kell felelnie. Ha az előtérben 15 perc vagy mögött található, a telepítés meghiúsulhat.
- Futtassa a telepítőt a helyi rendszergazda, a hello konfigurációs kiszolgáló számítógépén.
- Győződjön meg arról, hogy a TLS 1.0 engedélyezve van a virtuális gép hello.


[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurációs kiszolgálón is telepíthető [hello parancssorból](http://aka.ms/installconfigsrv).




## <a name="set-up-hello-target-environment"></a>Hello célkörnyezet beállítása

Hello célkörnyezet beállítása előtt győződjön meg arról, hogy az Azure storage-fiók és a virtuális hálózat beállítása.

1. Kattintson a **infrastruktúra előkészítése** > **cél**, és válassza ki a kívánt toouse Azure-előfizetés hello.
2. Adja meg-e a cél telepítési modell a Resource Manager-alapú, vagy a klasszikus.
3. A Site Recovery ellenőrzi, hogy rendelkezik-e legalább egy kompatibilis Azure-tárfiókkal és -hálózattal.

   ![cél](./media/physical-walkthrough-source-target/gs-target.png)

4. Ha még nem hozott létre a storage-fiók vagy a hálózaton, kattintson a **+ tárfiók** vagy **+ hálózat**, egy erőforrás-kezelő fiók vagy a hálózati beágyazott toocreate.

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[8. lépés: a replikációs házirend beállítása](physical-walkthrough-replication.md)
