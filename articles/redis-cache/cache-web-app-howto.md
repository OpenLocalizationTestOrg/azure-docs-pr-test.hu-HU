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
# <a name="how-toocreate-a-web-app-with-redis-cache"></a><span data-ttu-id="2eee6-103">Hogyan toocreate a webes alkalmazás a Redis Cache segítségével</span><span class="sxs-lookup"><span data-stu-id="2eee6-103">How toocreate a Web App with Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2eee6-104">.NET</span><span class="sxs-lookup"><span data-stu-id="2eee6-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="2eee6-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2eee6-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="2eee6-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="2eee6-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="2eee6-107">Java</span><span class="sxs-lookup"><span data-stu-id="2eee6-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="2eee6-108">Python</span><span class="sxs-lookup"><span data-stu-id="2eee6-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="2eee6-109">Ez az oktatóanyag bemutatja, hogyan toocreate és egy ASP.NET alkalmazás tooa webes webalkalmazás telepítése az Azure App Service segítségével a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="2eee6-109">This tutorial shows how toocreate and deploy an ASP.NET web application tooa web app in Azure App Service using Visual Studio 2017.</span></span> <span data-ttu-id="2eee6-110">hello mintaalkalmazás adatbázis team statisztikáit listáját jeleníti meg, és különböző módokon toouse Azure Redis Cache toostore jeleníti meg, és hello gyorsítótár adatainak lekérése.</span><span class="sxs-lookup"><span data-stu-id="2eee6-110">hello sample application displays a list of team statistics from a database and shows different ways toouse Azure Redis Cache toostore and retrieve data from hello cache.</span></span> <span data-ttu-id="2eee6-111">Hello oktatóanyag befejezésekor kell, hogy beolvassa és tooa adatbázis, az Azure Redis Cache optimalizált, fut, és az Azure-ban futó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2eee6-111">When you complete hello tutorial you'll have a running web app that reads and writes tooa database, optimized with Azure Redis Cache, and hosted in Azure.</span></span>

<span data-ttu-id="2eee6-112">Az oktatóanyagból a következőket sajátíthatja el:</span><span class="sxs-lookup"><span data-stu-id="2eee6-112">You'll learn:</span></span>

* <span data-ttu-id="2eee6-113">Hogyan toocreate egy ASP.NET MVC 5 webalkalmazást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="2eee6-113">How toocreate an ASP.NET MVC 5 web application in Visual Studio.</span></span>
* <span data-ttu-id="2eee6-114">Hogyan tooaccess-adatbázisból egy entitás-keretrendszer használatával.</span><span class="sxs-lookup"><span data-stu-id="2eee6-114">How tooaccess data from a database using Entity Framework.</span></span>
* <span data-ttu-id="2eee6-115">Hogyan tooimprove adatátvitelt, ami csökkenti az adatbázis betöltési tárolja és használja az Azure Redis Cache-adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2eee6-115">How tooimprove data throughput and reduce database load by storing and retrieving data using Azure Redis Cache.</span></span>
* <span data-ttu-id="2eee6-116">Egy Redis toouse rendezését set tooretrieve hello felső 5 csoportok.</span><span class="sxs-lookup"><span data-stu-id="2eee6-116">How toouse a Redis sorted set tooretrieve hello top 5 teams.</span></span>
* <span data-ttu-id="2eee6-117">Hogyan tooprovision hello Azure-erőforrások hello alkalmazás Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="2eee6-117">How tooprovision hello Azure resources for hello application using a Resource Manager template.</span></span>
* <span data-ttu-id="2eee6-118">Hogyan toopublish hello alkalmazás tooAzure Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="2eee6-118">How toopublish hello application tooAzure using Visual Studio.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2eee6-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2eee6-119">Prerequisites</span></span>
<span data-ttu-id="2eee6-120">toocomplete hello útmutató, rendelkeznie kell a következő előfeltételek hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-120">toocomplete hello tutorial, you must have hello following prerequisites.</span></span>

