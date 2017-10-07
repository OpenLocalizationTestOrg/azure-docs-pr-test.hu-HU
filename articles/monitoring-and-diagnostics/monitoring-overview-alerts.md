---
title: "a Microsoft Azure és az Azure-figyelő riasztások aaaOverview |} Microsoft Docs"
description: "Értesítések lehetővé teszik a toomonitor Azure-erőforrás metrikáit, eseményeket és naplókat, és értesíti, ha a megadott feltétel teljesül."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d080080666fedc9eefda6b35cdea3714785d2223
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-alerts-in-microsoft-azure"></a>Mik azok a riasztások a Microsoft Azure-ban?
Ez a cikk ismerteti a Microsoft Azure, Mik azok a riasztások különböző forrásokból hello vonatkozó riasztási, azok előnyeit, és hogyan tooget használja őket a következőkre hello. Kifejezetten tooAzure figyelő vonatkozik, de a riasztások, valamint tooother szolgáltatásokat nyújt a mutatók. Riasztások kínálnak, amelyek lehetővé teszik adatok tooconfigure feltételek és válnak értesítést kap, ha hello feltételek egyeznek meg a legújabb figyelési adatok hello Azure figyelésére szolgáló módszer.

## <a name="taxonomy-of-azure-alerts"></a>Az Azure riasztások besorolás
Az Azure által használt hello következő toodescribe riasztások és azok funkciói kapcsolatos kifejezések:
* **Riasztási** -definíciójának feltételek (egy vagy több szabály vagy -feltételeken), amelyek teljesülésekor lesz aktiválva.
* **Aktív** – hello állapot, amikor hello riasztást által meghatározott feltétel teljesülése esetén.
* **Megoldott** – hello állapot, amikor hello riasztást megadott feltételek korábban teljesítettnek után már nem teljesül.
* **Értesítési** -hello kijelentkezés váljon aktív riasztás alapján végrehajtott műveletet.
* **A művelet** -egy adott hívás küldött tooa fogadó egy értesítés (például e-mail címet vagy könyvelési tooa webhook URL-CÍMÉT). Értesítések általában elindítható több művelet.

## <a name="alerts-in-different-azure-services"></a>Riasztások a különböző Azure-szolgáltatások
Riasztások több Azure-szolgáltatások figyelésének keresztül érhetők el. Hogyan és mikor toouse ezek szolgáltatásokat, tájékoztatást [ebben a cikkben találhat](./monitoring-overview.md). Itt érhető információkat a riasztási típusokat hello Azure között:

| Szolgáltatás | Riasztás típusa | Támogatott szolgáltatások | Leírás |
|---|---|---|---|
| Azure Monitor | [Metrika riasztások](./insights-alerts-portal.md) | [Az Azure-figyelő támogatott metrikák](./monitoring-supported-metrics.md) | Értesítést kapnak, ha minden platform szintű metrika megfelel-e egy adott feltétel (például a virtuális gép CPU % érték nagyobb, mint 90 hello az elmúlt 5 perc). |
| Azure Monitor | [Tevékenység napló riasztások](./monitoring-activity-log-alerts.md) | Az összes elérhető az Azure Resource Manager erőforrástípus | Értesítés minden új esemény hello [Azure tevékenységnapló](./monitoring-overview-activity-logs.md) megegyezik a megadott feltételek (például ha a "Virtuális gép törlése" művelethez myProductionResourceGroup, vagy ha egy új szolgáltatás állapotesemény, amelynél "Active", hello állapota akkor jelenik meg). |
| Application Insights | [Metrika riasztások](../application-insights/app-insights-alerts.md) | Bármely alkalmazás tagolva toosend adatok tooApplication Insights | Értesítést kapnak, ha bármely alkalmazás szintű metrika megfelel-e egy adott feltétel (például kiszolgáló válaszideje érték nagyobb, mint 2). |
| Application Insights | [Webalkalmazás-teszt riasztások](../application-insights/app-insights-monitor-web-app-availability.md) | A webhely tagolva toosend adatok tooApplication Insights | Értesítést kaphat, ha a rendelkezésre állás vagy a webhely válaszképesség nem éri el elvárásainak. |
| Log Analytics | [Napló Analytics riasztások](../log-analytics/log-analytics-alerts.md) | Minden konfigurált toosend adatok Naplóelemzési | Értesítést kaphat, ha metrika és/vagy esemény adatok Naplóelemzési keresési lekérdezés megfelel bizonyos feltételeknek. |

