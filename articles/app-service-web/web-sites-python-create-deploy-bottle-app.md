---
title: "az Azure-ban Bottle aaaPython webalkalmazások"
description: "Ez az oktatóanyag bemutatja a toorunning a Python webalkalmazás az Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 84f043b8-9d05-479f-a9d0-d0d9a32a0bb9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2016
ms.author: huvalo
ms.openlocfilehash: 98acd7d8fcdbba326625121c20f9237d2663ea1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-bottle-in-azure"></a>Webalkalmazások létrehozása a Bottle az Azure-ban
Ez az oktatóanyag leírja, hogyan tooget indulása Python az Azure App Service Web Apps. A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja! Az alkalmazás forgalmához igazítható, megváltoztathatja az toopaid üzemeltető, és integrálhatja az összes hello más Azure-szolgáltatásokkal.

A webes alkalmazás hello Bottle webes keretrendszer használatával hoz létre (lásd az oktatóanyag alternatív verzióit [Django](web-sites-python-create-deploy-django-app.md) és [Flask](web-sites-python-create-deploy-flask-app.md)). Hello webalkalmazás létrehoz hello Azure Piactérről származó, beállítása a Git üzemelő példányt, és helyileg hello tárház klónozása. Ezután lesz hello webalkalmazás helyileg futtassa, módosításokat, véglegesítse és küldje le őket túl[Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). Hogyan hello oktatóanyag azt mutatja be toodo ezt a Windows vagy Mac/Linux.

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* Windows, Mac vagy Linux
* Python 2.7 vagy 3.4
* setuptools, pip, virtualenv (csak Python 2.7)
* Git
* [Python Tools 2.2 for Visual Studio][Python Tools 2.2 for Visual Studio] (PTVS) – Megjegyzés: Ez nem kötelező

**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.

### <a name="windows"></a>Windows
Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését. Ez a Python, setuptools, pip, virtualenv stb (az Azure-gazdagépeken hello telepített Python 32 bites) hello 32 bites verzióját telepíti. Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.

A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk. Ha a Visual Studio használata esetén használhatja hello integrált Git-támogatást.

Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését. Ez nem kötelező, de ha rendelkezik [Visual Studio], beleértve a hello ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat, és ekkor kap egy nagyszerű Python IDE.

### <a name="maclinux"></a>Mac/Linux
A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.

