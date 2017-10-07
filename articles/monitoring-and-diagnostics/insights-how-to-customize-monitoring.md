---
title: "a Microsoft Azure-ban mérőszámok aaaOverview |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize figyelési diagramokat az Azure-ban."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c36031eb-4df5-4cd5-9479-311d493a40d2
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2017
ms.author: robb
ms.openlocfilehash: 4196b2e9bda713e9a0abc349eed78685abcaf194
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-metrics-in-microsoft-azure"></a>A Microsoft Azure-ban mérőszámok áttekintése
Az összes Azure-szolgáltatások alapvető metrikákat, amelyek lehetővé teszik a toomonitor hello állapotát, teljesítmény, rendelkezésre állását és a szolgáltatások használatának nyomon követését. Megtekintheti a metrikák a hello Azure-portálon, és használhatja hello [REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx) vagy [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooaccess programozott módon hello metrikák teljes készletét.

Bizonyos szolgáltatások esetén szükség lehet a diagnosztika a rendelés toosee tooturn bármely metrikákat. Mások, például a virtuális gépek elérhetővé válik egy alapvető házirendcsoport metrikákat, de kell tooenable hello összes nagyon gyakori metrikákat. Lásd: [figyelés engedélyezésekor és diagnosztikai](insights-how-to-use-diagnostics.md) további toolearn.

## <a name="using-monitoring-charts"></a>Figyelési diagramok használatával
Akkor is diagram bármelyik hello metrikák bármely idő alatt, válassza ki őket.

1. A hello [Azure Portal](https://portal.azure.com/), kattintson a **Tallózás**, és majd erőforrás érdekli a figyelés.
2. Hello **figyelés** szakasz hello legfontosabb metrikákat, az egyes Azure-erőforrás. Például a webes alkalmazás rendelkezik **kérések és hibák követése**, ahol a virtuális gépek érvényesek **processzor** és **lemez olvasási és írási**: ![figyelés fókuszban](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)
3. Kattintson bármelyik olyan diagram a jelenít meg hello **metrika** panelen. Hello panelen továbbá toohello graph értéke a táblázat bemutatja, hogy összesítéseinek hello metrikák (például átlag, minimális és maximális hello időtartományát úgy döntött, hogy keresztül). Amely az alábbiakban hello hello erőforrás riasztási szabályok.
    ![Metrika panel](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)
4. toocustomize hello sorok jelennek meg, kattintson a hello **szerkesztése** hello diagram gombra, vagy hello **diagram szerkesztése** hello metrika panelen parancsot.
5. A lekérdezés szerkesztése panelen hello három műveleteket végezheti el:
   
   * Hello időtartomány módosítása
   * Hello megjelenését közötti váltás sáv és vonal
   * Válasszon másik metics ![lekérdezés szerkesztése](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)
6. Egy másik tartomány jelölnie úgy hello időtartományát (például **elmúlt egy órában**) elemre kattintva **mentése** hello hello panel alsó részén. Másik lehetőségként **egyéni**, amely lehetővé teszi toochoose minden idõszak, amíg keresztül hello elmúlt 2 hét. Például láthatja hello teljes két hét, vagy mappaverziót csak 1 óra. Írja be a hello szöveg mezőben tooenter egy másik óránként.
    ![Egyéni időtartomány](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)
7. Alább hello időtartományt, munkakör metrikák tooshow hello diagram tetszőleges számú válassza.
8. Kattintson a Mentés gombra a módosítások fogja menteni az adott erőforráshoz. Például ha két virtuális gép, és egy diagram módosításához, az nem befolyásolja hello más.

## <a name="creating-side-by-side-charts"></a>Egymás melletti diagramok létrehozása
Hello hatékony testreszabási hello portálon is hozzáadhat annyi diagramokat, ahányat csak szeretne.

1. A hello **...**  hello panelen kattintson a hello tetején menü **hozzáadhatja a csempéket**:  
    ![Hozzáadásra szolgáló menü](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. Majd, hogy válassza ki a diagram közül választhat hello **gyűjtemény** a képernyő jobb oldalán hello: ![gyűjteménye](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. Ha nem látja a kívánt hello metrika, bármikor hozzáadhat egy hello beállított metrikákat, és **szerkesztése** hello diagram tooshow hello metrika, amelyekre szüksége van.

## <a name="monitoring-usage-quotas"></a>Figyelési használati kvóták
A legtöbb metrikák is láthat trendek adott idő alatt, de egyes adatok, például használati kvótákat, az időpontban információk a küszöbértéket.

Használati kvóták hello panelen a erőforrásokat, amelyeket a kvóták is látható:

![Használat](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

Például a metrikák hello segítségével [REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx) vagy [.NET SDK](http://www.nuget.org/packages/Microsoft.Azure.Management.Monitor) tooaccess programozott módon hello használati kvóták teljes készletét.

## <a name="next-steps"></a>Következő lépések
* [Riasztási értesítéseket](insights-receive-alert-notifications.md) amikor metrika keverve használ a küszöbértéket.
* [Figyelés engedélyezésekor és diagnosztikai](insights-how-to-use-diagnostics.md) toocollect részletes nagyon gyakori metrikákat a szolgáltatásban.
* [Automatikus méretezése a példányok száma](insights-how-to-scale.md) toomake meg arról, hogy a szolgáltatás megfelelően üzemel és rugalmas.
* [Az alkalmazások teljesítményének figyelésére](../application-insights/app-insights-azure-web-apps.md) Ha azt szeretné, toounderstand pontosan hogyan a kód hajt végre hello felhőben.
* Használjon [Application Insights JavaScript alkalmazásokkal és weblapok](../application-insights/app-insights-web-track-usage.md) tooget ügyfél analytics egy weblap hello böngészők kapcsolatban.
* [Figyelése rendelkezésre állásának és bármilyen weblap válaszképességének](../application-insights/app-insights-monitor-web-app-availability.md) az Application insights szolgáltatással, így azt is megtudhatja, ha az oldala nem működik.

