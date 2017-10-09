---
title: "napló tevékenységriasztásokat aaaCreate |} Microsoft Docs"
description: "Értesítést SMS, webhook és e-mailek bizonyos események megtörténtekor hello műveletnaplóban."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: ba0716cc12a0b3a0024ee5562a025f3f153f8982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts"></a>Napló riasztások tevékenység létrehozása

## <a name="overview"></a>Áttekintés
Tevékenység napló riasztások az éppen riasztásokat, amelyek aktiválása, ha egy új tevékenység naplózási esemény következik be, amely megfelel a hello riasztásban feltüntetett hello feltételeknek. Azure-erőforrások, így azok hozhat létre Azure Resource Manager-sablonnal. Akkor is is létrehozott, frissítése, vagy törölve a hello Azure-portálon. Ez a cikk hello fogalmakat tevékenységriasztásokat napló be. Ezután bemutatja, hogyan toouse hello Azure portál tooset naplózási eseményeket a riasztásokat.

Általában létrehozhat napló tevékenységriasztásokat tooreceive értesítések során:

* Adott változások történnek az Ön Azure-előfizetéssel, gyakran hatókörön belüli tooparticular erőforráscsoportok vagy erőforrások erőforrástól függ. Például érdemes toobe értesíti, ha bármely myProductionResourceGroup a virtuális gép törlődik. Vagy, érdemes toobe értesítést kapnak, ha új szerepköröket rendelt tooa felhasználó az előfizetéshez.
* Egy szolgáltatás állapotát az esemény akkor következik be. Szolgáltatás állapotával kapcsolatos események értesítési események és karbantartási események, amelyek érvényesek az előfizetésében tooresources tartalmazza.

Mindkét esetben egy figyelmeztetés a napló csak az a mely hello riasztást hoz létre hello előfizetés eseményeket figyeli.

A legfelső szintű tulajdonság a hello JSON-objektumból az tevékenység naplóesemény alapján tevékenység napló riasztásokat lehet beállítani. Azonban a hello portal hello leggyakoribb lehetőségeket mutatja be:

