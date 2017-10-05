---
title: "Webalkalmazások létrehozása a Flask az Azure-ban"
description: "Ez az oktatóanyag bemutatja a Python webes alkalmazás Azure-on futó."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: b7f4ca3a-0b52-4108-90a7-f7e07ac73da0
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/20/2016
ms.author: huvalo
ms.openlocfilehash: 29457a39ee3df0bbdbc9869cdce0e14bd85b7302
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a>Webalkalmazások létrehozása a Flask az Azure-ban
Ez az oktatóanyag Python futtatásának első lépéseit ismerteti [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).  A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja!  Az alkalmazása növekedésével átválthat fizetős üzemeltetésre, valamint integrálhatja alkalmazásába az összes többi Azure-szolgáltatást is.

Egy alkalmazás, a Flask webes keretrendszer használatával hoz létre (lásd az oktatóanyag alternatív verzióit [Django](web-sites-python-create-deploy-django-app.md) és [Bottle](web-sites-python-create-deploy-bottle-app.md)).  Lesz a webhely létrehozása az Azure katalógusában, Git-telepítésének beállítása és klónozza a tárházat helyileg.  Ezt követően helyileg fogja futtatni az alkalmazást, módosításokat fog elvégezni rajta, majd véglegesíteni fogja őket, és el fogja küldeni az Azure-ba.  Ez az oktatóanyag ennek a folyamatnak a Windows vagy Mac/Linux operációs rendszeren történő végrehajtását mutatja be.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha nem szeretne regisztrálni Azure-fiókot az Azure App Service megismerése előtt, lépjen [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra, ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* Windows, Mac vagy Linux
* Python 2.7 vagy 3.4
* setuptools, pip, virtualenv (csak Python 2.7)
* Git
* [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) – Megjegyzés: szabadon választható

**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.

### <a name="windows"></a>Windows
Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését.  Ezzel többek között a következőket telepíti: a Python 32 bites verziója, setuptools, pip, virtualenv (a Python 32 bites verziója van telepítve az Azure-gazdagépeken).  Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.

A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk.  Visual Studio használata esetén használhatja a beépített Git-támogatást.

Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését.  Ez ugyan nem kötelező, de ha rendelkezik az ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat is magában foglaló [Visual Studio] szoftverrel, a nagyszerű Python IDE előnyeit is élvezheti.

### <a name="maclinux"></a>Mac/Linux
A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.

## <a name="web-app-create-on-the-azure-portal"></a>A webalkalmazás létrehozása az Azure-portál
Saját alkalmazása létrehozásának első lépése egy webalkalmazás létrehozása az [Azure portálon](https://portal.azure.com). 

1. Jelentkezzen be az Azure portálra, majd kattintson a bal alsó sarokban található **NEW** (ÚJ) gombra. 
2. Kattintson a **Web + mobil** elemre.
3. A keresőmezőbe írja be a „python” kifejezést.
4. A keresési eredmények között, válassza ki a **Flask**, majd kattintson a **létrehozása**.
5. Az új Flask alkalmazás, például egy új App Service-csomag és egy új erőforráscsoport létrehozásával az konfigurálása. Ezt követően kattintson a **Create** (Létrehozás) gombra.
6. Konfigurálja az újonnan létrehozott webalkalmazáshoz tartozó Git-közzétételt a [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben) részben megadott utasítások szerint.

## <a name="application-overview"></a>Az alkalmazás áttekintése
### <a name="git-repository-contents"></a>A Git-tárház tartalma
Az alábbiakban áttekintjük a kiindulási Git-tárházban található fájlokat. A következő szakaszban ezek klónozását fogjuk elvégezni.

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

Az alkalmazás fő forrásai.  3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel.  Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.

    \runserver.py

Helyi fejlesztési kiszolgáló támogatása. Ezzel az alkalmazás helyileg történő futtatása.

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.

    \ptvs_virtualenv_proxy.py

IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.

    \requirements.txt

Az alkalmazás számára szükséges külső csomagok. A telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítését.

    \web.2.7.config
    \web.3.4.config

IIS-konfigurációs fájlok.  A telepítési parancsfájl a megfelelő web.x.y.config fájlt fogja használni, és web.config fájlként fogja azt másolni.

### <a name="optional-files---customizing-deployment"></a>Opcionális fájlok – A telepítés testre szabása
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Opcionális fájlok – Python-futtatókörnyezet
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>További fájlok a kiszolgálón
Egyes fájlok megtalálhatóak ugyan a kiszolgálón, de nem lettek felvéve a Git-tárházba.  Ezeket a telepítési parancsfájl hozza létre.

    \web.config

IIS-konfigurációs fájl.  Minden telepítéshez a web.x.y.config fájlból jön létre.

    \env\

Python virtuális környezet.  Ha egy kompatibilis virtuális környezet már nem létezik az alkalmazás létre üzembe helyezése során.  Megtörténik a requirements.txt fájlban felsorolt csomagok telepítése, a korábban már telepített csomagokat azonban a pip már nem telepíti újra.

A következő három szakaszban a webalkalmazások fejlesztéséről talál információkat, három különböző környezetben:

* Windows, Python Tools for Visual Studio alkalmazással
* Windows, parancssorral
* Mac/Linux, parancssorral

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Webalkalmazás-fejlesztés – Windows – Python Tools for Visual Studio
### <a name="clone-the-repository"></a>A tárház klónozása
Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával. További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).

Nyissa meg a tárház gyökérkönyvtárában található megoldásfájlt (.sln).

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Most helyi telepítéshez tartozó virtuális környezetet hozunk létre.  Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.

* Győződjön meg arról, hogy a környezet neve a következő: `env`.
* Válassza ki az alapszintű értelmezőt.  Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).
* Győződjön meg arról, hogy a csomagok letöltése és telepítése be van jelölve.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

