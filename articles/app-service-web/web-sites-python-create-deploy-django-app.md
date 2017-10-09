---
title: "webalkalmazások aaaCreating djangóval az Azure-ban"
description: "Ez az oktatóanyag bemutatja a toorunning a Python webalkalmazás az Azure App Service Web Apps."
services: app-service\web
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: 9be1a05a-9460-49ae-94fb-9798f82c11cf
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/19/2016
ms.author: huvalo
ms.openlocfilehash: 26a131da358748bd6fe4ee5c114d0a8f91b83cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-django-in-azure"></a>Webalkalmazások létrehozása a Djangóval az Azure-ban
Ez az oktatóanyag leírja, hogyan tooget el Python futó [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714). A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja! Az alkalmazás forgalmához igazítható, megváltoztathatja az toopaid üzemeltető, és integrálhatja az összes hello más Azure-szolgáltatásokkal.

Egy alkalmazás hello Django webes keretrendszer használatával hoz létre (lásd az oktatóanyag alternatív verzióit [Flask](web-sites-python-create-deploy-flask-app.md) és [Bottle](web-sites-python-create-deploy-bottle-app.md)). Hello webalkalmazás létrehoz hello Azure Piactérről származó, beállítása a Git üzemelő példányt, és helyileg hello tárház klónozása. Ezután lesz hello alkalmazás helyileg történő futtatása, módosításokat, véglegesítse és küldje le őket tooAzure. Hogyan hello oktatóanyag azt mutatja be toodo ezt a Windows vagy Mac/Linux.

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
* [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) – Megjegyzés: szabadon választható

**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.

### <a name="windows"></a>Windows
Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését. Ez a Python, setuptools, pip, virtualenv stb (az Azure-gazdagépeken hello telepített Python 32 bites) hello 32 bites verzióját telepíti. Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.

A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk. Ha a Visual Studio használata esetén használhatja hello integrált Git-támogatást.

Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését. Ez nem kötelező, de ha rendelkezik [Visual Studio], beleértve a hello ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat, és ekkor kap egy nagyszerű Python IDE.

### <a name="maclinux"></a>Mac/Linux
A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.