## <a name="web-app-creation-on-hello-azure-portal"></a>Webalkalmazás létrehozása a hello Azure portál
hello saját alkalmazása létrehozásának első lépése az toocreate hello hello webalkalmazás [Azure Portal](https://portal.azure.com).  

1. Jelentkezzen be hello Azure Portal, majd kattintson a hello **új** hello bal alsó sarokban gombjára. 
2. Hello a keresőmezőbe írja be a "python".
3. Hello keresési eredmények között, válassza ki a **Bottle**, majd kattintson a **létrehozása**.
4. Hello új Bottle alkalmazás, például egy új App Service-csomag és egy új erőforráscsoport létrehozásával az konfigurálása. Ezt követően kattintson a **Create** (Létrehozás) gombra.
5. Az újonnan létrehozott webalkalmazáshoz tartozó Git-közzététel konfigurálása: hello utasításokat követve [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Az alkalmazás áttekintése
### <a name="git-repository-contents"></a>A Git-tárház tartalma
Itt a hello kezdeti Git-tárházban, amely klónozását fogjuk elvégezni hello a következő szakaszban találhat hello fájlok nyújt áttekintést.

    \routes.py
    \static\content\
    \static\fonts\
    \static\scripts\
    \views\about.tpl
    \views\contact.tpl
    \views\index.tpl
    \views\layout.tpl

Hello alkalmazás fő forrásai. 3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel.  Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.

    \app.py

Helyi fejlesztési kiszolgáló támogatása. Az toorun hello alkalmazás helyileg használatához.

    \BottleWebProject.pyproj
    \BottleWebProject.sln

A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.

    \ptvs_virtualenv_proxy.py

IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.

    \requirements.txt

Az alkalmazás számára szükséges külső csomagok. hello telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítése hello.

    \web.2.7.config
    \web.3.4.config

IIS-konfigurációs fájlok. hello telepítési parancsfájl hello megfelelő web.x.y.config használja, és web.config fájlként másolásához.

### <a name="optional-files---customizing-deployment"></a>Opcionális fájlok – A telepítés testre szabása
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Opcionális fájlok – Python-futtatókörnyezet
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>További fájlok a kiszolgálón
Egyes fájlok hello kiszolgálón léteznek, de nem kerülnek toohello git-tárház. Ezek hello telepítési parancsfájl hozza létre.

    \web.config

IIS-konfigurációs fájl. Minden telepítéshez a web.x.y.config fájlból jön létre.

    \env\

Python virtuális környezet. Telepítése során létrehozott Ha kompatibilis virtuális környezet már nem létezik a hello webalkalmazásban.  A requirements.txt fájlban felsorolt csomagok telepítése a pip, de a pip kihagyja a telepítést, ha már telepítve vannak a hello csomagok.

hello következő 3 szakaszok mutatják be, hogyan tooproceed hello a webalkalmazás-e a fejlesztés alatt 3 különböző környezetekben:

* Windows, Python Tools for Visual Studio alkalmazással
* Windows, parancssorral
* Mac/Linux, parancssorral

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Webes alkalmazás fejlesztés – Windows – Python Tools for Visual Studio
### <a name="clone-hello-repository"></a>Hello tárház klónozása
Első lépésben klónozza hello tárházat hello található URL-cím hello Azure portál használatával. További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

Nyissa meg a hello megoldásfájlt (.sln), hello hello tárház gyökérkönyvtárában található.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-solution-bottle.png)

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Most helyi telepítéshez tartozó virtuális környezetet hozunk létre. Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.

* Ellenőrizze, hogy hello név hello környezet `env`.
* Válassza ki a hello alapszintű értelmezőt. Győződjön meg arról, hogy toouse hello a webalkalmazás kiválasztott Python ugyanazon verzióját (a runtime.txt vagy hello **Alkalmazásbeállítások** panelen található webalkalmazását az Azure portál hello).
* Ellenőrizze, hogy hello beállítás toodownload és telepítési csomagok be van jelölve.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-add-virtual-env-27.png)

Kattintson a **Create** (Létrehozás) gombra. Ezzel hello virtuális környezet létrehozása, és telepítse a Requirements.txt fájlban található függőségek.

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
Nyomja le az F5 toostart Hibakeresés és a webböngészőjében automatikusan toohello helyileg futtatott lap ekkor megnyílik.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

Töréspontokat állíthat hello adatforrások, tekintse meg a windows hello, stb. Lásd: hello [Python Tools for Visual Studio dokumentációjában] hello további információt a különböző szolgáltatások.

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.

A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-commit-bottle.png)

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazás a Python és Bottle kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet. tooinstall egy csomagot, kattintson a jobb gombbal a hello virtuális környezetet, és válassza **Python-csomag telepítése**.

Például tooinstall hello hozzáférést tud biztosítani tooAzure storage, service bus és további Azure-szolgáltatásokhoz, írja be Pythonhoz készült Azure SDK `azure`:

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-install-package-dialog.png)

Kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **készítése a requirements.txt** tooupdate Requirements.txt fájlt.

Ezt követően véglegesítse hello módosítások toorequirements.txt toohello Git-tárházba.

### <a name="deploy-tooazure"></a>TooAzure telepítése
tootrigger a telepítést, kattintson a **szinkronizálási** vagy **leküldéses**. A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.

![](./media/web-sites-python-create-deploy-bottle-app/ptvs-git-push.png)

hello első központi telepítési időt vesz igénybe, mivel ekkor létrehoz egy virtuális környezethez, a csomagok telepítése, stb.

A Visual Studio nem jeleníti meg hello hello központi telepítés végrehajtási állapotát. Ha szeretné tooreview hello kimeneti, ld. hello [hibaelhárítás – üzembe helyezés](#troubleshooting-deployment).

Keresse meg a toohello Azure URL-cím tooview a módosításokat.

## <a name="web-app-development---windows---command-line"></a>Webes alkalmazások fejlesztéséhez. - Windows - parancs sor
### <a name="clone-hello-repository"></a>Hello tárház klónozása
Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat. További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház). Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.

Győződjön meg arról, hogy toouse hello (a runtime.txt vagy hello Alkalmazásbeállítások paneljén webalkalmazását az Azure portál hello) a webalkalmazás kiválasztott Python ugyanazon verzióját

