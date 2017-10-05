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
# <a name="flask-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a>Flask és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio
Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] hozhat létre egy egyszerű szavazások webalkalmazás használja a PTVS-minta sablonok egyikét. Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=qUtZWtPwbTk).

A szavazási webalkalmazást a tárházához absztrakciós határozza meg, így egyszerűen átválthatja különböző típusú tárházak (a memóriában, Azure Table Storage MongoDB) között.

A Microsoft Azure Storage-fiók létrehozása, a webalkalmazás Azure Table Storage használata konfigurálása és közzététele a webalkalmazás a megtanulhatja [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Tekintse meg a [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, a MongoDB, az Azure Table Storage, a MySQL és a SQL Database szolgáltatás. Az App Service-t tárgyaló jelen cikkben szereplő lépések hasonlóak az [Azure Cloud Services] fejlesztése esetében használtakhoz.

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2015
* [Python Tools 2.2 for Visual Studio]
* [Python Tools 2.2 for Visual Studio Samples VSIX]
* [Azure SDK Tools for VS 2015]
* [Python 2.7 32 bites] vagy [Python 3.4 32 bites]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="create-the-project"></a>A projekt létrehozása
Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával. Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat. Majd helyszínekről futtassuk az alkalmazást helyileg az alapértelmezett memórián belüli tárházba.

1. A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.
2. A [Python Tools 2.2 for Visual Studio Samples VSIX] projektsablonjai a **Python**, **Példák** elem alatt érhetők el. Válassza ki **szavazások Flask webes projekt** kattintson az OK gombra a projekt létrehozásához.
   
     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskNewProject.png)
3. A rendszer fel fogja kérni külső csomagok telepítésére. Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.
   
     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskExternalPackages.png)
4. Alapszintű értelmezőként válassza ki a **Python 2.7** vagy **Python 3.4** alkalmazást.
   
     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAddVirtualEnv.png)
