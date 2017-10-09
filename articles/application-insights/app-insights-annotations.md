---
title: az Application Insights aaaRelease jegyzetek |} Microsoft Docs
description: "Központi telepítés hozzáadása, vagy összeállíthatja a jelölők tooyour metrikák explorer diagramokat az Application Insightsban."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: 23173e33-d4f2-4528-a730-913a8fd5f02e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: bwren
ms.openlocfilehash: e802d22701cb69e96fd1a6b469ea67454195f77a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="annotations-on-metric-charts-in-application-insights"></a>Az Application Insightsban metrika diagramok jegyzetek
A jegyzetek [Metrikaböngésző](app-insights-metrics-explorer.md) diagramok megjelenítése, amelyen rendszerbe van állítva egy új buildverziót, vagy más jelentős esemény történt. Ezek teszik, hogy könnyen toosee, hogy a módosítások az alkalmazás teljesítményére hatással volt. Ezek automatikusan létrehozhatók hello [Visual Studio Team Services rendszer build](https://www.visualstudio.com/en-us/get-started/build/build-your-app-vs). Is létrehozhat jegyzetek tooflag tetszés szerint mindenképpen [hozza létre őket a Powershellből](#create-annotations-from-powershell).

![Kiszolgáló válaszideje látható korrelációban állnak a jegyzetek – példa](./media/app-insights-annotations/00.png)



## <a name="release-annotations-with-vsts-build"></a>Kiadási jegyzetek a VSTS-build

Kiadási jegyzetek a hello felhőalapú build szolgáltatása, és felszabadíthatja a Visual Studio Team Services szolgáltatás. 

### <a name="install-hello-annotations-extension-one-time"></a>Hello jegyzetek kiterjesztés (egyszer) telepítése
toobe képes toocreate kiadási jegyzetek, szüksége lesz egy tooinstall a hello sok Team bővítmények érhető el a Visual Studio piactér hello.

1. Jelentkezzen be tooyour [Visual Studio Team Services](https://www.visualstudio.com/en-us/get-started/setup/sign-up-for-visual-studio-online) projekt.
2. A Visual Studio piactéren [hello kiadási jegyzetek bővítmény](https://marketplace.visualstudio.com/items/ms-appinsights.appinsightsreleaseannotations), és adja hozzá tooyour Team Services-fiók.

![At Team Services weblap, nyissa meg piactér jobb felső. Jelölje ki Visual Team Services és a Build és verziószám, válassza lásd: több.](./media/app-insights-annotations/10.png)

Csak akkor kell toodo egyszer ebben a Visual Studio Team Services-fiókhoz. Kiadási jegyzetek a projektre a fiókját most konfigurálhatók. 

### <a name="configure-release-annotations"></a>Kiadási jegyzetek konfigurálása

Minden VSTS-kiadás sablon tooget egy külön API-kulcs szükséges.

1. Jelentkezzen be toohello [Microsoft Azure portál](https://portal.azure.com) , és nyissa meg a hello Application Insights-erőforrást, amely az alkalmazás figyeli. (Vagy [létrehozhat egy tárhelyet](app-insights-overview.md), ha még nem tette meg még.)
2. Nyissa meg **API-hozzáférés**, **Application Insights azonosító**.
   
    ![A portal.azure.com nyissa meg az Application Insights-erőforrást, majd válassza a beállítások. Nyissa meg az API-hozzáférést. Másolás hello Alkalmazásazonosító](./media/app-insights-annotations/20.png)

4. Egy külön böngészőablakban nyissa meg a (vagy hozzon létre), amely a központi telepítések kezeli a Visual Studio Team Services hello kiadási sablon. 
   
    Adjon hozzá egy feladatot, és válassza a hello Application Insights kiadási Megjegyzés feladat hello menüpontot.
   
    Beillesztés hello **alkalmazásazonosító** hello API-hozzáférés panel fájlból másolt.
   
    ![A Visual Studio Team Services nyissa meg a kiadási válassza ki a kiadási definícióját, kattintson a Szerkesztés. A feladat hozzáadása gombra, és válassza ki az Application Insights kiadási megjegyzés. Illessze be a hello Application Insights azonosítóját.](./media/app-insights-annotations/30.png)
4. Set hello **APIKey** mező tooa változó `$(ApiKey)`.

5. Hello Azure ablak hozzon létre egy új API-kulcsot, és igénybe vehet egy példányát.
   
    ![Hello API-hozzáférés panel az hello Azure ablak kattintson az API-kulcs létrehozása. Adja meg a megjegyzést, ellenőrizze az írási jegyzeteket és kattintson a kulcs létrehozása. Hello új kulcs másolása.](./media/app-insights-annotations/40.png)

6. Nyissa meg a hello kiadási sablon hello konfiguráció lapon.
   
    Hozzon létre egy változó definíciója `ApiKey`.
   
    Illessze be az API fő toohello ApiKey változó definícióját.
   
    ![Hello Team Services ablakban jelölje ki a hello konfigurációs lapján, és kattintson a változó hozzáadása. Hello neve tooApiKey beállítása és történő hello érték, illessze be az imént létrehozott hello kulcs, és kattintson hello zárolása.](./media/app-insights-annotations/50.png)
7. Végezetül **mentése** hello kiadás definíciója.


## <a name="view-annotations"></a>Jegyzetek megtekintése
Most hello kiadási sablon toodeploy új kiadását használja, amikor a jegyzet küld tooApplication Insights. hello jegyzetek a Metrikaböngészőben diagramok jelenik meg.

Kattintson a bármely hello kiadás, például kérelmező, forrás vezérlő fiókirodai, kiadási definition, környezetre és egyéb jegyzet jelölő tooopen részleteit.

![Kattintson a kiadási Megjegyzés jelölő.](./media/app-insights-annotations/60.png)

## <a name="create-custom-annotations-from-powershell"></a>Hozzon létre egyéni jegyzetek a Powershellből
Jegyzetek e folyamat (nélkül használja a Visual STUDIO Team System) is létrehozhat. 


1. Helyi másolat készítése a hello [Powershell-parancsfájl a Githubról](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1).

2. Hello Alkalmazásazonosító beszerzése és hello API-hozzáférés panelen az API-kulcs létrehozása.

3. Ehhez hasonló hello parancsfájl hívása:

```PS

     .\CreateReleaseAnnotation.ps1 `
      -applicationId "<applicationId>" `
      -apiKey "<apiKey>" `
      -releaseName "<myReleaseName>" `
      -releaseProperties @{
          "ReleaseDescription"="a description";
          "TriggerBy"="My Name" }
```

Egyszerű toomodify hello parancsfájl, például toocreate jegyzetek az elmúlt hello.

## <a name="next-steps"></a>Következő lépések

* [Munkaelemek létrehozása](app-insights-diagnostic-search.md#create-work-item)
* [Automatizálása a PowerShell](app-insights-powershell.md)
