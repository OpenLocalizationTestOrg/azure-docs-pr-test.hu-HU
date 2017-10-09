---
title: "aaaDjango és a Python Tools 2.2 for Visual Studio Azure SQL Database"
description: "Ismerje meg, hogyan toouse hello Python Tools Visual Studio toocreate egy Django-webalkalmazás, amely egy SQL-adatbázispéldány adatait tárolja, és telepítse azt tooAzure App Service Web Apps."
services: app-service\web
tags: python
documentationcenter: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 3a677e64-b5a9-4d43-b9c0-66246368b483
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 07/07/2016
ms.author: huvalo
ms.openlocfilehash: b5b2ef4f3292e7df85007465c5394c8660a7d231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="a309a-103">Django és SQL Database az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással</span><span class="sxs-lookup"><span data-stu-id="a309a-103">Django and SQL Database on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="a309a-104">Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] toocreate egy egyszerű lekérdezi a web app használatával hello PTVS minta sablonok egyikét.</span><span class="sxs-lookup"><span data-stu-id="a309a-104">In this tutorial, we'll use [Python Tools for Visual Studio] toocreate a simple polls web app using one of hello PTVS sample templates.</span></span> <span data-ttu-id="a309a-105">Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span><span class="sxs-lookup"><span data-stu-id="a309a-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span></span>

<span data-ttu-id="a309a-106">Azt megtanulhatja, hogyan toouse SQL adatbázis Azure-platformon futó, hogyan tooconfigure hello web app toouse SQL-adatbázis, és hogyan a toopublish hello webalkalmazás túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="a309a-106">We'll learn how toouse a SQL database hosted on Azure, how tooconfigure hello web app toouse a SQL database, and how toopublish hello web app too[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="a309a-107">Lásd: hello [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, az Azure Table Storage, a MySQL és az SQL-adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a309a-107">See hello [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="a309a-108">Amíg ez a cikk foglalkozik az App Service, hello lépések hasonlóak fejlesztésekor [Azure Felhőszolgáltatások].</span><span class="sxs-lookup"><span data-stu-id="a309a-108">While this article focuses on App Service, hello steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a309a-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a309a-109">Prerequisites</span></span>
* <span data-ttu-id="a309a-110">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="a309a-110">Visual Studio 2015</span></span>
* <span data-ttu-id="a309a-111">[Python 2.7 32 bites]</span><span class="sxs-lookup"><span data-stu-id="a309a-111">[Python 2.7 32-bit]</span></span>
* <span data-ttu-id="a309a-112">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="a309a-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="a309a-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="a309a-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="a309a-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="a309a-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="a309a-115">Django 1.9 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="a309a-115">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="a309a-116">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="a309a-116">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a309a-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="a309a-117">No credit cards required; no commitments.</span></span>
>
>

## <a name="create-hello-project"></a><span data-ttu-id="a309a-118">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="a309a-118">Create hello Project</span></span>
<span data-ttu-id="a309a-119">Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával.</span><span class="sxs-lookup"><span data-stu-id="a309a-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="a309a-120">Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="a309a-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="a309a-121">Létrehozunk egy helyi adatbázist az sqlite használatával.</span><span class="sxs-lookup"><span data-stu-id="a309a-121">We'll create a local database using sqlite.</span></span> <span data-ttu-id="a309a-122">Majd hello webalkalmazás helyileg helyszínekről futtassuk.</span><span class="sxs-lookup"><span data-stu-id="a309a-122">Then we'll run hello web app locally.</span></span>

1. <span data-ttu-id="a309a-123">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a309a-123">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="a309a-124">a hello projektsablonjai hello [Python Tools 2.2 for Visual Studio Samples VSIX] alatt érhetők el **Python**, **minták**.</span><span class="sxs-lookup"><span data-stu-id="a309a-124">hello project templates from hello [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="a309a-125">Válassza ki **Polls Django Web Project** , és kattintson az OK toocreate hello projekt.</span><span class="sxs-lookup"><span data-stu-id="a309a-125">Select **Polls Django Web Project** and click OK toocreate hello project.</span></span>

     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. <span data-ttu-id="a309a-127">Külső csomagok felszólító tooinstall lesz.</span><span class="sxs-lookup"><span data-stu-id="a309a-127">You will be prompted tooinstall external packages.</span></span> <span data-ttu-id="a309a-128">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a309a-128">Select **Install into a virtual environment**.</span></span>

     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="a309a-130">Válassza ki **Python 2.7** , hello alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="a309a-130">Select **Python 2.7** as hello base interpreter.</span></span>

     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="a309a-132">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.</span><span class="sxs-lookup"><span data-stu-id="a309a-132">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="a309a-133">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="a309a-133">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="a309a-134">Ez a Django felügyeleti konzol megnyitásához, és hello projektmappa sqlite-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a309a-134">This will open a Django Management Console and create a sqlite database in hello project folder.</span></span> <span data-ttu-id="a309a-135">Hajtsa végre a hello kér toocreate a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a309a-135">Follow hello prompts toocreate a user.</span></span>
