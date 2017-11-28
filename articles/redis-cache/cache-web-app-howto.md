---
title: "Webalkalmazás létrehozása a Redis Cache használatával | Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre webalkalmazást a Redis Cache használatával"
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
ms.openlocfilehash: f23f71cc01eccf17d36885f786de9a7517606803
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-web-app-with-redis-cache"></a><span data-ttu-id="48188-103">Webalkalmazás létrehozása a Redis Cache használatával</span><span class="sxs-lookup"><span data-stu-id="48188-103">How to create a Web App with Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48188-104">.NET</span><span class="sxs-lookup"><span data-stu-id="48188-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="48188-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="48188-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="48188-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="48188-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="48188-107">Java</span><span class="sxs-lookup"><span data-stu-id="48188-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="48188-108">Python</span><span class="sxs-lookup"><span data-stu-id="48188-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="48188-109">Ez az oktatóanyag bemutatja, hogyan hozhat létre és helyezhet üzembe egy ASP.NET-webalkalmazást az Azure App Service szolgáltatásban lévő webalkalmazásba a Visual Studio 2017 használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-109">This tutorial shows how to create and deploy an ASP.NET web application to a web app in Azure App Service using Visual Studio 2017.</span></span> <span data-ttu-id="48188-110">Ez a mintaalkalmazás a csoportstatisztikák adatbázisból származó listáját jeleníti meg, illetve az Azure Redis Cache használatának különböző módjait mutatja be a gyorsítótár adatainak tárolására és beolvasására.</span><span class="sxs-lookup"><span data-stu-id="48188-110">The sample application displays a list of team statistics from a database and shows different ways to use Azure Redis Cache to store and retrieve data from the cache.</span></span> <span data-ttu-id="48188-111">Az oktatóanyag befejezését követően egy olyan futó webalkalmazással fog rendelkezni, amely adatokat olvas be és ír egy adatbázisba, az Azure Redis Cache használatával lett optimalizálva, és az Azure-ban van üzemeltetve.</span><span class="sxs-lookup"><span data-stu-id="48188-111">When you complete the tutorial you'll have a running web app that reads and writes to a database, optimized with Azure Redis Cache, and hosted in Azure.</span></span>

<span data-ttu-id="48188-112">Az oktatóanyagból a következőket sajátíthatja el:</span><span class="sxs-lookup"><span data-stu-id="48188-112">You'll learn:</span></span>

* <span data-ttu-id="48188-113">ASP.NET MVC 5 webalkalmazás létrehozása a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-113">How to create an ASP.NET MVC 5 web application in Visual Studio.</span></span>
* <span data-ttu-id="48188-114">Adatbázisadatok elérése az Entity Framework használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-114">How to access data from a database using Entity Framework.</span></span>
* <span data-ttu-id="48188-115">Az adatteljesítmény növelésének és az adatbázis-terhelés csökkentése az Azure Redis Cache használatával történő adattárolás és -beolvasás révén.</span><span class="sxs-lookup"><span data-stu-id="48188-115">How to improve data throughput and reduce database load by storing and retrieving data using Azure Redis Cache.</span></span>
* <span data-ttu-id="48188-116">Egy rendezett Redis-készlet használata az 5 legjobb csoport lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="48188-116">How to use a Redis sorted set to retrieve the top 5 teams.</span></span>
* <span data-ttu-id="48188-117">Azure-erőforrások kiépítése egy Resource Manager-sablont használó alkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="48188-117">How to provision the Azure resources for the application using a Resource Manager template.</span></span>
* <span data-ttu-id="48188-118">Alkalmazás közzététele az Azure-ban a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-118">How to publish the application to Azure using Visual Studio.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48188-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="48188-119">Prerequisites</span></span>
<span data-ttu-id="48188-120">Az oktatóanyag elvégzéséhez az alábbi előfeltételekkel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="48188-120">To complete the tutorial, you must have the following prerequisites.</span></span>

