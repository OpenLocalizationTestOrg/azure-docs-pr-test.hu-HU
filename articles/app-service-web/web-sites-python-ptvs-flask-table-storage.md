---
title: "Flask és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio"
description: "Ismerje meg, hogyan lehet adatokat tároló Azure Table Storage a Flask-webalkalmazás létrehozása a Python Tools for Visual Studio segítségével, és központilag telepítenie kell az Azure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: d8e70a29-aca1-4010-95f5-cfe769e3be06
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 2e1bc8eebd0b67b965cc70ac4b5dfe03c4720ddf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="flask-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="9b9c3-103">Flask és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9b9c3-103">Flask and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="9b9c3-104">Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] hozhat létre egy egyszerű szavazások webalkalmazás használja a PTVS-minta sablonok egyikét.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-104">In this tutorial, we'll use [Python Tools for Visual Studio] to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="9b9c3-105">Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span><span class="sxs-lookup"><span data-stu-id="9b9c3-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=qUtZWtPwbTk).</span></span>

<span data-ttu-id="9b9c3-106">A szavazási webalkalmazást a tárházához absztrakciós határozza meg, így egyszerűen átválthatja különböző típusú tárházak (a memóriában, Azure Table Storage MongoDB) között.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-106">The polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="9b9c3-107">A Microsoft Azure Storage-fiók létrehozása, a webalkalmazás Azure Table Storage használata konfigurálása és közzététele a webalkalmazás a megtanulhatja [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="9b9c3-107">We'll learn how to create an Azure Storage account, how to configure the web app to use Azure Table Storage, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="9b9c3-108">Tekintse meg a [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, a MongoDB, az Azure Table Storage, a MySQL és a SQL Database szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-108">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="9b9c3-109">Az App Service-t tárgyaló jelen cikkben szereplő lépések hasonlóak az [Azure Cloud Services] fejlesztése esetében használtakhoz.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-109">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9b9c3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9b9c3-110">Prerequisites</span></span>
* <span data-ttu-id="9b9c3-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="9b9c3-111">Visual Studio 2015</span></span>
* <span data-ttu-id="9b9c3-112">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="9b9c3-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="9b9c3-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="9b9c3-115">[Python 2.7 32 bites] vagy [Python 3.4 32 bites]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="9b9c3-116">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="9b9c3-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-the-project"></a><span data-ttu-id="9b9c3-118">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b9c3-118">Create the Project</span></span>
<span data-ttu-id="9b9c3-119">Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="9b9c3-120">Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="9b9c3-121">Majd helyszínekről futtassuk az alkalmazást helyileg az alapértelmezett memórián belüli tárházba.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-121">Then we'll run the application locally using the default in-memory repository.</span></span>

1. <span data-ttu-id="9b9c3-122">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="9b9c3-123">A [Python Tools 2.2 for Visual Studio Samples VSIX] projektsablonjai a **Python**, **Példák** elem alatt érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-123">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="9b9c3-124">Válassza ki **szavazások Flask webes projekt** kattintson az OK gombra a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-124">Select **Polls Flask Web Project** and click OK to create the project.</span></span>
   
     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)
3. <span data-ttu-id="9b9c3-126">A rendszer fel fogja kérni külső csomagok telepítésére.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-126">You will be prompted to install external packages.</span></span> <span data-ttu-id="9b9c3-127">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-127">Select **Install into a virtual environment**.</span></span>
   
     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)
4. <span data-ttu-id="9b9c3-129">Alapszintű értelmezőként válassza ki a **Python 2.7** vagy **Python 3.4** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-129">Select **Python 2.7** or **Python 3.4** as the base interpreter.</span></span>
   
     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="9b9c3-131">Az alkalmazás működőképességét az `F5` billentyű lenyomásával ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-131">Confirm that the application works by pressing `F5`.</span></span> <span data-ttu-id="9b9c3-132">Alapértelmezés szerint az alkalmazás használja egy memórián belüli tárház, amelyhez nincs szükség beállításra.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-132">By default, the application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="9b9c3-133">Minden adat elveszik, amikor a webkiszolgáló leáll.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-133">All data is lost when the web server is stopped.</span></span>
