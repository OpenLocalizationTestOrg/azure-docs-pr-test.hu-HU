---
title: "Django and MySQL on Azure with Python Tools 2.2 for Visual Studio (Django és MySQL az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással)"
description: "Megismerheti a Python Tools for Visual Studio használatát olyan Django-webalkalmazás létrehozásához, amely az adatokat a MySQL-adatbázis egy példányában tárolja és az Azure App Service Web Apps szolgáltatáson helyezi üzembe."
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
ms.openlocfilehash: fd85337ecdc638a4c18065a0ce94f697da8197f1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="23e2b-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio (Django és MySQL az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással)</span><span class="sxs-lookup"><span data-stu-id="23e2b-103">Django and MySQL on Azure with Python Tools 2.2 for Visual Studio</span></span>
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

<span data-ttu-id="23e2b-104">Ebben az oktatóanyagban a [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) alkalmazásával fog létrehozni egy egyszerű lekérdezési webalkalmazást az egyik PTVS-mintasablon használatával.</span><span class="sxs-lookup"><span data-stu-id="23e2b-104">In this tutorial, you'll use [Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="23e2b-105">Elsajátíthatja egy, az Azure-ban üzemeltetett MySQL-szolgáltatás használatát, a webalkalmazás a MySQL használatára való konfigurálását, valamint annak az [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) szolgáltatásban történő közzétételét.</span><span class="sxs-lookup"><span data-stu-id="23e2b-105">You'll learn how to use a MySQL service hosted on Azure, how to configure the web app to use MySQL, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

> [!NOTE]
> <span data-ttu-id="23e2b-106">Az ebben az oktatóanyagban szereplő információk az alábbi videóban is megtekinthetők:</span><span class="sxs-lookup"><span data-stu-id="23e2b-106">The information contained in this tutorial is also available in the following video:</span></span>
> 
> <span data-ttu-id="23e2b-107">[PTVS 2.1: Django-alkalmazás és MySQL][video]</span><span class="sxs-lookup"><span data-stu-id="23e2b-107">[PTVS 2.1: Django app with MySQL][video]</span></span>
> 
> 