Python 2.7 esetén:

    c:\python27\python.exe -m virtualenv env

Python 3.4 esetén:

    c:\python34\python.exe -m venv env

Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat. A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:

    env\scripts\python app.py

hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:

![](./media/web-sites-python-create-deploy-bottle-app/windows-run-local-bottle.png)

Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.

![](./media/web-sites-python-create-deploy-bottle-app/windows-browser-bottle.png)

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.

A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazás a Python és Bottle kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet. Például tooinstall hello Azure SDK for Python, amely lehetővé teszi az elérhető tooAzure storage, service bus és további Azure-szolgáltatásokhoz, típus:

    env\scripts\pip install azure

Győződjön meg arról, hogy tooupdate Requirements.txt fájlt:

    env\scripts\pip freeze > requirements.txt

Hello változtatások véglegesítése a határidő:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a>TooAzure telepítése
a központi telepítés tootrigger, leküldéses hello tooAzure módosítja:

    git push azure master

Hello a telepítési parancsfájl, beleértve a virtuális környezet létrehozását, a csomagok telepítését és a Web.config fájl létrehozását hello kimenetet fog látni.

Keresse meg a toohello Azure URL-cím tooview a módosításokat.

## <a name="web-app-development---maclinux---command-line"></a>Webalkalmazás-fejlesztés – Mac/Linux – parancssor
### <a name="clone-hello-repository"></a>Hello tárház klónozása
Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat. További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház). Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.

Győződjön meg arról, hogy toouse hello (a runtime.txt vagy hello alkalmazás beállítások panel webalkalmazását az Azure portál hello) a webalkalmazás kiválasztott Python ugyanazon verzióját.

Python 2.7 esetén:

    python -m virtualenv env

Python 3.4 esetén:

    python -m venv env
vagy pyvenv env

Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat. A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:

    env/bin/python app.py

hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:

![](./media/web-sites-python-create-deploy-bottle-app/mac-run-local-bottle.png)

Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.

![](./media/web-sites-python-create-deploy-bottle-app/mac-browser-bottle.png)

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.

A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazás a Python és Bottle kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet. Például tooinstall hello Azure SDK for Python, amely lehetővé teszi az elérhető tooAzure storage, service bus és további Azure-szolgáltatásokhoz, típus:

    env/bin/pip install azure

Győződjön meg arról, hogy tooupdate Requirements.txt fájlt:

    env/bin/pip freeze > requirements.txt

Hello változtatások véglegesítése a határidő:

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a>TooAzure telepítése
a központi telepítés tootrigger, leküldéses hello tooAzure módosítja:

    git push azure master

Hello a telepítési parancsfájl, beleértve a virtuális környezet létrehozását, a csomagok telepítését és a Web.config fájl létrehozását hello kimenetet fog látni.

Keresse meg a toohello Azure URL-cím tooview a módosításokat.

## <a name="troubleshooting---package-installation"></a>Hibaelhárítás – Csomagok telepítése
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Hibaelhárítás – Virtuális környezet
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Következő lépések
Hajtsa végre az alábbi hivatkozások toolearn Bottle és a Python Tools kapcsolatos további Visual Studio: 

* [Bottle dokumentáció]
* [Python Tools for Visual Studio dokumentációjában]

Információk az Azure Table Storage és a MongoDB használatával:

* [Bottle és a Python Tools for Visual Studio Azure MongoDB]
* [Bottle és az Azure Table Storage Azure Python Tools for Visual Studio]

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Bottle és a Python Tools for Visual Studio Azure MongoDB]: web-sites-python-ptvs-bottle-table-storage.md
[Bottle és az Azure Table Storage Azure Python Tools for Visual Studio]: web-sites-python-ptvs-bottle-table-storage.md

<!--External Link references-->
[Python 2.7-hez készült Azure SDK]: http://go.microsoft.com/fwlink/?linkid=254281
[Python 3.4-hez készült Azure SDK]: http://go.microsoft.com/fwlink/?linkid=516990
[python.org]: http://www.python.org/
[Git for Windows]: http://msysgit.github.io/
[GitHub for Windows]: https://windows.github.com/
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Visual Studio]: http://www.visualstudio.com/
[Python Tools for Visual Studio dokumentációjában]: http://aka.ms/ptvsdocs 
[Bottle dokumentáció]: http://bottlepy.org/docs/dev/index.html