- **Kategória**: felügyeleti, a szolgáltatás állapotát, automatikus skálázás és javaslat. További információkért lásd: [hello Azure tevékenységnapló áttekintése](./monitoring-overview-activity-logs.md#categories-in-the-activity-log). További információ a szolgáltatás állapotának események, toolearn lásd: [szolgáltatáshoz értesítést tevékenység napló riasztásokat fogadhat](./monitoring-activity-log-alerts-on-service-notifications.md).
- **Erőforráscsoport**
- **Erőforrás**
- **Erőforrástípus**
- **A művelet neve**: hello Resource Manager szerepköralapú hozzáférés-vezérlés művelet neve.
- **Szint**: hello súlyossági szint (részletes, tájékoztatás, figyelmeztetés, hiba vagy kritikus) hello esemény.
- **Állapot**: hello állapotának hello esemény – általában elindult, sikertelen vagy sikeres volt.
- **Az esemény által kezdeményezett**: néven is ismert hello "hívó." hello e-mail címet, vagy Azure Active Directory hello műveletet végre hello felhasználó azonosítója.

>[!NOTE]
>Meg kell adnia legalább két megelőző feltételeknek a riasztás az egy folyamatban lévő hello hello kategóriát. Nem hozható létre egy riasztást, amely minden alkalommal, amikor egy esemény jön létre hello tevékenységi naplóit aktiválja.
>
>

Egy tevékenység napló riasztás aktiválásakor használja-e művelet toogenerate műveletek vagy értesítések csoportban. Egy olyan újrafelhasználható értesítési fogadók, például az e-mail címeket, a webhook URL-címek vagy SMS telefonszámokat. hello fogadók több riasztások toocentralize lehet hivatkozni, és csoportosítsa az értesítési csatornák. Ha a napló figyelmeztetés, két választási lehetősége van. A következőket teheti:

* A napló figyelmeztetés művelet meglévő csoport használata 
* Hozzon létre egy új művelet. 

További információk a művelet csoportok toolearn lásd: [létrehozása és az Azure-portálon hello művelet csoportok kezelése](monitoring-action-groups.md).

További információ a szolgáltatás állapotával kapcsolatos értesítésekre toolearn lásd [tevékenység napló értesítést a szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-activity-log-alerts-on-service-notifications.md).

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-hello-azure-portal"></a>Riasztás létrehozása a művelet új csoportot a tevékenység napló esemény hello Azure-portál használatával
1. A hello [portal](https://portal.azure.com), jelölje be **figyelő**.

    ![hello "Figyelés" szolgáltatás](./media/monitoring-activity-log-alerts/home-monitor.png)
2. A hello **tevékenységnapló** szakaszban jelölje be **riasztások**.

    ![hello "Értesítések" lapon](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. Válassza ki **Hozzáadás figyelmeztetés a napló**, és töltse ki hello mezőket.

4. Adjon meg egy nevet a hello **tevékenység napló riasztás neve** mezőbe, majd válassza ki a **leírás**.

    ![hello "Figyelmeztetés a napló hozzáadása" parancs](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. Hello **előfizetés** a jelenlegi előfizetés autofills mezőben. Ez az előfizetés egy hello mely hello művelet csoport menti. hello riasztási erőforrás telepített toothis előfizetés és a figyelők tevékenység naplóeseményeket belőle.

    ![hello "Figyelmeztetés a napló hozzáadása" párbeszédpanel](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. Jelölje be hello **erőforráscsoport** mely hello a riasztás létrehozása történt meg. Ez az hello erőforráscsoport, amelyet az hello riasztás. Ehelyett hello erőforráscsoport, ahol hello riasztási erőforrás is található.

7. Ha kijelöl egy **eseménykategória** toomodify hello további szűrőket látható. A felügyeleti események hello szűrők a következők **erőforráscsoport**, **erőforrás**, **erőforrástípus**, **műveletnév**, **Szint**, **állapot**, és **esemény által kezdeményezett**. Ezeket az értékeket a riasztás célszerű figyelemmel kísérni események azonosítása.

    >[!NOTE]
    >Meg kell adnia legalább egy hello megelőző feltételeknek a riasztás. Nem hozható létre egy riasztást, amely minden alkalommal, amikor egy esemény jön létre hello tevékenységi naplóit aktiválja.
    >
    >

8. Adjon meg egy nevet a hello **művelet csoportnév** mezőbe, majd írjon be egy nevet a hello **rövid név** mezőbe. hello rövid nevét helyett egy teljes művelet felügyeleticsoport-nevet használja, ha ez a csoport értesítések küldése.

9.  Adja meg azon műveletek listáját, adja meg a hello művelet:

    a. **Név**: Adja meg a hello művelet alias, név vagy azonosító.

    b. **Művelet típusa**: válassza ki az SMS, e-mailek vagy webhook.

    c. **Részletek**: hello művelet típusa alapján, adjon meg egy telefonszámot, az e-mail címet, illetve a webhook URI.

10. Válassza ki **OK** toocreate hello riasztás.

hello riasztás néhány percet vesz igénybe toofully propagálására, és majd aktívvá válik. Amikor új események hello riasztás feltételeknek elindítja.

További információkért lásd: [megértése hello webhook séma napló tevékenységriasztásokat használt](monitoring-activity-log-alerts-webhook.md).

>[!NOTE]
>Ezeket a lépéseket definiált hello művelet csoport újrafelhasználható, az összes jövőbeni riasztási definíciók egy meglévő művelet csoportként.
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-hello-azure-portal"></a>Riasztás létrehozása a meglévő művelet csoportok az tevékenység naplóesemény hello Azure-portál használatával
1. Végezze el az 1-7 hello előző szakasz toocreate a napló figyelmeztetés.

2. A **keresztül értesíti**, jelölje be hello **meglévő** művelet csoport gombra. Válasszon egy meglévő művelet csoportot hello listából.

3. Válassza ki **OK** toocreate hello riasztás.

hello riasztás néhány percet vesz igénybe toofully propagálására, és majd aktívvá válik. Amikor új események hello riasztás feltételeknek elindítja.

## <a name="manage-your-alerts"></a>A riasztások kezelése

Riasztás létrehozása után már látható hello riasztások szakaszban hello figyelő panelről. Jelölje ki azt szeretné, hogy toomanage hello riasztást:

* Szerkesztheti.
* Törölje a parancsikont.
* Tiltsa le, vagy engedélyezheti, ha folytatni hello riasztás értesítések fogadásának vagy tootemporarily le szeretné.

## <a name="next-steps"></a>Következő lépések
- Első egy [riasztások áttekintése](monitoring-overview-alerts.md).
- További tudnivalók [értesítési sebessége korlátozza az](monitoring-alerts-rate-limiting.md).
- Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md).
- További információ [művelet csoportok](monitoring-action-groups.md).  
- További tudnivalók [szolgáltatás állapotával kapcsolatos értesítésekre](monitoring-service-notifications.md).
- Hozzon létre egy [tevékenységet naplózni riasztási toomonitor az előfizetés minden automatikus skálázási motor műveletek](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).
- Hozzon létre egy [tevékenységet naplózni riasztási toomonitor az előfizetés összes sikertelen automatikus skálázás méretezési-a/kibővített művelete](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).
