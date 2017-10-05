---
title: "Bottle és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio"
description: "Ismerje meg, a Python Tools for Visual Studio használatával hozhat létre, amely tárolja az adatokat az Azure Table Storage Bottle alkalmazást, és a webalkalmazás telepítése az Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: f075124b-db79-4e51-b394-09187dd6c634
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: fb25f03607ac6e9af46b47f54e830e0283dd1b0a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="98921-103">Bottle és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="98921-103">Bottle and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="98921-104">Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] hozhat létre egy egyszerű szavazások webalkalmazás használja a PTVS-minta sablonok egyikét.</span><span class="sxs-lookup"><span data-stu-id="98921-104">In this tutorial, we'll use [Python Tools for Visual Studio] to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="98921-105">Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=GJXDGaEPy94).</span><span class="sxs-lookup"><span data-stu-id="98921-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=GJXDGaEPy94).</span></span>

<span data-ttu-id="98921-106">A szavazási webalkalmazást a tárházához absztrakciós határozza meg, így egyszerűen átválthatja különböző típusú tárházak (a memóriában, Azure Table Storage MongoDB) között.</span><span class="sxs-lookup"><span data-stu-id="98921-106">The polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="98921-107">A Microsoft Azure Storage-fiók létrehozása, a webalkalmazás Azure Table Storage használata konfigurálása és közzététele a webalkalmazás a megtanulhatja [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="98921-107">We'll learn how to create an Azure Storage account, how to configure the web app to use Azure Table Storage, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="98921-108">Tekintse meg a [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, a MongoDB, az Azure Table Storage, a MySQL és a SQL Database szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="98921-108">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="98921-109">Az App Service-t tárgyaló jelen cikkben szereplő lépések hasonlóak az [Azure Cloud Services] fejlesztése esetében használtakhoz.</span><span class="sxs-lookup"><span data-stu-id="98921-109">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98921-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="98921-110">Prerequisites</span></span>
* <span data-ttu-id="98921-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="98921-111">Visual Studio 2015</span></span>
* <span data-ttu-id="98921-112">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="98921-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="98921-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="98921-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="98921-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="98921-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="98921-115">[Python 2.7 32 bites] vagy [Python 3.4 32 bites]</span><span class="sxs-lookup"><span data-stu-id="98921-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="98921-116">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="98921-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="98921-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="98921-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-the-project"></a><span data-ttu-id="98921-118">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="98921-118">Create the Project</span></span>
<span data-ttu-id="98921-119">Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával.</span><span class="sxs-lookup"><span data-stu-id="98921-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="98921-120">Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="98921-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="98921-121">Majd helyszínekről futtassuk az alkalmazást helyileg az alapértelmezett memórián belüli tárházba.</span><span class="sxs-lookup"><span data-stu-id="98921-121">Then we'll run the application locally using the default in-memory repository.</span></span>

1. <span data-ttu-id="98921-122">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="98921-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="98921-123">A [Python Tools 2.2 for Visual Studio Samples VSIX] projektsablonjai a **Python**, **Példák** elem alatt érhetők el.</span><span class="sxs-lookup"><span data-stu-id="98921-123">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="98921-124">Válassza ki **szavazások Bottle webes projekt** kattintson az OK gombra a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="98921-124">Select **Polls Bottle Web Project** and click OK to create the project.</span></span>
   
     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. <span data-ttu-id="98921-126">A rendszer fel fogja kérni külső csomagok telepítésére.</span><span class="sxs-lookup"><span data-stu-id="98921-126">You will be prompted to install external packages.</span></span> <span data-ttu-id="98921-127">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="98921-127">Select **Install into a virtual environment**.</span></span>
   
     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. <span data-ttu-id="98921-129">Alapszintű értelmezőként válassza ki a **Python 2.7** vagy **Python 3.4** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="98921-129">Select **Python 2.7** or **Python 3.4** as the base interpreter.</span></span>
   
     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="98921-131">Az alkalmazás működőképességét az `F5` billentyű lenyomásával ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="98921-131">Confirm that the application works by pressing `F5`.</span></span> <span data-ttu-id="98921-132">Alapértelmezés szerint az alkalmazás használja egy memórián belüli tárház, amelyhez nincs szükség beállításra.</span><span class="sxs-lookup"><span data-stu-id="98921-132">By default, the application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="98921-133">Minden adat elveszik, amikor a webkiszolgáló leáll.</span><span class="sxs-lookup"><span data-stu-id="98921-133">All data is lost when the web server is stopped.</span></span>
6. <span data-ttu-id="98921-134">Kattintson a **létrehozása Sample Polls**, majd kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="98921-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="98921-136">Az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="98921-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="98921-137">Tárolási műveletek használatához Azure-tárfiók kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="98921-137">To use storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="98921-138">Ezeket a lépéseket követve létrehozhat egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="98921-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="98921-139">Jelentkezzen be a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="98921-139">Log into the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="98921-140">Kattintson a **új** a portál bal felső ikonra, majd kattintson a **adatok + tárolás** > **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="98921-140">Click the **New** icon on the top left of the Portal, then click **Data + Storage** > **Storage Account**.</span></span>  <span data-ttu-id="98921-141">Kattintson a **létrehozása** gombra, majd a tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.</span><span class="sxs-lookup"><span data-stu-id="98921-141">Click the **Create** button, then give the storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Gyors létrehozás](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="98921-143">A storage-fiók létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és a storage-fiók panelje meg nyitva, hogy a létrehozott új erőforráscsoporthoz tartozó megjeleníthető.</span><span class="sxs-lookup"><span data-stu-id="98921-143">When the storage account has been created, the **Notifications** button will flash a green **SUCCESS** and the storage account's blade is open to show that it belongs to the new resource group you created.</span></span>
3. <span data-ttu-id="98921-144">Kattintson a **hívóbetűk** részt a storage-fiók panelen.</span><span class="sxs-lookup"><span data-stu-id="98921-144">Click the **Access keys** part in the storage account's blade.</span></span> <span data-ttu-id="98921-145">Jegyezze fel a fiók nevét és key1.</span><span class="sxs-lookup"><span data-stu-id="98921-145">Take note of the account name and key1.</span></span>
   
      ![Kulcsok](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="98921-147">Ezt az információt a projekt konfigurálásához a következő szakaszban kell azt.</span><span class="sxs-lookup"><span data-stu-id="98921-147">We will need this information to configure your project in the next section.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="98921-148">A projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="98921-148">Configure the Project</span></span>
<span data-ttu-id="98921-149">Ebben a szakaszban az imént létrehozott tárfiókot használni, az alkalmazás konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="98921-149">In this section, we'll configure our application to use the storage account we just created.</span></span> <span data-ttu-id="98921-150">Ezt követően helyileg fogja futtatni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="98921-150">Then we'll run the application locally.</span></span>

1. <span data-ttu-id="98921-151">A Visual Studióban, kattintson a jobb gombbal a projekt csomópontjára a Megoldáskezelőben, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="98921-151">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="98921-152">Kattintson a **Debug** fülre.</span><span class="sxs-lookup"><span data-stu-id="98921-152">Click on the **Debug** tab.</span></span>
   
     ![Hibakeresési Projektbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="98921-154">Állítsa be az alkalmazás által igényelt környezeti változók értékeit **Debug Server parancs**, **környezet**.</span><span class="sxs-lookup"><span data-stu-id="98921-154">Set the values of environment variables required by the application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="98921-155">A környezeti változók állítja amikor Ön **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="98921-155">This will set the environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="98921-156">Ha azt szeretné, hogy a változók adható meg, ha Ön **indítása nélkül hibakeresés**, állítsa be ugyanazokat az értékeket a **Server parancs futtatása** is.</span><span class="sxs-lookup"><span data-stu-id="98921-156">If you want the variables to be set when you **Start Without Debugging**, set the same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="98921-157">Azt is megteheti megadhatja a környezeti változók a Windows Vezérlőpulton.</span><span class="sxs-lookup"><span data-stu-id="98921-157">Alternatively, you can define environment variables using the Windows Control Panel.</span></span> <span data-ttu-id="98921-158">Ez a beállítás nagyobb Ha azt szeretné, ne tárolja a hitelesítő adatokat a forráskódban / project fájl.</span><span class="sxs-lookup"><span data-stu-id="98921-158">This is a better option if you want to avoid storing credentials in source code / project file.</span></span> <span data-ttu-id="98921-159">Vegye figyelembe, hogy szüksége lesz az alkalmazás elérhető legyen az új környezet értékek Visual Studio újraindítására.</span><span class="sxs-lookup"><span data-stu-id="98921-159">Note that you will need to restart Visual Studio for the new environment values to be available to the application.</span></span>
3. <span data-ttu-id="98921-160">A kódot, amely megvalósítja az Azure Table Storage tárház van **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="98921-160">The code that implements the Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="98921-161">Tekintse meg a [dokumentáció] Python a Table szolgáltatás használatáról további információt.</span><span class="sxs-lookup"><span data-stu-id="98921-161">See the [documentation] for more information on how to use Table Service from Python.</span></span>
4. <span data-ttu-id="98921-162">Futtassa az alkalmazást az `F5` billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="98921-162">Run the application with `F5`.</span></span> <span data-ttu-id="98921-163">A létrehozása **létrehozása Sample Polls** és a szavazás során elküldött adatok mintaszavazások Azure Table Storage-ban.</span><span class="sxs-lookup"><span data-stu-id="98921-163">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="98921-164">A Python 2.7 virtuális környezetet a Visual Studio egy kivétel break okozhat.</span><span class="sxs-lookup"><span data-stu-id="98921-164">The Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="98921-165">Nyomja le az `F5` folytatja, a webes projekt betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="98921-165">Press `F5` to continue loading the web project.</span></span>
   > 
   > 
5. <span data-ttu-id="98921-166">Keresse meg a **kapcsolatos** lapon ellenőrizze, hogy az alkalmazást használ-e a **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="98921-166">Browse to the **About** page to verify that the application is using the **Azure Table Storage** repository.</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-the-azure-table-storage"></a><span data-ttu-id="98921-168">Az Azure Table-tároló tallózása</span><span class="sxs-lookup"><span data-stu-id="98921-168">Explore the Azure Table Storage</span></span>
<span data-ttu-id="98921-169">Akkor is könnyen megtekintése és szerkesztése a Visual Studio használatával a Cloud Explorer storage-táblákat.</span><span class="sxs-lookup"><span data-stu-id="98921-169">It's easy to view and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="98921-170">Ez a szakasz a Server Explorer a szavazások alkalmazás táblák tartalmának megtekintése használjuk.</span><span class="sxs-lookup"><span data-stu-id="98921-170">In this section we'll use Server Explorer to view the contents of the polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="98921-171">Ehhez a Microsoft Azure eszközök legyen telepítve, mint a rendelkezésre álló része a [Azure SDK for .NET].</span><span class="sxs-lookup"><span data-stu-id="98921-171">This requires Microsoft Azure Tools to be installed, which are available as part of the [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="98921-172">Nyissa meg **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="98921-172">Open **Cloud Explorer**.</span></span> <span data-ttu-id="98921-173">Bontsa ki a **Tárfiókok**, a tárfiók, majd **táblák**.</span><span class="sxs-lookup"><span data-stu-id="98921-173">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="98921-175">Kattintson duplán arra a a **szavazások** vagy **lehetőségek** táblázatban tekintheti meg a tábla tartalmát egy dokumentumablakra, valamint entitások hozzáadása/eltávolítása vagy módosítása.</span><span class="sxs-lookup"><span data-stu-id="98921-175">Double-click on the **polls** or **choices** table to view the contents of the table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tábla lekérdezés eredményei](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="98921-177">A webalkalmazás közzététele az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="98921-177">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="98921-178">Az Azure .NET SDK egyszerű módot kínál a webalkalmazása az Azure App Service szolgáltatásban történő közzétételére.</span><span class="sxs-lookup"><span data-stu-id="98921-178">The Azure .NET SDK provides an easy way to deploy your web app to Azure App Service.</span></span>

1. <span data-ttu-id="98921-179">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="98921-179">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>
   
     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="98921-181">Kattintson a **Microsoft Azure Web Apps** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="98921-181">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="98921-182">A **New** (Új) gombra kattintva hozzon létre egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="98921-182">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="98921-183">Töltse ki a következő mezőket és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="98921-183">Fill in the following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="98921-184">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="98921-184">**Web App name**</span></span>
   * <span data-ttu-id="98921-185">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="98921-185">**App Service plan**</span></span>
   * <span data-ttu-id="98921-186">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="98921-186">**Resource group**</span></span>
   * <span data-ttu-id="98921-187">**Régió**</span><span class="sxs-lookup"><span data-stu-id="98921-187">**Region**</span></span>
   * <span data-ttu-id="98921-188">Hagyja változatlanul a **Database server** (Adatbázis-kiszolgáló) **No database** (Nincs adatbázis) beállítását</span><span class="sxs-lookup"><span data-stu-id="98921-188">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="98921-189">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="98921-189">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="98921-190">A webböngészőjében automatikusan a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="98921-190">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="98921-191">Ha tallózással az oldalról, láthatja, hogy az általa használt a **memórián belüli** -tárházban, nem a **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="98921-191">If you browse to the about page, you'll see that it uses the **In-Memory** repository, not the **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="98921-192">Ennek oka a környezeti változók nem úgy van beállítva, az Azure App Service Web Apps-példányon a megadott alapértelmezett értékeket használ **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="98921-192">That's because the environment variables are not set on the Web Apps instance in Azure App Service, so it uses the default values specified in **settings.py**.</span></span>

## <a name="configure-the-web-apps-instance"></a><span data-ttu-id="98921-193">Web Apps-példány beállítása</span><span class="sxs-lookup"><span data-stu-id="98921-193">Configure the Web Apps instance</span></span>
<span data-ttu-id="98921-194">Ebben a szakaszban a webalkalmazások példány környezeti változók konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="98921-194">In this section, we'll configure environment variables for the Web Apps instance.</span></span>

1. <span data-ttu-id="98921-195">A [Azure Portal], megnyitásához kattintson a webalkalmazása panelén **Tallózás** > **alkalmazásszolgáltatások** > a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="98921-195">In [Azure Portal], open the web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="98921-196">A webalkalmazás panelen kattintson **összes beállítás**, majd kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="98921-196">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="98921-197">Görgessen le a **Alkalmazásbeállítások** szakaszt, és értékeinek beállítása **TÁRHÁZ\_neve**, **tárolási\_neve** és **tárolási\_kulcs** leírtak szerint a **a projekt konfigurálásához** szakasz fenti.</span><span class="sxs-lookup"><span data-stu-id="98921-197">Scroll down to the **App settings** section and set the values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in the **Configure the project** section above.</span></span>
   
     ![Alkalmazásbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="98921-199">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="98921-199">Click on **Save**.</span></span> <span data-ttu-id="98921-200">Az értesítéseket, hogy a módosítások alkalmazása megtörtént-e fogadását követően kattintson a **Tallózás** a webes alkalmazás fő paneljén.</span><span class="sxs-lookup"><span data-stu-id="98921-200">After you've received the notifications that the changes were applied, click on **Browse** from the Web app main blade.</span></span>
5. <span data-ttu-id="98921-201">A webes alkalmazás megfelelően működik, használatával kell megjelennie a **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="98921-201">You should see the web app working as expected, using the **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="98921-202">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="98921-202">Congratulations!</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="98921-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98921-204">Next steps</span></span>
<span data-ttu-id="98921-205">Kövesse az alábbi hivatkozásokat tudjon meg többet a Python Tools for Visual Studio, a Bottle és az Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="98921-205">Follow these links to learn more about Python Tools for Visual Studio, Bottle and Azure Table Storage.</span></span>

* <span data-ttu-id="98921-206">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="98921-206">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="98921-207">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="98921-207">[Web Projects]</span></span>
  * <span data-ttu-id="98921-208">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="98921-208">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="98921-209">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="98921-209">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="98921-210">[Bottle dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="98921-210">[Bottle Documentation]</span></span>
* <span data-ttu-id="98921-211">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="98921-211">[Azure Storage]</span></span>
* <span data-ttu-id="98921-212">[Pythonhoz készült Azure SDK]</span><span class="sxs-lookup"><span data-stu-id="98921-212">[Azure SDK for Python]</span></span>
* <span data-ttu-id="98921-213">[A Table Storage szolgáltatás a Python használata]</span><span class="sxs-lookup"><span data-stu-id="98921-213">[How to Use the Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="98921-214">A változások</span><span class="sxs-lookup"><span data-stu-id="98921-214">What's changed</span></span>
* <span data-ttu-id="98921-215">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="98921-215">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python fejlesztői központ]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentáció]:../cosmos-db/table-storage-how-to-use-python.md
[A Table Storage szolgáltatás a Python használata]:../cosmos-db/table-storage-how-to-use-python.md


<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK for .NET]: http://azure.microsoft.com/downloads/
[a Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkId=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkId=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517191
[Python Tools for Visual Studio – dokumentáció]: http://aka.ms/ptvsdocs
[Bottle dokumentáció]: http://bottlepy.org/docs/dev/index.html
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026 (Távoli hibakeresés a Microsoft Azure-ban)
[Webes projektek]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service-projektek]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Pythonhoz készült Azure SDK]: https://github.com/Azure/azure-sdk-for-python
