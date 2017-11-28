---
title: "Django és SQL Database az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással"
description: "Ismerje meg, amely tárolja az adatokat egy SQL-adatbázispéldány Django-webalkalmazás létrehozása a Python Tools for Visual Studio segítségével, és központilag telepítenie kell az Azure App Service Web Apps."
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
ms.openlocfilehash: 65b59dee2b7bddca77d31c692dab713c68d67e24
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a><span data-ttu-id="83ad8-103">Django és SQL Database az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással</span><span class="sxs-lookup"><span data-stu-id="83ad8-103">Django and SQL Database on Azure with Python Tools 2.2 for Visual Studio</span></span>
<span data-ttu-id="83ad8-104">Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] hozhat létre egy egyszerű szavazások webalkalmazás használja a PTVS-minta sablonok egyikét.</span><span class="sxs-lookup"><span data-stu-id="83ad8-104">In this tutorial, we'll use [Python Tools for Visual Studio] to create a simple polls web app using one of the PTVS sample templates.</span></span> <span data-ttu-id="83ad8-105">Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span><span class="sxs-lookup"><span data-stu-id="83ad8-105">This tutorial is also available as a [video](https://www.youtube.com/watch?v=ZwcoGcIeHF4).</span></span>

<span data-ttu-id="83ad8-106">A Microsoft megtanulhatja, hogyan használható az Azure-platformon futó SQL-adatbázis SQL-adatbázis használata a webalkalmazás konfigurálása és közzététele a webalkalmazás a [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="83ad8-106">We'll learn how to use a SQL database hosted on Azure, how to configure the web app to use a SQL database, and how to publish the web app to [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="83ad8-107">Tekintse meg a [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, az Azure Table Storage, a MySQL és az SQL-adatbázis-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="83ad8-107">See the [Python Developer Center] for more articles that cover development of Azure App Service Web Apps with PTVS using Bottle, Flask and Django web frameworks, with Azure Table Storage, MySQL and SQL Database services.</span></span> <span data-ttu-id="83ad8-108">Az App Service-t tárgyaló jelen cikkben szereplő lépések hasonlóak az [Azure Cloud Services] fejlesztése esetében használtakhoz.</span><span class="sxs-lookup"><span data-stu-id="83ad8-108">While this article focuses on App Service, the steps are similar when developing [Azure Cloud Services].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83ad8-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83ad8-109">Prerequisites</span></span>
* <span data-ttu-id="83ad8-110">Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="83ad8-110">Visual Studio 2015</span></span>
* <span data-ttu-id="83ad8-111">[Python 2.7, 32 bites]</span><span class="sxs-lookup"><span data-stu-id="83ad8-111">[Python 2.7 32-bit]</span></span>
* <span data-ttu-id="83ad8-112">[Python Tools 2.2 for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="83ad8-112">[Python Tools 2.2 for Visual Studio]</span></span>
* <span data-ttu-id="83ad8-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span><span class="sxs-lookup"><span data-stu-id="83ad8-113">[Python Tools 2.2 for Visual Studio Samples VSIX]</span></span>
* <span data-ttu-id="83ad8-114">[Azure SDK Tools for VS 2015]</span><span class="sxs-lookup"><span data-stu-id="83ad8-114">[Azure SDK Tools for VS 2015]</span></span>
* <span data-ttu-id="83ad8-115">Django 1.9 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="83ad8-115">Django 1.9 or later</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="83ad8-116">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="83ad8-116">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="83ad8-117">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="83ad8-117">No credit cards required; no commitments.</span></span>
>
>

## <a name="create-the-project"></a><span data-ttu-id="83ad8-118">A projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="83ad8-118">Create the Project</span></span>
<span data-ttu-id="83ad8-119">Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával.</span><span class="sxs-lookup"><span data-stu-id="83ad8-119">In this section, we'll create a Visual Studio project using a sample template.</span></span> <span data-ttu-id="83ad8-120">Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="83ad8-120">We'll create a virtual environment and install required packages.</span></span> <span data-ttu-id="83ad8-121">Létrehozunk egy helyi adatbázist az sqlite használatával.</span><span class="sxs-lookup"><span data-stu-id="83ad8-121">We'll create a local database using sqlite.</span></span> <span data-ttu-id="83ad8-122">Ezt követően helyileg fogja futtatni a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="83ad8-122">Then we'll run the web app locally.</span></span>

1. <span data-ttu-id="83ad8-123">A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="83ad8-123">In Visual Studio, select **File**, **New Project**.</span></span>
2. <span data-ttu-id="83ad8-124">A [Python Tools 2.2 for Visual Studio Samples VSIX] projektsablonjai a **Python**, **Példák** elem alatt érhetők el.</span><span class="sxs-lookup"><span data-stu-id="83ad8-124">The project templates from the [Python Tools 2.2 for Visual Studio Samples VSIX] are available under **Python**, **Samples**.</span></span> <span data-ttu-id="83ad8-125">Válassza a **Polls Django Web Project** (Szavazási Django webes projekt) lehetőséget, majd kattintson az OK gombra a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="83ad8-125">Select **Polls Django Web Project** and click OK to create the project.</span></span>

     ![A New Project (Új projekt) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. <span data-ttu-id="83ad8-127">A rendszer fel fogja kérni külső csomagok telepítésére.</span><span class="sxs-lookup"><span data-stu-id="83ad8-127">You will be prompted to install external packages.</span></span> <span data-ttu-id="83ad8-128">Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="83ad8-128">Select **Install into a virtual environment**.</span></span>

     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. <span data-ttu-id="83ad8-130">Alapszintű értelmezőként válassza ki a **Python 2.7** alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="83ad8-130">Select **Python 2.7** as the base interpreter.</span></span>

     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. <span data-ttu-id="83ad8-132">A **Megoldáskezelő** felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Python**, végül pedig a **Django Migrate** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="83ad8-132">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="83ad8-133">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="83ad8-133">Then select **Django Create Superuser**.</span></span>
6. <span data-ttu-id="83ad8-134">Ekkor megnyílik a Django felügyeleti konzol, majd sqlite-adatbázis jön létre a projektmappában.</span><span class="sxs-lookup"><span data-stu-id="83ad8-134">This will open a Django Management Console and create a sqlite database in the project folder.</span></span> <span data-ttu-id="83ad8-135">Kövesse az utasításokat a felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="83ad8-135">Follow the prompts to create a user.</span></span>
7. <span data-ttu-id="83ad8-136">Győződjön meg arról, hogy működik-e az alkalmazás billentyűkombináció lenyomásával <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="83ad8-136">Confirm that the application works by pressing <kbd>F5</kbd>.</span></span>
8. <span data-ttu-id="83ad8-137">Kattintson a felső rész navigációs sávján található **Log in** (Bejelentkezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="83ad8-137">Click **Log in** from the navigation bar at the top.</span></span>

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. <span data-ttu-id="83ad8-139">Adja meg azon felhasználó hitelesítő adatait, amelyeket az adatbázis szinkronizálásakor hozott létre.</span><span class="sxs-lookup"><span data-stu-id="83ad8-139">Enter the credentials for the user you created when you synchronized the database.</span></span>

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. <span data-ttu-id="83ad8-141">Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.</span><span class="sxs-lookup"><span data-stu-id="83ad8-141">Click **Create Sample Polls**.</span></span>

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. <span data-ttu-id="83ad8-143">Kattintson egy szavazásra, és szavazzon.</span><span class="sxs-lookup"><span data-stu-id="83ad8-143">Click on a poll and vote.</span></span>

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a><span data-ttu-id="83ad8-145">SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="83ad8-145">Create a SQL Database</span></span>
<span data-ttu-id="83ad8-146">Az adatbázis mi létrehozunk egy Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="83ad8-146">For the database, we'll create an Azure SQL database.</span></span>

<span data-ttu-id="83ad8-147">Ezeket a lépéseket követve létrehozhat egy adatbázist.</span><span class="sxs-lookup"><span data-stu-id="83ad8-147">You can create a database by following these steps.</span></span>

1. <span data-ttu-id="83ad8-148">Jelentkezzen be a [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="83ad8-148">Log into the [Azure Portal].</span></span>
2. <span data-ttu-id="83ad8-149">A navigációs ablaktábla alján kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="83ad8-149">At the bottom of the navigation pane, click **NEW**.</span></span> <span data-ttu-id="83ad8-150">, kattintson a **adatok + tárolás** > **SQL-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="83ad8-150">, click **Data + Storage** > **SQL Database**.</span></span>
3. <span data-ttu-id="83ad8-151">Konfigurálja az új SQL-adatbázis egy új erőforráscsoport létrehozásával, és válassza ki a megfelelő helyet.</span><span class="sxs-lookup"><span data-stu-id="83ad8-151">Configure the new SQL Database by creating a new resource group and select the appropriate location for it.</span></span>
4. <span data-ttu-id="83ad8-152">Az SQL-adatbázis létrehozása után kattintson **Megnyitás Visual Studio** az adatbázis paneljén.</span><span class="sxs-lookup"><span data-stu-id="83ad8-152">Once the SQL Database is created, click **Open in Visual Studio** in the database blade.</span></span>
5. <span data-ttu-id="83ad8-153">Kattintson a **úgy konfigurálni a tűzfalat**.</span><span class="sxs-lookup"><span data-stu-id="83ad8-153">Click **Configure your firewall**.</span></span>
6. <span data-ttu-id="83ad8-154">A a **tűzfalbeállítások** panelen fel egy tűzfalszabályt a **KEZDŐ IP-** és **záró IP-Címnél** állítsa be a fejlesztési számítógépén a nyilvános IP-címére.</span><span class="sxs-lookup"><span data-stu-id="83ad8-154">In the **Firewall Settings** blade, add a firewall rule with **START IP** and **END IP** set to the public IP address of your development machine.</span></span> <span data-ttu-id="83ad8-155">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="83ad8-155">Click **Save**.</span></span>

   <span data-ttu-id="83ad8-156">Ez lehetővé teszi kapcsolatok az adatbázis-kiszolgáló a fejlesztői gépen.</span><span class="sxs-lookup"><span data-stu-id="83ad8-156">This will allow connections to the database server from your development machine.</span></span>
7. <span data-ttu-id="83ad8-157">Vissza az adatbázis paneljén kattintson **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**.</span><span class="sxs-lookup"><span data-stu-id="83ad8-157">Back in the database blade, click **Properties**, then click **Show database connection strings**.</span></span>
8. <span data-ttu-id="83ad8-158">Használja a Másolás gombra, írja be az értéket a **ADO.NET** a vágólapon.</span><span class="sxs-lookup"><span data-stu-id="83ad8-158">Use the copy button to put the value of **ADO.NET** on the clipboard.</span></span>

## <a name="configure-the-project"></a><span data-ttu-id="83ad8-159">A projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83ad8-159">Configure the Project</span></span>
<span data-ttu-id="83ad8-160">Ebben a szakaszban a webalkalmazást az imént létrehozott SQL database szolgáltatást használna konfigurálását végezzük el.</span><span class="sxs-lookup"><span data-stu-id="83ad8-160">In this section, we'll configure our web app to use the SQL database we just created.</span></span> <span data-ttu-id="83ad8-161">SQL-adatbázisok használja djangóval szükséges további Python-csomagokat is telepítjük.</span><span class="sxs-lookup"><span data-stu-id="83ad8-161">We'll also install additional Python packages required to use SQL databases with Django.</span></span> <span data-ttu-id="83ad8-162">Ezt követően helyileg fogja futtatni a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="83ad8-162">Then we'll run the web app locally.</span></span>

1. <span data-ttu-id="83ad8-163">A Visual Studio felületén nyissa meg a **settings.py** fájlt a *ProjectName* mappából.</span><span class="sxs-lookup"><span data-stu-id="83ad8-163">In Visual Studio, open **settings.py**, from the *ProjectName* folder.</span></span> <span data-ttu-id="83ad8-164">Ideiglenesen illessze be a kapcsolati karakterláncot a szerkesztőbe.</span><span class="sxs-lookup"><span data-stu-id="83ad8-164">Temporarily paste the connection string in the editor.</span></span> <span data-ttu-id="83ad8-165">A kapcsolati karakterlánc formátuma a következő:</span><span class="sxs-lookup"><span data-stu-id="83ad8-165">The connection string is in this format:</span></span>

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

<span data-ttu-id="83ad8-166">Definíciójának szerkesztése `DATABASES` a fenti értékeket szeretné használni.</span><span class="sxs-lookup"><span data-stu-id="83ad8-166">Edit the definition of `DATABASES` to use the values above.</span></span>

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

1. <span data-ttu-id="83ad8-167">A Solution Explorer (Megoldáskezelő) **Python Environments** (Python-környezetek) területén kattintson a jobb gombbal a virtuális környezetre, majd válassza az **Install Python Package** (Python-csomag telepítése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="83ad8-167">In Solution Explorer, under **Python Environments**, right-click on the virtual environment and select **Install Python Package**.</span></span>
2. <span data-ttu-id="83ad8-168">Telepítse a `pyodbc` csomagot a **pip** használatával.</span><span class="sxs-lookup"><span data-stu-id="83ad8-168">Install the package `pyodbc` using **pip**.</span></span>

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. <span data-ttu-id="83ad8-170">Telepítse a `django-pyodbc-azure` csomagot a **pip** használatával.</span><span class="sxs-lookup"><span data-stu-id="83ad8-170">Install the package `django-pyodbc-azure` using **pip**.</span></span>

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. <span data-ttu-id="83ad8-172">A **Megoldáskezelő** felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Python**, végül pedig a **Django Migrate** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="83ad8-172">In **Solution Explorer**, right-click on the project node and select **Python**, and then select **Django Migrate**.</span></span>  <span data-ttu-id="83ad8-173">Ezután válassza a **Django Create Superuser** elemet.</span><span class="sxs-lookup"><span data-stu-id="83ad8-173">Then select **Django Create Superuser**.</span></span>

   <span data-ttu-id="83ad8-174">Ekkor létrejön az előző szakaszban létrehozott SQL-adatbázis tábláit.</span><span class="sxs-lookup"><span data-stu-id="83ad8-174">This will create the tables for the SQL database we created in the previous section.</span></span> <span data-ttu-id="83ad8-175">Kövesse az utasításokat, nem kell megegyeznie a felhasználónak az első szakaszban létrehozott sqlite-adatbázis a felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="83ad8-175">Follow the prompts to create a user, which doesn't have to match the user in the sqlite database created in the first section.</span></span>
5. <span data-ttu-id="83ad8-176">Futtassa az alkalmazást az `F5` billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="83ad8-176">Run the application with `F5`.</span></span> <span data-ttu-id="83ad8-177">A létrehozása **létrehozása Sample Polls** és a szavazás során elküldött adatok mintaszavazások az SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="83ad8-177">Polls that are created with **Create Sample Polls** and the data submitted by voting will be serialized in the SQL database.</span></span>

## <a name="publish-the-web-app-to-azure-app-service"></a><span data-ttu-id="83ad8-178">A webalkalmazás közzététele az Azure App Service szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="83ad8-178">Publish the web app to Azure App Service</span></span>
<span data-ttu-id="83ad8-179">Az Azure .NET SDK egyszerű módot nyújt a webkiszolgáló webalkalmazás telepítése az Azure App Service Web Apps biztosít.</span><span class="sxs-lookup"><span data-stu-id="83ad8-179">The Azure .NET SDK provides an easy way to deploy your web web app to Azure App Service Web Apps.</span></span>

1. <span data-ttu-id="83ad8-180">A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="83ad8-180">In **Solution Explorer**, right-click on the project node and select **Publish**.</span></span>

     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. <span data-ttu-id="83ad8-182">Kattintson a **Microsoft Azure Web Apps** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="83ad8-182">Click on **Microsoft Azure Web Apps**.</span></span>
3. <span data-ttu-id="83ad8-183">A **New** (Új) gombra kattintva hozzon létre egy új webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="83ad8-183">Click on **New** to create a new web app.</span></span>
4. <span data-ttu-id="83ad8-184">Töltse ki a következő mezőket és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="83ad8-184">Fill in the following fields and click **Create**.</span></span>

   * <span data-ttu-id="83ad8-185">**A webalkalmazás neve**</span><span class="sxs-lookup"><span data-stu-id="83ad8-185">**Web App name**</span></span>
   * <span data-ttu-id="83ad8-186">**App Service-csomag**</span><span class="sxs-lookup"><span data-stu-id="83ad8-186">**App Service plan**</span></span>
   * <span data-ttu-id="83ad8-187">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="83ad8-187">**Resource group**</span></span>
   * <span data-ttu-id="83ad8-188">**Régió**</span><span class="sxs-lookup"><span data-stu-id="83ad8-188">**Region**</span></span>
   * <span data-ttu-id="83ad8-189">Hagyja változatlanul a **Database server** (Adatbázis-kiszolgáló) **No database** (Nincs adatbázis) beállítását</span><span class="sxs-lookup"><span data-stu-id="83ad8-189">Leave **Database server** set to **No database**</span></span>
5. <span data-ttu-id="83ad8-190">Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="83ad8-190">Accept all other defaults and click **Publish**.</span></span>
6. <span data-ttu-id="83ad8-191">A webböngészőjében automatikusan a közzétett webalkalmazás nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="83ad8-191">Your web browser will open automatically to the published web app.</span></span> <span data-ttu-id="83ad8-192">A webes alkalmazás megfelelően működik, használatával kell megjelennie a **SQL** Azure-platformon futó adatbázis.</span><span class="sxs-lookup"><span data-stu-id="83ad8-192">You should see the web app working as expected, using the **SQL** database hosted on Azure.</span></span>

   <span data-ttu-id="83ad8-193">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="83ad8-193">Congratulations!</span></span>

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a><span data-ttu-id="83ad8-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="83ad8-195">Next steps</span></span>
<span data-ttu-id="83ad8-196">Kövesse az alábbi hivatkozásokat tudjon meg többet a Python Tools for Visual Studio, a Django és SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="83ad8-196">Follow these links to learn more about Python Tools for Visual Studio, Django and SQL Database.</span></span>

* <span data-ttu-id="83ad8-197">[Python Tools for Visual Studio – dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="83ad8-197">[Python Tools for Visual Studio Documentation]</span></span>
  * <span data-ttu-id="83ad8-198">[Webes projektek]</span><span class="sxs-lookup"><span data-stu-id="83ad8-198">[Web Projects]</span></span>
  * <span data-ttu-id="83ad8-199">[Cloud Service-projektek]</span><span class="sxs-lookup"><span data-stu-id="83ad8-199">[Cloud Service Projects]</span></span>
  * <span data-ttu-id="83ad8-200">[Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="83ad8-200">[Remote Debugging on Microsoft Azure]</span></span>
* <span data-ttu-id="83ad8-201">[A Django dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="83ad8-201">[Django Documentation]</span></span>
* <span data-ttu-id="83ad8-202">[SQL Database]</span><span class="sxs-lookup"><span data-stu-id="83ad8-202">[SQL Database]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="83ad8-203">A változások</span><span class="sxs-lookup"><span data-stu-id="83ad8-203">What's changed</span></span>
* <span data-ttu-id="83ad8-204">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="83ad8-204">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="83ad8-205">[Python fejlesztői központ]: /develop/python/</span><span class="sxs-lookup"><span data-stu-id="83ad8-205">[Python Developer Center]: /develop/python/</span></span>
<span data-ttu-id="83ad8-206">[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md</span><span class="sxs-lookup"><span data-stu-id="83ad8-206">[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md</span></span>

<!--External Link references-->
<span data-ttu-id="83ad8-207">[Azure-portálon]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="83ad8-207">[Azure Portal]: https://portal.azure.com</span></span>
<span data-ttu-id="83ad8-208">[a Python Tools for Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="83ad8-208">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="83ad8-209">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="83ad8-209">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="83ad8-210">[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="83ad8-210">[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="83ad8-211">[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003</span><span class="sxs-lookup"><span data-stu-id="83ad8-211">[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003</span></span>
<span data-ttu-id="83ad8-212">[Python 2.7, 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517190</span><span class="sxs-lookup"><span data-stu-id="83ad8-212">[Python 2.7 32-bit]: http://go.microsoft.com/fwlink/?LinkId=517190</span></span>
<span data-ttu-id="83ad8-213">[Python Tools for Visual Studio – dokumentáció]: http://aka.ms/ptvsdocs</span><span class="sxs-lookup"><span data-stu-id="83ad8-213">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs</span></span>
<span data-ttu-id="83ad8-214">[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026 (Távoli hibakeresés a Microsoft Azure-ban)</span><span class="sxs-lookup"><span data-stu-id="83ad8-214">[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026</span></span>
<span data-ttu-id="83ad8-215">[Webes projektek]: http://go.microsoft.com/fwlink/?LinkId=624027</span><span class="sxs-lookup"><span data-stu-id="83ad8-215">[Web Projects]: http://go.microsoft.com/fwlink/?LinkId=624027</span></span>
<span data-ttu-id="83ad8-216">[Cloud Service-projektek]: http://go.microsoft.com/fwlink/?LinkId=624028</span><span class="sxs-lookup"><span data-stu-id="83ad8-216">[Cloud Service Projects]: http://go.microsoft.com/fwlink/?LinkId=624028</span></span>
<span data-ttu-id="83ad8-217">[A Django dokumentációja]: https://www.djangoproject.com/</span><span class="sxs-lookup"><span data-stu-id="83ad8-217">[Django Documentation]: https://www.djangoproject.com/</span></span>
<span data-ttu-id="83ad8-218">[SQL Database]: /documentation/services/sql-database/</span><span class="sxs-lookup"><span data-stu-id="83ad8-218">[SQL Database]: /documentation/services/sql-database/</span></span>
