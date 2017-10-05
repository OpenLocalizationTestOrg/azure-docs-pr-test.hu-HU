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
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a>Django és SQL Database az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással
Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] hozhat létre egy egyszerű szavazások webalkalmazás használja a PTVS-minta sablonok egyikét. Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=ZwcoGcIeHF4).

A Microsoft megtanulhatja, hogyan használható az Azure-platformon futó SQL-adatbázis SQL-adatbázis használata a webalkalmazás konfigurálása és közzététele a webalkalmazás a [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Tekintse meg a [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, az Azure Table Storage, a MySQL és az SQL-adatbázis-szolgáltatás. Az App Service-t tárgyaló jelen cikkben szereplő lépések hasonlóak az [Azure Cloud Services] fejlesztése esetében használtakhoz.

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2015
* [Python 2.7, 32 bites]
* [Python Tools 2.2 for Visual Studio]
* [Python Tools 2.2 for Visual Studio Samples VSIX]
* [Azure SDK Tools for VS 2015]
* Django 1.9 vagy újabb

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
>
>

## <a name="create-the-project"></a>A projekt létrehozása
Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával. Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat. Létrehozunk egy helyi adatbázist az sqlite használatával. Ezt követően helyileg fogja futtatni a webalkalmazást.

1. A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.
2. A [Python Tools 2.2 for Visual Studio Samples VSIX] projektsablonjai a **Python**, **Példák** elem alatt érhetők el. Válassza a **Polls Django Web Project** (Szavazási Django webes projekt) lehetőséget, majd kattintson az OK gombra a projekt létrehozásához.

     ![A New Project (Új projekt) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. A rendszer fel fogja kérni külső csomagok telepítésére. Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.

     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. Alapszintű értelmezőként válassza ki a **Python 2.7** alkalmazást.

     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. A **Megoldáskezelő** felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Python**, végül pedig a **Django Migrate** lehetőséget.  Ezután válassza a **Django Create Superuser** elemet.
6. Ekkor megnyílik a Django felügyeleti konzol, majd sqlite-adatbázis jön létre a projektmappában. Kövesse az utasításokat a felhasználó létrehozásához.
7. Győződjön meg arról, hogy működik-e az alkalmazás billentyűkombináció lenyomásával <kbd>F5</kbd>.
8. Kattintson a felső rész navigációs sávján található **Log in** (Bejelentkezés) gombra.

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. Adja meg azon felhasználó hitelesítő adatait, amelyeket az adatbázis szinkronizálásakor hozott létre.

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. Kattintson egy szavazásra, és szavazzon.

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása
Az adatbázis mi létrehozunk egy Azure SQL-adatbázis.

Ezeket a lépéseket követve létrehozhat egy adatbázist.

1. Jelentkezzen be a [Azure-portálon].
2. A navigációs ablaktábla alján kattintson **új**. , kattintson a **adatok + tárolás** > **SQL-adatbázis**.
3. Konfigurálja az új SQL-adatbázis egy új erőforráscsoport létrehozásával, és válassza ki a megfelelő helyet.
4. Az SQL-adatbázis létrehozása után kattintson **Megnyitás Visual Studio** az adatbázis paneljén.
5. Kattintson a **úgy konfigurálni a tűzfalat**.
6. A a **tűzfalbeállítások** panelen fel egy tűzfalszabályt a **KEZDŐ IP-** és **záró IP-Címnél** állítsa be a fejlesztési számítógépén a nyilvános IP-címére. Kattintson a **Save** (Mentés) gombra.

   Ez lehetővé teszi kapcsolatok az adatbázis-kiszolgáló a fejlesztői gépen.
7. Vissza az adatbázis paneljén kattintson **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**.
8. Használja a Másolás gombra, írja be az értéket a **ADO.NET** a vágólapon.

## <a name="configure-the-project"></a>A projekt konfigurálása
Ebben a szakaszban a webalkalmazást az imént létrehozott SQL database szolgáltatást használna konfigurálását végezzük el. SQL-adatbázisok használja djangóval szükséges további Python-csomagokat is telepítjük. Ezt követően helyileg fogja futtatni a webalkalmazást.

1. A Visual Studio felületén nyissa meg a **settings.py** fájlt a *ProjectName* mappából. Ideiglenesen illessze be a kapcsolati karakterláncot a szerkesztőbe. A kapcsolati karakterlánc formátuma a következő:

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

Definíciójának szerkesztése `DATABASES` a fenti értékeket szeretné használni.

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

1. A Solution Explorer (Megoldáskezelő) **Python Environments** (Python-környezetek) területén kattintson a jobb gombbal a virtuális környezetre, majd válassza az **Install Python Package** (Python-csomag telepítése) lehetőséget.
2. Telepítse a `pyodbc` csomagot a **pip** használatával.

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. Telepítse a `django-pyodbc-azure` csomagot a **pip** használatával.

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. A **Megoldáskezelő** felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Python**, végül pedig a **Django Migrate** lehetőséget.  Ezután válassza a **Django Create Superuser** elemet.

   Ekkor létrejön az előző szakaszban létrehozott SQL-adatbázis tábláit. Kövesse az utasításokat, nem kell megegyeznie a felhasználónak az első szakaszban létrehozott sqlite-adatbázis a felhasználó létrehozásához.
5. Futtassa az alkalmazást az `F5` billentyű lenyomásával. A létrehozása **létrehozása Sample Polls** és a szavazás során elküldött adatok mintaszavazások az SQL-adatbázisban.

## <a name="publish-the-web-app-to-azure-app-service"></a>A webalkalmazás közzététele az Azure App Service szolgáltatásban
Az Azure .NET SDK egyszerű módot nyújt a webkiszolgáló webalkalmazás telepítése az Azure App Service Web Apps biztosít.

1. A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Publish** (Közzététel) lehetőséget.

     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. Kattintson a **Microsoft Azure Web Apps** lehetőségre.
3. A **New** (Új) gombra kattintva hozzon létre egy új webalkalmazást.
4. Töltse ki a következő mezőket és kattintson a **létrehozása**.

   * **A webalkalmazás neve**
   * **App Service-csomag**
   * **Erőforráscsoport**
   * **Régió**
   * Hagyja változatlanul a **Database server** (Adatbázis-kiszolgáló) **No database** (Nincs adatbázis) beállítását
5. Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.
6. A webböngészőjében automatikusan a közzétett webalkalmazás nyílik meg. A webes alkalmazás megfelelően működik, használatával kell megjelennie a **SQL** Azure-platformon futó adatbázis.

   Gratulálunk!

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a>Következő lépések
Kövesse az alábbi hivatkozásokat tudjon meg többet a Python Tools for Visual Studio, a Django és SQL-adatbázis.

* [Python Tools for Visual Studio – dokumentáció]
  * [Webes projektek]
  * [Cloud Service-projektek]
  * [Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)
* [A Django dokumentációja]
* [SQL Database]

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Python fejlesztői központ]: /develop/python/
[Azure Cloud Services]: ../cloud-services/cloud-services-python-ptvs.md

<!--External Link references-->
[Azure-portálon]: https://portal.azure.com
[a Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK Tools for VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7, 32 bites]: http://go.microsoft.com/fwlink/?LinkId=517190
[Python Tools for Visual Studio – dokumentáció]: http://aka.ms/ptvsdocs
[Remote Debugging on Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026 (Távoli hibakeresés a Microsoft Azure-ban)
[Webes projektek]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloud Service-projektek]: http://go.microsoft.com/fwlink/?LinkId=624028
[A Django dokumentációja]: https://www.djangoproject.com/
[SQL Database]: /documentation/services/sql-database/
