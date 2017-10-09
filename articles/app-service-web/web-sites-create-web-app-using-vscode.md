---
title: "egy ASP.NET Core webalkalmazásba a Visual Studio Code aaaCreate"
description: "Ez az oktatóanyag bemutatja, hogyan toocreate az ASP.NET Core webalkalmazás Visual Studio Code használatával."
services: app-service\web
documentationcenter: .net
author: erikre
manager: erikre
editor: jimbe
ms.assetid: 877bff08-9ef7-405a-a1ca-1194f33c55f2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 02/26/2016
ms.author: cephalin
ms.openlocfilehash: 1c18c94984d71e88d2a5b792d68cb1c81e4a96d8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Az ASP.NET Core webalkalmazás létrehozása a Visual Studio Code
## <a name="overview"></a>Áttekintés
Az oktatóanyag bemutatja, hogyan toocreate az ASP.NET Core web app használatával [Visual Studio (kód VS)](http://code.visualstudio.com//Docs/whyvscode) , és telepítse azt túl[Azure App Service](../app-service/app-service-value-prop-what-is.md). 

> [!NOTE]
> Bár ez a cikk tooweb alkalmazások hivatkozik, is érvényes tooAPI és mobilalkalmazásokhoz. 
> 
> 

Az ASP.NET Core ASP.NET jelentős újratervezése. Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú használó webalkalmazások .NET készítéséhez. További információkért lásd: [bemutatása tooASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html). További információ az Azure App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Előfeltételek
* Telepítés [Visual STUDIO Code](http://code.visualstudio.com/Docs/setup).
* Telepítse a Git - telepítheti ezeket a helyeket származó: [Chocolatey](https://chocolatey.org/packages/git) vagy [git-scm.com](http://git-scm.com/downloads). Ha új tooGit, kattintson az [git-scm.com](http://git-scm.com/downloads) , hello lehetőségen túl**a parancssort a Windows hello használata Git**. Miután telepíti a Git, biztosítani kell tooset hello Git felhasználói név és e-mail, szükség van hello oktatóanyag későbbi részében (végrehajtásakor a véglegesítési VS kódból).  

## <a name="install-aspnet-core"></a>Az ASP.NET Core telepítése
Az ASP.NET Core egy lean .NET verem modern felhő- és OS X, Linux és Windows rendszeren futó webalkalmazások készítéséhez. Az készült másolatot tooprovide szabad hello a egy optimalizált fejlesztési keretrendszer vagy telepített toohello felhőalapú vagy helyszíni alkalmazásokhoz. Azt moduláris összetevőből áll, a minimális terhelés mellett, így rugalmasságot megőrzi a megoldások létrehozása során.

Ez az oktatóanyag használatba az ASP.NET Core hello legújabb fejlesztői verzióval rendelkező alkalmazások tervezett tooget. az utasításoknak hello adott tooWindows. OS X, Linux és a Windows telepítési utasításokért lásd: [Ismerkedés az ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Az OS X, Linux és a Windows telepítési utasításokat, lásd: [telepíti az ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx). 
> 
> 

## <a name="create-hello-web-app"></a>Hello webalkalmazás létrehozása
Ez a szakasz bemutatja, hogyan tooscaffold egy új alkalmazást ASP.NET webalkalmazás hello .NET parancssori eszköz segítségével. 

1. Adja meg a hello következő hello parancssor toocreate hello projekt mappa és scaffold hello app parancsot.
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![CLI - ASP.NET Core generátor DotNet](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. toorestore hello szükséges NuGet-csomagok, futtassa a következő parancs hello:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-hello-web-app-locally"></a>Futtassa helyben a hello webalkalmazás
Hello webalkalmazás létrehozása, és minden hello NuGet-csomagok hello alkalmazás beolvasni, hello webalkalmazás helyileg is futtathatja.

1. Hello alkalmazás futtatását (hello `dotnet run` parancs hello alkalmazást fog létrehozni, ha az elavult):
    ```terminal
    dotnet run
    ```
2. Nyisson meg egy böngészőt, és keresse meg a következő URL-cím toohello.
   
    **http://localhost:5000**
   
    hello alapértelmezett lap hello webes alkalmazás a következőképpen jelenik.
   
    ![A böngészőben a helyi webalkalmazás](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Zárja be a böngészőt. A hello **parancsablakot**, nyomja le az ENTER **Ctrl + C** hello alkalmazás és Bezárás hello tooshut **parancsablakot**. 

## <a name="create-a-web-app-in-hello-azure-portal"></a>A webalkalmazás létrehozása az Azure portál hello
hello lépések végigvezeti Önt egy webalkalmazás létrehozása a hello Azure portálon.

1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).
2. Kattintson a **új** hello a bal felső részén hello portálon.
3. Kattintson a **webalkalmazások > webalkalmazás**.
   
    ![Új Azure webalkalmazás](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Adjon meg egy értéket a **neve**, például a **SampleWebAppDemo**. Vegye figyelembe, hogy ez a név toobe egyedi kell, és hello portal érvényesíti, amely tooenter hello neve tett kísérlet során. Ezért ha egy adjon meg egy másik értéket választja, lesz szüksége, amely érték toosubstitute minden egyes előfordulásakor **SampleWebAppDemo** ebben az oktatóanyagban látható. 
5. Válasszon ki egy létező **App Service-csomag** vagy hozzon létre egy újat. Ha létrehoz egy új tervet, jelölje be a hello tarifacsomag, hely, és egyéb beállítások. App Service-csomagokról a további információkért lásd: hello cikk [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Az Azure új webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Kattintson a **Create** (Létrehozás) gombra.
   
    ![webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-hello-new-web-app"></a>Hello új webalkalmazáshoz tartozó Git-közzététel engedélyezése
Git olyan elosztott verziókezelő rendszer használható toodeploy az Azure App Service webalkalmazás. A webalkalmazás egy helyi Git-tárházban írt hello kódot fogja tárolni, és a kód tooAzure tooa távoli tárházba küldésével fogja telepíteni.   

1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com).
2. Kattintson a **Browse** (Tallózás) gombra.
3. Kattintson a **webalkalmazások** tooview hello webes alkalmazásokat az Azure-előfizetéshez társítva.
4. Ebben az oktatóanyagban létrehozott hello webalkalmazás kiválasztása.
5. Hello webalkalmazás panelen kattintson **beállítások** > **folyamatos üzembe helyezés**. 
   
    ![Az Azure web app állomás](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Kattintson a **forrás kiválasztása > helyi Git-tárház**.
7. Kattintson az **OK** gombra.
   
    ![Az Azure helyi Git-tárház](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Ha korábban már nem beállított üzembe helyezési hitelesítő adatok webes alkalmazás vagy többi App Service alkalmazás-közzététel, állítsa be őket most:
   
   * Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**. Hello **üzembe helyezési hitelesítő adatok beállítása** panel fog megjelenni.
   * Hozzon létre felhasználónevet és jelszót.  Szüksége lesz a jelszót később Git beállítása során.
   * Kattintson a **Save** (Mentés) gombra.
9. A webalkalmazás panelen kattintson **beállítások > Tulajdonságok**. hello távoli Git-tárház, amely üzembe toois mezőben látható URL-címe hello **GIT URL-cím**.
10. Másolás hello **GIT URL-cím** érték hello az oktatóanyag későbbi használatra.
    
    ![Az Azure Git URL-cím](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-tooazure-app-service"></a>A webes alkalmazás tooAzure App Service közzététele
Ebben a szakaszban egy helyi Git-tárház létrehozása, és a webes alkalmazás tooAzure az adott tárház tooAzure toodeploy leküldéses.

1. A Visual STUDIO Code, válassza ki a hello **Git** hello bal oldali navigációs sávon a beállítást.
   
    ![Visual STUDIO Code Git ikon](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Válassza ki **inicializálása git-tárház** toomake meg arról, hogy a munkaterület egy git verziókövetési. 
   
    ![Git inicializálása](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Nyissa meg a hello parancsablakot, és módosítsa a könyvtárakat toohello könyvtárához, a webes alkalmazást. Ezután írja be a következő parancs hello:
   
        git config core.autocrlf false
   
    Ez a parancs megakadályozza, hogy a szöveg, amelyben CRLF végződések javítása és Soremelés végződések javítása van szó kapcsolatos problémát.
4. Visual STUDIO Code, adja hozzá a véglegesítési üzenetet, majd kattintson a hello **véglegesítési összes** pipa ikon.
   
    ![Git összes véglegesítése](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. Miután Git feldolgozása befejeződött, látni fogja, hogy nincsenek-e hello Git ablak felsorolt fájlok **módosítások**. 
   
    ![Git nem történtek változások](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Módosítsa a hátsó toohello, ahol a webalkalmazás hello parancssor mutat, ahol toohello directory parancsablakot.
7. Frissítések tooyour webalkalmazás tárházat hello korábban kimásolt Git URL (".git" végződő) segítségével távoli hivatkozás létrehozása.
   
        git remote add azure [URL for remote repository]
8. Konfigurálhatja a Git toosave a hitelesítő adatok helyben, hogy legyenek, automatikusan hozzáfűzött tooyour leküldéses parancsok által létrehozott Visual STUDIO Code.
   
        git config credential.helper store
9. A módosítások tooAzure leküldéses hello a következő parancs beírásával. A kezdeti leküldéses tooAzure után nem tud toodo összes hello leküldéses parancsok VS kódból fogja. 
   
        git push -u azure master
   
    Korábban létrehozott Azure hello jelszó megadását kéri. **Megjegyzés: A jelszó nem lesznek láthatók.**
   
    hello hello fent parancs kimenete egy üzenetet, hogy a telepítés sikeres végződik.
   
        remote: Deployment successful.
        toohttps://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Ha módosítások tooyour app, közvetlenül a Visual STUDIO Code funkciójával hello beépített Git hello kiválasztásával újbóli **véglegesíti az összes** beállítás követ hello **leküldéses** lehetőséget. Hello található **leküldéses** beállítás elérhető, a legördülő menü következő toohello hello **véglegesíti az összes** és **frissítése** gombokat.
> 
> 

Ha a projekt toocollaborate van szüksége, fontolja meg Between tooAzure küldését tooGitHub kérdez le.

## <a name="run-hello-app-in-azure"></a>Hello alkalmazás futtatása az Azure-ban
Most, hogy a webes alkalmazást telepített, akkor most amíg Azure-ban üzemeltetett hello alkalmazás futtatását. 

Ez kétféleképpen teheti:

* Nyisson meg egy böngészőt, és az alábbiak szerint adja meg a webalkalmazás hello nevét.   
  
        http://SampleWebAppDemo.azurewebsites.net
* A hello Azure portál, hello webalkalmazás a webalkalmazás panelen keresse meg és kattintson a **Tallózás** tooview az alkalmazás 
* az alapértelmezett böngészőben.

![Azure-webalkalmazásban](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban, megtudta, hogyan toocreate egy webalkalmazásba a Visual STUDIO Code, és telepítse azt tooAzure. Visual STUDIO Code kapcsolatos további információkért lásd: hello cikk [miért Visual Studio Code?](https://code.visualstudio.com/Docs/) További információ az App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md). 

