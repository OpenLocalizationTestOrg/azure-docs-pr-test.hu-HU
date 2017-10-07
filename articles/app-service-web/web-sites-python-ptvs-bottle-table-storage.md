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
# <a name="bottle-and-azure-table-storage-on-azure-with-python-tools-22-for-visual-studio"></a>Bottle és az Azure Table Storage Azure Python Tools 2.2 for Visual Studio
Ebben az oktatóanyagban fogjuk használni [a Python Tools for Visual Studio] toocreate egy egyszerű lekérdezi a web app használatával hello PTVS minta sablonok egyikét. Ez az oktatóanyag is rendelkezésre áll, mint egy [videó](https://www.youtube.com/watch?v=GJXDGaEPy94).

hello szavazási webalkalmazást a tárházához absztrakciós határozza meg, így egyszerűen átválthatja különböző típusú tárházak (a memóriában, Azure Table Storage MongoDB) között.

A Microsoft megtanulhatja, hogyan toocreate egy Azure Storage fiók, valamint hogyan tooconfigure hello web app toouse Azure Table Storage, és milyen toopublish hello webalkalmazás túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Lásd: hello [Python fejlesztői központ] további fejlesztését ismertető cikkeket az Azure App Service Web Apps használatával Bottle PTVS, a Flask és a Django webes keretrendszerek, a MongoDB, az Azure Table Storage, a MySQL és a SQL Database szolgáltatás. Amíg ez a cikk foglalkozik az App Service, hello lépések hasonlóak fejlesztésekor [Azure Felhőszolgáltatások].

## <a name="prerequisites"></a>Előfeltételek
* Visual Studio 2015
* [Python Tools 2.2 for Visual Studio]
* [Python Tools 2.2 for Visual Studio Samples VSIX]
* [Azure SDK Tools for VS 2015]
* [Python 2.7 32 bites] vagy [Python 3.4 32 bites]

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="create-hello-project"></a>Hello projekt létrehozása
Ebben a szakaszban létrehozunk egy Visual Studio-projekt mintasablon használatával. Azt a virtuális környezetet hozhat létre, és telepíti a szükséges csomagokat. Majd helyszínekről futtassuk hello alkalmazást helyileg hello alapértelmezett memórián belüli tárházba.

1. A Visual Studio felületén válassza a **File** (Fájl), **New Project** (Új projekt) lehetőséget.
2. a hello projektsablonjai hello [Python Tools 2.2 for Visual Studio Samples VSIX] alatt érhetők el **Python**, **minták**. Válassza ki **szavazások Bottle webes projekt** , és kattintson az OK toocreate hello projekt.
   
     ![Új projekt párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleNewProject.png)
3. Külső csomagok felszólító tooinstall lesz. Válassza az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőséget.
   
     ![Az External Packages (Külső csomagok) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleExternalPackages.png)
4. Válassza ki **Python 2.7** vagy **Python 3.4** , hello alapszintű értelmezőt.
   
     ![Az Add Virtual Environment (Virtuális környezet hozzáadása) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAddVirtualEnv.png)
5. Győződjön meg arról, hogy működik-e hello alkalmazás billentyűkombináció lenyomásával `F5`. Alapértelmezés szerint a hello alkalmazás használ egy memórián belüli tárház, amelyhez nincs szükség beállításra. Minden adat elvész, amikor hello webkiszolgáló le van állítva.
6. Kattintson a **létrehozása Sample Polls**, majd kattintson egy szavazásra, és szavazzon.
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleInMemoryBrowser.png)

## <a name="create-an-azure-storage-account"></a>Az Azure Storage-fiók létrehozása
toouse tárolási műveletek, egy az Azure storage-fiók szükséges. Ezeket a lépéseket követve létrehozhat egy tárfiókot.

1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/).
2. Kattintson a hello **új** hello felül ikon hello portálon a bal oldali, majd kattintson a **adatok + tárolás** > **Tárfiók**.  Kattintson a hello **létrehozása** gombra, majd hello tárfiók adjon egy egyedi nevet, és hozzon létre egy új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) hozzá.
   
      ![Gyors létrehozás](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageCreate.png)
   
    Hello tárfiók létrehozásakor hello **értesítések** gomb villogjon, egy zöld **sikeres** és hello tárolási fiók panelje meg nyitva, hogy tartozik-e új erőforrás toohello tooshow csoportjának, létre.
3. Kattintson a hello **hívóbetűk** hello tárolási fiók panelen részt. Jegyezze fel a hello fióknevet és key1.
   
      ![Kulcsok](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonAzureStorageKeys.png)
   
    Fel kell az adatokat tooconfigure hello a következő szakaszban a projekthez.

## <a name="configure-hello-project"></a>Hello projekt konfigurálása
Ebben a szakaszban az alkalmazás toouse hello tárolására imént létrehozott új fiók konfigurálását végezzük el. Ezt követően helyileg fogja futtatni hello alkalmazás.

1. A Visual Studióban, kattintson a jobb gombbal a projekt csomópontjára a Megoldáskezelőben, és válassza ki **tulajdonságok**. Kattintson a hello **Debug** fülre.
   
     ![Hibakeresési Projektbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageProjectDebugSettings.png)
2. Állítsa be a hello alkalmazáshoz szükséges környezeti változók értékeinek hello **Debug Server parancs**, **környezet**.
   
       REPOSITORY_NAME=azuretablestorage
       STORAGE_NAME=<storage account name>
       STORAGE_KEY=<primary access key>
   
   Hello környezeti változók állítja amikor Ön **Start Debugging**. Meg, ha azt szeretné, ha hello változók toobe meg **indítása nélkül hibakeresés**, azonos értékének beállítása hello **Server parancs futtatása** is.
   
   Azt is megteheti megadhatja a környezeti változók hello Windows Vezérlőpult használatával. Ez az jobb megoldás, ha szeretné, hogy hitelesítő adatok tárolása a forráskód tooavoid / project fájlt. Vegye figyelembe, hogy szüksége lesz a Visual Studio toorestart hello új környezet értékek toobe toohello elérhető alkalmazás.
