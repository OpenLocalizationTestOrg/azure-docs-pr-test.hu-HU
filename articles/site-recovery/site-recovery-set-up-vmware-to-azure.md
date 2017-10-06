---
title: "Hello forrás környezetet (VMware tooAzure) |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart replikálása VMware virtuális gépek tooAzure."
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
ms.openlocfilehash: cbeeffc1061b11223e12ff3e237fa7e55b999aca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-hello-source-environment-vmware-tooazure"></a>Hello forrás környezetet (VMware tooAzure)
> [!div class="op_single_selector"]
> * [VMware tooAzure](./site-recovery-set-up-vmware-to-azure.md)
> * [Fizikai tooAzure](./site-recovery-set-up-physical-to-azure.md)

Ez a cikk ismerteti, hogyan tooset fel a helyszíni környezet toostart replikáló virtuális gépek futnak a VMware tooAzure.

## <a name="prerequisites"></a>Előfeltételek

hello cikk feltételezi, hogy már létrehozta:
- A Recovery Services-tároló a hello [Azure-portálon](http://portal.azure.com "Azure-portálon").
- A VMware vcenter programban, amely nem használható egy külön fiókot [automatikus felderítés](./site-recovery-vmware-to-azure.md).
- A virtuális gép mely tooinstall hello konfigurációs kiszolgálón.

## <a name="configuration-server-minimum-requirements"></a>Konfigurációs kiszolgáló minimális követelményeknek
hello konfigurációs server szoftverre a következő VMware magas rendelkezésre állású virtuális gépen kell telepíteni. hello a következő táblázat felsorolja a hello minimális hardver, szoftverek és hálózati követelményei a konfigurációs kiszolgáló.
[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

> [!NOTE]
> Hello konfigurációs kiszolgáló által a HTTPS-alapú proxykiszolgálókat nem támogatottak.

## <a name="choose-your-protection-goals"></a>Védelmi célok megválasztása

1. A hello Azure-portálon, válassza a toohello **Recovery Services** tároló panelt, és válassza ki a tároló.
2. A hello tároló hello erőforrás menüben nyissa meg túl**bevezetés** > **Site Recovery** > **1. lépés: infrastruktúra előkészítése**  >  **Védelmi cél**.

    ![Célok megválasztása](./media/site-recovery-set-up-vmware-to-azure/choose-goals.png)
3. A **védelmi cél**, jelölje be **tooAzure**, és válassza a **Igen, amelyen a VMware vSphere Hipervizorra**. Ezután kattintson az **OK** gombra.

    ![Célok megválasztása](./media/site-recovery-set-up-vmware-to-azure/choose-goals2.png)

## <a name="set-up-hello-source-environment"></a>Hello forráskörnyezet beállítása
Hello forráskörnyezet beállítása magában foglalja a két fő tevékenység:

- Telepítse, és a konfigurációs kiszolgáló regisztrálása a Site Recovery szolgáltatással.
- A helyszíni virtuális gépek észlelése csatlakozzon a Site Recovery tooyour helyszíni VMware vCenter vagy vSphere EXSi gazdagépek.

### <a name="step-1-install-and-register-a-configuration-server"></a>1. lépés: Telepítse és regisztrálja a konfigurációs kiszolgálón

1. Kattintson a **1. lépés: infrastruktúra előkészítése** > **forrás**. A **forrás előkészítése**, ha nem rendelkezik a konfigurációs kiszolgáló, kattintson a **+ konfigurációs kiszolgáló** tooadd egyet.

    ![A forrás beállítása](./media/site-recovery-set-up-vmware-to-azure/set-source1.png)
2. A hello **kiszolgáló hozzáadása** panelen ellenőrizze, hogy **konfigurációs kiszolgáló** megjelenik **kiszolgálótípus**.
4. Töltse le a hello Site Recovery az egységes telepítő telepítőfájlját.
5. Hello tárolóbeli regisztrációs kulcs letöltése. Az egységes telepítő szüksége hello regisztrációs kulcsot. hello kulcs a generálásától öt napig esetén érvényes.

    ![A forrás beállítása](./media/site-recovery-set-up-vmware-to-azure/set-source2.png)
6. Hello gépen hello kiszolgálóként használ, futtassa **Azure Site Recovery az egységes telepítő** tooinstall hello konfigurációs kiszolgáló, a hello folyamatkiszolgáló és a hello fő célkiszolgáló.

#### <a name="run-azure-site-recovery-unified-setup"></a>Futtassa az Azure Site Recovery egyesített telepítő

> [!TIP]
> Konfigurációs kiszolgáló regisztrálása sikertelen lesz, ha hello a számítógép rendszerórája eltér a helyi idő több mint öt perc. A rendszer szinkronizálást, és egy [kiszolgálót](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service) hello telepítés megkezdése előtt.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

> [!NOTE]
> hello konfigurációs kiszolgálón a parancssorból is telepíthető. További információkért lásd: [telepítése hello konfigurációs kiszolgáló parancssori eszközökkel](http://aka.ms/installconfigsrv).

#### <a name="add-hello-vmware-account-for-automatic-discovery"></a>Automatikus felderítés hello VMware fiók hozzáadása

[!INCLUDE [site-recovery-add-vcenter-account](../../includes/site-recovery-add-vcenter-account.md)]

### <a name="step-2-add-a-vcenter"></a>2. lépés: A vCenter hozzáadása
tooallow Azure Site Recovery toodiscover futó virtuális gépek a helyszíni környezetben, kell tooconnect a VMware vCenter-kiszolgáló vagy vSphere ESXi-gazdagépek Site Recovery szolgáltatással.

Válassza ki **+ vCenter** toostart VMware vCenter-kiszolgáló vagy egy VMware vSphere ESXi-állomáson.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]


## <a name="common-issues"></a>Gyakori problémák
[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]


## <a name="next-steps"></a>Következő lépések
[A célkörnyezet beállítása](./site-recovery-prepare-target-vmware-to-azure.md) az Azure-ban.
