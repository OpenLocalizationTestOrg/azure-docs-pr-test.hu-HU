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
# <a name="django-and-sql-database-on-azure-with-python-tools-22-for-visual-studio"></a>Django és SQL Database az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással
Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] toocreate egy egyszerű lekérdezi a web app használatával hello PTVS minta sablonok egyikét. Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=ZwcoGcIeHF4).

Azt megtanulhatja, hogyan toouse SQL adatbázis Azure-platformon futó, hogyan tooconfigure hello web app toouse SQL-adatbázis, és hogyan a toopublish hello webalkalmazás túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Lásd: hello [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, az Azure Table Storage, a MySQL és az SQL-adatbázis-szolgáltatás. Amíg ez a cikk foglalkozik az App Service, hello lépések hasonlóak fejlesztésekor [Azure Felhőszolgáltatások].

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2015
* [Python 2.7 32 bites]
* [Python Tools 2.2 for Visual Studio]
* [Python Tools 2.2 for Visual Studio Samples VSIX]
* [Azure SDK Tools for VS 2015]
* Django 1.9 vagy újabb

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
>
>

## <a name="create-hello-project"></a>Hello projekt létrehozása
Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával. Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat. Létrehozunk egy helyi adatbázist az sqlite használatával. Majd hello webalkalmazás helyileg helyszínekről futtassuk.

1. A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.
2. a hello projektsablonjai hello [Python Tools 2.2 for Visual Studio Samples VSIX] alatt érhetők el **Python**, **minták**. Válassza ki **Polls Django Web Project** , és kattintson az OK toocreate hello projekt.

     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoNewProject.png)
3. Külső csomagok felszólító tooinstall lesz. Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.

     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoExternalPackages.png)
4. Válassza ki **Python 2.7** , hello alapszintű értelmezőt.

     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonAddVirtualEnv.png)
5. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.  Ezután válassza a **Django Create Superuser** elemet.
6. Ez a Django felügyeleti konzol megnyitásához, és hello projektmappa sqlite-adatbázis létrehozása. Hajtsa végre a hello kér toocreate a felhasználó.
7. Győződjön meg arról, hogy működik-e hello alkalmazás billentyűkombináció lenyomásával <kbd>F5</kbd>.
8. Kattintson a **jelentkezzen be** a hello navigációs sáv hello tetején.

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalMenu.png)
9. Adja meg hello hitelesítő hello Ön által létrehozott felhasználót hello adatbázis szinkronizálásakor.

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserLocalLogin.png)
10. Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoCommonBrowserNoPolls.png)
11. Kattintson egy szavazásra, és szavazzon.

      ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-sql-database"></a>SQL-adatbázis létrehozása
Hello adatbázishoz mi létrehozunk egy Azure SQL-adatbázis.

Ezeket a lépéseket követve létrehozhat egy adatbázist.

1. Jelentkezzen be a hello [Azure Portal].
2. Hello navigációs ablaktábla hello alul kattintson **új**. , kattintson a **adatok + tárolás** > **SQL-adatbázis**.
3. Konfigurálása új SQL-adatbázis hello hozzon létre egy új erőforráscsoportot, és válassza ki a megfelelő helyet hello.
4. Hello SQL-adatbázis létrehozása után kattintson **Megnyitás Visual Studio** hello adatbázis paneljén.
5. Kattintson a **úgy konfigurálni a tűzfalat**.
6. A hello **tűzfalbeállítások** panelen fel egy tűzfalszabályt a **KEZDŐ IP-** és **záró IP-Címnél** toohello nyilvános IP-cím a fejlesztési számítógép beállítása. Kattintson a **Save** (Mentés) gombra.

   Ez lehetővé teszi kapcsolatok toohello adatbázis-kiszolgálójának a fejlesztési számítógépén.
7. Hello adatbázis paneljén kattintson **tulajdonságok**, majd kattintson a **adatbázis-kapcsolati karakterláncok megjelenítése**.
8. Hello Másolás gombra tooput hello érték **ADO.NET** hello vágólapra.

## <a name="configure-hello-project"></a>Hello projekt konfigurálása
Ez a szakasz a webes alkalmazás toouse hello SQL adatbázis imént létrehozott konfigurálását végezzük el. További Python csomagok szükséges toouse SQL adatbázisok telepítjük a django alkalmazással is. Majd hello webalkalmazás helyileg helyszínekről futtassuk.

1. A Visual Studióban nyissa meg a **settings.py**, a hello *projektnév* mappát. Ideiglenesen illessze be a hello kapcsolati karakterláncot, hello-szerkesztőben. hello kapcsolati karakterlánc: a következő formátumban:

       Server=<ServerName>,<ServerPort>;Database=<DatabaseName>;User ID=<UserName>;Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;

Hello definíciójának szerkesztése `DATABASES` toouse hello fenti alapértékeket.

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

1. A Megoldáskezelőben a **Python-környezetek**, kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **Python-csomag telepítése**.
2. Hello telepítéséhez `pyodbc` használatával **pip**.

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackagePyodbc.png)
3. Hello telepítéséhez `django-pyodbc-azure` használatával **pip**.

     ![Telepítse a Python-csomag párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsDjangoSqlInstallPackageDjangoPyodbcAzure.png)
4. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.  Ezután válassza a **Django Create Superuser** elemet.

   Ezzel létrehoz hello táblák hello előző szakaszban létrehozott hello SQL-adatbázis. Hajtsa végre a hello kér toocreate egy felhasználó, aki nem rendelkezik toomatch hello felhasználói hello első szakaszban létrehozott hello sqlite-adatbázis.
5. Futtassa az alkalmazást hello `F5`. A létrehozása **létrehozása Sample Polls** és hello szavazás során elküldött adatok mintaszavazások hello SQL-adatbázis.

## <a name="publish-hello-web-app-tooazure-app-service"></a>Hello web app tooAzure App Service közzététele
hello Azure .NET SDK-t biztosít egy egyszerűen toodeploy a webes web app tooAzure App Service Web Apps.

1. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **közzététel**.

     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-sql/PollsCommonPublishWebSiteDialog.png)
2. Kattintson a **Microsoft Azure Web Apps** lehetőségre.
3. Kattintson a **új** toocreate egy új webalkalmazást.
4. Töltse ki a következő mezők hello **létrehozása**.

   * **A webalkalmazás neve**
   * **App Service-csomag**
   * **Erőforráscsoport**
   * **Régió**
   * Hagyja **adatbázis-kiszolgáló** túl beállítása**adatbázis**
5. Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.
6. A böngésző automatikusan toohello a közzétett webalkalmazás nyílik meg. Megtekintheti az hello web app működő elvárás hello segítségével **SQL** Azure-platformon futó adatbázis.

   Gratulálunk!

     ![Webböngésző](./media/web-sites-python-ptvs-django-sql/PollsDjangoAzureBrowser.png)

## <a name="next-steps"></a>Következő lépések
Hajtsa végre a Python-eszközökkel kapcsolatos további hivatkozások toolearn a Visual Studio, a Django és SQL-adatbázis.

* [Python Tools for Visual Studio – dokumentáció]
  * [Webes projektek]
  * [Cloud Service-projektek]
  * [Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)
* [A Django dokumentációja]
* [SQL Database]

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

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