5. Az alkalmazás működőképességét az `F5` billentyű lenyomásával ellenőrizze. Alapértelmezés szerint az alkalmazás használja egy memórián belüli tárház, amelyhez nincs szükség beállításra. Minden adat elveszik, amikor a webkiszolgáló leáll.
6. Kattintson a **létrehozása Sample Polls**, majd kattintson egy szavazásra, és szavazzon.
   
     ![Webböngésző](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a>Az Azure Storage-fiók létrehozása
Tárolási műveletek használatához Azure-tárfiók kell létrehoznia. Ezeket a lépéseket követve létrehozhat egy tárfiókot.

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **új** a portál bal felső ikonra, majd kattintson a **adatok + tárolás** > **Tárfiók**. Kattintson a **létrehozása**, majd a tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.
   
      ![Gyors létrehozás](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageCreate.png)
   
    A storage-fiók létrehozásakor a **értesítések** gomb villogjon, egy zöld **sikeres** és a storage-fiók panelje meg nyitva, hogy a létrehozott új erőforráscsoporthoz tartozó megjeleníthető.
3. Kattintson a **hívóbetűk** részt a storage-fiók panelen. Jegyezze fel a fiók nevét és a key1.
   
      ![Kulcsok](./media/web-sites-python-ptvs-flask-table-storage/PollsCommonAzureStorageKeys.png)
   
    Ezt az információt a projekt konfigurálásához a következő szakaszban kell azt.

## <a name="configure-the-project"></a>A projekt konfigurálása
Ebben a szakaszban az imént létrehozott tárfiókot használni, az alkalmazás konfigurálását végezzük el. Megtanulhatja az Azure portálról kapcsolódási beállítások beszerzése. Ezt követően helyileg fogja futtatni az alkalmazást.

1. A Visual Studióban, kattintson a jobb gombbal a projekt csomópontjára a Megoldáskezelőben, és válassza ki **tulajdonságok**. Kattintson a **Debug** fülre.
   
     ![Hibakeresési Projektbeállítások](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageProjectDebugSettings.png)
2. Állítsa be az alkalmazás által igényelt környezeti változók értékeit **Debug Server parancs**, **környezet**.
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   A környezeti változók állítja amikor Ön **Start Debugging**. Ha azt szeretné, hogy a változók adható meg, ha Ön **indítása nélkül hibakeresés**, állítsa be ugyanazokat az értékeket a **Server parancs futtatása** is.
   
   Azt is megteheti megadhatja a környezeti változók a Windows Vezérlőpulton. Ez a beállítás nagyobb Ha azt szeretné, ne tárolja a hitelesítő adatokat a forráskódban / project fájl. Vegye figyelembe, hogy szüksége lesz az alkalmazás elérhető legyen az új környezet értékek Visual Studio újraindítására.
3. A kódot, amely megvalósítja az Azure Table Storage tárház van **models/azuretablestorage.py**. Tekintse meg a [dokumentáció] Python a Table szolgáltatás használatáról további információt.
4. Futtassa az alkalmazást az `F5` billentyű lenyomásával. A létrehozása **létrehozása Sample Polls** és a szavazás során elküldött adatok mintaszavazások Azure Table Storage-ban.
   
   > [!NOTE]
   > A Python 2.7 virtuális környezetet a Visual Studio egy kivétel break okozhat.  Nyomja le az `F5` folytatja, a webes projekt betöltésekor.
   > 
   > 
5. Keresse meg a **kapcsolatos** lapon ellenőrizze, hogy az alkalmazást használ-e a **Azure Table Storage** tárházba.
   
     ![Webböngésző](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureTableStorageAbout.png)

## <a name="explore-the-azure-table-storage"></a>Az Azure Table-tároló tallózása
Akkor is könnyen megtekintése és szerkesztése a Visual Studio használatával a Cloud Explorer storage-táblákat. Ez a szakasz a Server Explorer a szavazások alkalmazás táblák tartalmának megtekintése használjuk.

> [!NOTE]
> Ehhez a Microsoft Azure eszközök legyen telepítve, mint a rendelkezésre álló része a [Azure SDK for .NET].
> 
> 

1. Nyissa meg **Cloud Explorer**. Bontsa ki a **Tárfiókok**, a tárfiók, majd **táblák**.
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. Kattintson duplán arra a a **szavazások** vagy **lehetőségek** táblázatban tekintheti meg a tábla tartalmát egy dokumentumablakra, valamint entitások hozzáadása/eltávolítása vagy módosítása.
   
     ![Tábla lekérdezés eredményei](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>A webalkalmazás közzététele az Azure App Service szolgáltatásban
Az Azure .NET SDK egyszerű módot kínál a webalkalmazása az Azure App Service szolgáltatásban történő közzétételére.

1. A **Solution Explorer** (Megoldáskezelő) felületén kattintson a jobb gombbal a projektcsomópontra, majd válassza a **Publish** (Közzététel) lehetőséget.
   
     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. Kattintson a **Microsoft Azure Web Apps** lehetőségre.
3. A **New** (Új) gombra kattintva hozzon létre egy új webalkalmazást.
4. Töltse ki a következő mezőket és kattintson a **létrehozása**.
   
   * **A webalkalmazás neve**
   * **App Service-csomag**
   * **Erőforráscsoport**
   * **Régió**
   * Hagyja változatlanul a **Database server** (Adatbázis-kiszolgáló) **No database** (Nincs adatbázis) beállítását
5. Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.
6. A webböngészőjében automatikusan a közzétett webalkalmazás nyílik meg. Ha tallózással az oldalról, láthatja, hogy az általa használt a **memórián belüli** -tárházban, nem a **Azure Table Storage** tárházba.
   
   Ennek oka a környezeti változók nem úgy van beállítva, az Azure App Service Web Apps-példányon a megadott alapértelmezett értékeket használ **settings.py**.

## <a name="configure-the-web-apps-instance"></a>Web Apps-példány beállítása
Ebben a szakaszban a webalkalmazások példány környezeti változók konfigurálását végezzük el.

1. A [Azure Portal](https://portal.azure.com), megnyitásához kattintson a webalkalmazása panelén **Tallózás** > **alkalmazásszolgáltatások** > a webes alkalmazás neve.
2. A webalkalmazás panelen kattintson **összes beállítás**, majd kattintson a **Alkalmazásbeállítások**.
3. Görgessen le a **Alkalmazásbeállítások** szakaszt, és értékeinek beállítása **TÁRHÁZ\_neve**, **tárolási\_neve** és **tárolási\_kulcs** leírtak szerint a **a projekt konfigurálásához** szakasz fenti.
   
     ![Alkalmazásbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. Kattintson a **Mentés** gombra. Az értesítéseket, hogy a módosítások alkalmazása megtörtént-e fogadását követően kattintson a **Tallózás** a webes alkalmazás fő paneljén.
5. A webes alkalmazás megfelelően működik, használatával kell megjelennie a **Azure Table Storage** tárházba.
   
   Gratulálunk!
   
     ![Webböngésző](./media/web-sites-python-ptvs-flask-table-storage/PollsFlaskAzureBrowser.png)

## <a name="next-steps"></a>Következő lépések
Kövesse az alábbi hivatkozásokat tudjon meg többet a Python Tools for Visual Studio, a Flask és az Azure Table Storage.

* [Python Tools for Visual Studio – dokumentáció]
  * [Webes projektek]
  * [Cloud Service-projektek]
  * [Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)
* [Flask-dokumentáció]
* [Azure Storage]
* [Pythonhoz készült Azure SDK]
* [A Table Storage szolgáltatás a Python használata]

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

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
