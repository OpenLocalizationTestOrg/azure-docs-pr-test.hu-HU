---
title: "a webes alkalmazás a Redis Cache aaaHow toocreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate a webes alkalmazás a Redis Cache segítségével"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 05/09/2017
ms.author: sdanie
ms.openlocfilehash: d3e6df97b06fdf9032570dc360944be4bd7715de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-web-app-with-redis-cache"></a>Hogyan toocreate a webes alkalmazás a Redis Cache segítségével
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Ez az oktatóanyag bemutatja, hogyan toocreate és egy ASP.NET alkalmazás tooa webes webalkalmazás telepítése az Azure App Service segítségével a Visual Studio 2017. hello mintaalkalmazás adatbázis team statisztikáit listáját jeleníti meg, és különböző módokon toouse Azure Redis Cache toostore jeleníti meg, és hello gyorsítótár adatainak lekérése. Hello oktatóanyag befejezésekor kell, hogy beolvassa és tooa adatbázis, az Azure Redis Cache optimalizált, fut, és az Azure-ban futó webalkalmazás.

Az oktatóanyagból a következőket sajátíthatja el:

* Hogyan toocreate egy ASP.NET MVC 5 webalkalmazást a Visual Studióban.
* Hogyan tooaccess-adatbázisból egy entitás-keretrendszer használatával.
* Hogyan tooimprove adatátvitelt, ami csökkenti az adatbázis betöltési tárolja és használja az Azure Redis Cache-adatok beolvasása.
* Egy Redis toouse rendezését set tooretrieve hello felső 5 csoportok.
* Hogyan tooprovision hello Azure-erőforrások hello alkalmazás Resource Manager-sablon használatával.
* Hogyan toopublish hello alkalmazás tooAzure Visual Studio használatával.

## <a name="prerequisites"></a>Előfeltételek
toocomplete hello útmutató, rendelkeznie kell a következő előfeltételek hello.

* [Azure-fiók](#azure-account)
* [Az Azure SDK for .NET hello Visual Studio 2017](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Azure-fiók
Egy Azure-fiók toocomplete hello oktatóanyag van szüksége. A következőket teheti:

* [Nyisson egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero). Amelyek lehetnek kimenő használt tootry fizetős Azure-szolgáltatások jóváírásokat kap. Hello jóváírásokat el is használta, után is megtarthatja hello fiókot, és ingyenes Azure-szolgáltatások és funkciók használatára.
* [Aktiválja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). Az MSDN-előfizetés minden hónapban biztosít Önnek krediteket, amelyekkel fizetős Azure-szolgáltatásokat használhat.

### <a name="visual-studio-2017-with-hello-azure-sdk-for-net"></a>Az Azure SDK for .NET hello Visual Studio 2017
hello az oktatóanyag a Visual Studio 2017 számára íródott hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools). hello Azure SDK 2.9.5 hello Visual Studio telepítő részét képezi.

Ha Visual Studio 2015-öt, amelyeket követve hello hello oktatóanyag [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 vagy újabb. [Letöltési hello legfrissebb Azure SDK-t a Visual Studio 2015 Itt](http://go.microsoft.com/fwlink/?linkid=518003). Ha már nincs a Visual Studio automatikusan telepítve hello SDK. Néhány képernyő megjelenése hello ábrán látható módon az oktatóanyag eltérhet.

Ha a Visual Studio 2013 van, akkor [letöltési hello legfrissebb Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Néhány képernyő megjelenése hello ábrán látható módon az oktatóanyag eltérhet.

## <a name="create-hello-visual-studio-project"></a>Hello Visual Studio-projekt létrehozása
1. Nyissa meg a Visual Studio alkalmazást, majd kattintson a **File** (File), **New** (Új), **Project** (Projekt) lehetőségre.
2. Bontsa ki a hello **Visual C#** hello csomópontja **sablonok** listáról válassza ki **felhő**, és kattintson a **ASP.NET Web Application**. Győződjön meg arról, hogy a **.NET Framework 4.5.2** vagy újabb keretrendszer van kiválasztva.  Típus **ContosoTeamStats** történő hello **neve** szövegmezőben kattintson **OK**.
   
    ![Projekt létrehozása][cache-create-project]
3. Válassza ki **MVC** hello projekt típusként. 

    Győződjön meg arról, hogy **nem hitelesítési** hello megadott **hitelesítési** beállításait. Attól függően, hogy a Visual Studio verziójának hello alapértelmezett számbavételekhez állítható be más toosomething. toochange, kattintson a **hitelesítés módosítása** válassza **nem hitelesítési**.

    Ha a Visual Studio 2015 együtt, törölje a jelet hello **hello felhőben lévő gazdagéphez** jelölőnégyzetet. Programra [rendelkezés hello Azure-erőforrások](#provision-the-azure-resources) és [hello alkalmazás tooAzure közzététele](#publish-the-application-to-azure) a későbbi lépésekben hello oktatóanyag. Példa egy App Service webalkalmazásba a Visual Studio eszközből kiépítés távozó **hello felhőben lévő gazdagéphez** be van jelölve, lásd: [Ismerkedés a webalkalmazásokkal az Azure App Service szolgáltatásban, az ASP.NET és a Visual Studio használatával](../app-service-web/app-service-web-get-started-dotnet.md).
   
    ![Projektsablon kiválasztása][cache-select-template]
4. Kattintson a **OK** toocreate hello projekt.

## <a name="create-hello-aspnet-mvc-application"></a>Hello ASP.NET MVC alkalmazás létrehozása
Ebben a szakaszban hello oktatóanyag létre fog hozni hello alapvető alkalmazás, amely olvas, és az adatbázis csoport statisztikáit jeleníti meg.

* [Hello Entity Framework NuGet-csomag hozzáadása](#add-the-entity-framework-nuget-package)
* [Hello modell hozzáadása](#add-the-model)
* [Hello vezérlő hozzáadása](#add-the-controller)
* [Hello nézetek konfigurálásához](#configure-the-views)

### <a name="add-hello-entity-framework-nuget-package"></a>Hello Entity Framework NuGet-csomag hozzáadása

1. Kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **eszközök** menü.
2. Futtatási hello következő parancsot a hello **Csomagkezelő konzol** ablak.
    
    ```
    Install-Package EntityFramework
    ```

Ezzel a csomaggal kapcsolatos további információkért lásd: hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet lap.

### <a name="add-hello-model"></a>Hello modell hozzáadása
1. Kattintson a jobb gombbal a **Models** (Modellek) elemre a **Solution Explorer** (Megoldáskezelő) területén, és válassza az **Add** (Hozzáadás), **Class** (Osztály) lehetőségeket. 
   
    ![Modell hozzáadása][cache-model-add-class]
2. Adja meg `Team` hello osztály nevét, majd kattintson **Hozzáadás**.
   
    ![Modellosztály hozzáadása][cache-model-add-class-dialog]
3. Cserélje le a hello `using` hello hello tetején utasítások `Team.cs` hello következőre fájl `using` utasításokat.

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. Cserélje le a hello hello definíciója `Team` a következő kódrészletet, amely tartalmaz egy frissített hello osztályt `Team` definícióját, valamint néhány más Entity Framework segítőosztályok osztályban. Hello kód első megközelítés tooEntity keretrendszer, amely ebben az oktatóanyagban használt további információkért lásd: [kód első tooa új adatbázis](https://msdn.microsoft.com/data/jj193542).

    ```c#
    public class Team
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
        public int Ties { get; set; }
    
        static public void PlayGames(IEnumerable<Team> teams)
        {
            // Simple random generation of statistics.
            Random r = new Random();
    
            foreach (var t in teams)
            {
                t.Wins = r.Next(33);
                t.Losses = r.Next(33);
                t.Ties = r.Next(0, 5);
            }
        }
    }
    
    public class TeamContext : DbContext
    {
        public TeamContext()
            : base("TeamContext")
        {
        }
    
        public DbSet<Team> Teams { get; set; }
    }
    
    public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
    {
        protected override void Seed(TeamContext context)
        {
            var teams = new List<Team>
            {
                new Team{Name="Adventure Works Cycles"},
                new Team{Name="Alpine Ski House"},
                new Team{Name="Blue Yonder Airlines"},
                new Team{Name="Coho Vineyard"},
                new Team{Name="Contoso, Ltd."},
                new Team{Name="Fabrikam, Inc."},
                new Team{Name="Lucerne Publishing"},
                new Team{Name="Northwind Traders"},
                new Team{Name="Consolidated Messenger"},
                new Team{Name="Fourth Coffee"},
                new Team{Name="Graphic Design Institute"},
                new Team{Name="Nod Publishers"}
            };
    
            Team.PlayGames(teams);
    
            teams.ForEach(t => context.Teams.Add(t));
            context.SaveChanges();
        }
    }
    
    public class TeamConfiguration : DbConfiguration
    {
        public TeamConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
        }
    }
    ```


1. A **Megoldáskezelőben**, kattintson duplán a **web.config** tooopen azt.
   
    ![Web.config][cache-web-config]
2. Adja hozzá a következő hello `connectionStrings` szakasz. hello hello kapcsolati karakterlánc nevét meg kell egyeznie a hello Entity Framework adatbázis környezeti osztályt, amely van hello neve `TeamContext`.

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    Hozzáadhat új hello `connectionStrings` , hogy azt a következő szakasz `configSections`, ahogy az alábbi példa hello.

    ```xml
    <configuration>
      <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
      </configSections>
      <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
      </connectionStrings>
      ...
      ```

    > [!NOTE]
    > A kapcsolati karakterláncot a Visual Studio hello verziójától függően eltérő lehet, és az SQL Server Express edition használt toocomplete hello oktatóanyag. hello web.config sablon kell konfigurált toomatch a telepítést, és tartalmazhat `Data Source` bejegyzéseket, például `(LocalDB)\v11.0` (az SQL Server Express 2012-ben) vagy `Data Source=(LocalDB)\MSSQLLocalDB` (az SQL Server Express 2014 és újabb). További információ a kapcsolati karakterláncokról és az SQL Express-verziókról: [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .

### <a name="add-hello-controller"></a>Hello vezérlő hozzáadása
1. Nyomja le az **F6** toobuild hello projekt. 
2. A **Megoldáskezelőben**, kattintson a jobb gombbal hello **tartományvezérlők** mappa, és válassza **Hozzáadás**, **vezérlő**.
   
    ![Vezérlő hozzáadása][cache-add-controller]
3. Válassza az **MVC 5 Controller with views, using Entity Framework** (MVC 5 vezérlő nézetekkel, az Entity Framework használatával) lehetőséget, majd kattintson az **Add** (Hozzáadás) lehetőségre. Ha hibaüzenetet kap, miután rákattintott **Hozzáadás**, győződjön meg arról, hogy először rendelkezik beépített hello projekt.
   
    ![Vezérlőosztály hozzáadása][cache-add-controller-class]
4. Válassza ki **Team (ContosoTeamStats.Models)** a hello **Model class** legördülő listából. Válassza ki **TeamContext (ContosoTeamStats.Models)** a hello **adatok környezetben osztály** legördülő listából. Típus `TeamsController` a hello **vezérlő** (Ha nem automatikusan a telepítéskor) szövegmezőben. Kattintson a **Hozzáadás** toocreate hello vezérlő osztályhoz, és adja hozzá a hello alapértelmezett nézeteket.
   
    ![Vezérlő konfigurálása][cache-configure-controller]
5. A **Megoldáskezelőben**, bontsa ki a **Global.asax** duplán **Global.asax.cs** tooopen azt.
   
    ![Global.asax.cs][cache-global-asax]
6. Adja hozzá a következő két hello `using` hello fájlt az egyéb hello hello tetején utasítások `using` utasításokat.

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. Adja hozzá a következő kódsort hello hello végén hello `Application_Start` metódust.

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. A **Solution Explorerben** (Megoldáskezelőben) bontsa ki az `App_Start` elemet, majd kattintson duplán a `RouteConfig.cs` elemre.
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. Cserélje le `controller = "Home"` található a következő kódot a hello hello `RegisterRoutes` metódus `controller = "Teams"` a hello a következő példában látható módon.

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-hello-views"></a>Hello nézetek konfigurálásához
1. A **Megoldáskezelőben**, bontsa ki a hello **nézetek** mappát, majd a hello **megosztott** mappára, majd kattintson duplán **_Layout.cshtml**. 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. Hello hello tartalmának módosítása `title` elem és a név felülírandó `My ASP.NET Application` rendelkező `Contoso Team Stats` a hello a következő példában látható módon.

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. A hello `body` szakaszban, először frissítse a hello `Html.ActionLink` utasítást, és cserélje ki `Application name` a `Contoso Team Stats` , és cserélje le `Home` rendelkező `Teams`.
   
   * Előtte: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
   * Utána: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`
     
     ![Kódmódosítások][cache-layout-cshtml-code]
2. Nyomja le az **Ctrl + F5** toobuild, és futtassa hello alkalmazás. Hello alkalmazás ezen verziójára hello eredmények közvetlenül hello adatbázisából olvassa be. Megjegyzés: hello **hozzon létre új**, **szerkesztése**, **részletek**, és **törlése** műveleteket, amelyek automatikusan hozzáadott toohello alkalmazás hello **MVC 5 Controller nézetek, entitás-keretrendszer használatával** scaffold. Hello hello oktatóprogram következő szakaszában a Redis Cache toooptimize hello adatok eléréséhez, és adja meg a további funkciók toohello alkalmazás fogja hozzáadni.

![Kezdő szintű alkalmazás][cache-starter-application]

## <a name="configure-hello-application-toouse-redis-cache"></a>Hello alkalmazás toouse Redis gyorsítótár konfigurálása
Hello oktatóanyag ezen részében bemutatjuk hello minta alkalmazás toostore konfigurálása, Contoso team statisztika le az Azure Redis Cache példány hello segítségével [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) gyorsítótárügyfél.

* [Hello alkalmazás toouse StackExchange.Redis konfigurálása](#configure-the-application-to-use-stackexchangeredis)
* [Hello TeamsController osztály tooreturn eredményeinek hello gyorsítótár vagy hello adatbázis frissítése](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [Hello létrehozása, szerkesztése, frissítse és módszerek toowork hello gyorsítótárával törlése](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [Hello csapatok Index nézet toowork hello gyorsítótár frissítése](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-hello-application-toouse-stackexchangeredis"></a>Hello alkalmazás toouse StackExchange.Redis konfigurálása
1. tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello StackExchange.Redis NuGet-csomagot, kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **Eszközök** menü.
2. Futtatási hello következő parancsot a hello `Package Manager Console` ablak.
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    hello NuGet csomag tölti le, és hozzáadja a hello szükséges összeállítási referenciát az ügyfél alkalmazás tooaccess Azure Redis Cache hello StackExchange.Redis gyorsítótár-ügyféllel. Ha inkább toouse hello erős névvel ellátott verziójának `StackExchange.Redis` ügyféloldali kódtár, a telepítés hello `StackExchange.Redis.StrongName` csomag.
3. A **Megoldáskezelőben**, bontsa ki a hello **tartományvezérlők** mappa, és kattintson duplán **TeamsController.cs** tooopen azt.
   
    ![Csoportvezérlő][cache-teamscontroller]
4. Adja hozzá a következő két hello `using` utasítások túl**TeamsController.cs**.

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. Adja hozzá a következő két tulajdonságok toohello hello `TeamsController` osztály.

    ```c#   
    // Redis Connection string info
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
        return ConnectionMultiplexer.Connect(cacheConnection);
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ```

6. Hozzon létre egy fájlt a számítógépen nevű `WebAppPlusCacheAppSecrets.config` és helyezheti el egy olyan helyre, nem kell való bejelentkezésének hello forráskódját, a mintaalkalmazást döntse toocheck valahol legyen. Az ebben a példában hello `AppSettingsSecrets.config` a fájl `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.
   
    Hello szerkesztése `WebAppPlusCacheAppSecrets.config` fájlt, és adja hozzá a tartalom a következő hello. Hello alkalmazás helyi futtatásához ezt az információt akkor használt tooconnect tooyour Azure Redis Cache példányt. Hello oktatóanyag későbbi részében fogja telepíteni az Azure Redis Cache példány, és hello gyorsítótár név és jelszó. Ha nem tervezi toorun hello mintaalkalmazás helyileg kihagyhatja ezt a fájlt hello létrehozását és hello későbbi lépésekben hello fájlt, mert amikor alkalmazást telepít központilag az tooAzure hello hivatkozó hello gyorsítótár kapcsolat adatait kérdezi le hello alkalmazás a beállítás hello Web App és nem az ebben a fájlban. Hello óta `WebAppPlusCacheAppSecrets.config` nincs telepítve az alkalmazással tooAzure, nem kell, kivéve, ha toorun hello alkalmazás helyi fog.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. A **Megoldáskezelőben**, kattintson duplán a **web.config** tooopen azt.
   
    ![Web.config][cache-web-config]
2. Adja hozzá a következő hello `file` toohello attribútum `appSettings` elemet. Ha egy másik fájlnevet vagy a hely, helyettesítse be ezeket az értékeket a hello ők hello példában látható módon.
   
   * Előtte: `<appSettings>`
   * Utána: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`
     
   hello ASP.NET futásidejű egyesíti hello külső hello markup hello a fájl tartalmát hello `<appSettings>` elemet. hello futásidejű figyelmen kívül hagyja hello attribútumot, ha hello megadott fájl nem található. A titkos kulcsokat (hello kapcsolati karakterlánc tooyour gyorsítótár), amelyek nem tartalmazzák a hello alkalmazás forráskódja hello. A webes alkalmazás tooAzure telepítésekor hello `WebAppPlusCacheAppSecrests.config` fájl nem telepíthető (Ez mit). Számos módon toospecify ezeknek a kulcsoknak az Azure-ban, és ebben az oktatóanyagban vannak konfigurálva automatikusan, amikor Ön [kiépítése hello Azure-erőforrások](#provision-the-azure-resources) oktatóanyag egy későbbi lépésben. A titkos kulcsok Azure használatáról további információk: [gyakorlati tanácsok a jelszavak és egyéb bizalmas adatok tooASP.NET és Azure App Service üzembe helyezésének](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).

### <a name="update-hello-teamscontroller-class-tooreturn-results-from-hello-cache-or-hello-database"></a>Hello TeamsController osztály tooreturn eredményeinek hello gyorsítótár vagy hello adatbázis frissítése
Ez a példa team statisztika lekérhető hello adatbázisból vagy hello gyorsítótárból. Team statisztika, egy szerializált hello-gyorsítótárában vannak tárolva `List<Team>`, és is készletként rendezett Redis-adattípusok használatával. Rendezett készletből történő lekérdezéskor egyes, az összes vagy bizonyos feltételnek megfelelő elemek lekérésére van lehetőség. Ez a példa hello felső 5 csoportjai wins száma szerinti sorrendben rendezve hello beállítása lesz lekérdezése.

> [!NOTE]
> Már nem szükséges toostore hello team statisztika hello rendelés toouse Azure Redis Cache-gyorsítótár több formátumban. Ez az oktatóanyag néhány használja több formátumok toodemonstrate hello különböző módokon és a különböző adattípusú toocache adatokat használhatja.
> 
> 

1. Adja hozzá a következő hello `using` utasítások toohello `TeamsController.cs` hello tetején, az egyéb hello fájl `using` utasításokat.

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. Cserélje le a jelenlegi hello `public ActionResult Index()` hello végrehajtása a következő metódus végrehajtása.

    ```c#
    // GET: Teams
    public ActionResult Index(string actionType, string resultType)
    {
        List<Team> teams = null;

        switch(actionType)
        {
            case "playGames": // Play a new season of games.
                PlayGames();
                break;

            case "clearCache": // Clear hello results from hello cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild hello database with sample data.
                RebuildDB();
                break;
        }

        // Measure hello time it takes tooretrieve hello results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve hello top 5 teams from hello sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from hello cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from hello database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add hello elapsed time of hello operation toohello ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. Adja hozzá a következő három módszer toohello hello `TeamsController` osztály tooimplement hello `playGames`, `clearCache`, és `rebuildDB` művelettípusok hello a Váltás hello előző kódrészletet hozzáadott utasítást.
   
    Hello `PlayGames` metódus hello team statisztika frissíti a játékok szezon szimulál menti hello eredmények toohello adatbázis és törlése hello most már elavult hello gyorsítótár adatait.

    ```c#
    void PlayGames()
    {
        ViewBag.msg += "Updating team statistics. ";
        // Play a "season" of games.
        var teams = from t in db.Teams
                    select t;

        Team.PlayGames(teams);

        db.SaveChanges();

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Hello `RebuildDB` metódus újból inicializálja hello adatbázis hello alapértelmezett készletét, csoportok, a számukra statisztika hoz létre, és törlése hello most már elavult hello gyorsítótár adatait.

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize hello database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    Hello `ClearCachedTeams` metódus hello gyorsítótárból eltávolítja a gyorsítótárazott team statisztikai adatokkal.

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. Adja hozzá a következő négy módszerek toohello hello `TeamsController` osztály tooimplement hello hello team statisztika lekérése hello gyorsítótár és hello adatbázis különféle módjait. Mindkét módszerhez ad vissza egy `List<Team>` hello nézet ezután megjelenik.
   
    Hello `GetFromDB` metódus olvassa be a hello team statisztika hello adatbázisból.
   
    ```c#
    List<Team> GetFromDB()
    {
        ViewBag.msg += "Results read from DB. ";
        var results = from t in db.Teams
            orderby t.Wins descending
            select t; 

        return results.ToList<Team>();
    }
    ```

    Hello `GetFromList` metódus hello team statisztika beolvassa a gyorsítótárból, mint egy szerializált `List<Team>`. Gyorsítótár-tévesztései esetén hello team statisztika olvasni hello adatbázisból, és aztán hello gyorsítótár a következő alkalommal. Ez a példa használunk JSON.NET szerializálási tooserialize hello .NET objektumok tooand hello gyorsítótárból. További információkért lásd: [hogyan toowork a .NET-objektumokat az Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).

    ```c#
    List<Team> GetFromList()
    {
        List<Team> teams = null;

        IDatabase cache = Connection.GetDatabase();
        string serializedTeams = cache.StringGet("teamsList");
        if (!String.IsNullOrEmpty(serializedTeams))
        {
            teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

            ViewBag.msg += "List read from cache. ";
        }
        else
        {
            ViewBag.msg += "Teams list cache miss. ";
            // Get from database and store in cache
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    Hello `GetFromSortedSet` metódus egy gyorsítótárazott rendezett készletből hello team statisztika olvassa be. Ha a gyorsítótár-tévesztései, hello team statisztika hello adatbázisból beolvasása és rendezett készletként hello gyorsítótárba.

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
        if (teamsSortedSet.Count() > 0)
        {
            ViewBag.msg += "Reading sorted set from cache. ";
            teams = new List<Team>();
            foreach (var t in teamsSortedSet)
            {
                Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                teams.Add(tt);
            }
        }
        else
        {
            ViewBag.msg += "Teams sorted set cache miss. ";

            // Read from DB
            teams = GetFromDB();

            ViewBag.msg += "Storing results toocache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding toosorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    Hello `GetFromSortedSetTop5` metódus hello felső 5 csapat a gyorsítótárazott hello rendezve set olvassa be. Hello gyorsítótárában keresi az hello hello meglétének ellenőrzésével kezdődik `teamsSortedSet` kulcs. Ha ez a kulcs nem található, hello `GetFromSortedSet` metódus tooread hello team statisztika nevezik, és a hello gyorsítótárban tárolja őket. A következő hello gyorsítótárazott rendezett állítsa le kell kérdezni hello felső 5 csoportjai visszaküldött.

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If hello key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load hello entire sorted set into hello cache.
            GetFromSortedSet();

            // Retrieve hello top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get hello top 5 teams from hello sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-hello-create-edit-and-delete-methods-toowork-with-hello-cache"></a>Hello létrehozása, szerkesztése, frissítse és módszerek toowork hello gyorsítótárával törlése
hello állványok kód lett létrehozva, mivel ez a minta része belefoglalja módszerek tooadd, szerkeszteni és törölni. Bármikor csoport hozzáadva, szerkesztésének vagy eltávolításának, hello adatok hello gyorsítótárában elavult válik. Ebben a szakaszban módosítania kell ezen három módszer tooclear hello csoportok gyorsítótárazza, így hello gyorsítótár nincs szinkronban a hello adatbázis nem lesz.

1. Keresse meg a toohello `Create(Team team)` metódus a hello `TeamsController` osztály. Adja hozzá a hívás toohello `ClearCachedTeams` módszer, ahogy az alábbi példa hello.

    ```c#
    // POST: Teams/Create
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. Keresse meg a toohello `Edit(Team team)` metódus a hello `TeamsController` osztály. Adja hozzá a hívás toohello `ClearCachedTeams` módszer, ahogy az alábbi példa hello.

    ```c#
    // POST: Teams/Edit/5
    // tooprotect from overposting attacks, please enable hello specific properties you want toobind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, hello cache is out of date.
            // Clear hello cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. Keresse meg a toohello `DeleteConfirmed(int id)` metódus a hello `TeamsController` osztály. Adja hozzá a hívás toohello `ClearCachedTeams` módszer, ahogy az alábbi példa hello.

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, hello cache is out of date.
        // Clear hello cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-hello-teams-index-view-toowork-with-hello-cache"></a>Hello csapatok Index nézet toowork hello gyorsítótár frissítése
1. A **Megoldáskezelőben**, bontsa ki a hello **nézetek** mappát, majd hello **csapatok** mappára, majd kattintson duplán **Index.cshtml**.
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. Tetején hello hello fájlt keresse meg a következő bekezdésszöveg elem hello.
   
    ![Művelettábla][cache-teams-index-table]
   
    Ez a hello hivatkozás toocreate egy új csoport. Cserélje le a következő táblázat hello hello bekezdésszöveg elem. Ez a táblázat egy-egy új szezon játékok, ha hello gyorsítótár kiürítése játszott új csoport létrehozása művelet mutató hivatkozásokat tartalmaz, hello csapatok lekérése hello gyorsítótár számos formátumban, hello csapatok lekérése hello adatbázis, majd újraépítése hello adatbázist friss mintaadatokkal.

    ```html
    <table class="table">
        <tr>
            <td>
                @Html.ActionLink("Create New", "Create")
            </td>
            <td>
                @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
            </td>
            <td>
                @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
            </td>
            <td>
                @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
            </td>
            <td>
                @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
            </td>
            <td>
                @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
            </td>
            <td>
                @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
            </td>
            <td>
                @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
            </td>
        </tr>    
    </table>
    ```


1. Görgessen toohello alsó részén hello **Index.cshtml** fájlt, és adja hozzá a következő hello `tr` elem úgy, hogy az utolsó sort hello hello utolsó tábla hello fájlban.
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    A sor hello értékét jeleníti meg `ViewBag.Msg` hello aktuális műveletekre vonatkozó jelentés, amely tartalmazza. Hello `ViewBag.Msg` hello előző lépésben hello művelet hivatkozásokra kattintva van beállítva.   
   
    ![Állapotüzenet][cache-status-message]
2. Nyomja le az **F6** toobuild hello projekt.

## <a name="provision-hello-azure-resources"></a>Kiépítés hello Azure-erőforrások
toohost az alkalmazás az Azure-ban, akkor először engedélyeznie kell az alkalmazás által használt Azure szolgáltatással hello. Ebben az oktatóanyagban hello mintaalkalmazás hello Azure-szolgáltatások a következő használ.

* Azure Redis Cache
* App Service webalkalmazás
* SQL Database

toodeploy ezen szolgáltatások tooa új vagy meglévő erőforráscsoport az Ön által választott, kattintson a következő hello **tooAzure telepítése** gombra.

[! [TooAzure központi telepítés] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Ez **tooAzure telepítése** gomb használja hello [hozzon létre egy webalkalmazást és Redis Cache mellett egy SQL Databaset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure gyors üzembe helyezési](https://github.com/Azure/azure-quickstart-templates) sablon tooprovision ezen szolgáltatások és a set hello hello SQL-adatbázis és hello Alkalmazásbeállítás hello Azure Redis Cache kapcsolati karakterlánc a kapcsolati karakterláncot.

> [!NOTE]
> Ha nincs Azure-fiókja, néhány perc alatt [létrehozhat egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).
> 
> 

Gombra kattintva hello **tooAzure telepítése** gomb viszi toohello Azure-portálon, és kezdeményezi hello hello sablon által ismertetett hello erőforrásokat létrehozásának folyamatán.

![TooAzure telepítése][cache-deploy-to-azure-step-1]

1. A hello **alapjai** szakaszt, válassza ki az Azure-előfizetés toouse hello, és válasszon ki egy meglévő erőforráscsoportot vagy hozzon létre egy újat, és adja meg az erőforráscsoport helye hello.
2. A hello **beállítások** területén adja meg egy **rendszergazda bejelentkezési** (ne használjon **admin**), **rendszergazda bejelentkezési jelszó**, és  **Adatbázis neve**. hello más paramétereket van állítva egy ingyenes App Service üzemeltetési terv és az alacsonyabb költségű beállítások hello SQL Database és Azure Redis Cache, amelyek nem rendelkeznek egy ingyenes szint.

    ![TooAzure telepítése][cache-deploy-to-azure-step-2]

3. Szükségeskonfiguráció-hello beállításainak konfigurálása után görgessen hello lap, olvasási hello feltételek és kikötések toohello végét, és ellenőrizze a hello **toohello feltételek és kikötések fenti elfogadom** jelölőnégyzetet.
4. üzembe helyezési hello erőforrások toobegin, kattintson a **beszerzési**.

a telepítés előrehaladását tooview hello hello értesítés ikonra, majd kattintson **telepítése megkezdődött**.

![Központi telepítés elindítva][cache-deployment-started]

A központi telepítés hello állapotát megtekintheti a hello **Microsoft.Template** panelen.

![TooAzure telepítése][cache-deploy-to-azure-step-3]

Ha kiépítése befejeződött, a Visual Studio alkalmazás tooAzure tehető közzé.

> [!NOTE]
> Bármely hello kiépítési folyamat során előforduló hibákat hello megjelenő **Microsoft.Template** panelen. Gyakori hiba például az előfizetésenkénti túl sok SQL Server-példány és a túl sok Ingyenes App Service-futtatási csomag. Javítsa ki a hibákat, és indítsa újra a hello folyamat kattintva **újratelepíteni** a hello **Microsoft.Template** panel vagy hello **tooAzure telepítése** ebben az oktatóanyagban gombra.
> 
> 

## <a name="publish-hello-application-tooazure"></a>Hello alkalmazás tooAzure közzététele
Ebben a lépésben hello oktatóanyag lesz hello alkalmazás tooAzure közzététele, majd futtassa azt hello felhő.

1. Kattintson a jobb gombbal hello **ContosoTeamStats** a Visual Studio projekt, és válassza a **közzététel**.
   
    ![Közzététel][cache-publish-app]
2. Kattintson a **Microsoft Azure App Service** lehetőségre, válassza a **Meglévő kiválasztása** elemet, majd kattintson a **Közzététel** gombra.
   
    ![Közzététel][cache-publish-to-app-service]
3. Válassza ki a hello hello erőforrásokat tartalmazó erőforráscsoport létrehozása hello Azure-erőforrások, bontsa ki, majd jelölje be hello szükségeskonfiguráció-webalkalmazás hello előfizetést. Ha hello **tooAzure telepítése** a webes alkalmazás neve kezdődik gomb **webhely** néhány további karakter követ.
   
    ![Webalkalmazás kiválasztása][cache-select-web-app]
4. Kattintson a **OK** toobegin hello közzétételi folyamat. Néhány másodpercen belül hello közzétételi folyamat befejeződik, és egy böngészőben a mintaalkalmazás futtatása hello nincs elindítva. Ha ellenőrzéskor vagy közzététele a DNS a hibaüzenet, és hello létesítésének folyamatát kell használnia a hello hello alkalmazás Azure-erőforrások csak nemrég befejeződött, várjon egy kicsit, és próbálja meg újból.
   
    ![Gyorsítótár hozzáadva][cache-added-to-application]

hello következő táblázat ismerteti a hello mintaalkalmazás minden művelet hivatkozására.

| Műveletek | Leírás |
| --- | --- |
| Új létrehozása |Létrehoz egy új csapatot. |
| Szezon végigjátszása |A játékok, frissítés hello team statisztikák szezon lejátszása, és törölje minden elavult hello gyorsítótár csoport adatait. |
| Gyorsítótár ürítése |Törölje a jelet hello team statisztikák hello gyorsítótárból. |
| Lista a gyorsítótárból |Hello team statisztikák lekérése a hello gyorsítótárból. Ha a gyorsítótár-tévesztései, hello statisztikák betöltése hello adatbázisból, és menthet toohello gyorsítótár legközelebb. |
| Rendezett készlet a gyorsítótárból |Hello team statisztikák lekérése hello gyorsítótár rendezett készletből. Ha a gyorsítótár-tévesztései, hello statisztikák betöltése hello adatbázisból, és rendezett készletből toohello gyorsítótár mentése. |
| Az 5 legjobb csapat a gyorsítótárból |Hello felső 5 csapatok lekérése hello gyorsítótár rendezett készletből. Ha a gyorsítótár-tévesztései, hello statisztikák betöltése hello adatbázisból, és rendezett készletből toohello gyorsítótár mentése. |
| Betöltés adatbázisból |Hello team statisztikák lekérése hello adatbázis. |
| Adatbázis újraépítése |Hello adatbázis újraépítése, és töltse be újra a csapat mintaadatokkal. |
| Szerkesztés / Részletek / Törlés |Szerkeszthet egy csapatot, megtekintheti annak részletes adatait, törölhet egy csapatot. |

Kattintson a hello műveletek némelyike, és kísérletezzen hello különböző forrásokból származó hello adatok beolvasása. Nem hello különbségeit hello időt toocomplete hello hello adatok lekérése hello adatbázis és hello gyorsítótár különféle módjait.

## <a name="delete-hello-resources-when-you-are-finished-with-hello-application"></a>Törli a hello erőforrást, amikor elkészült, hello alkalmazással
Ha elkészült, hello oktatóanyag mintaalkalmazást, törölheti a hello Azure rendelés tooconserve költségeket, a használt erőforrások és erőforrásokat. Hello használatakor **tooAzure telepítése** hello gombjára [rendelkezés hello Azure-erőforrások](#provision-the-azure-resources) szakasz és az összes erőforrást tartalmaznak hello ugyanabban az erőforráscsoportban, törölheti azokat együtt egy a művelet hello erőforráscsoport törlésével.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) kattintson **erőforráscsoportok**.
2. Az erőforráscsoport a hello hello nevét **elemek szűrése...**  szövegmező.
3. Kattintson a **...**  toohello sarkában található az erőforráscsoport.
4. Kattintson a **Törlés** gombra.
   
    ![Törlés][cache-delete-resource-group]
5. Hello nevét a erőforráscsoportban, és kattintson **törlése**.
   
    ![Törlés megerősítése][cache-delete-confirm]

Néhány perc múlva hello erőforrás után csoport és a benne lévő erőforrásokat törlődnek.

> [!IMPORTANT]
> Vegye figyelembe, hogy egy erőforráscsoport törlése nem vonható vissza, és hogy hello erőforráscsoport és az összes hello-erőforrásokat véglegesen törlődnek. Győződjön meg arról, hogy nem véletlenül törli hello megfelelő erőforráscsoport és erőforrások. Ha ez a minta egy meglévő erőforráscsoportot belül üzemeltetéséhez hello erőforrások hozott létre, törölheti az egyes erőforrások egyenként a megfelelő panelt a.
> 
> 

## <a name="run-hello-sample-application-on-your-local-machine"></a>A helyi gépen hello mintaalkalmazás futtatása
az Azure Redis Cache szükséges toorun hello alkalmazás helyben a számítógépre, mely toocache a példány adatait. 

* Miután közzétette az alkalmazást tooAzure hello előző szakaszban leírtak szerint, ha e lépés során lett kiépítve hello Azure Redis Cache példány is használhatja.
* Ha egy másik meglévő Azure Redis Cache példányt, használhatja a toorun Ez a minta helyileg.
* Azure Redis Cache példány toocreate van szüksége, ha a hello lépések végrehajtásával [gyorsítótár létrehozásához](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Miután kiválasztása vagy létrehozása hello gyorsítótár toouse, keresse meg a gyorsítótár toohello hello Azure-portálon, és hello beolvasása [állomásnév](cache-configure.md#properties) és [hívóbetűk](cache-configure.md#access-keys) a gyorsítótárhoz. Útmutatásért lásd: [A Redis Cache-gyorsítótár beállításai](cache-configure.md#configure-redis-cache-settings).

1. Nyissa meg hello `WebAppPlusCacheAppSecrets.config` hello során létrehozott fájl [hello alkalmazás toouse Redis gyorsítótár konfigurálása](#configure-the-application-to-use-redis-cache) . lépését Ez az oktatóanyag a választott szerkesztővel hello.
2. Hello szerkesztése `value` attribútumot, és cserélje le `MyCache.redis.cache.windows.net` a hello [állomásnév](cache-configure.md#properties) a gyorsítótár, és adja meg vagy hello [elsődleges vagy másodlagos kulcsot](cache-configure.md#access-keys) a gyorsítótár hello jelszóként.

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. Nyomja le az **Ctrl + F5** toorun hello alkalmazás.

> [!NOTE]
> Vegye figyelembe, hogy mivel hello alkalmazás, többek között a hello adatbázis helyben fut, és hello Redis gyorsítótárat üzemeltetni kívánja az Azure cache hello jelenhetnek meg toounder-hello adatbázis végrehajtani. A legjobb teljesítmény érdekében hello ügyfélalkalmazást és az Azure Redis Cache példány belül hello ugyanazon a helyen. 
> 
> 

## <a name="next-steps"></a>Következő lépések
* További információ [első ASP.NET MVC 5 használatába](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) a hello [ASP.NET](http://asp.net/) hely.
* ASP.NET webalkalmazás létrehozása az App Service további példákért lásd [létrehozása és telepítése az Azure App Service ASP.NET webalkalmazás](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) a hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).
  * Tekintse meg a hello HealthClinic.biz bemutató további quickstarts [Azure fejlesztői eszközök Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).
* Tudjon meg többet a hello [kód első tooa új adatbázis](https://msdn.microsoft.com/data/jj193542) közelítse tooEntity ebben az oktatóanyagban használt keretrendszer.
* További információ [az Azure App Service webalkalmazásairól](../app-service-web/app-service-web-overview.md).
* Ismerje meg, hogyan túl[figyelő](cache-how-to-monitor.md) saját gyorsítótárához az hello Azure-portálon.
* Az Azure Redis Cache prémium funkcióinak megismerése
  
  * [Hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache](cache-how-to-premium-persistence.md)
  * [Hogyan fürtözése a Premium Azure Redis Cache tooconfigure](cache-how-to-premium-clustering.md)
  * [Hogyan támogatják a virtuális hálózati tooconfigure a Premium Azure Redis Cache](cache-how-to-premium-vnet.md)
  * Lásd: hello [Azure Redis Cache – gyakori kérdések](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) méretét, az átviteli sebesség és a sávszélesség a prémium szintű gyorsítótárak kapcsolatos további részletekért.

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png

