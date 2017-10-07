---
title: "Azure SQL Database és az ASP.NET alkalmazás aaaBuild |} Microsoft Docs"
description: "Megtudhatja, hogyan tooget egy ASP.NET alkalmazás használata az Azure SQL-adatbázis kapcsolati tooa."
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
ms.openlocfilehash: d21c2bc404bfe038608c17e5a94d96847153002c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a><span data-ttu-id="ef120-103">Azure SQL Database és az ASP.NET alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef120-103">Build an ASP.NET app in Azure with SQL Database</span></span>

<span data-ttu-id="ef120-104">Az [Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) egy hatékonyan méretezhető, önjavító webes üzemeltetési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ef120-104">[Azure Web Apps](https://docs.microsoft.com/azure/app-service-web/app-service-web-overview) provides a highly scalable, self-patching web hosting service.</span></span> <span data-ttu-id="ef120-105">Az oktatóanyag bemutatja, hogyan toodeploy adatvezérelt ASP.NET webalkalmazás az Azure-ban, és csatlakoztassa túl[Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ef120-105">This tutorial shows you how toodeploy a data-driven ASP.NET web app in Azure and connect it too[Azure SQL Database](../sql-database/sql-database-technical-overview.md).</span></span> <span data-ttu-id="ef120-106">Amikor végzett, hogy egy ASP.NET-alkalmazás fusson az [Azure App Service](../app-service/app-service-value-prop-what-is.md) és tooSQL adatbázis csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="ef120-106">When you're finished, you have a ASP.NET app running in [Azure App Service](../app-service/app-service-value-prop-what-is.md) and connected tooSQL Database.</span></span>