## <a name="alerts-on-azure-monitor-data"></a>Azure-figyelő-riasztások
Nincsenek elérhető Azure figyelő--metrika riasztások és tevékenységnapló riasztások két típusú riasztások ki adatokat.

* **Metrika riasztások** -a riasztás elindítja a megadott metrika értékét hello ebbe a hozzárendelt küszöbértéket. hello riasztás értesítést állít elő, ha hello riasztás "aktív" (hello küszöbérték van túllépése esetén és hello figyelmeztetési feltétel teljesülése esetén), valamint megoldásakor,"" (hello küszöbérték van túllépése esetén újra és hello feltétel nem teljesül). A figyelő az Azure által támogatott elérhető mérőszámok egyre bővülő listáját lásd: [támogatott Azure-figyelő a metrikák listája](monitoring-supported-metrics.md).
* **Napló tevékenységriasztásokat** -egy adatfolyam-továbbítási váltja ki, ha az tevékenységnapló jön létre, hogy megfelel a keresési feltételeknek a hozzárendelt napló-értesítés. Ezek a riasztások rendelkezik csak egy állapota "Aktív", mivel hello riasztási motor egyszerűen alkalmazza hello szűrési feltételek tooany új esemény. Ezek a riasztások is lehet az értesítés a felhasznált toobecome, amikor egy új szolgáltatás állapota esemény következik be, vagy amikor egy felhasználó vagy alkalmazás egy műveletet végez az előfizetéshez, például "A virtuális gép törlése."

A diagnosztikai naplóadatokat Azure figyelő keresztül elérhető javasoljuk, hogy útválasztási hello adatok Naplóelemzési és a Log Analytics-riasztások segítségével. hello következő diagram foglalja össze az Azure figyelő és, fogalmilag, hogyan lehet adatok ki riasztást adatforrásokat.

![A riasztások ismertetése](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="how-do-i-receive-a-notification-on-an-azure-monitor-alert"></a>Hogyan jelenik meg egy értesítés az Azure a figyelő riasztást?
Hagyományosan az Azure-riasztások különböző szolgáltatásokhoz használt saját beépített értesítési módszerek. Következő lépésként Azure figyelő kínál nevű művelet csoportjainak újrafelhasználható értesítési csoportosítása. Művelet csoportok – e-mail címek tetszőleges számú, (SMS) telefonszámainak vagy webhook URL-címek – értesítés fogadókat-készletet adjon meg, és riasztást aktiválva van, hogy a hivatkozások hello művelet csoport, amikor az összes fogadó, hogy értesítést kapni. Ez lehetővé teszi tooreuse fogadók (például a visszafejtés listán) csoportosítása sok riasztás objektumokhoz. Jelenleg csak tevékenységnapló riasztások teszi a művelet csoportok használatát, de számos más Azure riasztástípusok dolgoznak művelet csoportok használatával is.

Művelet csoportok értesítés közzétételével tooa webhook URL-CÍMÉT, továbbá tooemail címek és az SMS számok támogatja. Ez lehetővé teszi a szervizelés, és automatizálási például használatával:
    - Azure Automation-runbook
    - Az Azure-függvény
    - Az Azure Logic Apps alkalmazást
    - egy külső szolgáltatás

Metrika riasztások még nem használható művelet csoportok. Egyéni metrika riasztást értesítések állítható be:
* E-mail küldhető értesítések toohello szolgáltatás rendszergazda, tooco-rendszergazdák vagy tooadditional e-mail-címek.
* Hívja a webhook, amely lehetővé teszi toolaunch további automatizálási műveleteket.

## <a name="next-steps"></a>Következő lépések
A riasztási szabályok és a konfigurálásuk adatainak beolvasása:

* További információ [metrikák](monitoring-overview-metrics.md)
* Konfigurálása [metrika riasztások Azure-portálon](insights-alerts-portal.md)
* Konfigurálása [metrika PowerShell riasztást küld.](insights-alerts-powershell.md)
* Konfigurálása [metrika riasztások parancssori felület (CLI)](insights-alerts-command-line-interface.md)
* Konfigurálása [metrika riasztások Azure figyelő REST API-n](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* További információ [műveletnapló](monitoring-overview-activity-logs.md)
* Konfigurálása [napló Tevékenységriasztásokat Azure-portálon](monitoring-activity-log-alerts.md)
* Konfigurálása [Tevékenységriasztásokat napló erőforrás-kezelő használatával](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Felülvizsgálati hello [műveletnapló riasztási webhook séma](monitoring-activity-log-alerts-webhook.md)
* További információ [szolgáltatáshoz értesítést](monitoring-service-notifications.md)
* További információ [művelet csoportok](monitoring-action-groups.md)
