---
title: a Visual Studio aaaAnalyzing trendek |} Microsoft Docs
description: "Elemezhet, megjeleníthet és felfedezhet trendeket a Visual Studióban található Application Insights telemetriáival."
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 3150c6fc-2691-44f6-a290-fc5cd68e692a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: 5c623ec040363f05e80ca927dc8855eb016adc99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyzing-trends-in-visual-studio"></a>Trendek elemzése a Visual Studióban
hello Application Insights trendek eszköz visualizes hogyan a webes alkalmazás fontos telemetriai események változnak az idők, így gyorsan azonosíthatja a problémákat és rendellenességeket. Létrehozhatja, toomore részletes diagnosztikai adatokat, trendeket segítséget az alkalmazás teljesítményének javítása, hello okok kivételek nyomon követi, és nyújt betekintést az az egyéni események.

![Példa a Trends ablakára](./media/app-insights-visual-studio-trends/app-insights-trends-hero-750.png)

## <a name="configure-your-web-app-for-application-insights"></a>A webapp konfigurálása az Application Insightshoz

Ha még nem tette meg, [konfigurálja a webappot az Application Insightshoz](app-insights-overview.md). Ez lehetővé teszi toosend telemetriai toohello Application Insights portálon találja meg. hello trendek eszköz hello telemetriai onnan olvassa be.

Az Application Insights Trends a Visual Studio 2015 Update 3 és újabb verziókban érhető el.

## <a name="open-application-insights-trends"></a>Az Application Insights Trends megnyitása
tooopen hello Application Insights trendek ablakban:

* Hello az Application Insights eszköztárgomb, válassza a **felfedezés Telemetriai trendek**, vagy
* Hello projekt helyi menüből **Application Insights > felfedezés Telemetriai trendek**, vagy
* Hello Visual Studio menüsorában kattintson **Nézet > más Windows > Application Insights trendek**.

Megjelenik egy kérdés tooselect erőforrás. Kattintson a **jelöljön ki egy erőforrást**, jelentkezzen be Azure-előfizetéssel, majd válassza ki az Application Insights-erőforrás hello listából, amelynek szeretné tooanalyze telemetriai trendeket.

## <a name="choose-a-trend-analysis"></a>Trendelemzés kiválasztása
![Trendelemzések általános típusainak menüje](./media/app-insights-visual-studio-trends/app-insights-trends-1-750.png)

Válassza ki az öt közös trend elemzések, minden egyes elemzésekor adatok az elmúlt 24 órában hello egyik első lépések:

* **Az a kiszolgáló által a teljesítményproblémák kivizsgálására** -kérések tooyour szolgáltatás válaszidejét szerint csoportosítva
* **A kiszolgálói kérelmek hibák elemzése** -kérések tooyour szolgáltatás HTTP-válaszkód szerint csoportosítva
* **Vizsgálja meg az alkalmazás hello kivételek** -szolgáltatás, kivételek kivétel típusa szerint csoportosítva
* **Ellenőrizze, hogy az alkalmazás függőségi hello teljesítményében** -szolgáltatások a szolgáltatás által meghívott válaszidők szerint csoportosítva
* **Egyéni események vizsgálata** – A szolgáltatáshoz beállított egyéni események, az esemény típusa szerint csoportosítva.

Előzetesen elkészített elemzések elérhetők később a hello **telemetriai adatok elemzését közös nézettípusok** hello trendek ablak bal felső sarkában hello gombra.

## <a name="visualize-trends-in-your-application"></a>Trendek megjelenítése az alkalmazásban
Az Application Insights Trends idősorozatos megjelenítést hoz létre az alkalmazás telemetriája alapján. Minden idősorozatos megjelenítés egy, a telemetria egyik tulajdonsága alapján csoportosított, adott időtartományba eső telemetriatípust jelenít meg. Például érdemes tooview kiszolgálói kérelmek, amelyből eredeti, hello utolsó 24 órán keresztül hello ország szerint csoportosítva. Ebben a példában minden egyes buborék hello képi megjelenítés a jelenti egy órán belül az egyes országában vagy régiójában hello kiszolgálói kérelmek számát.

Vezérlőkkel hello hello ablak tooadjust hello tetején milyen típusú telemetriai adatainak megtekintése. Először válasszon, amelyben kíváncsiak vagyunk hello telemetriai típusok:

* **Telemetria típusa** – Kiszolgálókérések, kivételek, függőségek vagy egyéni események
* **Időtartomány** – bárhol a hello utolsó 30 perc toohello utolsó 3 nap
* **Csoportosítási szempont** – Kivétel típusa, probléma azonosítója, ország/régió, és továbbiak.

Kattintson a **elemzése Telemetriai** toorun hello lekérdezés.

toonavigate buborékok a hello képi megjelenítés között:

* Kattintson a tooselect buborék, frissül hello szűrők hello ablakban, csak hello az eseményeket, amelyek egy adott időszakban történt összefoglalójához hello aljához
* Kattintson duplán a buborék toonavigate toohello keresési eszköz, és láthatja az összes hello egyéni telemetriai események, amelyek adott időszakban történt
* CTRL + kattintás a buborék toode jelölje ki azt a hello képi megjelenítés.

> [!TIP]
> hello a trendek és a keresési eszközök együttműködése toohelp hello problémák oka a szolgáltatás több ezer telemetriai események között a rögzítési ponthoz. Ha például az ügyfelei egyszer csak észreveszik, hogy az alkalmazás lassabban reagál, kezdjen a Trends használatával. Vizsgálja meg a kérelmet tooyour szolgáltatás keresztül hello elmúlt néhány órában válaszidő szerint csoportosítva. Ellenőrizze, hogy talál-e lassú kérelmekből álló, szokatlanul nagy fürtöt. Majd kattintson duplán buborék toogo toohello keresési eszköz, szűrt toothose kérelem események. Keresés, vizsgálatát hello tartalmát ezeket a kérelmeket, és keresse meg a toohello kód tooresolve hello probléma érint.
> 
> 

## <a name="filter"></a>Szűrés
Hello szűrő vezérlők hello ablak hello alján pontosabb trendek felderítése. a szűrő tooapply kattintson annak nevére. Előfordulhat, hogy egy adott dimenzió a telemetriai adatot kell elrejtése különböző szűrőket toodiscover trendek gyorsan válthat. Ha egy olyan dimenziót, például a kivételhiba típusát, a szűrő alkalmazása maradnak más dimenziók a szűrőket, kattintható, annak ellenére, hogy a szürke jelennek meg. tooun-szűrőt alkalmaz, kattintson rá. CTRL + kattintás tooselect több szűrő a hello ugyanazt a dimenziót.

![Trendszűrők](./media/app-insights-visual-studio-trends/TrendsFiltering-750.png)

Mi történik, ha azt szeretné, tooapply több szűrő? 

1. Hello első szűrőt alkalmazza. 
2. Kattintson a hello **kijelölt szűrőket alkalmazhat, és újra lekérdezés** hello nevű hello dimenzió az első szűrő gomb. Ez fogja kérdezni a telemetriai adatok csak az első hello szűrőnek megfelelő események újra. 
3. Alkalmazza a második szűrőt. 
4. Ismételje meg a meghatározott fájlcsoportokat a telemetriai adatokat a hello folyamat toofind trendeket. Megkeresheti például a „GET Home/Index” nevű *és* Németországból érkezett *és* 500-as válaszkóddal rendelkező kiszolgálókérelmeket. 

tooun-ezek a szűrők egyik alkalmazásához kattintson a hello **eltávolítani a kiválasztott szűrőket, és újra lekérdezés** hello dimenzió gombra.

![Több szűrő](./media/app-insights-visual-studio-trends/TrendsFiltering2-750.png)

## <a name="find-anomalies"></a>Rendellenességek keresése
hello trendek eszköz kiemelheti az események, amelyek a hello rendellenes összehasonlított tooother buborékok buborékok azonos time series. Hello nézettípus legördülő menüben válassza a **található az idő gyűjtő (kiemelési rendellenességeket)** vagy **idő gyűjtő (kiemelési rendellenességeket) szereplő százalékos**. A piros buborékok rendellenesek. Az száma/százalékos aránya meghaladja a 2.1-es alkalommal hello hello száma/százalékos aránya (a 48 óra, ha a megtekintett hello utolsó 24 óránként stb.) két időszakok túli hello történt szórása buborékok rendellenességeket is meg van adva.

![A színes pontok rendellenességeket jeleznek.](./media/app-insights-visual-studio-trends/TrendsAnomalies-750.png)

> [!TIP]
> A rendellenességek kiemelése különösen hasznos kis buborékok idősorozataiban található kiugró értékek kimutatásához, amelyek máskülönben ugyanakkora méretűnek tűnnének.  
> 
> 

## <a name="next"></a>Következő lépések
|  |  |
| --- | --- |
| **[Az Application Insights használata a Visual Studióban](app-insights-visual-studio.md)**<br/>Telemetriát kereshet, adatokat tekinthet meg a CodeLensben és konfigurálhatja az Application Insights alkalmazást. Mindezt a Visual Studión belül. |![Kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights keresése](./media/app-insights-visual-studio-trends/34.png) |
| **[További adatok hozzáadása](app-insights-asp-net-more.md)**<br/>Figyelheti a használatot, az elérhetőséget, a függőségeket és a kivételeket. Integrálhatja a nyomkövetéseket naplózási keretrendszerekből. Egyéni telemetriát írhat. |![Visual Studio](./media/app-insights-visual-studio-trends/64.png) |
| **[Hello Application Insights portál használata](app-insights-dashboards.md)**<br/>Az irányítópultok, a hatékony diagnosztikai és elemző eszközök, riasztások, egy élő függőségi térkép az alkalmazásához, valamint a telemetria exportálása. |![Visual Studio](./media/app-insights-visual-studio-trends/62.png) |

