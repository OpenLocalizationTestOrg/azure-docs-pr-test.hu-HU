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
# <a name="django-and-mysql-on-azure-with-python-tools-22-for-visual-studio"></a>Django and MySQL on Azure with Python Tools 2.2 for Visual Studio (Django és MySQL az Azure-ban, Python Tools 2.2 for Visual Studio alkalmazással)
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Az oktatóanyag azt ismertetjük [a Python Tools for Visual Studio](https://www.visualstudio.com/vs/python) toocreate egy egyszerű lekérdezi a web app használatával hello PTVS minta sablonok egyikét. Megtudhatja, hogyan toouse MySQL-szolgáltatás Azure-platformon futó, hogyan tooconfigure hello web app toouse MySQL, és hogyan a toopublish hello webalkalmazás túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

> [!NOTE]
> Ebben az oktatóanyagban lévő hello információt is érhető el a következő videó hello:
> 
> [PTVS 2.1: Django-alkalmazás és MySQL][video]
> 
> 

Lásd: hello [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, az Azure Table Storage, a MySQL és az SQL-adatbázis szolgáltatás. Amíg ez a cikk foglalkozik az App Service, hello lépések hasonlóak fejlesztésekor [Azure Felhőszolgáltatások].

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2015
* [Python 2.7 32 bites] vagy [Python 3.4 32 bites]
* [Python Tools 2.2 for Visual Studio]
* [Python Tools 2.2 for Visual Studio Samples VSIX]
* [Azure SDK Tools for VS 2015]
* Django 1.9 vagy újabb

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of hello hello previous include. -->

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="create-hello-project"></a>Hello projekt létrehozása
Ebben a szakaszban mintasablon használatával fog létrehozni Visual Studio-projektet. Létrehozza majd a virtuális környezetet, és telepíti a szükséges csomagokat. Az sqlite használatával létre fog hozni egy helyi adatbázist. Majd hello alkalmazás helyileg fogja futtatni.

1. A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.
2. a hello projektsablonjai hello [Python Tools 2.2 for Visual Studio Samples VSIX] alatt érhetők el **Python**, **minták**. Válassza ki **Polls Django Web Project** , és kattintson az OK toocreate hello projekt.
   
    ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. Külső csomagok felszólító tooinstall lesz. Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.
   
    ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. Válassza ki **Python 2.7** vagy **Python 3.4** , hello alapszintű értelmezőt.
   
    ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.  Ezután válassza a **Django Create Superuser** elemet.
6. Ez a Django felügyeleti konzol megnyitásához, és hello projektmappa sqlite-adatbázis létrehozása. Hajtsa végre a hello kér toocreate a felhasználó.
7. Győződjön meg arról, hogy működik-e hello alkalmazás billentyűkombináció lenyomásával `F5`.
8. Kattintson a **jelentkezzen be** a hello navigációs sáv hello tetején.
   
    ![Django navigációs sáv](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. Adja meg hello hitelesítő hello Ön által létrehozott felhasználót hello adatbázis szinkronizálásakor.
   
    ![Bejelentkezési űrlap](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. Kattintson a **Create Sample Polls** (Mintaszavazások létrehozása) gombra.
    
     ![Create Sample Polls (Mintaszavazások létrehozása)](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. Kattintson egy szavazásra, és szavazzon.
    
     ![Szavazás mintaszavazásokon](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## <a name="create-a-mysql-database"></a>MySQL-adatbázis létrehozása
Hello adatbázis létre fog hozni üzemeltetett ClearDB MySQL-adatbázis az Azure-on.

Másik lehetőségként létrehozhatja saját Azure-beli virtuális gépét, majd telepítheti és felügyelheti a MySQL-t.

Az alábbi lépéseket követve ingyenes csomaggal rendelkező adatbázist hozhat létre.

1. Jelentkezzen be toohello [Azure Portal].
2. Hello hello navigációs ablaktábla tetején, kattintson **új**, majd kattintson a **adatok + tárolás**, és kattintson a **MySQL-adatbázis**.
3. Hello új MySQL-adatbázis konfigurálja egy új erőforráscsoport létrehozásával, és válassza ki a hello megfelelő helyet.
4. Hello MySQL-adatbázis létrehozása után kattintson **tulajdonságok** hello adatbázis paneljén.
5. Hello Másolás gombra tooput hello érték **KAPCSOLATI karakterlánc** hello vágólapra.

## <a name="configure-hello-project"></a>Hello projekt konfigurálása
Ebben a szakaszban konfigurálhatja a webes alkalmazás toouse hello MySQL-adatbázis most létrehozott. Emellett telepíteni fog olyan további Python csomagok szükséges toouse MySQL-adatbázisok a django alkalmazással. Majd hello webalkalmazást helyileg fogja futtatni.

1. A Visual Studióban nyissa meg a **settings.py**, a hello *projektnév* mappát. Ideiglenesen illessze be a hello kapcsolati karakterláncot, hello-szerkesztőben. hello kapcsolati karakterlánc: a következő formátumban:
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    Változás hello alapértelmezett adatbázis **motor** toouse MySQL, és állítsa be hello értékeinek **neve**, **felhasználói**, **jelszó** és  **ÁLLOMÁS** a hello **CONNECTIONSTRING**.
   
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
2. A Megoldáskezelőben a **Python-környezetek**, kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **Python-csomag telepítése**.
3. Hello telepítéséhez `mysqlclient` használatával **pip**.
   
    ![Az Install Package (Csomag telepítése) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **Python**, majd válassza ki **Django áttelepítése**.  Ezután válassza a **Django Create Superuser** elemet.
   
    Ezzel létrehoz hello táblák hello előző szakaszban létrehozott hello MySQL-adatbázis. Hajtsa végre a hello kér toocreate egy felhasználó, aki nem rendelkezik toomatch hello felhasználói hello Ez a cikk első szakaszában létrehozott hello sqlite-adatbázis.
5. Futtassa az alkalmazást hello `F5`. A létrehozása **létrehozása Sample Polls** és hello szavazás során elküldött adatok mintaszavazások hello MySQL-adatbázisban.

## <a name="publish-hello-web-app-tooazure-app-service"></a>Hello web app tooAzure App Service közzététele
hello Azure .NET SDK-t biztosít egy egyszerűen toodeploy a webes alkalmazás tooAzure App Service.

1. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **közzététel**.
   
    ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. Kattintson a **Microsoft Azure App Service** lehetőségre.
3. Kattintson a **új** toocreate egy új webalkalmazást.
4. Töltse ki a következő mezők hello **létrehozása**:
   
   * **A webalkalmazás neve**
   * **App Service-csomag**
   * **Erőforráscsoport**
   * **Régió**
   * Hagyja **adatbázis-kiszolgáló** túl beállítása**adatbázis**
5. Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.
6. A böngésző automatikusan toohello a közzétett webalkalmazás nyílik meg. Megtekintheti az hello web app működő elvárás hello segítségével **MySQL** Azure-platformon futó adatbázis.
   
    ![Webböngésző](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    Gratulálunk! A MySQL-alapú webes alkalmazás tooAzure közzététele sikeresen megtörtént.

## <a name="next-steps"></a>Következő lépések
Hajtsa végre a Python-eszközökkel kapcsolatos további hivatkozások toolearn a Visual Studio, a Django és MySQL.

* [Python Tools for Visual Studio – dokumentáció]
  * [Webes projektek]
  * [Cloud Service-projektek]
  * [Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)
* [A Django dokumentációja]
* [MySQL]

További információkért lásd: hello [Python fejlesztői központ](/develop/python/).

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
