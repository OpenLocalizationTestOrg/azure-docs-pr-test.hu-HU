---
title: "aaaAzure szolgáltatásának állapota áttekintése |} Microsoft Docs"
description: "Személyre szabott információk az Azure-alkalmazásokban az Azure szolgáltatás jelenlegi és jövőbeli problémák és karbantartási által érintett hogyan."
services: Resource health
documentationcenter: 
author: rboucher
manager: 
editor: 
ms.assetid: 
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 07/07/2017
ms.author: robb
ms.openlocfilehash: 2b536ee2f19757d4f2baf5529866c3d159a4670c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-health"></a>Azure Service Health
Azure szolgáltatás állapota megfelelő időben és személyre szabott kapcsolatos információkat biztosít, ha az Azure-szolgáltatásokban problémák hatással van a szolgáltatások.  Emellett segítséget nyújt küszöbönálló tervezett karbantartási előkészítése.

## <a name="service-health-events"></a>Szolgáltatás állapotával kapcsolatos események
Szolgáltatás állapotát követi nyomon, hogy háromféle állapotával kapcsolatos események, amely hatással lehet az erőforrások:
1. **Problémák szolgáltatás** -problémák hello Azure-szolgáltatások előforduló most. 
2. **A tervezett karbantartások** -közeledő karbantartással, amelyek hatással lehetnek a szolgáltatások későbbi hello hello rendelkezésre állását.  
3. **Állapotfigyelő tanácsadók** -Azure-szolgáltatások figyelmet igénylő változásairól. Például ha az Azure-funkció elavult, vagy ha az túllépi a memóriahasználati kvóta.

    ![Szolgáltatás állapotával kapcsolatos események](./media/service-health-overview/azure-service-health-overview-7.png)

## <a name="get-started-with-service-health"></a>Ismerkedés a szolgáltatás állapota
toolaunch szolgáltatásának állapota irányítópulton, válassza hello szolgáltatás állapotát a portál irányítópultján csempét. Ha korábban eltávolított hello csempe, vagy egyéni irányítópult használata, keresse meg a "További szolgáltatások" szolgáltatás Állapotfigyelő szolgáltatás (az irányítópulton bal alsó).
![Ismerkedés a szolgáltatás állapota](./media/service-health-overview/azure-service-health-overview-1.png)

## <a name="see-current-issues-which-impact-your-services"></a>Aktuális problémák, amely hatással van a szolgáltatások lásd:
Hello **problémák szolgáltatás** a nézet jeleníti meg a folyamatban lévő problémákat, az Azure-szolgáltatásokat, hogy az erőforrások vannak hatással. Ha hello probléma ekkor kezdődött, és milyen szolgáltatásokat és régiók érintett tudja értelmezni. Is olvasható hello legutóbbi frissítés toounderstand mi Azure tooresolve hello probléma végez műveletet. 
![Szolgáltatási probléma kezelése](./media/service-health-overview/azure-service-health-overview-2.png)

Válassza ki a hello **célgyűjtemény** lapon toosee hello adott lista hello probléma által érintett előfordulhat, hogy saját erőforrásokat. Ezen erőforrások tooshare CSV listája, a csapat letölthető.
![Szolgáltatási probléma - hatás kezelése](./media/service-health-overview/azure-service-health-overview-4.png)

## <a name="get-links-and-downloadable-explanations"></a>Hivatkozások és a letölthető magyarázata 
Hivatkozás a probléma felügyeleti rendszer kaphat hello probléma toouse. PDF- és egyes esetekben a CSV-fájlok tooshare, akik nem rendelkeznek hozzáféréssel toohello Azure-portálon letölthető.   
![Szolgáltatási probléma - problémakezelés kezelése](./media/service-health-overview/azure-service-health-overview-3.png)

## <a name="get-support-from-microsoft"></a>Segítségre van szüksége a Microsofttól
Ha az erőforrás hibás állapotban marad, akkor is hello a probléma, forduljon a támogatási szolgálathoz.  Hello sarkában hello lap hello támogatási hivatkozások használata.  

## <a name="pin-a-personalized-health-map-tooyour-dashboard"></a>PIN-kód egy személyre szabott térkép tooyour irányítópult
Szolgáltatás állapota tooshow szűrésére, az üzleti szempontból kritikus fontosságú előfizetések, régiók és erőforrástípusok esetében. Hello szűrő és PIN-kód egy személyre szabott globális térkép tooyour portál irányítópult mentése. 
![Szűrő személyre szabott egészségügyi térkép](./media/service-health-overview/azure-service-health-overview-6a.png)
![PIN-kód egy személyre szabott egészségügyi térkép](./media/service-health-overview/azure-service-health-overview-6b.png)

## <a name="configure-service-health-alerts"></a>Szolgáltatás állapota riasztások konfigurálása
Az Azure szolgáltatás állapota integrálható a Azure figyelő tooalert, e-maileket, a szöveges üzenetek és a webhook értesítések, amikor az üzleti szempontból kulcsfontosságú erőforrások érintett. Állítson be egy tevékenység napló riasztást hello megfelelő szolgáltatásának állapota esemény. Olyan útvonal, amelyek riasztást toohello illetékes személyek férhessenek hozzá a szervezet művelet csoportok használatával. További információkért lásd: [riasztásainak konfigurálása szolgáltatásának állapota](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md)

# <a name="next-steps"></a>Következő lépések
Riasztások beállítása, így ügynökállapottal kapcsolatos hibákkal értesítést kap. További információkért lásd: [riasztások konfigurálása a szolgáltatás állapotát](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 