![Közzétett ASP.NET-alkalmazást az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="ef120-108">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="ef120-108">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef120-109">SQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ef120-109">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="ef120-110">Csatlakozás egy ASP.NET alkalmazás tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="ef120-110">Connect an ASP.NET app tooSQL Database</span></span>
> * <span data-ttu-id="ef120-111">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="ef120-111">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="ef120-112">Hello adatmodell frissítése, és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="ef120-112">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="ef120-113">Stream naplók az Azure tooyour terminál</span><span class="sxs-lookup"><span data-stu-id="ef120-113">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="ef120-114">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="ef120-114">Manage hello app in hello Azure portal</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef120-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ef120-115">Prerequisites</span></span>

<span data-ttu-id="ef120-116">toocomplete Ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="ef120-116">toocomplete this tutorial:</span></span>

* <span data-ttu-id="ef120-117">Telepítés [Visual Studio 2017](https://www.visualstudio.com/downloads/) az alábbi munkaterhelések hello:</span><span class="sxs-lookup"><span data-stu-id="ef120-117">Install [Visual Studio 2017](https://www.visualstudio.com/downloads/) with hello following workloads:</span></span>
  - <span data-ttu-id="ef120-118">**ASP.NET és webfejlesztés**</span><span class="sxs-lookup"><span data-stu-id="ef120-118">**ASP.NET and web development**</span></span>
  - <span data-ttu-id="ef120-119">**Azure-fejlesztés**</span><span class="sxs-lookup"><span data-stu-id="ef120-119">**Azure development**</span></span>

  ![ASP.NET és webfejlesztés és Azure-fejlesztés (Web és felhőszolgáltatások alatt)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-hello-sample"></a><span data-ttu-id="ef120-121">Hello minta letöltése</span><span class="sxs-lookup"><span data-stu-id="ef120-121">Download hello sample</span></span>

<span data-ttu-id="ef120-122">[Töltse le a hello mintaprojektet](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span><span class="sxs-lookup"><span data-stu-id="ef120-122">[Download hello sample project](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).</span></span>

<span data-ttu-id="ef120-123">Extract (csomagolja ki) hello *dotnet-sqldb-oktatóanyag – master.zip* fájlt.</span><span class="sxs-lookup"><span data-stu-id="ef120-123">Extract (unzip) hello  *dotnet-sqldb-tutorial-master.zip* file.</span></span>

<span data-ttu-id="ef120-124">minta projektben egy alapszintű hello [ASP.NET MVC](https://www.asp.net/mvc) CRUD (hozzon létre-olvasás-Módosítás-Törlés) alkalmazás használatával [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="ef120-124">hello sample project contains a basic [ASP.NET MVC](https://www.asp.net/mvc) CRUD (create-read-update-delete) app using [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="run-hello-app"></a><span data-ttu-id="ef120-125">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="ef120-125">Run hello app</span></span>

<span data-ttu-id="ef120-126">Nyissa meg hello *dotnet-sqldb-oktatóanyag – master/DotNetAppSqlDb.sln* fájlt a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ef120-126">Open hello *dotnet-sqldb-tutorial-master/DotNetAppSqlDb.sln* file in Visual Studio.</span></span> 

<span data-ttu-id="ef120-127">Típus `Ctrl+F5` toorun hello app hibakeresés nélkül.</span><span class="sxs-lookup"><span data-stu-id="ef120-127">Type `Ctrl+F5` toorun hello app without debugging.</span></span> <span data-ttu-id="ef120-128">hello alkalmazás az alapértelmezett böngésző jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="ef120-128">hello app is displayed in your default browser.</span></span> <span data-ttu-id="ef120-129">Jelölje be hello **hozzon létre új** hivatkozásra, és hozzon létre több *tennivaló* elemeket.</span><span class="sxs-lookup"><span data-stu-id="ef120-129">Select hello **Create New** link and create a couple *to-do* items.</span></span> 

![A New ASP.NET Project (Új ASP.NET-projekt) párbeszédpanel](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

<span data-ttu-id="ef120-131">Teszt hello **szerkesztése**, **részletek**, és **törlése** hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="ef120-131">Test hello **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="ef120-132">hello app hello adatbázis egy adatbázis-környezet tooconnect használ.</span><span class="sxs-lookup"><span data-stu-id="ef120-132">hello app uses a database context tooconnect with hello database.</span></span> <span data-ttu-id="ef120-133">Ez a példa hello adatbázis-környezet nevű kapcsolati karakterláncot használ `MyDbConnection`.</span><span class="sxs-lookup"><span data-stu-id="ef120-133">In this sample, hello database context uses a connection string named `MyDbConnection`.</span></span> <span data-ttu-id="ef120-134">hello kapcsolati karakterlánc megadása hello *Web.config* fájl- és a hivatkozott hello *Models/MyDatabaseContext.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="ef120-134">hello connection string is set in hello *Web.config* file and referenced in hello *Models/MyDatabaseContext.cs* file.</span></span> <span data-ttu-id="ef120-135">hello kapcsolati karakterlánc nevét később hello oktatóanyag tooconnect hello Azure web app tooan Azure SQL Database használatos.</span><span class="sxs-lookup"><span data-stu-id="ef120-135">hello connection string name is used later in hello tutorial tooconnect hello Azure web app tooan Azure SQL Database.</span></span> 

## <a name="publish-tooazure-with-sql-database"></a><span data-ttu-id="ef120-136">Az SQL Database tooAzure közzététele</span><span class="sxs-lookup"><span data-stu-id="ef120-136">Publish tooAzure with SQL Database</span></span>

<span data-ttu-id="ef120-137">A hello **Megoldáskezelőben**, kattintson a jobb gombbal a **DotNetAppSqlDb** projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ef120-137">In hello **Solution Explorer**, right-click your **DotNetAppSqlDb** project and select **Publish**.</span></span>

![Közzététel a Megoldáskezelőből](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

<span data-ttu-id="ef120-139">Győződjön meg arról, hogy a **Microsoft Azure App Service** van kiválasztva, és kattintson a **Publish** (Közzététel) elemre.</span><span class="sxs-lookup"><span data-stu-id="ef120-139">Make sure that **Microsoft Azure App Service** is selected and click **Publish**.</span></span>

![Közzététel a projekt áttekintő oldaláról](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

<span data-ttu-id="ef120-141">A közzététel megnyitja hello **létrehozása az App Service** párbeszédpanel, amely segít hoz létre minden hello Azure-erőforrások toorun az ASP.NET webalkalmazás az Azure-ban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="ef120-141">Publishing opens hello **Create App Service** dialog, which helps you create all hello Azure resources you need toorun your ASP.NET web app in Azure.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="ef120-142">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="ef120-142">Sign in tooAzure</span></span>

<span data-ttu-id="ef120-143">A hello **létrehozása az App Service** párbeszédpanel, kattintson a **vegyen fel egy fiókot**, majd jelentkezzen be Azure-előfizetés tooyour.</span><span class="sxs-lookup"><span data-stu-id="ef120-143">In hello **Create App Service** dialog, click **Add an account**, and then sign in tooyour Azure subscription.</span></span> <span data-ttu-id="ef120-144">Ha már be van jelentkezve egy Microsoft-fiókba, győződjön meg arról, hogy abban a fiókban található az előfizetése.</span><span class="sxs-lookup"><span data-stu-id="ef120-144">If you're already signed into a Microsoft account, make sure that account holds your Azure subscription.</span></span> <span data-ttu-id="ef120-145">Ha hello bejelentkezett Microsoft-fiók nem rendelkezik Azure-előfizetése, kattintson rá tooadd hello megfelelő fiókot.</span><span class="sxs-lookup"><span data-stu-id="ef120-145">If hello signed-in Microsoft account doesn't have your Azure subscription, click it tooadd hello correct account.</span></span>
   
![Jelentkezzen be tooAzure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

<span data-ttu-id="ef120-147">Miután bejelentkezett, most készen áll a toocreate összes hello ezen a párbeszédpanelen az Azure webalkalmazás számára szükséges erőforrások.</span><span class="sxs-lookup"><span data-stu-id="ef120-147">Once signed in, you're ready toocreate all hello resources you need for your Azure web app in this dialog.</span></span>

### <a name="configure-hello-web-app-name"></a><span data-ttu-id="ef120-148">Hello webalkalmazás nevének konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ef120-148">Configure hello web app name</span></span>

<span data-ttu-id="ef120-149">Megtartja-hello létrehozott webes alkalmazás neve, vagy módosítsa azt tooanother egyedi nevét (érvényes karakterek: `a-z`, `0-9`, és `-`).</span><span class="sxs-lookup"><span data-stu-id="ef120-149">You can keep hello generated web app name, or change it tooanother unique name (valid characters are `a-z`, `0-9`, and `-`).</span></span> <span data-ttu-id="ef120-150">az alkalmazás hello alapértelmezett URL-cím részeként használatos hello webes alkalmazás neve (`<app_name>.azurewebsites.net`, ahol `<app_name>` a webes alkalmazás neve).</span><span class="sxs-lookup"><span data-stu-id="ef120-150">hello web app name is used as part of hello default URL for your app (`<app_name>.azurewebsites.net`, where `<app_name>` is your web app name).</span></span> <span data-ttu-id="ef120-151">hello webalkalmazásnév kell toobe egyedi az Azure-ban minden alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="ef120-151">hello web app name needs toobe unique across all apps in Azure.</span></span> 

![Hozzon létre az app service párbeszédpanelen](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

### <a name="create-a-resource-group"></a><span data-ttu-id="ef120-153">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="ef120-153">Create a resource group</span></span>

[!INCLUDE [resource-group](../../includes/resource-group.md)]

<span data-ttu-id="ef120-154">Következő túl**erőforráscsoport**, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="ef120-154">Next too**Resource Group**, click **New**.</span></span>

![Következő tooResource csoport, az új gombra.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

<span data-ttu-id="ef120-156">Hello erőforráscsoport neve **myResourceGroup**.</span><span class="sxs-lookup"><span data-stu-id="ef120-156">Name hello resource group **myResourceGroup**.</span></span>

> [!NOTE]
> <span data-ttu-id="ef120-157">Ne kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="ef120-157">Do not click **Create**.</span></span> <span data-ttu-id="ef120-158">Először egy SQL-adatbázis egy későbbi lépésben tooset.</span><span class="sxs-lookup"><span data-stu-id="ef120-158">You first need tooset up a SQL Database in a later step.</span></span>

### <a name="create-an-app-service-plan"></a><span data-ttu-id="ef120-159">App Service-csomag létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef120-159">Create an App Service plan</span></span>

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

<span data-ttu-id="ef120-160">Következő túl**App Service-csomag**, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="ef120-160">Next too**App Service Plan**, click **New**.</span></span> 

<span data-ttu-id="ef120-161">A hello **App Service-csomag konfigurálása** párbeszédpanelen a következő beállítások hello hello új App Service-csomag konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="ef120-161">In hello **Configure App Service Plan** dialog, configure hello new App Service plan with hello following settings:</span></span>

![App Service-csomag létrehozása](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| <span data-ttu-id="ef120-163">Beállítás</span><span class="sxs-lookup"><span data-stu-id="ef120-163">Setting</span></span>  | <span data-ttu-id="ef120-164">Ajánlott érték</span><span class="sxs-lookup"><span data-stu-id="ef120-164">Suggested value</span></span> | <span data-ttu-id="ef120-165">További információ</span><span class="sxs-lookup"><span data-stu-id="ef120-165">For more information</span></span> |
| ----------------- | ------------ | ----|
|<span data-ttu-id="ef120-166">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="ef120-166">**App Service Plan**</span></span>| <span data-ttu-id="ef120-167">myAppServicePlan</span><span class="sxs-lookup"><span data-stu-id="ef120-167">myAppServicePlan</span></span> | [<span data-ttu-id="ef120-168">App Service-csomagok</span><span class="sxs-lookup"><span data-stu-id="ef120-168">App Service plans</span></span>](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|<span data-ttu-id="ef120-169">**Hely**</span><span class="sxs-lookup"><span data-stu-id="ef120-169">**Location**</span></span>| <span data-ttu-id="ef120-170">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="ef120-170">West Europe</span></span> | [<span data-ttu-id="ef120-171">Azure-régiók</span><span class="sxs-lookup"><span data-stu-id="ef120-171">Azure regions</span></span>](https://azure.microsoft.com/regions/) |
|<span data-ttu-id="ef120-172">**Méret**</span><span class="sxs-lookup"><span data-stu-id="ef120-172">**Size**</span></span>| <span data-ttu-id="ef120-173">Ingyenes</span><span class="sxs-lookup"><span data-stu-id="ef120-173">Free</span></span> | [<span data-ttu-id="ef120-174">Tarifacsomagok</span><span class="sxs-lookup"><span data-stu-id="ef120-174">Pricing tiers</span></span>](https://azure.microsoft.com/pricing/details/app-service/)|

### <a name="create-a-sql-server-instance"></a><span data-ttu-id="ef120-175">SQL Server-példány létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef120-175">Create a SQL Server instance</span></span>

<span data-ttu-id="ef120-176">Adatbázis létrehozása előtt kell egy [Azure SQL Database logikai kiszolgáló](../sql-database/sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="ef120-176">Before creating a database, you need an [Azure SQL Database logical server](../sql-database/sql-database-features.md).</span></span> <span data-ttu-id="ef120-177">A logikai kiszolgálók adatbázisok egy csoportját tartalmazzák, amelyeket a rendszer egy csoportként kezel.</span><span class="sxs-lookup"><span data-stu-id="ef120-177">A logical server contains a group of databases managed as a group.</span></span>

<span data-ttu-id="ef120-178">Válassza ki **további Azure-szolgáltatások felfedezés**.</span><span class="sxs-lookup"><span data-stu-id="ef120-178">Select **Explore additional Azure services**.</span></span>

![A webapp nevének konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

<span data-ttu-id="ef120-180">A hello **szolgáltatások** lapra, majd hello  **+**  ikon következő túl**SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="ef120-180">In hello **Services** tab, click hello **+** icon next too**SQL Database**.</span></span> 

![A hello szolgáltatások lapon kattintson a hello + ikon következő tooSQL adatbázis.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

<span data-ttu-id="ef120-182">A hello **SQL-adatbázis beállítása** párbeszédpanel, kattintson a **új** következő túl**SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="ef120-182">In hello **Configure SQL Database** dialog, click **New** next too**SQL Server**.</span></span> 

<span data-ttu-id="ef120-183">Létrejön egy egyedi kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="ef120-183">A unique server name is generated.</span></span> <span data-ttu-id="ef120-184">Ez a név részeként hello alapértelmezett URL-cím a logikai kiszolgálóhoz használt `<server_name>.database.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="ef120-184">This name is used as part of hello default URL for your logical server, `<server_name>.database.windows.net`.</span></span> <span data-ttu-id="ef120-185">Egyedinek kell lennie az Azure-ban minden logikai kiszolgáló példányára.</span><span class="sxs-lookup"><span data-stu-id="ef120-185">It must be unique across all logical server instances in Azure.</span></span> <span data-ttu-id="ef120-186">Módosítsa hello kiszolgáló nevét, de ebben az oktatóanyagban tartsa meg a létrehozott hello értéket.</span><span class="sxs-lookup"><span data-stu-id="ef120-186">You can change hello server name, but for this tutorial, keep hello generated value.</span></span>

<span data-ttu-id="ef120-187">Adja hozzá egy rendszergazdai jogosultságú felhasználónevet és jelszót, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef120-187">Add an administrator username and password, and then select **OK**.</span></span> <span data-ttu-id="ef120-188">Összetettségi követelményeknek, lásd: [jelszóházirend](/sql/relational-databases/security/password-policy).</span><span class="sxs-lookup"><span data-stu-id="ef120-188">For password complexity requirements, see [Password Policy](/sql/relational-databases/security/password-policy).</span></span>

<span data-ttu-id="ef120-189">Ne felejtse el ezt a felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="ef120-189">Remember this username and password.</span></span> <span data-ttu-id="ef120-190">Szüksége van rá toomanage hello logikai kiszolgáló újabb példányt.</span><span class="sxs-lookup"><span data-stu-id="ef120-190">You need them toomanage hello logical server instance later.</span></span>

![SQL Server-példány létrehozása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

### <a name="create-a-sql-database"></a><span data-ttu-id="ef120-192">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef120-192">Create a SQL Database</span></span>

<span data-ttu-id="ef120-193">A hello **SQL-adatbázis beállítása** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="ef120-193">In hello **Configure SQL Database** dialog:</span></span> 

* <span data-ttu-id="ef120-194">Tartsa hello alapértelmezés szerint generált **adatbázisnév**.</span><span class="sxs-lookup"><span data-stu-id="ef120-194">Keep hello default generated **Database Name**.</span></span>
* <span data-ttu-id="ef120-195">A **kapcsolati karakterlánc nevét**, típus *MyDbConnection*.</span><span class="sxs-lookup"><span data-stu-id="ef120-195">In **Connection String Name**, type *MyDbConnection*.</span></span> <span data-ttu-id="ef120-196">Ezt a nevet meg kell egyeznie a hivatkozott hello kapcsolati karakterlánc *Models/MyDatabaseContext.cs*.</span><span class="sxs-lookup"><span data-stu-id="ef120-196">This name must match hello connection string that is referenced in *Models/MyDatabaseContext.cs*.</span></span>
* <span data-ttu-id="ef120-197">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="ef120-197">Select **OK**.</span></span>

![SQL-adatbázis konfigurálása](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

<span data-ttu-id="ef120-199">Hello **létrehozása az App Service** párbeszédpanelen látható hello erőforrások létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ef120-199">hello **Create App Service** dialog shows hello resources you've created.</span></span> <span data-ttu-id="ef120-200">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ef120-200">Click **Create**.</span></span> 

![létrehozott hello erőforrások](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

<span data-ttu-id="ef120-202">Miután hello varázsló végzett a létrehozással hello Azure-erőforrások, az ASP.NET alkalmazás tooAzure tesz közzé.</span><span class="sxs-lookup"><span data-stu-id="ef120-202">Once hello wizard finishes creating hello Azure resources, it  publishes your ASP.NET app tooAzure.</span></span> <span data-ttu-id="ef120-203">Az alapértelmezett böngésző hello URL-cím toohello telepített alkalmazáshoz nincs elindítva.</span><span class="sxs-lookup"><span data-stu-id="ef120-203">Your default browser is launched with hello URL toohello deployed app.</span></span> 

<span data-ttu-id="ef120-204">Adjon hozzá néhány Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="ef120-204">Add a few to-do items.</span></span>

![Közzétett ASP.NET-alkalmazást az Azure-webalkalmazásban](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

<span data-ttu-id="ef120-206">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="ef120-206">Congratulations!</span></span> <span data-ttu-id="ef120-207">Az adatvezérelt ASP.NET-alkalmazás fut élő az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="ef120-207">Your data-driven ASP.NET application is running live in Azure App Service.</span></span>

## <a name="access-hello-sql-database-locally"></a><span data-ttu-id="ef120-208">SQL-adatbázis Access-hello helyileg</span><span class="sxs-lookup"><span data-stu-id="ef120-208">Access hello SQL Database locally</span></span>

<span data-ttu-id="ef120-209">A Visual Studio lehetővé teszi, hogy vizsgálatát, és az új SQL-adatbázis egyszerűen a hello kezelése **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ef120-209">Visual Studio lets you explore and manage your new SQL Database easily in hello **SQL Server Object Explorer**.</span></span>

### <a name="create-a-database-connection"></a><span data-ttu-id="ef120-210">Adatbázis-kapcsolat létrehozása</span><span class="sxs-lookup"><span data-stu-id="ef120-210">Create a database connection</span></span>

<span data-ttu-id="ef120-211">A hello **nézet** menü **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ef120-211">From hello **View** menu, select **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="ef120-212">Hello felső **SQL Server Object Explorer**, kattintson a hello **SQL-kiszolgáló hozzáadása** gombra.</span><span class="sxs-lookup"><span data-stu-id="ef120-212">At hello top of **SQL Server Object Explorer**, click hello **Add SQL Server** button.</span></span>

### <a name="configure-hello-database-connection"></a><span data-ttu-id="ef120-213">Hello adatbázis-kapcsolat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ef120-213">Configure hello database connection</span></span>

<span data-ttu-id="ef120-214">A hello **Connect** párbeszédpanelen bontsa ki a hello **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="ef120-214">In hello **Connect** dialog, expand hello **Azure** node.</span></span> <span data-ttu-id="ef120-215">Az SQL-adatbázis példányainak az Azure-ban az itt felsorolt.</span><span class="sxs-lookup"><span data-stu-id="ef120-215">All your SQL Database instances in Azure are listed here.</span></span>

<span data-ttu-id="ef120-216">Jelölje be hello `DotNetAppSqlDb` SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ef120-216">Select hello `DotNetAppSqlDb` SQL Database.</span></span> <span data-ttu-id="ef120-217">a korábban létrehozott hello kapcsolatot a rendszer automatikusan kitölti hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="ef120-217">hello connection you created earlier is automatically filled at hello bottom.</span></span>

<span data-ttu-id="ef120-218">Írja be a korábban létrehozott hello adatbázis rendszergazdai jelszót, és kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="ef120-218">Type hello database administrator password you created earlier and click **Connect**.</span></span>

![A Visual Studio adatbázis-kapcsolat konfigurálása](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a><span data-ttu-id="ef120-220">A számítógép az ügyfél-kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ef120-220">Allow client connection from your computer</span></span>

<span data-ttu-id="ef120-221">Hello **hozzon létre egy új tűzfalszabályt** párbeszédpanel már meg van nyitva.</span><span class="sxs-lookup"><span data-stu-id="ef120-221">hello **Create a new firewall rule** dialog is opened.</span></span> <span data-ttu-id="ef120-222">Alapértelmezés szerint az SQL Database-példányt csak Azure-szolgáltatások, például az Azure-webalkalmazásban kapcsolatokat engedélyez.</span><span class="sxs-lookup"><span data-stu-id="ef120-222">By default, your SQL Database instance only allows connections from Azure services, such as your Azure web app.</span></span> <span data-ttu-id="ef120-223">tooconnect tooyour adatbázisra, és hozzon létre egy tűzfalszabályt hello SQL-adatbázispéldány.</span><span class="sxs-lookup"><span data-stu-id="ef120-223">tooconnect tooyour database, create a firewall rule in hello SQL Database instance.</span></span> <span data-ttu-id="ef120-224">hello tűzfalszabály lehetővé teszi, hogy a helyi számítógép hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ef120-224">hello firewall rule allows hello public IP address of your local computer.</span></span>

<span data-ttu-id="ef120-225">hello párbeszédpanel már ki van töltve, a számítógép nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="ef120-225">hello dialog is already filled with your computer's public IP address.</span></span>

<span data-ttu-id="ef120-226">Győződjön meg arról, hogy **a ügyfél IP-cím hozzáadása** van kiválasztva, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef120-226">Make sure that **Add my client IP** is selected and click **OK**.</span></span> 

![SQL Database-példányt a tűzfal beállítása](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

<span data-ttu-id="ef120-228">Visual Studio végzett a létrehozással hello tűzfal beállítása a SQL Database-példányt, ha a kapcsolat megjelennek **SQL Server Object Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ef120-228">Once Visual Studio finishes creating hello firewall setting for your SQL Database instance, your connection shows up in **SQL Server Object Explorer**.</span></span>

<span data-ttu-id="ef120-229">Itt végezheti el hello leggyakoribb adatbázis műveletek, például a lekérdezések futtatása hozhat létre, nézetek és tárolt eljárások, és több.</span><span class="sxs-lookup"><span data-stu-id="ef120-229">Here, you can perform hello most common database operations, such as run queries, create views and stored procedures, and more.</span></span> 

<span data-ttu-id="ef120-230">Kattintson a jobb gombbal a hello `Todoes` tábla, és válassza ki **adatok megtekintéséhez**.</span><span class="sxs-lookup"><span data-stu-id="ef120-230">Right-click on hello `Todoes` table and select **View Data**.</span></span> 

![Fedezze fel az SQL adatbázis-objektumok](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a><span data-ttu-id="ef120-232">Frissítse app Code First áttelepítést</span><span class="sxs-lookup"><span data-stu-id="ef120-232">Update app with Code First Migrations</span></span>

<span data-ttu-id="ef120-233">Használhatja a Visual Studio tooupdate hello a jól ismert eszközökkel az adatbázis és a webes alkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ef120-233">You can use hello familiar tools in Visual Studio tooupdate your database and web app in Azure.</span></span> <span data-ttu-id="ef120-234">Ebben a lépésben az Entity Framework toomake módosítás tooyour adatbázisséma Code First áttelepítést használni, és tegye közzé azt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="ef120-234">In this step, you use Code First Migrations in Entity Framework toomake a change tooyour database schema and publish it tooAzure.</span></span>

<span data-ttu-id="ef120-235">Entity Framework Code First áttelepítést használatával kapcsolatos további információkért lásd: [Ismerkedés az Entity Framework 6 Code First MVC 5-öt használó](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span><span class="sxs-lookup"><span data-stu-id="ef120-235">For more information about using Entity Framework Code First Migrations, see [Getting Started with Entity Framework 6 Code First using MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

### <a name="update-your-data-model"></a><span data-ttu-id="ef120-236">Az adatmodell frissítése</span><span class="sxs-lookup"><span data-stu-id="ef120-236">Update your data model</span></span>

<span data-ttu-id="ef120-237">Nyissa meg _Models\Todo.cs_ hello kód szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="ef120-237">Open _Models\Todo.cs_ in hello code editor.</span></span> <span data-ttu-id="ef120-238">Adja hozzá a következő tulajdonság toohello hello `ToDo` osztály:</span><span class="sxs-lookup"><span data-stu-id="ef120-238">Add hello following property toohello `ToDo` class:</span></span>

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a><span data-ttu-id="ef120-239">Code First áttelepítést helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="ef120-239">Run Code First Migrations locally</span></span>

<span data-ttu-id="ef120-240">Néhány parancsok toomake frissítések tooyour helyi adatbázis futtatásához.</span><span class="sxs-lookup"><span data-stu-id="ef120-240">Run a few commands toomake updates tooyour local database.</span></span> 

<span data-ttu-id="ef120-241">A hello **eszközök** menüben kattintson a **NuGet-Csomagkezelő** > **Csomagkezelő konzol**.</span><span class="sxs-lookup"><span data-stu-id="ef120-241">From hello **Tools** menu, click **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="ef120-242">A Package Manager Console ablakban hello engedélyezze a Code First áttelepítést:</span><span class="sxs-lookup"><span data-stu-id="ef120-242">In hello Package Manager Console window, enable Code First Migrations:</span></span>

```PowerShell
Enable-Migrations
```

<span data-ttu-id="ef120-243">Vegye fel az áttelepítés:</span><span class="sxs-lookup"><span data-stu-id="ef120-243">Add a migration:</span></span>

```PowerShell
Add-Migration AddProperty
```

<span data-ttu-id="ef120-244">Hello helyi adatbázis frissítése:</span><span class="sxs-lookup"><span data-stu-id="ef120-244">Update hello local database:</span></span>

```PowerShell
Update-Database
```

<span data-ttu-id="ef120-245">Típus `Ctrl+F5` toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ef120-245">Type `Ctrl+F5` toorun hello app.</span></span> <span data-ttu-id="ef120-246">Teszt hello adatok szerkesztéséhez, és hivatkozások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ef120-246">Test hello edit, details, and create links.</span></span>

<span data-ttu-id="ef120-247">Hello alkalmazás betölt nem jelenik meg hibaüzenet, ha Code First áttelepítést sikeresen befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ef120-247">If hello application loads without errors, then Code First Migrations has succeeded.</span></span> <span data-ttu-id="ef120-248">Azonban a lap továbbra is keres hello ugyanaz, mert az alkalmazás logikáját még nem használja ezt az új tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="ef120-248">However, your page still looks hello same because your application logic is not using this new property yet.</span></span> 

### <a name="use-hello-new-property"></a><span data-ttu-id="ef120-249">Hello új tulajdonsággal</span><span class="sxs-lookup"><span data-stu-id="ef120-249">Use hello new property</span></span>

<span data-ttu-id="ef120-250">Módosítja a kód toouse hello `Done` tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ef120-250">Make some changes in your code toouse hello `Done` property.</span></span> <span data-ttu-id="ef120-251">Az egyszerűség kedvéért ebben az oktatóanyagban, csak fog toochange hello `Index` és `Create` toosee hello tulajdonság megtekinti a művelet.</span><span class="sxs-lookup"><span data-stu-id="ef120-251">For simplicity in this tutorial, you're only going toochange hello `Index` and `Create` views toosee hello property in action.</span></span>

<span data-ttu-id="ef120-252">Nyissa meg _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="ef120-252">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="ef120-253">Hello található `Create()` metódust, és adja hozzá `Done` hello tulajdonságok listája toohello `Bind` attribútum.</span><span class="sxs-lookup"><span data-stu-id="ef120-253">Find hello `Create()` method and add `Done` toohello list of properties in hello `Bind` attribute.</span></span> <span data-ttu-id="ef120-254">Amikor elkészült, a `Create()` metódus-aláírás néz hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="ef120-254">When you're done, your `Create()` method signature looks like hello following code:</span></span>

```csharp
public ActionResult Create([Bind(Include = "id,Description,CreatedDate,Done")] Todo todo)
```

<span data-ttu-id="ef120-255">Nyissa meg _Views\Todos\Create.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="ef120-255">Open _Views\Todos\Create.cshtml_.</span></span>

<span data-ttu-id="ef120-256">A hello Razor kódot, megjelenik egy `<div class="form-group">` elem által használt `model.Description`, és majd egy másik `<div class="form-group">` elem által használt `model.CreatedDate`.</span><span class="sxs-lookup"><span data-stu-id="ef120-256">In hello Razor code, you should see a `<div class="form-group">` element that uses `model.Description`, and then another `<div class="form-group">` element that uses `model.CreatedDate`.</span></span> <span data-ttu-id="ef120-257">Ez a két elem követő hozzáadása egy másik `<div class="form-group">` elem által használt `model.Done`:</span><span class="sxs-lookup"><span data-stu-id="ef120-257">Immediately following these two elements, add another `<div class="form-group">` element that uses `model.Done`:</span></span>

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

<span data-ttu-id="ef120-258">Nyissa meg _Views\Todos\Index.cshtml_.</span><span class="sxs-lookup"><span data-stu-id="ef120-258">Open _Views\Todos\Index.cshtml_.</span></span>

<span data-ttu-id="ef120-259">Keresse meg az üres hello `<th></th>` elemet.</span><span class="sxs-lookup"><span data-stu-id="ef120-259">Search for hello empty `<th></th>` element.</span></span> <span data-ttu-id="ef120-260">Ez az elem felett Razor kód a következő hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ef120-260">Just above this element, add hello following Razor code:</span></span>

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

<span data-ttu-id="ef120-261">Hello található `<td>` elem, amely tartalmazza a hello `Html.ActionLink()` segédmódszereket.</span><span class="sxs-lookup"><span data-stu-id="ef120-261">Find hello `<td>` element that contains hello `Html.ActionLink()` helper methods.</span></span> <span data-ttu-id="ef120-262">Ez az elem felett Razor kód a következő hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ef120-262">Just above this element, add hello following Razor code:</span></span>

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

<span data-ttu-id="ef120-263">Ez minden toosee hello változásai hello kell `Index` és `Create` nézetek.</span><span class="sxs-lookup"><span data-stu-id="ef120-263">That's all you need toosee hello changes in hello `Index` and `Create` views.</span></span> 

<span data-ttu-id="ef120-264">Típus `Ctrl+F5` toorun hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ef120-264">Type `Ctrl+F5` toorun hello app.</span></span>

<span data-ttu-id="ef120-265">Ezután egy teendő hozzáadása és ellenőrzése **végzett**.</span><span class="sxs-lookup"><span data-stu-id="ef120-265">You can now add a to-do item and check **Done**.</span></span> <span data-ttu-id="ef120-266">Ezután azt kell jelennek meg az Ön újragondolt befejezett elemként.</span><span class="sxs-lookup"><span data-stu-id="ef120-266">Then it should show up in your homepage as a completed item.</span></span> <span data-ttu-id="ef120-267">Ne feledje, hogy hello `Edit` nézet nem jeleníti meg hello `Done` mezőben, már nem módosíthatók hello `Edit` nézet.</span><span class="sxs-lookup"><span data-stu-id="ef120-267">Remember that hello `Edit` view doesn't show hello `Done` field, because you didn't change hello `Edit` view.</span></span>

### <a name="enable-code-first-migrations-in-azure"></a><span data-ttu-id="ef120-268">Engedélyezze a Code First áttelepítést az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ef120-268">Enable Code First Migrations in Azure</span></span>

<span data-ttu-id="ef120-269">Most, hogy a kód módosítása működéséről, beleértve az adatbázis az áttelepítés akkor közzéteszi az Azure-webalkalmazásban tooyour, és frissítse az SQL-adatbázis Code First áttelepítést túl.</span><span class="sxs-lookup"><span data-stu-id="ef120-269">Now that your code change works, including database migration, you publish it tooyour Azure web app and update your SQL Database with Code First Migrations too.</span></span>

<span data-ttu-id="ef120-270">Fentiekhez hasonló, kattintson jobb gombbal a projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ef120-270">Just like before, right-click your project and select **Publish**.</span></span>

<span data-ttu-id="ef120-271">Kattintson a **beállítások** tooopen hello közzététele varázsló.</span><span class="sxs-lookup"><span data-stu-id="ef120-271">Click **Settings** tooopen hello publish wizard.</span></span>

![Nyissa meg a közzétételi beállítások](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

<span data-ttu-id="ef120-273">Hello varázslóban kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ef120-273">In hello wizard, click **Next**.</span></span>

<span data-ttu-id="ef120-274">Ellenőrizze, hogy hello kapcsolati karakterlánc az SQL-adatbázis nem üres **MyDatabaseContext (MyDbConnection)**.</span><span class="sxs-lookup"><span data-stu-id="ef120-274">Make sure that hello connection string for your SQL Database is populated in **MyDatabaseContext (MyDbConnection)**.</span></span> <span data-ttu-id="ef120-275">Előfordulhat, hogy tooselect hello **myToDoAppDb** adatbázis hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="ef120-275">You may need tooselect hello **myToDoAppDb** database from hello dropdown.</span></span> 

<span data-ttu-id="ef120-276">Válassza ki **hajtható végre Code First áttelepítést (fut az alkalmazás indítása)**, majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ef120-276">Select **Execute Code First Migrations (runs on application start)**, then click **Save**.</span></span>

![Az Azure web app alkalmazásban Code First áttelepítést engedélyezése](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a><span data-ttu-id="ef120-278">A változtatások közzététele</span><span class="sxs-lookup"><span data-stu-id="ef120-278">Publish your changes</span></span>

<span data-ttu-id="ef120-279">Most, hogy Code First áttelepítést az Azure web app alkalmazásban engedélyezve van, a kód változtatások közzététele.</span><span class="sxs-lookup"><span data-stu-id="ef120-279">Now that you enabled Code First Migrations in your Azure web app, publish your code changes.</span></span>

<span data-ttu-id="ef120-280">Hello közzéteszi a lapot, majd kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ef120-280">In hello publish page, click **Publish**.</span></span>

<span data-ttu-id="ef120-281">Próbálja meg újból hozzáadni a Tennivalólista elemein, és válassza ki **végzett**, és Ön újragondolt befejezett elemként jelenniük.</span><span class="sxs-lookup"><span data-stu-id="ef120-281">Try adding to-do items again and select **Done**, and they should show up in your homepage as a completed item.</span></span>

![Azure-webalkalmazásban kód első áttelepítés után](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

<span data-ttu-id="ef120-283">A meglévő Tennivalólista elemein továbbra is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="ef120-283">All your existing to-do items are still displayed.</span></span> <span data-ttu-id="ef120-284">Ha az ASP.NET-alkalmazás közzétételéhez meglévő az SQL-adatbázis adatai nem elveszett.</span><span class="sxs-lookup"><span data-stu-id="ef120-284">When you republish your ASP.NET application, existing data in your SQL Database is not lost.</span></span> <span data-ttu-id="ef120-285">Emellett Code First áttelepítést csak akkor változik meg hello adatséma és a meglévő adatok sértetlenek.</span><span class="sxs-lookup"><span data-stu-id="ef120-285">Also, Code First Migrations only changes hello data schema and leaves your existing data intact.</span></span>


## <a name="stream-application-logs"></a><span data-ttu-id="ef120-286">Az adatfolyam alkalmazásnaplók</span><span class="sxs-lookup"><span data-stu-id="ef120-286">Stream application logs</span></span>

<span data-ttu-id="ef120-287">Az Azure web app tooVisual Studio közvetlenül a nyomkövetési üzeneteket is adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="ef120-287">You can stream tracing messages directly from your Azure web app tooVisual Studio.</span></span>

<span data-ttu-id="ef120-288">Nyissa meg _Controllers\TodosController.cs_.</span><span class="sxs-lookup"><span data-stu-id="ef120-288">Open _Controllers\TodosController.cs_.</span></span>

<span data-ttu-id="ef120-289">Minden egyes művelethez kezdődik-e a `Trace.WriteLine()` metódust.</span><span class="sxs-lookup"><span data-stu-id="ef120-289">Each action starts with a `Trace.WriteLine()` method.</span></span> <span data-ttu-id="ef120-290">Ezzel a kóddal egészül ki tooshow, hogyan tooadd nyomkövetési üzenetek tooyour Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ef120-290">This code is added tooshow you how tooadd trace messages tooyour Azure web app.</span></span>

### <a name="open-server-explorer"></a><span data-ttu-id="ef120-291">Nyissa meg a Server Explorer</span><span class="sxs-lookup"><span data-stu-id="ef120-291">Open Server Explorer</span></span>

<span data-ttu-id="ef120-292">A hello **nézet** menü **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ef120-292">From hello **View** menu, select **Server Explorer**.</span></span> <span data-ttu-id="ef120-293">Naplózás konfigurálása az Azure-webalkalmazásban a **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ef120-293">You can configure logging for your Azure web app in **Server Explorer**.</span></span> 

### <a name="enable-log-streaming"></a><span data-ttu-id="ef120-294">Adatfolyamként-napló</span><span class="sxs-lookup"><span data-stu-id="ef120-294">Enable log streaming</span></span>

<span data-ttu-id="ef120-295">A **Server Explorer**, bontsa ki a **Azure** > **App Service**.</span><span class="sxs-lookup"><span data-stu-id="ef120-295">In **Server Explorer**, expand **Azure** > **App Service**.</span></span>

<span data-ttu-id="ef120-296">Bontsa ki a hello **myResourceGroup** erőforráscsoportot, a létrehozott Azure-webalkalmazásban hello kezdeti létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="ef120-296">Expand hello **myResourceGroup** resource group, you created when you first created hello Azure web app.</span></span>

<span data-ttu-id="ef120-297">Kattintson a jobb gombbal az Azure-webalkalmazásban, és válassza ki **folyamatos átviteli naplók megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="ef120-297">Right-click your Azure web app and select **View Streaming Logs**.</span></span>

![Adatfolyamként-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

<span data-ttu-id="ef120-299">hello naplók mostantól átvitt be hello **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="ef120-299">hello logs are now streamed into hello **Output** window.</span></span> 

![A kimeneti ablakban adatfolyam-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

<span data-ttu-id="ef120-301">Azonban nem jelennek meg a nyomkövetési köszönőüzenetei még.</span><span class="sxs-lookup"><span data-stu-id="ef120-301">However, you don't see any of hello trace messages yet.</span></span> <span data-ttu-id="ef120-302">Meg mert először kiválasztásakor **folyamatos átviteli naplók megtekintése**, az Azure-webalkalmazásban túl hello nyomkövetési szint beállítása`Error`, amely csak naplózza hibaesemények (a hello `Trace.TraceError()` metódus).</span><span class="sxs-lookup"><span data-stu-id="ef120-302">That's because when you first select **View Streaming Logs**, your Azure web app sets hello trace level too`Error`, which only logs error events (with hello `Trace.TraceError()` method).</span></span>

### <a name="change-trace-levels"></a><span data-ttu-id="ef120-303">Nyomkövetési szintek módosítása</span><span class="sxs-lookup"><span data-stu-id="ef120-303">Change trace levels</span></span>

<span data-ttu-id="ef120-304">toochange hello nyomkövetési szintek toooutput többi nyomkövetési üzenet, lépjen vissza túl**Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ef120-304">toochange hello trace levels toooutput other trace messages, go back too**Server Explorer**.</span></span>

<span data-ttu-id="ef120-305">Az Azure-webalkalmazás ismét gombbal és válassza ki **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ef120-305">Right-click your Azure web app again and select **Settings**.</span></span>

<span data-ttu-id="ef120-306">A hello **Alkalmazásnaplózást (fájlrendszer)** legördülő menüből válassza **részletes**.</span><span class="sxs-lookup"><span data-stu-id="ef120-306">In hello **Application Logging (File System)** dropdown, select **Verbose**.</span></span> <span data-ttu-id="ef120-307">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="ef120-307">Click **Save**.</span></span>

![A nyomkövetési szint tooVerbose módosítása](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> <span data-ttu-id="ef120-309">Kísérletezhet különböző nyomkövetési szintek toosee egyes szinteken milyen típusú üzenetek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ef120-309">You can experiment with different trace levels toosee what types of messages are displayed for each level.</span></span> <span data-ttu-id="ef120-310">Például hello **információk** szint továbbít minden által létrehozott naplók `Trace.TraceInformation()`, `Trace.TraceWarning()`, és `Trace.TraceError()`, de nem által létrehozott naplók `Trace.WriteLine()`.</span><span class="sxs-lookup"><span data-stu-id="ef120-310">For example, hello **Information** level includes all logs created by `Trace.TraceInformation()`, `Trace.TraceWarning()`, and `Trace.TraceError()`, but not logs created by `Trace.WriteLine()`.</span></span>
>
>

<span data-ttu-id="ef120-311">A böngészőben kattintson körül hello tennivalók listája alkalmazás az Azure-ban próbálja.</span><span class="sxs-lookup"><span data-stu-id="ef120-311">In your browser, try clicking around hello to-do list application in Azure.</span></span> <span data-ttu-id="ef120-312">nyomkövetési köszönőüzenetei most átvitt toohello **kimeneti** ablak a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="ef120-312">hello trace messages are now streamed toohello **Output** window in Visual Studio.</span></span>

```
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a><span data-ttu-id="ef120-313">Állítsa le az adatfolyam-napló</span><span class="sxs-lookup"><span data-stu-id="ef120-313">Stop log streaming</span></span>

<span data-ttu-id="ef120-314">toostop hello napló-adatfolyam-szolgáltatás, kattintson a hello **figyelés leállításának** hello gombjára **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="ef120-314">toostop hello log-streaming service, click hello **Stop monitoring** button in hello **Output** window.</span></span>

![Állítsa le az adatfolyam-napló](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a><span data-ttu-id="ef120-316">Az Azure-webalkalmazásban kezelése</span><span class="sxs-lookup"><span data-stu-id="ef120-316">Manage your Azure web app</span></span>

<span data-ttu-id="ef120-317">Nyissa meg toohello [Azure-portálon](https://portal.azure.com) toosee hello létrehozott webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ef120-317">Go toohello [Azure portal](https://portal.azure.com) toosee hello web app you created.</span></span> 



<span data-ttu-id="ef120-318">Hello bal oldali menüben kattintson **App Service**, majd kattintson az Azure-webalkalmazásban hello nevére.</span><span class="sxs-lookup"><span data-stu-id="ef120-318">From hello left menu, click **App Service**, then click hello name of your Azure web app.</span></span>

![Portálnavigációjával tooAzure webalkalmazás](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

<span data-ttu-id="ef120-320">A webalkalmazás lapon rendelkezik ki.</span><span class="sxs-lookup"><span data-stu-id="ef120-320">You have landed in your web app's page.</span></span> 

<span data-ttu-id="ef120-321">Alapértelmezés szerint hello portal mutatja hello **áttekintése** lap.</span><span class="sxs-lookup"><span data-stu-id="ef120-321">By default, hello portal shows hello **Overview** page.</span></span> <span data-ttu-id="ef120-322">Ezen az oldalon megtekintheti az alkalmazás állapotát.</span><span class="sxs-lookup"><span data-stu-id="ef120-322">This page gives you a view of how your app is doing.</span></span> <span data-ttu-id="ef120-323">Itt elvégezhet olyan alapszintű felügyeleti feladatokat is, mint a böngészés, leállítás, elindítás, újraindítás és törlés.</span><span class="sxs-lookup"><span data-stu-id="ef120-323">Here, you can also perform basic management tasks like browse, stop, start, restart, and delete.</span></span> <span data-ttu-id="ef120-324">hello lapok hello bal oldalán található hello hello különböző konfigurációs lapok megnyithatja megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ef120-324">hello tabs on hello left side of hello page show hello different configuration pages you can open.</span></span> 

![Az App Service lap az Azure Portalon](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a><span data-ttu-id="ef120-326">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ef120-326">Next steps</span></span>

<span data-ttu-id="ef120-327">Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ef120-327">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ef120-328">SQL-adatbázis létrehozása az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="ef120-328">Create a SQL Database in Azure</span></span>
> * <span data-ttu-id="ef120-329">Csatlakozás egy ASP.NET alkalmazás tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="ef120-329">Connect an ASP.NET app tooSQL Database</span></span>
> * <span data-ttu-id="ef120-330">Hello app tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="ef120-330">Deploy hello app tooAzure</span></span>
> * <span data-ttu-id="ef120-331">Hello adatmodell frissítése, és telepítse újra hello alkalmazást</span><span class="sxs-lookup"><span data-stu-id="ef120-331">Update hello data model and redeploy hello app</span></span>
> * <span data-ttu-id="ef120-332">Stream naplók az Azure tooyour terminál</span><span class="sxs-lookup"><span data-stu-id="ef120-332">Stream logs from Azure tooyour terminal</span></span>
> * <span data-ttu-id="ef120-333">Az Azure-portálon hello hello alkalmazás kezelése</span><span class="sxs-lookup"><span data-stu-id="ef120-333">Manage hello app in hello Azure portal</span></span>

<span data-ttu-id="ef120-334">Előzetes toohello következő útmutató toolearn hogyan toomap egy egyéni DNS name toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ef120-334">Advance toohello next tutorial toolearn how toomap a custom DNS name toohello web app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="ef120-335">Egy meglévő egyéni DNS nevét tooAzure webalkalmazások leképezése</span><span class="sxs-lookup"><span data-stu-id="ef120-335">Map an existing custom DNS name tooAzure Web Apps</span></span>](app-service-web-tutorial-custom-domain.md)