Kattintson a **Create** (Létrehozás) gombra.  Ezzel létrejön a virtuális környezet, valamint települnek a requirements.txt fájlban található függőségek.

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A hibakeresés elindításához nyomja le az F5 billentyűt, a webböngészőjében. Ekkor automatikusan a helyileg futtatott lap nyílik meg.

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

Többek között töréspontokat állíthat be a forrásokban, vagy használhatja a figyelőablakokat.  A különböző funkciókról további információkat [a Python Tools for Visual Studio dokumentációjában] talál.

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.

A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet.  Csomag telepítéséhez kattintson a jobb gombbal a virtuális környezetre, majd válassza az **Install Python Package** (Python-csomag telepítése) lehetőséget.

Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez adja meg az `azure` karakterláncot:

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

Kattintson a jobb gombbal a virtuális környezetre, majd a requirements.txt fájl frissítéséhez válassza a **Generate requirements.txt** (requirements.txt fájl létrehozása) lehetőséget.

Ezt követően véglegesítse a requirements.txt fájl módosításait a Git-tárházban.

### <a name="deploy-to-azure"></a>Üzembe helyezés az Azure-ban
Az üzembe helyezés indításához kattintson a **Sync** (Szinkronizálás) vagy a **Push** (Leküldés) lehetőségre.  A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

Az első üzembe helyezés hosszabb időt vesz igénybe, mivel ennek során megtörténik többek között a virtuális környezet létrehozása és a csomagok telepítése is.

A Visual Studio nem jeleníti meg az üzembe helyezés állapotát.  A kimenet áttekintéséhez lásd: [Hibaelhárítás – Üzembe helyezés](#troubleshooting-deployment).

A módosítások megtekintéséhez lépjen az Azure URL-címére.

## <a name="web-app-development---windows---command-line"></a>Webalkalmazás-fejlesztés – Windows – parancssor
### <a name="clone-the-repository"></a>A tárház klónozása
Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával, majd távoli tárházként vegye fel az Azure-tárházat. További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Fejlesztői célra létre fogunk hozni egy új virtuális környezetet (ezt ne vegye fel a tárházba).  A Python virtuális környezetei nem helyezhetőek át, így az alkalmazáson dolgozó összes fejlesztő helyileg hozza létre a sajátját.

Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).

Python 2.7 esetén:

    c:\python27\python.exe -m virtualenv env

