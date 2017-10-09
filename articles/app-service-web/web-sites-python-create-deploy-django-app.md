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
# <a name="creating-web-apps-with-django-in-azure"></a><span data-ttu-id="240fd-103">Webalkalmazások létrehozása a Djangóval az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="240fd-103">Creating web apps with Django in Azure</span></span>
<span data-ttu-id="240fd-104">Ez az oktatóanyag leírja, hogyan tooget el Python futó [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="240fd-104">This tutorial describes how tooget started running Python on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="240fd-105">A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja!</span><span class="sxs-lookup"><span data-stu-id="240fd-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="240fd-106">Az alkalmazás forgalmához igazítható, megváltoztathatja az toopaid üzemeltető, és integrálhatja az összes hello más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="240fd-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="240fd-107">Egy alkalmazás hello Django webes keretrendszer használatával hoz létre (lásd az oktatóanyag alternatív verzióit [Flask](web-sites-python-create-deploy-flask-app.md) és [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="240fd-107">You will create an application using hello Django web framework (see alternate versions of this tutorial for [Flask](web-sites-python-create-deploy-flask-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span> <span data-ttu-id="240fd-108">Hello webalkalmazás létrehoz hello Azure Piactérről származó, beállítása a Git üzemelő példányt, és helyileg hello tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="240fd-108">You will create hello web app from hello Azure Marketplace, set up Git deployment, and clone hello repository locally.</span></span> <span data-ttu-id="240fd-109">Ezután lesz hello alkalmazás helyileg történő futtatása, módosításokat, véglegesítse és küldje le őket tooAzure.</span><span class="sxs-lookup"><span data-stu-id="240fd-109">Then you will run hello application locally, make changes, commit and push them tooAzure.</span></span> <span data-ttu-id="240fd-110">Hogyan hello oktatóanyag azt mutatja be toodo ezt a Windows vagy Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="240fd-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="240fd-111">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="240fd-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="240fd-112">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="240fd-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="240fd-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="240fd-113">Prerequisites</span></span>
* <span data-ttu-id="240fd-114">Windows, Mac vagy Linux</span><span class="sxs-lookup"><span data-stu-id="240fd-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="240fd-115">Python 2.7 vagy 3.4</span><span class="sxs-lookup"><span data-stu-id="240fd-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="240fd-116">setuptools, pip, virtualenv (csak Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="240fd-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="240fd-117">Git</span><span class="sxs-lookup"><span data-stu-id="240fd-117">Git</span></span>
* <span data-ttu-id="240fd-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) – Megjegyzés: szabadon választható</span><span class="sxs-lookup"><span data-stu-id="240fd-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="240fd-119">**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="240fd-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="240fd-120">Windows</span><span class="sxs-lookup"><span data-stu-id="240fd-120">Windows</span></span>
<span data-ttu-id="240fd-121">Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését.</span><span class="sxs-lookup"><span data-stu-id="240fd-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="240fd-122">Ez a Python, setuptools, pip, virtualenv stb (az Azure-gazdagépeken hello telepített Python 32 bites) hello 32 bites verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="240fd-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span> <span data-ttu-id="240fd-123">Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="240fd-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="240fd-124">A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="240fd-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="240fd-125">Ha a Visual Studio használata esetén használhatja hello integrált Git-támogatást.</span><span class="sxs-lookup"><span data-stu-id="240fd-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="240fd-126">Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését.</span><span class="sxs-lookup"><span data-stu-id="240fd-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="240fd-127">Ez nem kötelező, de ha rendelkezik [Visual Studio], beleértve a hello ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat, és ekkor kap egy nagyszerű Python IDE.</span><span class="sxs-lookup"><span data-stu-id="240fd-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="240fd-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="240fd-128">Mac/Linux</span></span>
<span data-ttu-id="240fd-129">A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.</span><span class="sxs-lookup"><span data-stu-id="240fd-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-portal"></a><span data-ttu-id="240fd-130">Webalkalmazás létrehozása a portálon</span><span class="sxs-lookup"><span data-stu-id="240fd-130">Web App Creation on Portal</span></span>
<span data-ttu-id="240fd-131">hello saját alkalmazása létrehozásának első lépése az toocreate hello hello webalkalmazás [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="240fd-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="240fd-132">Jelentkezzen be hello Azure Portal, majd kattintson a hello **új** hello bal alsó sarokban gombjára.</span><span class="sxs-lookup"><span data-stu-id="240fd-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span>
2. <span data-ttu-id="240fd-133">Hello a keresőmezőbe írja be a "python".</span><span class="sxs-lookup"><span data-stu-id="240fd-133">In hello search box, type "python".</span></span>
3. <span data-ttu-id="240fd-134">Hello keresési eredmények között, válassza ki a **Django** (PTVS által közzétett), majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="240fd-134">In hello search results, select **Django** (published by PTVS), then click **Create**.</span></span>
4. <span data-ttu-id="240fd-135">Hello új Django-alkalmazást, például egy új App Service-csomag és egy új erőforráscsoport létrehozása, konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="240fd-135">Configure hello new Django app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="240fd-136">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="240fd-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="240fd-137">Az újonnan létrehozott webalkalmazáshoz tartozó Git-közzététel konfigurálása: hello utasításokat követve [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="240fd-137">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="240fd-138">Az alkalmazás áttekintése</span><span class="sxs-lookup"><span data-stu-id="240fd-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="240fd-139">A Git-tárház tartalma</span><span class="sxs-lookup"><span data-stu-id="240fd-139">Git repository contents</span></span>
<span data-ttu-id="240fd-140">Itt a hello kezdeti Git-tárházban, amely klónozását fogjuk elvégezni hello a következő szakaszban találhat hello fájlok nyújt áttekintést.</span><span class="sxs-lookup"><span data-stu-id="240fd-140">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

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

<span data-ttu-id="240fd-141">Hello alkalmazás fő forrásai.</span><span class="sxs-lookup"><span data-stu-id="240fd-141">Main sources for hello application.</span></span> <span data-ttu-id="240fd-142">3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel.</span><span class="sxs-lookup"><span data-stu-id="240fd-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span> <span data-ttu-id="240fd-143">Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.</span><span class="sxs-lookup"><span data-stu-id="240fd-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \manage.py

<span data-ttu-id="240fd-144">A helyi felügyelet és a fejlesztési kiszolgáló támogatása.</span><span class="sxs-lookup"><span data-stu-id="240fd-144">Local management and development server support.</span></span> <span data-ttu-id="240fd-145">Az toorun hello alkalmazás helyileg használatához, szinkronizálása hello adatbázis stb.</span><span class="sxs-lookup"><span data-stu-id="240fd-145">Use this toorun hello application locally, synchronize hello database, etc.</span></span>

    \db.sqlite3

<span data-ttu-id="240fd-146">Az alapértelmezett adatbázis.</span><span class="sxs-lookup"><span data-stu-id="240fd-146">Default database.</span></span> <span data-ttu-id="240fd-147">Hello alkalmazás toorun hello szükséges táblákat tartalmaz, de nem tartalmazza azokat a felhasználókat (hello adatbázis toocreate a felhasználó szinkronizálása).</span><span class="sxs-lookup"><span data-stu-id="240fd-147">Includes hello necessary tables for hello application toorun, but does not contain any users (synchronize hello database toocreate a user).</span></span>

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

<span data-ttu-id="240fd-148">A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.</span><span class="sxs-lookup"><span data-stu-id="240fd-148">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="240fd-149">IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.</span><span class="sxs-lookup"><span data-stu-id="240fd-149">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="240fd-150">Az alkalmazás számára szükséges külső csomagok.</span><span class="sxs-lookup"><span data-stu-id="240fd-150">External packages needed by this application.</span></span> <span data-ttu-id="240fd-151">hello telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítése hello.</span><span class="sxs-lookup"><span data-stu-id="240fd-151">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="240fd-152">IIS-konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="240fd-152">IIS configuration files.</span></span> <span data-ttu-id="240fd-153">hello telepítési parancsfájl hello megfelelő web.x.y.config használja, és web.config fájlként másolásához.</span><span class="sxs-lookup"><span data-stu-id="240fd-153">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="240fd-154">Opcionális fájlok – A telepítés testre szabása</span><span class="sxs-lookup"><span data-stu-id="240fd-154">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="240fd-155">Opcionális fájlok – Python-futtatókörnyezet</span><span class="sxs-lookup"><span data-stu-id="240fd-155">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="240fd-156">További fájlok a kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="240fd-156">Additional files on server</span></span>
<span data-ttu-id="240fd-157">Egyes fájlok hello kiszolgálón léteznek, de nem kerülnek toohello git-tárház.</span><span class="sxs-lookup"><span data-stu-id="240fd-157">Some files exist on hello server but are not added toohello git repository.</span></span> <span data-ttu-id="240fd-158">Ezek hello telepítési parancsfájl hozza létre.</span><span class="sxs-lookup"><span data-stu-id="240fd-158">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="240fd-159">IIS-konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="240fd-159">IIS configuration file.</span></span> <span data-ttu-id="240fd-160">Minden telepítéshez a web.x.y.config fájlból jön létre.</span><span class="sxs-lookup"><span data-stu-id="240fd-160">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="240fd-161">Python virtuális környezet.</span><span class="sxs-lookup"><span data-stu-id="240fd-161">Python virtual environment.</span></span> <span data-ttu-id="240fd-162">Telepítése során létrehozott Ha kompatibilis virtuális környezet már nem létezik a hello webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="240fd-162">Created during deployment if a compatible virtual environment doesn't already exist on hello web app.</span></span> <span data-ttu-id="240fd-163">A requirements.txt fájlban felsorolt csomagok telepítése a pip, de a pip kihagyja a telepítést, ha már telepítve vannak a hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="240fd-163">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="240fd-164">hello következő 3 szakaszok mutatják be, hogyan tooproceed hello a webalkalmazás-e a fejlesztés alatt 3 különböző környezetekben:</span><span class="sxs-lookup"><span data-stu-id="240fd-164">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="240fd-165">Windows, Python Tools for Visual Studio alkalmazással</span><span class="sxs-lookup"><span data-stu-id="240fd-165">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="240fd-166">Windows, parancssorral</span><span class="sxs-lookup"><span data-stu-id="240fd-166">Windows, with command line</span></span>
* <span data-ttu-id="240fd-167">Mac/Linux, parancssorral</span><span class="sxs-lookup"><span data-stu-id="240fd-167">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="240fd-168">Webalkalmazás-fejlesztés – Windows – Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="240fd-168">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="240fd-169">Hello tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="240fd-169">Clone hello repository</span></span>
<span data-ttu-id="240fd-170">Első lépésben klónozza hello tárházat hello található URL-cím hello Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="240fd-170">First, clone hello repository using hello URL provided on hello Azure Portal.</span></span> <span data-ttu-id="240fd-171">További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="240fd-171">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="240fd-172">Nyissa meg a hello megoldásfájlt (.sln), hello hello tárház gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="240fd-172">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="240fd-173">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="240fd-173">Create virtual environment</span></span>
<span data-ttu-id="240fd-174">Most helyi telepítéshez tartozó virtuális környezetet hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="240fd-174">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="240fd-175">Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="240fd-175">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="240fd-176">Ellenőrizze, hogy hello név hello környezet `env`.</span><span class="sxs-lookup"><span data-stu-id="240fd-176">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="240fd-177">Válassza ki a hello alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="240fd-177">Select hello base interpreter.</span></span> <span data-ttu-id="240fd-178">Győződjön meg arról, hogy toouse hello a webalkalmazás kiválasztott Python ugyanazon verzióját (a runtime.txt vagy hello **Alkalmazásbeállítások** panelen található webalkalmazását az Azure portál hello).</span><span class="sxs-lookup"><span data-stu-id="240fd-178">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="240fd-179">Ellenőrizze, hogy hello beállítás toodownload és telepítési csomagok be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="240fd-179">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="240fd-180">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="240fd-180">Click **Create**.</span></span> <span data-ttu-id="240fd-181">Ezzel hello virtuális környezet létrehozása, és telepítse a Requirements.txt fájlban található függőségek.</span><span class="sxs-lookup"><span data-stu-id="240fd-181">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="create-a-superuser"></a><span data-ttu-id="240fd-182">Felügyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="240fd-182">Create a superuser</span></span>
<span data-ttu-id="240fd-183">hello alkalmazáshoz tartozó hello adatbázishoz nincs megadva felügyelő.</span><span class="sxs-lookup"><span data-stu-id="240fd-183">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="240fd-184">A sorrend toouse hello bejelentkezési funkciójának hello alkalmazás vagy hello a Django felügyeleti felülete (Ha úgy dönt, hogy tooenable azt), toocreate felügyelőt lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="240fd-184">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="240fd-185">Futtassa a következő parancsot hello parancssori a projektmappa parancssorából:</span><span class="sxs-lookup"><span data-stu-id="240fd-185">Run this from hello command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="240fd-186">Hajtsa végre a hello kér tooset hello felhasználónevet, jelszót, stb.</span><span class="sxs-lookup"><span data-stu-id="240fd-186">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="240fd-187">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="240fd-187">Run using development server</span></span>
<span data-ttu-id="240fd-188">Nyomja le az F5 toostart Hibakeresés és a webböngészőjében automatikusan toohello helyileg futtatott lap ekkor megnyílik.</span><span class="sxs-lookup"><span data-stu-id="240fd-188">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

<span data-ttu-id="240fd-189">Töréspontokat állíthat hello adatforrások, tekintse meg a windows hello, stb. Lásd: hello [Python Tools for Visual Studio dokumentációjában] hello további információt a különböző szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="240fd-189">You can set breakpoints in hello sources, use hello watch windows, etc. See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="240fd-190">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="240fd-190">Make changes</span></span>
<span data-ttu-id="240fd-191">Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.</span><span class="sxs-lookup"><span data-stu-id="240fd-191">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="240fd-192">A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="240fd-192">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a><span data-ttu-id="240fd-193">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-193">Install more packages</span></span>
<span data-ttu-id="240fd-194">Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="240fd-194">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="240fd-195">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="240fd-195">You can install additional packages using pip.</span></span> <span data-ttu-id="240fd-196">tooinstall egy csomagot, kattintson a jobb gombbal a hello virtuális környezetet, és válassza **Python-csomag telepítése**.</span><span class="sxs-lookup"><span data-stu-id="240fd-196">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="240fd-197">Például tooinstall hello hozzáférést tud biztosítani tooAzure storage, service bus és további Azure-szolgáltatásokhoz, írja be Pythonhoz készült Azure SDK `azure`:</span><span class="sxs-lookup"><span data-stu-id="240fd-197">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

<span data-ttu-id="240fd-198">Kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **készítése a requirements.txt** tooupdate Requirements.txt fájlt.</span><span class="sxs-lookup"><span data-stu-id="240fd-198">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="240fd-199">Ezt követően véglegesítse hello módosítások toorequirements.txt toohello Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="240fd-199">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="240fd-200">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-200">Deploy tooAzure</span></span>
<span data-ttu-id="240fd-201">tootrigger a telepítést, kattintson a **szinkronizálási** vagy **leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="240fd-201">tootrigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="240fd-202">A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="240fd-202">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

<span data-ttu-id="240fd-203">hello első központi telepítési időt vesz igénybe, mivel ekkor létrehoz egy virtuális környezethez, a csomagok telepítése, stb.</span><span class="sxs-lookup"><span data-stu-id="240fd-203">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="240fd-204">A Visual Studio nem jeleníti meg hello hello központi telepítés végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="240fd-204">Visual Studio doesn't show hello progress of hello deployment.</span></span> <span data-ttu-id="240fd-205">Ha szeretné tooreview hello kimeneti, ld. hello [hibaelhárítás – üzembe helyezés](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="240fd-205">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="240fd-206">Keresse meg a toohello Azure URL-cím tooview a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="240fd-206">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="240fd-207">Webalkalmazás-fejlesztés – Windows – parancssor</span><span class="sxs-lookup"><span data-stu-id="240fd-207">Web app development - Windows - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="240fd-208">Hello tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="240fd-208">Clone hello repository</span></span>
<span data-ttu-id="240fd-209">Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="240fd-209">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="240fd-210">További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="240fd-210">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="240fd-211">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="240fd-211">Create virtual environment</span></span>
<span data-ttu-id="240fd-212">Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház).</span><span class="sxs-lookup"><span data-stu-id="240fd-212">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="240fd-213">Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.</span><span class="sxs-lookup"><span data-stu-id="240fd-213">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="240fd-214">Győződjön meg arról, hogy toouse hello (a runtime.txt vagy hello alkalmazás beállítások panel webalkalmazását az Azure portál hello) a webalkalmazás kiválasztott Python ugyanazon verzióját.</span><span class="sxs-lookup"><span data-stu-id="240fd-214">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="240fd-215">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="240fd-215">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="240fd-216">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="240fd-216">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="240fd-217">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="240fd-217">Install any external packages required by your application.</span></span> <span data-ttu-id="240fd-218">A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:</span><span class="sxs-lookup"><span data-stu-id="240fd-218">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="240fd-219">Felügyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="240fd-219">Create a superuser</span></span>
<span data-ttu-id="240fd-220">hello alkalmazáshoz tartozó hello adatbázishoz nincs megadva felügyelő.</span><span class="sxs-lookup"><span data-stu-id="240fd-220">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="240fd-221">A sorrend toouse hello bejelentkezési funkciójának hello alkalmazás vagy hello a Django felügyeleti felülete (Ha úgy dönt, hogy tooenable azt), toocreate felügyelőt lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="240fd-221">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="240fd-222">Futtassa a következő parancsot hello parancssori a projektmappa parancssorából:</span><span class="sxs-lookup"><span data-stu-id="240fd-222">Run this from hello command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="240fd-223">Hajtsa végre a hello kér tooset hello felhasználónevet, jelszót, stb.</span><span class="sxs-lookup"><span data-stu-id="240fd-223">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="240fd-224">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="240fd-224">Run using development server</span></span>
<span data-ttu-id="240fd-225">A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="240fd-225">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python manage.py runserver

<span data-ttu-id="240fd-226">hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="240fd-226">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

<span data-ttu-id="240fd-227">Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="240fd-227">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="240fd-228">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="240fd-228">Make changes</span></span>
<span data-ttu-id="240fd-229">Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.</span><span class="sxs-lookup"><span data-stu-id="240fd-229">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="240fd-230">A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="240fd-230">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="240fd-231">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-231">Install more packages</span></span>
<span data-ttu-id="240fd-232">Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="240fd-232">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="240fd-233">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="240fd-233">You can install additional packages using pip.</span></span> <span data-ttu-id="240fd-234">Például tooinstall hello Azure SDK for Python, amely lehetővé teszi az elérhető tooAzure storage, service bus és további Azure-szolgáltatásokhoz, típus:</span><span class="sxs-lookup"><span data-stu-id="240fd-234">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="240fd-235">Győződjön meg arról, hogy tooupdate Requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="240fd-235">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="240fd-236">Hello változtatások véglegesítése a határidő:</span><span class="sxs-lookup"><span data-stu-id="240fd-236">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="240fd-237">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-237">Deploy tooAzure</span></span>
<span data-ttu-id="240fd-238">a központi telepítés tootrigger, leküldéses hello tooAzure módosítja:</span><span class="sxs-lookup"><span data-stu-id="240fd-238">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="240fd-239">Hello a telepítési parancsfájl, beleértve a virtuális környezet létrehozását, a csomagok telepítését és a Web.config fájl létrehozását hello kimenetet fog látni.</span><span class="sxs-lookup"><span data-stu-id="240fd-239">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="240fd-240">Keresse meg a toohello Azure URL-cím tooview a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="240fd-240">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="240fd-241">Webalkalmazás-fejlesztés – Mac/Linux – parancssor</span><span class="sxs-lookup"><span data-stu-id="240fd-241">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="240fd-242">Hello tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="240fd-242">Clone hello repository</span></span>
<span data-ttu-id="240fd-243">Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="240fd-243">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="240fd-244">További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="240fd-244">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="240fd-245">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="240fd-245">Create virtual environment</span></span>
<span data-ttu-id="240fd-246">Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház).</span><span class="sxs-lookup"><span data-stu-id="240fd-246">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span> <span data-ttu-id="240fd-247">Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.</span><span class="sxs-lookup"><span data-stu-id="240fd-247">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="240fd-248">Győződjön meg arról, hogy toouse hello (a runtime.txt vagy hello alkalmazás beállítások panel webalkalmazását az Azure portál hello) a webalkalmazás kiválasztott Python ugyanazon verzióját.</span><span class="sxs-lookup"><span data-stu-id="240fd-248">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello Application Settings blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="240fd-249">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="240fd-249">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="240fd-250">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="240fd-250">For Python 3.4:</span></span>

    python -m venv env

<span data-ttu-id="240fd-251">vagy</span><span class="sxs-lookup"><span data-stu-id="240fd-251">or</span></span>

    pyvenv env

<span data-ttu-id="240fd-252">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="240fd-252">Install any external packages required by your application.</span></span> <span data-ttu-id="240fd-253">A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:</span><span class="sxs-lookup"><span data-stu-id="240fd-253">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="240fd-254">Felügyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="240fd-254">Create a superuser</span></span>
<span data-ttu-id="240fd-255">hello alkalmazáshoz tartozó hello adatbázishoz nincs megadva felügyelő.</span><span class="sxs-lookup"><span data-stu-id="240fd-255">hello database included with hello application does not have any superuser defined.</span></span> <span data-ttu-id="240fd-256">A sorrend toouse hello bejelentkezési funkciójának hello alkalmazás vagy hello a Django felügyeleti felülete (Ha úgy dönt, hogy tooenable azt), toocreate felügyelőt lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="240fd-256">In order toouse hello sign-in functionality in hello application, or hello Django admin interface (if you decide tooenable it), you'll need toocreate a superuser.</span></span>

<span data-ttu-id="240fd-257">Futtassa a következő parancsot hello parancssori a projektmappa parancssorából:</span><span class="sxs-lookup"><span data-stu-id="240fd-257">Run this from hello command-line from your project folder:</span></span>

    env/bin/python manage.py createsuperuser

<span data-ttu-id="240fd-258">Hajtsa végre a hello kér tooset hello felhasználónevet, jelszót, stb.</span><span class="sxs-lookup"><span data-stu-id="240fd-258">Follow hello prompts tooset hello user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="240fd-259">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="240fd-259">Run using development server</span></span>
<span data-ttu-id="240fd-260">A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="240fd-260">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python manage.py runserver

<span data-ttu-id="240fd-261">hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="240fd-261">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

<span data-ttu-id="240fd-262">Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="240fd-262">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="240fd-263">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="240fd-263">Make changes</span></span>
<span data-ttu-id="240fd-264">Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.</span><span class="sxs-lookup"><span data-stu-id="240fd-264">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="240fd-265">A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="240fd-265">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="240fd-266">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-266">Install more packages</span></span>
<span data-ttu-id="240fd-267">Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="240fd-267">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="240fd-268">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="240fd-268">You can install additional packages using pip.</span></span> <span data-ttu-id="240fd-269">Például tooinstall hello Azure SDK for Python, amely lehetővé teszi az elérhető tooAzure storage, service bus és további Azure-szolgáltatásokhoz, típus:</span><span class="sxs-lookup"><span data-stu-id="240fd-269">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="240fd-270">Győződjön meg arról, hogy tooupdate Requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="240fd-270">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="240fd-271">Hello változtatások véglegesítése a határidő:</span><span class="sxs-lookup"><span data-stu-id="240fd-271">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="240fd-272">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-272">Deploy tooAzure</span></span>
<span data-ttu-id="240fd-273">a központi telepítés tootrigger, leküldéses hello tooAzure módosítja:</span><span class="sxs-lookup"><span data-stu-id="240fd-273">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="240fd-274">Hello a telepítési parancsfájl, beleértve a virtuális környezet létrehozását, a csomagok telepítését és a Web.config fájl létrehozását hello kimenetet fog látni.</span><span class="sxs-lookup"><span data-stu-id="240fd-274">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="240fd-275">Keresse meg a toohello Azure URL-cím tooview a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="240fd-275">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="240fd-276">Hibaelhárítás – Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="240fd-276">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="240fd-277">Hibaelhárítás – Virtuális környezet</span><span class="sxs-lookup"><span data-stu-id="240fd-277">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a><span data-ttu-id="240fd-278">Hibaelhárítás – Statikus fájlok</span><span class="sxs-lookup"><span data-stu-id="240fd-278">Troubleshooting - Static Files</span></span>
<span data-ttu-id="240fd-279">A Django statikus fájlokat gyűjt hello fogalma rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="240fd-279">Django has hello concept of collecting static files.</span></span> <span data-ttu-id="240fd-280">Hello összes statikus fájlt eredeti helyükről fogad, és a tooa egyetlen mappába másolja.</span><span class="sxs-lookup"><span data-stu-id="240fd-280">This takes all hello static files from their original location and copies them tooa single folder.</span></span> <span data-ttu-id="240fd-281">Ehhez az alkalmazáshoz, másolja őket túl`/static`.</span><span class="sxs-lookup"><span data-stu-id="240fd-281">For this application, they are copied too`/static`.</span></span>

<span data-ttu-id="240fd-282">Ez azért történik, mert a statikus fájlok különböző Django-alkalmazásokból származhatnak.</span><span class="sxs-lookup"><span data-stu-id="240fd-282">This is done because static files may come from different Django 'apps'.</span></span> <span data-ttu-id="240fd-283">Hello hello Django felügyeleti felületeiről származó statikus fájlok például hello virtuális környezetben a Django-könyvtár almappájában található.</span><span class="sxs-lookup"><span data-stu-id="240fd-283">For example, hello static files from hello Django admin interfaces are located in a Django library subfolder in hello virtual environment.</span></span> <span data-ttu-id="240fd-284">A jelen alkalmazásban megadott statikus fájlok itt találhatóak: `/app/static`.</span><span class="sxs-lookup"><span data-stu-id="240fd-284">Static files defined by this application are located in `/app/static`.</span></span> <span data-ttu-id="240fd-285">Ahogy egyre több Django-alkalmazást használ, egyre több helyen lesznek statikus fájlok.</span><span class="sxs-lookup"><span data-stu-id="240fd-285">As you use more Django 'apps', you'll have static files located in multiple places.</span></span>

<span data-ttu-id="240fd-286">Hello alkalmazás hibakeresési módban futó, hello alkalmazás hello statikus fájlt eredeti helyükről szolgálja ki.</span><span class="sxs-lookup"><span data-stu-id="240fd-286">When running hello application in debug mode, hello application serves hello static files from their original location.</span></span>

<span data-ttu-id="240fd-287">Hello alkalmazás kiadási módban futó, hello alkalmazás teszi **nem** hello statikus fájlok.</span><span class="sxs-lookup"><span data-stu-id="240fd-287">When running hello application in release mode, hello application does **not** serve hello static files.</span></span> <span data-ttu-id="240fd-288">Feladata hello hello web server tooserve hello fájlok.</span><span class="sxs-lookup"><span data-stu-id="240fd-288">It is hello responsibility of hello web server tooserve hello files.</span></span> <span data-ttu-id="240fd-289">Az alkalmazás IIS szolgálja hello származó statikus fájlok `/static`.</span><span class="sxs-lookup"><span data-stu-id="240fd-289">For this application, IIS will serve hello static files from `/static`.</span></span>

<span data-ttu-id="240fd-290">statikus fájlok összegyűjtését hello részét hello telepítési parancsfájlt, törölje a korábban összegyűjtött fájlok automatikusan történik.</span><span class="sxs-lookup"><span data-stu-id="240fd-290">hello collection of static files is done automatically as part of hello deployment script, clearing previously collected files.</span></span> <span data-ttu-id="240fd-291">Ez azt jelenti, hogy hello gyűjtemény következik be, minden üzembe helyezés lelassítja kissé a folyamatot, azonban biztosítja, hogy elavult fájlok ne legyen elérhetőek a potenciális biztonsági kockázatot.</span><span class="sxs-lookup"><span data-stu-id="240fd-291">This means hello collection occurs on every deployment, slowing down deployment a bit, but it ensures that obsolete files won't be available, avoiding a potential security issue.</span></span>

<span data-ttu-id="240fd-292">Ha tooskip statikus fájlok összegyűjtését a Django-alkalmazáshoz használni szeretne:</span><span class="sxs-lookup"><span data-stu-id="240fd-292">If you want tooskip collection of static files for your Django application:</span></span>

    \.skipDjango

<span data-ttu-id="240fd-293">Majd be kell toodo hello gyűjtemény manuálisan a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="240fd-293">Then you'll need toodo hello collection manually on your local machine:</span></span>

    env\scripts\python manage.py collectstatic

<span data-ttu-id="240fd-294">Távolítsa el a hello `\static` mappát `.gitignore` , és adja hozzá toohello Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="240fd-294">Then remove hello `\static` folder from `.gitignore` and add it toohello Git repository.</span></span>

## <a name="troubleshooting---settings"></a><span data-ttu-id="240fd-295">Hibaelhárítás – Beállítások</span><span class="sxs-lookup"><span data-stu-id="240fd-295">Troubleshooting - Settings</span></span>
<span data-ttu-id="240fd-296">Hello alkalmazás számos beállítása módosítható `DjangoWebProject/settings.py`.</span><span class="sxs-lookup"><span data-stu-id="240fd-296">Various settings for hello application can be changed in `DjangoWebProject/settings.py`.</span></span>

<span data-ttu-id="240fd-297">A hibakeresési mód a fejlesztők kényelme érdekében engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="240fd-297">For developer convenience, debug mode is enabled.</span></span> <span data-ttu-id="240fd-298">Szerencsés módon be fog tudni toosee képek és egyéb statikus tartalmakat futtatásakor helyileg, anélkül, hogy toocollect statikus fájlok.</span><span class="sxs-lookup"><span data-stu-id="240fd-298">One nice side effect of that is you'll be able toosee images and other static content when running locally, without having toocollect static files.</span></span>

<span data-ttu-id="240fd-299">toodisable hibakeresési mód:</span><span class="sxs-lookup"><span data-stu-id="240fd-299">toodisable debug mode:</span></span>

    DEBUG = False

<span data-ttu-id="240fd-300">Ha a hibakeresés le van tiltva, a következő hello `ALLOWED_HOSTS` igényeinek toobe frissített tooinclude hello Azure-gazdagép nevét.</span><span class="sxs-lookup"><span data-stu-id="240fd-300">When debug is disabled, hello value for `ALLOWED_HOSTS` needs toobe updated tooinclude hello Azure host name.</span></span> <span data-ttu-id="240fd-301">Példa:</span><span class="sxs-lookup"><span data-stu-id="240fd-301">For example:</span></span>

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

<span data-ttu-id="240fd-302">vagy tooenable bármely:</span><span class="sxs-lookup"><span data-stu-id="240fd-302">or tooenable any:</span></span>

    ALLOWED_HOSTS = (
        '*',
    )

<span data-ttu-id="240fd-303">A gyakorlatban érdemes lehet toodo valami közötti váltás összetettebb toodeal hibakeresését és a kiadási mód, és az első hello állomásnevet.</span><span class="sxs-lookup"><span data-stu-id="240fd-303">In practice, you may want toodo something more complex toodeal with switching between debug and release mode, and getting hello host name.</span></span>

<span data-ttu-id="240fd-304">Beállíthatja a környezeti változók hello Azure-portálon keresztül **KONFIGURÁLÁSA** lap hello **Alkalmazásbeállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="240fd-304">You can set environment variables through hello Azure portal **CONFIGURE** page, in hello **app settings** section.</span></span>  <span data-ttu-id="240fd-305">Ez lehet hasznos, ha a beállítás értéke lehet, hogy nem szeretné, hogy tooappear a hello forrásokban (kapcsolati karakterláncok, jelszavak stb.), vagy a megjeleníteni kívánt tooset eltérően az Azure és a helyi számítógép között.</span><span class="sxs-lookup"><span data-stu-id="240fd-305">This can be useful for setting values that you may not want tooappear in hello sources (connection strings, passwords, etc), or that you want tooset differently between Azure and your local machine.</span></span> <span data-ttu-id="240fd-306">A `settings.py`, hello környezeti változók használatával kérdezheti `os.getenv`.</span><span class="sxs-lookup"><span data-stu-id="240fd-306">In `settings.py`, you can query hello environment variables using `os.getenv`.</span></span>

## <a name="using-a-database"></a><span data-ttu-id="240fd-307">Adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="240fd-307">Using a Database</span></span>
<span data-ttu-id="240fd-308">hello hello alkalmazás részét képező adatbázisa egy sqlite-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="240fd-308">hello database that is included with hello application is a sqlite database.</span></span> <span data-ttu-id="240fd-309">Ez az egy kényelmes és hasznos alapértelmezett adatbázis toouse fejlesztési, szinte telepítés nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="240fd-309">This is a convenient and useful default database toouse for development, as it requires almost no setup.</span></span> <span data-ttu-id="240fd-310">hello adatbázis hello hello projektmappa db.sqlite3 fájlt tárolja.</span><span class="sxs-lookup"><span data-stu-id="240fd-310">hello database is stored in hello db.sqlite3 file in hello project folder.</span></span>

<span data-ttu-id="240fd-311">Azure a Django-alkalmazásból könnyen toouse adatbázis-szolgáltatásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="240fd-311">Azure provides database services which are easy toouse from a Django application.</span></span> <span data-ttu-id="240fd-312">Az oktatóanyagok [SQL-adatbázis] és [MySQL] Django-alkalmazásból megjelenítése hello lépéseket szükséges toocreate hello adatbázis-szolgáltatás, hello adatbázis beállításainak módosítását `DjangoWebProject/settings.py`, és hello szalagtárak tooinstall szükséges.</span><span class="sxs-lookup"><span data-stu-id="240fd-312">Tutorials for using [SQL Database] and [MySQL] from a Django application show hello steps necessary toocreate hello database service, change hello database settings in `DjangoWebProject/settings.py`, and hello libraries required tooinstall.</span></span>

<span data-ttu-id="240fd-313">Természetesen Ha toomanage inkább a saját adatbázis-kiszolgálók, megteheti az Azure-on futó Windows vagy Linux rendszerű virtuális gépek használatával.</span><span class="sxs-lookup"><span data-stu-id="240fd-313">Of course, if you prefer toomanage your own database servers, you can do so using Windows or Linux virtual machines running on Azure.</span></span>

## <a name="django-admin-interface"></a><span data-ttu-id="240fd-314">A Django felügyeleti felülete</span><span class="sxs-lookup"><span data-stu-id="240fd-314">Django Admin Interface</span></span>
<span data-ttu-id="240fd-315">Amikor hozzákezd a modelljei felépítéséhez, érdemes toopopulate hello adatbázist adatokkal.</span><span class="sxs-lookup"><span data-stu-id="240fd-315">Once you start building your models, you'll want toopopulate hello database with some data.</span></span> <span data-ttu-id="240fd-316">Egyszerűen toodo hozzáadása, és interaktív módon szerkeszthet tartalmakat toouse hello Django felügyeleti felületének.</span><span class="sxs-lookup"><span data-stu-id="240fd-316">An easy way toodo add and edit content interactively is toouse hello Django administration interface.</span></span>

<span data-ttu-id="240fd-317">hello felügyeleti felületének hello kódját a hello alkalmazás forrásokban megjegyzésként szerepel, de azt egyértelműen meg van jelölve, könnyen engedélyezheti azt (keressen rá "admin").</span><span class="sxs-lookup"><span data-stu-id="240fd-317">hello code for hello admin interface is commented out in hello application sources, but it's clearly marked so you can easily enable it (search for 'admin').</span></span>

<span data-ttu-id="240fd-318">Az engedélyezése után hello adatbázis szinkronizálása, hello alkalmazás futtatásához, és keresse meg a túl`/admin`.</span><span class="sxs-lookup"><span data-stu-id="240fd-318">After it's enabled, synchronize hello database, run hello application and navigate too`/admin`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="240fd-319">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="240fd-319">Next Steps</span></span>
<span data-ttu-id="240fd-320">Hajtsa végre ezeket a Django és a Python Tools kapcsolatos további hivatkozások toolearn Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="240fd-320">Follow these links toolearn more about Django and Python Tools for Visual Studio:</span></span>

* <span data-ttu-id="240fd-321">[A Django dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="240fd-321">[Django Documentation]</span></span>
* <span data-ttu-id="240fd-322">[Python Tools for Visual Studio dokumentációjában]</span><span class="sxs-lookup"><span data-stu-id="240fd-322">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="240fd-323">Információk az SQL Database és a MySQL használatáról:</span><span class="sxs-lookup"><span data-stu-id="240fd-323">For information on using SQL Database and MySQL:</span></span>

* <span data-ttu-id="240fd-324">[Django and MySQL on Azure with Python Tools for Visual Studio] (Django és MySQL az Azure-ban, Python Tools for Visual Studio alkalmazással)</span><span class="sxs-lookup"><span data-stu-id="240fd-324">[Django and MySQL on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="240fd-325">[Django and SQL Database on Azure with Python Tools for Visual Studio] (Django és SQL Database az Azure-ban, Python Tools for Visual Studio alkalmazással)</span><span class="sxs-lookup"><span data-stu-id="240fd-325">[Django and SQL Database on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="240fd-326">További információkért lásd: hello [Python fejlesztői központ](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="240fd-326">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="240fd-327">A változások</span><span class="sxs-lookup"><span data-stu-id="240fd-327">What's changed</span></span>
* <span data-ttu-id="240fd-328">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="240fd-328">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