3. hello kódot, amely megvalósítja az hello Azure Table Storage tárház van **models/azuretablestorage.py**. Lásd: hello [dokumentáció] további információt a toouse Table szolgáltatás a Python.
4. Futtassa az alkalmazást hello `F5`. A létrehozása **létrehozása Sample Polls** és hello szavazás során elküldött adatok mintaszavazások Azure Table Storage-ban.
   
   > [!NOTE]
   > Visual Studio egy kivétel break hello Python 2.7 virtuális környezet vezethet.  Nyomja le az `F5` toocontinue hello webes projekt betöltésekor.
   > 
   > 
5. Keresse meg a toohello **kapcsolatos** lap tooverify, amely hello alkalmazás által használt hello **Azure Table Storage** tárházba.
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureTableStorageAbout.png)

## <a name="explore-hello-azure-table-storage"></a>Hello Azure Table Storage felfedezés
Könnyen tooview és szerkesztése a Visual Studio használatával a Cloud Explorer storage-táblákat. Ez a szakasz a Server Explorer tooview hello hello szavazások alkalmazás táblák tartalmát fogjuk használni.

> [!NOTE]
> Ehhez szükséges a Microsoft Azure eszközök toobe telepítve, amelyek állnak rendelkezésre hello részeként [Azure SDK for .NET].
> 
> 

1. Nyissa meg **Cloud Explorer**. Bontsa ki a **Tárfiókok**, a tárfiók, majd **táblák**.
   
     ![Cloud Explorer](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorer.png)
2. Kattintson duplán arra a hello **szavazások** vagy **lehetőségek** tábla tooview hello tartalmát egy dokumentumablakra, valamint hozzáadása/eltávolítása vagy módosítása entitások hello táblájában.
   
     ![Tábla lekérdezés eredményei](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonServerExplorerTable.png)

## <a name="publish-hello-web-app-tooazure-app-service"></a>Hello web app tooAzure App Service közzététele
hello Azure .NET SDK-t biztosít egy egyszerűen toodeploy a webes alkalmazás tooAzure App Service.

1. A **Megoldáskezelőben**jobb gombbal a projektcsomópontra hello, és válassza ki **közzététel**.
   
     ![A Publish Web (Webes közzététel) párbeszédpanel](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonPublishWebSiteDialog.png)
2. Kattintson a **Microsoft Azure Web Apps** lehetőségre.
3. Kattintson a **új** toocreate egy új webalkalmazást.
4. Töltse ki a következő mezők hello **létrehozása**.
   
   * **A webalkalmazás neve**
   * **App Service-csomag**
   * **Erőforráscsoport**
   * **Régió**
   * Hagyja **adatbázis-kiszolgáló** túl beállítása**adatbázis**
5. Fogadja el az összes többi alapértelmezett értéket, majd kattintson a **Publish** (Közzététel) gombra.
6. A böngésző automatikusan toohello a közzétett webalkalmazás nyílik meg. Ha tallózással toohello oldalról, látni fogja, hogy használja-e hello **memórián belüli** tárház, nem hello **Azure Table Storage** tárházba.
   
   Ennek oka hello környezeti változók nem úgy van beállítva, az Azure App Service Web Apps-példányon hello megadott hello alapértelmezett értékeket használ **settings.py**.

## <a name="configure-hello-web-apps-instance"></a>Webalkalmazások példány hello konfigurálása
Ebben a szakaszban hello webalkalmazások példány környezeti változók konfigurálását végezzük el.

1. A [Azure Portal], megnyitásához hello webalkalmazása panelén **Tallózás** > **alkalmazásszolgáltatások** > a webes alkalmazás neve.
2. A webalkalmazás panelen kattintson **összes beállítás**, majd kattintson a **Alkalmazásbeállítások**.
3. Görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és hello értékeinek beállítása **TÁRHÁZ\_neve**, **tárolási\_neve** és  **TÁROLÁSI\_kulcs** hello leírtak **konfigurálása hello projekt** fenti szakaszban.
   
     ![Alkalmazásbeállítások](./media/web-sites-python-ptvs-bottle-table-storage/PollsCommonWebSiteConfigureSettingsTableStorage.png)
4. Kattintson a **Mentés** gombra. Hello értesítéseket, hogy alkalmazása megtörtént-e a hello módosítások fogadását követően kattintson a **Tallózás** hello webes alkalmazás fő paneljén.
5. Megtekintheti az hello web app működő elvárás hello segítségével **Azure Table Storage** tárházba.
   
   Gratulálunk!
   
     ![Webböngésző](./media/web-sites-python-ptvs-bottle-table-storage/PollsBottleAzureBrowser.png)

## <a name="next-steps"></a>Következő lépések
Hajtsa végre a Python-eszközökkel kapcsolatos további hivatkozások toolearn a Visual Studio, a Bottle és az Azure Table Storage.

* [Python Tools for Visual Studio – dokumentáció]
  * [Webes projektek]
  * [Cloud Service-projektek]
  * [Remote Debugging on Microsoft Azure] (Távoli hibakeresés a Microsoft Azure-ban)
* [Bottle dokumentáció]
* [Azure Storage]
* [Pythonhoz készült Azure SDK]
* [Hogyan tooUse hello Table Storage szolgáltatást Python]

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

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
