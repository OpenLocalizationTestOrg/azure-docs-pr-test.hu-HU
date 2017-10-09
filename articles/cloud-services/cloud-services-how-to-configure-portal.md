---
title: "a felhőszolgáltatás (portál) aaaHow tooconfigure |} Microsoft Docs"
description: "Ismerje meg, hogy miként tooconfigure felhőalapú szolgáltatásokat az Azure-ban. Ismerje meg, tooupdate hello felhőalapú szolgáltatás konfigurációja, és konfigurálja a távelérés toorole példányok. Ezekben a példákban hello Azure-portálon."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7308f3c0-825e-499d-bfa5-c60f86371921
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: adegeo
ms.openlocfilehash: 969a08558473e8c79153192942bfda587eb5ada5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-cloud-services"></a>TooConfigure Cloud Services
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-how-to-configure-portal.md)
> * [klasszikus Azure portál](cloud-services-how-to-configure.md)
>
>

Konfigurálhatja a felhőszolgáltatás hello leggyakrabban használt beállításait a hello Azure-portálon. Vagy, tetszés szerint tooupdate a konfigurációs fájlok közvetlen, töltse le a szolgáltatás konfigurációs fájl tooupdate, majd töltse fel a frissített hello fájl- és frissítési hello felhőszolgáltatás hello konfigurációs módosítások. Mindkét módszer esetén tooall szerepkörpéldányokat hello konfigurációfrissítések vannak leküldött.

Emellett kezelheti hello példányait a felhőszolgáltatás szerepköreit, vagy a távoli asztal be őket.

Azure is csak 99,95 % szolgáltatás rendelkezésre állásának biztosításához során hello konfigurációfrissítések ha legalább két szerepkör minden egyes szerepkörhöz. Amely lehetővé teszi, hogy egy virtuális gép tooprocess ügyfélkérelmek más hello frissítése közben. További információkért lásd: [szolgáltatói szerződések](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Egy felhőalapú szolgáltatás módosítása
Hello megnyitása után [Azure-portálon](https://portal.azure.com/), keresse meg a tooyour felhőalapú szolgáltatás. Itt azt sok szempontból kezelhető.

![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)

Hello **beállítások** vagy **összes beállítás** hivatkozásokra kattintva megnyílik hello **beállítások** panel, ahol módosíthatók a hello **tulajdonságok**, hello módosítása **Konfigurációs**, hello kezelése **tanúsítványok**, a telepítő **riasztási szabályok**, és kezelheti a hello **felhasználók** hozzáféréssel rendelkező a felhőalapú szolgáltatás toothis.

![Azure cloud service beállítások panel](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

### <a name="manage-guest-os-version"></a>Kezelheti a vendég operációs rendszer verziója

Alapértelmezés szerint Azure rendszeres időközönként frissíti a vendég operációs rendszer toohello legújabb támogatott képet hello a szolgáltatás konfigurációs (.cscfg), a megadott operációsrendszer-termékcsalád például a Windows Server 2016.

Ha egy adott operációs rendszer verziója tootarget van szüksége, beállíthatja azt hello **konfigurációs** panelen.

![Az operációs rendszer verzió beállítása](./media/cloud-services-how-to-configure-portal/cs-settings-config-guestosversion.png)


>[!IMPORTANT]
> Egy adott operációs rendszer verziója letiltja az operációs rendszer automatikus kiválasztása frissíti, és lehetővé teszi a felhasználó felelőssége javítását. Gondoskodnia kell arról, hogy a szerepkörpéldányok frissítések kap, vagy az alkalmazás toosecurity biztonsági rések tehetik közzé.

## <a name="monitoring"></a>Figyelés
Riasztások tooyour felhőalapú szolgáltatás is hozzáadhat. Kattintson a **beállítások** > **riasztás szabályok** > **riasztás hozzáadása**.

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Itt beállíthatja egy riasztást. A hello **metrika** legördülő listája, beállíthatja a következő típusú adatok hello egy riasztást.

* Lemez sebessége olvasott
* Lemez írási
* A hálózati
* Kimenő hálózati
* Processzorhasználat (%)

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Egy mérték csempe a figyelés konfigurálása
Helyett **beállítások** > **riasztási szabályok**, rákattinthat a hello metrika csempék hello az egyik **figyelés** hello szakasza **felhő szolgáltatás** panelen.

![A felhőalapú szolgáltatás figyelése](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Itt hello csempe használt hello diagram testreszabása, vagy vegye fel a riasztási szabályt.

## <a name="reboot-reimage-or-remote-desktop"></a>Rendszer újraindítása, a lemezkép-visszaállítási vagy a távoli asztal
Jelenleg nem lehet konfigurálni a távoli asztal használatával hello **Azure-portálon**. Azonban beállíthatja azt keresztül hello [a klasszikus Azure portálon](cloud-services-role-enable-remote-desktop.md), [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md), illetve [Visual Studio](../vs-azure-tools-remote-desktop-roles.md).

Első lépésként kattintson a hello cloud service-példány.

![Cloud Service-példány](./media/cloud-services-how-to-configure-portal/cs-instance.png)

A hello panel, amely megnyitja azt a távoli asztali kapcsolat kezdeményezéséhez, távolról újraindítás hello, vagy távolról lemezkép-visszaállítási (kezdő friss képének) hello példány.

![Cloud Service példány gombok](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)

## <a name="reconfigure-your-cscfg"></a>Konfigurálja újra a .cscfg
Szükség lehet tooreconfigure hello segítségével a felhőalapú szolgáltatás [szolgáltatás konfigurációs (szolgáltatáskonfigurációs séma)](cloud-services-model-and-package.md#cscfg) fájlt. Először meg kell toodownload a .cscfg fájl, módosítsa, majd töltse fel azt.

1. Kattintson a hello **beállítások** ikon vagy hello **összes beállítás** tooopen mentése hello hivatkozás **beállítások** panelen.

    ![Beállítások lap](./media/cloud-services-how-to-configure-portal/cloud-service.png)
2. Kattintson a hello **konfigurációs** elemet.

    ![Konfigurációs panel](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)
3. Kattintson a hello **letöltése** gombra.

    ![Letöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)
4. Hello szolgáltatás konfigurációs fájl frissítése után feltöltése és hello konfigurációs frissítések alkalmazásához:

    ![Feltöltés](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png)
5. Válassza ki a hello .cscfg fájlban, és kattintson a **OK**.

## <a name="next-steps"></a>Következő lépések
* Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).
* Konfigurálja a [egyéni tartománynév](cloud-services-custom-domain-name-portal.md).
* [A felhőalapú szolgáltatás kezelése](cloud-services-how-to-manage-portal.md).
* Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).
