---
title: "Azure SQL Database és az ASP.NET alkalmazás létrehozása |} Microsoft Docs"
description: "Ismerje meg, hogyan kérhet egy ASP.NET-alkalmazást az Azure SQL adatbázis-kapcsolat használata."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: c22b8ef4866fe2f1ae32c7cb9158fc7866788b26
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a><span data-ttu-id="37c8c-103">Azure SQL Database és az ASP.NET alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="37c8c-103">Build an ASP.NET app in Azure with SQL Database</span></span>

<span data-ttu-id="37c8c-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="37c8c-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="37c8c-105">Az oktatóanyag bemutatja, hogyan telepítheti az adatvezérelt ASP.NET webalkalmazás az Azure-ban, és kösse össze [Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="37c8c-105">This tutorial shows you how to deploy a data-driven ASP.NET web app in Azure and connect it to [Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span></span> <span data-ttu-id="37c8c-106">Amikor végzett, hogy egy ASP.NET-alkalmazás fusson az [Azure App Service](../app-service/app-service-value-prop-what-is.md) és az SQL-adatbázishoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="37c8c-106">When you're finished, you have a ASP.NET app running in [Azure App Service](../app-service/app-service-value-prop-what-is.md) and connected to SQL Database.</span></span>

![Közzétett ASP.NET-alkalmazást az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="37c8c-108">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="37c8c-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37c8c-109">SQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="37c8c-109">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="37c8c-110">ASP.NET alkalmazás csatlakoztatása az SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="37c8c-110">Connect an ASP.NET app to SQL Database</span></span>
> * <span data-ttu-id="37c8c-111">Az alkalmazás telepítése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="37c8c-111">Deploy the app to Azure</span></span>
> * <span data-ttu-id="37c8c-112">Az adatmodell frissítése, és telepítse újra az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="37c8c-112">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="37c8c-113">Stream naplók az Azure-ból a Terminálszolgáltatások számára</span><span class="sxs-lookup"><span data-stu-id="37c8c-113">Stream logs from Azure to your terminal</span></span>
> * <span data-ttu-id="37c8c-114">Felügyelheti az alkalmazást az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="37c8c-114">Manage the app in the Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="37c8c-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="37c8c-115">Prerequisites</span></span>

<span data-ttu-id="37c8c-116">Az oktatóanyag elvégzéséhez:</span><span class="sxs-lookup"><span data-stu-id="37c8c-116">To complete this tutorial:</span></span>

* <span data-ttu-id="37c8c-117">Telepítse a [Visual Studio 2017](https://www.visualstudio.com/downloads/) szoftvert a következő számítási feladatokkal:</span><span class="sxs-lookup"><span data-stu-id="37c8c-117">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with the following workloads:</span></span>
  - <span data-ttu-id="37c8c-118">**ASP.NET és webfejlesztés**</span><span class="sxs-lookup"><span data-stu-id="37c8c-118">**ASP.NET and web development**</span></span>
  - <span data-ttu-id="37c8c-119">**Azure-fejlesztés**</span><span class="sxs-lookup"><span data-stu-id="37c8c-119">**Azure development**</span></span>

  ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a><span data-ttu-id="37c8c-121">A minta letöltése</span><span class="sxs-lookup"><span data-stu-id="37c8c-121">Download the sample</span></span>

<span data-ttu-id="37c8c-122">[A minta-projekt letöltése](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="37c8c-122">[Download the sample project](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span></span>

<span data-ttu-id="37c8c-123">Extract (csomagolja ki) a *dotnet-sqldb-oktatóanyag – master.zip* fájlt.</span><span class="sxs-lookup"><span data-stu-id="37c8c-123">Extract (unzip) the  *dotnet-sqldb-tutorial-master.zip* file.</span></span>

<span data-ttu-id="37c8c-124">A minta-projekt tartalmazza az alapvető [ASP.NET MVC](https://www.asp.net/mvc) CRUD (hozzon létre-olvasás-Módosítás-Törlés) alkalmazás használatával [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="37c8c-124">The sample project contains a basic [ASP.NET MVC](https://www.asp.net/mvc) CRUD (create-read-update-delete) app using [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="run-the-app"></a><span data-ttu-id="37c8c-125">Az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="37c8c-125">Run the app</span></span>

<span data-ttu-id="37c8c-126">Nyissa meg a *dotnet-sqldb-oktatóanyag – master/DotNetAppSqlDb.sln* fájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="37c8c-126">Open the *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* file in Visual Studio.</span></span> 

<span data-ttu-id="37c8c-127">Típus `Ctrl+F5` hibakeresés nélkül az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="37c8c-127">Type `Ctrl+F5` to run the app without debugging.</span></span> <span data-ttu-id="37c8c-128">Az alkalmazás az alapértelmezett böngésző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="37c8c-128">The app is displayed in your default browser.</span></span> <span data-ttu-id="37c8c-129">Válassza ki a **hozzon létre új** hivatkozásra, és hozzon létre több *tennivaló* elemeket.</span><span class="sxs-lookup"><span data-stu-id="37c8c-129">Select the **Create New** link and create a couple *to-do* items.</span></span> 

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

<span data-ttu-id="37c8c-131">Teszt a **szerkesztése**, **részletek**, és **törlése** hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="37c8c-131">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="37c8c-132">Az alkalmazás egy adatbázis-környezet használatával kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="37c8c-132">The app uses a database context to connect with the database.</span></span> <span data-ttu-id="37c8c-133">Ez a példa az adatbázis-környezet nevű kapcsolati karakterláncot használ `MyDbConnection`.</span><span class="sxs-lookup"><span data-stu-id="37c8c-133">In this sample, the database context uses a connection string named `MyDbConnection`.</span></span> <span data-ttu-id="37c8c-134">A kapcsolati karakterlánc megadása a *Web.config* fájlt, és hivatkozik a *Models/MyDatabaseContext.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="37c8c-134">The connection string is set in the *Web.config* file and referenced in the *Models/MyDatabaseContext.cs* file.</span></span> <span data-ttu-id="37c8c-135">A kapcsolati karakterlánc nevét az Azure web app egy Azure SQL adatbázishoz való csatlakozáshoz használt az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="37c8c-135">The connection string name is used later in the tutorial to connect the Azure web app to an Azure SQL Database.</span></span> 

## <a name="publish-to-azure-with-sql-database"></a><span data-ttu-id="37c8c-136">Az Azure SQL Database közzététele</span><span class="sxs-lookup"><span data-stu-id="37c8c-136">Publish to Azure with SQL Database</span></span>

<span data-ttu-id="37c8c-137">Az a **Megoldáskezelőben**, kattintson a jobb gombbal a **DotNetAppSqlDb** projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-137">In the **Solution Explorer**, right-click your **DotNetAppSqlDb** project and select **Publish**.</span></span>

![Közzététel a Megoldáskezelőből](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

<span data-ttu-id="37c8c-139">Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="37c8c-139">Make sure that **Microsoft Azure App Service** is selected and click **Publish**.</span></span>

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

<span data-ttu-id="37c8c-141">Közzétételi megnyílik a **létrehozása az App Service** párbeszédpanel, amely segít az összes Azure-erőforrást az Azure-ban az ASP.NET webalkalmazás futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="37c8c-141">Publishing opens the **Create App Service** dialog, which helps you create all the Azure resources you need to run your ASP.NET web app in Azure.</span></span>

### <a name="sign-in-to-azure"></a><span data-ttu-id="37c8c-142">Bejelentkezés az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="37c8c-142">Sign in to Azure</span></span>

<span data-ttu-id="37c8c-143">A **Create App Service** (App Service létrehozása) párbeszédpanelen kattintson az **Add an account** (Fiók hozzáadása) gombra, majd jelentkezzen be az Azure-előfizetésébe.</span><span class="sxs-lookup"><span data-stu-id="37c8c-143">In the **Create App Service** dialog, click **Add an account**, and then sign in to your Azure subscription.</span></span> <span data-ttu-id="37c8c-144">Ha már be van jelentkezve egy Microsoft-fiókba, győződjön meg arról, hogy abban a fiókban található az előfizetése.</span><span class="sxs-lookup"><span data-stu-id="37c8c-144">If you're already signed into a Microsoft account, make sure that account holds your Azure subscription.</span></span> <span data-ttu-id="37c8c-145">Ha nem abban a Microsoft-fiókban van az Azure-előfizetése, amelyikbe be van jelentkezve, kattintással adja hozzá a helyes fiókot.</span><span class="sxs-lookup"><span data-stu-id="37c8c-145">If the signed-in Microsoft account doesn't have your Azure subscription, click it to add the correct account.</span></span>
   
![Bejelentkezés az Azure-ba](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

<span data-ttu-id="37c8c-147">Ha már bejelentkezett, ezen a panelen már létre is hozhatja az összes, az Azure-webapphoz szükséges erőforrást.</span><span class="sxs-lookup"><span data-stu-id="37c8c-147">Once signed in, you're ready to create all the resources you need for your Azure web app in this dialog.</span></span>

### <a name="configure-the-web-app-name"></a><span data-ttu-id="37c8c-148">A webalkalmazás nevének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37c8c-148">Configure the web app name</span></span>

<span data-ttu-id="37c8c-149">Megtartja-e a létrehozott webalkalmazás neve, vagy módosítsa azt egy másik egyedi nevét (érvényes karakterek: `a-z`, `0-9`, és `-`).</span><span class="sxs-lookup"><span data-stu-id="37c8c-149">You can keep the generated web app name, or change it to another unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="37c8c-150">A webalkalmazás-nevet az alkalmazás használatos az alapértelmezett URL-cím (`<app_name>.azurewebsites.net`, ahol `<app_name>` a webes alkalmazás neve).</span><span class="sxs-lookup"><span data-stu-id="37c8c-150">The web app name is used as part of the default URL for your app (`<app_name>.azurewebsites.net`, where `<app_name>` is your web app name).</span></span> <span data-ttu-id="37c8c-151">A webalkalmazás nevének egyedinek kell lennie az Azure-ban minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="37c8c-151">The web app name needs to be unique across all apps in Azure.</span></span> 

![Hozzon létre az app service párbeszédpanelen](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a><span data-ttu-id="37c8c-153">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="37c8c-153">Create a resource group</span></span>

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="37c8c-154">A **Resource Group** (Erőforráscsoport) mellett kattintson a **New** (Új) elemre.</span><span class="sxs-lookup"><span data-stu-id="37c8c-154">Next to **Resource Group**, click **New**.</span></span>

![Erőforráscsoport mellett az új gombra.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

<span data-ttu-id="37c8c-156">Nevezze el az erőforráscsoportot **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-156">Name the resource group **myResourceGroup**.</span></span>

> [!NOTE]
> <span data-ttu-id="37c8c-157">Ne kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-157">Do not click **Create**.</span></span> <span data-ttu-id="37c8c-158">Először egy későbbi lépésben egy SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37c8c-158">You first need to set up a SQL Database in a later step.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="37c8c-159">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="37c8c-159">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="37c8c-160">Az **App Service Plan** (App Service-csomag) mellett kattintson a **New** (Új) elemre.</span><span class="sxs-lookup"><span data-stu-id="37c8c-160">Next to **App Service Plan**, click **New**.</span></span> 

<span data-ttu-id="37c8c-161">A **Configure App Service Plan** (App Service-csomag konfigurálása) párbeszédpanelen konfigurálja az új App Service-csomagot a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="37c8c-161">In the **Configure App Service Plan** dialog, configure the new App Service plan with the following settings:</span></span>

![App Service-csomag létrehozása](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| <span data-ttu-id="37c8c-163">Beállítás</span><span class="sxs-lookup"><span data-stu-id="37c8c-163">Setting</span></span>  | <span data-ttu-id="37c8c-164">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="37c8c-164">Suggested value</span></span> | <span data-ttu-id="37c8c-165">További információ</span><span class="sxs-lookup"><span data-stu-id="37c8c-165">For more information</span></span> |
| ----------------- | ------------ | ----|
|<span data-ttu-id="37c8c-166">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="37c8c-166">**App Service Plan**</span></span>| <span data-ttu-id="37c8c-167">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="37c8c-167">myAppServicePlan</span></span> | [<span data-ttu-id="37c8c-168">App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="37c8c-168">App Service plans</span></span>](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|<span data-ttu-id="37c8c-169">**Hely**</span><span class="sxs-lookup"><span data-stu-id="37c8c-169">**Location**</span></span>| <span data-ttu-id="37c8c-170">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="37c8c-170">West Europe</span></span> | [<span data-ttu-id="37c8c-171">Azure-régiók</span><span class="sxs-lookup"><span data-stu-id="37c8c-171">Azure regions</span></span>](https://azure.microsoft.com/regions/) |
|<span data-ttu-id="37c8c-172">**Méret**</span><span class="sxs-lookup"><span data-stu-id="37c8c-172">**Size**</span></span>| <span data-ttu-id="37c8c-173">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="37c8c-173">Free</span></span> | [<span data-ttu-id="37c8c-174">Tarifacsomagok</span><span class="sxs-lookup"><span data-stu-id="37c8c-174">Pricing tiers</span></span>](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a><span data-ttu-id="37c8c-175">SQL Server-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="37c8c-175">Create a SQL Server instance</span></span>

<span data-ttu-id="37c8c-176">Adatbázis létrehozása előtt kell egy [Azure SQL Database logikai kiszolgáló](../sql-database/sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="37c8c-176">Before creating a database, you need an [Azure SQL Database logical server](../sql-database/sql-database-features.md).</span></span> <span data-ttu-id="37c8c-177">A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="37c8c-177">A logical server contains a group of databases managed as a group.</span></span>

<span data-ttu-id="37c8c-178">Válassza ki **további Azure-szolgáltatások felfedezés**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-178">Select **Explore additional Azure services**.</span></span>

![A webapp nevének konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

<span data-ttu-id="37c8c-180">Az a **szolgáltatások** lapra, majd a  **+**  melletti ikon **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-180">In the **Services** tab, click the **+** icon next to **SQL Database**.</span></span> 

![A szolgáltatások lapon kattintson a + SQL-adatbázis melletti ikonra.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

<span data-ttu-id="37c8c-182">Az a **SQL-adatbázis beállítása** párbeszédpanel, kattintson a **új** melletti **SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-182">In the **Configure SQL Database** dialog, click **New** next to **SQL Server**.</span></span> 

<span data-ttu-id="37c8c-183">Létrejön egy egyedi kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="37c8c-183">A unique server name is generated.</span></span> <span data-ttu-id="37c8c-184">Ez a név részeként az alapértelmezett URL-cím a logikai kiszolgálóhoz használt `<server_name>.database.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="37c8c-184">This name is used as part of the default URL for your logical server, `<server_name>.database.windows.net`.</span></span> <span data-ttu-id="37c8c-185">Egyedinek kell lennie az Azure-ban minden logikai kiszolgáló példányára.</span><span class="sxs-lookup"><span data-stu-id="37c8c-185">It must be unique across all logical server instances in Azure.</span></span> <span data-ttu-id="37c8c-186">Módosítsa a kiszolgáló nevét, de ebben az oktatóanyagban tartsa meg a létrehozott értéket.</span><span class="sxs-lookup"><span data-stu-id="37c8c-186">You can change the server name, but for this tutorial, keep the generated value.</span></span>

<span data-ttu-id="37c8c-187">Adja hozzá egy rendszergazdai jogosultságú felhasználónevet és jelszót, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-187">Add an administrator username and password, and then select **OK**.</span></span> <span data-ttu-id="37c8c-188">Összetettségi követelményeknek, lásd: [jelszóházirend](/sql/relational-databases/security/password-policy).</span><span class="sxs-lookup"><span data-stu-id="37c8c-188">For password complexity requirements, see [Password Policy](/sql/relational-databases/security/password-policy).</span></span>

<span data-ttu-id="37c8c-189">Ne felejtse el ezt a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="37c8c-189">Remember this username and password.</span></span> <span data-ttu-id="37c8c-190">Azokat a logikai kiszolgálópéldány később kezelésére van szüksége.</span><span class="sxs-lookup"><span data-stu-id="37c8c-190">You need them to manage the logical server instance later.</span></span>

![SQL Server-példány létrehozása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a><span data-ttu-id="37c8c-192">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="37c8c-192">Create a SQL Database</span></span>

<span data-ttu-id="37c8c-193">Az a **SQL-adatbázis beállítása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="37c8c-193">In the **Configure SQL Database** dialog:</span></span> 

* <span data-ttu-id="37c8c-194">Tartsa meg az alapértelmezett generált **adatbázisnév**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-194">Keep the default generated **Database Name**.</span></span>
* <span data-ttu-id="37c8c-195">A **kapcsolati karakterlánc nevét**, típus *MyDbConnection*.</span><span class="sxs-lookup"><span data-stu-id="37c8c-195">In **Connection String Name**, type *MyDbConnection*.</span></span> <span data-ttu-id="37c8c-196">Ezt a nevet meg kell egyeznie a kapcsolati karakterláncot, amely hivatkozik a *Models/MyDatabaseContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="37c8c-196">This name must match the connection string that is referenced in *Models/MyDatabaseContext.cs*.</span></span>
* <span data-ttu-id="37c8c-197">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="37c8c-197">Select **OK**.</span></span>

![SQL-adatbázis konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

<span data-ttu-id="37c8c-199">A **létrehozása az App Service** párbeszédpanel jeleníti meg az erőforrások létrehozott.</span><span class="sxs-lookup"><span data-stu-id="37c8c-199">The **Create App Service** dialog shows the resources you've created.</span></span> <span data-ttu-id="37c8c-200">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="37c8c-200">Click **Create**.</span></span> 

![a létrehozott erőforrások](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

<span data-ttu-id="37c8c-202">Miután a varázsló létrehozta az Azure-erőforrások, az Azure-bA közzéteszi az ASP.NET-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="37c8c-202">Once the wizard finishes creating the Azure resources, it  publishes your ASP.NET app to Azure.</span></span> <span data-ttu-id="37c8c-203">Az alapértelmezett böngésző indították el a telepített alkalmazás URL-címét.</span><span class="sxs-lookup"><span data-stu-id="37c8c-203">Your default browser is launched with the URL to the deployed app.</span></span> 

<span data-ttu-id="37c8c-204">Adjon hozzá néhány Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="37c8c-204">Add a few to-do items.</span></span>

![Közzétett ASP.NET-alkalmazást az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="37c8c-206">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="37c8c-206">Congratulations!</span></span> <span data-ttu-id="37c8c-207">Az adatvezérelt ASP.NET-alkalmazás fut élő az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="37c8c-207">Your data-driven ASP.NET application is running live in Azure App Service.</span></span>

## <a name="access-the-sql-database-locally"></a><span data-ttu-id="37c8c-208">Hozzáférés az SQL-adatbázis helyi</span><span class="sxs-lookup"><span data-stu-id="37c8c-208">Access the SQL Database locally</span></span>

<span data-ttu-id="37c8c-209">A Visual Studio lehetővé teszi, hogy vizsgálatát, és az új SQL-adatbázis egyszerűen a kezelése a **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-209">Visual Studio lets you explore and manage your new SQL Database easily in the **SQL Server Object Explorer**.</span></span>

### <a name="create-a-database-connection"></a><span data-ttu-id="37c8c-210">Adatbázis-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="37c8c-210">Create a database connection</span></span>

<span data-ttu-id="37c8c-211">Az a **nézet** menü **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-211">From the **View** menu, select **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="37c8c-212">Felső részén **SQL Server Object Explorer**, kattintson a **SQL-kiszolgáló hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="37c8c-212">At the top of **SQL Server Object Explorer**, click the **Add SQL Server** button.</span></span>

### <a name="configure-the-database-connection"></a><span data-ttu-id="37c8c-213">Az adatbázis-kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="37c8c-213">Configure the database connection</span></span>

<span data-ttu-id="37c8c-214">Az a **Connect** párbeszédpanelen bontsa ki a **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="37c8c-214">In the **Connect** dialog, expand the **Azure** node.</span></span> <span data-ttu-id="37c8c-215">Az SQL-adatbázis példányainak az Azure-ban az itt felsorolt.</span><span class="sxs-lookup"><span data-stu-id="37c8c-215">All your SQL Database instances in Azure are listed here.</span></span>

<span data-ttu-id="37c8c-216">Válassza ki a `DotNetAppSqlDb` SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="37c8c-216">Select the `DotNetAppSqlDb` SQL Database.</span></span> <span data-ttu-id="37c8c-217">A korábban létrehozott kapcsolat a program automatikusan kitölti a lap alján.</span><span class="sxs-lookup"><span data-stu-id="37c8c-217">The connection you created earlier is automatically filled at the bottom.</span></span>

<span data-ttu-id="37c8c-218">Írja be a korábban létrehozott adatbázis-rendszergazda jelszavát, és kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-218">Type the database administrator password you created earlier and click **Connect**.</span></span>

![A Visual Studio adatbázis-kapcsolat konfigurálása](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a><span data-ttu-id="37c8c-220">A számítógép az ügyfél-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="37c8c-220">Allow client connection from your computer</span></span>

<span data-ttu-id="37c8c-221">A **hozzon létre egy új tűzfalszabályt** párbeszédpanel már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="37c8c-221">The **Create a new firewall rule** dialog is opened.</span></span> <span data-ttu-id="37c8c-222">Alapértelmezés szerint az SQL Database-példányt csak Azure-szolgáltatások, például az Azure-webalkalmazásban kapcsolatokat engedélyez.</span><span class="sxs-lookup"><span data-stu-id="37c8c-222">By default, your SQL Database instance only allows connections from Azure services, such as your Azure web app.</span></span> <span data-ttu-id="37c8c-223">Kapcsolódás saját adatbázishoz, hozzon létre egy tűzfalszabályt SQL-adatbázispéldány.</span><span class="sxs-lookup"><span data-stu-id="37c8c-223">To connect to your database, create a firewall rule in the SQL Database instance.</span></span> <span data-ttu-id="37c8c-224">A tűzfalszabály lehetővé teszi, hogy a helyi számítógépen a nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="37c8c-224">The firewall rule allows the public IP address of your local computer.</span></span>

<span data-ttu-id="37c8c-225">A párbeszédpanel már ki van töltve, a számítógép nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="37c8c-225">The dialog is already filled with your computer's public IP address.</span></span>

<span data-ttu-id="37c8c-226">Győződjön meg arról, hogy **a ügyfél IP-cím hozzáadása** van kiválasztva, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-226">Make sure that **Add my client IP** is selected and click **OK**.</span></span> 

![SQL Database-példányt a tűzfal beállítása](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

<span data-ttu-id="37c8c-228">Visual Studio befejezi a tűzfal beállítása a SQL Database-példányt, ha a kapcsolat megjelennek **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-228">Once Visual Studio finishes creating the firewall setting for your SQL Database instance, your connection shows up in **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="37c8c-229">Itt végezheti a leggyakoribb adatbázis műveletek, például a lekérdezések futtatása hozhat létre, nézetek és tárolt eljárások, és több.</span><span class="sxs-lookup"><span data-stu-id="37c8c-229">Here, you can perform the most common database operations, such as run queries, create views and stored procedures, and more.</span></span> 

<span data-ttu-id="37c8c-230">Kattintson a jobb gombbal a `Todoes` tábla, és válassza ki **adatok megtekintéséhez**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-230">Right-click on the `Todoes` table and select **View Data**.</span></span> 

![Fedezze fel az SQL adatbázis-objektumok](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a><span data-ttu-id="37c8c-232">Frissítse app Code First áttelepítést</span><span class="sxs-lookup"><span data-stu-id="37c8c-232">Update app with Code First Migrations</span></span>

<span data-ttu-id="37c8c-233">Az adatbázis és a webes alkalmazást az Azure-ban frissíti használhatja a Visual Studio a jól ismert eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="37c8c-233">You can use the familiar tools in Visual Studio to update your database and web app in Azure.</span></span> <span data-ttu-id="37c8c-234">Ebben a lépésben segítségével Code First áttelepítést az Entity Framework megváltoztatja az adatbázis-séma, és tegye közzé az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="37c8c-234">In this step, you use Code First Migrations in Entity Framework to make a change to your database schema and publish it to Azure.</span></span>

<span data-ttu-id="37c8c-235">Entity Framework Code First áttelepítést használatával kapcsolatos további információkért lásd: [Ismerkedés az Entity Framework 6 Code First MVC 5-öt használó](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="37c8c-235">For more information about using Entity Framework Code First Migrations, see [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="update-your-data-model"></a><span data-ttu-id="37c8c-236">Az adatmodell frissítése</span><span class="sxs-lookup"><span data-stu-id="37c8c-236">Update your data model</span></span>

<span data-ttu-id="37c8c-237">Nyissa meg _Models\Todo.cs_ kód-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="37c8c-237">Open _Models\Todo.cs_ in the code editor.</span></span> <span data-ttu-id="37c8c-238">A következő tulajdonság hozzáadása a `ToDo` osztály:</span><span class="sxs-lookup"><span data-stu-id="37c8c-238">Add the following property to the `ToDo` class:</span></span>

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a><span data-ttu-id="37c8c-239">Code First áttelepítést helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="37c8c-239">Run Code First Migrations locally</span></span>

<span data-ttu-id="37c8c-240">Futtassa a frissítések a helyi adatbázis néhány parancsokat.</span><span class="sxs-lookup"><span data-stu-id="37c8c-240">Run a few commands to make updates to your local database.</span></span> 

<span data-ttu-id="37c8c-241">Az a **eszközök** menüben kattintson a **NuGet-Csomagkezelő** > **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-241">From the **Tools** menu, click **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="37c8c-242">A Package Manager Console ablakban Code First áttelepítést engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="37c8c-242">In the Package Manager Console window, enable Code First Migrations:</span></span>

```PowerShell
Enable-Migrations
```

<span data-ttu-id="37c8c-243">Vegye fel az áttelepítés:</span><span class="sxs-lookup"><span data-stu-id="37c8c-243">Add a migration:</span></span>

```PowerShell
Add-Migration AddProperty
```

<span data-ttu-id="37c8c-244">A helyi adatbázis frissítése:</span><span class="sxs-lookup"><span data-stu-id="37c8c-244">Update the local database:</span></span>

```PowerShell
Update-Database
```

<span data-ttu-id="37c8c-245">Típus `Ctrl+F5` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="37c8c-245">Type `Ctrl+F5` to run the app.</span></span> <span data-ttu-id="37c8c-246">A Szerkesztés, részletek tesztelése, és hivatkozások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37c8c-246">Test the edit, details, and create links.</span></span>

<span data-ttu-id="37c8c-247">Ha az alkalmazás betölt nem jelenik meg hibaüzenet, Code First áttelepítést sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="37c8c-247">If the application loads without errors, then Code First Migrations has succeeded.</span></span> <span data-ttu-id="37c8c-248">Azonban a lap továbbra is ugyanúgy az alkalmazáslogikát még nem használja ezt az új tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="37c8c-248">However, your page still looks the same because your application logic is not using this new property yet.</span></span> 

### <a name="use-the-new-property"></a><span data-ttu-id="37c8c-249">Az új tulajdonsággal</span><span class="sxs-lookup"><span data-stu-id="37c8c-249">Use the new property</span></span>

<span data-ttu-id="37c8c-250">Néhány módosítást használata a kódban a `Done` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="37c8c-250">Make some changes in your code to use the `Done` property.</span></span> <span data-ttu-id="37c8c-251">Az egyszerűség kedvéért ebben az oktatóanyagban, csak fogja módosítani a `Index` és `Create` nézeteket, művelet: a tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="37c8c-251">For simplicity in this tutorial, you're only going to change the `Index` and `Create` views to see the property in action.</span></span>

<span data-ttu-id="37c8c-252">Nyissa meg _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="37c8c-252">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="37c8c-253">Keresés a `Create()` metódust, és adja hozzá `Done` a tulajdonságok listájának a `Bind` attribútum.</span><span class="sxs-lookup"><span data-stu-id="37c8c-253">Find the `Create()` method and add `Done` to the list of properties in the `Bind` attribute.</span></span> <span data-ttu-id="37c8c-254">Amikor elkészült, a `Create()` metódus-aláírás néz ki a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="37c8c-254">When you're done, your `Create()` method signature looks like the following code:</span></span>

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

<span data-ttu-id="37c8c-255">Nyissa meg _Views\Todos\Create.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="37c8c-255">Open _Views\Todos\Create.cshtml_.</span></span>

<span data-ttu-id="37c8c-256">A Razor kódban kell megjelennie a `<div class="form-group">` elem által használt `model.Description`, és majd egy másik `<div class="form-group">` elem által használt `model.CreatedDate`.</span><span class="sxs-lookup"><span data-stu-id="37c8c-256">In the Razor code, you should see a `<div class="form-group">` element that uses `model.Description`, and then another `<div class="form-group">` element that uses `model.CreatedDate`.</span></span> <span data-ttu-id="37c8c-257">Ez a két elem követő hozzáadása egy másik `<div class="form-group">` elem által használt `model.Done`:</span><span class="sxs-lookup"><span data-stu-id="37c8c-257">Immediately following these two elements, add another `<div class="form-group">` element that uses `model.Done`:</span></span>

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
    </div>
</div>
```

<span data-ttu-id="37c8c-258">Nyissa meg _Views\Todos\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="37c8c-258">Open _Views\Todos\Index.cshtml_.</span></span>

<span data-ttu-id="37c8c-259">Keresse meg az üres `<th></th>` elemet.</span><span class="sxs-lookup"><span data-stu-id="37c8c-259">Search for the empty `<th></th>` element.</span></span> <span data-ttu-id="37c8c-260">Ez az elem felett adja hozzá a következő Razor-kódot:</span><span class="sxs-lookup"><span data-stu-id="37c8c-260">Just above this element, add the following Razor code:</span></span>

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

<span data-ttu-id="37c8c-261">Keresés a `<td>` elem, amely tartalmazza a `Html.ActionLink()` segédmódszereket.</span><span class="sxs-lookup"><span data-stu-id="37c8c-261">Find the `<td>` element that contains the `Html.ActionLink()` helper methods.</span></span> <span data-ttu-id="37c8c-262">Ez az elem felett adja hozzá a következő Razor-kódot:</span><span class="sxs-lookup"><span data-stu-id="37c8c-262">Just above this element, add the following Razor code:</span></span>

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

<span data-ttu-id="37c8c-263">Ez minden szeretne látni a változtatásokat a `Index` és `Create` nézetek.</span><span class="sxs-lookup"><span data-stu-id="37c8c-263">That's all you need to see the changes in the `Index` and `Create` views.</span></span> 

<span data-ttu-id="37c8c-264">Típus `Ctrl+F5` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="37c8c-264">Type `Ctrl+F5` to run the app.</span></span>

<span data-ttu-id="37c8c-265">Ezután egy teendő hozzáadása és ellenőrzése **végzett**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-265">You can now add a to-do item and check **Done**.</span></span> <span data-ttu-id="37c8c-266">Ezután azt kell jelennek meg az Ön újragondolt befejezett elemként.</span><span class="sxs-lookup"><span data-stu-id="37c8c-266">Then it should show up in your homepage as a completed item.</span></span> <span data-ttu-id="37c8c-267">Ne feledje, hogy a `Edit` nézet nem jeleníti meg a `Done` mezőben, mert nem módosítja a `Edit` nézet.</span><span class="sxs-lookup"><span data-stu-id="37c8c-267">Remember that the `Edit` view doesn't show the `Done` field, because you didn't change the `Edit` view.</span></span>

### <a name="enable-code-first-migrations-in-azure"></a><span data-ttu-id="37c8c-268">Engedélyezze a Code First áttelepítést az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="37c8c-268">Enable Code First Migrations in Azure</span></span>

<span data-ttu-id="37c8c-269">Most, hogy a kód módosítása működéséről, beleértve az adatbázis az áttelepítés akkor tegye közzé az Azure-webalkalmazásban, és frissítse az SQL-adatbázis Code First áttelepítést túl.</span><span class="sxs-lookup"><span data-stu-id="37c8c-269">Now that your code change works, including database migration, you publish it to your Azure web app and update your SQL Database with Code First Migrations too.</span></span>

<span data-ttu-id="37c8c-270">Fentiekhez hasonló, kattintson jobb gombbal a projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-270">Just like before, right-click your project and select **Publish**.</span></span>

<span data-ttu-id="37c8c-271">Kattintson a **beállítások** a Közzététel varázsló megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="37c8c-271">Click **Settings** to open the publish wizard.</span></span>

![Nyissa meg a közzétételi beállítások](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

<span data-ttu-id="37c8c-273">A varázslóban kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-273">In the wizard, click **Next**.</span></span>

<span data-ttu-id="37c8c-274">Győződjön meg arról, hogy az SQL-adatbázis kapcsolati karakterlánca nem üres **MyDatabaseContext (MyDbConnection)**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-274">Make sure that the connection string for your SQL Database is populated in **MyDatabaseContext (MyDbConnection)**.</span></span> <span data-ttu-id="37c8c-275">Előfordulhat, hogy kell kiválasztania a **myToDoAppDb** adatbázis a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="37c8c-275">You may need to select the **myToDoAppDb** database from the dropdown.</span></span> 

<span data-ttu-id="37c8c-276">Válassza ki **hajtható végre Code First áttelepítést (fut az alkalmazás indítása)**, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-276">Select **Execute Code First Migrations (runs on application start)**, then click **Save**.</span></span>

![Az Azure web app alkalmazásban Code First áttelepítést engedélyezése](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a><span data-ttu-id="37c8c-278">A változtatások közzététele</span><span class="sxs-lookup"><span data-stu-id="37c8c-278">Publish your changes</span></span>

<span data-ttu-id="37c8c-279">Most, hogy Code First áttelepítést az Azure web app alkalmazásban engedélyezve van, a kód változtatások közzététele.</span><span class="sxs-lookup"><span data-stu-id="37c8c-279">Now that you enabled Code First Migrations in your Azure web app, publish your code changes.</span></span>

<span data-ttu-id="37c8c-280">A közzétételi oldalon kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="37c8c-280">In the publish page, click **Publish**.</span></span>

<span data-ttu-id="37c8c-281">Próbálja meg újból hozzáadni a Tennivalólista elemein, és válassza ki **végzett**, és Ön újragondolt befejezett elemként jelenniük.</span><span class="sxs-lookup"><span data-stu-id="37c8c-281">Try adding to-do items again and select **Done**, and they should show up in your homepage as a completed item.</span></span>

![Azure-webalkalmazásban kód első áttelepítés után](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

<span data-ttu-id="37c8c-283">A meglévő Tennivalólista elemein továbbra is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="37c8c-283">All your existing to-do items are still displayed.</span></span> <span data-ttu-id="37c8c-284">Ha az ASP.NET-alkalmazás közzétételéhez meglévő az SQL-adatbázis adatai nem elveszett.</span><span class="sxs-lookup"><span data-stu-id="37c8c-284">When you republish your ASP.NET application, existing data in your SQL Database is not lost.</span></span> <span data-ttu-id="37c8c-285">Code First áttelepítést is, csak a Adatséma változik, és a meglévő adatok sértetlenek.</span><span class="sxs-lookup"><span data-stu-id="37c8c-285">Also, Code First Migrations only changes the data schema and leaves your existing data intact.</span></span>


## <a name="stream-application-logs"></a><span data-ttu-id="37c8c-286">Az adatfolyam alkalmazásnaplók</span><span class="sxs-lookup"><span data-stu-id="37c8c-286">Stream application logs</span></span>

<span data-ttu-id="37c8c-287">Nyomkövetési üzenet az Azure-webalkalmazásban közvetlenül a Visual Studio adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="37c8c-287">You can stream tracing messages directly from your Azure web app to Visual Studio.</span></span>

<span data-ttu-id="37c8c-288">Nyissa meg _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="37c8c-288">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="37c8c-289">Minden egyes művelethez kezdődik-e a `Trace.WriteLine()` metódust.</span><span class="sxs-lookup"><span data-stu-id="37c8c-289">Each action starts with a `Trace.WriteLine()` method.</span></span> <span data-ttu-id="37c8c-290">Ez a kód bemutatják a nyomkövetési üzenetek hozzáadása az Azure-webalkalmazásban kerül.</span><span class="sxs-lookup"><span data-stu-id="37c8c-290">This code is added to show you how to add trace messages to your Azure web app.</span></span>

### <a name="open-server-explorer"></a><span data-ttu-id="37c8c-291">Nyissa meg a Server Explorer</span><span class="sxs-lookup"><span data-stu-id="37c8c-291">Open Server Explorer</span></span>

<span data-ttu-id="37c8c-292">Az a **nézet** menü **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-292">From the **View** menu, select **Server Explorer**.</span></span> <span data-ttu-id="37c8c-293">Naplózás konfigurálása az Azure-webalkalmazásban a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-293">You can configure logging for your Azure web app in **Server Explorer**.</span></span> 

### <a name="enable-log-streaming"></a><span data-ttu-id="37c8c-294">Adatfolyamként-napló</span><span class="sxs-lookup"><span data-stu-id="37c8c-294">Enable log streaming</span></span>

<span data-ttu-id="37c8c-295">A **Server Explorer**, bontsa ki a **Azure** > **App Service**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-295">In **Server Explorer**, expand **Azure** > **App Service**.</span></span>

<span data-ttu-id="37c8c-296">Bontsa ki a **myResourceGroup** erőforráscsoport, létrehozta az Azure web app kezdeti létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="37c8c-296">Expand the **myResourceGroup** resource group, you created when you first created the Azure web app.</span></span>

<span data-ttu-id="37c8c-297">Kattintson a jobb gombbal az Azure-webalkalmazásban, és válassza ki **folyamatos átviteli naplók megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-297">Right-click your Azure web app and select **View Streaming Logs**.</span></span>

![Adatfolyamként-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

<span data-ttu-id="37c8c-299">A naplók mostantól a folyamatos átviteli a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="37c8c-299">The logs are now streamed into the **Output** window.</span></span> 

![A kimeneti ablakban adatfolyam-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

<span data-ttu-id="37c8c-301">Azonban nem jelennek meg a nyomkövetési üzenetek még.</span><span class="sxs-lookup"><span data-stu-id="37c8c-301">However, you don't see any of the trace messages yet.</span></span> <span data-ttu-id="37c8c-302">Meg mert először kiválasztásakor **folyamatos átviteli naplók megtekintése**, az Azure-webalkalmazásban beállítja a nyomkövetési szint beállítása azokhoz a `Error`, amely csak naplózza hibaesemények (az a `Trace.TraceError()` metódus).</span><span class="sxs-lookup"><span data-stu-id="37c8c-302">That's because when you first select **View Streaming Logs**, your Azure web app sets the trace level to `Error`, which only logs error events (with the `Trace.TraceError()` method).</span></span>

### <a name="change-trace-levels"></a><span data-ttu-id="37c8c-303">Nyomkövetési szintek módosítása</span><span class="sxs-lookup"><span data-stu-id="37c8c-303">Change trace levels</span></span>

<span data-ttu-id="37c8c-304">Ha módosítani szeretné a nyomkövetési szintek többi nyomkövetési üzenet kimeneti, lépjen vissza **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-304">To change the trace levels to output other trace messages, go back to **Server Explorer**.</span></span>

<span data-ttu-id="37c8c-305">Az Azure-webalkalmazás ismét gombbal és válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-305">Right-click your Azure web app again and select **Settings**.</span></span>

<span data-ttu-id="37c8c-306">Az a **Alkalmazásnaplózást (fájlrendszer)** legördülő menüből válassza **részletes**.</span><span class="sxs-lookup"><span data-stu-id="37c8c-306">In the **Application Logging (File System)** dropdown, select **Verbose**.</span></span> <span data-ttu-id="37c8c-307">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="37c8c-307">Click **Save**.</span></span>

![A részletes nyomkövetési szint módosítása](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> <span data-ttu-id="37c8c-309">Kísérletezhet, és hogy milyen típusú üzenetek jelennek meg az egyes különböző nyomkövetési szintek.</span><span class="sxs-lookup"><span data-stu-id="37c8c-309">You can experiment with different trace levels to see what types of messages are displayed for each level.</span></span> <span data-ttu-id="37c8c-310">Például a **információkat** szint továbbít minden által létrehozott naplók `Trace.TraceInformation()`, `Trace.TraceWarning()`, és `Trace.TraceError()`, de nem által létrehozott naplók `Trace.WriteLine()`.</span><span class="sxs-lookup"><span data-stu-id="37c8c-310">For example, the **Information** level includes all logs created by `Trace.TraceInformation()`, `Trace.TraceWarning()`, and `Trace.TraceError()`, but not logs created by `Trace.WriteLine()`.</span></span>
>
>

<span data-ttu-id="37c8c-311">A böngészőben a Tennivalók listája alkalmazás az Azure-ban körül kattintva próbálja.</span><span class="sxs-lookup"><span data-stu-id="37c8c-311">In your browser, try clicking around the to-do list application in Azure.</span></span> <span data-ttu-id="37c8c-312">A nyomkövetési üzenetek most részére a **kimeneti** ablak a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="37c8c-312">The trace messages are now streamed to the **Output** window in Visual Studio.</span></span>

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a><span data-ttu-id="37c8c-313">Állítsa le az adatfolyam-napló</span><span class="sxs-lookup"><span data-stu-id="37c8c-313">Stop log streaming</span></span>

<span data-ttu-id="37c8c-314">A napló-adatfolyam-szolgáltatás leállításához kattintson a **figyelés leállításának** gombra a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="37c8c-314">To stop the log-streaming service, click the **Stop monitoring** button in the **Output** window.</span></span>

![Állítsa le az adatfolyam-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="37c8c-316">Az Azure-webalkalmazásban kezelése</span><span class="sxs-lookup"><span data-stu-id="37c8c-316">Manage your Azure web app</span></span>

<span data-ttu-id="37c8c-317">Lépjen a [Azure-portálon](https://portal.azure.com) létrehozott webalkalmazás megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="37c8c-317">Go to the [Azure portal](https://portal.azure.com) to see the web app you created.</span></span> 



<span data-ttu-id="37c8c-318">A bal oldali menüben kattintson az **App Service** lehetőségre, majd az Azure-webapp nevére.</span><span class="sxs-lookup"><span data-stu-id="37c8c-318">From the left menu, click **App Service**, then click the name of your Azure web app.</span></span>

![Navigálás a portálon az Azure-webapphoz](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

<span data-ttu-id="37c8c-320">A webalkalmazás lapon rendelkezik ki.</span><span class="sxs-lookup"><span data-stu-id="37c8c-320">You have landed in your web app's page.</span></span> 

<span data-ttu-id="37c8c-321">Alapértelmezés szerint a portál megjeleníti a **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="37c8c-321">By default, the portal shows the **Overview** page.</span></span> <span data-ttu-id="37c8c-322">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="37c8c-322">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="37c8c-323">Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="37c8c-323">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="37c8c-324">A lap bal oldalán a lapok megnyithatja a különböző konfigurációs lapok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="37c8c-324">The tabs on the left side of the page show the different configuration pages you can open.</span></span> 

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="37c8c-326">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37c8c-326">Next steps</span></span>

<span data-ttu-id="37c8c-327">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="37c8c-327">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="37c8c-328">SQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="37c8c-328">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="37c8c-329">ASP.NET alkalmazás csatlakoztatása az SQL-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="37c8c-329">Connect an ASP.NET app to SQL Database</span></span>
> * <span data-ttu-id="37c8c-330">Az alkalmazás telepítése az Azure-bA</span><span class="sxs-lookup"><span data-stu-id="37c8c-330">Deploy the app to Azure</span></span>
> * <span data-ttu-id="37c8c-331">Az adatmodell frissítése, és telepítse újra az alkalmazást</span><span class="sxs-lookup"><span data-stu-id="37c8c-331">Update the data model and redeploy the app</span></span>
> * <span data-ttu-id="37c8c-332">Stream naplók az Azure-ból a Terminálszolgáltatások számára</span><span class="sxs-lookup"><span data-stu-id="37c8c-332">Stream logs from Azure to your terminal</span></span>
> * <span data-ttu-id="37c8c-333">Felügyelheti az alkalmazást az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="37c8c-333">Manage the app in the Azure portal</span></span>

<span data-ttu-id="37c8c-334">A következő oktatóanyag megtudhatja, hogyan képezheti egy egyéni DNS-nevet a webalkalmazás továbblépés.</span><span class="sxs-lookup"><span data-stu-id="37c8c-334">Advance to the next tutorial to learn how to map a custom DNS name to the web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="37c8c-335">Meglévő egyéni DNS-név hozzákapcsolása az Azure-webalkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="37c8c-335">Map an existing custom DNS name to Azure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