7. <span data-ttu-id="a309a-136">Győződjön meg arról, hogy működik-e hello alkalmazás billentyűkombináció lenyomásával <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="a309a-136">Confirm that hello application works by pressing <kbd>F5</kbd>.</span></span>
8. <span data-ttu-id="a309a-137">Kattintson a **jelentkezzen be** a hello navigációs sáv hello tetején.</span><span class="sxs-lookup"><span data-stu-id="a309a-137">Click **Log in** from hello navigation bar at hello top.</span></span>

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="a309a-139">Adja meg hello hitelesítő hello Ön által létrehozott felhasználót hello adatbázis szinkronizálásakor.</span><span class="sxs-lookup"><span data-stu-id="a309a-139">Enter hello credentials for hello user you created when you synchronized hello database.</span></span>

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="a309a-141">Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="a309a-141">Click **Create Sample Polls**.</span></span>

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="a309a-143">Kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="a309a-143">Click on a poll and vote.</span></span>

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a><span data-ttu-id="a309a-145">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="a309a-145">Create a SQL Database</span></span>
<span data-ttu-id="a309a-146">Hello adatbázishoz mi létrehozunk egy Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a309a-146">For hello database, we'll create an Azure SQL database.</span></span>

<span data-ttu-id="a309a-147">Ezeket a lépéseket követve létrehozhat egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a309a-147">You can create a database by following these steps.</span></span>

1. <span data-ttu-id="a309a-148">Jelentkezzen be a hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="a309a-148">Log into hello [Azure Portal].</span></span>
2. <span data-ttu-id="a309a-149">Hello navigációs ablaktábla hello alul kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="a309a-149">At hello bottom of hello navigation pane, click **NEW**.</span></span> <span data-ttu-id="a309a-150">, kattintson a **adatok + tárolás** > **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="a309a-150">, click **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="a309a-151">Konfigurálása új SQL-adatbázis hello hozzon létre egy új erőforráscsoportot, és válassza ki a megfelelő helyet hello.</span><span class="sxs-lookup"><span data-stu-id="a309a-151">Configure hello new SQL Database by creating a new resource group and select hello appropriate location for it.</span></span>
4. <span data-ttu-id="a309a-152">Hello SQL-adatbázis létrehozása után kattintson **Megnyitás Visual Studio** hello adatbázis paneljén.</span><span class="sxs-lookup"><span data-stu-id="a309a-152">Once hello SQL Database is created, click **Open in Visual Studio** in hello database blade.</span></span>
5. <span data-ttu-id="a309a-153">Kattintson a **úgy konfigurálni a tűzfalat**.</span><span class="sxs-lookup"><span data-stu-id="a309a-153">Click **Configure your firewall**.</span></span>
6. <span data-ttu-id="a309a-154">A hello **tűzfalbeállítások** panelen fel egy tűzfalszabályt a **KEZDŐ IP-** és **záró IP-Címnél** toohello nyilvános IP-cím a fejlesztési számítógép beállítása.</span><span class="sxs-lookup"><span data-stu-id="a309a-154">In hello **Firewall Settings** blade, add a firewall rule with **START IP** and **END IP** set toohello public IP address of your development machine.</span></span> <span data-ttu-id="a309a-155">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a309a-155">Click **Save**.</span></span>

   <span data-ttu-id="a309a-156">Ez lehetővé teszi kapcsolatok toohello adatbázis-kiszolgálójának a fejlesztési számítógépén.</span><span class="sxs-lookup"><span data-stu-id="a309a-156">This will allow connections toohello database server from your development machine.</span></span>
