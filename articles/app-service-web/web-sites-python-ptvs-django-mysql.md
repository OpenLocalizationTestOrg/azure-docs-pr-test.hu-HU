---
title: "aaaDjango és MySQL az Azure Python Tools 2.2 for Visual Studio"
description: "Ismerje meg, hogyan toouse hello Python Tools Visual Studio toocreate a Django webes alkalmazás, amely tárolja az adatokat egy MySQL adatbázis-példányt, és telepítse azt tooAzure App Service Web Apps."
services: app-service\web
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: c60a50b5-8b5e-4818-a442-16362273dabb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: 1597c391d20c8e8ef629b4e4d05c9eb64c83bffc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="ebc26-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio (Django és MySQL az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással)</span><span class="sxs-lookup"><span data-stu-id="ebc26-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="ebc26-104">Az oktatóanyag azt ismertetjük [a Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) toocreate egy egyszerű lekérdezi a web app használatával hello PTVS minta sablonok egyikét.</span><span class="sxs-lookup"><span data-stu-id="ebc26-104">In this tutorial, you'll use [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="ebc26-105">Megtudhatja, hogyan toouse MySQL-szolgáltatás Azure-platformon futó, hogyan tooconfigure hello web app toouse MySQL, és hogyan a toopublish hello webalkalmazás túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="ebc26-105">You'll learn how toouse a MySQL service hosted on Azure, how tooconfigure hello web app toouse MySQL, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

> [!NOTE]
> <span data-ttu-id="ebc26-106">Ebben az oktatóanyagban lévő hello információt is érhető el a következő videó hello:</span><span class="sxs-lookup"><span data-stu-id="ebc26-106">hello information contained in this tutorial is also available in hello following video:</span></span>
> 
> <span data-ttu-id="ebc26-107">[PTVS 2.1: Django-alkalmazás és MySQL][video]</span><span class="sxs-lookup"><span data-stu-id="ebc26-107">[PTVS 2.1: Django app with MySQL][video]</span></span>
> 
> 