6. <span data-ttu-id="9b9c3-134">Kattintson a **létrehozása Sample Polls**, majd kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="9b9c3-136">Az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b9c3-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="9b9c3-137">Tárolási műveletek használatához Azure-tárfiók kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-137">To use storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="9b9c3-138">Ezeket a lépéseket követve létrehozhat egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="9b9c3-139">Jelentkezzen be a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="9b9c3-139">Log into the [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="9b9c3-140">Kattintson a **új** a portál bal felső ikonra, majd kattintson a **adatok + tárolás** > **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-140">Click the **New** icon on the top left of the Portal, then click **Data + Storage** > **Storage Account**.</span></span> <span data-ttu-id="9b9c3-141">Kattintson a **létrehozása**, majd a tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-141">Click on **Create**, then give the storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Gyors létrehozás](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="9b9c3-143">A storage-fiók létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és a storage-fiók panelje meg nyitva, hogy a létrehozott új erőforráscsoporthoz tartozó megjeleníthető.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-143">When the storage account has been created, the **Notifications** button will flash a green **SUCCESS** and the storage account's blade is open to show that it belongs to the new resource group you created.</span></span>
3. <span data-ttu-id="9b9c3-144">Kattintson a **hívóbetűk** részt a storage-fiók panelen.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-144">Click the **Access Keys** part in the storage account's blade.</span></span> <span data-ttu-id="9b9c3-145">Jegyezze fel a fiók nevét és a key1.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-145">Take note of the account name and the key1.</span></span>
   
      ![Kulcsok](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="9b9c3-147">Ezt az információt a projekt konfigurálásához a következő szakaszban kell azt.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-147">We will need this information to configure your project in the next section.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="9b9c3-148">A projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9b9c3-148">Configure the Project</span></span>
<span data-ttu-id="9b9c3-149">Ebben a szakaszban az imént létrehozott tárfiókot használni, az alkalmazás konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-149">In this section, we'll configure our application to use the storage account we just created.</span></span> <span data-ttu-id="9b9c3-150">Megtanulhatja az Azure portálról kapcsolódási beállítások beszerzése.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-150">We'll see how to obtain connection settings from the Azure Portal.</span></span> <span data-ttu-id="9b9c3-151">Ezt követően helyileg fogja futtatni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-151">Then we'll run the application locally.</span></span>

1. <span data-ttu-id="9b9c3-152">A Visual Studióban, kattintson a jobb gombbal a projekt csomópontjára a Megoldáskezelőben, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-152">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="9b9c3-153">Kattintson a **Debug** fülre.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-153">Click on the **Debug** tab.</span></span>
   
     ![Hibakeresési Projektbeállítások](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="9b9c3-155">Állítsa be az alkalmazás által igényelt környezeti változók értékeit **Debug Server parancs**, **környezet**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-155">Set the values of environment variables required by the application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="9b9c3-156">A környezeti változók állítja amikor Ön **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-156">This will set the environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="9b9c3-157">Ha azt szeretné, hogy a változók adható meg, ha Ön **indítása nélkül hibakeresés**, állítsa be ugyanazokat az értékeket a **Server parancs futtatása** is.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-157">If you want the variables to be set when you **Start Without Debugging**, set the same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="9b9c3-158">Azt is megteheti megadhatja a környezeti változók a Windows Vezérlőpulton.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-158">Alternatively, you can define environment variables using the Windows Control Panel.</span></span> <span data-ttu-id="9b9c3-159">Ez a beállítás nagyobb Ha azt szeretné, ne tárolja a hitelesítő adatokat a forráskódban / project fájl.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-159">This is a better option if you want to avoid storing credentials in source code / project file.</span></span> <span data-ttu-id="9b9c3-160">Vegye figyelembe, hogy szüksége lesz az alkalmazás elérhető legyen az új környezet értékek Visual Studio újraindítására.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-160">Note that you will need to restart Visual Studio for the new environment values to be available to the application.</span></span>
3. <span data-ttu-id="9b9c3-161">A kódot, amely megvalósítja az Azure Table Storage tárház van **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-161">The code that implements the Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="9b9c3-162">Tekintse meg a [dokumentáció] Python a Table szolgáltatás használatáról további információt.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-162">See the [documentation] for more information on how to use Table Service from Python.</span></span>
4. <span data-ttu-id="9b9c3-163">Futtassa az alkalmazást az `F5` billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-163">Run the application with `F5`.</span></span> <span data-ttu-id="9b9c3-164">A létrehozása **létrehozása Sample Polls** és a szavazás során elküldött adatok mintaszavazások Azure Table Storage-ban.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-164">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="9b9c3-165">A Python 2.7 virtuális környezetet a Visual Studio egy kivétel break okozhat.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-165">The Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="9b9c3-166">Nyomja le az `F5` folytatja, a webes projekt betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-166">Press `F5` to continue loading the web project.</span></span>
   > 
   > 
5. <span data-ttu-id="9b9c3-167">Keresse meg a **kapcsolatos** lapon ellenőrizze, hogy az alkalmazást használ-e a **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-167">Browse to the **About** page to verify that the application is using the **Azure Table Storage** repository.</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## <a name="explore-the-azure-table-storage"></a><span data-ttu-id="9b9c3-169">Az Azure Table-tároló tallózása</span><span class="sxs-lookup"><span data-stu-id="9b9c3-169">Explore the Azure Table Storage</span></span>
<span data-ttu-id="9b9c3-170">Akkor is könnyen megtekintése és szerkesztése a Visual Studio használatával a Cloud Explorer storage-táblákat.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-170">It's easy to view and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="9b9c3-171">Ez a szakasz a Server Explorer a szavazások alkalmazás táblák tartalmának megtekintése használjuk.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-171">In this section we'll use Server Explorer to view the contents of the polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="9b9c3-172">Ehhez a Microsoft Azure eszközök legyen telepítve, mint a rendelkezésre álló része a [Azure SDK for .NET].</span><span class="sxs-lookup"><span data-stu-id="9b9c3-172">This requires Microsoft Azure Tools to be installed, which are available as part of the [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="9b9c3-173">Nyissa meg **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-173">Open **Cloud Explorer**.</span></span> <span data-ttu-id="9b9c3-174">Bontsa ki a **Tárfiókok**, a tárfiók, majd **táblák**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-174">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="9b9c3-176">Kattintson duplán arra a a **szavazások** vagy **lehetőségek** táblázatban tekintheti meg a tábla tartalmát egy dokumentumablakra, valamint entitások hozzáadása/eltávolítása vagy módosítása.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-176">Double-click on the **polls** or **choices** table to view the contents of the table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tábla lekérdezés eredményei](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="9b9c3-178">A webalkalmazás közzététele az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="9b9c3-178">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="9b9c3-179">Az Azure .NET SDK egyszerű módot kínál a webalkalmazása az Azure App Service szolgáltatásban történő közzétételére.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-179">The Azure .NET SDK provides an easy way to deploy your web app to Azure App Service.</span></span>

1. <span data-ttu-id="9b9c3-180">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-180">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>
   
     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="9b9c3-182">Kattintson a **Microsoft Azure Web Apps** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="9b9c3-183">A **New** (Új) gombra kattintva hozzon létre egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-183">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="9b9c3-184">Töltse ki a következő mezőket és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-184">Fill in the following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="9b9c3-185">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="9b9c3-185">**Web App name**</span></span>
   * <span data-ttu-id="9b9c3-186">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="9b9c3-186">**App Service plan**</span></span>
   * <span data-ttu-id="9b9c3-187">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="9b9c3-187">**Resource group**</span></span>
   * <span data-ttu-id="9b9c3-188">**Régió**</span><span class="sxs-lookup"><span data-stu-id="9b9c3-188">**Region**</span></span>
   * <span data-ttu-id="9b9c3-189">Hagyja változatlanul a **Database server** (Adatbázis-kiszolgáló) **No database** (Nincs adatbázis) beállítását</span><span class="sxs-lookup"><span data-stu-id="9b9c3-189">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="9b9c3-190">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="9b9c3-191">A webböngészőjében automatikusan a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-191">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="9b9c3-192">Ha tallózással az oldalról, láthatja, hogy az általa használt a **memórián belüli** -tárházban, nem a **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-192">If you browse to the about page, you'll see that it uses the **In-Memory** repository, not the **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="9b9c3-193">Ennek oka a környezeti változók nem úgy van beállítva, az Azure App Service Web Apps-példányon a megadott alapértelmezett értékeket használ **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-193">That's because the environment variables are not set on the Web Apps instance in Azure App Service, so it uses the default values specified in **settings.py**.</span></span>

## <a name="configure-the-web-apps-instance"></a><span data-ttu-id="9b9c3-194">Web Apps-példány beállítása</span><span class="sxs-lookup"><span data-stu-id="9b9c3-194">Configure the Web Apps instance</span></span>
<span data-ttu-id="9b9c3-195">Ebben a szakaszban a webalkalmazások példány környezeti változók konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-195">In this section, we'll configure environment variables for the Web Apps instance.</span></span>

1. <span data-ttu-id="9b9c3-196">A [Azure Portal](https://portal.azure.com), megnyitásához kattintson a webalkalmazása panelén **Tallózás** > **alkalmazásszolgáltatások** > a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-196">In [Azure Portal](https://portal.azure.com), open the web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="9b9c3-197">A webalkalmazás panelen kattintson **összes beállítás**, majd kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-197">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="9b9c3-198">Görgessen le a **Alkalmazásbeállítások** szakaszt, és értékeinek beállítása **TÁRHÁZ\_neve**, **tárolási\_neve** és **tárolási\_kulcs** leírtak szerint a **a projekt konfigurálásához** szakasz fenti.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-198">Scroll down to the **App settings** section and set the values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in the **Configure the project** section above.</span></span>
   
     ![Alkalmazásbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="9b9c3-200">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-200">Click on **Save**.</span></span> <span data-ttu-id="9b9c3-201">Az értesítéseket, hogy a módosítások alkalmazása megtörtént-e fogadását követően kattintson a **Tallózás** a webes alkalmazás fő paneljén.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-201">After you've received the notifications that the changes were applied, click on **Browse** from the Web app main blade.</span></span>
5. <span data-ttu-id="9b9c3-202">A webes alkalmazás megfelelően működik, használatával kell megjelennie a **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-202">You should see the web app working as expected, using the **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="9b9c3-203">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="9b9c3-203">Congratulations!</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="9b9c3-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b9c3-205">Next steps</span></span>
<span data-ttu-id="9b9c3-206">Kövesse az alábbi hivatkozásokat tudjon meg többet a Python Tools for Visual Studio, a Flask és az Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="9b9c3-206">Follow these links to learn more about Python Tools for Visual Studio, Flask and Azure Table Storage.</span></span>

* <span data-ttu-id="9b9c3-207">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-207">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="9b9c3-208">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-208">[Web Projects]</span></span>
  * <span data-ttu-id="9b9c3-209">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-209">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="9b9c3-210">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="9b9c3-210">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="9b9c3-211">[Flask-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-211">[Flask Documentation]</span></span>
* <span data-ttu-id="9b9c3-212">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-212">[Azure Storage]</span></span>
* <span data-ttu-id="9b9c3-213">[Pythonhoz készült Azure SDK]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-213">[Azure SDK for Python]</span></span>
* <span data-ttu-id="9b9c3-214">[A Table Storage szolgáltatás a Python használata]</span><span class="sxs-lookup"><span data-stu-id="9b9c3-214">[How to Use the Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="9b9c3-215">A változások</span><span class="sxs-lookup"><span data-stu-id="9b9c3-215">What's changed</span></span>
* <span data-ttu-id="9b9c3-216">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="9b9c3-216">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python fejlesztői központ]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentáció]:../cosmos-db/table-storage-how-to-use-python.md
[A Table Storage szolgáltatás a Python használata]:../cosmos-db/table-storage-how-to-use-python.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[Azure SDK for .NET]: http://azure.microsoft.com/downloads/
[a Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?linkid=518003
[Python 2.7 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517191
[Python Tools for Visual Studio – dokumentáció]: http://aka.ms/ptvsdocs
[Flask-dokumentáció]: http://flask.pocoo.org/
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026 (Távoli hibakeresés a Microsoft Azure-ban)
[Webes projektek]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service-projektek]: http://go.microsoft.com/fwlink/?LinkId=624028
[Azure Storage]: http://azure.microsoft.com/documentation/services/storage/
[Pythonhoz készült Azure SDK]: https://github.com/Azure/azure-sdk-for-python