7. <span data-ttu-id="a309a-157">Hello adatbázis paneljén kattintson **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="a309a-157">Back in hello database blade, click **Properties**, then click **Show database connection strings**.</span></span>
8. <span data-ttu-id="a309a-158">Hello Másolás gombra tooput hello érték **ADO.NET** hello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="a309a-158">Use hello copy button tooput hello value of **ADO.NET** on hello clipboard.</span></span>

## <a name="configure-hello-project"></a><span data-ttu-id="a309a-159">Hello projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a309a-159">Configure hello Project</span></span>
<span data-ttu-id="a309a-160">Ez a szakasz a webes alkalmazás toouse hello SQL adatbázis imént létrehozott konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="a309a-160">In this section, we'll configure our web app toouse hello SQL database we just created.</span></span> <span data-ttu-id="a309a-161">További Python csomagok szükséges toouse SQL adatbázisok telepítjük a django alkalmazással is.</span><span class="sxs-lookup"><span data-stu-id="a309a-161">We'll also install additional Python packages required toouse SQL databases with Django.</span></span> <span data-ttu-id="a309a-162">Majd hello webalkalmazás helyileg helyszínekről futtassuk.</span><span class="sxs-lookup"><span data-stu-id="a309a-162">Then we'll run hello web app locally.</span></span>

1. <span data-ttu-id="a309a-163">A Visual Studióban nyissa meg a **settings.py**, a hello *projektnév* mappát.</span><span class="sxs-lookup"><span data-stu-id="a309a-163">In Visual Studio, open **settings.py**, from hello *ProjectName* folder.</span></span> <span data-ttu-id="a309a-164">Ideiglenesen illessze be a hello kapcsolati karakterláncot, hello-szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a309a-164">Temporarily paste hello connection string in hello editor.</span></span> <span data-ttu-id="a309a-165">hello kapcsolati karakterlánc: a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="a309a-165">hello connection string is in this format:</span></span>

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

<span data-ttu-id="a309a-166">Hello definíciójának szerkesztése `DATABASES` toouse hello fenti alapértékeket.</span><span class="sxs-lookup"><span data-stu-id="a309a-166">Edit hello definition of `DATABASES` toouse hello values above.</span></span>

        DATABASES = {
            'default': {
                'ENGINE': 'sql_server.pyodbc',
                'NAME': '<DatabaseName>',
                'USER': '<UserName>',
                'PASSWORD': '{your_password_here}',
                'HOST': '<ServerName>',
                'PORT': '<ServerPort>',
                'OPTIONS': {
                    'driver': 'SQL Server Native Client 11.0',
                    'MARS_Connection': 'True',
                }
            }
        }

1. <span data-ttu-id="a309a-167">A Megoldáskezelőben a **Python-környezetek**, kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **Python-csomag telepítése**.</span><span class="sxs-lookup"><span data-stu-id="a309a-167">In Solution Explorer, under **Python Environments**, right-click on hello virtual environment and select **Install Python Package**.</span></span>
2. <span data-ttu-id="a309a-168">Hello telepítéséhez `pyodbc` használatával **pip**.</span><span class="sxs-lookup"><span data-stu-id="a309a-168">Install hello package `pyodbc` using **pip**.</span></span>

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. <span data-ttu-id="a309a-170">Hello telepítéséhez `django-pyodbc-azure` használatával **pip**.</span><span class="sxs-lookup"><span data-stu-id="a309a-170">Install hello package `django-pyodbc-azure` using **pip**.</span></span>

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. <span data-ttu-id="a309a-172">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.</span><span class="sxs-lookup"><span data-stu-id="a309a-172">In **Solution Explorer**, right-click on hello project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="a309a-173">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="a309a-173">Then select **Django Create Superuser**.</span></span>

   <span data-ttu-id="a309a-174">Ezzel létrehoz hello táblák hello előző szakaszban létrehozott hello SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a309a-174">This will create hello tables for hello SQL database we created in hello previous section.</span></span> <span data-ttu-id="a309a-175">Hajtsa végre a hello kér toocreate egy felhasználó, aki nem rendelkezik toomatch hello felhasználói hello első szakaszban létrehozott hello sqlite-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a309a-175">Follow hello prompts toocreate a user, which doesn't have toomatch hello user in hello sqlite database created in hello first section.</span></span>
