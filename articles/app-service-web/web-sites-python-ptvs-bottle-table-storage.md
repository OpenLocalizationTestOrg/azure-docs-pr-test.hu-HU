---
title: "aaaBottle és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio"
description: "Ismerje meg, hogyan toouse hello Python Tools Visual Studio toocreate egy Bottle alkalmazás, amely Azure Table Storage tárolja az adatokat, és hello web app tooAzure App Service Web Apps telepítése."
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
ms.openlocfilehash: 25b9eb002b8748483d5b9458b7b5860a958b4bb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="170d5-103">Bottle és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="170d5-103">Bottle and Azure Table Storage on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="170d5-104">Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] toocreate egy egyszerű lekérdezi a web app használatával hello PTVS minta sablonok egyikét.</span><span class="sxs-lookup"><span data-stu-id="170d5-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="170d5-105">Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=GJXDGaEPy94).</span><span class="sxs-lookup"><span data-stu-id="170d5-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=GJXDGaEPy94).</span></span>

<span data-ttu-id="170d5-106">hello szavazási webalkalmazást a tárházához absztrakciós határozza meg, így egyszerűen átválthatja különböző típusú tárházak (a memóriában, Azure Table Storage MongoDB) között.</span><span class="sxs-lookup"><span data-stu-id="170d5-106">hello polls web app defines an abstraction for its repository, so you can easily switch between different types of repositories (In-Memory, Azure Table Storage, MongoDB).</span></span>

<span data-ttu-id="170d5-107">A Microsoft megtanulhatja, hogyan toocreate egy Azure Storage fiók, valamint hogyan tooconfigure hello web app toouse Azure Table Storage, és milyen toopublish hello webalkalmazás túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="170d5-107">We'll learn how toocreate an Azure Storage account, how tooconfigure hello web app toouse Azure Table Storage, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="170d5-108">Lásd: hello [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, a MongoDB, az Azure Table Storage, a MySQL és a SQL Database szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="170d5-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with MongoDB, Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="170d5-109">Amíg ez a cikk foglalkozik az App Service, hello lépések hasonlóak fejlesztésekor [Azure Felhőszolgáltatások].</span><span class="sxs-lookup"><span data-stu-id="170d5-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="170d5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="170d5-110">Prerequisites</span></span>
* <span data-ttu-id="170d5-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="170d5-111">Visual Studio 2015</span></span>
* <span data-ttu-id="170d5-112">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="170d5-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="170d5-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="170d5-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="170d5-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="170d5-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="170d5-115">[Python 2.7 32 bites] vagy [Python 3.4 32 bites]</span><span class="sxs-lookup"><span data-stu-id="170d5-115">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="170d5-116">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="170d5-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="170d5-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="170d5-117">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="170d5-118">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="170d5-118">Create hello Project</span></span>
<span data-ttu-id="170d5-119">Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával.</span><span class="sxs-lookup"><span data-stu-id="170d5-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="170d5-120">Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="170d5-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="170d5-121">Majd helyszínekről futtassuk hello alkalmazást helyileg hello alapértelmezett memórián belüli tárházba.</span><span class="sxs-lookup"><span data-stu-id="170d5-121">Then we'll run hello application locally using hello default in-memory repository.</span></span>

1. <span data-ttu-id="170d5-122">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="170d5-122">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="170d5-123">a hello projektsablonjai hello [Python Tools 2.2 for Visual Studio Samples VSIX] alatt érhetők el **Python**, **minták**.</span><span class="sxs-lookup"><span data-stu-id="170d5-123">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="170d5-124">Válassza ki **szavazások Bottle webes projekt** , és kattintson az OK toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="170d5-124">Select **Polls Bottle Web Project** and click OK toocreate hello project.</span></span>
   
     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. <span data-ttu-id="170d5-126">Külső csomagok felszólító tooinstall lesz.</span><span class="sxs-lookup"><span data-stu-id="170d5-126">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="170d5-127">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="170d5-127">Select **Install into a virtual environment**.</span></span>
   
     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. <span data-ttu-id="170d5-129">Válassza ki **Python 2.7** vagy **Python 3.4** , hello alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="170d5-129">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="170d5-131">Győződjön meg arról, hogy működik-e hello alkalmazás billentyűkombináció lenyomásával `F5`.</span><span class="sxs-lookup"><span data-stu-id="170d5-131">Confirm that hello application works by pressing `F5`.</span></span> <span data-ttu-id="170d5-132">Alapértelmezés szerint a hello alkalmazás használ egy memórián belüli tárház, amelyhez nincs szükség beállításra.</span><span class="sxs-lookup"><span data-stu-id="170d5-132">By default, hello application uses an in-memory repository which doesn't require any configuration.</span></span> <span data-ttu-id="170d5-133">Minden adat elvész, amikor hello webkiszolgáló le van állítva.</span><span class="sxs-lookup"><span data-stu-id="170d5-133">All data is lost when hello web server is stopped.</span></span>
