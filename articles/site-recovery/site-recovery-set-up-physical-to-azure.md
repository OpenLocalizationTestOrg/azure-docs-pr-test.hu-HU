---
title: "Hello forrás környezetet (fizikai kiszolgálók tooAzure) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart az Azure Windows vagy Linux operációs rendszert futtató fizikai kiszolgálók replikálása."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.openlocfilehash: d4702265bf36910015685d2bba99d6e577531bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-physical-server-tooazure"></a>Hello forrás környezetet (fizikai kiszolgáló tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fizikai tooAzure](./site-recovery-set-up-physical-to-azure.md)

Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart az Azure Windows vagy Linux operációs rendszert futtató fizikai kiszolgálók replikálása.

## <a name="prerequisites"></a>Előfeltételek

hello cikk feltételezi, hogy már rendelkezik:
1. A Recovery Services-tárolónak a hello [Azure-portálon](http://portal.azure.com "Azure-portálon").
3. A fizikai számítógép melyik tooinstall hello konfigurációs kiszolgálón.

### <a name="configuration-server-minimum-requirements"></a>Konfigurációs kiszolgáló minimális követelményeknek
hello a következő táblázat felsorolja a hello minimális hardver, szoftverek és hálózati követelményei a konfigurációs kiszolgáló.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Hello konfigurációs kiszolgáló által a HTTPS-alapú proxykiszolgálókat nem támogatottak.

## <a name="choose-your-protection-goals"></a>Védelmi célok megválasztása

1. A hello Azure-portálon, válassza a toohello **Recovery Services** -tárolók panelt, és válassza ki a tároló.
2. A hello **erőforrás** hello tároló menüjében kattintson **bevezetés** > **Site Recovery** > **1. lépés: előkészítése Infrastruktúra** > **védelmi cél**.

    ![Célok megválasztása](./media/site-recovery-set-up-physical-to-azure/choose-goals.png)
3. A **védelmi cél**, jelölje be **tooAzure** és **nem virtualizált vagy egyéb**, és kattintson a **OK**.

    ![Célok megválasztása](./media/site-recovery-set-up-physical-to-azure/physical-protection-goal.PNG)

## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása

1. A **forrás előkészítése**, ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló** tooadd egyet.

  ![A forrás beállítása](./media/site-recovery-set-up-physical-to-azure/plus-config-srv.png)
2. A hello **kiszolgáló hozzáadása** panelen ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.
5. Hello tárolóbeli regisztrációs kulcs letöltése. Az egységes telepítő szüksége hello regisztrációs kulcsot. hello kulcs a generálásától öt napig esetén érvényes.

    ![A forrás beállítása](./media/site-recovery-set-up-physical-to-azure/set-source2.png)
6. Hello gépen hello kiszolgálóként használ, futtassa **Azure Site Recovery az egységes telepítő** tooinstall hello konfigurációs kiszolgáló, a hello folyamatkiszolgáló és a hello fő célkiszolgáló.

#### <a name="run-azure-site-recovery-unified-setup"></a>Futtassa az Azure Site Recovery egyesített telepítő

> [!TIP]
> Konfigurációs kiszolgáló regisztrálása meghiúsul, ha a számítógép rendszerórája hello idő ki a helyi idő több mint öt perc. A rendszer szinkronizálást, és egy [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) hello telepítés megkezdése előtt.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurációs kiszolgálón a parancssorból is telepíthető. További információkért lásd: [telepítése konfigurációs kiszolgáló parancssori eszközökkel](http://aka.ms/installconfigsrv).


## <a name="common-issues"></a>Gyakori problémák

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Következő lépések

Magában foglalja a következő lépés [a célkörnyezet beállítása](./site-recovery-prepare-target-physical-to-azure.md) az Azure-ban.