5. <span data-ttu-id="a309a-176">Futtassa az alkalmazást hello `F5`.</span><span class="sxs-lookup"><span data-stu-id="a309a-176">Run hello application with `F5`.</span></span> <span data-ttu-id="a309a-177">A létrehozása **létrehozása Sample Polls** és hello szavazás során elküldött adatok mintaszavazások hello SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a309a-177">Polls that are created with **Create Sample Polls** and hello data submitted by voting will be serialized in hello SQL database.</span></span>

## <a name="publish-hello-web-app-tooazure-app-service"></a><span data-ttu-id="a309a-178">Hello web app tooAzure App Service közzététele</span><span class="sxs-lookup"><span data-stu-id="a309a-178">Publish hello web app tooAzure App Service</span></span>
<span data-ttu-id="a309a-179">hello Azure .NET SDK-t biztosít egy egyszerűen toodeploy a webes web app tooAzure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a309a-179">hello Azure .NET SDK provides an easy way toodeploy your web web app tooAzure App Service Web Apps.</span></span>

1. <span data-ttu-id="a309a-180">A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="a309a-180">In **Solution Explorer**, right-click on hello project node and select **Publish**.</span></span>

     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="a309a-182">Kattintson a **Microsoft Azure Web Apps** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="a309a-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="a309a-183">Kattintson a **új** toocreate egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a309a-183">Click on **New** toocreate a new web app.</span></span>
4. <span data-ttu-id="a309a-184">Töltse ki a következő mezők hello **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a309a-184">Fill in hello following fields and click **Create**.</span></span>

   * <span data-ttu-id="a309a-185">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="a309a-185">**Web App name**</span></span>
   * <span data-ttu-id="a309a-186">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="a309a-186">**App Service plan**</span></span>
   * <span data-ttu-id="a309a-187">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="a309a-187">**Resource group**</span></span>
   * <span data-ttu-id="a309a-188">**Régió**</span><span class="sxs-lookup"><span data-stu-id="a309a-188">**Region**</span></span>
   * <span data-ttu-id="a309a-189">Hagyja **adatbázis-kiszolgáló** túl beállítása**adatbázis**</span><span class="sxs-lookup"><span data-stu-id="a309a-189">Leave **Database server** set too**No database**</span></span>
5. <span data-ttu-id="a309a-190">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="a309a-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="a309a-191">A böngésző automatikusan toohello a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a309a-191">Your web browser will open automatically toohello published web app.</span></span> <span data-ttu-id="a309a-192">Megtekintheti az hello web app működő elvárás hello segítségével **SQL** Azure-platformon futó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a309a-192">You should see hello web app working as expected, using hello **SQL** database hosted on Azure.</span></span>

   <span data-ttu-id="a309a-193">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="a309a-193">Congratulations!</span></span>

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="a309a-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a309a-195">Next steps</span></span>
<span data-ttu-id="a309a-196">Hajtsa végre a Python-eszközökkel kapcsolatos további hivatkozások toolearn a Visual Studio, a Django és SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a309a-196">Follow these links toolearn more about Python Tools for Visual Studio, Django and SQL Database.</span></span>

* <span data-ttu-id="a309a-197">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="a309a-197">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="a309a-198">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="a309a-198">[Web Projects]</span></span>
  * <span data-ttu-id="a309a-199">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="a309a-199">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="a309a-200">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="a309a-200">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="a309a-201">[A Django dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="a309a-201">[Django Documentation]</span></span>
* <span data-ttu-id="a309a-202">[SQL Database]</span><span class="sxs-lookup"><span data-stu-id="a309a-202">[SQL Database]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="a309a-203">A változások</span><span class="sxs-lookup"><span data-stu-id="a309a-203">What's changed</span></span>
* <span data-ttu-id="a309a-204">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="a309a-204">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Python fejlesztői központ]: /develop/python/
[Azure Felhőszolgáltatások]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->
[Azure Portal]: https://portal.azure.com
[a Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python Tools for Visual Studio – dokumentáció]: http://aka.ms/ptvsdocs
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026 (Távoli hibakeresés a Microsoft Azure-ban)
[Webes projektek]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service-projektek]: http://go.microsoft.com/fwlink/?LinkId=624028
[A Django dokumentációja]: https://www.djangoproject.com/
[SQL Database]: /documentation/services/sql-database/
