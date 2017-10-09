---
title: "napló tevékenységriasztásokat aaaReceive szolgáltatás értesítések |} Microsoft Docs"
description: "Értesítés SMS, e-mailben vagy webhook Azure-szolgáltatás esetén."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: dd35e8f39d2a522efdae4dfed20779c992c1dd27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a>A szolgáltatás értesítések napló riasztások tevékenység létrehozása
## <a name="overview"></a>Áttekintés
Ez a cikk bemutatja, hogyan tooset tevékenységnapló be riasztásokat a szolgáltatás állapotával kapcsolatos értesítésekre hello Azure-portál használatával.  

Akkor is figyelmezteti, ha Azure szolgáltatás állapota értesítések tooyour Azure-előfizetés küld. Hello riasztás alapján konfigurálhatja:

- hello-osztály szolgáltatás állapota (incidens, karbantartási, stb.).
- hello Services-szolgáltatás(ok) hatással.
- hello régió(k) hatással.
- hello állapota hello értesítés (aktív vagy megoldott).
- hello szintje hello értesítések (információs, figyelmeztetési, hiba).

Is konfigurálhatja, akik hello riasztást küldjön el:

- Válasszon egy meglévő művelet csoportot.
- Hozzon létre egy új művelet (riasztást használható).

További információk a művelet csoportok toolearn lásd: [létrehozása és művelet csoportok kezelése](monitoring-action-groups.md).

Hogyan tooconfigure szolgáltatás állapotának értesítési riasztást küld, Azure Resource Manager-sablonok használatával kapcsolatos tudnivalókért lásd: [Resource Manager-sablonok](monitoring-create-activity-log-alerts-with-resource-manager-template.md).

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-hello-azure-portal"></a>Riasztás létrehozása egy új művelet csoport Állapotfigyelő szolgáltatás értesítést a hello Azure-portál használatával
1. A hello [portal](https://portal.azure.com), jelölje be **figyelő**.

    ![hello "Figyelés" szolgáltatás](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. A hello **tevékenységnapló** szakaszban jelölje be **riasztások**.

    ![hello "Értesítések" lapon](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. Válassza ki **Hozzáadás figyelmeztetés a napló**, és töltse ki hello mezőket.

    ![hello "Figyelmeztetés a napló hozzáadása" parancs](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. Adjon meg egy nevet a hello **tevékenység napló riasztás neve** mezőbe, majd adjon meg egy **leírás**.

    ![hello "Figyelmeztetés a napló hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. Hello **előfizetés** a jelenlegi előfizetés autofills mezőben. Ez az előfizetés használt toosave hello napló figyelmeztetés. hello riasztási erőforrás hello műveletnaplóban az előfizetés és a figyelők események telepített toothis.

6. Jelölje be hello **erőforráscsoport** mely hello a riasztás létrehozása történt meg. Ez nem hello erőforráscsoport, amelyet az hello riasztás. Ehelyett hello erőforráscsoport, ahol hello riasztási erőforrás is található.

7. A hello **eseménykategória** mezőben válassza **szolgáltatásának állapota**. Bejelölheti a hello **szolgáltatás**, **régió**, **típus**, **állapot**, és **szint** szolgáltatás állapotával kapcsolatos értesítésekre, amelyet az tooreceive.

8. A **keresztül riasztási**, jelölje be hello **új** művelet csoport gombra. Adjon meg egy nevet a hello **művelet csoportnév** mezőbe, majd írjon be egy nevet a hello **rövid név** mezőbe. a riasztás aktiválódásakor közreműködésével küldött értesítő hello hivatkozik hello rövid nevét.

9. Adja meg a fogadók listáját hello fogadó megadásával:

    a. **Név**: Adja meg a hello fogadó alias, név vagy azonosító.

    b. **Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.

    c. **Részletek**: hello művelet CustomProperty alapján, adjon meg egy telefonszám, e-mail címét vagy webhook URI.

10. Válassza ki **OK** toocreate hello riasztás.

Néhány percen belül hello aktív és riasztás létrehozása során megadott hello feltételek alapján tootrigger kezdődik.

A napló tevékenységriasztásokat hello webhook sémája információkért lásd: [Webhookok Azure tevékenység naplózása riasztások](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Ezeket a lépéseket definiált hello művelet csoport újrafelhasználható, az összes jövőbeni riasztási definíciók egy meglévő művelet csoportként.
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-hello-azure-portal"></a>Hozzon létre egy riasztást az állapotfigyelő szolgáltatás értesítést meglévő művelet csoportok hello Azure-portál használatával

1. Végezze el az 1-7 hello előző szakasz toocreate a szolgáltatás állapotának értesítési. 

2. A **keresztül riasztási**, jelölje be hello **meglévő** művelet csoport gombra. Válassza ki hello megfelelő lépéseket.

3. Válassza ki **OK** toocreate hello riasztás.

Néhány percen belül hello aktív és riasztás létrehozása során megadott hello feltételek alapján tootrigger kezdődik.

## <a name="manage-your-alerts"></a>A riasztások kezelése

Riasztás létrehozása után is látható a hello **riasztások** hello szakasza **figyelő** panelen. Jelölje ki azt szeretné, hogy toomanage hello riasztást:

* Szerkesztheti.
* Törölje a parancsikont.
* Tiltsa le, vagy engedélyezheti, ha folytatni hello riasztás értesítések fogadásának vagy tootemporarily le szeretné.

## <a name="next-steps"></a>Következő lépések
- További tudnivalók [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).
- További tudnivalók [értesítési sebessége korlátozza az](monitoring-alerts-rate-limiting.md).
- Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).
- Lekérése egy [tevékenység napló riasztások áttekintése](monitoring-overview-alerts.md), és megtudhatja, hogyan tooreceive riasztásokat. 
- További információ [művelet csoportok](monitoring-action-groups.md).
