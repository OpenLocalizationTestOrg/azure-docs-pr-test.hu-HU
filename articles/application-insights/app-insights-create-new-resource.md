---
title: "egy új Azure Application Insights-erőforrás aaaCreate |} Microsoft Docs"
description: "Manuálisan állítsa be az Application Insights egy új élő alkalmazás figyelését."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: bwren
ms.openlocfilehash: 3aba7045e1f8fe43d473f0be01dd52106ab992a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-insights-resource"></a>Application Insights-erőforrás létrehozása
Azure Application Insights az alkalmazással kapcsolatos adatokat jeleníti meg a Microsoft Azure *erőforrás*. Új erőforrás létrehozása része ezért [beállítása az Application Insights toomonitor egy új alkalmazás][start]. Sok esetben erőforrás létrehozása automatikusan úgy teheti hello IDE. De néhány esetben erőforrás-létrehozhat egy manuálisan – például külön erőforrások toohave fejlesztési és éles az alkalmazás létrehozza.

Hello erőforrás létrehozása után annak instrumentation kulcs lekérése, és adott tooconfigure hello SDK hello alkalmazás használja. hello erőforrás kulcs hivatkozások hello telemetriai toohello erőforrás.

## <a name="sign-up-toomicrosoft-azure"></a>Regisztráljon az Azure tooMicrosoft
Ha még nem kapott egy [Microsoft fiókot, egy letöltése](http://live.com). (Például Outlook.com, OneDrive, Windows Phone vagy XBox Live-szolgáltatást használ, ha már rendelkezik Microsoft-fiók.)

Emellett szükség van egy előfizetési túl[Microsoft Azure](http://azure.com). Ha a csapat vagy szervezet Azure-előfizetéssel, hello tulajdonosa adhat hozzá, tooit, a Windows Live ID azonosítójával. Most csak felszámított a valóban használt funkciókért. hello alapértelmezett alapszintű kísérleti díjmentesen használható bizonyos mennyiségű teszi lehetővé.

Amikor hozzáférési tooa előfizetése van, jelentkezzen be, tooApplication Insights [http://portal.azure.com](https://portal.azure.com), és a Live ID toologin használja.

## <a name="create-an-application-insights-resource"></a>Application Insights-erőforrás létrehozása
A hello [portal.azure.com](https://portal.azure.com), vegyen fel egy Application Insights-erőforrást:

![Kattintson az Új, majd az Application Insights lehetőségre](./media/app-insights-create-new-resource/01-new.png)

* **Az alkalmazástípus** elemnél mi jelenik hello áttekintése panel és hello tulajdonságok érhetők el [metrika explorer][metrics]. Ha nem látja a típusú alkalmazást, válassza az általános.
* **Előfizetés** a fizetési fiók az Azure-ban.
* **Erőforráscsoport** a könnyebb tulajdonságainak kezelésére van, például hozzáférés-vezérlést. Ha már létrehozott más Azure-erőforrások, választhatja ki tooput az új erőforrás hello ugyanabban a csoportban.
* **Hely** van, ahol azt megőrizni az adatokat.
* **PIN-kód toodashboard** helyezi az erőforrás egy gyors elérést csempe az Azure kezdőlapján. Ajánlott.

Ha az alkalmazás létrehozása után egy új panelen nyitja meg. Ezen a panelen, ahol látható teljesítmény- és használati adatokat az alkalmazásra vonatkozó. 

tooget hátsó tooit következő bejelentkezéskor tooAzure, keresse meg az alkalmazás gyors üzembe helyezési csempe hello indítsa el board (kezdőképernyő). Vagy kattintson a Tallózás toofind azt.

## <a name="copy-hello-instrumentation-key"></a>Hello instrumentation kulcs másolása
hello instrumentation kulcs hello erőforrás létrehozott azonosítja. Esetleg szükség lenne rá toogive toohello SDK.

![Essentials kattintson, majd hello Instrumentation kulcs CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-hello-sdk-in-your-app"></a>Az alkalmazás hello SDK telepítése
Hello Application Insights SDK telepítése az alkalmazásban. Ezt a lépést az alkalmazás hello típusú fokozottan függ. 

Használja a hello instrumentation kulcs tooconfigure [, amely az alkalmazás telepítése SDK hello][start].

hello SDK magában foglalja a telemetriai adatokat küldhet anélkül, hogy toowrite kódok modulban. tootrack felhasználói műveletek vagy eseményadatokat részletesen, [hello API-t használó] [ api] toosend saját telemetriai adatokat.

## <a name="monitor"></a>Lásd: a telemetriai adatok
Gyors Bezárás hello hello Azure-portál panel tooreturn tooyour alkalmazás panel elindítása.

Kattintson a hello keresési csempe toosee [diagnosztikai keresési][diagnostic], ahol a hello első események jelennek meg. 

Ha több adatot várt, kattintson a **frissítése** néhány másodperc múlva.

## <a name="creating-a-resource-automatically"></a>Erőforrás automatikus létrehozása
Írhat egy [PowerShell-parancsfájl](app-insights-powershell.md) toocreate erőforrás automatikusan.

## <a name="next-steps"></a>Következő lépések
* [Irányítópult létrehozása](app-insights-dashboards.md)
* [Diagnosztikai keresés](app-insights-diagnostic-search.md)
* [Metrikák böngészése](app-insights-metrics-explorer.md)
* [Analytics-lekérdezések](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