* [<span data-ttu-id="48188-121">Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="48188-121">Azure account</span></span>](#azure-account)
* [<span data-ttu-id="48188-122">Visual Studio 2017 Azure SDK for .NET csomaggal</span><span class="sxs-lookup"><span data-stu-id="48188-122">Visual Studio 2017 with the Azure SDK for .NET</span></span>](#visual-studio-2017-with-the-azure-sdk-for-net)

### <a name="azure-account"></a><span data-ttu-id="48188-123">Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="48188-123">Azure account</span></span>
<span data-ttu-id="48188-124">Az oktatóanyag elvégzéséhez szüksége lesz egy Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="48188-124">You need an Azure account to complete the tutorial.</span></span> <span data-ttu-id="48188-125">A következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="48188-125">You can:</span></span>

* <span data-ttu-id="48188-126">[Nyisson egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="48188-126">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="48188-127">Jóváírásokat kap, amelyeket fizetős Azure-szolgáltatások kipróbálására használhat fel.</span><span class="sxs-lookup"><span data-stu-id="48188-127">You get credits that can be used to try out paid Azure services.</span></span> <span data-ttu-id="48188-128">Még ha a keretét el is használta, továbbra is megtarthatja a fiókot, és használhatja az ingyenes szolgáltatásokat és lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="48188-128">Even after the credits are used up, you can keep the account and use free Azure services and features.</span></span>
* <span data-ttu-id="48188-129">[Aktiválja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="48188-129">[Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero).</span></span> <span data-ttu-id="48188-130">Az MSDN-előfizetés minden hónapban biztosít Önnek krediteket, amelyekkel fizetős Azure-szolgáltatásokat használhat.</span><span class="sxs-lookup"><span data-stu-id="48188-130">Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

### <a name="visual-studio-2017-with-the-azure-sdk-for-net"></a><span data-ttu-id="48188-131">Visual Studio 2017 Azure SDK for .NET csomaggal</span><span class="sxs-lookup"><span data-stu-id="48188-131">Visual Studio 2017 with the Azure SDK for .NET</span></span>
<span data-ttu-id="48188-132">Az oktatóanyag a Visual Studio 2017-hez, valamint az [Azure SDK for .NET-hez](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools) készült.</span><span class="sxs-lookup"><span data-stu-id="48188-132">The tutorial is written for Visual Studio 2017 with the [Azure SDK for .NET](https://www.visualstudio.com/news/releasenotes/vs2017-relnotes#azuretools).</span></span> <span data-ttu-id="48188-133">Az Azure SDK 2.9.5 a Visual Studio telepítőjének részét képezi.</span><span class="sxs-lookup"><span data-stu-id="48188-133">The Azure SDK 2.9.5 is included with the Visual Studio installer.</span></span>

<span data-ttu-id="48188-134">Ha a gépén a Visual Studio 2015 van telepítve, kövesse az [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 vagy újabb verzió oktatóanyagát.</span><span class="sxs-lookup"><span data-stu-id="48188-134">If you have Visual Studio 2015, you can follow the tutorial with the [Azure SDK for .NET](../dotnet-sdk.md) 2.8.2 or later.</span></span> <span data-ttu-id="48188-135">[Innen letöltheti a legfrissebb Azure SDK-t a Visual Studio 2015-höz](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="48188-135">[Download the latest Azure SDK for Visual Studio 2015 here](http://go.microsoft.com/fwlink/?linkid=518003).</span></span> <span data-ttu-id="48188-136">Ha a Visual Studio még nincs telepítve, a rendszer automatikusan telepíti azt, az SDK-val együtt.</span><span class="sxs-lookup"><span data-stu-id="48188-136">Visual Studio is automatically installed with the SDK if you don't already have it.</span></span> <span data-ttu-id="48188-137">Egyes képernyők eltérhetnek a jelen oktatóanyag ábráin láthatóaktól.</span><span class="sxs-lookup"><span data-stu-id="48188-137">Some screens may look different from the illustrations shown in this tutorial.</span></span>

<span data-ttu-id="48188-138">Ha a számítógépén a Visual Studio 2013 van telepítve, [töltse le a legfrissebb Azure SDK for Visual Studio 2013 alkalmazást](http://go.microsoft.com/fwlink/?LinkID=324322).</span><span class="sxs-lookup"><span data-stu-id="48188-138">If you have Visual Studio 2013, you can [download the latest Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322).</span></span> <span data-ttu-id="48188-139">Egyes képernyők eltérhetnek a jelen oktatóanyag ábráin láthatóaktól.</span><span class="sxs-lookup"><span data-stu-id="48188-139">Some screens may look different from the illustrations shown in this tutorial.</span></span>

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="48188-140">A Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="48188-140">Create the Visual Studio project</span></span>
1. <span data-ttu-id="48188-141">Nyissa meg a Visual Studio alkalmazást, majd kattintson a **File** (File), **New** (Új), **Project** (Projekt) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="48188-141">Open Visual Studio and click **File**, **New**, **Project**.</span></span>
2. <span data-ttu-id="48188-142">Bontsa ki a **Visual C#** csomópontot a **Templates** (Sablonok) listában, válassza a **Cloud** (Felhő) lehetőséget, majd kattintson az **ASP.NET Web Application** (ASP.NET-webalkalmazás) elemre.</span><span class="sxs-lookup"><span data-stu-id="48188-142">Expand the **Visual C#** node in the **Templates** list, select **Cloud**, and click **ASP.NET Web Application**.</span></span> <span data-ttu-id="48188-143">Győződjön meg arról, hogy a **.NET Framework 4.5.2** vagy újabb keretrendszer van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="48188-143">Ensure that **.NET Framework 4.5.2** or higher is selected.</span></span>  <span data-ttu-id="48188-144">Írja be a **ContosoTeamStats** szöveget a **Name** (Név) szövegmezőbe, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-144">Type **ContosoTeamStats** into the **Name** textbox and click **OK**.</span></span>
   
    ![Projekt létrehozása][cache-create-project]
3. <span data-ttu-id="48188-146">A projekt típusaként válassza az **MVC** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="48188-146">Select **MVC** as the project type.</span></span> 

    <span data-ttu-id="48188-147">Ellenőrizze, hogy az **Authentication** (Hitelesítés) beállításai között a **No Authentication** (Nincs hitelesítés) van megadva.</span><span class="sxs-lookup"><span data-stu-id="48188-147">Ensure that **No Authentication** is specified for the **Authentication** settings.</span></span> <span data-ttu-id="48188-148">A Visual Studio verziójától függően az alapértelmezett beállítás más lehet.</span><span class="sxs-lookup"><span data-stu-id="48188-148">Depending on your version of Visual Studio, the default may be set to something else.</span></span> <span data-ttu-id="48188-149">A beállítás módosításához kattintson a **Change Authentication** (Hitelesítés módosítása) gombra, és válassza a **No Authentication** (Nincs hitelesítés) értéket.</span><span class="sxs-lookup"><span data-stu-id="48188-149">To change it, click **Change Authentication** and select **No Authentication**.</span></span>

    <span data-ttu-id="48188-150">Ha a Visual Studio 2015-öt használja, törölje a **Host in the cloud** (Üzemeltetés a felhőben) jelölőnégyzet jelölését.</span><span class="sxs-lookup"><span data-stu-id="48188-150">If you are following along with Visual Studio 2015, clear the **Host in the cloud** checkbox.</span></span> <span data-ttu-id="48188-151">Az oktatóanyag következő lépéseiben megismerkedhet az [Azure-erőforrások kiépítésével](#provision-the-azure-resources) és az [alkalmazások közzétételével az Azure-ban](#publish-the-application-to-azure).</span><span class="sxs-lookup"><span data-stu-id="48188-151">You'll [provision the Azure resources](#provision-the-azure-resources) and [publish the application to Azure](#publish-the-application-to-azure) in subsequent steps in the tutorial.</span></span> <span data-ttu-id="48188-152">A **Host in the cloud** (Üzemeltetés a felhőben) jelölőnégyzet bejelölésével a Visual Studio felületéről egy App Service-webalkalmazás létrehozására itt láthat példát: [Ismerkedés a webalkalmazásokkal az Azure App Service-ben, az ASP.NET és a Visual Studio használatával](../app-service-web/app-service-web-get-started-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="48188-152">For an example of provisioning an App Service web app from Visual Studio by leaving **Host in the cloud** checked, see [Get started with Web Apps in Azure App Service, using ASP.NET and Visual Studio](../app-service-web/app-service-web-get-started-dotnet.md).</span></span>
   
    ![Projektsablon kiválasztása][cache-select-template]
4. <span data-ttu-id="48188-154">A projekt létrehozásához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-154">Click **OK** to create the project.</span></span>

## <a name="create-the-aspnet-mvc-application"></a><span data-ttu-id="48188-155">Az ASP.NET MVC alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="48188-155">Create the ASP.NET MVC application</span></span>
<span data-ttu-id="48188-156">Az oktatóanyag ezen szakaszában egy olyan alapszintű alkalmazást fog létrehozni, amely adatbázisból olvas be és jelenít meg csoportstatisztikákat.</span><span class="sxs-lookup"><span data-stu-id="48188-156">In this section of the tutorial, you'll create the basic application that reads and displays team statistics from a database.</span></span>

* [<span data-ttu-id="48188-157">Az Entity Framework NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48188-157">Add the Entity Framework NuGet package</span></span>](#add-the-entity-framework-nuget-package)
* [<span data-ttu-id="48188-158">Modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48188-158">Add the model</span></span>](#add-the-model)
* [<span data-ttu-id="48188-159">Vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48188-159">Add the controller</span></span>](#add-the-controller)
* [<span data-ttu-id="48188-160">A nézetek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48188-160">Configure the views</span></span>](#configure-the-views)

### <a name="add-the-entity-framework-nuget-package"></a><span data-ttu-id="48188-161">Az Entity Framework NuGet-csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48188-161">Add the Entity Framework NuGet package</span></span>

1. <span data-ttu-id="48188-162">Kattintson a **Tools** (Eszközök) menü **NuGet Package Manager** (NuGet-csomagkezelő), **Package Manager Console** (Csomagkezelő konzol) elemére.</span><span class="sxs-lookup"><span data-stu-id="48188-162">Click **NuGet Package Manager**, **Package Manager Console** from the **Tools** menu.</span></span>
2. <span data-ttu-id="48188-163">Futtassa a következő parancsot a **Csomagkezelő konzol** ablakából.</span><span class="sxs-lookup"><span data-stu-id="48188-163">Run the following command from the **Package Manager Console** window.</span></span>
    
    ```
    Install-Package EntityFramework
    ```

<span data-ttu-id="48188-164">A csomaggal kapcsolatos további információt az [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet-oldalon talál.</span><span class="sxs-lookup"><span data-stu-id="48188-164">For more information about this package, see the [EntityFramework](https://www.nuget.org/packages/EntityFramework/) NuGet page.</span></span>

### <a name="add-the-model"></a><span data-ttu-id="48188-165">Modell hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48188-165">Add the model</span></span>
1. <span data-ttu-id="48188-166">Kattintson a jobb gombbal a **Models** (Modellek) elemre a **Solution Explorer** (Megoldáskezelő) területén, és válassza az **Add** (Hozzáadás), **Class** (Osztály) lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="48188-166">Right-click **Models** in **Solution Explorer**, and choose **Add**, **Class**.</span></span> 
   
    ![Modell hozzáadása][cache-model-add-class]
2. <span data-ttu-id="48188-168">Az osztály neveként adja meg a `Team` nevet, majd kattintson az **Add** (Hozzáadás) gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-168">Enter `Team` for the class name and click **Add**.</span></span>
   
    ![Modellosztály hozzáadása][cache-model-add-class-dialog]
3. <span data-ttu-id="48188-170">A `Team.cs` fájl elején cserélje le a `using` utasításokat az alábbi `using` utasításokra.</span><span class="sxs-lookup"><span data-stu-id="48188-170">Replace the `using` statements at the top of the `Team.cs` file with the following `using` statements.</span></span>

    ```c#
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Data.Entity.SqlServer;
    ```


1. <span data-ttu-id="48188-171">Cserélje le a `Team` osztály definícióját az alábbi kódrészlettel, amely a `Team` osztály frissített definícióját, valamint néhány további Entity Framework-súgóosztályt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="48188-171">Replace the definition of the `Team` class with the following code snippet that contains an updated `Team` class definition as well as some other Entity Framework helper classes.</span></span> <span data-ttu-id="48188-172">További információk a jelen oktatóanyagban használt, Code First nevű Entity Framework-megközelítésról: [Code First alkalmazása egy új adatbázisra](https://msdn.microsoft.com/data/jj193542).</span><span class="sxs-lookup"><span data-stu-id="48188-172">For more information on the code first approach to Entity Framework that's used in this tutorial, see [Code first to a new database](https://msdn.microsoft.com/data/jj193542).</span></span>

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


1. <span data-ttu-id="48188-173">A **Solution Explorerben** (Megoldáskezelőben) kattintson duplán a **web.config** fájlra annak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="48188-173">In **Solution Explorer**, double-click **web.config** to open it.</span></span>
   
    ![Web.config][cache-web-config]
2. <span data-ttu-id="48188-175">Adja hozzá a következő `connectionStrings` szakaszt.</span><span class="sxs-lookup"><span data-stu-id="48188-175">Add the following `connectionStrings` section.</span></span> <span data-ttu-id="48188-176">A kapcsolati karakterlánc nevének meg kell egyeznie az Entity Framework-adatbáziskörnyezet osztályának nevével, amely a következő: `TeamContext`.</span><span class="sxs-lookup"><span data-stu-id="48188-176">The name of the connection string must match the name of the Entity Framework database context class which is `TeamContext`.</span></span>

    ```xml
    <connectionStrings>
        <add name="TeamContext" connectionString="Data Source=(LocalDB)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
    </connectionStrings>
    ```

    <span data-ttu-id="48188-177">Hozzáadhatja az új `connectionStrings` szakaszt a `configSections` után, ahogyan az az alábbi példában látható.</span><span class="sxs-lookup"><span data-stu-id="48188-177">You can add the new `connectionStrings` section so that it follows `configSections`, as shown in the following example.</span></span>

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
    > <span data-ttu-id="48188-178">A kapcsolati karakterlánc eltérő lehet az oktatóanyag elvégzéséhez használt Visual Studio- és SQL Server Express-verziótól függően.</span><span class="sxs-lookup"><span data-stu-id="48188-178">Your connection string may be different depending on the version of Visual Studio and SQL Server Express edition used to complete the tutorial.</span></span> <span data-ttu-id="48188-179">A web.config sablont úgy kell konfigurálni, hogy megfeleljen a telepítésnek, és tartalmazhat `Data Source` bejegyzéseket, mint a `(LocalDB)\v11.0` (az SQL Server Express 2012-ből) vagy a `Data Source=(LocalDB)\MSSQLLocalDB` (az SQL Server Express 2014 és újabb verziókból).</span><span class="sxs-lookup"><span data-stu-id="48188-179">The web.config template should be configured to match your installation, and may contain `Data Source` entries like `(LocalDB)\v11.0` (from SQL Server Express 2012) or `Data Source=(LocalDB)\MSSQLLocalDB` (from SQL Server Express 2014 and newer).</span></span> <span data-ttu-id="48188-180">További információ a kapcsolati karakterláncokról és az SQL Express-verziókról: [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span><span class="sxs-lookup"><span data-stu-id="48188-180">For more information about connection strings and SQL Express versions, see [SQL Server 2016 Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) .</span></span>

### <a name="add-the-controller"></a><span data-ttu-id="48188-181">Vezérlő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48188-181">Add the controller</span></span>
1. <span data-ttu-id="48188-182">A projekt létrehozásához nyomja le az **F6** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="48188-182">Press **F6** to build the project.</span></span> 
2. <span data-ttu-id="48188-183">A **Solution Explorerben** (Megoldáskezelőben) kattintson a jobb gombbal a **Controllers** (Vezérlők) mappára, majd válassza az **Add** (Hozzáadás), **Controller** (Vezérlő) lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="48188-183">In **Solution Explorer**, right-click the **Controllers** folder and choose **Add**, **Controller**.</span></span>
   
    ![Vezérlő hozzáadása][cache-add-controller]
3. <span data-ttu-id="48188-185">Válassza az **MVC 5 Controller with views, using Entity Framework** (MVC 5 vezérlő nézetekkel, az Entity Framework használatával) lehetőséget, majd kattintson az **Add** (Hozzáadás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="48188-185">Choose **MVC 5 Controller with views, using Entity Framework**, and click **Add**.</span></span> <span data-ttu-id="48188-186">Ha az **Add** (Hozzáadás) gombra kattintva a rendszer hibaüzenetet küld, győződjön meg arról, hogy a projekt létrehozása előzetesen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="48188-186">If you get an error after clicking **Add**, ensure that you have built the project first.</span></span>
   
    ![Vezérlőosztály hozzáadása][cache-add-controller-class]
4. <span data-ttu-id="48188-188">Válassza ki a **Team (ContosoTeamStats.Models)** elemet a **Model class** (Modellosztály) legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="48188-188">Select **Team (ContosoTeamStats.Models)** from the **Model class** drop-down list.</span></span> <span data-ttu-id="48188-189">Válassza ki a **TeamContext (ContosoTeamStats.Models)** elemet a **Adatkörnyezet osztálya** (Adatkörnyezet osztálya) legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="48188-189">Select **TeamContext (ContosoTeamStats.Models)** from the **Data context class** drop-down list.</span></span> <span data-ttu-id="48188-190">Írja be a `TeamsController` szöveget a **Controller** (Vezérlő) névmezőbe (ha az nincs automatikusan kitöltve).</span><span class="sxs-lookup"><span data-stu-id="48188-190">Type `TeamsController` in the **Controller** name textbox (if it is not automatically populated).</span></span> <span data-ttu-id="48188-191">Kattintson az **Add** (Hozzáadás) gombra a vezérlőosztály létrehozásához és az alapértelmezett nézetek hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="48188-191">Click **Add** to create the controller class and add the default views.</span></span>
   
    ![Vezérlő konfigurálása][cache-configure-controller]
5. <span data-ttu-id="48188-193">A **Solution Explorerben** (Megoldáskezelőben) bontsa ki a **Global.asax** elemet, majd kattintson duplán a **Global.asax.cs** fájlra annak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="48188-193">In **Solution Explorer**, expand **Global.asax** and double-click **Global.asax.cs** to open it.</span></span>
   
    ![Global.asax.cs][cache-global-asax]
6. <span data-ttu-id="48188-195">Adja hozzá a következő két `using` utasítást a fájl elejéhez, a többi `using` utasítás alá.</span><span class="sxs-lookup"><span data-stu-id="48188-195">Add the following two `using` statements at the top of the file under the other `using` statements.</span></span>

    ```c#
    using System.Data.Entity;
    using ContosoTeamStats.Models;
    ```


1. <span data-ttu-id="48188-196">Illessze az alábbi kódsort az `Application_Start` módszer végére.</span><span class="sxs-lookup"><span data-stu-id="48188-196">Add the following line of code at the end of the `Application_Start` method.</span></span>

    ```c#
    Database.SetInitializer<TeamContext>(new TeamInitializer());
    ```


1. <span data-ttu-id="48188-197">A **Solution Explorerben** (Megoldáskezelőben) bontsa ki az `App_Start` elemet, majd kattintson duplán a `RouteConfig.cs` elemre.</span><span class="sxs-lookup"><span data-stu-id="48188-197">In **Solution Explorer**, expand `App_Start` and double-click `RouteConfig.cs`.</span></span>
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. <span data-ttu-id="48188-199">Cserélje le a `controller = "Home"` elemet a `RegisterRoutes` módszer alábbi kódjában a `controller = "Teams"` szövegre, a következő példán látható módon.</span><span class="sxs-lookup"><span data-stu-id="48188-199">Replace `controller = "Home"` in the following code in the `RegisterRoutes` method with `controller = "Teams"` as shown in the following example.</span></span>

    ```c#
    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
    );
    ```


### <a name="configure-the-views"></a><span data-ttu-id="48188-200">A nézetek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48188-200">Configure the views</span></span>
1. <span data-ttu-id="48188-201">A **Solution Explorerben** (Megoldáskezelőben) bontsa ki a **Views**(Nézetek), majd a **Shared** (Közös) mappát, és kattintson duplán a **_Layout.cshtml** fájlra.</span><span class="sxs-lookup"><span data-stu-id="48188-201">In **Solution Explorer**, expand the **Views** folder and then the **Shared** folder, and double-click **_Layout.cshtml**.</span></span> 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. <span data-ttu-id="48188-203">Az alábbi példában látható módon módosítsa a `title` elem tartalmát, majd cserélje le a `My ASP.NET Application` szöveget a `Contoso Team Stats` szövegre.</span><span class="sxs-lookup"><span data-stu-id="48188-203">Change the contents of the `title` element and replace `My ASP.NET Application` with `Contoso Team Stats` as shown in the following example.</span></span>

    ```html
    <title>@ViewBag.Title - Contoso Team Stats</title>
    ```


1. <span data-ttu-id="48188-204">A `body` szakaszban frissítse az első `Html.ActionLink` utasítást, és cserélje le az `Application name` szöveget a `Contoso Team Stats` szövegre, majd a `Home` szöveget a `Teams` szövegre.</span><span class="sxs-lookup"><span data-stu-id="48188-204">In the `body` section, update the first `Html.ActionLink` statement and replace `Application name` with `Contoso Team Stats` and replace `Home` with `Teams`.</span></span>
   
   * <span data-ttu-id="48188-205">Előtte: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="48188-205">Before: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
   * <span data-ttu-id="48188-206">Utána: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span><span class="sxs-lookup"><span data-stu-id="48188-206">After: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`</span></span>
     
     ![Kódmódosítások][cache-layout-cshtml-code]
2. <span data-ttu-id="48188-208">Az alkalmazás fordításához és futtatásához nyomja le a **Ctrl+F5** billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="48188-208">Press **Ctrl+F5** to build and run the application.</span></span> <span data-ttu-id="48188-209">Az alkalmazás ezen verziója az eredményeket közvetlenül az adatbázisból olvassa ki.</span><span class="sxs-lookup"><span data-stu-id="48188-209">This version of the application reads the results directly from the database.</span></span> <span data-ttu-id="48188-210">Figyelje meg, hogy az **Új létrehozása**, a **Szerkesztés**, a **Részletek**és a **Törlés** parancsok az **MVC 5 Controller with views, using Entity Framework** (MVC 5 vezérlő nézetekkel, az Entity Framework használatával) szerkezettel automatikusan bekerültek az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="48188-210">Note the **Create New**, **Edit**, **Details**, and **Delete** actions that were automatically added to the application by the **MVC 5 Controller with views, using Entity Framework** scaffold.</span></span> <span data-ttu-id="48188-211">Az oktatóanyag következő szakaszában az adatelérés optimalizálása és további alkalmazásszolgáltatások biztosítása érdekében el fogja végezni a Redis Cache hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="48188-211">In the next section of the tutorial you'll add Redis Cache to optimize the data access and provide additional features to the application.</span></span>

![Kezdő szintű alkalmazás][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a><span data-ttu-id="48188-213">Az alkalmazás konfigurálása a Redis Cache használatára</span><span class="sxs-lookup"><span data-stu-id="48188-213">Configure the application to use Redis Cache</span></span>
<span data-ttu-id="48188-214">Az oktatóanyag jelen szakaszában el fogja végezni a mintaalkalmazás konfigurálását az Azure Redis Cache-példányból származó Contoso-csoportstatisztikák tárolására és beolvasására a [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) gyorsítótárügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-214">In this section of the tutorial, you'll configure the sample application to store and retrieve Contoso team statistics from an Azure Redis Cache instance by using the [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) cache client.</span></span>

* [<span data-ttu-id="48188-215">Az alkalmazás konfigurálása a StackExchange.Redis használatára</span><span class="sxs-lookup"><span data-stu-id="48188-215">Configure the application to use StackExchange.Redis</span></span>](#configure-the-application-to-use-stackexchangeredis)
* [<span data-ttu-id="48188-216">A TeamsController osztály frissítése a gyorsítótárból vagy az adatbázisból eredmények visszaadásához</span><span class="sxs-lookup"><span data-stu-id="48188-216">Update the TeamsController class to return results from the cache or the database</span></span>](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [<span data-ttu-id="48188-217">A Létrehozás, Szerkesztés és Törlés módszerek frissítése a gyorsítótárral való együttműködéshez</span><span class="sxs-lookup"><span data-stu-id="48188-217">Update the Create, Edit, and Delete methods to work with the cache</span></span>](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [<span data-ttu-id="48188-218">A Teams Index nézet frissítése a gyorsítótárral való együttműködéshez</span><span class="sxs-lookup"><span data-stu-id="48188-218">Update the Teams Index view to work with the cache</span></span>](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-the-application-to-use-stackexchangeredis"></a><span data-ttu-id="48188-219">Az alkalmazás konfigurálása a StackExchange.Redis használatára</span><span class="sxs-lookup"><span data-stu-id="48188-219">Configure the application to use StackExchange.Redis</span></span>
1. <span data-ttu-id="48188-220">Ha egy ügyfélalkalmazást a StackExchange.Redis NuGet-csomaggal szeretne konfigurálni a Visual Studióban, kattintson a **Tools** (Eszközök) menü **NuGet Package Manager** (NuGet-csomagkezelő), **Package Manager Console** (Csomagkezelő konzol) elemére.</span><span class="sxs-lookup"><span data-stu-id="48188-220">To configure a client application in Visual Studio using the StackExchange.Redis NuGet package, click **NuGet Package Manager**, **Package Manager Console** from the **Tools** menu.</span></span>
2. <span data-ttu-id="48188-221">Futtassa az alábbi parancsot a `Package Manager Console` ablakából.</span><span class="sxs-lookup"><span data-stu-id="48188-221">Run the following command from the `Package Manager Console` window.</span></span>
    
    ```
    Install-Package StackExchange.Redis
    ```
   
    <span data-ttu-id="48188-222">A NuGet-csomag letölti és hozzáadja az ügyfélalkalmazás számára szükséges szerelvényhivatkozásokat az Azure Redis Cache a StackExchange.Redis gyorsítótárügyféllel történő eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="48188-222">The NuGet package downloads and adds the required assembly references for your client application to access Azure Redis Cache with the StackExchange.Redis cache client.</span></span> <span data-ttu-id="48188-223">Ha inkább a `StackExchange.Redis` ügyfélkönyvtár erős elnevezésű verzióját kívánja használni, telepítse a `StackExchange.Redis.StrongName` csomagot.</span><span class="sxs-lookup"><span data-stu-id="48188-223">If you prefer to use a strong-named version of the `StackExchange.Redis` client library, install the `StackExchange.Redis.StrongName` package.</span></span>
3. <span data-ttu-id="48188-224">A **Solution Explorerben** (Megoldáskezelőben) bontsa ki a **Controllers** (Vezérlők) mappát, majd kattintson duplán a **TeamsController.cs** fájlra annak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="48188-224">In **Solution Explorer**, expand the **Controllers** folder and double-click **TeamsController.cs** to open it.</span></span>
   
    ![Csoportvezérlő][cache-teamscontroller]
4. <span data-ttu-id="48188-226">Adja hozzá az alábbi két `using` utasítást a **TeamsController.cs** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="48188-226">Add the following two `using` statements to **TeamsController.cs**.</span></span>

    ```c#   
    using System.Configuration;
    using StackExchange.Redis;
    ```

5. <span data-ttu-id="48188-227">Adja hozzá az alábbi két tulajdonságot a `TeamsController` osztályhoz.</span><span class="sxs-lookup"><span data-stu-id="48188-227">Add the following two properties to the `TeamsController` class.</span></span>

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

6. <span data-ttu-id="48188-228">Hozzon létre egy `WebAppPlusCacheAppSecrets.config` nevű fájlt a számítógépen, majd mentse azt egy olyan helyre, amelyet a mintaalkalmazás forráskódja nem fog ellenőrizni, amennyiben úgy dönt, hogy valahol ellenőrizni kívánja azt.</span><span class="sxs-lookup"><span data-stu-id="48188-228">Create a file on your computer named `WebAppPlusCacheAppSecrets.config` and place it in a location that won't be checked in with the source code of your sample application, should you decide to check it in somewhere.</span></span> <span data-ttu-id="48188-229">Jelen példában az `AppSettingsSecrets.config` fájl elérési útja: `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span><span class="sxs-lookup"><span data-stu-id="48188-229">In this example the `AppSettingsSecrets.config` file is located at `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.</span></span>
   
    <span data-ttu-id="48188-230">Módosítsa a `WebAppPlusCacheAppSecrets.config` fájlt, és adja hozzá az alábbi tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="48188-230">Edit the `WebAppPlusCacheAppSecrets.config` file and add the following contents.</span></span> <span data-ttu-id="48188-231">Az alkalmazás helyi futtatásakor ezen információk az Azure Redis Cache-példányhoz történő kapcsolódáshoz lesznek felhasználva.</span><span class="sxs-lookup"><span data-stu-id="48188-231">If you run the application locally this information is used to connect to your Azure Redis Cache instance.</span></span> <span data-ttu-id="48188-232">Az oktatóanyag későbbi szakaszában egy Azure Redis Cache-példány létrehozását, valamint a gyorsítótár nevének és jelszavának módosítását fogja elvégezni.</span><span class="sxs-lookup"><span data-stu-id="48188-232">Later in the tutorial you'll provision an Azure Redis Cache instance and update the cache name and password.</span></span> <span data-ttu-id="48188-233">Ha nem tervezi az alkalmazás helyi futtatását, kihagyhatja ennek a fájlnak a létrehozását, illetve a fájlra hivatkozó következő lépéseket, mivel az Azure-on történő telepítéskor az alkalmazás a gyorsítótár csatlakoztatási információit a webalkalmazás beállításaiból kéri le, nem pedig ebből a fájlból.</span><span class="sxs-lookup"><span data-stu-id="48188-233">If you don't plan to run the sample application locally you can skip the creation of this file and the subsequent steps that reference the file, because when you deploy to Azure the application retrieves the cache connection information from the app setting for the Web App and not from this file.</span></span> <span data-ttu-id="48188-234">Mivel a `WebAppPlusCacheAppSecrets.config` nem települ az Azure-on az alkalmazással együtt, csak abban az esetben van rá szüksége, ha az alkalmazást helyileg kívánja futtatni.</span><span class="sxs-lookup"><span data-stu-id="48188-234">Since the `WebAppPlusCacheAppSecrets.config` is not deployed to Azure with your application, you don't need it unless you are going to run the application locally.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="48188-235">A **Solution Explorerben** (Megoldáskezelőben) kattintson duplán a **web.config** fájlra annak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="48188-235">In **Solution Explorer**, double-click **web.config** to open it.</span></span>
   
    ![Web.config][cache-web-config]
2. <span data-ttu-id="48188-237">Adja hozzá az alábbi `file` attribútumot az `appSettings` elemhez.</span><span class="sxs-lookup"><span data-stu-id="48188-237">Add the following `file` attribute to the `appSettings` element.</span></span> <span data-ttu-id="48188-238">Ha más fájlnevet vagy helyet használ, helyettesítse azokat a példában látható értékekkel.</span><span class="sxs-lookup"><span data-stu-id="48188-238">If you used a different file name or location, substitute those values for the ones shown in the example.</span></span>
   
   * <span data-ttu-id="48188-239">Előtte: `<appSettings>`</span><span class="sxs-lookup"><span data-stu-id="48188-239">Before: `<appSettings>`</span></span>
   * <span data-ttu-id="48188-240">Utána: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span><span class="sxs-lookup"><span data-stu-id="48188-240">After: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`</span></span>
     
   <span data-ttu-id="48188-241">Az ASP.NET futtatási környezet a külső fájl tartalmát egyesíti az `<appSettings>` elem kódjával.</span><span class="sxs-lookup"><span data-stu-id="48188-241">The ASP.NET runtime merges the contents of the external file with the markup in the `<appSettings>` element.</span></span> <span data-ttu-id="48188-242">Ha a megadott fájl nem található, a futtatási környezet figyelmen kívül hagyja a fájlattribútumot.</span><span class="sxs-lookup"><span data-stu-id="48188-242">The runtime ignores the file attribute if the specified file cannot be found.</span></span> <span data-ttu-id="48188-243">A titkos kulcsok (a gyorsítótárhoz tartozó kapcsolati karakterláncok) nem képezik részét az alkalmazás forráskódjának.</span><span class="sxs-lookup"><span data-stu-id="48188-243">Your secrets (the connection string to your cache) are not included as part of the source code for the application.</span></span> <span data-ttu-id="48188-244">A webalkalmazás Azure-on történő üzembe helyezésekor a `WebAppPlusCacheAppSecrests.config` fájl nem lesz telepítve (ez megfelel a szándékainknak).</span><span class="sxs-lookup"><span data-stu-id="48188-244">When you deploy your web app to Azure, the `WebAppPlusCacheAppSecrests.config` file won't be deployed (that's what you want).</span></span> <span data-ttu-id="48188-245">A titkos kulcsok megadására számos mód létezik az Azure-ban, ezek pedig ennek az oktatóanyagnak a későbbi lépéseiben automatikusan konfigurálva lesznek az [Azure-erőforrások kiépítésekor](#provision-the-azure-resources).</span><span class="sxs-lookup"><span data-stu-id="48188-245">There are several ways to specify these secrets in Azure, and in this tutorial they are configured automatically for you when you [provision the Azure resources](#provision-the-azure-resources) in a subsequent tutorial step.</span></span> <span data-ttu-id="48188-246">További információk a titkos kulcsok használatáról az Azure-ban: [Ajánlott eljárások a jelszavak és egyéb érzékeny adatok telepítéséhez az ASP.NET és az Azure App Service szolgáltatásokban](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span><span class="sxs-lookup"><span data-stu-id="48188-246">For more information about working with secrets in Azure, see [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure).</span></span>

### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a><span data-ttu-id="48188-247">A TeamsController osztály frissítése a gyorsítótárból vagy az adatbázisból eredmények visszaadásához</span><span class="sxs-lookup"><span data-stu-id="48188-247">Update the TeamsController class to return results from the cache or the database</span></span>
<span data-ttu-id="48188-248">Jelen példában a csapatstatisztikák az adatbázisból vagy a gyorsítótárból is lekérdezhetők.</span><span class="sxs-lookup"><span data-stu-id="48188-248">In this sample, team statistics can be retrieved from the database or from the cache.</span></span> <span data-ttu-id="48188-249">A csapatstatisztikák a gyorsítótárban szerializált `List<Team>`, illetve (Redis adattípusok használatával) rendezett készlet formájában vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="48188-249">Team statistics are stored in the cache as a serialized `List<Team>`, and also as a sorted set using Redis data types.</span></span> <span data-ttu-id="48188-250">Rendezett készletből történő lekérdezéskor egyes, az összes vagy bizonyos feltételnek megfelelő elemek lekérésére van lehetőség.</span><span class="sxs-lookup"><span data-stu-id="48188-250">When retrieving items from a sorted set, you can retrieve some, all, or query for certain items.</span></span> <span data-ttu-id="48188-251">Jelen példában lekérdezünk egy rendezett készletet a győzelmek száma szerint rangsorolt 5 legjobb csapatra.</span><span class="sxs-lookup"><span data-stu-id="48188-251">In this sample you'll query the sorted set for the top 5 teams ranked by number of wins.</span></span>

> [!NOTE]
> <span data-ttu-id="48188-252">Az Azure Redis Cache használatához nem szükséges a csapatstatisztikák többféle formátumban történő elmentése a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="48188-252">It is not required to store the team statistics in multiple formats in the cache in order to use Azure Redis Cache.</span></span> <span data-ttu-id="48188-253">Ez az oktatóanyag többféle formátumot használ az adatok gyorsítótárazásához használható különböző módszerek és adattípusok példáinak bemutatására.</span><span class="sxs-lookup"><span data-stu-id="48188-253">This tutorial uses multiple formats to demonstrate some of the different ways and different data types you can use to cache data.</span></span>
> 
> 

1. <span data-ttu-id="48188-254">Adja hozzá az alábbi `using` utasításokat a `TeamsController.cs` fájl elejéhez, a többi `using` utasítással együtt.</span><span class="sxs-lookup"><span data-stu-id="48188-254">Add the following `using` statements to the `TeamsController.cs` file at the top with the other `using` statements.</span></span>

    ```c#   
    using System.Diagnostics;
    using Newtonsoft.Json;
    ```

2. <span data-ttu-id="48188-255">Az aktuális `public ActionResult Index()` metódusmegvalósítást cserélje le az alábbi megvalósításra.</span><span class="sxs-lookup"><span data-stu-id="48188-255">Replace the current `public ActionResult Index()` method implementation with the following implementation.</span></span>

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

            case "clearCache": // Clear the results from the cache.
                ClearCachedTeams();
                break;

            case "rebuildDB": // Rebuild the database with sample data.
                RebuildDB();
                break;
        }

        // Measure the time it takes to retrieve the results.
        Stopwatch sw = Stopwatch.StartNew();

        switch(resultType)
        {
            case "teamsSortedSet": // Retrieve teams from sorted set.
                teams = GetFromSortedSet();
                break;

            case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                teams = GetFromSortedSetTop5();
                break;

            case "teamsList": // Retrieve teams from the cached List<Team>.
                teams = GetFromList();
                break;

            case "fromDB": // Retrieve results from the database.
            default:
                teams = GetFromDB();
                break;
        }

        sw.Stop();
        double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

        // Add the elapsed time of the operation to the ViewBag.msg.
        ViewBag.msg += " MS: " + ms.ToString();

        return View(teams);
    }
    ```


1. <span data-ttu-id="48188-256">Vegye fel az alábbi három módszert a `TeamsController` osztályba azon `playGames`, `clearCache`, és `rebuildDB` művelettípusok megvalósításához, amelyek az előző kódrészletben hozzáadott „switch” utasításból származnak.</span><span class="sxs-lookup"><span data-stu-id="48188-256">Add the following three methods to the `TeamsController` class to implement the `playGames`, `clearCache`, and `rebuildDB` action types from the switch statement added in the previous code snippet.</span></span>
   
    <span data-ttu-id="48188-257">Egy játékszezon szimulálásával a `PlayGames` módszer frissíti a csapatstatisztikákat, az eredményeket elmenti az adatbázisba, majd törli a gyorsítótárból a már elavult adatokat.</span><span class="sxs-lookup"><span data-stu-id="48188-257">The `PlayGames` method updates the team statistics by simulating a season of games, saves the results to the database, and clears the now outdated data from the cache.</span></span>

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

    <span data-ttu-id="48188-258">A `RebuildDB` módszer újrainicializálja az adatbázist az alapértelmezett csapatokkal, statisztikákat állít elő számukra, és törli a gyorsítótárból a már elavult adatokat.</span><span class="sxs-lookup"><span data-stu-id="48188-258">The `RebuildDB` method reinitializes the database with the default set of teams, generates statistics for them, and clears the now outdated data from the cache.</span></span>

    ```c#
    void RebuildDB()
    {
        ViewBag.msg += "Rebuilding DB. ";
        // Delete and re-initialize the database with sample data.
        db.Database.Delete();
        db.Database.Initialize(true);

        // Clear any cached results
        ClearCachedTeams();
    }
    ```

    <span data-ttu-id="48188-259">A `ClearCachedTeams` módszer eltávolítja a gyorsítótárazott csapatstatisztikákat a gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="48188-259">The `ClearCachedTeams` method removes any cached team statistics from the cache.</span></span>

    ```c#
    void ClearCachedTeams()
    {
        IDatabase cache = Connection.GetDatabase();
        cache.KeyDelete("teamsList");
        cache.KeyDelete("teamsSortedSet");
        ViewBag.msg += "Team data removed from cache. ";
    } 
    ```


1. <span data-ttu-id="48188-260">Vegye fel az alábbi négy módszert a `TeamsController` osztályba a csapatstatisztikák gyorsítótárból és adatbázisból különböző módszerekkel történő lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="48188-260">Add the following four methods to the `TeamsController` class to implement the various ways of retrieving the team statistics from the cache and the database.</span></span> <span data-ttu-id="48188-261">Ezen módszerek mindegyike egy, a nézetben megjelenített `List<Team>` választ ad vissza.</span><span class="sxs-lookup"><span data-stu-id="48188-261">Each of these methods returns a `List<Team>` which is then displayed by the view.</span></span>
   
    <span data-ttu-id="48188-262">A `GetFromDB` módszer beolvassa a csapatstatisztikákat a gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="48188-262">The `GetFromDB` method reads the team statistics from the database.</span></span>
   
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

    <span data-ttu-id="48188-263">A `GetFromList` módszer szerializált `List<Team>` formájában olvassa be a csapatstatisztikákat a gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="48188-263">The `GetFromList` method reads the team statistics from cache as a serialized `List<Team>`.</span></span> <span data-ttu-id="48188-264">Gyorsítótár-tévesztés esetén a rendszer az adatbázisból olvassa be a statisztikákat, és azokat a gyorsítótárba menti a következő alkalomra.</span><span class="sxs-lookup"><span data-stu-id="48188-264">If there is a cache miss, the team statistics are read from the database and then stored in the cache for next time.</span></span> <span data-ttu-id="48188-265">Jelen mintában a JSON.NET szerializálást alkalmazzuk a .NET-objektumok gyorsítótárba és gyorsítótárból történő szerializálására.</span><span class="sxs-lookup"><span data-stu-id="48188-265">In this sample we're using JSON.NET serialization to serialize the .NET objects to and from the cache.</span></span> <span data-ttu-id="48188-266">További információk: [.NET-objektumokkal való munka az Azure Redis Cache-ben](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span><span class="sxs-lookup"><span data-stu-id="48188-266">For more information, see [How to work with .NET objects in Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache).</span></span>

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

            ViewBag.msg += "Storing results to cache. ";
            cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
        }
        return teams;
    }
    ```

    <span data-ttu-id="48188-267">A `GetFromSortedSet` módszer beolvassa a csapatstatisztikákat egy gyorsítótárazott rendezett készletből.</span><span class="sxs-lookup"><span data-stu-id="48188-267">The `GetFromSortedSet` method reads the team statistics from a cached sorted set.</span></span> <span data-ttu-id="48188-268">Gyorsítótár-tévesztés esetén a rendszer az adatbázisból olvassa be a statisztikákat, és azokat a gyorsítótárba menti, rendezett készletként.</span><span class="sxs-lookup"><span data-stu-id="48188-268">If there is a cache miss, the team statistics are read from the database and stored in the cache as a sorted set.</span></span>

    ```c#
    List<Team> GetFromSortedSet()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();
        // If the key teamsSortedSet is not present, this method returns a 0 length collection.
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

            ViewBag.msg += "Storing results to cache. ";
            foreach (var t in teams)
            {
                Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
            }
        }
        return teams;
    }
    ```

    <span data-ttu-id="48188-269">A `GetFromSortedSetTop5` módszer beolvassa az 5 legjobb csapatot a gyorsítótárazott rendezett készletből.</span><span class="sxs-lookup"><span data-stu-id="48188-269">The `GetFromSortedSetTop5` method reads the top 5 teams from the cached sorted set.</span></span> <span data-ttu-id="48188-270">Első lépésben a `teamsSortedSet` kulcsot keresi meg a gyorsítótárban.</span><span class="sxs-lookup"><span data-stu-id="48188-270">It starts by checking the cache for the existence of the `teamsSortedSet` key.</span></span> <span data-ttu-id="48188-271">Ha a kulcs nem található, a rendszer a `GetFromSortedSet` módszert hívja meg a csapatstatisztikák beolvasásához és azoknak a gyorsítótárban történő tárolásához.</span><span class="sxs-lookup"><span data-stu-id="48188-271">If this key is not present, the `GetFromSortedSet` method is called to read the team statistics and store them in the cache.</span></span> <span data-ttu-id="48188-272">Ezt a gyorsítótárazott rendezett készlet lekérdezése követi, amely az 5 legjobb csapatot adja vissza.</span><span class="sxs-lookup"><span data-stu-id="48188-272">Next, the cached sorted set is queried for the top 5 teams which are returned.</span></span>

    ```c#
    List<Team> GetFromSortedSetTop5()
    {
        List<Team> teams = null;
        IDatabase cache = Connection.GetDatabase();

        // If the key teamsSortedSet is not present, this method returns a 0 length collection.
        var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        if(teamsSortedSet.Count() == 0)
        {
            // Load the entire sorted set into the cache.
            GetFromSortedSet();

            // Retrieve the top 5 teams.
            teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
        }

        ViewBag.msg += "Retrieving top 5 teams from cache. ";
        // Get the top 5 teams from the sorted set
        teams = new List<Team>();
        foreach (var team in teamsSortedSet)
        {
            teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
        }
        return teams;
    }
    ```

### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a><span data-ttu-id="48188-273">A Létrehozás, Szerkesztés és Törlés módszerek frissítése a gyorsítótárral való együttműködéshez</span><span class="sxs-lookup"><span data-stu-id="48188-273">Update the Create, Edit, and Delete methods to work with the cache</span></span>
<span data-ttu-id="48188-274">A szerkezeti kódot a rendszer ezen minta részeként állítja elő a csapatok hozzáadásához, szerkesztéséhez és törléséhez.</span><span class="sxs-lookup"><span data-stu-id="48188-274">The scaffolding code that was generated as part of this sample includes methods to add, edit, and delete teams.</span></span> <span data-ttu-id="48188-275">Egy csapat hozzáadását, szerkesztését vagy eltávolítását követően a gyorsítótárban található adatok elavulttá válnak.</span><span class="sxs-lookup"><span data-stu-id="48188-275">Anytime a team is added, edited, or removed, the data in the cache becomes outdated.</span></span> <span data-ttu-id="48188-276">Jelen szakaszban ezen három módszer módosítását fogja elvégezni a gyorsítótárazott csapatok törlése érdekében, így a gyorsítótár szinkronizálva lesz az adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="48188-276">In this section you'll modify these three methods to clear the cached teams so that the cache won't be out of sync with the database.</span></span>

1. <span data-ttu-id="48188-277">Keresse meg a `Create(Team team)` módszert a `TeamsController` osztályban.</span><span class="sxs-lookup"><span data-stu-id="48188-277">Browse to the `Create(Team team)` method in the `TeamsController` class.</span></span> <span data-ttu-id="48188-278">Adjon hozzá hívást a `ClearCachedTeams` módszerhez, ahogy az az alábbi példában is látható.</span><span class="sxs-lookup"><span data-stu-id="48188-278">Add a call to the `ClearCachedTeams` method, as shown in the following example.</span></span>

    ```c#
    // POST: Teams/Create
    // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Teams.Add(team);
            db.SaveChanges();
            // When a team is added, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }

        return View(team);
    }
    ```


1. <span data-ttu-id="48188-279">Keresse meg a `Edit(Team team)` módszert a `TeamsController` osztályban.</span><span class="sxs-lookup"><span data-stu-id="48188-279">Browse to the `Edit(Team team)` method in the `TeamsController` class.</span></span> <span data-ttu-id="48188-280">Adjon hozzá hívást a `ClearCachedTeams` módszerhez, ahogy az az alábbi példában is látható.</span><span class="sxs-lookup"><span data-stu-id="48188-280">Add a call to the `ClearCachedTeams` method, as shown in the following example.</span></span>

    ```c#
    // POST: Teams/Edit/5
    // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
    // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
    [HttpPost]
    [ValidateAntiForgeryToken]
    public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
    {
        if (ModelState.IsValid)
        {
            db.Entry(team).State = EntityState.Modified;
            db.SaveChanges();
            // When a team is edited, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }
        return View(team);
    }
    ```


1. <span data-ttu-id="48188-281">Keresse meg a `DeleteConfirmed(int id)` módszert a `TeamsController` osztályban.</span><span class="sxs-lookup"><span data-stu-id="48188-281">Browse to the `DeleteConfirmed(int id)` method in the `TeamsController` class.</span></span> <span data-ttu-id="48188-282">Adjon hozzá hívást a `ClearCachedTeams` módszerhez, ahogy az az alábbi példában is látható.</span><span class="sxs-lookup"><span data-stu-id="48188-282">Add a call to the `ClearCachedTeams` method, as shown in the following example.</span></span>

    ```c#
    // POST: Teams/Delete/5
    [HttpPost, ActionName("Delete")]
    [ValidateAntiForgeryToken]
    public ActionResult DeleteConfirmed(int id)
    {
        Team team = db.Teams.Find(id);
        db.Teams.Remove(team);
        db.SaveChanges();
        // When a team is deleted, the cache is out of date.
        // Clear the cached teams.
        ClearCachedTeams();
        return RedirectToAction("Index");
    }
    ```


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a><span data-ttu-id="48188-283">A Teams Index nézet frissítése a gyorsítótárral való együttműködéshez</span><span class="sxs-lookup"><span data-stu-id="48188-283">Update the Teams Index view to work with the cache</span></span>
1. <span data-ttu-id="48188-284">A **Solution Explorer** (Megoldáskezelőben) bontsa ki a **Views** (Nézetek), majd a **Teams** (Csapatok) mappát, és kattintson duplán az **Index.cshtml** fájlra.</span><span class="sxs-lookup"><span data-stu-id="48188-284">In **Solution Explorer**, expand the **Views** folder, then the **Teams** folder, and double-click **Index.cshtml**.</span></span>
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. <span data-ttu-id="48188-286">A fájl elején keresse meg az alábbi bekezdéselemet.</span><span class="sxs-lookup"><span data-stu-id="48188-286">Near the top of the file, look for the following paragraph element.</span></span>
   
    ![Művelettábla][cache-teams-index-table]
   
    <span data-ttu-id="48188-288">Itt található az új csapat létrehozására szolgáló hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="48188-288">This is the link to create a new team.</span></span> <span data-ttu-id="48188-289">Cserélje le a bekezdéselemet az alábbi táblával.</span><span class="sxs-lookup"><span data-stu-id="48188-289">Replace the paragraph element with the following table.</span></span> <span data-ttu-id="48188-290">Ez a tábla műveleti hivatkozásokkal rendelkezik egy új csapat létrehozása, egy új játékszezon lejátszása, a gyorsítótár kiürítése, a csapatok gyorsítótárból, több formátumban történő lekérdezése, a csapatok adatbázisból történő lekérdezése, valamint az adatbázis friss mintaadatokkal való újraépítése céljából.</span><span class="sxs-lookup"><span data-stu-id="48188-290">This table has action links for creating a new team, playing a new season of games, clearing the cache, retrieving the teams from the cache in several formats, retrieving the teams from the database, and rebuilding the database with fresh sample data.</span></span>

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


1. <span data-ttu-id="48188-291">Görgessen lefelé az **Index.cshtml** fájl aljához, és vegye fel az alábbi `tr` elemet, így ez lesz a fájl utolsó táblájának utolsó sora.</span><span class="sxs-lookup"><span data-stu-id="48188-291">Scroll to the bottom of the **Index.cshtml** file and add the following `tr` element so that it is the last row in the last table in the file.</span></span>
   
    ```html
    <tr><td colspan="5">@ViewBag.Msg</td></tr>
    ```
   
    <span data-ttu-id="48188-292">Ez a sor a `ViewBag.Msg` értékét jeleníti meg, amely az aktuális művelet állapotjelentését tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="48188-292">This row displays the value of `ViewBag.Msg` which contains a status report about the current operation.</span></span> <span data-ttu-id="48188-293">A `ViewBag.Msg` beállítása az előző lépés egyik műveleti hivatkozására kattintva történhet.</span><span class="sxs-lookup"><span data-stu-id="48188-293">The `ViewBag.Msg` is set when you click any of the action links from the previous step.</span></span>   
   
    ![Állapotüzenet][cache-status-message]
2. <span data-ttu-id="48188-295">A projekt létrehozásához nyomja le az **F6** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="48188-295">Press **F6** to build the project.</span></span>

## <a name="provision-the-azure-resources"></a><span data-ttu-id="48188-296">Azure-erőforrások kiépítése</span><span class="sxs-lookup"><span data-stu-id="48188-296">Provision the Azure resources</span></span>
<span data-ttu-id="48188-297">Az alkalmazásnak az Azure-on történő üzemeltetéséhez először is létre kell hoznia az alkalmazás számára szükséges Azure-szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="48188-297">To host your application in Azure, you must first provision the Azure services that your application requires.</span></span> <span data-ttu-id="48188-298">A jelen oktatóanyagban szereplő mintaalkalmazás az alábbi Azure-szolgáltatásokat használja.</span><span class="sxs-lookup"><span data-stu-id="48188-298">The sample application in this tutorial uses the following Azure services.</span></span>

* <span data-ttu-id="48188-299">Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="48188-299">Azure Redis Cache</span></span>
* <span data-ttu-id="48188-300">App Service webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="48188-300">App Service Web App</span></span>
* <span data-ttu-id="48188-301">SQL Database</span><span class="sxs-lookup"><span data-stu-id="48188-301">SQL Database</span></span>

<span data-ttu-id="48188-302">Ezen szolgáltatások új vagy létező, szabadon választott erőforráscsoporton történő üzembe helyezéséhez kattintson az alábbi **Deploy to Azure** (Üzembe helyezés az Azure-ban) gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-302">To deploy these services to a new or existing resource group of your choice, click the following **Deploy to Azure** button.</span></span>

<span data-ttu-id="48188-303">[![Deploy to Azure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span><span class="sxs-lookup"><span data-stu-id="48188-303">[![Deploy to Azure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)</span></span>

<span data-ttu-id="48188-304">Ez az **Deploy to Azure** (Üzembe helyezés az Azure-ban) gomb a [Webalkalmazás, Redis Cache és SQL Database egyidejű létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure gyors üzembe helyezés](https://github.com/Azure/azure-quickstart-templates) sablonját használja ezen szolgáltatások kiépítéséhez, illetve az SQL Database kapcsolati karakterláncának megadásához és az Azure Redis Cache-kapcsolatikarakterlánc alkalmazás-beállításához.</span><span class="sxs-lookup"><span data-stu-id="48188-304">This **Deploy to Azure** button uses the [Create a Web App plus Redis Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) template to provision these services and set the connection string for the SQL Database and the application setting for the Azure Redis Cache connection string.</span></span>

> [!NOTE]
> <span data-ttu-id="48188-305">Ha nincs Azure-fiókja, néhány perc alatt [létrehozhat egy ingyenes Azure-fiókot](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero).</span><span class="sxs-lookup"><span data-stu-id="48188-305">If you don't have an Azure account, you can [create a free Azure account](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) in just a couple of minutes.</span></span>
> 
> 

<span data-ttu-id="48188-306">Az **Üzembe helyezés az Azure-ban** gombra kattintva megnyílik az Azure portál, majd elindul a sablonban megadott erőforrások létrehozásának folyamata.</span><span class="sxs-lookup"><span data-stu-id="48188-306">Clicking the **Deploy to Azure** button takes you to the Azure portal and initiates the process of creating the resources described by the template.</span></span>

![Üzembe helyezés az Azure-ban][cache-deploy-to-azure-step-1]

1. <span data-ttu-id="48188-308">Az **Alapok** szekcióban válassza ki a használni kívánt Azure-előfizetést, jelöljön ki egy meglévő erőforráscsoportot vagy hozzon létre egy újat, majd adja meg az erőforráscsoport helyét.</span><span class="sxs-lookup"><span data-stu-id="48188-308">In the **Basics** section, select the Azure subscription to use, and select an existing resource group or create a new one, and specify the resource group location.</span></span>
2. <span data-ttu-id="48188-309">A **Beállítások** részben adja meg az **rendszergazdai bejelentkezési nevet** (ne használja az **admin** kifejezést), a **rendszergazdai bejelentkezési jelszót** és az **adatbázisnevet**.</span><span class="sxs-lookup"><span data-stu-id="48188-309">In the **Settings** section, specify an **Administrator Login** (don't use **admin**), **Administrator Login Password**, and **Database Name**.</span></span> <span data-ttu-id="48188-310">A többi paraméter egy ingyenes App Service futtatási csomaghoz van konfigurálva, valamint alacsonyabb költségszint érhető el az SQL Database és az Azure Redis Cache esetében, amelyek nem részei az ingyenes szintnek.</span><span class="sxs-lookup"><span data-stu-id="48188-310">The other parameters are configured for a free App Service hosting plan, and lower-cost options for the SQL Database and Azure Redis Cache, which don't come with a free tier.</span></span>

    ![Üzembe helyezés az Azure-ban][cache-deploy-to-azure-step-2]

3. <span data-ttu-id="48188-312">A kívánt beállítások konfigurálása után görgessen az oldal végére, olvassa el a feltételeket és kikötéseket, és pipálja ki az **Elfogadom a fenti feltételeket és kikötéseket** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="48188-312">After configuring the desired settings, scroll to the end of the page, read the terms and conditions, and check the **I agree to the terms and conditions stated above** checkbox.</span></span>
4. <span data-ttu-id="48188-313">Az erőforrások kiépítésének megkezdéséhez kattintson a **Vásárlás** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-313">To begin provisioning the resources, click **Purchase**.</span></span>

<span data-ttu-id="48188-314">Az üzembe helyezési folyamat előrehaladásának megtekintéséhez kattintson az értesítési ikonra, majd a **Központi telepítés elindítva** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-314">To view the progress of your deployment, click the notification icon and click **Deployment started**.</span></span>

![Központi telepítés elindítva][cache-deployment-started]

<span data-ttu-id="48188-316">A központi telepítés állapotát a **Microsoft.Template** panelen tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="48188-316">You can view the status of your deployment on the **Microsoft.Template** blade.</span></span>

![Üzembe helyezés az Azure-ban][cache-deploy-to-azure-step-3]

<span data-ttu-id="48188-318">A kiépítés után a Visual Studio felületéről közzéteheti alkalmazását az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="48188-318">When provisioning is complete, you can publish your application to Azure from Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="48188-319">A kiépítési folyamat során esetlegesen jelentkező hibák a **Microsoft.Template** panelen jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="48188-319">Any errors that may occur during the provisioning process are displayed on the **Microsoft.Template** blade.</span></span> <span data-ttu-id="48188-320">Gyakori hiba például az előfizetésenkénti túl sok SQL Server-példány és a túl sok Ingyenes App Service-futtatási csomag.</span><span class="sxs-lookup"><span data-stu-id="48188-320">Common errors are too many SQL Servers or too many Free App Service hosting plans per subscription.</span></span> <span data-ttu-id="48188-321">A hibák elhárítása és a folyamat újraindítása a **Microsoft.Template** panel **Ismételt üzembe helyezés** elemére, vagy a jelen oktatóanyag **Üzembe helyezés az Azure-ban** gombjára kattintva végezhető el.</span><span class="sxs-lookup"><span data-stu-id="48188-321">Resolve any errors and restart the process by clicking **Redeploy** on the **Microsoft.Template** blade or the **Deploy to Azure** button in this tutorial.</span></span>
> 
> 

## <a name="publish-the-application-to-azure"></a><span data-ttu-id="48188-322">Az alkalmazás közzététele az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="48188-322">Publish the application to Azure</span></span>
<span data-ttu-id="48188-323">Az oktatóanyag ezen lépésben közzéteszi alkalmazását az Azure-ban, majd futtatja azt a felhőben</span><span class="sxs-lookup"><span data-stu-id="48188-323">In this step of the tutorial, you'll publish the application to Azure and run it in the cloud.</span></span>

1. <span data-ttu-id="48188-324">Kattintson a jobb gombbal a Visual Studio **ContosoTeamStats** projektjére, majd válassza a **Publish** (Közzététel) lehetőséget</span><span class="sxs-lookup"><span data-stu-id="48188-324">Right-click the **ContosoTeamStats** project in Visual Studio and choose **Publish**.</span></span>
   
    ![Közzététel][cache-publish-app]
2. <span data-ttu-id="48188-326">Kattintson a **Microsoft Azure App Service** lehetőségre, válassza a **Meglévő kiválasztása** elemet, majd kattintson a **Közzététel** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-326">Click **Microsoft Azure App Service**, choose **Select Existing**, and click **Publish**.</span></span>
   
    ![Közzététel][cache-publish-to-app-service]
3. <span data-ttu-id="48188-328">Válassza ki az Azure-erőforrások létrehozásakor használt előfizetést, bontsa ki az erőforrásokat tartalmazó erőforráscsoportot, és válassza ki a kívánt webappot.</span><span class="sxs-lookup"><span data-stu-id="48188-328">Select the subscription used when creating the Azure resources, expand the resource group containing the resources, and select the desired Web App.</span></span> <span data-ttu-id="48188-329">Ha az **Üzembe helyezés az Azure-ban** gombot használta, a webalkalmazás neve a **webSite** kifejezéssel kezdődik, amit néhány további karakter követ.</span><span class="sxs-lookup"><span data-stu-id="48188-329">If you used the **Deploy to Azure** button your Web App name starts with **webSite** followed by some additional characters.</span></span>
   
    ![Webalkalmazás kiválasztása][cache-select-web-app]
4. <span data-ttu-id="48188-331">A közzétételi folyamat elindításához kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-331">Click **OK** to begin the publishing process.</span></span> <span data-ttu-id="48188-332">A közzétételi folyamat néhány pillanat múlva befejeződik, és a elindul böngésző a futó mintaalkalmazással együtt.</span><span class="sxs-lookup"><span data-stu-id="48188-332">After a few moments the publishing process completes and a browser is launched with the running sample application.</span></span> <span data-ttu-id="48188-333">Ha érvényesítés vagy közzététel közben a rendszer DNS-hibát ad vissza, az alkalmazáshoz tartozó Azure-erőforrások kiépítési folyamata pedig csak az imént fejeződött be, várjon egy kicsit, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="48188-333">If you get a DNS error when validating or publishing, and the provisioning process for the Azure resources for the application has just recently completed, wait a moment and try again.</span></span>
   
    ![Gyorsítótár hozzáadva][cache-added-to-application]

<span data-ttu-id="48188-335">A mintaalkalmazás egyes műveleti hivatkozásait a következő táblázat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="48188-335">The following table describes each action link from the sample application.</span></span>

| <span data-ttu-id="48188-336">Műveletek</span><span class="sxs-lookup"><span data-stu-id="48188-336">Action</span></span> | <span data-ttu-id="48188-337">Leírás</span><span class="sxs-lookup"><span data-stu-id="48188-337">Description</span></span> |
| --- | --- |
| <span data-ttu-id="48188-338">Új létrehozása</span><span class="sxs-lookup"><span data-stu-id="48188-338">Create New</span></span> |<span data-ttu-id="48188-339">Létrehoz egy új csapatot.</span><span class="sxs-lookup"><span data-stu-id="48188-339">Create a new Team.</span></span> |
| <span data-ttu-id="48188-340">Szezon végigjátszása</span><span class="sxs-lookup"><span data-stu-id="48188-340">Play Season</span></span> |<span data-ttu-id="48188-341">Végigjátszik egy szezont, frissíti a csapatstatisztikákat, és törli a gyorsítótárból az elavult adatokat.</span><span class="sxs-lookup"><span data-stu-id="48188-341">Play a season of games, update the team stats, and clear any outdated team data from the cache.</span></span> |
| <span data-ttu-id="48188-342">Gyorsítótár ürítése</span><span class="sxs-lookup"><span data-stu-id="48188-342">Clear Cache</span></span> |<span data-ttu-id="48188-343">Törli a csapatstatisztikákat a gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="48188-343">Clear the team stats from the cache.</span></span> |
| <span data-ttu-id="48188-344">Lista a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="48188-344">List from Cache</span></span> |<span data-ttu-id="48188-345">Lekérdezi a csapatstatisztikákat a gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="48188-345">Retrieve the team stats from the cache.</span></span> <span data-ttu-id="48188-346">Gyorsítótár-tévesztés esetén az adatbázisból tölti be a statisztikákat, és menti azt a gyorsítótárba a következő alkalomra.</span><span class="sxs-lookup"><span data-stu-id="48188-346">If there is a cache miss, load the stats from the database and save to the cache for next time.</span></span> |
| <span data-ttu-id="48188-347">Rendezett készlet a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="48188-347">Sorted Set from Cache</span></span> |<span data-ttu-id="48188-348">Lekérdezi a csapatstatisztikákat a gyorsítótárból egy rendezett készlet használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-348">Retrieve the team stats from the cache using a sorted set.</span></span> <span data-ttu-id="48188-349">Gyorsítótár-tévesztés esetén az adatbázisból tölti be a statisztikákat, és menti azt a gyorsítótárba egy rendezett készlet használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-349">If there is a cache miss, load the stats from the database and save to the cache using a sorted set.</span></span> |
| <span data-ttu-id="48188-350">Az 5 legjobb csapat a gyorsítótárból</span><span class="sxs-lookup"><span data-stu-id="48188-350">Top 5 Teams from Cache</span></span> |<span data-ttu-id="48188-351">Lekérdezi az 5 legjobb csapatot a gyorsítótárból egy rendezett készlet használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-351">Retrieve the top 5 teams from the cache using a sorted set.</span></span> <span data-ttu-id="48188-352">Gyorsítótár-tévesztés esetén az adatbázisból tölti be a statisztikákat, és menti azt a gyorsítótárba egy rendezett készlet használatával.</span><span class="sxs-lookup"><span data-stu-id="48188-352">If there is a cache miss, load the stats from the database and save to the cache using a sorted set.</span></span> |
| <span data-ttu-id="48188-353">Betöltés adatbázisból</span><span class="sxs-lookup"><span data-stu-id="48188-353">Load from DB</span></span> |<span data-ttu-id="48188-354">Lekérdezi a csapatstatisztikákat az adatbázisból.</span><span class="sxs-lookup"><span data-stu-id="48188-354">Retrieve the team stats from the database.</span></span> |
| <span data-ttu-id="48188-355">Adatbázis újraépítése</span><span class="sxs-lookup"><span data-stu-id="48188-355">Rebuild DB</span></span> |<span data-ttu-id="48188-356">Újraépíti az adatbázist, és ismét feltölti azt minta-csapatadatokkal.</span><span class="sxs-lookup"><span data-stu-id="48188-356">Rebuild the database and reload it with sample team data.</span></span> |
| <span data-ttu-id="48188-357">Szerkesztés / Részletek / Törlés</span><span class="sxs-lookup"><span data-stu-id="48188-357">Edit / Details / Delete</span></span> |<span data-ttu-id="48188-358">Szerkeszthet egy csapatot, megtekintheti annak részletes adatait, törölhet egy csapatot.</span><span class="sxs-lookup"><span data-stu-id="48188-358">Edit a team, view details for a team, delete a team.</span></span> |

<span data-ttu-id="48188-359">Kattintson néhány műveletre, és kísérletezzen az adatok különböző forrásokból történő lekérdezésével.</span><span class="sxs-lookup"><span data-stu-id="48188-359">Click some of the actions and experiment with retrieving the data from the different sources.</span></span> <span data-ttu-id="48188-360">Figyelje meg az adatbázisból és a gyorsítótárból történő adatlekérdezés különböző módjainak végrehajtásához szükséges időbeli eltéréseket.</span><span class="sxs-lookup"><span data-stu-id="48188-360">Not the differences in the time it takes to complete the various ways of retrieving the data from the database and the cache.</span></span>

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a><span data-ttu-id="48188-361">Az erőforrások törlése az alkalmazás bezárását követően</span><span class="sxs-lookup"><span data-stu-id="48188-361">Delete the resources when you are finished with the application</span></span>
<span data-ttu-id="48188-362">Ha befejezte az oktatóanyag mintaalkalmazásának használatát, a költség- és erőforrás-takarékosság érdekében törölheti az ott használt Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="48188-362">When you are finished with the sample tutorial application, you can delete the Azure resources used in order to conserve cost and resources.</span></span> <span data-ttu-id="48188-363">Ha a [Azure-erőforrások kiépítése](#provision-the-azure-resources) szakasz **Üzembe helyezés az Azure-ban** gombját használja, és valamennyi erőforrás azonos erőforráscsoportban található, az erőforráscsoport törlésével egy művelettel, együttesen is törölheti azokat.</span><span class="sxs-lookup"><span data-stu-id="48188-363">If you use the **Deploy to Azure** button in the [Provision the Azure resources](#provision-the-azure-resources) section and all of your resources are contained in the same resource group, you can delete them together in one operation by deleting the resource group.</span></span>

1. <span data-ttu-id="48188-364">Jelentkezzen be az [Azure portálra](https://portal.azure.com), és kattintson az **Erőforráscsoportok** elemre.</span><span class="sxs-lookup"><span data-stu-id="48188-364">Sign in to the [Azure portal](https://portal.azure.com) and click **Resource groups**.</span></span>
2. <span data-ttu-id="48188-365">Írja be az erőforráscsoport nevét az **Elemek szűrése...** szövegmezőbe.</span><span class="sxs-lookup"><span data-stu-id="48188-365">Type the name of your resource group into the **Filter items...** textbox.</span></span>
3. <span data-ttu-id="48188-366">Kattintson az erőforráscsoporttól jobbra lévő **...** elemre.</span><span class="sxs-lookup"><span data-stu-id="48188-366">Click **...** to the right of your resource group.</span></span>
4. <span data-ttu-id="48188-367">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-367">Click **Delete**.</span></span>
   
    ![Törlés][cache-delete-resource-group]
5. <span data-ttu-id="48188-369">Írja be az erőforráscsoport nevét, és kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="48188-369">Type the name of your resource group and click **Delete**.</span></span>
   
    ![Törlés megerősítése][cache-delete-confirm]

<span data-ttu-id="48188-371">A rendszer néhány pillanaton belül törli az erőforráscsoportot és a benne foglalt erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="48188-371">After a few moments the resource group and all of its contained resources are deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48188-372">Ügyeljen arra, hogy az erőforráscsoport törlése nem visszaállítható; az erőforráscsoport és a benne foglalt erőforrások véglegesen törlődnek.</span><span class="sxs-lookup"><span data-stu-id="48188-372">Note that deleting a resource group is irreversible and that the resource group and all the resources in it are permanently deleted.</span></span> <span data-ttu-id="48188-373">Figyeljen arra, hogy ne töröljön véletlenül erőforráscsoportot vagy erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="48188-373">Make sure that you do not accidentally delete the wrong resource group or resources.</span></span> <span data-ttu-id="48188-374">Ha a jelen minta üzemeltetését végző erőforrásokat egy meglévő erőforráscsoportban hozta létre, az erőforrásokat külön-külön törölheti a megfelelő panelekről.</span><span class="sxs-lookup"><span data-stu-id="48188-374">If you created the resources for hosting this sample inside an existing resource group, you can delete each resource individually from their respective blades.</span></span>
> 
> 

## <a name="run-the-sample-application-on-your-local-machine"></a><span data-ttu-id="48188-375">Mintaalkalmazás futtatása helyi gépen</span><span class="sxs-lookup"><span data-stu-id="48188-375">Run the sample application on your local machine</span></span>
<span data-ttu-id="48188-376">Az alkalmazás helyi számítógépen történő futtatásához egy olyan Azure Redis Cache-példányra van szükség, amelyen az adatok gyorsítótárazása elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="48188-376">To run the application locally on your machine, you need an Azure Redis Cache instance in which to cache your data.</span></span> 

* <span data-ttu-id="48188-377">Ha az alkalmazás Azure-on történő közzétételét az előző szakaszban leírt módon hajtotta végre, használhatja az abban a lépésben üzembe helyezett Azure Redis Cache-példányt.</span><span class="sxs-lookup"><span data-stu-id="48188-377">If you have published your application to Azure as described in the previous section, you can use the Azure Redis Cache instance that was provisioned during that step.</span></span>
* <span data-ttu-id="48188-378">Ha rendelkezik egy másik meglévő Azure Redis Cache-példánnyal, használhatja azt ezen minta helyi futtatásához.</span><span class="sxs-lookup"><span data-stu-id="48188-378">If you have another existing Azure Redis Cache instance, you can use that to run this sample locally.</span></span>
* <span data-ttu-id="48188-379">Amennyiben létre kell hoznia egy Azure Redis Cache-példányt, ennek műveleti lépéseit megtalálja a [Gyorsítótár létrehozása](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) részben.</span><span class="sxs-lookup"><span data-stu-id="48188-379">If you need to create an Azure Redis Cache instance, you can follow the steps in [Create a cache](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).</span></span>

<span data-ttu-id="48188-380">A használni kívánt gyorsítótár kiválasztása vagy létrehozása után keresse meg azt az Azure portálon, majd kérje le a hozzá tartozó [állomásnév](cache-configure.md#properties) és [hívóbetű](cache-configure.md#access-keys) paramétereket.</span><span class="sxs-lookup"><span data-stu-id="48188-380">Once you have selected or created the cache to use, browse to the cache in the Azure portal and retrieve the [host name](cache-configure.md#properties) and [access keys](cache-configure.md#access-keys) for your cache.</span></span> <span data-ttu-id="48188-381">Útmutatásért lásd: [A Redis Cache-gyorsítótár beállításai](cache-configure.md#configure-redis-cache-settings).</span><span class="sxs-lookup"><span data-stu-id="48188-381">For instructions, see [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings).</span></span>

1. <span data-ttu-id="48188-382">A kívánt szerkesztővel nyissa meg a jelem oktatóanyag [Az alkalmazás konfigurálása a Redis Cache használatára](#configure-the-application-to-use-redis-cache) lépésében létrehozott `WebAppPlusCacheAppSecrets.config` fájlt.</span><span class="sxs-lookup"><span data-stu-id="48188-382">Open the `WebAppPlusCacheAppSecrets.config` file that you created during the [Configure the application to use Redis Cache](#configure-the-application-to-use-redis-cache) step of this tutorial using the editor of your choice.</span></span>
2. <span data-ttu-id="48188-383">Módosítsa a `value` attribútumot, és cserélje le a `MyCache.redis.cache.windows.net` elemet a gyorsítótár [állomásnevével](cache-configure.md#properties), majd jelszóként adja meg a gyorsítótár [elsődleges vagy másodlagos kulcsát](cache-configure.md#access-keys).</span><span class="sxs-lookup"><span data-stu-id="48188-383">Edit the `value` attribute and replace `MyCache.redis.cache.windows.net` with the [host name](cache-configure.md#properties) of your cache, and specify either the [primary or secondary key](cache-configure.md#access-keys) of your cache as the password.</span></span>

    ```xml
    <appSettings>
      <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
    </appSettings>
    ```


1. <span data-ttu-id="48188-384">Az alkalmazás futtatásához nyomja le a **Ctrl+F5** billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="48188-384">Press **Ctrl+F5** to run the application.</span></span>

> [!NOTE]
> <span data-ttu-id="48188-385">Vegye figyelembe, hogy mivel az alkalmazás (beleértve az adatbázist is) futtatása helyileg történik, a Redis Cache üzemeltetését pedig az Azure végzi, a gyorsítótár teljesítménye az adatbázisénál kisebbnek tűnhet.</span><span class="sxs-lookup"><span data-stu-id="48188-385">Note that because the application, including the database, is running locally and the Redis Cache is hosted in Azure, the cache may appear to under-perform the database.</span></span> <span data-ttu-id="48188-386">A legjobb teljesítmény érdekében az ügyfélalkalmazásnak és az Azure Redis Cache-példánynak azonos helyen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="48188-386">For best performance, the client application and Azure Redis Cache instance should be in the same location.</span></span> 
> 
> 

## <a name="next-steps"></a><span data-ttu-id="48188-387">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48188-387">Next steps</span></span>
* <span data-ttu-id="48188-388">Az [ASP.NET MVC 5 – Első lépések](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) elvégzéséről további információkat az [ASP.NET](http://asp.net/) webhelyén talál.</span><span class="sxs-lookup"><span data-stu-id="48188-388">Learn more about [Getting Started with ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) on the [ASP.NET](http://asp.net/) site.</span></span>
* <span data-ttu-id="48188-389">További példák egy ASP.NET-webalkalmazás létrehozására az App Service szolgáltatásban: [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) (ASP.NET-webalkalmazás létrehozása és üzembe helyezése az Azure App Service szolgáltatásban) a [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [bemutatóból](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span><span class="sxs-lookup"><span data-stu-id="48188-389">For more examples of creating an ASP.NET Web App in App Service, see [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) from the [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz) 2015 Connect [demo](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/).</span></span>
  * <span data-ttu-id="48188-390">A HealthClinic.biz bemutató további gyors útmutatóit lásd: [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Azure fejlesztői eszközök – gyors útmutatók).</span><span class="sxs-lookup"><span data-stu-id="48188-390">For more quickstarts from the HealthClinic.biz demo, see [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts).</span></span>
* <span data-ttu-id="48188-391">Itt további információkat talál a jelen oktatóanyagban használt, [Code first to a new database](https://msdn.microsoft.com/data/jj193542) (Code First alkalmazása egy új adatbázisra) nevű Entity Framework-megközelítésról.</span><span class="sxs-lookup"><span data-stu-id="48188-391">Learn more about the [Code first to a new database](https://msdn.microsoft.com/data/jj193542) approach to Entity Framework that's used in this tutorial.</span></span>
* <span data-ttu-id="48188-392">További információ [az Azure App Service webalkalmazásairól](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="48188-392">Learn more about [web apps in Azure App Service](../app-service-web/app-service-web-overview.md).</span></span>
* <span data-ttu-id="48188-393">Tudnivalók a gyorsítótár [figyeléséről](cache-how-to-monitor.md) az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="48188-393">Learn how to [monitor](cache-how-to-monitor.md) your cache in the Azure portal.</span></span>
* <span data-ttu-id="48188-394">Az Azure Redis Cache prémium funkcióinak megismerése</span><span class="sxs-lookup"><span data-stu-id="48188-394">Explore Azure Redis Cache premium features</span></span>
  
  * [<span data-ttu-id="48188-395">Adatmegőrzés konfigurálása prémium szintű Azure Redis Cache-gyorsítótárhoz</span><span class="sxs-lookup"><span data-stu-id="48188-395">How to configure persistence for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-persistence.md)
  * [<span data-ttu-id="48188-396">Fürtözés konfigurálása prémium szintű Azure Redis Cache-gyorsítótárhoz</span><span class="sxs-lookup"><span data-stu-id="48188-396">How to configure clustering for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-clustering.md)
  * [<span data-ttu-id="48188-397">Virtuális hálózat támogatásának konfigurálása prémium szintű Azure Redis Cache-gyorsítótárhoz</span><span class="sxs-lookup"><span data-stu-id="48188-397">How to configure Virtual Network support for a Premium Azure Redis Cache</span></span>](cache-how-to-premium-vnet.md)
  * <span data-ttu-id="48188-398">További részletes információk a prémium gyorsítótárak méretével, teljesítményével és a sávszélességével kapcsolatban: [Azure Redis Cache – Gyakori kérdések](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="48188-398">See the [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) for more details about size, throughput, and bandwidth with premium caches.</span></span>

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