<span data-ttu-id="23e2b-108">A [Python fejlesztői központban] találhat további, az Azure App Service Web Apps szolgáltatásának PTVS-sel történő fejlesztését ismertető cikkeket a Bottle, a Flask és a Django webes keretrendszerek használatával, olyan szolgáltatások esetében, mint az Azure Table Storage, a MySQL és az SQL Database.</span><span class="sxs-lookup"><span data-stu-id="23e2b-108">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL, and SQL Database services.</span></span> <span data-ttu-id="23e2b-109">Az App Service-t tárgyaló jelen cikkben szereplő lépések hasonlóak az [Azure Cloud Services] fejlesztése esetében használtakhoz.</span><span class="sxs-lookup"><span data-stu-id="23e2b-109">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="23e2b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="23e2b-110">Prerequisites</span></span>
* <span data-ttu-id="23e2b-111">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="23e2b-111">Visual Studio 2015</span></span>
* <span data-ttu-id="23e2b-112">[Python 2.7 32 bites] vagy [Python 3.4 32 bites]</span><span class="sxs-lookup"><span data-stu-id="23e2b-112">[Python 2.7 32-bit] or [Python 3.4 32-bit]</span></span>
* <span data-ttu-id="23e2b-113">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="23e2b-113">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="23e2b-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="23e2b-114">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="23e2b-115">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="23e2b-115">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="23e2b-116">Django 1.9 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="23e2b-116">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [!NOTE]
> <span data-ttu-id="23e2b-117">Ha nem szeretne regisztrálni Azure-fiókot az Azure App Service megismerése előtt, lépjen [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra, ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="23e2b-117">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="23e2b-118">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="23e2b-118">No credit card is required, and no commitments are necessary.</span></span>
> 
> 

## <a name="create-the-project"></a><span data-ttu-id="23e2b-119">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="23e2b-119">Create the Project</span></span>
<span data-ttu-id="23e2b-120">Ebben a szakaszban mintasablon használatával fog létrehozni Visual Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="23e2b-120">In this section, you'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="23e2b-121">Létrehozza majd a virtuális környezetet, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="23e2b-121">You'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="23e2b-122">Az sqlite használatával létre fog hozni egy helyi adatbázist.</span><span class="sxs-lookup"><span data-stu-id="23e2b-122">You'll create a local database using sqlite.</span></span> <span data-ttu-id="23e2b-123">Ezt követően az alkalmazást helyileg fogja futtatni.</span><span class="sxs-lookup"><span data-stu-id="23e2b-123">Then you'll run the application locally.</span></span>

1. <span data-ttu-id="23e2b-124">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="23e2b-124">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="23e2b-125">A [Python Tools 2.2 for Visual Studio Samples VSIX] projektsablonjai a **Python**, **Példák** elem alatt érhetők el.</span><span class="sxs-lookup"><span data-stu-id="23e2b-125">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="23e2b-126">Válassza a **Polls Django Web Project** (Szavazási Django webes projekt) lehetőséget, majd kattintson az OK gombra a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="23e2b-126">Select **Polls Django Web Project** and click OK to create the project.</span></span>
   
    ![A New Project (Új projekt) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. <span data-ttu-id="23e2b-128">A rendszer fel fogja kérni külső csomagok telepítésére.</span><span class="sxs-lookup"><span data-stu-id="23e2b-128">You will be prompted to install external packages.</span></span> <span data-ttu-id="23e2b-129">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="23e2b-129">Select **Install into a virtual environment**.</span></span>
   
    ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="23e2b-131">Alapszintű értelmezőként válassza ki a **Python 2.7** vagy **Python 3.4** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="23e2b-131">Select **Python 2.7** or **Python 3.4** as the base interpreter.</span></span>
   
    ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="23e2b-133">A **Megoldáskezelő** felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Python**, végül pedig a **Django Migrate** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="23e2b-133">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="23e2b-134">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="23e2b-134">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="23e2b-135">Ekkor megnyílik a Django felügyeleti konzol, majd sqlite-adatbázis jön létre a projektmappában.</span><span class="sxs-lookup"><span data-stu-id="23e2b-135">This will open a Django Management Console and create a sqlite database in the project folder.</span></span> <span data-ttu-id="23e2b-136">Kövesse az utasításokat a felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="23e2b-136">Follow the prompts to create a user.</span></span>
7. <span data-ttu-id="23e2b-137">Az alkalmazás működőképességét az `F5` billentyű lenyomásával ellenőrizze.</span><span class="sxs-lookup"><span data-stu-id="23e2b-137">Confirm that the application works by pressing `F5`.</span></span>
8. <span data-ttu-id="23e2b-138">Kattintson a felső rész navigációs sávján található **Log in** (Bejelentkezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="23e2b-138">Click **Log in** from the navigation bar at the top.</span></span>
   
    ![Django navigációs sáv](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="23e2b-140">Adja meg azon felhasználó hitelesítő adatait, amelyeket az adatbázis szinkronizálásakor hozott létre.</span><span class="sxs-lookup"><span data-stu-id="23e2b-140">Enter the credentials for the user you created when you synchronized the database.</span></span>
   
    ![Bejelentkezési űrlap](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="23e2b-142">Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="23e2b-142">Click **Create Sample Polls**.</span></span>
    
     ![Create Sample Polls (Mintaszavazások létrehozása)](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="23e2b-144">Kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="23e2b-144">Click on a poll and vote.</span></span>
    
     ![Szavazás mintaszavazásokon](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a><span data-ttu-id="23e2b-146">MySQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="23e2b-146">Create a MySQL Database</span></span>
<span data-ttu-id="23e2b-147">Az adatbázis tekintetében az Azure felületén létre fog hozni egy, a MySQL által üzemeltetett ClearDB adatbázist.</span><span class="sxs-lookup"><span data-stu-id="23e2b-147">For the database, you'll create a ClearDB MySQL hosted database on Azure.</span></span>

<span data-ttu-id="23e2b-148">Másik lehetőségként létrehozhatja saját Azure-beli virtuális gépét, majd telepítheti és felügyelheti a MySQL-t.</span><span class="sxs-lookup"><span data-stu-id="23e2b-148">As an alternative, you can create your own Virtual Machine running in Azure, then install and administer MySQL yourself.</span></span>

<span data-ttu-id="23e2b-149">Az alábbi lépéseket követve ingyenes csomaggal rendelkező adatbázist hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="23e2b-149">You can create a database with a free plan by following these steps.</span></span>

1. <span data-ttu-id="23e2b-150">Jelentkezzen be az [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="23e2b-150">Log in to the [Azure Portal].</span></span>
2. <span data-ttu-id="23e2b-151">A navigációs ablaktábla felső részén kattintson a **NEW** (ÚJ) > **Data + Storage** (Adatok + tárolás) > **MySQL Database** (MySQL-adatbázis) elemre.</span><span class="sxs-lookup"><span data-stu-id="23e2b-151">At the Top of the navigation pane, click **NEW**, then click **Data + Storage**, and then click **MySQL Database**.</span></span>
3. <span data-ttu-id="23e2b-152">Konfigurálja az új MySQL-adatbázist új erőforráscsoport létrehozásával, majd válassza ki számára a megfelelő helyet.</span><span class="sxs-lookup"><span data-stu-id="23e2b-152">Configure the new MySQL database by creating a new resource group and select the appropriate location for it.</span></span>
4. <span data-ttu-id="23e2b-153">A MySQL-adatbázis létrehozását követően kattintson az adatbázis paneljén a **Properties** (Tulajdonságok) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="23e2b-153">Once the MySQL database is created, click **Properties** in the database blade.</span></span>
5. <span data-ttu-id="23e2b-154">A **CONNECTION STRING** (KAPCSOLATI KARAKTERLÁNC) vágólapra helyezéséhez használja a Copy (Másolás) gombot.</span><span class="sxs-lookup"><span data-stu-id="23e2b-154">Use the copy button to put the value of **CONNECTION STRING** on the clipboard.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="23e2b-155">A projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="23e2b-155">Configure the Project</span></span>
<span data-ttu-id="23e2b-156">Ebben a szakaszban a webalkalmazást a most létrehozott MySQL-adatbázis használatára fogja konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="23e2b-156">In this section, you'll configure our web app to use the MySQL database you just created.</span></span> <span data-ttu-id="23e2b-157">Emellett telepíteni fog olyan további Python-csomagokat, amelyek a MySQL-adatbázisok a Django alkalmazással történő használatához szükségesek.</span><span class="sxs-lookup"><span data-stu-id="23e2b-157">You'll also install additional Python packages required to use MySQL databases with Django.</span></span> <span data-ttu-id="23e2b-158">Ezt követően a webalkalmazást helyileg fogja futtatni.</span><span class="sxs-lookup"><span data-stu-id="23e2b-158">Then you'll run the web app locally.</span></span>

1. <span data-ttu-id="23e2b-159">A Visual Studio felületén nyissa meg a **settings.py** fájlt a *ProjectName* mappából.</span><span class="sxs-lookup"><span data-stu-id="23e2b-159">In Visual Studio, open **settings.py**, from the *ProjectName* folder.</span></span> <span data-ttu-id="23e2b-160">Ideiglenesen illessze be a kapcsolati karakterláncot a szerkesztőbe.</span><span class="sxs-lookup"><span data-stu-id="23e2b-160">Temporarily paste the connection string in the editor.</span></span> <span data-ttu-id="23e2b-161">A kapcsolati karakterlánc formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="23e2b-161">The connection string is in this format:</span></span>
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    <span data-ttu-id="23e2b-162">Módosítsa az adatbázis alapértelmezett **ENGINE** értékét a MySQL használatára, majd állítsa be a **NAME**, a **USER**, a **PASSWORD** és a **HOST** paraméter értékét a **CONNECTIONSTRING** (KAPCSOLATI KARAKTERLÁNC) karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="23e2b-162">Change the default database **ENGINE** to use MySQL, and set the values for **NAME**, **USER**, **PASSWORD** and **HOST** from the **CONNECTIONSTRING**.</span></span>
   
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
2. <span data-ttu-id="23e2b-163">A Solution Explorer (Megoldáskezelő) **Python Environments** (Python-környezetek) területén kattintson a jobb gombbal a virtuális környezetre, majd válassza az **Install Python Package** (Python-csomag telepítése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="23e2b-163">In Solution Explorer, under **Python Environments**, right-click on the virtual environment and select **Install Python Package**.</span></span>
3. <span data-ttu-id="23e2b-164">Telepítse a `mysqlclient` csomagot a **pip** használatával.</span><span class="sxs-lookup"><span data-stu-id="23e2b-164">Install the package `mysqlclient` using **pip**.</span></span>
   
    ![Az Install Package (Csomag telepítése) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. <span data-ttu-id="23e2b-166">A **Megoldáskezelő** felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Python**, végül pedig a **Django Migrate** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="23e2b-166">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="23e2b-167">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="23e2b-167">Then select **Django Create Superuser**.</span></span>
   
    <span data-ttu-id="23e2b-168">Ekkor létrejönnek az előző szakaszban a MySQL-adatbázishoz létrehozott táblák.</span><span class="sxs-lookup"><span data-stu-id="23e2b-168">This will create the tables for the MySQL database you created in the previous section.</span></span> <span data-ttu-id="23e2b-169">Kövesse az utasításokat a felhasználó létrehozásához. Ennek a felhasználónak nem kell megegyeznie a jelen cikk első szakaszában létrehozott felhasználóval, amely az sqlite-adatbázisban található.</span><span class="sxs-lookup"><span data-stu-id="23e2b-169">Follow the prompts to create a user, which doesn't have to match the user in the sqlite database created in the first section of this article.</span></span>
5. <span data-ttu-id="23e2b-170">Futtassa az alkalmazást az `F5` billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="23e2b-170">Run the application with `F5`.</span></span> <span data-ttu-id="23e2b-171">A **Create Sample Polls** (Mintaszavazások létrehozása) szolgáltatással előállított szavazások, illetve a szavazás során elküldött adatok szerializálása a MySQL-adatbázisban történik.</span><span class="sxs-lookup"><span data-stu-id="23e2b-171">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in the MySQL database.</span></span>

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="23e2b-172">A webalkalmazás közzététele az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="23e2b-172">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="23e2b-173">Az Azure .NET SDK egyszerű módot kínál a webalkalmazása az Azure App Service szolgáltatásban történő közzétételére.</span><span class="sxs-lookup"><span data-stu-id="23e2b-173">The Azure .NET SDK provides an easy way to deploy your web app to Azure App Service.</span></span>

1. <span data-ttu-id="23e2b-174">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="23e2b-174">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>
   
    ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="23e2b-176">Kattintson a **Microsoft Azure App Service** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="23e2b-176">Click on **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="23e2b-177">A **New** (Új) gombra kattintva hozzon létre egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="23e2b-177">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="23e2b-178">Töltse ki az alábbi mezőket, majd kattintson a **Create** (Létrehozás) gombra:</span><span class="sxs-lookup"><span data-stu-id="23e2b-178">Fill in the following fields and click **Create**:</span></span>
   
   * <span data-ttu-id="23e2b-179">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="23e2b-179">**Web App name**</span></span>
   * <span data-ttu-id="23e2b-180">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="23e2b-180">**App Service plan**</span></span>
   * <span data-ttu-id="23e2b-181">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="23e2b-181">**Resource group**</span></span>
   * <span data-ttu-id="23e2b-182">**Régió**</span><span class="sxs-lookup"><span data-stu-id="23e2b-182">**Region**</span></span>
   * <span data-ttu-id="23e2b-183">Hagyja változatlanul a **Database server** (Adatbázis-kiszolgáló) **No database** (Nincs adatbázis) beállítását</span><span class="sxs-lookup"><span data-stu-id="23e2b-183">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="23e2b-184">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="23e2b-184">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="23e2b-185">A webböngészőjében automatikusan a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="23e2b-185">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="23e2b-186">Ekkor azt kell látnia, hogy a webalkalmazás a várt módon, az Azure által üzemeltetett **MySQL**-adatbázist használva működik.</span><span class="sxs-lookup"><span data-stu-id="23e2b-186">You should see the web app working as expected, using the **MySQL** database hosted on Azure.</span></span>
   
    ![Webböngésző](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    <span data-ttu-id="23e2b-188">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="23e2b-188">Congratulations!</span></span> <span data-ttu-id="23e2b-189">MySQL-alapú webalkalmazásának közzététele sikeresen megtörtént az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="23e2b-189">You have successfully published your MySQL-based web app to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="23e2b-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="23e2b-190">Next steps</span></span>
<span data-ttu-id="23e2b-191">A következő hivatkozásokat követve tudhat meg többet a Python Tools for Visual Studio-, a Django- és a MySQL.</span><span class="sxs-lookup"><span data-stu-id="23e2b-191">Follow these links to learn more about Python Tools for Visual Studio, Django and MySQL.</span></span>

* <span data-ttu-id="23e2b-192">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="23e2b-192">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="23e2b-193">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="23e2b-193">[Web Projects]</span></span>
  * <span data-ttu-id="23e2b-194">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="23e2b-194">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="23e2b-195">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="23e2b-195">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="23e2b-196">[A Django dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="23e2b-196">[Django Documentation]</span></span>
* <span data-ttu-id="23e2b-197">[MySQL]</span><span class="sxs-lookup"><span data-stu-id="23e2b-197">[MySQL]</span></span>

<span data-ttu-id="23e2b-198">További információ: [Python fejlesztői központban](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="23e2b-198">For more information, see the [Python Developer Center](/develop/python/).</span></span>

<!--Link references-->

[Python fejlesztői központban]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

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