* [<span data-ttu-id="2eee6-121">Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="2eee6-121">Azure account</span></span>](#azure-account)
* [<span data-ttu-id="2eee6-122">Az Azure SDK for .NET hello Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2eee6-122">Visual Studio 2017 with hello Azure SDK for .NET</span></span>](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a><span data-ttu-id="2eee6-123">Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="2eee6-123">Azure account</span></span>
<span data-ttu-id="2eee6-124">Egy Azure-fiók toocomplete hello oktatóanyag van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2eee6-124">You need an Azure account toocomplete hello tutorial.</span></span> <span data-ttu-id="2eee6-125">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="2eee6-125">You can:</span></span>

* <span data-ttu-id="2eee6-126">[Nyisson egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="2eee6-126">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="2eee6-127">Amelyek lehetnek kimenő használt tootry fizetős Azure-szolgáltatások jóváírásokat kap.</span><span class="sxs-lookup"><span data-stu-id="2eee6-127">You get credits that can be used tootry out paid Azure services.</span></span> <span data-ttu-id="2eee6-128">Hello jóváírásokat el is használta, után is megtarthatja hello fiókot, és ingyenes Azure-szolgáltatások és funkciók használatára.</span><span class="sxs-lookup"><span data-stu-id="2eee6-128">Even after hello credits are used up, you can keep hello account and use free Azure services and features.</span></span>
* <span data-ttu-id="2eee6-129">[Aktiválja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="2eee6-129">[Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="2eee6-130">Az MSDN-előfizetés minden hónapban biztosít Önnek krediteket, amelyekkel fizetős Azure-szolgáltatásokat használhat.</span><span class="sxs-lookup"><span data-stu-id="2eee6-130">Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

### <a name="visual-studio-2017-with-hello-azure-sdk-for-net"></a><span data-ttu-id="2eee6-131">Az Azure SDK for .NET hello Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="2eee6-131">Visual Studio 2017 with hello Azure SDK for .NET</span></span>
<span data-ttu-id="2eee6-132">hello az oktatóanyag a Visual Studio 2017 számára íródott hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span><span class="sxs-lookup"><span data-stu-id="2eee6-132">hello tutorial is written for Visual Studio 2017 with hello [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span></span> <span data-ttu-id="2eee6-133">hello Azure SDK 2.9.5 hello Visual Studio telepítő részét képezi.</span><span class="sxs-lookup"><span data-stu-id="2eee6-133">hello Azure SDK 2.9.5 is included with hello Visual Studio installer.</span></span>

<span data-ttu-id="2eee6-134">Ha Visual Studio 2015-öt, amelyeket követve hello hello oktatóanyag [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2eee6-134">If you have Visual Studio 2015, you can follow hello tutorial with hello [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 or later.</span></span> <span data-ttu-id="2eee6-135">[Letöltési hello legfrissebb Azure SDK-t a Visual Studio 2015 Itt](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="2eee6-135">[Download hello latest Azure SDK for Visual Studio 2015 here](http://go.microsoft.com/fwlink/?linkid=518003).</span></span> <span data-ttu-id="2eee6-136">Ha már nincs a Visual Studio automatikusan telepítve hello SDK.</span><span class="sxs-lookup"><span data-stu-id="2eee6-136">Visual Studio is automatically installed with hello SDK if you don't already have it.</span></span> <span data-ttu-id="2eee6-137">Néhány képernyő megjelenése hello ábrán látható módon az oktatóanyag eltérhet.</span><span class="sxs-lookup"><span data-stu-id="2eee6-137">Some screens may look different from hello illustrations shown in this tutorial.</span></span>

<span data-ttu-id="2eee6-138">Ha a Visual Studio 2013 van, akkor [letöltési hello legfrissebb Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span><span class="sxs-lookup"><span data-stu-id="2eee6-138">If you have Visual Studio 2013, you can [download hello latest Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span></span> <span data-ttu-id="2eee6-139">Néhány képernyő megjelenése hello ábrán látható módon az oktatóanyag eltérhet.</span><span class="sxs-lookup"><span data-stu-id="2eee6-139">Some screens may look different from hello illustrations shown in this tutorial.</span></span>

## <a name="create-hello-visual-studio-project"></a><span data-ttu-id="2eee6-140">Hello Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2eee6-140">Create hello Visual Studio project</span></span>
1. <span data-ttu-id="2eee6-141">Nyissa meg a Visual Studio alkalmazást, majd kattintson a **File** (File), **New** (Új), **Project** (Projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2eee6-141">Open Visual Studio and click **File**, **New**, **Project**.</span></span>
2. <span data-ttu-id="2eee6-142">Bontsa ki a hello **Visual C#** hello csomópontja **sablonok** listáról válassza ki **felhő**, és kattintson a **ASP.NET Web Application**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-142">Expand hello **Visual C#** node in hello **Templates** list, select **Cloud**, and click **ASP.NET Web Application**.</span></span> <span data-ttu-id="2eee6-143">Győződjön meg arról, hogy a **.NET Framework 4.5.2** vagy újabb keretrendszer van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="2eee6-143">Ensure that **.NET Framework 4.5.2** or higher is selected.</span></span>  <span data-ttu-id="2eee6-144">Típus **ContosoTeamStats** történő hello **neve** szövegmezőben kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-144">Type **ContosoTeamStats** into hello **Name** textbox and click **OK**.</span></span>
   
    ![Projekt létrehozása][cache-create-project]
3. <span data-ttu-id="2eee6-146">Válassza ki **MVC** hello projekt típusként.</span><span class="sxs-lookup"><span data-stu-id="2eee6-146">Select **MVC** as hello project type.</span></span> 

    <span data-ttu-id="2eee6-147">Győződjön meg arról, hogy **nem hitelesítési** hello megadott **hitelesítési** beállításait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-147">Ensure that **No Authentication** is specified for hello **Authentication** settings.</span></span> <span data-ttu-id="2eee6-148">Attól függően, hogy a Visual Studio verziójának hello alapértelmezett számbavételekhez állítható be más toosomething.</span><span class="sxs-lookup"><span data-stu-id="2eee6-148">Depending on your version of Visual Studio, hello default may be set toosomething else.</span></span> <span data-ttu-id="2eee6-149">toochange, kattintson a **hitelesítés módosítása** válassza **nem hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-149">toochange it, click **Change Authentication** and select **No Authentication**.</span></span>

    <span data-ttu-id="2eee6-150">Ha a Visual Studio 2015 együtt, törölje a jelet hello **hello felhőben lévő gazdagéphez** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="2eee6-150">If you are following along with Visual Studio 2015, clear hello **Host in hello cloud** checkbox.</span></span> <span data-ttu-id="2eee6-151">Programra [rendelkezés hello Azure-erőforrások](#provision-the-azure-resources) és [hello alkalmazás tooAzure közzététele](#publish-the-application-to-azure) a későbbi lépésekben hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2eee6-151">You'll [provision hello Azure resources](#provision-the-azure-resources) and [publish hello application tooAzure](#publish-the-application-to-azure) in subsequent steps in hello tutorial.</span></span> <span data-ttu-id="2eee6-152">Példa egy App Service webalkalmazásba a Visual Studio eszközből kiépítés távozó **hello felhőben lévő gazdagéphez** be van jelölve, lásd: [Ismerkedés a webalkalmazásokkal az Azure App Service szolgáltatásban, az ASP.NET és a Visual Studio használatával](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="2eee6-152">For an example of provisioning an App Service web app from Visual Studio by leaving **Host in hello cloud** checked, see [Get started with Web Apps in Azure App Service, using ASP.NET and Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
    ![Projektsablon kiválasztása][cache-select-template]
4. <span data-ttu-id="2eee6-154">Kattintson a **OK** toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-154">Click **OK** toocreate hello project.</span></span>

## <a name="create-hello-aspnet-mvc-application"></a><span data-ttu-id="2eee6-155">Hello ASP.NET MVC alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2eee6-155">Create hello ASP.NET MVC application</span></span>
<span data-ttu-id="2eee6-156">Ebben a szakaszban hello oktatóanyag létre fog hozni hello alapvető alkalmazás, amely olvas, és az adatbázis csoport statisztikáit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2eee6-156">In this section of hello tutorial, you'll create hello basic application that reads and displays team statistics from a database.</span></span>

* [<span data-ttu-id="2eee6-157">Hello Entity Framework NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2eee6-157">Add hello Entity Framework NuGet package</span></span>](#add-the-entity-framework-nuget-package)
* [<span data-ttu-id="2eee6-158">Hello modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2eee6-158">Add hello model</span></span>](#add-the-model)
* [<span data-ttu-id="2eee6-159">Hello vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2eee6-159">Add hello controller</span></span>](#add-the-controller)
* [<span data-ttu-id="2eee6-160">Hello nézetek konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="2eee6-160">Configure hello views</span></span>](#configure-the-views)

### <a name="add-hello-entity-framework-nuget-package"></a><span data-ttu-id="2eee6-161">Hello Entity Framework NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2eee6-161">Add hello Entity Framework NuGet package</span></span>

1. <span data-ttu-id="2eee6-162">Kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **eszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="2eee6-162">Click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>
2. <span data-ttu-id="2eee6-163">Futtatási hello következő parancsot a hello **Csomagkezelő konzol** ablak.</span><span class="sxs-lookup"><span data-stu-id="2eee6-163">Run hello following command from hello **Package Manager Console** window.</span></span>
    
    ```
    Install-Package EntityFramework
    ```

<span data-ttu-id="2eee6-164">Ezzel a csomaggal kapcsolatos további információkért lásd: hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet lap.</span><span class="sxs-lookup"><span data-stu-id="2eee6-164">For more information about this package, see hello [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet page.</span></span>

### <a name="add-hello-model"></a><span data-ttu-id="2eee6-165">Hello modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2eee6-165">Add hello model</span></span>
1. <span data-ttu-id="2eee6-166">Kattintson a jobb gombbal a **Models** (Modellek) elemre a **Solution Explorer** (Megoldáskezelő) területén, és válassza az **Add** (Hozzáadás), **Class** (Osztály) lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="2eee6-166">Right-click **Models** in **Solution Explorer**, and choose **Add**, **Class**.</span></span> 
   
    ![Modell hozzáadása][cache-model-add-class]
2. <span data-ttu-id="2eee6-168">Adja meg `Team` hello osztály nevét, majd kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-168">Enter `Team` for hello class name and click **Add**.</span></span>
   
    ![Modellosztály hozzáadása][cache-model-add-class-dialog]
3. <span data-ttu-id="2eee6-170">Cserélje le a hello `using` hello hello tetején utasítások `Team.cs` hello következőre fájl `using` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2eee6-170">Replace hello `using` statements at hello top of hello `Team.cs` file with hello following `using` statements.</span></span>

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. <span data-ttu-id="2eee6-171">Cserélje le a hello hello definíciója `Team` a következő kódrészletet, amely tartalmaz egy frissített hello osztályt `Team` definícióját, valamint néhány más Entity Framework segítőosztályok osztályban.</span><span class="sxs-lookup"><span data-stu-id="2eee6-171">Replace hello definition of hello `Team` class with hello following code snippet that contains an updated `Team` class definition as well as some other Entity Framework helper classes.</span></span> <span data-ttu-id="2eee6-172">Hello kód első megközelítés tooEntity keretrendszer, amely ebben az oktatóanyagban használt további információkért lásd: [kód első tooa új adatbázis](https://msdn.microsoft.com/data/jj193542).</span><span class="sxs-lookup"><span data-stu-id="2eee6-172">For more information on hello code first approach tooEntity Framework that's used in this tutorial, see [Code first tooa new database](https://msdn.microsoft.com/data/jj193542).</span></span>

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


1. <span data-ttu-id="2eee6-173">A **Megoldáskezelőben**, kattintson duplán a **web.config** tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-173">In **Solution Explorer**, double-click **web.config** tooopen it.</span></span>
   
    ![Web.config][cache-web-config]
2. <span data-ttu-id="2eee6-175">Adja hozzá a következő hello `connectionStrings` szakasz.</span><span class="sxs-lookup"><span data-stu-id="2eee6-175">Add hello following `connectionStrings` section.</span></span> <span data-ttu-id="2eee6-176">hello hello kapcsolati karakterlánc nevét meg kell egyeznie a hello Entity Framework adatbázis környezeti osztályt, amely van hello neve `TeamContext`.</span><span class="sxs-lookup"><span data-stu-id="2eee6-176">hello name of hello connection string must match hello name of hello Entity Framework database context class which is `TeamContext`.</span></span>

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="2eee6-177">Hozzáadhat új hello `connectionStrings` , hogy azt a következő szakasz `configSections`, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-177">You can add hello new `connectionStrings` section so that it follows `configSections`, as shown in hello following example.</span></span>

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
    > <span data-ttu-id="2eee6-178">A kapcsolati karakterláncot a Visual Studio hello verziójától függően eltérő lehet, és az SQL Server Express edition használt toocomplete hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="2eee6-178">Your connection string may be different depending on hello version of Visual Studio and SQL Server Express edition used toocomplete hello tutorial.</span></span> <span data-ttu-id="2eee6-179">hello web.config sablon kell konfigurált toomatch a telepítést, és tartalmazhat `Data Source` bejegyzéseket, például `(LocalDB)\v11.0` (az SQL Server Express 2012-ben) vagy `Data Source=(LocalDB)\MSSQLLocalDB` (az SQL Server Express 2014 és újabb).</span><span class="sxs-lookup"><span data-stu-id="2eee6-179">hello web.config template should be configured toomatch your installation, and may contain `Data Source` entries like `(LocalDB)\v11.0` (from SQL Server Express 2012) or `Data Source=(LocalDB)\MSSQLLocalDB` (from SQL Server Express 2014 and newer).</span></span> <span data-ttu-id="2eee6-180">További információ a kapcsolati karakterláncokról és az SQL Express-verziókról: [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span><span class="sxs-lookup"><span data-stu-id="2eee6-180">For more information about connection strings and SQL Express versions, see [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span></span>

### <a name="add-hello-controller"></a><span data-ttu-id="2eee6-181">Hello vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2eee6-181">Add hello controller</span></span>
1. <span data-ttu-id="2eee6-182">Nyomja le az **F6** toobuild hello projekt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-182">Press **F6** toobuild hello project.</span></span> 
2. <span data-ttu-id="2eee6-183">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **tartományvezérlők** mappa, és válassza **Hozzáadás**, **vezérlő**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-183">In **Solution Explorer**, right-click hello **Controllers** folder and choose **Add**, **Controller**.</span></span>
   
    ![Vezérlő hozzáadása][cache-add-controller]
3. <span data-ttu-id="2eee6-185">Válassza az **MVC 5 Controller with views, using Entity Framework** (MVC 5 vezérlő nézetekkel, az Entity Framework használatával) lehetőséget, majd kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="2eee6-185">Choose **MVC 5 Controller with views, using Entity Framework**, and click **Add**.</span></span> <span data-ttu-id="2eee6-186">Ha hibaüzenetet kap, miután rákattintott **Hozzáadás**, győződjön meg arról, hogy először rendelkezik beépített hello projekt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-186">If you get an error after clicking **Add**, ensure that you have built hello project first.</span></span>
   
    ![Vezérlőosztály hozzáadása][cache-add-controller-class]
4. <span data-ttu-id="2eee6-188">Válassza ki **Team (ContosoTeamStats.Models)** a hello **Model class** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="2eee6-188">Select **Team (ContosoTeamStats.Models)** from hello **Model class** drop-down list.</span></span> <span data-ttu-id="2eee6-189">Válassza ki **TeamContext (ContosoTeamStats.Models)** a hello **adatok környezetben osztály** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="2eee6-189">Select **TeamContext (ContosoTeamStats.Models)** from hello **Data context class** drop-down list.</span></span> <span data-ttu-id="2eee6-190">Típus `TeamsController` a hello **vezérlő** (Ha nem automatikusan a telepítéskor) szövegmezőben.</span><span class="sxs-lookup"><span data-stu-id="2eee6-190">Type `TeamsController` in hello **Controller** name textbox (if it is not automatically populated).</span></span> <span data-ttu-id="2eee6-191">Kattintson a **Hozzáadás** toocreate hello vezérlő osztályhoz, és adja hozzá a hello alapértelmezett nézeteket.</span><span class="sxs-lookup"><span data-stu-id="2eee6-191">Click **Add** toocreate hello controller class and add hello default views.</span></span>
   
    ![Vezérlő konfigurálása][cache-configure-controller]
5. <span data-ttu-id="2eee6-193">A **Megoldáskezelőben**, bontsa ki a **Global.asax** duplán **Global.asax.cs** tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-193">In **Solution Explorer**, expand **Global.asax** and double-click **Global.asax.cs** tooopen it.</span></span>
   
    ![Global.asax.cs][cache-global-asax]
6. <span data-ttu-id="2eee6-195">Adja hozzá a következő két hello `using` hello fájlt az egyéb hello hello tetején utasítások `using` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2eee6-195">Add hello following two `using` statements at hello top of hello file under hello other `using` statements.</span></span>

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. <span data-ttu-id="2eee6-196">Adja hozzá a következő kódsort hello hello végén hello `Application_Start` metódust.</span><span class="sxs-lookup"><span data-stu-id="2eee6-196">Add hello following line of code at hello end of hello `Application_Start` method.</span></span>

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. <span data-ttu-id="2eee6-197">A **Solution Explorerben** (Megoldáskezelőben) bontsa ki az `App_Start` elemet, majd kattintson duplán a `RouteConfig.cs` elemre.</span><span class="sxs-lookup"><span data-stu-id="2eee6-197">In **Solution Explorer**, expand `App_Start` and double-click `RouteConfig.cs`.</span></span>
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. <span data-ttu-id="2eee6-199">Cserélje le `controller = "Home"` található a következő kódot a hello hello `RegisterRoutes` metódus `controller = "Teams"` a hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="2eee6-199">Replace `controller = "Home"` in hello following code in hello `RegisterRoutes` method with `controller = "Teams"` as shown in hello following example.</span></span>

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-hello-views"></a><span data-ttu-id="2eee6-200">Hello nézetek konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="2eee6-200">Configure hello views</span></span>
1. <span data-ttu-id="2eee6-201">A **Megoldáskezelőben**, bontsa ki a hello **nézetek** mappát, majd a hello **megosztott** mappára, majd kattintson duplán **_Layout.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-201">In **Solution Explorer**, expand hello **Views** folder and then hello **Shared** folder, and double-click **_Layout.cshtml**.</span></span> 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. <span data-ttu-id="2eee6-203">Hello hello tartalmának módosítása `title` elem és a név felülírandó `My ASP.NET Application` rendelkező `Contoso Team Stats` a hello a következő példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="2eee6-203">Change hello contents of hello `title` element and replace `My ASP.NET Application` with `Contoso Team Stats` as shown in hello following example.</span></span>

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. <span data-ttu-id="2eee6-204">A hello `body` szakaszban, először frissítse a hello `Html.ActionLink` utasítást, és cserélje ki `Application name` a `Contoso Team Stats` , és cserélje le `Home` rendelkező `Teams`.</span><span class="sxs-lookup"><span data-stu-id="2eee6-204">In hello `body` section, update hello first `Html.ActionLink` statement and replace `Application name` with `Contoso Team Stats` and replace `Home` with `Teams`.</span></span>
   
   * <span data-ttu-id="2eee6-205">Előtte: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="2eee6-205">Before: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
   * <span data-ttu-id="2eee6-206">Utána: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="2eee6-206">After: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
     
     ![Kódmódosítások][cache-layout-cshtml-code]
2. <span data-ttu-id="2eee6-208">Nyomja le az **Ctrl + F5** toobuild, és futtassa hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2eee6-208">Press **Ctrl+F5** toobuild and run hello application.</span></span> <span data-ttu-id="2eee6-209">Hello alkalmazás ezen verziójára hello eredmények közvetlenül hello adatbázisából olvassa be.</span><span class="sxs-lookup"><span data-stu-id="2eee6-209">This version of hello application reads hello results directly from hello database.</span></span> <span data-ttu-id="2eee6-210">Megjegyzés: hello **hozzon létre új**, **szerkesztése**, **részletek**, és **törlése** műveleteket, amelyek automatikusan hozzáadott toohello alkalmazás hello **MVC 5 Controller nézetek, entitás-keretrendszer használatával** scaffold.</span><span class="sxs-lookup"><span data-stu-id="2eee6-210">Note hello **Create New**, **Edit**, **Details**, and **Delete** actions that were automatically added toohello application by hello **MVC 5 Controller with views, using Entity Framework** scaffold.</span></span> <span data-ttu-id="2eee6-211">Hello hello oktatóprogram következő szakaszában a Redis Cache toooptimize hello adatok eléréséhez, és adja meg a további funkciók toohello alkalmazás fogja hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="2eee6-211">In hello next section of hello tutorial you'll add Redis Cache toooptimize hello data access and provide additional features toohello application.</span></span>

![Kezdő szintű alkalmazás][cache-starter-application]

## <a name="configure-hello-application-toouse-redis-cache"></a><span data-ttu-id="2eee6-213">Hello alkalmazás toouse Redis gyorsítótár konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2eee6-213">Configure hello application toouse Redis Cache</span></span>
<span data-ttu-id="2eee6-214">Hello oktatóanyag ezen részében bemutatjuk hello minta alkalmazás toostore konfigurálása, Contoso team statisztika le az Azure Redis Cache példány hello segítségével [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) gyorsítótárügyfél.</span><span class="sxs-lookup"><span data-stu-id="2eee6-214">In this section of hello tutorial, you'll configure hello sample application toostore and retrieve Contoso team statistics from an Azure Redis Cache instance by using hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cache client.</span></span>

* [<span data-ttu-id="2eee6-215">Hello alkalmazás toouse StackExchange.Redis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2eee6-215">Configure hello application toouse StackExchange.Redis</span></span>](#configure-the-application-to-use-stackexchangeredis)
* [<span data-ttu-id="2eee6-216">Hello TeamsController osztály tooreturn eredményeinek hello gyorsítótár vagy hello adatbázis frissítése</span><span class="sxs-lookup"><span data-stu-id="2eee6-216">Update hello TeamsController class tooreturn results from hello cache or hello database</span></span>](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [<span data-ttu-id="2eee6-217">Hello létrehozása, szerkesztése, frissítse és módszerek toowork hello gyorsítótárával törlése</span><span class="sxs-lookup"><span data-stu-id="2eee6-217">Update hello Create, Edit, and Delete methods toowork with hello cache</span></span>](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [<span data-ttu-id="2eee6-218">Hello csapatok Index nézet toowork hello gyorsítótár frissítése</span><span class="sxs-lookup"><span data-stu-id="2eee6-218">Update hello Teams Index view toowork with hello cache</span></span>](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-hello-application-toouse-stackexchangeredis"></a><span data-ttu-id="2eee6-219">Hello alkalmazás toouse StackExchange.Redis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2eee6-219">Configure hello application toouse StackExchange.Redis</span></span>
1. <span data-ttu-id="2eee6-220">tooconfigure egy ügyfélalkalmazást, a Visual Studio használatával hello StackExchange.Redis NuGet-csomagot, kattintson a **NuGet-Csomagkezelő**, **Csomagkezelő konzol** a hello **Eszközök** menü.</span><span class="sxs-lookup"><span data-stu-id="2eee6-220">tooconfigure a client application in Visual Studio using hello StackExchange.Redis NuGet package, click **NuGet Package Manager**, **Package Manager Console** from hello **Tools** menu.</span></span>
2. <span data-ttu-id="2eee6-221">Futtatási hello következő parancsot a hello `Package Manager Console` ablak.</span><span class="sxs-lookup"><span data-stu-id="2eee6-221">Run hello following command from hello `Package Manager Console` window.</span></span>
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    <span data-ttu-id="2eee6-222">hello NuGet csomag tölti le, és hozzáadja a hello szükséges összeállítási referenciát az ügyfél alkalmazás tooaccess Azure Redis Cache hello StackExchange.Redis gyorsítótár-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="2eee6-222">hello NuGet package downloads and adds hello required assembly references for your client application tooaccess Azure Redis Cache with hello StackExchange.Redis cache client.</span></span> <span data-ttu-id="2eee6-223">Ha inkább toouse hello erős névvel ellátott verziójának `StackExchange.Redis` ügyféloldali kódtár, a telepítés hello `StackExchange.Redis.StrongName` csomag.</span><span class="sxs-lookup"><span data-stu-id="2eee6-223">If you prefer toouse a strong-named version of hello `StackExchange.Redis` client library, install hello `StackExchange.Redis.StrongName` package.</span></span>
3. <span data-ttu-id="2eee6-224">A **Megoldáskezelőben**, bontsa ki a hello **tartományvezérlők** mappa, és kattintson duplán **TeamsController.cs** tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-224">In **Solution Explorer**, expand hello **Controllers** folder and double-click **TeamsController.cs** tooopen it.</span></span>
   
    ![Csoportvezérlő][cache-teamscontroller]
4. <span data-ttu-id="2eee6-226">Adja hozzá a következő két hello `using` utasítások túl**TeamsController.cs**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-226">Add hello following two `using` statements too**TeamsController.cs**.</span></span>

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. <span data-ttu-id="2eee6-227">Adja hozzá a következő két tulajdonságok toohello hello `TeamsController` osztály.</span><span class="sxs-lookup"><span data-stu-id="2eee6-227">Add hello following two properties toohello `TeamsController` class.</span></span>

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

6. <span data-ttu-id="2eee6-228">Hozzon létre egy fájlt a számítógépen nevű `WebAppPlusCacheAppSecrets.config` és helyezheti el egy olyan helyre, nem kell való bejelentkezésének hello forráskódját, a mintaalkalmazást döntse toocheck valahol legyen.</span><span class="sxs-lookup"><span data-stu-id="2eee6-228">Create a file on your computer named `WebAppPlusCacheAppSecrets.config` and place it in a location that won't be checked in with hello source code of your sample application, should you decide toocheck it in somewhere.</span></span> <span data-ttu-id="2eee6-229">Az ebben a példában hello `AppSettingsSecrets.config` a fájl `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span><span class="sxs-lookup"><span data-stu-id="2eee6-229">In this example hello `AppSettingsSecrets.config` file is located at `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span></span>
   
    <span data-ttu-id="2eee6-230">Hello szerkesztése `WebAppPlusCacheAppSecrets.config` fájlt, és adja hozzá a tartalom a következő hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-230">Edit hello `WebAppPlusCacheAppSecrets.config` file and add hello following contents.</span></span> <span data-ttu-id="2eee6-231">Hello alkalmazás helyi futtatásához ezt az információt akkor használt tooconnect tooyour Azure Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-231">If you run hello application locally this information is used tooconnect tooyour Azure Redis Cache instance.</span></span> <span data-ttu-id="2eee6-232">Hello oktatóanyag későbbi részében fogja telepíteni az Azure Redis Cache példány, és hello gyorsítótár név és jelszó.</span><span class="sxs-lookup"><span data-stu-id="2eee6-232">Later in hello tutorial you'll provision an Azure Redis Cache instance and update hello cache name and password.</span></span> <span data-ttu-id="2eee6-233">Ha nem tervezi toorun hello mintaalkalmazás helyileg kihagyhatja ezt a fájlt hello létrehozását és hello későbbi lépésekben hello fájlt, mert amikor alkalmazást telepít központilag az tooAzure hello hivatkozó hello gyorsítótár kapcsolat adatait kérdezi le hello alkalmazás a beállítás hello Web App és nem az ebben a fájlban.</span><span class="sxs-lookup"><span data-stu-id="2eee6-233">If you don't plan toorun hello sample application locally you can skip hello creation of this file and hello subsequent steps that reference hello file, because when you deploy tooAzure hello application retrieves hello cache connection information from hello app setting for hello Web App and not from this file.</span></span> <span data-ttu-id="2eee6-234">Hello óta `WebAppPlusCacheAppSecrets.config` nincs telepítve az alkalmazással tooAzure, nem kell, kivéve, ha toorun hello alkalmazás helyi fog.</span><span class="sxs-lookup"><span data-stu-id="2eee6-234">Since hello `WebAppPlusCacheAppSecrets.config` is not deployed tooAzure with your application, you don't need it unless you are going toorun hello application locally.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="2eee6-235">A **Megoldáskezelőben**, kattintson duplán a **web.config** tooopen azt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-235">In **Solution Explorer**, double-click **web.config** tooopen it.</span></span>
   
    ![Web.config][cache-web-config]
2. <span data-ttu-id="2eee6-237">Adja hozzá a következő hello `file` toohello attribútum `appSettings` elemet.</span><span class="sxs-lookup"><span data-stu-id="2eee6-237">Add hello following `file` attribute toohello `appSettings` element.</span></span> <span data-ttu-id="2eee6-238">Ha egy másik fájlnevet vagy a hely, helyettesítse be ezeket az értékeket a hello ők hello példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="2eee6-238">If you used a different file name or location, substitute those values for hello ones shown in hello example.</span></span>
   
   * <span data-ttu-id="2eee6-239">Előtte: `<appSettings>`</span><span class="sxs-lookup"><span data-stu-id="2eee6-239">Before: `<appSettings>`</span></span>
   * <span data-ttu-id="2eee6-240">Utána: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span><span class="sxs-lookup"><span data-stu-id="2eee6-240">After: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span></span>
     
   <span data-ttu-id="2eee6-241">hello ASP.NET futásidejű egyesíti hello külső hello markup hello a fájl tartalmát hello `<appSettings>` elemet.</span><span class="sxs-lookup"><span data-stu-id="2eee6-241">hello ASP.NET runtime merges hello contents of hello external file with hello markup in hello `<appSettings>` element.</span></span> <span data-ttu-id="2eee6-242">hello futásidejű figyelmen kívül hagyja hello attribútumot, ha hello megadott fájl nem található.</span><span class="sxs-lookup"><span data-stu-id="2eee6-242">hello runtime ignores hello file attribute if hello specified file cannot be found.</span></span> <span data-ttu-id="2eee6-243">A titkos kulcsokat (hello kapcsolati karakterlánc tooyour gyorsítótár), amelyek nem tartalmazzák a hello alkalmazás forráskódja hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-243">Your secrets (hello connection string tooyour cache) are not included as part of hello source code for hello application.</span></span> <span data-ttu-id="2eee6-244">A webes alkalmazás tooAzure telepítésekor hello `WebAppPlusCacheAppSecrests.config` fájl nem telepíthető (Ez mit).</span><span class="sxs-lookup"><span data-stu-id="2eee6-244">When you deploy your web app tooAzure, hello `WebAppPlusCacheAppSecrests.config` file won't be deployed (that's what you want).</span></span> <span data-ttu-id="2eee6-245">Számos módon toospecify ezeknek a kulcsoknak az Azure-ban, és ebben az oktatóanyagban vannak konfigurálva automatikusan, amikor Ön [kiépítése hello Azure-erőforrások](#provision-the-azure-resources) oktatóanyag egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="2eee6-245">There are several ways toospecify these secrets in Azure, and in this tutorial they are configured automatically for you when you [provision hello Azure resources](#provision-the-azure-resources) in a subsequent tutorial step.</span></span> <span data-ttu-id="2eee6-246">A titkos kulcsok Azure használatáról további információk: [gyakorlati tanácsok a jelszavak és egyéb bizalmas adatok tooASP.NET és Azure App Service üzembe helyezésének](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span><span class="sxs-lookup"><span data-stu-id="2eee6-246">For more information about working with secrets in Azure, see [Best practices for deploying passwords and other sensitive data tooASP.NET and Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span></span>

### <a name="update-hello-teamscontroller-class-tooreturn-results-from-hello-cache-or-hello-database"></a><span data-ttu-id="2eee6-247">Hello TeamsController osztály tooreturn eredményeinek hello gyorsítótár vagy hello adatbázis frissítése</span><span class="sxs-lookup"><span data-stu-id="2eee6-247">Update hello TeamsController class tooreturn results from hello cache or hello database</span></span>
<span data-ttu-id="2eee6-248">Ez a példa team statisztika lekérhető hello adatbázisból vagy hello gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="2eee6-248">In this sample, team statistics can be retrieved from hello database or from hello cache.</span></span> <span data-ttu-id="2eee6-249">Team statisztika, egy szerializált hello-gyorsítótárában vannak tárolva `List<Team>`, és is készletként rendezett Redis-adattípusok használatával.</span><span class="sxs-lookup"><span data-stu-id="2eee6-249">Team statistics are stored in hello cache as a serialized `List<Team>`, and also as a sorted set using Redis data types.</span></span> <span data-ttu-id="2eee6-250">Rendezett készletből történő lekérdezéskor egyes, az összes vagy bizonyos feltételnek megfelelő elemek lekérésére van lehetőség.</span><span class="sxs-lookup"><span data-stu-id="2eee6-250">When retrieving items from a sorted set, you can retrieve some, all, or query for certain items.</span></span> <span data-ttu-id="2eee6-251">Ez a példa hello felső 5 csoportjai wins száma szerinti sorrendben rendezve hello beállítása lesz lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="2eee6-251">In this sample you'll query hello sorted set for hello top 5 teams ranked by number of wins.</span></span>

> [!NOTE]
> <span data-ttu-id="2eee6-252">Már nem szükséges toostore hello team statisztika hello rendelés toouse Azure Redis Cache-gyorsítótár több formátumban.</span><span class="sxs-lookup"><span data-stu-id="2eee6-252">It is not required toostore hello team statistics in multiple formats in hello cache in order toouse Azure Redis Cache.</span></span> <span data-ttu-id="2eee6-253">Ez az oktatóanyag néhány használja több formátumok toodemonstrate hello különböző módokon és a különböző adattípusú toocache adatokat használhatja.</span><span class="sxs-lookup"><span data-stu-id="2eee6-253">This tutorial uses multiple formats toodemonstrate some of hello different ways and different data types you can use toocache data.</span></span>
> 
> 

1. <span data-ttu-id="2eee6-254">Adja hozzá a következő hello `using` utasítások toohello `TeamsController.cs` hello tetején, az egyéb hello fájl `using` utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2eee6-254">Add hello following `using` statements toohello `TeamsController.cs` file at hello top with hello other `using` statements.</span></span>

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. <span data-ttu-id="2eee6-255">Cserélje le a jelenlegi hello `public ActionResult Index()` hello végrehajtása a következő metódus végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2eee6-255">Replace hello current `public ActionResult Index()` method implementation with hello following implementation.</span></span>

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


1. <span data-ttu-id="2eee6-256">Adja hozzá a következő három módszer toohello hello `TeamsController` osztály tooimplement hello `playGames`, `clearCache`, és `rebuildDB` művelettípusok hello a Váltás hello előző kódrészletet hozzáadott utasítást.</span><span class="sxs-lookup"><span data-stu-id="2eee6-256">Add hello following three methods toohello `TeamsController` class tooimplement hello `playGames`, `clearCache`, and `rebuildDB` action types from hello switch statement added in hello previous code snippet.</span></span>
   
    <span data-ttu-id="2eee6-257">Hello `PlayGames` metódus hello team statisztika frissíti a játékok szezon szimulál menti hello eredmények toohello adatbázis és törlése hello most már elavult hello gyorsítótár adatait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-257">hello `PlayGames` method updates hello team statistics by simulating a season of games, saves hello results toohello database, and clears hello now outdated data from hello cache.</span></span>

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

    <span data-ttu-id="2eee6-258">Hello `RebuildDB` metódus újból inicializálja hello adatbázis hello alapértelmezett készletét, csoportok, a számukra statisztika hoz létre, és törlése hello most már elavult hello gyorsítótár adatait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-258">hello `RebuildDB` method reinitializes hello database with hello default set of teams, generates statistics for them, and clears hello now outdated data from hello cache.</span></span>

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

    <span data-ttu-id="2eee6-259">Hello `ClearCachedTeams` metódus hello gyorsítótárból eltávolítja a gyorsítótárazott team statisztikai adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2eee6-259">hello `ClearCachedTeams` method removes any cached team statistics from hello cache.</span></span>

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. <span data-ttu-id="2eee6-260">Adja hozzá a következő négy módszerek toohello hello `TeamsController` osztály tooimplement hello hello team statisztika lekérése hello gyorsítótár és hello adatbázis különféle módjait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-260">Add hello following four methods toohello `TeamsController` class tooimplement hello various ways of retrieving hello team statistics from hello cache and hello database.</span></span> <span data-ttu-id="2eee6-261">Mindkét módszerhez ad vissza egy `List<Team>` hello nézet ezután megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2eee6-261">Each of these methods returns a `List<Team>` which is then displayed by hello view.</span></span>
   
    <span data-ttu-id="2eee6-262">Hello `GetFromDB` metódus olvassa be a hello team statisztika hello adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="2eee6-262">hello `GetFromDB` method reads hello team statistics from hello database.</span></span>
   
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

    <span data-ttu-id="2eee6-263">Hello `GetFromList` metódus hello team statisztika beolvassa a gyorsítótárból, mint egy szerializált `List<Team>`.</span><span class="sxs-lookup"><span data-stu-id="2eee6-263">hello `GetFromList` method reads hello team statistics from cache as a serialized `List<Team>`.</span></span> <span data-ttu-id="2eee6-264">Gyorsítótár-tévesztései esetén hello team statisztika olvasni hello adatbázisból, és aztán hello gyorsítótár a következő alkalommal.</span><span class="sxs-lookup"><span data-stu-id="2eee6-264">If there is a cache miss, hello team statistics are read from hello database and then stored in hello cache for next time.</span></span> <span data-ttu-id="2eee6-265">Ez a példa használunk JSON.NET szerializálási tooserialize hello .NET objektumok tooand hello gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="2eee6-265">In this sample we're using JSON.NET serialization tooserialize hello .NET objects tooand from hello cache.</span></span> <span data-ttu-id="2eee6-266">További információkért lásd: [hogyan toowork a .NET-objektumokat az Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span><span class="sxs-lookup"><span data-stu-id="2eee6-266">For more information, see [How toowork with .NET objects in Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span></span>

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

    <span data-ttu-id="2eee6-267">Hello `GetFromSortedSet` metódus egy gyorsítótárazott rendezett készletből hello team statisztika olvassa be.</span><span class="sxs-lookup"><span data-stu-id="2eee6-267">hello `GetFromSortedSet` method reads hello team statistics from a cached sorted set.</span></span> <span data-ttu-id="2eee6-268">Ha a gyorsítótár-tévesztései, hello team statisztika hello adatbázisból beolvasása és rendezett készletként hello gyorsítótárba.</span><span class="sxs-lookup"><span data-stu-id="2eee6-268">If there is a cache miss, hello team statistics are read from hello database and stored in hello cache as a sorted set.</span></span>

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

    <span data-ttu-id="2eee6-269">Hello `GetFromSortedSetTop5` metódus hello felső 5 csapat a gyorsítótárazott hello rendezve set olvassa be.</span><span class="sxs-lookup"><span data-stu-id="2eee6-269">hello `GetFromSortedSetTop5` method reads hello top 5 teams from hello cached sorted set.</span></span> <span data-ttu-id="2eee6-270">Hello gyorsítótárában keresi az hello hello meglétének ellenőrzésével kezdődik `teamsSortedSet` kulcs.</span><span class="sxs-lookup"><span data-stu-id="2eee6-270">It starts by checking hello cache for hello existence of hello `teamsSortedSet` key.</span></span> <span data-ttu-id="2eee6-271">Ha ez a kulcs nem található, hello `GetFromSortedSet` metódus tooread hello team statisztika nevezik, és a hello gyorsítótárban tárolja őket.</span><span class="sxs-lookup"><span data-stu-id="2eee6-271">If this key is not present, hello `GetFromSortedSet` method is called tooread hello team statistics and store them in hello cache.</span></span> <span data-ttu-id="2eee6-272">A következő hello gyorsítótárazott rendezett állítsa le kell kérdezni hello felső 5 csoportjai visszaküldött.</span><span class="sxs-lookup"><span data-stu-id="2eee6-272">Next, hello cached sorted set is queried for hello top 5 teams which are returned.</span></span>

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

### <a name="update-hello-create-edit-and-delete-methods-toowork-with-hello-cache"></a><span data-ttu-id="2eee6-273">Hello létrehozása, szerkesztése, frissítse és módszerek toowork hello gyorsítótárával törlése</span><span class="sxs-lookup"><span data-stu-id="2eee6-273">Update hello Create, Edit, and Delete methods toowork with hello cache</span></span>
<span data-ttu-id="2eee6-274">hello állványok kód lett létrehozva, mivel ez a minta része belefoglalja módszerek tooadd, szerkeszteni és törölni.</span><span class="sxs-lookup"><span data-stu-id="2eee6-274">hello scaffolding code that was generated as part of this sample includes methods tooadd, edit, and delete teams.</span></span> <span data-ttu-id="2eee6-275">Bármikor csoport hozzáadva, szerkesztésének vagy eltávolításának, hello adatok hello gyorsítótárában elavult válik.</span><span class="sxs-lookup"><span data-stu-id="2eee6-275">Anytime a team is added, edited, or removed, hello data in hello cache becomes outdated.</span></span> <span data-ttu-id="2eee6-276">Ebben a szakaszban módosítania kell ezen három módszer tooclear hello csoportok gyorsítótárazza, így hello gyorsítótár nincs szinkronban a hello adatbázis nem lesz.</span><span class="sxs-lookup"><span data-stu-id="2eee6-276">In this section you'll modify these three methods tooclear hello cached teams so that hello cache won't be out of sync with hello database.</span></span>

1. <span data-ttu-id="2eee6-277">Keresse meg a toohello `Create(Team team)` metódus a hello `TeamsController` osztály.</span><span class="sxs-lookup"><span data-stu-id="2eee6-277">Browse toohello `Create(Team team)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="2eee6-278">Adja hozzá a hívás toohello `ClearCachedTeams` módszer, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-278">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

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


1. <span data-ttu-id="2eee6-279">Keresse meg a toohello `Edit(Team team)` metódus a hello `TeamsController` osztály.</span><span class="sxs-lookup"><span data-stu-id="2eee6-279">Browse toohello `Edit(Team team)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="2eee6-280">Adja hozzá a hívás toohello `ClearCachedTeams` módszer, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-280">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

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


1. <span data-ttu-id="2eee6-281">Keresse meg a toohello `DeleteConfirmed(int id)` metódus a hello `TeamsController` osztály.</span><span class="sxs-lookup"><span data-stu-id="2eee6-281">Browse toohello `DeleteConfirmed(int id)` method in hello `TeamsController` class.</span></span> <span data-ttu-id="2eee6-282">Adja hozzá a hívás toohello `ClearCachedTeams` módszer, ahogy az alábbi példa hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-282">Add a call toohello `ClearCachedTeams` method, as shown in hello following example.</span></span>

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


### <a name="update-hello-teams-index-view-toowork-with-hello-cache"></a><span data-ttu-id="2eee6-283">Hello csapatok Index nézet toowork hello gyorsítótár frissítése</span><span class="sxs-lookup"><span data-stu-id="2eee6-283">Update hello Teams Index view toowork with hello cache</span></span>
1. <span data-ttu-id="2eee6-284">A **Megoldáskezelőben**, bontsa ki a hello **nézetek** mappát, majd hello **csapatok** mappára, majd kattintson duplán **Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-284">In **Solution Explorer**, expand hello **Views** folder, then hello **Teams** folder, and double-click **Index.cshtml**.</span></span>
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. <span data-ttu-id="2eee6-286">Tetején hello hello fájlt keresse meg a következő bekezdésszöveg elem hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-286">Near hello top of hello file, look for hello following paragraph element.</span></span>
   
    ![Művelettábla][cache-teams-index-table]
   
    <span data-ttu-id="2eee6-288">Ez a hello hivatkozás toocreate egy új csoport.</span><span class="sxs-lookup"><span data-stu-id="2eee6-288">This is hello link toocreate a new team.</span></span> <span data-ttu-id="2eee6-289">Cserélje le a következő táblázat hello hello bekezdésszöveg elem.</span><span class="sxs-lookup"><span data-stu-id="2eee6-289">Replace hello paragraph element with hello following table.</span></span> <span data-ttu-id="2eee6-290">Ez a táblázat egy-egy új szezon játékok, ha hello gyorsítótár kiürítése játszott új csoport létrehozása művelet mutató hivatkozásokat tartalmaz, hello csapatok lekérése hello gyorsítótár számos formátumban, hello csapatok lekérése hello adatbázis, majd újraépítése hello adatbázist friss mintaadatokkal.</span><span class="sxs-lookup"><span data-stu-id="2eee6-290">This table has action links for creating a new team, playing a new season of games, clearing hello cache, retrieving hello teams from hello cache in several formats, retrieving hello teams from hello database, and rebuilding hello database with fresh sample data.</span></span>

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


1. <span data-ttu-id="2eee6-291">Görgessen toohello alsó részén hello **Index.cshtml** fájlt, és adja hozzá a következő hello `tr` elem úgy, hogy az utolsó sort hello hello utolsó tábla hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="2eee6-291">Scroll toohello bottom of hello **Index.cshtml** file and add hello following `tr` element so that it is hello last row in hello last table in hello file.</span></span>
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    <span data-ttu-id="2eee6-292">A sor hello értékét jeleníti meg `ViewBag.Msg` hello aktuális műveletekre vonatkozó jelentés, amely tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2eee6-292">This row displays hello value of `ViewBag.Msg` which contains a status report about hello current operation.</span></span> <span data-ttu-id="2eee6-293">Hello `ViewBag.Msg` hello előző lépésben hello művelet hivatkozásokra kattintva van beállítva.</span><span class="sxs-lookup"><span data-stu-id="2eee6-293">hello `ViewBag.Msg` is set when you click any of hello action links from hello previous step.</span></span>   
   
    ![Állapotüzenet][cache-status-message]
2. <span data-ttu-id="2eee6-295">Nyomja le az **F6** toobuild hello projekt.</span><span class="sxs-lookup"><span data-stu-id="2eee6-295">Press **F6** toobuild hello project.</span></span>

## <a name="provision-hello-azure-resources"></a><span data-ttu-id="2eee6-296">Kiépítés hello Azure-erőforrások</span><span class="sxs-lookup"><span data-stu-id="2eee6-296">Provision hello Azure resources</span></span>
<span data-ttu-id="2eee6-297">toohost az alkalmazás az Azure-ban, akkor először engedélyeznie kell az alkalmazás által használt Azure szolgáltatással hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-297">toohost your application in Azure, you must first provision hello Azure services that your application requires.</span></span> <span data-ttu-id="2eee6-298">Ebben az oktatóanyagban hello mintaalkalmazás hello Azure-szolgáltatások a következő használ.</span><span class="sxs-lookup"><span data-stu-id="2eee6-298">hello sample application in this tutorial uses hello following Azure services.</span></span>

* <span data-ttu-id="2eee6-299">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="2eee6-299">Azure Redis Cache</span></span>
* <span data-ttu-id="2eee6-300">App Service webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="2eee6-300">App Service Web App</span></span>
* <span data-ttu-id="2eee6-301">SQL Database</span><span class="sxs-lookup"><span data-stu-id="2eee6-301">SQL Database</span></span>

<span data-ttu-id="2eee6-302">toodeploy ezen szolgáltatások tooa új vagy meglévő erőforráscsoport az Ön által választott, kattintson a következő hello **tooAzure telepítése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2eee6-302">toodeploy these services tooa new or existing resource group of your choice, click hello following **Deploy tooAzure** button.</span></span>

<span data-ttu-id="2eee6-303">[! [TooAzure központi telepítés] [deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="2eee6-303">[![Deploy tooAzure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span></span>

<span data-ttu-id="2eee6-304">Ez **tooAzure telepítése** gomb használja hello [hozzon létre egy webalkalmazást és Redis Cache mellett egy SQL Databaset](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure gyors üzembe helyezési](https://github.com/Azure/azure-quickstart-templates) sablon tooprovision ezen szolgáltatások és a set hello hello SQL-adatbázis és hello Alkalmazásbeállítás hello Azure Redis Cache kapcsolati karakterlánc a kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="2eee6-304">This **Deploy tooAzure** button uses hello [Create a Web App plus Redis Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) template tooprovision these services and set hello connection string for hello SQL Database and hello application setting for hello Azure Redis Cache connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="2eee6-305">Ha nincs Azure-fiókja, néhány perc alatt [létrehozhat egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="2eee6-305">If you don't have an Azure account, you can [create a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in just a couple of minutes.</span></span>
> 
> 

<span data-ttu-id="2eee6-306">Gombra kattintva hello **tooAzure telepítése** gomb viszi toohello Azure-portálon, és kezdeményezi hello hello sablon által ismertetett hello erőforrásokat létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="2eee6-306">Clicking hello **Deploy tooAzure** button takes you toohello Azure portal and initiates hello process of creating hello resources described by hello template.</span></span>

![TooAzure telepítése][cache-deploy-to-azure-step-1]

1. <span data-ttu-id="2eee6-308">A hello **alapjai** szakaszt, válassza ki az Azure-előfizetés toouse hello, és válasszon ki egy meglévő erőforráscsoportot vagy hozzon létre egy újat, és adja meg az erőforráscsoport helye hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-308">In hello **Basics** section, select hello Azure subscription toouse, and select an existing resource group or create a new one, and specify hello resource group location.</span></span>
2. <span data-ttu-id="2eee6-309">A hello **beállítások** területén adja meg egy **rendszergazda bejelentkezési** (ne használjon **admin**), **rendszergazda bejelentkezési jelszó**, és  **Adatbázis neve**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-309">In hello **Settings** section, specify an **Administrator Login** (don't use **admin**), **Administrator Login Password**, and **Database Name**.</span></span> <span data-ttu-id="2eee6-310">hello más paramétereket van állítva egy ingyenes App Service üzemeltetési terv és az alacsonyabb költségű beállítások hello SQL Database és Azure Redis Cache, amelyek nem rendelkeznek egy ingyenes szint.</span><span class="sxs-lookup"><span data-stu-id="2eee6-310">hello other parameters are configured for a free App Service hosting plan, and lower-cost options for hello SQL Database and Azure Redis Cache, which don't come with a free tier.</span></span>

    ![TooAzure telepítése][cache-deploy-to-azure-step-2]

3. <span data-ttu-id="2eee6-312">Szükségeskonfiguráció-hello beállításainak konfigurálása után görgessen hello lap, olvasási hello feltételek és kikötések toohello végét, és ellenőrizze a hello **toohello feltételek és kikötések fenti elfogadom** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="2eee6-312">After configuring hello desired settings, scroll toohello end of hello page, read hello terms and conditions, and check hello **I agree toohello terms and conditions stated above** checkbox.</span></span>
4. <span data-ttu-id="2eee6-313">üzembe helyezési hello erőforrások toobegin, kattintson a **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-313">toobegin provisioning hello resources, click **Purchase**.</span></span>

<span data-ttu-id="2eee6-314">a telepítés előrehaladását tooview hello hello értesítés ikonra, majd kattintson **telepítése megkezdődött**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-314">tooview hello progress of your deployment, click hello notification icon and click **Deployment started**.</span></span>

![Központi telepítés elindítva][cache-deployment-started]

<span data-ttu-id="2eee6-316">A központi telepítés hello állapotát megtekintheti a hello **Microsoft.Template** panelen.</span><span class="sxs-lookup"><span data-stu-id="2eee6-316">You can view hello status of your deployment on hello **Microsoft.Template** blade.</span></span>

![TooAzure telepítése][cache-deploy-to-azure-step-3]

<span data-ttu-id="2eee6-318">Ha kiépítése befejeződött, a Visual Studio alkalmazás tooAzure tehető közzé.</span><span class="sxs-lookup"><span data-stu-id="2eee6-318">When provisioning is complete, you can publish your application tooAzure from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="2eee6-319">Bármely hello kiépítési folyamat során előforduló hibákat hello megjelenő **Microsoft.Template** panelen.</span><span class="sxs-lookup"><span data-stu-id="2eee6-319">Any errors that may occur during hello provisioning process are displayed on hello **Microsoft.Template** blade.</span></span> <span data-ttu-id="2eee6-320">Gyakori hiba például az előfizetésenkénti túl sok SQL Server-példány és a túl sok Ingyenes App Service-futtatási csomag.</span><span class="sxs-lookup"><span data-stu-id="2eee6-320">Common errors are too many SQL Servers or too many Free App Service hosting plans per subscription.</span></span> <span data-ttu-id="2eee6-321">Javítsa ki a hibákat, és indítsa újra a hello folyamat kattintva **újratelepíteni** a hello **Microsoft.Template** panel vagy hello **tooAzure telepítése** ebben az oktatóanyagban gombra.</span><span class="sxs-lookup"><span data-stu-id="2eee6-321">Resolve any errors and restart hello process by clicking **Redeploy** on hello **Microsoft.Template** blade or hello **Deploy tooAzure** button in this tutorial.</span></span>
> 
> 

## <a name="publish-hello-application-tooazure"></a><span data-ttu-id="2eee6-322">Hello alkalmazás tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="2eee6-322">Publish hello application tooAzure</span></span>
<span data-ttu-id="2eee6-323">Ebben a lépésben hello oktatóanyag lesz hello alkalmazás tooAzure közzététele, majd futtassa azt hello felhő.</span><span class="sxs-lookup"><span data-stu-id="2eee6-323">In this step of hello tutorial, you'll publish hello application tooAzure and run it in hello cloud.</span></span>

1. <span data-ttu-id="2eee6-324">Kattintson a jobb gombbal hello **ContosoTeamStats** a Visual Studio projekt, és válassza a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-324">Right-click hello **ContosoTeamStats** project in Visual Studio and choose **Publish**.</span></span>
   
    ![Közzététel][cache-publish-app]
2. <span data-ttu-id="2eee6-326">Kattintson a **Microsoft Azure App Service** lehetőségre, válassza a **Meglévő kiválasztása** elemet, majd kattintson a **Közzététel** gombra.</span><span class="sxs-lookup"><span data-stu-id="2eee6-326">Click **Microsoft Azure App Service**, choose **Select Existing**, and click **Publish**.</span></span>
   
    ![Közzététel][cache-publish-to-app-service]
3. <span data-ttu-id="2eee6-328">Válassza ki a hello hello erőforrásokat tartalmazó erőforráscsoport létrehozása hello Azure-erőforrások, bontsa ki, majd jelölje be hello szükségeskonfiguráció-webalkalmazás hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="2eee6-328">Select hello subscription used when creating hello Azure resources, expand hello resource group containing hello resources, and select hello desired Web App.</span></span> <span data-ttu-id="2eee6-329">Ha hello **tooAzure telepítése** a webes alkalmazás neve kezdődik gomb **webhely** néhány további karakter követ.</span><span class="sxs-lookup"><span data-stu-id="2eee6-329">If you used hello **Deploy tooAzure** button your Web App name starts with **webSite** followed by some additional characters.</span></span>
   
    ![Webalkalmazás kiválasztása][cache-select-web-app]
4. <span data-ttu-id="2eee6-331">Kattintson a **OK** toobegin hello közzétételi folyamat.</span><span class="sxs-lookup"><span data-stu-id="2eee6-331">Click **OK** toobegin hello publishing process.</span></span> <span data-ttu-id="2eee6-332">Néhány másodpercen belül hello közzétételi folyamat befejeződik, és egy böngészőben a mintaalkalmazás futtatása hello nincs elindítva.</span><span class="sxs-lookup"><span data-stu-id="2eee6-332">After a few moments hello publishing process completes and a browser is launched with hello running sample application.</span></span> <span data-ttu-id="2eee6-333">Ha ellenőrzéskor vagy közzététele a DNS a hibaüzenet, és hello létesítésének folyamatát kell használnia a hello hello alkalmazás Azure-erőforrások csak nemrég befejeződött, várjon egy kicsit, és próbálja meg újból.</span><span class="sxs-lookup"><span data-stu-id="2eee6-333">If you get a DNS error when validating or publishing, and hello provisioning process for hello Azure resources for hello application has just recently completed, wait a moment and try again.</span></span>
   
    ![Gyorsítótár hozzáadva][cache-added-to-application]

<span data-ttu-id="2eee6-335">hello következő táblázat ismerteti a hello mintaalkalmazás minden művelet hivatkozására.</span><span class="sxs-lookup"><span data-stu-id="2eee6-335">hello following table describes each action link from hello sample application.</span></span>

| <span data-ttu-id="2eee6-336">Műveletek</span><span class="sxs-lookup"><span data-stu-id="2eee6-336">Action</span></span> | <span data-ttu-id="2eee6-337">Leírás</span><span class="sxs-lookup"><span data-stu-id="2eee6-337">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2eee6-338">Új létrehozása</span><span class="sxs-lookup"><span data-stu-id="2eee6-338">Create New</span></span> |<span data-ttu-id="2eee6-339">Létrehoz egy új csapatot.</span><span class="sxs-lookup"><span data-stu-id="2eee6-339">Create a new Team.</span></span> |
| <span data-ttu-id="2eee6-340">Szezon végigjátszása</span><span class="sxs-lookup"><span data-stu-id="2eee6-340">Play Season</span></span> |<span data-ttu-id="2eee6-341">A játékok, frissítés hello team statisztikák szezon lejátszása, és törölje minden elavult hello gyorsítótár csoport adatait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-341">Play a season of games, update hello team stats, and clear any outdated team data from hello cache.</span></span> |
| <span data-ttu-id="2eee6-342">Gyorsítótár ürítése</span><span class="sxs-lookup"><span data-stu-id="2eee6-342">Clear Cache</span></span> |<span data-ttu-id="2eee6-343">Törölje a jelet hello team statisztikák hello gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="2eee6-343">Clear hello team stats from hello cache.</span></span> |
| <span data-ttu-id="2eee6-344">Lista a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="2eee6-344">List from Cache</span></span> |<span data-ttu-id="2eee6-345">Hello team statisztikák lekérése a hello gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="2eee6-345">Retrieve hello team stats from hello cache.</span></span> <span data-ttu-id="2eee6-346">Ha a gyorsítótár-tévesztései, hello statisztikák betöltése hello adatbázisból, és menthet toohello gyorsítótár legközelebb.</span><span class="sxs-lookup"><span data-stu-id="2eee6-346">If there is a cache miss, load hello stats from hello database and save toohello cache for next time.</span></span> |
| <span data-ttu-id="2eee6-347">Rendezett készlet a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="2eee6-347">Sorted Set from Cache</span></span> |<span data-ttu-id="2eee6-348">Hello team statisztikák lekérése hello gyorsítótár rendezett készletből.</span><span class="sxs-lookup"><span data-stu-id="2eee6-348">Retrieve hello team stats from hello cache using a sorted set.</span></span> <span data-ttu-id="2eee6-349">Ha a gyorsítótár-tévesztései, hello statisztikák betöltése hello adatbázisból, és rendezett készletből toohello gyorsítótár mentése.</span><span class="sxs-lookup"><span data-stu-id="2eee6-349">If there is a cache miss, load hello stats from hello database and save toohello cache using a sorted set.</span></span> |
| <span data-ttu-id="2eee6-350">Az 5 legjobb csapat a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="2eee6-350">Top 5 Teams from Cache</span></span> |<span data-ttu-id="2eee6-351">Hello felső 5 csapatok lekérése hello gyorsítótár rendezett készletből.</span><span class="sxs-lookup"><span data-stu-id="2eee6-351">Retrieve hello top 5 teams from hello cache using a sorted set.</span></span> <span data-ttu-id="2eee6-352">Ha a gyorsítótár-tévesztései, hello statisztikák betöltése hello adatbázisból, és rendezett készletből toohello gyorsítótár mentése.</span><span class="sxs-lookup"><span data-stu-id="2eee6-352">If there is a cache miss, load hello stats from hello database and save toohello cache using a sorted set.</span></span> |
| <span data-ttu-id="2eee6-353">Betöltés adatbázisból</span><span class="sxs-lookup"><span data-stu-id="2eee6-353">Load from DB</span></span> |<span data-ttu-id="2eee6-354">Hello team statisztikák lekérése hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2eee6-354">Retrieve hello team stats from hello database.</span></span> |
| <span data-ttu-id="2eee6-355">Adatbázis újraépítése</span><span class="sxs-lookup"><span data-stu-id="2eee6-355">Rebuild DB</span></span> |<span data-ttu-id="2eee6-356">Hello adatbázis újraépítése, és töltse be újra a csapat mintaadatokkal.</span><span class="sxs-lookup"><span data-stu-id="2eee6-356">Rebuild hello database and reload it with sample team data.</span></span> |
| <span data-ttu-id="2eee6-357">Szerkesztés / Részletek / Törlés</span><span class="sxs-lookup"><span data-stu-id="2eee6-357">Edit / Details / Delete</span></span> |<span data-ttu-id="2eee6-358">Szerkeszthet egy csapatot, megtekintheti annak részletes adatait, törölhet egy csapatot.</span><span class="sxs-lookup"><span data-stu-id="2eee6-358">Edit a team, view details for a team, delete a team.</span></span> |

<span data-ttu-id="2eee6-359">Kattintson a hello műveletek némelyike, és kísérletezzen hello különböző forrásokból származó hello adatok beolvasása.</span><span class="sxs-lookup"><span data-stu-id="2eee6-359">Click some of hello actions and experiment with retrieving hello data from hello different sources.</span></span> <span data-ttu-id="2eee6-360">Nem hello különbségeit hello időt toocomplete hello hello adatok lekérése hello adatbázis és hello gyorsítótár különféle módjait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-360">Not hello differences in hello time it takes toocomplete hello various ways of retrieving hello data from hello database and hello cache.</span></span>

## <a name="delete-hello-resources-when-you-are-finished-with-hello-application"></a><span data-ttu-id="2eee6-361">Törli a hello erőforrást, amikor elkészült, hello alkalmazással</span><span class="sxs-lookup"><span data-stu-id="2eee6-361">Delete hello resources when you are finished with hello application</span></span>
<span data-ttu-id="2eee6-362">Ha elkészült, hello oktatóanyag mintaalkalmazást, törölheti a hello Azure rendelés tooconserve költségeket, a használt erőforrások és erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2eee6-362">When you are finished with hello sample tutorial application, you can delete hello Azure resources used in order tooconserve cost and resources.</span></span> <span data-ttu-id="2eee6-363">Hello használatakor **tooAzure telepítése** hello gombjára [rendelkezés hello Azure-erőforrások](#provision-the-azure-resources) szakasz és az összes erőforrást tartalmaznak hello ugyanabban az erőforráscsoportban, törölheti azokat együtt egy a művelet hello erőforráscsoport törlésével.</span><span class="sxs-lookup"><span data-stu-id="2eee6-363">If you use hello **Deploy tooAzure** button in hello [Provision hello Azure resources](#provision-the-azure-resources) section and all of your resources are contained in hello same resource group, you can delete them together in one operation by deleting hello resource group.</span></span>

1. <span data-ttu-id="2eee6-364">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) kattintson **erőforráscsoportok**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-364">Sign in toohello [Azure portal](https://portal.azure.com) and click **Resource groups**.</span></span>
2. <span data-ttu-id="2eee6-365">Az erőforráscsoport a hello hello nevét **elemek szűrése...**  szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2eee6-365">Type hello name of your resource group into hello **Filter items...** textbox.</span></span>
3. <span data-ttu-id="2eee6-366">Kattintson a **...**  toohello sarkában található az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="2eee6-366">Click **...** toohello right of your resource group.</span></span>
4. <span data-ttu-id="2eee6-367">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2eee6-367">Click **Delete**.</span></span>
   
    ![Törlés][cache-delete-resource-group]
5. <span data-ttu-id="2eee6-369">Hello nevét a erőforráscsoportban, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="2eee6-369">Type hello name of your resource group and click **Delete**.</span></span>
   
    ![Törlés megerősítése][cache-delete-confirm]

<span data-ttu-id="2eee6-371">Néhány perc múlva hello erőforrás után csoport és a benne lévő erőforrásokat törlődnek.</span><span class="sxs-lookup"><span data-stu-id="2eee6-371">After a few moments hello resource group and all of its contained resources are deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2eee6-372">Vegye figyelembe, hogy egy erőforráscsoport törlése nem vonható vissza, és hogy hello erőforráscsoport és az összes hello-erőforrásokat véglegesen törlődnek.</span><span class="sxs-lookup"><span data-stu-id="2eee6-372">Note that deleting a resource group is irreversible and that hello resource group and all hello resources in it are permanently deleted.</span></span> <span data-ttu-id="2eee6-373">Győződjön meg arról, hogy nem véletlenül törli hello megfelelő erőforráscsoport és erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2eee6-373">Make sure that you do not accidentally delete hello wrong resource group or resources.</span></span> <span data-ttu-id="2eee6-374">Ha ez a minta egy meglévő erőforráscsoportot belül üzemeltetéséhez hello erőforrások hozott létre, törölheti az egyes erőforrások egyenként a megfelelő panelt a.</span><span class="sxs-lookup"><span data-stu-id="2eee6-374">If you created hello resources for hosting this sample inside an existing resource group, you can delete each resource individually from their respective blades.</span></span>
> 
> 

## <a name="run-hello-sample-application-on-your-local-machine"></a><span data-ttu-id="2eee6-375">A helyi gépen hello mintaalkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="2eee6-375">Run hello sample application on your local machine</span></span>
<span data-ttu-id="2eee6-376">az Azure Redis Cache szükséges toorun hello alkalmazás helyben a számítógépre, mely toocache a példány adatait.</span><span class="sxs-lookup"><span data-stu-id="2eee6-376">toorun hello application locally on your machine, you need an Azure Redis Cache instance in which toocache your data.</span></span> 

* <span data-ttu-id="2eee6-377">Miután közzétette az alkalmazást tooAzure hello előző szakaszban leírtak szerint, ha e lépés során lett kiépítve hello Azure Redis Cache példány is használhatja.</span><span class="sxs-lookup"><span data-stu-id="2eee6-377">If you have published your application tooAzure as described in hello previous section, you can use hello Azure Redis Cache instance that was provisioned during that step.</span></span>
* <span data-ttu-id="2eee6-378">Ha egy másik meglévő Azure Redis Cache példányt, használhatja a toorun Ez a minta helyileg.</span><span class="sxs-lookup"><span data-stu-id="2eee6-378">If you have another existing Azure Redis Cache instance, you can use that toorun this sample locally.</span></span>
* <span data-ttu-id="2eee6-379">Azure Redis Cache példány toocreate van szüksége, ha a hello lépések végrehajtásával [gyorsítótár létrehozásához](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span><span class="sxs-lookup"><span data-stu-id="2eee6-379">If you need toocreate an Azure Redis Cache instance, you can follow hello steps in [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

<span data-ttu-id="2eee6-380">Miután kiválasztása vagy létrehozása hello gyorsítótár toouse, keresse meg a gyorsítótár toohello hello Azure-portálon, és hello beolvasása [állomásnév](cache-configure.md#properties) és [hívóbetűk](cache-configure.md#access-keys) a gyorsítótárhoz.</span><span class="sxs-lookup"><span data-stu-id="2eee6-380">Once you have selected or created hello cache toouse, browse toohello cache in hello Azure portal and retrieve hello [host name](cache-configure.md#properties) and [access keys](cache-configure.md#access-keys) for your cache.</span></span> <span data-ttu-id="2eee6-381">Útmutatásért lásd: [A Redis Cache-gyorsítótár beállításai](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="2eee6-381">For instructions, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

1. <span data-ttu-id="2eee6-382">Nyissa meg hello `WebAppPlusCacheAppSecrets.config` hello során létrehozott fájl [hello alkalmazás toouse Redis gyorsítótár konfigurálása](#configure-the-application-to-use-redis-cache) . lépését Ez az oktatóanyag a választott szerkesztővel hello.</span><span class="sxs-lookup"><span data-stu-id="2eee6-382">Open hello `WebAppPlusCacheAppSecrets.config` file that you created during hello [Configure hello application toouse Redis Cache](#configure-the-application-to-use-redis-cache) step of this tutorial using hello editor of your choice.</span></span>
2. <span data-ttu-id="2eee6-383">Hello szerkesztése `value` attribútumot, és cserélje le `MyCache.redis.cache.windows.net` a hello [állomásnév](cache-configure.md#properties) a gyorsítótár, és adja meg vagy hello [elsődleges vagy másodlagos kulcsot](cache-configure.md#access-keys) a gyorsítótár hello jelszóként.</span><span class="sxs-lookup"><span data-stu-id="2eee6-383">Edit hello `value` attribute and replace `MyCache.redis.cache.windows.net` with hello [host name](cache-configure.md#properties) of your cache, and specify either hello [primary or secondary key](cache-configure.md#access-keys) of your cache as hello password.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="2eee6-384">Nyomja le az **Ctrl + F5** toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2eee6-384">Press **Ctrl+F5** toorun hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="2eee6-385">Vegye figyelembe, hogy mivel hello alkalmazás, többek között a hello adatbázis helyben fut, és hello Redis gyorsítótárat üzemeltetni kívánja az Azure cache hello jelenhetnek meg toounder-hello adatbázis végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="2eee6-385">Note that because hello application, including hello database, is running locally and hello Redis Cache is hosted in Azure, hello cache may appear toounder-perform hello database.</span></span> <span data-ttu-id="2eee6-386">A legjobb teljesítmény érdekében hello ügyfélalkalmazást és az Azure Redis Cache példány belül hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="2eee6-386">For best performance, hello client application and Azure Redis Cache instance should be in hello same location.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="2eee6-387">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2eee6-387">Next steps</span></span>
* <span data-ttu-id="2eee6-388">További információ [első ASP.NET MVC 5 használatába](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) a hello [ASP.NET](http://asp.net/) hely.</span><span class="sxs-lookup"><span data-stu-id="2eee6-388">Learn more about [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) on hello [ASP.NET](http://asp.net/) site.</span></span>
* <span data-ttu-id="2eee6-389">ASP.NET webalkalmazás létrehozása az App Service további példákért lásd [létrehozása és telepítése az Azure App Service ASP.NET webalkalmazás](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) a hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [bemutató](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="2eee6-389">For more examples of creating an ASP.NET Web App in App Service, see [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) from hello [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span>
  * <span data-ttu-id="2eee6-390">Tekintse meg a hello HealthClinic.biz bemutató további quickstarts [Azure fejlesztői eszközök Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span><span class="sxs-lookup"><span data-stu-id="2eee6-390">For more quickstarts from hello HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>
* <span data-ttu-id="2eee6-391">Tudjon meg többet a hello [kód első tooa új adatbázis](https://msdn.microsoft.com/data/jj193542) közelítse tooEntity ebben az oktatóanyagban használt keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="2eee6-391">Learn more about hello [Code first tooa new database](https://msdn.microsoft.com/data/jj193542) approach tooEntity Framework that's used in this tutorial.</span></span>
* <span data-ttu-id="2eee6-392">További információ [az Azure App Service webalkalmazásairól](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="2eee6-392">Learn more about [web apps in Azure App Service](../app-service-web/app-service-web-overview.md).</span></span>
* <span data-ttu-id="2eee6-393">Ismerje meg, hogyan túl[figyelő](cache-how-to-monitor.md) saját gyorsítótárához az hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2eee6-393">Learn how too[monitor](cache-how-to-monitor.md) your cache in hello Azure portal.</span></span>
* <span data-ttu-id="2eee6-394">Az Azure Redis Cache prémium funkcióinak megismerése</span><span class="sxs-lookup"><span data-stu-id="2eee6-394">Explore Azure Redis Cache premium features</span></span>
  
  * [<span data-ttu-id="2eee6-395">Hogyan tooconfigure megőrzését egy prémium szintű Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="2eee6-395">How tooconfigure persistence for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-persistence.md)
  * [<span data-ttu-id="2eee6-396">Hogyan fürtözése a Premium Azure Redis Cache tooconfigure</span><span class="sxs-lookup"><span data-stu-id="2eee6-396">How tooconfigure clustering for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-clustering.md)
  * [<span data-ttu-id="2eee6-397">Hogyan támogatják a virtuális hálózati tooconfigure a Premium Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="2eee6-397">How tooconfigure Virtual Network support for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-vnet.md)
  * <span data-ttu-id="2eee6-398">Lásd: hello [Azure Redis Cache – gyakori kérdések](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) méretét, az átviteli sebesség és a sávszélesség a prémium szintű gyorsítótárak kapcsolatos további részletekért.</span><span class="sxs-lookup"><span data-stu-id="2eee6-398">See hello [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) for more details about size, throughput, and bandwidth with premium caches.</span></span>

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

