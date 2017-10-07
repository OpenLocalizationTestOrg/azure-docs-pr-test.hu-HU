---
title: "aaaMicrosoft Dynamics CRM-hez és az Azure Application Insights |} Microsoft Docs"
description: "Telemetriai adatok beszerzése a Microsoft Dynamics CRM Online Application Insights segítségével. Forgatókönyv: a telepítés az első adatok, a képi megjelenítés és exportálása."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a39398060d6553fb18a26c101f085b7d87443636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a>Forgatókönyv: Telemetriai engedélyezése a Microsoft Dynamics CRM Online Application Insights segítségével
Ez a cikk bemutatja, hogyan tooget telemetriai adatokat [Microsoft Dynamics CRM Online](https://www.dynamics.com/) használatával [Azure Application Insights](https://azure.microsoft.com/services/application-insights/). Végigvezetjük a teljes folyamat hello Application Insights parancsfájl tooyour alkalmazás hozzáadása, adatokat és az adatok vizuális rögzítése.

> [!NOTE]
> [Keresse meg a hello megoldást](https://dynamicsandappinsights.codeplex.com/).
> 
> 

## <a name="add-application-insights-toonew-or-existing-crm-online-instance"></a>Az Application Insights toonew vagy meglévő CRM Online példány hozzáadása
toomonitor az alkalmazás Application Insights SDK tooyour alkalmazás hozzáadása. hello SDK küld telemetriai toohello [Application Insights portál](https://portal.azure.com), amelyen használja a hatékony elemzés és diagnosztikai eszközöket, vagy hello adatok toostorage exportálása.

### <a name="create-an-application-insights-resource-in-azure"></a>Az Application Insights-erőforrás létrehozása az Azure-ban
1. Első [egy fiókot a Microsoft Azure-ban](http://azure.com/pricing). 
2. Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com) , és adja hozzá egy új Application Insights-erőforrást. Ez az, ha az adatok feldolgozása és jelenik meg.
   
    ![Kattintson a +, fejlesztői szolgáltatások, az Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    Válassza ki az ASP.NET hello alkalmazás típusként.
3. Hello első lépések lap megnyitásához, és nyissa meg a "a figyelő és diagnosztizálhatja az ügyféloldali".
   
    ![A weblap beszúrásához kódrészletet](./media/app-insights-sample-mscrm/03.png)

**Tartsa nyitva, hello kódlap** közben a következő lépés egy másik böngészőablakban hello. Hello kód hamarosan lesz szüksége. 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a>A Microsoft Dynamics CRM JavaScript webes erőforrás létrehozása
1. Nyissa meg az CRM Online-példány és a bejelentkezési rendszergazdai jogosultságokkal.
2. Nyissa meg a Microsoft Dynamics CRM beállításait, testreszabás is szerepelt, testreszabás hello rendszer
   
    ![A Microsoft Dynamics CRM-beállítások](./media/app-insights-sample-mscrm/04.png)
   
    ![Beállítások > Testreszabás](./media/app-insights-sample-mscrm/05.png)

    ![Hello beállítást testreszabása](./media/app-insights-sample-mscrm/06.png)

1. Hozzon létre egy JavaScript-erőforrást.
   
    ![Az új webes erőforrás párbeszédpanel](./media/app-insights-sample-mscrm/07.png)
   
    Adjon neki egy névvel, jelölje be **parancsfájl (JScript)** és a nyitott hello szövegszerkesztőben.
   
    ![Nyissa meg hello szövegszerkesztőben](./media/app-insights-sample-mscrm/08.png)
2. Az Application Insights hello kód másolása. Másolás közben győződjön meg arról, hogy tooignore parancsprogramcímkéket. Tekintse meg az alábbi képernyőfelvételen:
   
    ![A rendszerállapot-kulcs beállítása](./media/app-insights-sample-mscrm/09.png)
   
    hello kód hello instrumentation kulcsot, amely azonosítja az Application insights-erőforrást tartalmaz.
3. Mentse, és tegye közzé.
   
    ![Mentse és közzététele](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a>Eszköz űrlapok
1. A Microsoft CRM Online hello fiók képernyő megnyitása
   
    ![Fiók képernyő](./media/app-insights-sample-mscrm/11.png)
2. Nyissa meg a hello űrlap tulajdonságai
   
    ![Űrlap tulajdonságai](./media/app-insights-sample-mscrm/12.png)
3. Hello létrehozott JavaScript webes erőforrás hozzáadása
   
    ![Hozzáadásra szolgáló menü](./media/app-insights-sample-mscrm/13.png)
   
    ![Hello webes erőforrás hozzáadása](./media/app-insights-sample-mscrm/14.png)
4. Mentse, és tegye közzé az űrlapok testreszabásai.

## <a name="metrics-captured"></a>Rögzített metrikák
Most már készen hello űrlap a telemetria-rögzítést. Azokat alkalmazni, amikor adatokat küld tooyour Application Insights-erőforrást.

Az alábbiakban láthatja hello adatok minták.

#### <a name="application-health"></a>Alkalmazás állapotának
![Példa lapbetöltési idő](./media/app-insights-sample-mscrm/15.png)

![Példa lap nézetek diagram](./media/app-insights-sample-mscrm/16.png)

Böngésző kivételek:

![Böngésző kivételek diagram](./media/app-insights-sample-mscrm/17.png)

Kattintson a hello diagram tooget további részletek:

![Kivételek listájáról](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a>Használat
![Felhasználók, a munkamenetek és az oldalmegtekintéseket](./media/app-insights-sample-mscrm/19.png)

![Sesion diagramok](./media/app-insights-sample-mscrm/20.png)

![Böngésző-verziók](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a>Böngészők
![Lapbetöltési idő bontása](./media/app-insights-sample-mscrm/22.png)

![A böngésző verziója-munkamenetek száma](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a>Földrajzi hely
![Ország munkamenetek száma](./media/app-insights-sample-mscrm/24.png)

![A munkamenetek és országonként felhasználók](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a>Belső lapkérelem megtekintése
![Lap összegzésének megtekintése](./media/app-insights-sample-mscrm/26.png)

![Keresés lap eseményeinek megtekintése](./media/app-insights-sample-mscrm/27.png)

![Hasonló Lapmegtekintések](./media/app-insights-sample-mscrm/28.png)

![Lapmegtekintési tulajdonságok](./media/app-insights-sample-mscrm/29.png)

![Egy munkamenet oldal](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a>Mintakód
[Keresse meg a hello mintakód](https://dynamicsandappinsights.codeplex.com/).

## <a name="power-bi"></a>Power BI
Lehetőség van még mélyebb elemzés, ha Ön [hello adatok tooMicrosoft Power BI exportálása](app-insights-export-power-bi.md).

## <a name="sample-microsoft-dynamics-crm-solution"></a>A Microsoft Dynamics CRM megoldást
[Itt van megvalósítva a Microsoft Dynamics CRM hello megoldást](https://dynamicsandappinsights.codeplex.com/).

## <a name="learn-more"></a>Részletek
* [Mi az Application Insights?](app-insights-overview.md)
* [Az Application Insights webes](app-insights-javascript.md)
* [További mintákat és forgatókönyvek](app-insights-code-samples.md)
