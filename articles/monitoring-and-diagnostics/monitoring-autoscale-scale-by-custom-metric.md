---
title: "egyéni mértéket az Azure által az automatikus skálázási lépései aaaGet |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale az erőforrás által egyéni mértéket az Azure-ban."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: d3e268ec322698d0d367361cd9c156b21e0fb6e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-auto-scale-by-custom-metric-in-azure"></a>Ismerkedés az automatikus skálázási által egyéni mértéket az Azure-ban
Ez a cikk ismerteti, hogyan tooscale az erőforrás által az Azure portálon egyéni metrika.

Az Azure a figyelő automatikus méretezési csak tooVirtual gép méretezési készletek (VMSS), felhőszolgáltatások, app service-csomagokról és app service Environment-környezetek vonatkozik. 

# <a name="lets-get-started"></a>Lehetővé teszi, hogy első lépései
Ez a cikk feltételezi, hogy rendelkezik-e egy webalkalmazást az application insights szolgáltatással konfigurált. Ha egy már nem rendelkezik, akkor [Application Insights beállítása az ASP.NET-webhely][1]

- Nyissa meg [Azure-portálon][2]
- Kattintson a bal oldali navigációs panelen hello Azure figyelő ikonra.
  ![Az Azure-figyelő indítása][3]
- Kattintson az automatikus skálázási beállítás tooview, amelyen az automatikus méretezési nem alkalmazható, az automatikus skálázás aktuális állapotával együtt minden hello erőforrások ![automatikus méretezési Azure figyelőben felderítése][4]
- Azure-figyelő a "automatikus skálázási" panel megnyitásához, és válassza ki a kívánt tooscale erőforrás
> Megjegyzés: hello lépéseket az app service-csomag, amely rendelkezik beállított app insights webes alkalmazáshoz kapcsolódó használja.
- Hello skálázási beállítást a panelen hello erőforrás láthatja, hogy hello aktuális példányszám 1. Kattintson az "Enable automatikus skálázás".
  ![Új webalkalmazás skálázási beállítást][5]
- Adjon meg egy nevet a hello méretezés beállítása és hello kattintson a "Szabály hozzáadása". Figyelje meg hello skálázási szabály beállítások egy környezet ablaktáblát hello jobb oldalt megnyíló. Alapértelmezés szerint állítja a hello beállítás tooscale példány száma 1, ha hello CPU percetage hello erőforrás meghaladja a 70 %. Hello metrika forrás módosítása hello felső túl "Application Insights", válassza ki hello app insights-erőforrás hello "Resource" legördülő, és jelölje ki hello egyéni metrika alapján kívánja tooscale.
  ![Egyéni metrika szerint][6]
- A fenti hasonló toohello lépés, amelyek a méretezhető és csökkentése hello méretezési száma 1 hello egyéni metrika esetén is a küszöbérték alá skálázási szabály hozzáadása.
  ![A skála cpu alapján][7]
- Ön példány korlátok hello beállítása. Például, ha azt szeretné, attól függően, hogy az egyéni mérték ingadozását hello 2 – 5 példányai között tooscale, állítsa be "minimális" túl "2", "maximális" túl "5" és "alapértelmezett" túl 2
> Megjegyzés: hello erőforrás metrikáit olvasása probléma merül fel, és hello aktuális kapacitás hello alapértelmezett kapacitásértéket alatt van, majd tooensure hello rendelkezésre állási hello erőforrás automatikus skálázási lehessen méretezni toohello alapértelmezett értéket. Ha hello aktuális kapacitás már nagyobb, mint az alapértelmezett kapacitásértéket, automatikus skálázás nem méretezni a.
- Kattintson a "Mentés"

Gratulálunk! Miután sikeresen megtörtént a skálázási beállítás tooauto méretezni a webalkalmazás egyéni metrika alapján.

> Megjegyzés: hello azonos lépésekre vonatkozó tooget VMSS vagy a felhőbeli szolgáltatás szerepkör elindult.

<!--Reference-->
[1]: https://docs.microsoft.com/en-us/azure/application-insights/app-insights-asp-net
[2]: https://portal.azure.com
[3]: ./media/monitoring-autoscale-scale-by-custom-metric/azure-monitor-launch.png
[4]: ./media/monitoring-autoscale-scale-by-custom-metric/discover-autoscale-azure-monitor.png
[5]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-scale-by-custom-metric/scale-by-custom-metric.png
[7]: ./media/monitoring-autoscale-scale-by-custom-metric/autoscale-setting-custom-metrics-ai.png
