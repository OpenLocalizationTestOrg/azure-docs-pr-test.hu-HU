---
title: "az Azure tevékenység aaaView naplózza a Naplóelemzési |} Microsoft Docs"
description: "Használhat Azure tevékenységi naplóit megoldás tooanalyze hello és keresési hello Azure tevékenységnapló összes Azure-előfizetések között."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: 171d0d604d03a5714a9599cc0b448fc5f6471f69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="view-azure-activity-logs"></a>Az Azure tevékenység naplók megtekintése

![Az Azure tevékenységi naplóit szimbólum](./media/log-analytics-activity/activity-log-analytics.png)

hello tevékenység Naplóelemzési megoldás segítségével elemezheti és hello keresési [Azure tevékenységnapló](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) minden Azure-előfizetés között. hello Azure tevékenységnapló, amely az előfizetések erőforrások hello műveleteket betekintést nyújt a naplót. hello tevékenységnapló korábban hívták *naplók* vagy *műveleti naplókat* mivel azt jelenti, hogy az előfizetések eseményeket.

Az hello tevékenységnapló is meghatározható hello *mi*, *ki*, és *amikor* az esetleges írási műveleteket (PUT, POST, Törlés) vonatkozó hello erőforrást az előfizetésében. Hello műveletek és egyéb kapcsolódó tulajdonságainak hello állapotának értelmezni is lehet. hello tevékenységnapló nem vonatkozik olvasási (GET) vagy a hello klasszikus telepítési modellt használó erőforrások.

Csatlakozás az Azure tevékenység naplók tooLog elemzés, akkor a következőket teheti:

- Előre definiált nézetekkel hello tevékenység naplóinak elemzése
- Elemzése és a Keresés és a tevékenység naplókat a több Azure-előfizetéssel
- Több mint 90 nap tevékenységi naplóit tartsa<sup>1</sup>
- Tevékenységi naplóit okozták az egyéb Azure platform-és alkalmazásadatok
- Működési állapot összesítve tevékenységek
- Az Azure-szolgáltatások minden egyes történik tevékenységek tendenciák megtekintése
- Jelentés az összes Azure-erőforrások engedélyezési módosításai
- Az erőforrások érintő vagy szolgáltatáskimaradás esetén egészségügyi problémák azonosításához
- Naplófájl-keresési toocorrelate felhasználói tevékenységet, automatikus méretezése műveletek, engedélyezési módosításokat, és szolgáltatásnaplók állapotfigyelő tooother vagy metrikákat a környezetéből

<sup>1</sup>alapértelmezés szerint a Naplóelemzési tartja az Azure tevékenységi naplóit 90 napra vonatkozó akkor is, ha a hello ingyenes szint. Vagy, ha egy munkaterület megőrzési beállítása legalább 90 nappal lehet. Ha a munkaterületet 90 napnál hosszabb megőrzési, hello tevékenységi naplóit hello megőrzési időtartam a munkaterület tartanak.

A Naplóelemzési tevékenységi naplóit ingyenesen gyűjt, és hello naplók ingyenesen 90 napig tárolja. Ha a naplófájlok 90 napnál hosszabb ideig tárolja, tájékozódnia a adatok megőrzési díjak hello adatok 90 napnál hosszabb.

Amikor az ingyenes tarifacsomag hello, tevékenységi naplóit nem alkalmazhatók tooyour napi adatok felhasználásához.

## <a name="connected-sources"></a>Összekapcsolt források

A legtöbb más Naplóelemzési megoldásoktól eltérően nem adatgyűjtés tevékenységi naplóit ügynököket. Hello megoldás által használt összes adatok származnak, közvetlenül az Azure-ból.

| Összekapcsolt forrás | Támogatott | Leírás |
| --- | --- | --- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Nem | hello megoldás a Windows-ügynökök nem gyűjt adatokat. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem | hello megoldás a Linux-ügynökök nem gyűjt adatokat. |
| [SCOM felügyeleti csoport](log-analytics-om-agents.md) | Nem | hello megoldás az ügynökök a csatlakoztatott SCOM felügyeleti csoport nem gyűjt adatokat. |
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | hello megoldás az Azure storage nem gyűjt adatokat. |

## <a name="prerequisites"></a>Előfeltételek

- tooaccess Azure tevékenység szereplő információkat, rendelkeznie kell Azure-előfizetéssel.

## <a name="configuration"></a>Konfiguráció

Hajtsa végre a következő lépéseket tooconfigure hello tevékenység Naplóelemzési megoldás a munkaterületek hello.