<span data-ttu-id="ebc26-108">Lásd: hello [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, az Azure Table Storage, a MySQL és az SQL-adatbázis szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="ebc26-108">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL, and SQL Database services.</span></span> <span data-ttu-id="ebc26-109">Amíg ez a cikk foglalkozik az App Service, hello lépések hasonlóak fejlesztésekor [Azure Felhőszolgáltatások].</span><span class="sxs-lookup"><span data-stu-id="ebc26-109">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ebc26-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ebc26-110">Prerequisites</span></span>
* <span data-ttu-id="ebc26-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="ebc26-111">Visual Studio 2015</span></span>
* <span data-ttu-id="ebc26-112">[Python 2.7 32 bites] vagy [Python 3.4 32 bites]</span><span class="sxs-lookup"><span data-stu-id="ebc26-112">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>
* <span data-ttu-id="ebc26-113">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="ebc26-113">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="ebc26-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="ebc26-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="ebc26-115">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="ebc26-115">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="ebc26-116">Django 1.9 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="ebc26-116">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of hello hello previous include. -->

> [!NOTE]
> <span data-ttu-id="ebc26-117">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="ebc26-117">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="ebc26-118">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="ebc26-118">No credit card is required, and no commitments are necessary.</span></span>
> 
> 

## <a name="create-hello-project"></a><span data-ttu-id="ebc26-119">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebc26-119">Create hello Project</span></span>
<span data-ttu-id="ebc26-120">Ebben a szakaszban mintasablon használatával fog létrehozni Visual Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="ebc26-120">In this section, you'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="ebc26-121">Létrehozza majd a virtuális környezetet, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="ebc26-121">You'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="ebc26-122">Az sqlite használatával létre fog hozni egy helyi adatbázist.</span><span class="sxs-lookup"><span data-stu-id="ebc26-122">You'll create a local database using sqlite.</span></span> <span data-ttu-id="ebc26-123">Majd hello alkalmazás helyileg fogja futtatni.</span><span class="sxs-lookup"><span data-stu-id="ebc26-123">Then you'll run hello application locally.</span></span>

1. <span data-ttu-id="ebc26-124">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ebc26-124">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="ebc26-125">a hello projektsablonjai hello [Python Tools 2.2 for Visual Studio Samples VSIX] alatt érhetők el **Python**, **minták**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-125">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="ebc26-126">Válassza ki **Polls Django Web Project** , és kattintson az OK toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="ebc26-126">Select **Polls Django Web Project** and click OK toocreate hello project.</span></span>
   
    ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. <span data-ttu-id="ebc26-128">Külső csomagok felszólító tooinstall lesz.</span><span class="sxs-lookup"><span data-stu-id="ebc26-128">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="ebc26-129">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ebc26-129">Select **Install into a virtual environment**.</span></span>
   
    ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="ebc26-131">Válassza ki **Python 2.7** vagy **Python 3.4** , hello alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="ebc26-131">Select **Python 2.7** or **Python 3.4** as hello base interpreter.</span></span>
   
    ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="ebc26-133">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-133">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="ebc26-134">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="ebc26-134">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="ebc26-135">Ez a Django felügyeleti konzol megnyitásához, és hello projektmappa sqlite-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ebc26-135">This will open a Django Management Console and create a sqlite database in hello project folder.</span></span> <span data-ttu-id="ebc26-136">Hajtsa végre a hello kér toocreate a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="ebc26-136">Follow hello prompts toocreate a user.</span></span>
7. <span data-ttu-id="ebc26-137">Győződjön meg arról, hogy működik-e hello alkalmazás billentyűkombináció lenyomásával `F5`.</span><span class="sxs-lookup"><span data-stu-id="ebc26-137">Confirm that hello application works by pressing `F5`.</span></span>
8. <span data-ttu-id="ebc26-138">Kattintson a **jelentkezzen be** a hello navigációs sáv hello tetején.</span><span class="sxs-lookup"><span data-stu-id="ebc26-138">Click **Log in** from hello navigation bar at hello top.</span></span>
   
    ![Django navigációs sáv](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="ebc26-140">Adja meg hello hitelesítő hello Ön által létrehozott felhasználót hello adatbázis szinkronizálásakor.</span><span class="sxs-lookup"><span data-stu-id="ebc26-140">Enter hello credentials for hello user you created when you synchronized hello database.</span></span>
   
    ![Bejelentkezési űrlap](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="ebc26-142">Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="ebc26-142">Click **Create Sample Polls**.</span></span>
    
     ![Create Sample Polls (Mintaszavazások létrehozása)](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="ebc26-144">Kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="ebc26-144">Click on a poll and vote.</span></span>
    
     ![Szavazás mintaszavazásokon](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a><span data-ttu-id="ebc26-146">MySQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="ebc26-146">Create a MySQL Database</span></span>
<span data-ttu-id="ebc26-147">Hello adatbázis létre fog hozni üzemeltetett ClearDB MySQL-adatbázis az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="ebc26-147">For hello database, you'll create a ClearDB MySQL hosted database on Azure.</span></span>

<span data-ttu-id="ebc26-148">Másik lehetőségként létrehozhatja saját Azure-beli virtuális gépét, majd telepítheti és felügyelheti a MySQL-t.</span><span class="sxs-lookup"><span data-stu-id="ebc26-148">As an alternative, you can create your own Virtual Machine running in Azure, then install and administer MySQL yourself.</span></span>

<span data-ttu-id="ebc26-149">Az alábbi lépéseket követve ingyenes csomaggal rendelkező adatbázist hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="ebc26-149">You can create a database with a free plan by following these steps.</span></span>

1. <span data-ttu-id="ebc26-150">Jelentkezzen be toohello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="ebc26-150">Log in toohello [Azure Portal].</span></span>
2. <span data-ttu-id="ebc26-151">Hello hello navigációs ablaktábla tetején, kattintson **új**, majd kattintson a **adatok + tárolás**, és kattintson a **MySQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-151">At hello Top of hello navigation pane, click **NEW**, then click **Data + Storage**, and then click **MySQL Database**.</span></span>
3. <span data-ttu-id="ebc26-152">Hello új MySQL-adatbázis konfigurálja egy új erőforráscsoport létrehozásával, és válassza ki a hello megfelelő helyet.</span><span class="sxs-lookup"><span data-stu-id="ebc26-152">Configure hello new MySQL database by creating a new resource group and select hello appropriate location for it.</span></span>
4. <span data-ttu-id="ebc26-153">Hello MySQL-adatbázis létrehozása után kattintson **tulajdonságok** hello adatbázis paneljén.</span><span class="sxs-lookup"><span data-stu-id="ebc26-153">Once hello MySQL database is created, click **Properties** in hello database blade.</span></span>
5. <span data-ttu-id="ebc26-154">Hello Másolás gombra tooput hello érték **KAPCSOLATI karakterlánc** hello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="ebc26-154">Use hello copy button tooput hello value of **CONNECTION STRING** on hello clipboard.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="ebc26-155">Hello projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ebc26-155">Configure hello Project</span></span>
<span data-ttu-id="ebc26-156">Ebben a szakaszban konfigurálhatja a webes alkalmazás toouse hello MySQL-adatbázis most létrehozott.</span><span class="sxs-lookup"><span data-stu-id="ebc26-156">In this section, you'll configure our web app toouse hello MySQL database you just created.</span></span> <span data-ttu-id="ebc26-157">Emellett telepíteni fog olyan további Python csomagok szükséges toouse MySQL-adatbázisok a django alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="ebc26-157">You'll also install additional Python packages required toouse MySQL databases with Django.</span></span> <span data-ttu-id="ebc26-158">Majd hello webalkalmazást helyileg fogja futtatni.</span><span class="sxs-lookup"><span data-stu-id="ebc26-158">Then you'll run hello web app locally.</span></span>

1. <span data-ttu-id="ebc26-159">A Visual Studióban nyissa meg a **settings.py**, a hello *projektnév* mappát.</span><span class="sxs-lookup"><span data-stu-id="ebc26-159">In Visual Studio, open **settings.py**, from hello *ProjectName* folder.</span></span> <span data-ttu-id="ebc26-160">Ideiglenesen illessze be a hello kapcsolati karakterláncot, hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="ebc26-160">Temporarily paste hello connection string in hello editor.</span></span> <span data-ttu-id="ebc26-161">hello kapcsolati karakterlánc: a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="ebc26-161">hello connection string is in this format:</span></span>
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    <span data-ttu-id="ebc26-162">Változás hello alapértelmezett adatbázis **motor** toouse MySQL, és állítsa be hello értékeinek **neve**, **felhasználói**, **jelszó** és  **ÁLLOMÁS** a hello **CONNECTIONSTRING**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-162">Change hello default database **ENGINE** toouse MySQL, and set hello values for **NAME**, **USER**, **PASSWORD** and **HOST** from hello **CONNECTIONSTRING**.</span></span>
   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }
2. <span data-ttu-id="ebc26-163">A Megoldáskezelőben a **Python-környezetek**, kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **Python-csomag telepítése**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-163">In Solution Explorer, under **Python Environments**, right-click on hello virtual environment and select **Install Python Package**.</span></span>
3. <span data-ttu-id="ebc26-164">Hello telepítéséhez `mysqlclient` használatával **pip**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-164">Install hello package `mysqlclient` using **pip**.</span></span>
   
    ![Az Install Package (Csomag telepítése) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. <span data-ttu-id="ebc26-166">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-166">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="ebc26-167">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="ebc26-167">Then select **Django Create Superuser**.</span></span>
   
    <span data-ttu-id="ebc26-168">Ezzel létrehoz hello táblák hello előző szakaszban létrehozott hello MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ebc26-168">This will create hello tables for hello MySQL database you created in hello previous section.</span></span> <span data-ttu-id="ebc26-169">Hajtsa végre a hello kér toocreate egy felhasználó, aki nem rendelkezik toomatch hello felhasználói hello Ez a cikk első szakaszában létrehozott hello sqlite-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ebc26-169">Follow hello prompts toocreate a user, which doesn't have toomatch hello user in hello sqlite database created in hello first section of this article.</span></span>
5. <span data-ttu-id="ebc26-170">Futtassa az alkalmazást hello `F5`.</span><span class="sxs-lookup"><span data-stu-id="ebc26-170">Run hello application with `F5`.</span></span> <span data-ttu-id="ebc26-171">A létrehozása **létrehozása Sample Polls** és hello szavazás során elküldött adatok mintaszavazások hello MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="ebc26-171">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in hello MySQL database.</span></span>

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="ebc26-172">Hello web app tooAzure App Service közzététele</span><span class="sxs-lookup"><span data-stu-id="ebc26-172">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="ebc26-173">hello Azure .NET SDK-t biztosít egy egyszerűen toodeploy a webes alkalmazás tooAzure App Service.</span><span class="sxs-lookup"><span data-stu-id="ebc26-173">hello Azure .NET SDK provides an easy way toodeploy your web app tooAzure App Service.</span></span>

1. <span data-ttu-id="ebc26-174">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ebc26-174">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>
   
    ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="ebc26-176">Kattintson a **Microsoft Azure App Service** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ebc26-176">Click on **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="ebc26-177">Kattintson a **új** toocreate egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ebc26-177">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="ebc26-178">Töltse ki a következő mezők hello **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="ebc26-178">Fill in hello following fields and click **Create**:</span></span>
   
   * <span data-ttu-id="ebc26-179">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="ebc26-179">**Web App name**</span></span>
   * <span data-ttu-id="ebc26-180">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="ebc26-180">**App Service plan**</span></span>
   * <span data-ttu-id="ebc26-181">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="ebc26-181">**Resource group**</span></span>
   * <span data-ttu-id="ebc26-182">**Régió**</span><span class="sxs-lookup"><span data-stu-id="ebc26-182">**Region**</span></span>
   * <span data-ttu-id="ebc26-183">Hagyja **adatbázis-kiszolgáló** túl beállítása**adatbázis**</span><span class="sxs-lookup"><span data-stu-id="ebc26-183">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="ebc26-184">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="ebc26-184">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="ebc26-185">A böngésző automatikusan toohello a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="ebc26-185">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="ebc26-186">Megtekintheti az hello web app működő elvárás hello segítségével **MySQL** Azure-platformon futó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="ebc26-186">You should see hello web app working as expected, using hello **MySQL** database hosted on Azure.</span></span>
   
    ![Webböngésző](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    <span data-ttu-id="ebc26-188">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="ebc26-188">Congratulations!</span></span> <span data-ttu-id="ebc26-189">A MySQL-alapú webes alkalmazás tooAzure közzététele sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="ebc26-189">You have successfully published your MySQL-based web app tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ebc26-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ebc26-190">Next steps</span></span>
<span data-ttu-id="ebc26-191">Hajtsa végre a Python-eszközökkel kapcsolatos további hivatkozások toolearn a Visual Studio, a Django és MySQL.</span><span class="sxs-lookup"><span data-stu-id="ebc26-191">Follow these links toolearn more about Python Tools for Visual Studio, Django and MySQL.</span></span>

* <span data-ttu-id="ebc26-192">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="ebc26-192">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="ebc26-193">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="ebc26-193">[Web Projects]</span></span>
  * <span data-ttu-id="ebc26-194">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="ebc26-194">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="ebc26-195">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="ebc26-195">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="ebc26-196">[A Django dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="ebc26-196">[Django Documentation]</span></span>
* <span data-ttu-id="ebc26-197">[MySQL]</span><span class="sxs-lookup"><span data-stu-id="ebc26-197">[MySQL]</span></span>

<span data-ttu-id="ebc26-198">További információkért lásd: hello [Python fejlesztői központ](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="ebc26-198">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

<!--Link references-->

[Python fejlesztői központ]: /develop/python/
[Azure Felhőszolgáltatások]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
[Python Tools for Visual Studio]: https://www.visualstudio.com/vs/python/
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python 3.4 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517191
[Python Tools for Visual Studio – dokumentáció]: http://aka.ms/ptvsdocs
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026 (Távoli hibakeresés a Microsoft Azure-ban)
[Webes projektek]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service-projektek]: http://go.microsoft.com/fwlink/?LinkId=624028
[A Django dokumentációja]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo
