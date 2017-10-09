---
title: a Visual Studio CodeLens Insights telemetria aaaApplication |} Microsoft Docs
description: "A CodeLens használatával gyorsan elérheti az Application Insights-kérést és a kivételtelemetriát a Visual Studióban."
services: application-insights
documentationcenter: .net
author: numberbycolors
manager: carmonm
ms.assetid: 93559e44-23cb-4b9d-8425-60f7f0d0a82c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: e812aa48f2a67eea860e7ecde341855763bb8a8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="application-insights-telemetry-in-visual-studio-codelens"></a>Application Insights Telemetria a Visual Studio CodeLensben
Webalkalmazás hello kódban módszerek is feliratozni a futásidejű kivételekre vonatkozó telemetriai adatokat, és kérje a válaszidők. Ha telepíteni [Azure Application Insights](app-insights-overview.md) az alkalmazás hello telemetriai jelenik meg a Visual Studio [CodeLens](https://msdn.microsoft.com/library/dn269218.aspx) -hello tetején, ahol használt függvény megjegyzések hello tooseeing hasznos információkat, például a helyek hello függvény hello hivatkozik, vagy hello legutóbbi szerkesztőjének azt.

![CodeLens](./media/app-insights-visual-studio-codelens/codelens-overview.png)

> [!NOTE]
> Az Application Insights az CodeLens, a Visual Studio 2015 Update 3 és újabb, vagy a hello legújabb verziójával [Analytics Fejlesztőeszközök bővítmény](https://visualstudiogallery.msdn.microsoft.com/82367b81-3f97-4de1-bbf1-eaf52ddc635a). CodeLens a hello Enterprise és a Visual Studio Professional kiadása érhető el.
> 
> 

## <a name="where-toofind-application-insights-data"></a>Ha toofind Application Insights adatainak
Keresse meg az Application Insights telemetria hello CodeLens mutatók hello nyilvános kérelem módszer a webes alkalmazás. A CodeLens-jelzők a C#- és a Visual Basic-kódokban a metódus és egyéb deklarációk fölött jelennek meg. Ha az Application Insights-adatok elérhetők egy metódushoz, akkor a kérések és kivételek jelzőit fogja látni, például „100 kérés, 1% meghiúsult” vagy „10 kivétel”. További információkért kattintson a CodeLens-jelzőre. 

> [!TIP]
> Az Application Insights kérése és kivétel mutatók lehet néhány másodpercre extra tooload után CodeLens utaló egyéb jeleket jelennek meg.
> 
> 

## <a name="exceptions-in-codelens"></a>Kivételek a CodeLensben
![TBD](./media/app-insights-visual-studio-codelens/codelens-exceptions.png)

hello kivétel CodeLens mutató történt hello az elmúlt 24 órában a legtöbb gyakran előforduló kivételek hello kérelem feldolgozása közben. ezen időszak alatt az alkalmazás hello metódus által kiszolgált 15 hello kivételek hello számát jeleníti meg.

toosee több részleteit, kattintson a hello kivételek CodeLens kijelző:

* hello legutóbbi 24 órában relatív toohello előzetes alóli kivételek száma 24 óra hello százalékos módosítása
* Válasszon **toocode Ugrás** toonavigate toohello forráskódja hello függvény hello Kivétel kiváltása
* Válasszon **keresési** tooquery Ez a kivétel történt az összes példányát hello az elmúlt 24 órában
* Válasszon **Trend** tooview trend képi megjelenítés előfordulásait a ennek a kivételnek hello az elmúlt 24 órában
* Válasszon **tekintse meg az alkalmazás összes kivétel** minden kivétel történt az elmúlt 24 órában hello tooquery
* Válasszon **Fedezze fel kivételhiba trendek** tooview trend képi megjelenítés hello elmúlt 24 órában történt összes kivételekhez. 

> [!TIP]
> Ha "0 kivétel" című CodeLens, de nem kell kivételek tudja, ellenőrizze a hello jobb Application Insights-erőforrás nincs bejelölve a CodeLens toomake. tooselect egy másik erőforrás, kattintson a jobb gombbal a projektre a Solution Explorer hello, és válassza a **Application Insights > Telemetriai forrás választása**. CodeLens csak jelennek hello 15 legtöbb gyakran előforduló kivételek hello az alkalmazást az elmúlt 24 órában, tehát ha kivétel gyakran hello a 16 legtöbb vagy kevesebb, látni fogja a "0 kivétel." ASP.NET nézetek kivételek szerepelhet a generált nézetek hello vezérlő metódusokhoz.
> 
> [!TIP]
> Ha a „? kivételek"CodeLens, lehet, hogy lejárt a Visual Studio vagy az Azure-fiók hitelesítő adatait az Azure-fiókjával tooassociate kell. Mindkét esetben kattintson a „? kivétel"válassza **fiók hozzáadása...**  tooenter a hitelesítő adatait.
> 
> 

## <a name="requests-in-codelens"></a>Kérések a CodeLensben
![TBD](./media/app-insights-visual-studio-codelens/codelens-requests.png)

hello CodeLens kijelző látható hello HTTP számú kérelem kéri, hogy megtörtént az elmúlt 24 órában, valamint ezeket a kérelmeket, melyeknél nem sikerült hello százaléka hello metódus által kiszolgált.

toosee további részletekért kattintson hello CodeLens mutató kéri:

* kérelmek, a sikertelen kérelmek és az átlagos válaszidő szám abszolút és százalékos megváltoztatása hello keresztül az elmúlt 24 órában képest toohello előzetes hello 24 órában
* hello metódus, a kérést, amely adott nem sikertelen hello az elmúlt 24 órában hello százalékában számított hello megbízhatóság
* Válasszon **keresési** a kéréseket, és a sikertelen kérelmek tooquery összes hello (sikertelen) kérelmek hello elmúlt 24 órában történt
* Válasszon **Trend** tooview kérelmek, a sikertelen kérelmek vagy átlagos válasz trend képi megjelenítés alkalommal hello elmúlt 24 órában.
* Hello bal felső sarkában hello CodeLens Részletek nézet toochange melyik erőforrást egy hello CodeLens adatok hello hello Application Insights-erőforrás nevét adja meg.

## <a name="next"></a>Következő lépések
|  |  |
| --- | --- |
| **[Az Application Insights használata a Visual Studióban](app-insights-visual-studio.md)**<br/>Telemetriát kereshet, adatokat tekinthet meg a CodeLensben és konfigurálhatja az Application Insights alkalmazást. Mindezt a Visual Studión belül. |![Kattintson a jobb gombbal a projekt hello, és válassza ki az Application Insights keresése](./media/app-insights-visual-studio-codelens/34.png) |
| **[További adatok hozzáadása](app-insights-asp-net-more.md)**<br/>Figyelheti a használatot, az elérhetőséget, a függőségeket és a kivételeket. Integrálhatja a nyomkövetéseket naplózási keretrendszerekből. Egyéni telemetriát írhat. |![Visual Studio](./media/app-insights-visual-studio-codelens/64.png) |
| **[Hello Application Insights portál használata](app-insights-dashboards.md)**<br/>Az irányítópultok, a hatékony diagnosztikai és elemző eszközök, riasztások, egy élő függőségi térkép az alkalmazásához, valamint a telemetria exportálása. |![Visual Studio](./media/app-insights-visual-studio-codelens/62.png) |