1. Hello tevékenység Naplóelemzési megoldást hello engedélyezése [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.AzureActivityOMS?tab=Overview) vagy ismertetett folyamatot hello segítségével [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).
2. Tevékenység naplók toogo tooyour Naplóelemzési munkaterület konfigurálása.
    1. A hello Azure-portálon, válassza ki a munkaterületet, és kattintson **Azure tevékenységnapló**.
    2. Az egyes előfizetésekhez kattintson a hello előfizetés nevét.  
        ![Előfizetés hozzáadása](./media/log-analytics-activity/add-subscription.png)
    3. A hello *SubscriptionName* panelen kattintson a **Connect**.  
        ![Csatlakozás az előfizetés](./media/log-analytics-activity/subscription-connect.png)

Hello megoldás használatával hello OMS-portálon való hozzáadásakor láthatja hello következő csempére. Jelentkezzen be az Azure portál tooconnect toohello az Azure-előfizetés tooyour munkaterülethez.  
![értékelés végrehajtása](./media/log-analytics-activity/tile-performing-assessment.png)

## <a name="using-hello-solution"></a>Hello megoldással

Hello tevékenység megoldás tooyour munkaterületet felvételekor hello **Azure tevékenységi naplóit** csempe kerül tooyour áttekintő irányítópulthoz. Ez a csempe hello Azure tevékenység rekordok száma számát jeleníti meg, a hello Azure-előfizetések, amelyek hello-megoldás a hozzáférést.

![Az Azure tevékenységi naplóit csempe](./media/log-analytics-activity/azure-activity-logs-tile.png)

### <a name="view-azure-activity-logs"></a>Naplózza az Azure-tevékenység megtekintése

Kattintson a hello **Azure tevékenységi naplóit** csempe tooopen hello **Azure tevékenységi naplóit** irányítópult. hello irányítópult hello paneleken hello a következő táblázat tartalmazza. Minden egyes panel be, hogy a panel a feltételek hello megadva hatókör és a kívánt időtartományt megfelelő too10 elemeket sorolja fel. A napló keresési, amely visszaadja az összes rekord kattintva futtathatja **láthatja az összes** hello panel vagy hello panel fejléc kattintva hello alján.

Tevékenység naplóadatokat csak akkor jelenik meg *után* konfigurálását a tevékenység naplók toogo toohello megoldás, így adatai nem tekinthetők meg addig.

| Panel | Leírás |
| --- | --- |
| Az Azure tevékenység naplóbejegyzések | Jeleníti meg a sávdiagram hello TOP Azure tevékenység naplóbejegyzés hello dátumtartományon belül a kiválasztott rekord összegek és felső 10 tevékenység hívóknak hello listáját tartalmazza. Kattintson sávdiagram toorun hello napló keresése <code>Type=AzureActivity</code>. Egy hívó elem toorun egy napló kattintson a Keresés gombra, hogy az elem minden tevékenység naplóbejegyzések ad vissza. |
| Tevékenységi naplóit állapot szerint | A perecdiagram Azure tevékenység napló állapot hello dátumtartományon belül a kiválasztott látható. Hello felső tíz állapot rekordok listáját is megjeleníti a listáját. Kattintson a diagram toorun hello napló keresése <code>Type=AzureActivity &#124; measure count() by ActivityStatus</code>. Egy állapot elem toorun a napló kattintson a Keresés gombra az összes tevékenység naplóbejegyzések állapot rekord visszaadása. |
| Erőforrás tevékenységi naplóit | Hello tevékenységi naplóit erőforrások összesített számát mutatja, és hello első tíz erőforrások rekord száma az egyes erőforrások sorolja fel. Teljes terület toorun hello napló keresése kattintson <code>Type=AzureActivity &#124; measure count() by Resource</code>, amely az összes Azure-erőforrások elérhető toohello megoldást mutat. Egy erőforrás toorun a napló kattintson a Keresés gombra az adott erőforrás minden tevékenység rekordot ad vissza. |
| Erőforrás-szolgáltató tevékenységi naplóit | Azt mutatja be hello száma tevékenység létrehozott erőforrás-szolgáltató naplózza, és hello tíz sorolja fel. Teljes terület toorun hello napló keresése kattintson <code>Type=AzureActivity &#124; measure count() by ResourceProvider</code>, amely jelzi, hogy az összes Azure-erőforrás-szolgáltatók. Kattintson egy erőforrás-szolgáltató toorun napló keresés hello szolgáltató minden tevékenység rekordot ad vissza. |

![Az Azure tevékenységi naplóit irányítópult](./media/log-analytics-activity/activity-log-dash.png)

## <a name="next-steps"></a>Következő lépések

- Hozzon létre egy [riasztás](log-analytics-alerts-creating.md) Ha egy adott tevékenységre történik.
- Használjon [naplófájl-keresési](log-analytics-log-searches.md) tooview részletes adatait a tevékenységi naplóit.