6. <span data-ttu-id="170d5-134">Kattintson a **létrehozása Sample Polls**, majd kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="170d5-134">Click **Create Sample Polls**, then click on a poll and vote.</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="170d5-136">Az Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="170d5-136">Create an Azure Storage Account</span></span>
<span data-ttu-id="170d5-137">toouse tárolási műveletek, egy az Azure storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="170d5-137">toouse storage operations, you need an Azure storage account.</span></span> <span data-ttu-id="170d5-138">Ezeket a lépéseket követve létrehozhat egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="170d5-138">You can create a storage account by following these steps.</span></span>

1. <span data-ttu-id="170d5-139">Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="170d5-139">Log into hello [Azure Portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="170d5-140">Kattintson a hello **új** hello felül ikon hello portálon a bal oldali, majd kattintson a **adatok + tárolás** > **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="170d5-140">Click hello **New** icon on hello top left of hello Portal, then click **Data + Storage** > **Storage Account**.</span></span>  <span data-ttu-id="170d5-141">Kattintson a hello **létrehozása** gombra, majd hello tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.</span><span class="sxs-lookup"><span data-stu-id="170d5-141">Click hello **Create** button, then give hello storage account a unique name and create a new [resource group](../azure-resource-manager/resource-group-overview.md) for it.</span></span>
   
      ![Gyors létrehozás](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    <span data-ttu-id="170d5-143">Hello tárfiók létrehozásakor hello **értesítések** gomb villogjon, egy zöld **sikeres** és hello tárolási fiók panelje meg nyitva, hogy tartozik-e új erőforrás toohello tooshow csoportjának, létre.</span><span class="sxs-lookup"><span data-stu-id="170d5-143">When hello storage account has been created, hello **Notifications** button will flash a green **SUCCESS** and hello storage account's blade is open tooshow that it belongs toohello new resource group you created.</span></span>
3. <span data-ttu-id="170d5-144">Kattintson a hello **hívóbetűk** hello tárolási fiók panelen részt.</span><span class="sxs-lookup"><span data-stu-id="170d5-144">Click hello **Access keys** part in hello storage account's blade.</span></span> <span data-ttu-id="170d5-145">Jegyezze fel a hello fióknevet és key1.</span><span class="sxs-lookup"><span data-stu-id="170d5-145">Take note of hello account name and key1.</span></span>
   
      ![Kulcsok](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    <span data-ttu-id="170d5-147">Fel kell az adatokat tooconfigure hello a következő szakaszban a projekthez.</span><span class="sxs-lookup"><span data-stu-id="170d5-147">We will need this information tooconfigure your project in hello next section.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="170d5-148">Hello projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="170d5-148">Configure hello Project</span></span>
<span data-ttu-id="170d5-149">Ebben a szakaszban az alkalmazás toouse hello tárolására imént létrehozott új fiók konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="170d5-149">In this section, we'll configure our application toouse hello storage account we just created.</span></span> <span data-ttu-id="170d5-150">Ezt követően helyileg fogja futtatni hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="170d5-150">Then we'll run hello application locally.</span></span>

1. <span data-ttu-id="170d5-151">A Visual Studióban, kattintson a jobb gombbal a projekt csomópontjára a Megoldáskezelőben, és válassza ki **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="170d5-151">In Visual Studio, right-click on your project node in Solution Explorer and select **Properties**.</span></span> <span data-ttu-id="170d5-152">Kattintson a hello **Debug** fülre.</span><span class="sxs-lookup"><span data-stu-id="170d5-152">Click on hello **Debug** tab.</span></span>
   
     ![Hibakeresési Projektbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. <span data-ttu-id="170d5-154">Állítsa be a hello alkalmazáshoz szükséges környezeti változók értékeinek hello **Debug Server parancs**, **környezet**.</span><span class="sxs-lookup"><span data-stu-id="170d5-154">Set hello values of environment variables required by hello application in **Debug Server Command**, **Environment**.</span></span>
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   <span data-ttu-id="170d5-155">Hello környezeti változók állítja amikor Ön **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="170d5-155">This will set hello environment variables when you **Start Debugging**.</span></span> <span data-ttu-id="170d5-156">Meg, ha azt szeretné, ha hello változók toobe meg **indítása nélkül hibakeresés**, azonos értékének beállítása hello **Server parancs futtatása** is.</span><span class="sxs-lookup"><span data-stu-id="170d5-156">If you want hello variables toobe set when you **Start Without Debugging**, set hello same values under **Run Server Command** as well.</span></span>
   
   <span data-ttu-id="170d5-157">Azt is megteheti megadhatja a környezeti változók hello Windows Vezérlőpult használatával.</span><span class="sxs-lookup"><span data-stu-id="170d5-157">Alternatively, you can define environment variables using hello Windows Control Panel.</span></span> <span data-ttu-id="170d5-158">Ez az jobb megoldás, ha szeretné, hogy hitelesítő adatok tárolása a forráskód tooavoid / project fájlt.</span><span class="sxs-lookup"><span data-stu-id="170d5-158">This is a better option if you want tooavoid storing credentials in source code / project file.</span></span> <span data-ttu-id="170d5-159">Vegye figyelembe, hogy szüksége lesz a Visual Studio toorestart hello új környezet értékek toobe toohello elérhető alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="170d5-159">Note that you will need toorestart Visual Studio for hello new environment values toobe available toohello application.</span></span>
3. <span data-ttu-id="170d5-160">hello kódot, amely megvalósítja az hello Azure Table Storage tárház van **models/azuretablestorage.py**.</span><span class="sxs-lookup"><span data-stu-id="170d5-160">hello code that implements hello Azure Table Storage repository is in **models/azuretablestorage.py**.</span></span> <span data-ttu-id="170d5-161">Lásd: hello [dokumentáció] további információt a toouse Table szolgáltatás a Python.</span><span class="sxs-lookup"><span data-stu-id="170d5-161">See hello [documentation] for more information on how toouse Table Service from Python.</span></span>
4. <span data-ttu-id="170d5-162">Futtassa az alkalmazást hello `F5`.</span><span class="sxs-lookup"><span data-stu-id="170d5-162">Run hello application with `F5`.</span></span> <span data-ttu-id="170d5-163">A létrehozása **létrehozása Sample Polls** és hello szavazás során elküldött adatok mintaszavazások Azure Table Storage-ban.</span><span class="sxs-lookup"><span data-stu-id="170d5-163">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in Azure Table Storage.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="170d5-164">Visual Studio egy kivétel break hello Python 2.7 virtuális környezet vezethet.</span><span class="sxs-lookup"><span data-stu-id="170d5-164">hello Python 2.7 Virtual Environment may cause an exception break in Visual Studio.</span></span>  <span data-ttu-id="170d5-165">Nyomja le az `F5` toocontinue hello webes projekt betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="170d5-165">Press `F5` toocontinue loading hello web project.</span></span>
   > 
   > 
5. <span data-ttu-id="170d5-166">Keresse meg a toohello **kapcsolatos** lap tooverify, amely hello alkalmazás által használt hello **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="170d5-166">Browse toohello **About** page tooverify that hello application is using hello **Azure Table Storage** repository.</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a><span data-ttu-id="170d5-168">Hello Azure Table Storage felfedezés</span><span class="sxs-lookup"><span data-stu-id="170d5-168">Explore hello Azure Table Storage</span></span>
<span data-ttu-id="170d5-169">Könnyen tooview és szerkesztése a Visual Studio használatával a Cloud Explorer storage-táblákat.</span><span class="sxs-lookup"><span data-stu-id="170d5-169">It's easy tooview and edit storage tables using Cloud Explorer in Visual Studio.</span></span> <span data-ttu-id="170d5-170">Ez a szakasz a Server Explorer tooview hello hello szavazások alkalmazás táblák tartalmát fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="170d5-170">In this section we'll use Server Explorer tooview hello contents of hello polls application tables.</span></span>

> [!NOTE]
> <span data-ttu-id="170d5-171">Ehhez szükséges a Microsoft Azure eszközök toobe telepítve, amelyek állnak rendelkezésre hello részeként [Azure SDK for .NET].</span><span class="sxs-lookup"><span data-stu-id="170d5-171">This requires Microsoft Azure Tools toobe installed, which are available as part of hello [Azure SDK for .NET].</span></span>
> 
> 

1. <span data-ttu-id="170d5-172">Nyissa meg **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="170d5-172">Open **Cloud Explorer**.</span></span> <span data-ttu-id="170d5-173">Bontsa ki a **Tárfiókok**, a tárfiók, majd **táblák**.</span><span class="sxs-lookup"><span data-stu-id="170d5-173">Expand **Storage Accounts**, your storage account, then **Tables**.</span></span>
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. <span data-ttu-id="170d5-175">Kattintson duplán arra a hello **szavazások** vagy **lehetőségek** tábla tooview hello tartalmát egy dokumentumablakra, valamint hozzáadása/eltávolítása vagy módosítása entitások hello táblájában.</span><span class="sxs-lookup"><span data-stu-id="170d5-175">Double-click on hello **polls** or **choices** table tooview hello contents of hello table in a document window, as well as add/remove/edit entities.</span></span>
   
     ![Tábla lekérdezés eredményei](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="170d5-177">Hello web app tooAzure App Service közzététele</span><span class="sxs-lookup"><span data-stu-id="170d5-177">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="170d5-178">hello Azure .NET SDK-t biztosít egy egyszerűen toodeploy a webes alkalmazás tooAzure App Service.</span><span class="sxs-lookup"><span data-stu-id="170d5-178">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="170d5-179">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="170d5-179">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="170d5-181">Kattintson a **Microsoft Azure Web Apps** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="170d5-181">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="170d5-182">Kattintson a **új** toocreate egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="170d5-182">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="170d5-183">Töltse ki a következő mezők hello **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="170d5-183">Fill in hello following fields and click **Create**.</span></span>
   
   * <span data-ttu-id="170d5-184">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="170d5-184">**Web App name**</span></span>
   * <span data-ttu-id="170d5-185">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="170d5-185">**App Service plan**</span></span>
   * <span data-ttu-id="170d5-186">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="170d5-186">**Resource group**</span></span>
   * <span data-ttu-id="170d5-187">**Régió**</span><span class="sxs-lookup"><span data-stu-id="170d5-187">**Region**</span></span>
   * <span data-ttu-id="170d5-188">Hagyja **adatbázis-kiszolgáló** túl beállítása**adatbázis**</span><span class="sxs-lookup"><span data-stu-id="170d5-188">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="170d5-189">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="170d5-189">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="170d5-190">A böngésző automatikusan toohello a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="170d5-190">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="170d5-191">Ha tallózással toohello oldalról, látni fogja, hogy használja-e hello **memórián belüli** tárház, nem hello **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="170d5-191">If you browse toohello about page, you'll see that it uses hello **In-Memory** repository, not hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="170d5-192">Ennek oka hello környezeti változók nem úgy van beállítva, az Azure App Service Web Apps-példányon hello megadott hello alapértelmezett értékeket használ **settings.py**.</span><span class="sxs-lookup"><span data-stu-id="170d5-192">That's because hello environment variables are not set on hello Web Apps instance in Azure App Service, so it uses hello default values specified in **settings.py**.</span></span>

## <a name="configure-hello-web-apps-instance"></a><span data-ttu-id="170d5-193">Webalkalmazások példány hello konfigurálása</span><span class="sxs-lookup"><span data-stu-id="170d5-193">Configure hello Web Apps instance</span></span>
<span data-ttu-id="170d5-194">Ebben a szakaszban hello webalkalmazások példány környezeti változók konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="170d5-194">In this section, we'll configure environment variables for hello Web Apps instance.</span></span>

1. <span data-ttu-id="170d5-195">A [Azure Portal], megnyitásához hello webalkalmazása panelén **Tallózás** > **alkalmazásszolgáltatások** > a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="170d5-195">In [Azure Portal], open hello web app's blade by clicking **Browse** > **App Services** > your web app name.</span></span>
2. <span data-ttu-id="170d5-196">A webalkalmazás panelen kattintson **összes beállítás**, majd kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="170d5-196">In your web app's blade, click **All Settings**, then click **Application Settings**.</span></span>
3. <span data-ttu-id="170d5-197">Görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és hello értékeinek beállítása **TÁRHÁZ\_neve**, **tárolási\_neve** és  **TÁROLÁSI\_kulcs** hello leírtak **konfigurálása hello projekt** fenti szakaszban.</span><span class="sxs-lookup"><span data-stu-id="170d5-197">Scroll down toohello **App settings** section and set hello values for **REPOSITORY\_NAME**, **STORAGE\_NAME** and **STORAGE\_KEY** as described in hello **Configure hello project** section above.</span></span>
   
     ![Alkalmazásbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. <span data-ttu-id="170d5-199">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="170d5-199">Click on **Save**.</span></span> <span data-ttu-id="170d5-200">Hello értesítéseket, hogy alkalmazása megtörtént-e a hello módosítások fogadását követően kattintson a **Tallózás** hello webes alkalmazás fő paneljén.</span><span class="sxs-lookup"><span data-stu-id="170d5-200">After you've received hello notifications that hello changes were applied, click on **Browse** from hello Web app main blade.</span></span>
5. <span data-ttu-id="170d5-201">Megtekintheti az hello web app működő elvárás hello segítségével **Azure Table Storage** tárházba.</span><span class="sxs-lookup"><span data-stu-id="170d5-201">You should see hello web app working as expected, using hello **Azure Table Storage** repository.</span></span>
   
   <span data-ttu-id="170d5-202">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="170d5-202">Congratulations!</span></span>
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="170d5-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="170d5-204">Next steps</span></span>
<span data-ttu-id="170d5-205">Hajtsa végre a Python-eszközökkel kapcsolatos további hivatkozások toolearn a Visual Studio, a Bottle és az Azure Table Storage.</span><span class="sxs-lookup"><span data-stu-id="170d5-205">Follow these links toolearn more about Python Tools for Visual Studio, Bottle and Azure Table Storage.</span></span>

* <span data-ttu-id="170d5-206">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="170d5-206">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="170d5-207">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="170d5-207">[Web Projects]</span></span>
  * <span data-ttu-id="170d5-208">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="170d5-208">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="170d5-209">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="170d5-209">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="170d5-210">[Bottle dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="170d5-210">[Bottle Documentation]</span></span>
* <span data-ttu-id="170d5-211">[Azure Storage]</span><span class="sxs-lookup"><span data-stu-id="170d5-211">[Azure Storage]</span></span>
* <span data-ttu-id="170d5-212">[Pythonhoz készült Azure SDK]</span><span class="sxs-lookup"><span data-stu-id="170d5-212">[Azure SDK for Python]</span></span>
* <span data-ttu-id="170d5-213">[Hogyan tooUse hello Table Storage szolgáltatást Python]</span><span class="sxs-lookup"><span data-stu-id="170d5-213">[How tooUse hello Table Storage Service from Python]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="170d5-214">A változások</span><span class="sxs-lookup"><span data-stu-id="170d5-214">What's changed</span></span>
* <span data-ttu-id="170d5-215">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="170d5-215">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python fejlesztői központ]: /develop/python/
[Azure Felhőszolgáltatások]: ../cloud-services/cloud-services-python-ptvs.md
[dokumentáció]:../cosmos-db/table-storage-how-to-use-python.md
[Hogyan tooUse hello Table Storage szolgáltatást Python]:../cosmos-db/table-storage-how-to-use-python.md


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
