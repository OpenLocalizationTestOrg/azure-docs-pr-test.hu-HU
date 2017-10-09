---
title: "aaaWhat a szolgáltatás állapotával kapcsolatos értesítésekre |} Microsoft Docs"
description: "Szolgáltatás állapotával kapcsolatos értesítésekre engedélyezése tooview szolgáltatás állapotának üzenetek közzététele a Microsoft Azure által."
author: anirudhcavale
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
ms.author: ancav
ms.openlocfilehash: 6f2fe72154c3e80d85062655c49dd1799b718e3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-health-notifications"></a>Szolgáltatás állapotával kapcsolatos értesítésekre
## <a name="overview"></a>Áttekintés

Ez a cikk bemutatja, hogyan tooview szolgáltatás állapotával kapcsolatos értesítésekre használatával hello Azure-portálon.

Szolgáltatás állapotával kapcsolatos értesítésekre, tooview szolgáltatás állapotának üzenetek engedélyezése hello Azure-csapat, ami hatással lehet az előfizetéshez tartozó hello erőforrások tett közzé. Ezek az értesítések tevékenység alárendelt osztályát alkalmazásnapló-események, és hello tevékenység napló panel is található. Szolgáltatás állapotával kapcsolatos értesítésekre tájékoztató vagy hajtható végre attól függően, hogy hello osztály is lehet.

A szolgáltatás állapotával kapcsolatos értesítésekre öt osztályait van:  

- **Beavatkozás szükséges:** az idő tootime azt tapasztalhatja, valami szokatlan fordulhat elő, a fiókban. Előfordulhat, hogy szeretnénk az Ön tooremedy toowork ez. Kapni fog egy értesítést vagy tootake kell hello műveletek részletező vagy részletes információt a toocontact Azure mérnöki vagy támogatja.  
- **Támogatott helyreállítási:** olyan esemény történt, és a mérnökök megerősítette, hogy továbbra is problémát hatása. Mérnöki csapathoz toowork Önnek kell közvetlenül toobring a szolgáltatások toorestoration.  
- **Incidens:** esemény érintő szolgáltatás egy vagy több hello erőforrást az előfizetésében jelenleg érinti.  
- **Karbantartás:** ezt az egy értesítést a tájékoztat a tervezett karbantartások tevékenység, amely hatással lehet az előfizetéshez tartozó hello erőforrások közül legalább egyet.  
- **Információ:** az idő tootime is kapni, hogy egy kommunikáljon tooyou kapcsolatos lehetséges optimalizálást, amelyek hozzájárulhatnak a erőforrás hasznosíthatóságát javítják értesítések.  
- **Biztonsági:** sürgős biztonsági az Azure-on futó solution(s) vonatkozó információkat.

Minden szolgáltatás állapotának értesítés fogja hajtani részletek hello hatókör és a hatás tooyour erőforrástól függ. Részletek tartalmazza:

Tulajdonság neve | Leírás
-------- | -----------
csatornák | Hello a következő értékek egyike: "Rendszergazda", "Művelet"
correlationId | Az általában egy GUID Azonosítót a hello karakterlánc-formátum. Események, amelyek azonos uber művelet általában megosztása toohello tartozik azonos correlationId hello.
eventDataId | Egy esemény hello egyedi azonosítója
EventName | Hello cím hello esemény
szint | Hello esemény szintje. Hello a következő értékek egyikét: "Kritikus", "Error", "Figyelmeztetés", "Tájékoztató" és "Részletes"
resourceProviderName | Hello hello erőforrás-szolgáltató neve érintett erőforrás
a resourceType| hello típusú erőforrás hello az érintett erőforrás
a részállapot | Általában hello REST-hívást megfelelő hello HTTP-állapotkód, de más a részállapot, például a gyakori értékek leíró karakterláncok is használható: OK (HTTP-állapotkód: 200), létrehozott (HTTP-állapotkód: 201-es), elfogadott (HTTP-állapotkód: 202), nincs tartalom (HTTP Állapotkód: 204), hibás kérelem (HTTP-állapotkód: 400), nem található (HTTP-állapotkód: 404), ütközés (HTTP-állapotkód: 409), belső kiszolgálóhiba (HTTP-állapotkód: 500), a Service nem érhető el (HTTP-állapotkód: 503-as), átjáró időtúllépése (HTTP-állapotkód: 504).
eventTimestamp | Hello esemény lett létrehozva, amikor hello Azure szolgáltatás feldolgozása hello idejét jelző időbélyegző megfelelő hello esemény kérelmet.
submissionTimestamp |   Hello esemény váltak elérhetővé lekérdezése idejét jelző időbélyegző.
subscriptionId | Azure-előfizetés, amelyben ez az esemény naplózásának hello
status | Hello művelet hello állapotát leíró karakterlánc. Néhány gyakori értékek a következők: folyamatban van, sikeres, sikertelen, aktív, a lépések megoldva.
operationName | Hello művelet neve.
category | "ServiceHealth"
resourceId | Erőforrás-azonosítója, hello érintett erőforrás.
Properties.Title | Ez a kommunikáció hello honosított címét. Angol hello alapértelmezett nyelvét.
Properties.Communication | honosított hello részletei hello kommunikáció a HTML-kódot. Angol hello alapértelmezett beállítás.
Properties.incidentType | A lehetséges értékek: AssistedRecovery, ActionRequired, információk, incidens, karbantartási, biztonsági
Properties.trackingId | Az eseményhez kapcsolódó hello incidens azonosítja. Használja az toocorrelate hello események kapcsolódó tooan esemény.
Properties.impactedServices | Az escape-karakterrel megjelölt JSON-blob hello szolgáltatások és hello incidens által érintett területek leíró. Szolgáltatások listáját, amelyek mindegyike rendelkezik egy szolgáltatásnév és ImpactedRegions, amelyek mindegyikének a RegionName listáját.
Properties.defaultLanguageTitle | hello kommunikációs angol nyelven
Properties.defaultLanguageContent | hello kommunikációs html-kódot vagy egyszerű szöveg angol nyelven
Properties.Stage | AssistedRecovery, a ActionRequired, a információkat, az incidensek, a biztonsági a lehetséges értékek: aktív, amelyek megoldva. A karbantartás vannak: aktív, tervezett, esetbejegyzések, visszavonva, Rescheduled, megoldva, kész
Properties.communicationId | hello kommunikációs ezt az eseményt hozzá rendelve.


## <a name="viewing-your-service-health-notifications-in-hello-azure-portal"></a>A szolgáltatás állapotával kapcsolatos értesítésekre tekinti meg hello Azure-portálon
1.  A hello [portal](https://portal.azure.com), keresse meg a toohello **figyelő** szolgáltatás

    ![Figyelés](./media/monitoring-service-notifications/home-monitor.png)
2.  Kattintson a hello **figyelő** beállítás tooopen hello figyelő panel be. Ez a panel egyetlen, összevont nézetben jeleníti meg az összes figyelési beállítást és adatot. Először megnyitja a toohello **tevékenységnapló** szakasz.

3.  Most kattintson a **szolgáltatáshoz értesítést** szakasz

    ![Figyelés](./media/monitoring-service-notifications/service-health-summary.png)
4.  Kattintson bármelyik hello sor elemek tooview további részletek

5. Kattintson a hello **+ Hozzáadás tevékenység napló riasztási** művelet tooreceive értesítések tooensure értesítés jelenik meg az ilyen típusú jövőbeli szolgáltatási értesítésekhez. a riasztások konfigurálása a szolgáltatáshoz értesítést további toolearn [kattintson ide](monitoring-activity-log-alerts-on-service-notifications.md)

## <a name="next-steps"></a>További lépések:
Fogadási [riasztási értesítések, amikor az állapotfigyelő szolgáltatáshoz értesítést](monitoring-activity-log-alerts-on-service-notifications.md) van közzétéve.  
További információ [tevékenységriasztásokat napló](monitoring-activity-log-alerts.md)