Python 3.4 esetén:

    c:\python34\python.exe -m venv env

Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat. A csomagok virtuális környezetben történő telepítéséhez a tárház gyökérkönyvtárában található requirements.txt fájlt használhatja:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A következő paranccsal indíthatja el az alkalmazást egy fejlesztési kiszolgáló alatt:

    env\scripts\python runserver.py

A konzolon megjelenik az URL-cím és az a port, amelyen a kiszolgáló figyel:

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

Ezt követően nyissa meg a webböngészőjében ezt az URL-címet.

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.

A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet.  Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez írja be a következőt:

    env\scripts\pip install azure

Győződjön meg arról, hogy frissítette a requirements.txt fájlt:

    env\scripts\pip freeze > requirements.txt

Véglegesítse a módosításokat:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Üzembe helyezés az Azure-ban
Az üzembe helyezés indításához küldje le a módosításokat az Azure-ba:

    git push azure master

Itt megtekinthető a telepítési parancsfájl kimenete, amely tartalmazza a virtuális környezet létrehozását, a csomagok telepítését és a web.config fájl létrehozását.

A módosítások megtekintéséhez lépjen az Azure URL-címére.

## <a name="web-app-development---maclinux---command-line"></a>Webalkalmazás-fejlesztés – Mac/Linux – parancssor
### <a name="clone-the-repository"></a>A tárház klónozása
Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával, majd távoli tárházként vegye fel az Azure-tárházat. További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Fejlesztői célra létre fogunk hozni egy új virtuális környezetet (ezt ne vegye fel a tárházba).  A Python virtuális környezetei nem helyezhetőek át, így az alkalmazáson dolgozó összes fejlesztő helyileg hozza létre a sajátját.

Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).

Python 2.7 esetén:

    python -m virtualenv env

Python 3.4 esetén:

    python -m venv env
vagy pyvenv env

Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat. A csomagok virtuális környezetben történő telepítéséhez a tárház gyökérkönyvtárában található requirements.txt fájlt használhatja:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A következő paranccsal indíthatja el az alkalmazást egy fejlesztési kiszolgáló alatt:

    env/bin/python runserver.py

A konzolon megjelenik az URL-cím és az a port, amelyen a kiszolgáló figyel:

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

Ezt követően nyissa meg a webböngészőjében ezt az URL-címet.

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.

A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet.  Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez írja be a következőt:

    env/bin/pip install azure

Győződjön meg arról, hogy frissítette a requirements.txt fájlt:

    env/bin/pip freeze > requirements.txt

Véglegesítse a módosításokat:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a>Üzembe helyezés az Azure-ban
Az üzembe helyezés indításához küldje le a módosításokat az Azure-ba:

    git push azure master

Itt megtekinthető a telepítési parancsfájl kimenete, amely tartalmazza a virtuális környezet létrehozását, a csomagok telepítését és a web.config fájl létrehozását.

A módosítások megtekintéséhez lépjen az Azure URL-címére.

## <a name="troubleshooting---package-installation"></a>Hibaelhárítás – Csomagok telepítése
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Hibaelhárítás – Virtuális környezet
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Következő lépések
Az alábbi hivatkozásokból tudhat meg többet a Flask és a Python Tools for Visual Studio: 

* [Flask-dokumentáció]
* [a Python Tools for Visual Studio dokumentációjában]

Információk az Azure Table Storage és a MongoDB használatával:

* [Flask és a Python Tools for Visual Studio Azure MongoDB]
* [Flask és az Azure Table Storage Azure Python Tools for Visual Studio]

További információ: a [Python fejlesztői központ](/develop/python/).

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Flask és a Python Tools for Visual Studio Azure MongoDB]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure
[Flask és az Azure Table Storage Azure Python Tools for Visual Studio]: web-sites-python-ptvs-flask-table-storage.md

<!--External Link references-->
[Python 2.7-hez készült Azure SDK]: http://go.microsoft.com/fwlink/?linkid=254281
[Python 3.4-hez készült Azure SDK]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git for Windows]: http://msysgit.github.io/
[GitHub for Windows]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[a Python Tools for Visual Studio dokumentációjában]: http://aka.ms/ptvsdocs
[Flask-dokumentáció]: http://flask.pocoo.org/ 

