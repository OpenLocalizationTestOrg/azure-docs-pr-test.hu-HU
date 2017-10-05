---
title: "Az ASP.NET Core webalkalmazás létrehozása a Visual Studio Code"
description: "Ez az oktatóanyag bemutatja, hogyan használja a Visual Studio Code az ASP.NET Core-webalkalmazás létrehozása."
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
ms.openlocfilehash: 46e3852dc84265de41bb358f482dec06608e7efa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-aspnet-core-web-app-in-visual-studio-code"></a>Az ASP.NET Core webalkalmazás létrehozása a Visual Studio Code
## <a name="overview"></a>Áttekintés
Ez az oktatóanyag bemutatja, hogyan hozzon létre az ASP.NET Core web app [Visual Studio (kód VS)](http://code.visualstudio.com//Docs/whyvscode) és telepítheti azt [Azure App Service](../app-service/app-service-value-prop-what-is.md). 

> [!NOTE]
> Habár ez a cikk a webalkalmazásokra vonatkozik, az API-alkalmazásokra és mobilalkalmazásokra egyaránt érvényes. 
> 
> 

Az ASP.NET Core ASP.NET jelentős újratervezése. Az ASP.NET Core egy új nyílt forráskódú és platformfüggetlen keretrendszer modern felhőalapú használó webalkalmazások .NET készítéséhez. További információkért lásd: [Bevezetés az ASP.NET Core](http://docs.asp.net/latest/conceptual-overview/aspnet.html). További információ az Azure App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md).

[!INCLUDE [app-service-web-try-app-service.md](../../includes/app-service-web-try-app-service.md)]

## <a name="prerequisites"></a>Előfeltételek
* Telepítés [Visual STUDIO Code](http://code.visualstudio.com/Docs/setup).
* Telepítse a Git - telepítheti ezeket a helyeket származó: [Chocolatey](https://chocolatey.org/packages/git) vagy [git-scm.com](http://git-scm.com/downloads). Ha most ismerkedik a Git, válassza a [git-scm.com](http://git-scm.com/downloads) , és jelölje be a **használható Git a Windows parancssor**. Miután telepíti a Git, is szüksége lesz a Git-felhasználó nevét és e-mail-, szükség van az oktatóanyag későbbi részében (végrehajtásakor a véglegesítési VS kódból).  

## <a name="install-aspnet-core"></a>Az ASP.NET Core telepítése
Az ASP.NET Core egy lean .NET verem modern felhő- és OS X, Linux és Windows rendszeren futó webalkalmazások készítéséhez. Az épített egészen az alapoktól egy optimalizált fejlesztési keretrendszer biztosít a felhőbe telepített vagy a helyszíni alkalmazások. Azt moduláris összetevőből áll, a minimális terhelés mellett, így rugalmasságot megőrzi a megoldások létrehozása során.

Ez az oktatóanyag célja, hogy elkezdhesse alkalmazásokat ASP.NET Core legújabb fejlesztői verzióját. Az alábbi utasítások alapján Windows vonatkoznak. OS X, Linux és a Windows telepítési utasításokért lásd: [Ismerkedés az ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started). 


> [!NOTE]
> Az OS X, Linux és a Windows telepítési utasításokat, lásd: [telepíti az ASP.NET Core](https://code.visualstudio.com/Docs/ASPnet5#_installing-aspnet-5-and-dnx). 
> 
> 

## <a name="create-the-web-app"></a>A webapp létrehozása
Ez a szakasz bemutatja, hogyan generálja le egy új alkalmazást ASP.NET webalkalmazást a .NET parancssori eszközzel. 

1. Adja meg a következő parancsot a parancssorba a projektmappa létrehozásához, és ezek az alkalmazást.
   
```terminal
mkdir SampleWebApp
cd SampleWebApp
dotnet new mvc
```
![CLI - ASP.NET Core generátor DotNet](./media/web-sites-create-web-app-using-vscode/dotnetcore-mvc-01.png)

2. A szükséges NuGet-csomagok visszaállításához futtassa a következő parancsot:
   
    ```terminal
    dotnet restore
    ```

## <a name="run-the-web-app-locally"></a>Futtassa helyben a webes alkalmazás
Most, hogy létrehozta a webalkalmazást és lekérése az alkalmazáshoz tartozó NuGet-csomagok, a webalkalmazást helyileg is futtathatja.

1. Futtassa az alkalmazást (a `dotnet run` parancs fog létrehozni az alkalmazás elavult esetén):
    ```terminal
    dotnet run
    ```
2. Nyisson meg egy böngészőt, és keresse meg a következő URL-címet.
   
    **http://localhost:5000**
   
    A webalkalmazás az alapértelmezett lapon jelenik meg az alábbiak szerint.
   
    ![A böngészőben a helyi webalkalmazás](./media/web-sites-create-web-app-using-vscode/08-web-app.png)
3. Zárja be a böngészőt. Az a **parancsablakot**, nyomja le az ENTER **Ctrl + C** az alkalmazás és a záró le kell állítania a **parancsablakot**. 

## <a name="create-a-web-app-in-the-azure-portal"></a>Webalkalmazás létrehozása az Azure portálon
Az alábbi lépéseket végigvezeti Önt egy webalkalmazás létrehozása az Azure portálon.

1. Jelentkezzen be az [Azure Portal](https://portal.azure.com).
2. Kattintson a **új** a portál bal felső részén.
3. Kattintson a **webalkalmazások > webalkalmazás**.
   
    ![Új Azure webalkalmazás](./media/web-sites-create-web-app-using-vscode/09-azure-newwebapp.png)
4. Adjon meg egy értéket a **neve**, például a **SampleWebAppDemo**. Vegye figyelembe, hogy ez a név egyedinek kell lennie, és a portál érvényesíti, amely a nevének megadása tett kísérlet során. Ezért ha egy adjon meg egy másik értéket választja, meg kell helyettesítse be ezt az értéket minden egyes előfordulásakor **SampleWebAppDemo** ebben az oktatóanyagban látható. 
5. Válasszon ki egy létező **App Service-csomag** vagy hozzon létre egy újat. Ha létrehoz egy új tervet, válassza ki a tarifacsomagot, helyét és egyéb beállítások. App Service-csomagokról a további információkért lásd: a cikk [Azure App Service-csomagok részletes áttekintése](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
   
    ![Az Azure új webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/10-azure-newappblade.png)
6. Kattintson a **Create** (Létrehozás) gombra.
   
    ![webalkalmazás panelen](./media/web-sites-create-web-app-using-vscode/11-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Az új webalkalmazáshoz tartozó Git-közzététel engedélyezése
Git olyan elosztott verziókezelő rendszer, amely segítségével az Azure App Service webalkalmazás telepítése. A webalkalmazásához írt kódot egy helyi Git-tárházban fogja tárolni, és a kódot az Azure-on a kód egy távoli tárházba történő küldésével fogja üzembe helyezni.   

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com).
2. Kattintson a **Browse** (Tallózás) gombra.
3. Kattintson a **webalkalmazások** a web Apps, az Azure-előfizetéshez társított alkalmazások listájának megtekintéséhez.
4. Ebben az oktatóanyagban létrehozott webalkalmazás kiválasztása
5. A webalkalmazás panelen kattintson **beállítások** > **folyamatos üzembe helyezés**. 
   
    ![Az Azure web app állomás](./media/web-sites-create-web-app-using-vscode/14-azure-deployment.png)
6. Kattintson a **forrás kiválasztása > helyi Git-tárház**.
7. Kattintson az **OK** gombra.
   
    ![Az Azure helyi Git-tárház](./media/web-sites-create-web-app-using-vscode/15-azure-localrepository.png)
8. Ha korábban már nem beállított üzembe helyezési hitelesítő adatok webes alkalmazás vagy többi App Service alkalmazás-közzététel, állítsa be őket most:
   
   * Kattintson a **beállítások** > **üzembe helyezési hitelesítő adatok**. A **üzembe helyezési hitelesítő adatok beállítása** panel fog megjelenni.
   * Hozzon létre felhasználónevet és jelszót.  Szüksége lesz a jelszót később Git beállítása során.
   * Kattintson a **Save** (Mentés) gombra.
9. A webalkalmazás panelen kattintson **beállítások > Tulajdonságok**. A távoli teheti elérhetővé a Git-tárház URL-címe alatt látható **GIT URL-cím**.
10. Másolás a **GIT URL-cím** értéket az oktatóanyag későbbi használatra.
    
    ![Az Azure Git URL-cím](./media/web-sites-create-web-app-using-vscode/17-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>A webalkalmazás közzététele az Azure App Service
Ebben a szakaszban hoz létre egy helyi Git-tárház és leküldéses ebből az Azure-bA a webalkalmazás telepítése az Azure-bA.

1. A Visual STUDIO Code, válassza ki a **Git** lehetőséget a bal oldali navigációs sávon.
   
    ![Visual STUDIO Code Git ikon](./media/web-sites-create-web-app-using-vscode/git-icon.png)
2. Válassza ki **inicializálása git-tárház** ellenőrizze, hogy a munkaterület git verziókezelő alatt. 
   
    ![Git inicializálása](./media/web-sites-create-web-app-using-vscode/19-initgit.png)
3. Nyissa meg a parancsablakot, és módosítsa a könyvtárat könyvtárához, a webes alkalmazást. Ezután írja be a következő parancsot:
   
        git config core.autocrlf false
   
    Ez a parancs megakadályozza, hogy a szöveg, amelyben CRLF végződések javítása és Soremelés végződések javítása van szó kapcsolatos problémát.
4. Visual STUDIO Code, adja hozzá a véglegesítési üzenetet, majd kattintson a **véglegesítési összes** pipa ikon.
   
    ![Git összes véglegesítése](./media/web-sites-create-web-app-using-vscode/20-git-commit.png)
5. Miután Git feldolgozása befejeződött, látni fogja, hogy nincsenek-e a Git ablak felsorolt fájlok **módosítások**. 
   
    ![Git nem történtek változások](./media/web-sites-create-web-app-using-vscode/no-changes.png)
6. Állítsa vissza a hol helyezkedik el a webalkalmazás ahol arra a könyvtárra mutat a parancssor parancssori ablakban.
7. A frissítések terjesztése a webalkalmazáshoz korábban kimásolt Git URL (".git" végződő) segítségével távoli hivatkozás létrehozása.
   
        git remote add azure [URL for remote repository]
8. Helyileg mentheti a hitelesítő adatokat, így azok automatikusan hozzáfűzi a Visual STUDIO Code előállított leküldéses parancsok Git beállítható.
   
        git config credential.helper store
9. Továbbítsa a módosításokat az Azure, a következő parancs beírásával. Az Azure-ba, a kezdeti leküldéses után lesz a leküldéses parancsok ehhez a Visual STUDIO-kódjában. 
   
        git push -u azure master
   
    Kéri a jelszót, amelyet korábban az Azure-ban. **Megjegyzés: A jelszó nem lesznek láthatók.**
   
    A fenti parancs kimenetében, hogy a telepítés sikeres üzenet végződik.
   
        remote: Deployment successful.
        To https://user@testsite.scm.azurewebsites.net/testsite.git
        [new branch]      master -> master

> [!NOTE]
> Ha módosítja az alkalmazáshoz, közvetlenül a funkcióval a beépített Git kiválasztásával VS kód újbóli a **véglegesíti az összes** beállítás követi a **leküldéses** lehetőséget. Megtalálja a **leküldéses** beállítás található meg a legördülő menü mellett a **véglegesíti az összes** és **frissítése** gombokat.
> 
> 

Ha ülve dolgozhatnak van szüksége, fontolja meg a GitHub Between kérdez le, hogy Azure kérdez le.

## <a name="run-the-app-in-azure"></a>Futtassa az alkalmazást az Azure-ban
Most, hogy a webes alkalmazást telepített, akkor most futtassa az alkalmazást, miközben az Azure-ban üzemeltetett. 

Ez kétféleképpen teheti:

* Nyisson meg egy böngészőt, és az alábbiak szerint adja meg a webalkalmazás nevét.   
  
        http://SampleWebAppDemo.azurewebsites.net
* Az Azure portálon keresse meg a webalkalmazás panelen a webalkalmazás, és kattintson **Tallózás** az alkalmazás megtekintéséhez 
* az alapértelmezett böngészőben.

![Azure-webalkalmazásban](./media/web-sites-create-web-app-using-vscode/21-azurewebapp.png)

## <a name="summary"></a>Összefoglalás
Ebben az oktatóprogramban megtanulhatta webalkalmazás létrehozása a Visual STUDIO Code, és telepítheti az Azure. Visual STUDIO Code kapcsolatos további információkért lásd: a cikk [miért Visual Studio Code?](https://code.visualstudio.com/Docs/) További információ az App Service web apps: [webes alkalmazások – áttekintés](app-service-web-overview.md). 

