---
title: Azure Application Insights adatainak Hockeyappra aaaExploring |} Microsoft Docs
description: "Elemezze a használati és teljesítményadatokat az Azure-alkalmazás az Application insights szolgáltatással."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: ed7cf99b48f5ec78d6b401bb954cfcd014b9d1f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Az Application Insightsban Hockeyappra adatok
[Hockeyappra](https://azure.microsoft.com/services/hockeyapp/) ajánlott hello platform élő az asztali és mobil alkalmazások figyeléséhez. A Hockeyappra küldhet egyéni és nyomkövetési telemetria toomonitor használati és diagnosztikai (a hozzáadása toogetting összeomlási adatait) támogatása. Ez az adatfolyam telemetriai adatot lekérdezhetők, hello hatékony használata [Analytics](app-insights-analytics.md) szolgáltatása [Azure Application Insights](app-insights-overview.md). Emellett képes [exportálja az egyéni hello és nyomkövetési telemetria](app-insights-export-telemetry.md). Ezek a szolgáltatások tooenable, állít be, amely továbbítja a Hockeyappra egyéni adatok tooApplication Insights hidat.

## <a name="hello-hockeyapp-bridge-app"></a>hello Hockeyappra híd alkalmazás
hello Hockeyappra híd alkalmazás, amely lehetővé teszi a Hockeyappra egyéni és hello Analytics keresztül az Application Insights – nyomkövetési telemetria tooaccess hello core szolgáltatás és a szolgáltatások folyamatos exportálása. Egyéni és a nyomkövetési események hello Hockeyappra híd App hello létrehozása után a Hockeyappra által gyűjtött fog érhető el ezeket a szolgáltatásokat. Nézzük meg, hogyan tooset híd alkalmazások közül.

A Hockeyappra, nyissa meg a fiók beállításait, [API jogkivonatok](https://rink.hockeyapp.net/manage/auth_tokens). Hozzon létre egy új jogkivonatot, vagy egy meglévő használja fel. hello minimális jogosultságokkal szükséges, amelyek "csak olvasható". Hello API másolatát jogkivonat érvénybe.

![A Hockeyappra API-token beszerzése](./media/app-insights-hockeyapp-bridge-app/01.png)

Nyissa meg hello Microsoft Azure-portálon és [Application Insights-erőforrás létrehozása](app-insights-create-new-resource.md). Alkalmazás típusa túl beállítása "Hockeyappra híd alkalmazás":

![Új Application Insights-erőforrás](./media/app-insights-hockeyapp-bridge-app/02.png)

Nem kell tooset nevét – ez a program automatikusan állítja hello Hockeyappra neve alapján.

hello Hockeyappra híd mezők jelennek meg. 

![Adja meg a híd mezők](./media/app-insights-hockeyapp-bridge-app/03.png)

Adja meg a korábban feljegyzett hello Hockeyappra jogkivonat. Ez a művelet feltölti hello "Hockeyappra alkalmazás" legördülő menüjében a Hockeyappra alkalmazásokkal. Válassza ki a kívánt toouse és hello mezők teljes hello további része egy hello. 

Nyissa meg a hello új erőforrást. 

Vegye figyelembe, hogy hello adatok eltart egy ideig toostart továbbítására.

![Várakozás az adatok Application Insights-erőforrás](./media/app-insights-hockeyapp-bridge-app/04.png)

Készen is van. Ettől kezdve a Hockeyappra tagolva alkalmazásban gyűjtött egyéni és nyomkövetési adatok mostantól is hello Analytics a rendelkezésre álló tooyou és az Application Insights a folyamatos exportálás funkcióit.

Most rövid időre tekintse át az összes ezen szolgáltatások most már hozzáférhető tooyou.

## <a name="analytics"></a>Elemzés
Elemzés alkalmi az adatok lekérdezése, hogy lehetővé teszi a toodiagnose hatékony eszköz és a telemetriai adatok elemzése és gyorsan Fedezze fel az alapvető okok és a minták.

![Elemzés](./media/app-insights-hockeyapp-bridge-app/05.png)

* [További információ az elemzés](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Folyamatos exportálás
A folyamatos exportálás tooexport lehetővé teszi az adatok az Azure Blob Storage tárolóba. Nagyon hasznos, ha szüksége tookeep az adatok hosszabb, mint jelenleg az Application Insights által kínált hello megőrzési időtartam azt. A blob Storage tárolóban hello adatok megőrzése, SQL-adatbázis, vagy az elsődleges adatok tekinthetünk feldolgozni azt.

[További információ a folyamatos exportálás](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Következő lépések
* [Elemzés tooyour adatok alkalmazásához](app-insights-analytics-tour.md)

