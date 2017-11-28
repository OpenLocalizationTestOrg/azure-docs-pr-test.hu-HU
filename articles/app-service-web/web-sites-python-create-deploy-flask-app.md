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
# <a name="creating-web-apps-with-flask-in-azure"></a><span data-ttu-id="918a1-103">Webalkalmazások létrehozása a Flask az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="918a1-103">Creating web apps with Flask in Azure</span></span>
<span data-ttu-id="918a1-104">Ez az oktatóanyag Python futtatásának első lépéseit ismerteti [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="918a1-104">This tutorial describes how to get started running Python in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>  <span data-ttu-id="918a1-105">A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja!</span><span class="sxs-lookup"><span data-stu-id="918a1-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span>  <span data-ttu-id="918a1-106">Az alkalmazása növekedésével átválthat fizetős üzemeltetésre, valamint integrálhatja alkalmazásába az összes többi Azure-szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="918a1-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="918a1-107">Egy alkalmazás, a Flask webes keretrendszer használatával hoz létre (lásd az oktatóanyag alternatív verzióit [Django](web-sites-python-create-deploy-django-app.md) és [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="918a1-107">You will create an application using the Flask web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span>  <span data-ttu-id="918a1-108">Lesz a webhely létrehozása az Azure katalógusában, Git-telepítésének beállítása és klónozza a tárházat helyileg.</span><span class="sxs-lookup"><span data-stu-id="918a1-108">You will create the website from the Azure gallery, set up Git deployment, and clone the repository locally.</span></span>  <span data-ttu-id="918a1-109">Ezt követően helyileg fogja futtatni az alkalmazást, módosításokat fog elvégezni rajta, majd véglegesíteni fogja őket, és el fogja küldeni az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="918a1-109">Then you will run the application locally, make changes, commit and push them to Azure.</span></span>  <span data-ttu-id="918a1-110">Ez az oktatóanyag ennek a folyamatnak a Windows vagy Mac/Linux operációs rendszeren történő végrehajtását mutatja be.</span><span class="sxs-lookup"><span data-stu-id="918a1-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="918a1-111">Ha nem szeretne regisztrálni Azure-fiókot az Azure App Service megismerése előtt, lépjen [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra, ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="918a1-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="918a1-112">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="918a1-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="918a1-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="918a1-113">Prerequisites</span></span>
* <span data-ttu-id="918a1-114">Windows, Mac vagy Linux</span><span class="sxs-lookup"><span data-stu-id="918a1-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="918a1-115">Python 2.7 vagy 3.4</span><span class="sxs-lookup"><span data-stu-id="918a1-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="918a1-116">setuptools, pip, virtualenv (csak Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="918a1-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="918a1-117">Git</span><span class="sxs-lookup"><span data-stu-id="918a1-117">Git</span></span>
* <span data-ttu-id="918a1-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) – Megjegyzés: szabadon választható</span><span class="sxs-lookup"><span data-stu-id="918a1-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="918a1-119">**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="918a1-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="918a1-120">Windows</span><span class="sxs-lookup"><span data-stu-id="918a1-120">Windows</span></span>
<span data-ttu-id="918a1-121">Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését.</span><span class="sxs-lookup"><span data-stu-id="918a1-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span>  <span data-ttu-id="918a1-122">Ezzel többek között a következőket telepíti: a Python 32 bites verziója, setuptools, pip, virtualenv (a Python 32 bites verziója van telepítve az Azure-gazdagépeken).</span><span class="sxs-lookup"><span data-stu-id="918a1-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span>  <span data-ttu-id="918a1-123">Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="918a1-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="918a1-124">A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="918a1-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span>  <span data-ttu-id="918a1-125">Visual Studio használata esetén használhatja a beépített Git-támogatást.</span><span class="sxs-lookup"><span data-stu-id="918a1-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="918a1-126">Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését.</span><span class="sxs-lookup"><span data-stu-id="918a1-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span>  <span data-ttu-id="918a1-127">Ez ugyan nem kötelező, de ha rendelkezik az ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat is magában foglaló [Visual Studio] szoftverrel, a nagyszerű Python IDE előnyeit is élvezheti.</span><span class="sxs-lookup"><span data-stu-id="918a1-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="918a1-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="918a1-128">Mac/Linux</span></span>
<span data-ttu-id="918a1-129">A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.</span><span class="sxs-lookup"><span data-stu-id="918a1-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-create-on-the-azure-portal"></a><span data-ttu-id="918a1-130">A webalkalmazás létrehozása az Azure-portál</span><span class="sxs-lookup"><span data-stu-id="918a1-130">Web app create on the Azure Portal</span></span>
<span data-ttu-id="918a1-131">Saját alkalmazása létrehozásának első lépése egy webalkalmazás létrehozása az [Azure portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="918a1-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span> 

1. <span data-ttu-id="918a1-132">Jelentkezzen be az Azure portálra, majd kattintson a bal alsó sarokban található **NEW** (ÚJ) gombra.</span><span class="sxs-lookup"><span data-stu-id="918a1-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span> 
2. <span data-ttu-id="918a1-133">Kattintson a **Web + mobil** elemre.</span><span class="sxs-lookup"><span data-stu-id="918a1-133">Click **Web + Mobile**.</span></span>
3. <span data-ttu-id="918a1-134">A keresőmezőbe írja be a „python” kifejezést.</span><span class="sxs-lookup"><span data-stu-id="918a1-134">In the search box, type "python".</span></span>
4. <span data-ttu-id="918a1-135">A keresési eredmények között, válassza ki a **Flask**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="918a1-135">In the search results, select **Flask**, then click **Create**.</span></span>
5. <span data-ttu-id="918a1-136">Az új Flask alkalmazás, például egy új App Service-csomag és egy új erőforráscsoport létrehozásával az konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="918a1-136">Configure the new Flask app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="918a1-137">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="918a1-137">Then, click **Create**.</span></span>
6. <span data-ttu-id="918a1-138">Konfigurálja az újonnan létrehozott webalkalmazáshoz tartozó Git-közzétételt a [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben) részben megadott utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="918a1-138">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="918a1-139">Az alkalmazás áttekintése</span><span class="sxs-lookup"><span data-stu-id="918a1-139">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="918a1-140">A Git-tárház tartalma</span><span class="sxs-lookup"><span data-stu-id="918a1-140">Git repository contents</span></span>
<span data-ttu-id="918a1-141">Az alábbiakban áttekintjük a kiindulási Git-tárházban található fájlokat. A következő szakaszban ezek klónozását fogjuk elvégezni.</span><span class="sxs-lookup"><span data-stu-id="918a1-141">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

<span data-ttu-id="918a1-142">Az alkalmazás fő forrásai.</span><span class="sxs-lookup"><span data-stu-id="918a1-142">Main sources for the application.</span></span>  <span data-ttu-id="918a1-143">3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel.</span><span class="sxs-lookup"><span data-stu-id="918a1-143">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="918a1-144">Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.</span><span class="sxs-lookup"><span data-stu-id="918a1-144">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \runserver.py

<span data-ttu-id="918a1-145">Helyi fejlesztési kiszolgáló támogatása.</span><span class="sxs-lookup"><span data-stu-id="918a1-145">Local development server support.</span></span> <span data-ttu-id="918a1-146">Ezzel az alkalmazás helyileg történő futtatása.</span><span class="sxs-lookup"><span data-stu-id="918a1-146">Use this to run the application locally.</span></span>

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

<span data-ttu-id="918a1-147">A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.</span><span class="sxs-lookup"><span data-stu-id="918a1-147">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="918a1-148">IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.</span><span class="sxs-lookup"><span data-stu-id="918a1-148">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="918a1-149">Az alkalmazás számára szükséges külső csomagok.</span><span class="sxs-lookup"><span data-stu-id="918a1-149">External packages needed by this application.</span></span> <span data-ttu-id="918a1-150">A telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítését.</span><span class="sxs-lookup"><span data-stu-id="918a1-150">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="918a1-151">IIS-konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="918a1-151">IIS configuration files.</span></span>  <span data-ttu-id="918a1-152">A telepítési parancsfájl a megfelelő web.x.y.config fájlt fogja használni, és web.config fájlként fogja azt másolni.</span><span class="sxs-lookup"><span data-stu-id="918a1-152">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="918a1-153">Opcionális fájlok – A telepítés testre szabása</span><span class="sxs-lookup"><span data-stu-id="918a1-153">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="918a1-154">Opcionális fájlok – Python-futtatókörnyezet</span><span class="sxs-lookup"><span data-stu-id="918a1-154">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="918a1-155">További fájlok a kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="918a1-155">Additional files on server</span></span>
<span data-ttu-id="918a1-156">Egyes fájlok megtalálhatóak ugyan a kiszolgálón, de nem lettek felvéve a Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="918a1-156">Some files exist on the server but are not added to the git repository.</span></span>  <span data-ttu-id="918a1-157">Ezeket a telepítési parancsfájl hozza létre.</span><span class="sxs-lookup"><span data-stu-id="918a1-157">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="918a1-158">IIS-konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="918a1-158">IIS configuration file.</span></span>  <span data-ttu-id="918a1-159">Minden telepítéshez a web.x.y.config fájlból jön létre.</span><span class="sxs-lookup"><span data-stu-id="918a1-159">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="918a1-160">Python virtuális környezet.</span><span class="sxs-lookup"><span data-stu-id="918a1-160">Python virtual environment.</span></span>  <span data-ttu-id="918a1-161">Ha egy kompatibilis virtuális környezet már nem létezik az alkalmazás létre üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="918a1-161">Created during deployment if a compatible virtual environment doesn't already exist on the app.</span></span>  <span data-ttu-id="918a1-162">Megtörténik a requirements.txt fájlban felsorolt csomagok telepítése, a korábban már telepített csomagokat azonban a pip már nem telepíti újra.</span><span class="sxs-lookup"><span data-stu-id="918a1-162">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="918a1-163">A következő három szakaszban a webalkalmazások fejlesztéséről talál információkat, három különböző környezetben:</span><span class="sxs-lookup"><span data-stu-id="918a1-163">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="918a1-164">Windows, Python Tools for Visual Studio alkalmazással</span><span class="sxs-lookup"><span data-stu-id="918a1-164">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="918a1-165">Windows, parancssorral</span><span class="sxs-lookup"><span data-stu-id="918a1-165">Windows, with command line</span></span>
* <span data-ttu-id="918a1-166">Mac/Linux, parancssorral</span><span class="sxs-lookup"><span data-stu-id="918a1-166">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="918a1-167">Webalkalmazás-fejlesztés – Windows – Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="918a1-167">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="918a1-168">A tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="918a1-168">Clone the repository</span></span>
<span data-ttu-id="918a1-169">Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="918a1-169">First, clone the repository using the URL provided on the Azure Portal.</span></span> <span data-ttu-id="918a1-170">További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="918a1-170">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="918a1-171">Nyissa meg a tárház gyökérkönyvtárában található megoldásfájlt (.sln).</span><span class="sxs-lookup"><span data-stu-id="918a1-171">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="918a1-172">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="918a1-172">Create virtual environment</span></span>
<span data-ttu-id="918a1-173">Most helyi telepítéshez tartozó virtuális környezetet hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="918a1-173">Now we'll create a virtual environment for local development.</span></span>  <span data-ttu-id="918a1-174">Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="918a1-174">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="918a1-175">Győződjön meg arról, hogy a környezet neve a következő: `env`.</span><span class="sxs-lookup"><span data-stu-id="918a1-175">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="918a1-176">Válassza ki az alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="918a1-176">Select the base interpreter.</span></span>  <span data-ttu-id="918a1-177">Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).</span><span class="sxs-lookup"><span data-stu-id="918a1-177">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="918a1-178">Győződjön meg arról, hogy a csomagok letöltése és telepítése be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="918a1-178">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="918a1-179">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="918a1-179">Click **Create**.</span></span>  <span data-ttu-id="918a1-180">Ezzel létrejön a virtuális környezet, valamint települnek a requirements.txt fájlban található függőségek.</span><span class="sxs-lookup"><span data-stu-id="918a1-180">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="918a1-181">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="918a1-181">Run using development server</span></span>
<span data-ttu-id="918a1-182">A hibakeresés elindításához nyomja le az F5 billentyűt, a webböngészőjében. Ekkor automatikusan a helyileg futtatott lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="918a1-182">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

<span data-ttu-id="918a1-183">Többek között töréspontokat állíthat be a forrásokban, vagy használhatja a figyelőablakokat.  A különböző funkciókról további információkat [a Python Tools for Visual Studio dokumentációjában] talál.</span><span class="sxs-lookup"><span data-stu-id="918a1-183">You can set breakpoints in the sources, use the watch windows, etc.  See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="918a1-184">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="918a1-184">Make changes</span></span>
<span data-ttu-id="918a1-185">Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.</span><span class="sxs-lookup"><span data-stu-id="918a1-185">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="918a1-186">A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="918a1-186">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a><span data-ttu-id="918a1-187">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="918a1-187">Install more packages</span></span>
<span data-ttu-id="918a1-188">Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="918a1-188">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="918a1-189">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="918a1-189">You can install additional packages using pip.</span></span>  <span data-ttu-id="918a1-190">Csomag telepítéséhez kattintson a jobb gombbal a virtuális környezetre, majd válassza az **Install Python Package** (Python-csomag telepítése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="918a1-190">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="918a1-191">Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez adja meg az `azure` karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="918a1-191">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

<span data-ttu-id="918a1-192">Kattintson a jobb gombbal a virtuális környezetre, majd a requirements.txt fájl frissítéséhez válassza a **Generate requirements.txt** (requirements.txt fájl létrehozása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="918a1-192">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="918a1-193">Ezt követően véglegesítse a requirements.txt fájl módosításait a Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="918a1-193">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="918a1-194">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="918a1-194">Deploy to Azure</span></span>
<span data-ttu-id="918a1-195">Az üzembe helyezés indításához kattintson a **Sync** (Szinkronizálás) vagy a **Push** (Leküldés) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="918a1-195">To trigger a deployment, click on **Sync** or **Push**.</span></span>  <span data-ttu-id="918a1-196">A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="918a1-196">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

<span data-ttu-id="918a1-197">Az első üzembe helyezés hosszabb időt vesz igénybe, mivel ennek során megtörténik többek között a virtuális környezet létrehozása és a csomagok telepítése is.</span><span class="sxs-lookup"><span data-stu-id="918a1-197">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="918a1-198">A Visual Studio nem jeleníti meg az üzembe helyezés állapotát.</span><span class="sxs-lookup"><span data-stu-id="918a1-198">Visual Studio doesn't show the progress of the deployment.</span></span>  <span data-ttu-id="918a1-199">A kimenet áttekintéséhez lásd: [Hibaelhárítás – Üzembe helyezés](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="918a1-199">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="918a1-200">A módosítások megtekintéséhez lépjen az Azure URL-címére.</span><span class="sxs-lookup"><span data-stu-id="918a1-200">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="918a1-201">Webalkalmazás-fejlesztés – Windows – parancssor</span><span class="sxs-lookup"><span data-stu-id="918a1-201">Web app development - Windows - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="918a1-202">A tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="918a1-202">Clone the repository</span></span>
<span data-ttu-id="918a1-203">Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával, majd távoli tárházként vegye fel az Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="918a1-203">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="918a1-204">További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="918a1-204">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="918a1-205">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="918a1-205">Create virtual environment</span></span>
<span data-ttu-id="918a1-206">Fejlesztői célra létre fogunk hozni egy új virtuális környezetet (ezt ne vegye fel a tárházba).</span><span class="sxs-lookup"><span data-stu-id="918a1-206">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span>  <span data-ttu-id="918a1-207">A Python virtuális környezetei nem helyezhetőek át, így az alkalmazáson dolgozó összes fejlesztő helyileg hozza létre a sajátját.</span><span class="sxs-lookup"><span data-stu-id="918a1-207">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="918a1-208">Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).</span><span class="sxs-lookup"><span data-stu-id="918a1-208">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="918a1-209">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="918a1-209">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="918a1-210">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="918a1-210">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="918a1-211">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="918a1-211">Install any external packages required by your application.</span></span> <span data-ttu-id="918a1-212">A csomagok virtuális környezetben történő telepítéséhez a tárház gyökérkönyvtárában található requirements.txt fájlt használhatja:</span><span class="sxs-lookup"><span data-stu-id="918a1-212">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="918a1-213">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="918a1-213">Run using development server</span></span>
<span data-ttu-id="918a1-214">A következő paranccsal indíthatja el az alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="918a1-214">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python runserver.py

<span data-ttu-id="918a1-215">A konzolon megjelenik az URL-cím és az a port, amelyen a kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="918a1-215">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

<span data-ttu-id="918a1-216">Ezt követően nyissa meg a webböngészőjében ezt az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="918a1-216">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="918a1-217">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="918a1-217">Make changes</span></span>
<span data-ttu-id="918a1-218">Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.</span><span class="sxs-lookup"><span data-stu-id="918a1-218">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="918a1-219">A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="918a1-219">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="918a1-220">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="918a1-220">Install more packages</span></span>
<span data-ttu-id="918a1-221">Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="918a1-221">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="918a1-222">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="918a1-222">You can install additional packages using pip.</span></span>  <span data-ttu-id="918a1-223">Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="918a1-223">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="918a1-224">Győződjön meg arról, hogy frissítette a requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="918a1-224">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="918a1-225">Véglegesítse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="918a1-225">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="918a1-226">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="918a1-226">Deploy to Azure</span></span>
<span data-ttu-id="918a1-227">Az üzembe helyezés indításához küldje le a módosításokat az Azure-ba:</span><span class="sxs-lookup"><span data-stu-id="918a1-227">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="918a1-228">Itt megtekinthető a telepítési parancsfájl kimenete, amely tartalmazza a virtuális környezet létrehozását, a csomagok telepítését és a web.config fájl létrehozását.</span><span class="sxs-lookup"><span data-stu-id="918a1-228">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="918a1-229">A módosítások megtekintéséhez lépjen az Azure URL-címére.</span><span class="sxs-lookup"><span data-stu-id="918a1-229">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="918a1-230">Webalkalmazás-fejlesztés – Mac/Linux – parancssor</span><span class="sxs-lookup"><span data-stu-id="918a1-230">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="918a1-231">A tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="918a1-231">Clone the repository</span></span>
<span data-ttu-id="918a1-232">Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával, majd távoli tárházként vegye fel az Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="918a1-232">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="918a1-233">További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="918a1-233">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="918a1-234">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="918a1-234">Create virtual environment</span></span>
<span data-ttu-id="918a1-235">Fejlesztői célra létre fogunk hozni egy új virtuális környezetet (ezt ne vegye fel a tárházba).</span><span class="sxs-lookup"><span data-stu-id="918a1-235">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span>  <span data-ttu-id="918a1-236">A Python virtuális környezetei nem helyezhetőek át, így az alkalmazáson dolgozó összes fejlesztő helyileg hozza létre a sajátját.</span><span class="sxs-lookup"><span data-stu-id="918a1-236">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="918a1-237">Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).</span><span class="sxs-lookup"><span data-stu-id="918a1-237">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="918a1-238">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="918a1-238">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="918a1-239">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="918a1-239">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="918a1-240">vagy pyvenv env</span><span class="sxs-lookup"><span data-stu-id="918a1-240">or pyvenv env</span></span>

<span data-ttu-id="918a1-241">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="918a1-241">Install any external packages required by your application.</span></span> <span data-ttu-id="918a1-242">A csomagok virtuális környezetben történő telepítéséhez a tárház gyökérkönyvtárában található requirements.txt fájlt használhatja:</span><span class="sxs-lookup"><span data-stu-id="918a1-242">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="918a1-243">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="918a1-243">Run using development server</span></span>
<span data-ttu-id="918a1-244">A következő paranccsal indíthatja el az alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="918a1-244">You can launch the application under a development server with the following command:</span></span>

    env/bin/python runserver.py

<span data-ttu-id="918a1-245">A konzolon megjelenik az URL-cím és az a port, amelyen a kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="918a1-245">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

<span data-ttu-id="918a1-246">Ezt követően nyissa meg a webböngészőjében ezt az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="918a1-246">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="918a1-247">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="918a1-247">Make changes</span></span>
<span data-ttu-id="918a1-248">Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.</span><span class="sxs-lookup"><span data-stu-id="918a1-248">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="918a1-249">A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="918a1-249">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="918a1-250">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="918a1-250">Install more packages</span></span>
<span data-ttu-id="918a1-251">Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="918a1-251">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="918a1-252">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="918a1-252">You can install additional packages using pip.</span></span>  <span data-ttu-id="918a1-253">Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="918a1-253">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="918a1-254">Győződjön meg arról, hogy frissítette a requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="918a1-254">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="918a1-255">Véglegesítse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="918a1-255">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="918a1-256">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="918a1-256">Deploy to Azure</span></span>
<span data-ttu-id="918a1-257">Az üzembe helyezés indításához küldje le a módosításokat az Azure-ba:</span><span class="sxs-lookup"><span data-stu-id="918a1-257">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="918a1-258">Itt megtekinthető a telepítési parancsfájl kimenete, amely tartalmazza a virtuális környezet létrehozását, a csomagok telepítését és a web.config fájl létrehozását.</span><span class="sxs-lookup"><span data-stu-id="918a1-258">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="918a1-259">A módosítások megtekintéséhez lépjen az Azure URL-címére.</span><span class="sxs-lookup"><span data-stu-id="918a1-259">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="918a1-260">Hibaelhárítás – Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="918a1-260">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="918a1-261">Hibaelhárítás – Virtuális környezet</span><span class="sxs-lookup"><span data-stu-id="918a1-261">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="918a1-262">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="918a1-262">Next Steps</span></span>
<span data-ttu-id="918a1-263">Az alábbi hivatkozásokból tudhat meg többet a Flask és a Python Tools for Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="918a1-263">Follow these links to learn more about Flask and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="918a1-264">[Flask-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="918a1-264">[Flask Documentation]</span></span>
* <span data-ttu-id="918a1-265">[a Python Tools for Visual Studio dokumentációjában]</span><span class="sxs-lookup"><span data-stu-id="918a1-265">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="918a1-266">Információk az Azure Table Storage és a MongoDB használatával:</span><span class="sxs-lookup"><span data-stu-id="918a1-266">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="918a1-267">[Flask és a Python Tools for Visual Studio Azure MongoDB]</span><span class="sxs-lookup"><span data-stu-id="918a1-267">[Flask and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="918a1-268">[Flask és az Azure Table Storage Azure Python Tools for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="918a1-268">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="918a1-269">További információ: a [Python fejlesztői központ](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="918a1-269">For more information, see also the [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="918a1-270">A változások</span><span class="sxs-lookup"><span data-stu-id="918a1-270">What's changed</span></span>
* <span data-ttu-id="918a1-271">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="918a1-271">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
<span data-ttu-id="918a1-272">[Flask és a Python Tools for Visual Studio Azure MongoDB]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure</span><span class="sxs-lookup"><span data-stu-id="918a1-272">[Flask and MongoDB on Azure with Python Tools for Visual Studio]: https://github.com/microsoft/ptvs/wiki/Flask-and-MongoDB-on-Azure</span></span>
<span data-ttu-id="918a1-273">[Flask és az Azure Table Storage Azure Python Tools for Visual Studio]: web-sites-python-ptvs-flask-table-storage.md</span><span class="sxs-lookup"><span data-stu-id="918a1-273">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-flask-table-storage.md</span></span>

<!--External Link references-->
<span data-ttu-id="918a1-274">[Python 2.7-hez készült Azure SDK]: http://go.microsoft.com/fwlink/?linkid=254281</span><span class="sxs-lookup"><span data-stu-id="918a1-274">[Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?linkid=254281</span></span>
<span data-ttu-id="918a1-275">[Python 3.4-hez készült Azure SDK]: http://go.microsoft.com/fwlink/?linkid=516990</span><span class="sxs-lookup"><span data-stu-id="918a1-275">[Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?linkid=516990</span></span>
<span data-ttu-id="918a1-276">[python.org]: http://www.python.org/</span><span class="sxs-lookup"><span data-stu-id="918a1-276">[python.org]: http://www.python.org/</span></span>
<span data-ttu-id="918a1-277">[Git for Windows]: http://msysgit.github.io/</span><span class="sxs-lookup"><span data-stu-id="918a1-277">[Git for Windows]: http://msysgit.github.io/</span></span>
<span data-ttu-id="918a1-278">[GitHub for Windows]: https://windows.github.com/</span><span class="sxs-lookup"><span data-stu-id="918a1-278">[GitHub for Windows]: https://windows.github.com/</span></span>
<span data-ttu-id="918a1-279">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span><span class="sxs-lookup"><span data-stu-id="918a1-279">[Python Tools for Visual Studio]: http://aka.ms/ptvs</span></span>
<span data-ttu-id="918a1-280">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span><span class="sxs-lookup"><span data-stu-id="918a1-280">[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025</span></span>
<span data-ttu-id="918a1-281">[Visual Studio]: http://www.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="918a1-281">[Visual Studio]: http://www.visualstudio.com/</span></span>
<span data-ttu-id="918a1-282">[a Python Tools for Visual Studio dokumentációjában]: http://aka.ms/ptvsdocs</span><span class="sxs-lookup"><span data-stu-id="918a1-282">[Python Tools for Visual Studio Documentation]: http://aka.ms/ptvsdocs</span></span>
<span data-ttu-id="918a1-283">[Flask-dokumentáció]: http://flask.pocoo.org/</span><span class="sxs-lookup"><span data-stu-id="918a1-283">[Flask Documentation]: http://flask.pocoo.org/</span></span> 