## <a name="web-app-creation-on-portal"></a>Webalkalmazás létrehozása a portálon
hello saját alkalmazása létrehozásának első lépése az toocreate hello hello webalkalmazás [Azure Portal](https://portal.azure.com).

1. Jelentkezzen be hello Azure Portal, majd kattintson a hello **új** hello bal alsó sarokban gombjára.
2. Hello a keresőmezőbe írja be a "python".
3. Hello keresési eredmények között, válassza ki a **Django** (PTVS által közzétett), majd kattintson a **létrehozása**.
4. Hello új Django-alkalmazást, például egy új App Service-csomag és egy új erőforráscsoport létrehozása, konfigurálása. Ezt követően kattintson a **Create** (Létrehozás) gombra.
5. Az újonnan létrehozott webalkalmazáshoz tartozó Git-közzététel konfigurálása: hello utasításokat követve [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

## <a name="application-overview"></a>Az alkalmazás áttekintése
### <a name="git-repository-contents"></a>A Git-tárház tartalma
Itt a hello kezdeti Git-tárházban, amely klónozását fogjuk elvégezni hello a következő szakaszban találhat hello fájlok nyújt áttekintést.

    \app\__init__.py
    \app\forms.py
    \app\models.py
    \app\tests.py
    \app\views.py
    \app\static\content\
    \app\static\fonts\
    \app\static\scripts\
    \app\templates\about.html
    \app\templates\contact.html
    \app\templates\index.html
    \app\templates\layout.html
    \app\templates\login.html
    \app\templates\loginpartial.html
    \DjangoWebProject\__init__.py
    \DjangoWebProject\settings.py
    \DjangoWebProject\urls.py
    \DjangoWebProject\wsgi.py

Hello alkalmazás fő forrásai. 3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel. Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.

    \manage.py

A helyi felügyelet és a fejlesztési kiszolgáló támogatása. Az toorun hello alkalmazás helyileg használatához, szinkronizálása hello adatbázis stb.

    \db.sqlite3

Az alapértelmezett adatbázis. Hello alkalmazás toorun hello szükséges táblákat tartalmaz, de nem tartalmazza azokat a felhasználókat (hello adatbázis toocreate a felhasználó szinkronizálása).

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.

    \ptvs_virtualenv_proxy.py

IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.

    \requirements.txt

Az alkalmazás számára szükséges külső csomagok. hello telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítése hello.

    \web.2.7.config
    \web.3.4.config

IIS-konfigurációs fájlok. hello telepítési parancsfájl hello megfelelő web.x.y.config használja, és web.config fájlként másolásához.

### <a name="optional-files---customizing-deployment"></a>Opcionális fájlok – A telepítés testre szabása
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a>Opcionális fájlok – Python-futtatókörnyezet
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a>További fájlok a kiszolgálón
Egyes fájlok hello kiszolgálón léteznek, de nem kerülnek toohello git-tárház. Ezek hello telepítési parancsfájl hozza létre.

    \web.config

IIS-konfigurációs fájl. Minden telepítéshez a web.x.y.config fájlból jön létre.

    \env\

Python virtuális környezet. Telepítése során létrehozott Ha kompatibilis virtuális környezet már nem létezik a hello webalkalmazásban. A requirements.txt fájlban felsorolt csomagok telepítése a pip, de a pip kihagyja a telepítést, ha már telepítve vannak a hello csomagok.

hello következő 3 szakaszok mutatják be, hogyan tooproceed hello a webalkalmazás-e a fejlesztés alatt 3 különböző környezetekben:

* Windows, Python Tools for Visual Studio alkalmazással
* Windows, parancssorral
* Mac/Linux, parancssorral

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a>Webalkalmazás-fejlesztés – Windows – Python Tools for Visual Studio
### <a name="clone-hello-repository"></a>Hello tárház klónozása
Első lépésben klónozza hello tárházat hello található URL-cím hello Azure portál használatával. További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

Nyissa meg a hello megoldásfájlt (.sln), hello hello tárház gyökérkönyvtárában található.

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Most helyi telepítéshez tartozó virtuális környezetet hozunk létre. Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.

* Ellenőrizze, hogy hello név hello környezet `env`.
* Válassza ki a hello alapszintű értelmezőt. Győződjön meg arról, hogy toouse hello a webalkalmazás kiválasztott Python ugyanazon verzióját (a runtime.txt vagy hello **Alkalmazásbeállítások** panelen található webalkalmazását az Azure portál hello).
* Ellenőrizze, hogy hello beállítás toodownload és telepítési csomagok be van jelölve.

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

Kattintson a **Create** (Létrehozás) gombra. Ezzel hello virtuális környezet létrehozása, és telepítse a Requirements.txt fájlban található függőségek.

### <a name="create-a-superuser"></a>Felügyelő létrehozása
hello alkalmazáshoz tartozó hello adatbázishoz nincs megadva felügyelő. A sorrend toouse hello bejelentkezési funkciójának hello alkalmazás vagy hello a Django felügyeleti felülete (Ha úgy dönt, hogy tooenable azt), toocreate felügyelőt lesz szüksége.

Futtassa a következő parancsot hello parancssori a projektmappa parancssorából:

    env\scripts\python manage.py createsuperuser

Hajtsa végre a hello kér tooset hello felhasználónevet, jelszót, stb.

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
Nyomja le az F5 toostart Hibakeresés és a webböngészőjében automatikusan toohello helyileg futtatott lap ekkor megnyílik.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

Töréspontokat állíthat hello adatforrások, tekintse meg a windows hello, stb. Lásd: hello [Python Tools for Visual Studio dokumentációjában] hello további információt a különböző szolgáltatások.

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.

A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.

A pip használatával további csomagokat is telepíthet. tooinstall egy csomagot, kattintson a jobb gombbal a hello virtuális környezetet, és válassza **Python-csomag telepítése**.

Például tooinstall hello hozzáférést tud biztosítani tooAzure storage, service bus és további Azure-szolgáltatásokhoz, írja be Pythonhoz készült Azure SDK `azure`:

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

Kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **készítése a requirements.txt** tooupdate Requirements.txt fájlt.

Ezt követően véglegesítse hello módosítások toorequirements.txt toohello Git-tárházba.

### <a name="deploy-tooazure"></a>TooAzure telepítése
tootrigger a telepítést, kattintson a **szinkronizálási** vagy **leküldéses**. A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

hello első központi telepítési időt vesz igénybe, mivel ekkor létrehoz egy virtuális környezethez, a csomagok telepítése, stb.

A Visual Studio nem jeleníti meg hello hello központi telepítés végrehajtási állapotát. Ha szeretné tooreview hello kimeneti, ld. hello [hibaelhárítás – üzembe helyezés](#troubleshooting-deployment).

Keresse meg a toohello Azure URL-cím tooview a módosításokat.

## <a name="web-app-development---windows---command-line"></a>Webalkalmazás-fejlesztés – Windows – parancssor
### <a name="clone-hello-repository"></a>Hello tárház klónozása
Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat. További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a>Virtuális környezet létrehozása
Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház). Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.

Győződjön meg arról, hogy toouse hello (a runtime.txt vagy hello alkalmazás beállítások panel webalkalmazását az Azure portál hello) a webalkalmazás kiválasztott Python ugyanazon verzióját.

Python 2.7 esetén:

    c:\python27\python.exe -m virtualenv env

Python 3.4 esetén:

    c:\python34\python.exe -m venv env

Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat. A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a>Felügyelő létrehozása
hello alkalmazáshoz tartozó hello adatbázishoz nincs megadva felügyelő. A sorrend toouse hello bejelentkezési funkciójának hello alkalmazás vagy hello a Django felügyeleti felülete (Ha úgy dönt, hogy tooenable azt), toocreate felügyelőt lesz szüksége.

Futtassa a következő parancsot hello parancssori a projektmappa parancssorából:

    env\scripts\python manage.py createsuperuser

Hajtsa végre a hello kér tooset hello felhasználónevet, jelszót, stb.

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:

    env\scripts\python manage.py runserver

hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.

A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.

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

vagy

    pyvenv env

Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat. A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a>Felügyelő létrehozása
hello alkalmazáshoz tartozó hello adatbázishoz nincs megadva felügyelő. A sorrend toouse hello bejelentkezési funkciójának hello alkalmazás vagy hello a Django felügyeleti felülete (Ha úgy dönt, hogy tooenable azt), toocreate felügyelőt lesz szüksége.

Futtassa a következő parancsot hello parancssori a projektmappa parancssorából:

    env/bin/python manage.py createsuperuser

Hajtsa végre a hello kér tooset hello felhasználónevet, jelszót, stb.

### <a name="run-using-development-server"></a>Futtatás fejlesztési kiszolgálóval
A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:

    env/bin/python manage.py runserver

hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a>Módosítások végrehajtása
Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.

A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a>További csomagok telepítése
Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.

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

## <a name="troubleshooting---static-files"></a>Hibaelhárítás – Statikus fájlok
A Django statikus fájlokat gyűjt hello fogalma rendelkezik. Hello összes statikus fájlt eredeti helyükről fogad, és a tooa egyetlen mappába másolja. Ehhez az alkalmazáshoz, másolja őket túl`/static`.

Ez azért történik, mert a statikus fájlok különböző Django-alkalmazásokból származhatnak. Hello hello Django felügyeleti felületeiről származó statikus fájlok például hello virtuális környezetben a Django-könyvtár almappájában található. A jelen alkalmazásban megadott statikus fájlok itt találhatóak: `/app/static`. Ahogy egyre több Django-alkalmazást használ, egyre több helyen lesznek statikus fájlok.

Hello alkalmazás hibakeresési módban futó, hello alkalmazás hello statikus fájlt eredeti helyükről szolgálja ki.

Hello alkalmazás kiadási módban futó, hello alkalmazás teszi **nem** hello statikus fájlok. Feladata hello hello web server tooserve hello fájlok. Az alkalmazás IIS szolgálja hello származó statikus fájlok `/static`.

statikus fájlok összegyűjtését hello részét hello telepítési parancsfájlt, törölje a korábban összegyűjtött fájlok automatikusan történik. Ez azt jelenti, hogy hello gyűjtemény következik be, minden üzembe helyezés lelassítja kissé a folyamatot, azonban biztosítja, hogy elavult fájlok ne legyen elérhetőek a potenciális biztonsági kockázatot.

Ha tooskip statikus fájlok összegyűjtését a Django-alkalmazáshoz használni szeretne:

    \.skipDjango

Majd be kell toodo hello gyűjtemény manuálisan a helyi számítógépen:

    env\scripts\python manage.py collectstatic

Távolítsa el a hello `\static` mappát `.gitignore` , és adja hozzá toohello Git-tárházba.

## <a name="troubleshooting---settings"></a>Hibaelhárítás – Beállítások
Hello alkalmazás számos beállítása módosítható `DjangoWebProject/settings.py`.

A hibakeresési mód a fejlesztők kényelme érdekében engedélyezve van. Szerencsés módon be fog tudni toosee képek és egyéb statikus tartalmakat futtatásakor helyileg, anélkül, hogy toocollect statikus fájlok.

toodisable hibakeresési mód:

    DEBUG = False

Ha a hibakeresés le van tiltva, a következő hello `ALLOWED_HOSTS` igényeinek toobe frissített tooinclude hello Azure-gazdagép nevét. Példa:

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

vagy tooenable bármely:

    ALLOWED_HOSTS = (
        '*',
    )

A gyakorlatban érdemes lehet toodo valami közötti váltás összetettebb toodeal hibakeresését és a kiadási mód, és az első hello állomásnevet.

Beállíthatja a környezeti változók hello Azure-portálon keresztül **KONFIGURÁLÁSA** lap hello **Alkalmazásbeállítások** szakasz.  Ez lehet hasznos, ha a beállítás értéke lehet, hogy nem szeretné, hogy tooappear a hello forrásokban (kapcsolati karakterláncok, jelszavak stb.), vagy a megjeleníteni kívánt tooset eltérően az Azure és a helyi számítógép között. A `settings.py`, hello környezeti változók használatával kérdezheti `os.getenv`.

## <a name="using-a-database"></a>Adatbázis használata
hello hello alkalmazás részét képező adatbázisa egy sqlite-adatbázis. Ez az egy kényelmes és hasznos alapértelmezett adatbázis toouse fejlesztési, szinte telepítés nem szükséges. hello adatbázis hello hello projektmappa db.sqlite3 fájlt tárolja.

Azure a Django-alkalmazásból könnyen toouse adatbázis-szolgáltatásokat biztosít. Az oktatóanyagok [SQL-adatbázis] és [MySQL] Django-alkalmazásból megjelenítése hello lépéseket szükséges toocreate hello adatbázis-szolgáltatás, hello adatbázis beállításainak módosítását `DjangoWebProject/settings.py`, és hello szalagtárak tooinstall szükséges.

Természetesen Ha toomanage inkább a saját adatbázis-kiszolgálók, megteheti az Azure-on futó Windows vagy Linux rendszerű virtuális gépek használatával.

## <a name="django-admin-interface"></a>A Django felügyeleti felülete
Amikor hozzákezd a modelljei felépítéséhez, érdemes toopopulate hello adatbázist adatokkal. Egyszerűen toodo hozzáadása, és interaktív módon szerkeszthet tartalmakat toouse hello Django felügyeleti felületének.

hello felügyeleti felületének hello kódját a hello alkalmazás forrásokban megjegyzésként szerepel, de azt egyértelműen meg van jelölve, könnyen engedélyezheti azt (keressen rá "admin").

Az engedélyezése után hello adatbázis szinkronizálása, hello alkalmazás futtatásához, és keresse meg a túl`/admin`.

## <a name="next-steps"></a>Következő lépések
Hajtsa végre ezeket a Django és a Python Tools kapcsolatos további hivatkozások toolearn Visual Studio:

* [A Django dokumentációja]
* [Python Tools for Visual Studio dokumentációjában]

Információk az SQL Database és a MySQL használatáról:

* [Django and MySQL on Azure with Python Tools for Visual Studio] (Django és MySQL az Azure-ban, Python Tools for Visual Studio alkalmazással)
* [Django and SQL Database on Azure with Python Tools for Visual Studio] (Django és SQL Database az Azure-ban, Python Tools for Visual Studio alkalmazással)

További információkért lásd: hello [Python fejlesztői központ](/develop/python/).

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

<!--Link references-->
[Django and MySQL on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-django-mysql.md (Django és MySQL az Azure-ban, Python Tools for Visual Studio alkalmazással)
[Django and SQL Database on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-django-sql.md (Django és SQL Database az Azure-ban, Python Tools for Visual Studio alkalmazással)
[SQL-adatbázis]: web-sites-python-ptvs-django-sql.md
[MySQL]: web-sites-python-ptvs-django-mysql.md

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
[A Django dokumentációja]: https://www.djangoproject.com/
