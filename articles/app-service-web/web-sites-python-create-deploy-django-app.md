---
title: "Webalkalmazások létrehozása a Djangóval az Azure-ban"
description: "A Python webalkalmazás az Azure App Service Web Apps szolgáltatásban történő futtatását bemutató oktatóanyag."
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
ms.openlocfilehash: 388a2db21dd1669b48b3204aaa322d7915905506
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="creating-web-apps-with-django-in-azure"></a><span data-ttu-id="4f80b-103">Webalkalmazások létrehozása a Djangóval az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4f80b-103">Creating web apps with Django in Azure</span></span>
<span data-ttu-id="4f80b-104">Ez az oktatóanyag a Python futtatásának első lépéseit mutatja be az [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) felületén.</span><span class="sxs-lookup"><span data-stu-id="4f80b-104">This tutorial describes how to get started running Python on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="4f80b-105">A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja!</span><span class="sxs-lookup"><span data-stu-id="4f80b-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span> <span data-ttu-id="4f80b-106">Az alkalmazása növekedésével átválthat fizetős üzemeltetésre, valamint integrálhatja alkalmazásába az összes többi Azure-szolgáltatást is.</span><span class="sxs-lookup"><span data-stu-id="4f80b-106">As your app grows, you can switch to paid hosting, and you can also integrate with all of the other Azure services.</span></span>

<span data-ttu-id="4f80b-107">A Django webes keretrendszer használatával fog létrehozni egy alkalmazást (lásd az oktatóanyag alternatív verzióit a [Flask](web-sites-python-create-deploy-flask-app.md) és a [Bottle](web-sites-python-create-deploy-bottle-app.md) használatával).</span><span class="sxs-lookup"><span data-stu-id="4f80b-107">You will create an application using the Django web framework (see alternate versions of this tutorial for [Flask](web-sites-python-create-deploy-flask-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span> <span data-ttu-id="4f80b-108">Létre fog hozni egy webalkalmazást az Azure Piactérről, be fogja állítani a Git üzemelő példányt, majd helyileg fogja klónozni a tárházat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-108">You will create the web app from the Azure Marketplace, set up Git deployment, and clone the repository locally.</span></span> <span data-ttu-id="4f80b-109">Ezt követően helyileg fogja futtatni az alkalmazást, módosításokat fog elvégezni rajta, majd véglegesíteni fogja őket, és el fogja küldeni az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="4f80b-109">Then you will run the application locally, make changes, commit and push them to Azure.</span></span> <span data-ttu-id="4f80b-110">Ez az oktatóanyag ennek a folyamatnak a Windows vagy Mac/Linux operációs rendszeren történő végrehajtását mutatja be.</span><span class="sxs-lookup"><span data-stu-id="4f80b-110">The tutorial shows how to do this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="4f80b-111">Ha nem szeretne regisztrálni Azure-fiókot az Azure App Service megismerése előtt, lépjen [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra, ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="4f80b-111">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="4f80b-112">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="4f80b-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4f80b-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4f80b-113">Prerequisites</span></span>
* <span data-ttu-id="4f80b-114">Windows, Mac vagy Linux</span><span class="sxs-lookup"><span data-stu-id="4f80b-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="4f80b-115">Python 2.7 vagy 3.4</span><span class="sxs-lookup"><span data-stu-id="4f80b-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="4f80b-116">setuptools, pip, virtualenv (csak Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="4f80b-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="4f80b-117">Git</span><span class="sxs-lookup"><span data-stu-id="4f80b-117">Git</span></span>
* <span data-ttu-id="4f80b-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) – Megjegyzés: szabadon választható</span><span class="sxs-lookup"><span data-stu-id="4f80b-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="4f80b-119">**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="4f80b-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="4f80b-120">Windows</span><span class="sxs-lookup"><span data-stu-id="4f80b-120">Windows</span></span>
<span data-ttu-id="4f80b-121">Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését.</span><span class="sxs-lookup"><span data-stu-id="4f80b-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span> <span data-ttu-id="4f80b-122">Ezzel többek között a következőket telepíti: a Python 32 bites verziója, setuptools, pip, virtualenv (a Python 32 bites verziója van telepítve az Azure-gazdagépeken).</span><span class="sxs-lookup"><span data-stu-id="4f80b-122">This installs the 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on the Azure host machines).</span></span> <span data-ttu-id="4f80b-123">Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="4f80b-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="4f80b-124">A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="4f80b-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span> <span data-ttu-id="4f80b-125">Visual Studio használata esetén használhatja a beépített Git-támogatást.</span><span class="sxs-lookup"><span data-stu-id="4f80b-125">If you use Visual Studio, you can use the integrated Git support.</span></span>

<span data-ttu-id="4f80b-126">Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését.</span><span class="sxs-lookup"><span data-stu-id="4f80b-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span> <span data-ttu-id="4f80b-127">Ez ugyan nem kötelező, de ha rendelkezik az ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat is magában foglaló [Visual Studio] szoftverrel, a nagyszerű Python IDE előnyeit is élvezheti.</span><span class="sxs-lookup"><span data-stu-id="4f80b-127">This is optional, but if you have [Visual Studio], including the free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="4f80b-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="4f80b-128">Mac/Linux</span></span>
<span data-ttu-id="4f80b-129">A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.</span><span class="sxs-lookup"><span data-stu-id="4f80b-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-creation-on-portal"></a><span data-ttu-id="4f80b-130">Webalkalmazás létrehozása a portálon</span><span class="sxs-lookup"><span data-stu-id="4f80b-130">Web App Creation on Portal</span></span>
<span data-ttu-id="4f80b-131">Saját alkalmazása létrehozásának első lépése egy webalkalmazás létrehozása az [Azure portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4f80b-131">The first step in creating your app is to create the web app via the [Azure Portal](https://portal.azure.com).</span></span>

1. <span data-ttu-id="4f80b-132">Jelentkezzen be az Azure portálra, majd kattintson a bal alsó sarokban található **NEW** (ÚJ) gombra.</span><span class="sxs-lookup"><span data-stu-id="4f80b-132">Log into the Azure Portal and click the **NEW** button in the bottom left corner.</span></span>
2. <span data-ttu-id="4f80b-133">A keresőmezőbe írja be a „python” kifejezést.</span><span class="sxs-lookup"><span data-stu-id="4f80b-133">In the search box, type "python".</span></span>
3. <span data-ttu-id="4f80b-134">A keresési eredmények közül válassza ki a **Django** elemet (amelyet a PTVS tett közzé), majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4f80b-134">In the search results, select **Django** (published by PTVS), then click **Create**.</span></span>
4. <span data-ttu-id="4f80b-135">Konfigurálja az új Django-alkalmazást, például új App Service-csomag és egy ahhoz tartozó új erőforráscsoport létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="4f80b-135">Configure the new Django app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="4f80b-136">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4f80b-136">Then, click **Create**.</span></span>
5. <span data-ttu-id="4f80b-137">Konfigurálja az újonnan létrehozott webalkalmazáshoz tartozó Git-közzétételt a [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben) részben megadott utasítások szerint.</span><span class="sxs-lookup"><span data-stu-id="4f80b-137">Configure Git publishing for your newly created web app by following the instructions at [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="4f80b-138">Az alkalmazás áttekintése</span><span class="sxs-lookup"><span data-stu-id="4f80b-138">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="4f80b-139">A Git-tárház tartalma</span><span class="sxs-lookup"><span data-stu-id="4f80b-139">Git repository contents</span></span>
<span data-ttu-id="4f80b-140">Az alábbiakban áttekintjük a kiindulási Git-tárházban található fájlokat. A következő szakaszban ezek klónozását fogjuk elvégezni.</span><span class="sxs-lookup"><span data-stu-id="4f80b-140">Here's an overview of the files you'll find in the initial Git repository, which we'll clone in the next section.</span></span>

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

<span data-ttu-id="4f80b-141">Az alkalmazás fő forrásai.</span><span class="sxs-lookup"><span data-stu-id="4f80b-141">Main sources for the application.</span></span> <span data-ttu-id="4f80b-142">3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel.</span><span class="sxs-lookup"><span data-stu-id="4f80b-142">Consists of 3 pages (index, about, contact) with a master layout.</span></span> <span data-ttu-id="4f80b-143">Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.</span><span class="sxs-lookup"><span data-stu-id="4f80b-143">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \manage.py

<span data-ttu-id="4f80b-144">A helyi felügyelet és a fejlesztési kiszolgáló támogatása.</span><span class="sxs-lookup"><span data-stu-id="4f80b-144">Local management and development server support.</span></span> <span data-ttu-id="4f80b-145">Válassza ezt a lehetőséget például az alkalmazás helyi futtatásához vagy az adatbázis szinkronizálásához.</span><span class="sxs-lookup"><span data-stu-id="4f80b-145">Use this to run the application locally, synchronize the database, etc.</span></span>

    \db.sqlite3

<span data-ttu-id="4f80b-146">Az alapértelmezett adatbázis.</span><span class="sxs-lookup"><span data-stu-id="4f80b-146">Default database.</span></span> <span data-ttu-id="4f80b-147">Tartalmazza az alkalmazás futtatásához szükséges táblákat, de nem tartalmaz felhasználókat (felhasználó az adatbázis szinkronizálásával hozható létre).</span><span class="sxs-lookup"><span data-stu-id="4f80b-147">Includes the necessary tables for the application to run, but does not contain any users (synchronize the database to create a user).</span></span>

    \DjangoWebProject.pyproj
    \DjangoWebProject.sln

<span data-ttu-id="4f80b-148">A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.</span><span class="sxs-lookup"><span data-stu-id="4f80b-148">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="4f80b-149">IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.</span><span class="sxs-lookup"><span data-stu-id="4f80b-149">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="4f80b-150">Az alkalmazás számára szükséges külső csomagok.</span><span class="sxs-lookup"><span data-stu-id="4f80b-150">External packages needed by this application.</span></span> <span data-ttu-id="4f80b-151">A telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítését.</span><span class="sxs-lookup"><span data-stu-id="4f80b-151">The deployment script will pip install the packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="4f80b-152">IIS-konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="4f80b-152">IIS configuration files.</span></span> <span data-ttu-id="4f80b-153">A telepítési parancsfájl a megfelelő web.x.y.config fájlt fogja használni, és web.config fájlként fogja azt másolni.</span><span class="sxs-lookup"><span data-stu-id="4f80b-153">The deployment script will use the appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="4f80b-154">Opcionális fájlok – A telepítés testre szabása</span><span class="sxs-lookup"><span data-stu-id="4f80b-154">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-django-customizing-deployment](../../includes/web-sites-python-django-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="4f80b-155">Opcionális fájlok – Python-futtatókörnyezet</span><span class="sxs-lookup"><span data-stu-id="4f80b-155">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="4f80b-156">További fájlok a kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="4f80b-156">Additional files on server</span></span>
<span data-ttu-id="4f80b-157">Egyes fájlok megtalálhatóak ugyan a kiszolgálón, de nem lettek felvéve a Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="4f80b-157">Some files exist on the server but are not added to the git repository.</span></span> <span data-ttu-id="4f80b-158">Ezeket a telepítési parancsfájl hozza létre.</span><span class="sxs-lookup"><span data-stu-id="4f80b-158">These are created by the deployment script.</span></span>

    \web.config

<span data-ttu-id="4f80b-159">IIS-konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="4f80b-159">IIS configuration file.</span></span> <span data-ttu-id="4f80b-160">Minden telepítéshez a web.x.y.config fájlból jön létre.</span><span class="sxs-lookup"><span data-stu-id="4f80b-160">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="4f80b-161">Python virtuális környezet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-161">Python virtual environment.</span></span> <span data-ttu-id="4f80b-162">A telepítés során jön létre, ha a webalkalmazáson még nem található kompatibilis virtuális környezet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-162">Created during deployment if a compatible virtual environment doesn't already exist on the web app.</span></span> <span data-ttu-id="4f80b-163">Megtörténik a requirements.txt fájlban felsorolt csomagok telepítése, a korábban már telepített csomagokat azonban a pip már nem telepíti újra.</span><span class="sxs-lookup"><span data-stu-id="4f80b-163">Packages listed in requirements.txt are pip installed, but pip will skip installation if the packages are already installed.</span></span>

<span data-ttu-id="4f80b-164">A következő három szakaszban a webalkalmazások fejlesztéséről talál információkat, három különböző környezetben:</span><span class="sxs-lookup"><span data-stu-id="4f80b-164">The next 3 sections describe how to proceed with the web app development under 3 different environments:</span></span>

* <span data-ttu-id="4f80b-165">Windows, Python Tools for Visual Studio alkalmazással</span><span class="sxs-lookup"><span data-stu-id="4f80b-165">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="4f80b-166">Windows, parancssorral</span><span class="sxs-lookup"><span data-stu-id="4f80b-166">Windows, with command line</span></span>
* <span data-ttu-id="4f80b-167">Mac/Linux, parancssorral</span><span class="sxs-lookup"><span data-stu-id="4f80b-167">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="4f80b-168">Webalkalmazás-fejlesztés – Windows – Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f80b-168">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="4f80b-169">A tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-169">Clone the repository</span></span>
<span data-ttu-id="4f80b-170">Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="4f80b-170">First, clone the repository using the URL provided on the Azure Portal.</span></span> <span data-ttu-id="4f80b-171">További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="4f80b-171">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="4f80b-172">Nyissa meg a tárház gyökérkönyvtárában található megoldásfájlt (.sln).</span><span class="sxs-lookup"><span data-stu-id="4f80b-172">Open the solution file (.sln) that is included in the root of the repository.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-solution-django.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="4f80b-173">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-173">Create virtual environment</span></span>
<span data-ttu-id="4f80b-174">Most helyi telepítéshez tartozó virtuális környezetet hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="4f80b-174">Now we'll create a virtual environment for local development.</span></span> <span data-ttu-id="4f80b-175">Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4f80b-175">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="4f80b-176">Győződjön meg arról, hogy a környezet neve a következő: `env`.</span><span class="sxs-lookup"><span data-stu-id="4f80b-176">Make sure the name of the environment is `env`.</span></span>
* <span data-ttu-id="4f80b-177">Válassza ki az alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="4f80b-177">Select the base interpreter.</span></span> <span data-ttu-id="4f80b-178">Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az **Application Settings** (Alkalmazásbeállítások) panelen található meg).</span><span class="sxs-lookup"><span data-stu-id="4f80b-178">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the **Application Settings** blade of your web app in the Azure Portal).</span></span>
* <span data-ttu-id="4f80b-179">Győződjön meg arról, hogy a csomagok letöltése és telepítése be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="4f80b-179">Make sure the option to download and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="4f80b-180">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4f80b-180">Click **Create**.</span></span> <span data-ttu-id="4f80b-181">Ezzel létrejön a virtuális környezet, valamint települnek a requirements.txt fájlban található függőségek.</span><span class="sxs-lookup"><span data-stu-id="4f80b-181">This will create the virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="create-a-superuser"></a><span data-ttu-id="4f80b-182">Felügyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-182">Create a superuser</span></span>
<span data-ttu-id="4f80b-183">Az alkalmazáshoz tartozó adatbázishoz nincs megadva felügyelő.</span><span class="sxs-lookup"><span data-stu-id="4f80b-183">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="4f80b-184">Az alkalmazás bejelentkezési funkciójának vagy a Django felügyeleti felületének használatához (ha úgy dönt, hogy engedélyezi) felügyelőt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4f80b-184">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="4f80b-185">A projektmappa parancssorából futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4f80b-185">Run this from the command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="4f80b-186">Kövesse a megjelenő utasításokat a felhasználónév és a jelszó beállításához és egyéb műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="4f80b-186">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="4f80b-187">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="4f80b-187">Run using development server</span></span>
<span data-ttu-id="4f80b-188">A hibakeresés elindításához nyomja le az F5 billentyűt, a webböngészőjében. Ekkor automatikusan a helyileg futtatott lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="4f80b-188">Press F5 to start debugging, and your web browser will open automatically to the page running locally.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

<span data-ttu-id="4f80b-189">Többek között töréspontokat állíthat be a forrásokban, vagy használhatja a figyelőablakokat. A különböző funkciókról további információkat [a Python Tools for Visual Studio dokumentációjában] talál.</span><span class="sxs-lookup"><span data-stu-id="4f80b-189">You can set breakpoints in the sources, use the watch windows, etc. See the [Python Tools for Visual Studio Documentation] for more information on the various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="4f80b-190">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="4f80b-190">Make changes</span></span>
<span data-ttu-id="4f80b-191">Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.</span><span class="sxs-lookup"><span data-stu-id="4f80b-191">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="4f80b-192">A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="4f80b-192">After you've tested your changes, commit them to the Git repository:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-commit-django.png)

### <a name="install-more-packages"></a><span data-ttu-id="4f80b-193">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="4f80b-193">Install more packages</span></span>
<span data-ttu-id="4f80b-194">Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-194">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="4f80b-195">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-195">You can install additional packages using pip.</span></span> <span data-ttu-id="4f80b-196">Csomag telepítéséhez kattintson a jobb gombbal a virtuális környezetre, majd válassza az **Install Python Package** (Python-csomag telepítése) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4f80b-196">To install a package, right-click on the virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="4f80b-197">Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez adja meg az `azure` karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="4f80b-197">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-install-package-dialog.png)

<span data-ttu-id="4f80b-198">Kattintson a jobb gombbal a virtuális környezetre, majd a requirements.txt fájl frissítéséhez válassza a **Generate requirements.txt** (requirements.txt fájl létrehozása) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4f80b-198">Right-click on the virtual environment and select **Generate requirements.txt** to update requirements.txt.</span></span>

<span data-ttu-id="4f80b-199">Ezt követően véglegesítse a requirements.txt fájl módosításait a Git-tárházban.</span><span class="sxs-lookup"><span data-stu-id="4f80b-199">Then, commit the changes to requirements.txt to the Git repository.</span></span>

### <a name="deploy-to-azure"></a><span data-ttu-id="4f80b-200">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4f80b-200">Deploy to Azure</span></span>
<span data-ttu-id="4f80b-201">Az üzembe helyezés indításához kattintson a **Sync** (Szinkronizálás) vagy a **Push** (Leküldés) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="4f80b-201">To trigger a deployment, click on **Sync** or **Push**.</span></span> <span data-ttu-id="4f80b-202">A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4f80b-202">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-django-app/ptvs-git-push.png)

<span data-ttu-id="4f80b-203">Az első üzembe helyezés hosszabb időt vesz igénybe, mivel ennek során megtörténik többek között a virtuális környezet létrehozása és a csomagok telepítése is.</span><span class="sxs-lookup"><span data-stu-id="4f80b-203">The first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="4f80b-204">A Visual Studio nem jeleníti meg az üzembe helyezés állapotát.</span><span class="sxs-lookup"><span data-stu-id="4f80b-204">Visual Studio doesn't show the progress of the deployment.</span></span> <span data-ttu-id="4f80b-205">A kimenet áttekintéséhez lásd: [Hibaelhárítás – Üzembe helyezés](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="4f80b-205">If you'd like to review the output, see the section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="4f80b-206">A módosítások megtekintéséhez lépjen az Azure URL-címére.</span><span class="sxs-lookup"><span data-stu-id="4f80b-206">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="4f80b-207">Webalkalmazás-fejlesztés – Windows – parancssor</span><span class="sxs-lookup"><span data-stu-id="4f80b-207">Web app development - Windows - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="4f80b-208">A tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-208">Clone the repository</span></span>
<span data-ttu-id="4f80b-209">Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával, majd távoli tárházként vegye fel az Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-209">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="4f80b-210">További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="4f80b-210">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="4f80b-211">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-211">Create virtual environment</span></span>
<span data-ttu-id="4f80b-212">Fejlesztői célra létre fogunk hozni egy új virtuális környezetet (ezt ne vegye fel a tárházba).</span><span class="sxs-lookup"><span data-stu-id="4f80b-212">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="4f80b-213">A Python virtuális környezetei nem helyezhetőek át, így az alkalmazáson dolgozó összes fejlesztő helyileg hozza létre a sajátját.</span><span class="sxs-lookup"><span data-stu-id="4f80b-213">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="4f80b-214">Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az Application Settings (Alkalmazásbeállítások) panelen található meg).</span><span class="sxs-lookup"><span data-stu-id="4f80b-214">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="4f80b-215">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="4f80b-215">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="4f80b-216">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="4f80b-216">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="4f80b-217">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-217">Install any external packages required by your application.</span></span> <span data-ttu-id="4f80b-218">A csomagok virtuális környezetben történő telepítéséhez a tárház gyökérkönyvtárában található requirements.txt fájlt használhatja:</span><span class="sxs-lookup"><span data-stu-id="4f80b-218">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="4f80b-219">Felügyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-219">Create a superuser</span></span>
<span data-ttu-id="4f80b-220">Az alkalmazáshoz tartozó adatbázishoz nincs megadva felügyelő.</span><span class="sxs-lookup"><span data-stu-id="4f80b-220">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="4f80b-221">Az alkalmazás bejelentkezési funkciójának vagy a Django felügyeleti felületének használatához (ha úgy dönt, hogy engedélyezi) felügyelőt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4f80b-221">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="4f80b-222">A projektmappa parancssorából futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4f80b-222">Run this from the command-line from your project folder:</span></span>

    env\scripts\python manage.py createsuperuser

<span data-ttu-id="4f80b-223">Kövesse a megjelenő utasításokat a felhasználónév és a jelszó beállításához és egyéb műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="4f80b-223">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="4f80b-224">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="4f80b-224">Run using development server</span></span>
<span data-ttu-id="4f80b-225">A következő paranccsal indíthatja el az alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-225">You can launch the application under a development server with the following command:</span></span>

    env\scripts\python manage.py runserver

<span data-ttu-id="4f80b-226">A konzolon megjelenik az URL-cím és az a port, amelyen a kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="4f80b-226">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-run-local-django.png)

<span data-ttu-id="4f80b-227">Ezt követően nyissa meg a webböngészőjében ezt az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-227">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/windows-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="4f80b-228">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="4f80b-228">Make changes</span></span>
<span data-ttu-id="4f80b-229">Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.</span><span class="sxs-lookup"><span data-stu-id="4f80b-229">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="4f80b-230">A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="4f80b-230">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="4f80b-231">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="4f80b-231">Install more packages</span></span>
<span data-ttu-id="4f80b-232">Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-232">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="4f80b-233">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-233">You can install additional packages using pip.</span></span> <span data-ttu-id="4f80b-234">Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-234">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="4f80b-235">Győződjön meg arról, hogy frissítette a requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-235">Make sure to update requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="4f80b-236">Véglegesítse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="4f80b-236">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="4f80b-237">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4f80b-237">Deploy to Azure</span></span>
<span data-ttu-id="4f80b-238">Az üzembe helyezés indításához küldje le a módosításokat az Azure-ba:</span><span class="sxs-lookup"><span data-stu-id="4f80b-238">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="4f80b-239">Itt megtekinthető a telepítési parancsfájl kimenete, amely tartalmazza a virtuális környezet létrehozását, a csomagok telepítését és a web.config fájl létrehozását.</span><span class="sxs-lookup"><span data-stu-id="4f80b-239">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="4f80b-240">A módosítások megtekintéséhez lépjen az Azure URL-címére.</span><span class="sxs-lookup"><span data-stu-id="4f80b-240">Browse to the Azure URL to view your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="4f80b-241">Webalkalmazás-fejlesztés – Mac/Linux – parancssor</span><span class="sxs-lookup"><span data-stu-id="4f80b-241">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-the-repository"></a><span data-ttu-id="4f80b-242">A tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-242">Clone the repository</span></span>
<span data-ttu-id="4f80b-243">Első lépésben klónozza a tárházat az Azure Portalon található URL-cím használatával, majd távoli tárházként vegye fel az Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-243">First, clone the repository using the URL provided on the Azure Portal, and add the Azure repository as a remote.</span></span> <span data-ttu-id="4f80b-244">További információ: [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md) (Helyi Git-üzembehelyezés az Azure App Service-ben).</span><span class="sxs-lookup"><span data-stu-id="4f80b-244">For more information, see [Local Git Deployment to Azure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url>

### <a name="create-virtual-environment"></a><span data-ttu-id="4f80b-245">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-245">Create virtual environment</span></span>
<span data-ttu-id="4f80b-246">Fejlesztői célra létre fogunk hozni egy új virtuális környezetet (ezt ne vegye fel a tárházba).</span><span class="sxs-lookup"><span data-stu-id="4f80b-246">We'll create a new virtual environment for development purposes (do not add it to the repository).</span></span> <span data-ttu-id="4f80b-247">A Python virtuális környezetei nem helyezhetőek át, így az alkalmazáson dolgozó összes fejlesztő helyileg hozza létre a sajátját.</span><span class="sxs-lookup"><span data-stu-id="4f80b-247">Virtual environments in Python are not relocatable, so every developer working on the application will create their own locally.</span></span>

<span data-ttu-id="4f80b-248">Győződjön meg arról, hogy a Python ugyanazon verzióját használja, mint ami a webalkalmazás esetében ki lett választva (ez az információ az Azure portálon lévő webalkalmazásának runtime.txt fájljában vagy az Application Settings (Alkalmazásbeállítások) panelen található meg).</span><span class="sxs-lookup"><span data-stu-id="4f80b-248">Make sure to use the same version of Python that is selected for your web app (in runtime.txt or the Application Settings blade of your web app in the Azure Portal).</span></span>

<span data-ttu-id="4f80b-249">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="4f80b-249">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="4f80b-250">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="4f80b-250">For Python 3.4:</span></span>

    python -m venv env

<span data-ttu-id="4f80b-251">vagy</span><span class="sxs-lookup"><span data-stu-id="4f80b-251">or</span></span>

    pyvenv env

<span data-ttu-id="4f80b-252">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-252">Install any external packages required by your application.</span></span> <span data-ttu-id="4f80b-253">A csomagok virtuális környezetben történő telepítéséhez a tárház gyökérkönyvtárában található requirements.txt fájlt használhatja:</span><span class="sxs-lookup"><span data-stu-id="4f80b-253">You can use the requirements.txt file at the root of the repository to install the packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="create-a-superuser"></a><span data-ttu-id="4f80b-254">Felügyelő létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f80b-254">Create a superuser</span></span>
<span data-ttu-id="4f80b-255">Az alkalmazáshoz tartozó adatbázishoz nincs megadva felügyelő.</span><span class="sxs-lookup"><span data-stu-id="4f80b-255">The database included with the application does not have any superuser defined.</span></span> <span data-ttu-id="4f80b-256">Az alkalmazás bejelentkezési funkciójának vagy a Django felügyeleti felületének használatához (ha úgy dönt, hogy engedélyezi) felügyelőt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4f80b-256">In order to use the sign-in functionality in the application, or the Django admin interface (if you decide to enable it), you'll need to create a superuser.</span></span>

<span data-ttu-id="4f80b-257">A projektmappa parancssorából futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4f80b-257">Run this from the command-line from your project folder:</span></span>

    env/bin/python manage.py createsuperuser

<span data-ttu-id="4f80b-258">Kövesse a megjelenő utasításokat a felhasználónév és a jelszó beállításához és egyéb műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="4f80b-258">Follow the prompts to set the user name, password, etc.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="4f80b-259">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="4f80b-259">Run using development server</span></span>
<span data-ttu-id="4f80b-260">A következő paranccsal indíthatja el az alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-260">You can launch the application under a development server with the following command:</span></span>

    env/bin/python manage.py runserver

<span data-ttu-id="4f80b-261">A konzolon megjelenik az URL-cím és az a port, amelyen a kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="4f80b-261">The console will display the URL and port the server listens to:</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-run-local-django.png)

<span data-ttu-id="4f80b-262">Ezt követően nyissa meg a webböngészőjében ezt az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-262">Then, open your web browser to that URL.</span></span>

![](./media/web-sites-python-create-deploy-django-app/mac-browser-django.png)

### <a name="make-changes"></a><span data-ttu-id="4f80b-263">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="4f80b-263">Make changes</span></span>
<span data-ttu-id="4f80b-264">Most kísérletezhet azzal, hogy módosításokat hajt végre az alkalmazásforrásokon és/vagy -sablonokon.</span><span class="sxs-lookup"><span data-stu-id="4f80b-264">Now you can experiment by making changes to the application sources and/or templates.</span></span>

<span data-ttu-id="4f80b-265">A módosítások tesztelését követően véglegesítse azokat a Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="4f80b-265">After you've tested your changes, commit them to the Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="4f80b-266">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="4f80b-266">Install more packages</span></span>
<span data-ttu-id="4f80b-267">Az alkalmazása a Python és a Django eszközön kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-267">Your application may have dependencies beyond Python and Django.</span></span>

<span data-ttu-id="4f80b-268">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="4f80b-268">You can install additional packages using pip.</span></span> <span data-ttu-id="4f80b-269">Például az Azure Storage, Service Bus és további Azure-szolgáltatásokhoz hozzáférést biztosító, Pythonhoz készült Azure SDK telepítéséhez írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-269">For example, to install the Azure SDK for Python, which gives you access to Azure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="4f80b-270">Győződjön meg arról, hogy frissítette a requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-270">Make sure to update requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="4f80b-271">Véglegesítse a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="4f80b-271">Commit the changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-to-azure"></a><span data-ttu-id="4f80b-272">Üzembe helyezés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="4f80b-272">Deploy to Azure</span></span>
<span data-ttu-id="4f80b-273">Az üzembe helyezés indításához küldje le a módosításokat az Azure-ba:</span><span class="sxs-lookup"><span data-stu-id="4f80b-273">To trigger a deployment, push the changes to Azure:</span></span>

    git push azure master

<span data-ttu-id="4f80b-274">Itt megtekinthető a telepítési parancsfájl kimenete, amely tartalmazza a virtuális környezet létrehozását, a csomagok telepítését és a web.config fájl létrehozását.</span><span class="sxs-lookup"><span data-stu-id="4f80b-274">You will see the output of the deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="4f80b-275">A módosítások megtekintéséhez lépjen az Azure URL-címére.</span><span class="sxs-lookup"><span data-stu-id="4f80b-275">Browse to the Azure URL to view your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="4f80b-276">Hibaelhárítás – Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="4f80b-276">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="4f80b-277">Hibaelhárítás – Virtuális környezet</span><span class="sxs-lookup"><span data-stu-id="4f80b-277">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="troubleshooting---static-files"></a><span data-ttu-id="4f80b-278">Hibaelhárítás – Statikus fájlok</span><span class="sxs-lookup"><span data-stu-id="4f80b-278">Troubleshooting - Static Files</span></span>
<span data-ttu-id="4f80b-279">A Django statikus fájlokat gyűjt.</span><span class="sxs-lookup"><span data-stu-id="4f80b-279">Django has the concept of collecting static files.</span></span> <span data-ttu-id="4f80b-280">Ez azt jelenti, hogy az összes statikus fájlt eredeti helyükről egyetlen mappába másolja.</span><span class="sxs-lookup"><span data-stu-id="4f80b-280">This takes all the static files from their original location and copies them to a single folder.</span></span> <span data-ttu-id="4f80b-281">A jelen alkalmazás esetében a másolás helye: `/static`.</span><span class="sxs-lookup"><span data-stu-id="4f80b-281">For this application, they are copied to `/static`.</span></span>

<span data-ttu-id="4f80b-282">Ez azért történik, mert a statikus fájlok különböző Django-alkalmazásokból származhatnak.</span><span class="sxs-lookup"><span data-stu-id="4f80b-282">This is done because static files may come from different Django 'apps'.</span></span> <span data-ttu-id="4f80b-283">A Django felügyeleti felületeiről származó statikus fájlok például a virtuális környezetben, egy Django-könyvtár almappájában találhatóak.</span><span class="sxs-lookup"><span data-stu-id="4f80b-283">For example, the static files from the Django admin interfaces are located in a Django library subfolder in the virtual environment.</span></span> <span data-ttu-id="4f80b-284">A jelen alkalmazásban megadott statikus fájlok itt találhatóak: `/app/static`.</span><span class="sxs-lookup"><span data-stu-id="4f80b-284">Static files defined by this application are located in `/app/static`.</span></span> <span data-ttu-id="4f80b-285">Ahogy egyre több Django-alkalmazást használ, egyre több helyen lesznek statikus fájlok.</span><span class="sxs-lookup"><span data-stu-id="4f80b-285">As you use more Django 'apps', you'll have static files located in multiple places.</span></span>

<span data-ttu-id="4f80b-286">Az alkalmazás hibakeresési módban történő futtatásakor az alkalmazás az eredeti helyükről szolgálja ki a statikus fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-286">When running the application in debug mode, the application serves the static files from their original location.</span></span>

<span data-ttu-id="4f80b-287">Az alkalmazás kiadási módban történő futtatásakor az alkalmazás **nem** szolgálja ki a statikus fájlokat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-287">When running the application in release mode, the application does **not** serve the static files.</span></span> <span data-ttu-id="4f80b-288">A fájlok kiszolgálása a webkiszolgáló feladata.</span><span class="sxs-lookup"><span data-stu-id="4f80b-288">It is the responsibility of the web server to serve the files.</span></span> <span data-ttu-id="4f80b-289">A jelen alkalmazás esetében az IIS a következő helyről szolgálja ki a statikus fájlokat: `/static`.</span><span class="sxs-lookup"><span data-stu-id="4f80b-289">For this application, IIS will serve the static files from `/static`.</span></span>

<span data-ttu-id="4f80b-290">A statikus fájlok összegyűjtése automatikusan történik az üzembe helyezési parancsfájl futtatása során, az előzőleg összegyűjtött fájlok törlésével.</span><span class="sxs-lookup"><span data-stu-id="4f80b-290">The collection of static files is done automatically as part of the deployment script, clearing previously collected files.</span></span> <span data-ttu-id="4f80b-291">Ez azt jelenti, hogy a fájlok összegyűjtése minden üzembe helyezés során megtörténik, ami lelassítja kissé a folyamatot, ugyanakkor biztosítja, hogy a potenciális biztonsági kockázatot jelentő elavult fájlok ne legyen elérhetőek.</span><span class="sxs-lookup"><span data-stu-id="4f80b-291">This means the collection occurs on every deployment, slowing down deployment a bit, but it ensures that obsolete files won't be available, avoiding a potential security issue.</span></span>

<span data-ttu-id="4f80b-292">Ha ki szeretné hagyni a Django-alkalmazáshoz a statikus fájlok összegyűjtését, használja ezt:</span><span class="sxs-lookup"><span data-stu-id="4f80b-292">If you want to skip collection of static files for your Django application:</span></span>

    \.skipDjango

<span data-ttu-id="4f80b-293">Ezt követően manuálisan kell elvégeznie a gyűjtést a helyi gépen:</span><span class="sxs-lookup"><span data-stu-id="4f80b-293">Then you'll need to do the collection manually on your local machine:</span></span>

    env\scripts\python manage.py collectstatic

<span data-ttu-id="4f80b-294">Ezután távolítsa el a `\static` mappát a `.gitignore` elemből, és vegye fel azt a Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="4f80b-294">Then remove the `\static` folder from `.gitignore` and add it to the Git repository.</span></span>

## <a name="troubleshooting---settings"></a><span data-ttu-id="4f80b-295">Hibaelhárítás – Beállítások</span><span class="sxs-lookup"><span data-stu-id="4f80b-295">Troubleshooting - Settings</span></span>
<span data-ttu-id="4f80b-296">Az alkalmazás számos beállítása módosítható a következő helyen: `DjangoWebProject/settings.py`.</span><span class="sxs-lookup"><span data-stu-id="4f80b-296">Various settings for the application can be changed in `DjangoWebProject/settings.py`.</span></span>

<span data-ttu-id="4f80b-297">A hibakeresési mód a fejlesztők kényelme érdekében engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4f80b-297">For developer convenience, debug mode is enabled.</span></span> <span data-ttu-id="4f80b-298">Ez szerencsés módon azzal is jár, hogy helyi futtatáskor anélkül tekintheti meg a képeket és az egyéb statikus tartalmakat, hogy a statikus fájlokat össze kellene gyűjtenie.</span><span class="sxs-lookup"><span data-stu-id="4f80b-298">One nice side effect of that is you'll be able to see images and other static content when running locally, without having to collect static files.</span></span>

<span data-ttu-id="4f80b-299">A hibakeresési mód letiltása:</span><span class="sxs-lookup"><span data-stu-id="4f80b-299">To disable debug mode:</span></span>

    DEBUG = False

<span data-ttu-id="4f80b-300">Ha a hibakeresés le van tiltva, az `ALLOWED_HOSTS` értékét módosítani kell úgy, hogy tartalmazza az Azure-gazdagép nevét.</span><span class="sxs-lookup"><span data-stu-id="4f80b-300">When debug is disabled, the value for `ALLOWED_HOSTS` needs to be updated to include the Azure host name.</span></span> <span data-ttu-id="4f80b-301">Példa:</span><span class="sxs-lookup"><span data-stu-id="4f80b-301">For example:</span></span>

    ALLOWED_HOSTS = (
        'pythonapp.azurewebsites.net',
    )

<span data-ttu-id="4f80b-302">vagy az engedélyezéshez:</span><span class="sxs-lookup"><span data-stu-id="4f80b-302">or to enable any:</span></span>

    ALLOWED_HOSTS = (
        '*',
    )

<span data-ttu-id="4f80b-303">A gyakorlatban lehetséges, hogy olyan összetettebb műveleteket is végre kíván hajtani, mint a hibakeresési és a kiadási mód közötti váltás, illetve a gazdagépnév beszerzése.</span><span class="sxs-lookup"><span data-stu-id="4f80b-303">In practice, you may want to do something more complex to deal with switching between debug and release mode, and getting the host name.</span></span>

<span data-ttu-id="4f80b-304">A környezeti változókat az Azure portál **CONFIGURE** (KONFIGURÁLÁS) lapjának **app settings** (alkalmazásbeállítások) szakaszában állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="4f80b-304">You can set environment variables through the Azure portal **CONFIGURE** page, in the **app settings** section.</span></span>  <span data-ttu-id="4f80b-305">Ez olyan értékek beállításakor lehet hasznos, amelyeket esetleg nem kíván megjeleníteni a forrásokban (például kapcsolati karakterláncok, jelszavak stb.), illetve amelyeket az Azure és a helyi gép esetében eltérően kíván megadni.</span><span class="sxs-lookup"><span data-stu-id="4f80b-305">This can be useful for setting values that you may not want to appear in the sources (connection strings, passwords, etc), or that you want to set differently between Azure and your local machine.</span></span> <span data-ttu-id="4f80b-306">A `settings.py` elemben az `os.getenv` használatával kérdezhetők le a környezeti változók.</span><span class="sxs-lookup"><span data-stu-id="4f80b-306">In `settings.py`, you can query the environment variables using `os.getenv`.</span></span>

## <a name="using-a-database"></a><span data-ttu-id="4f80b-307">Adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="4f80b-307">Using a Database</span></span>
<span data-ttu-id="4f80b-308">Az alkalmazásban egy sqlite-adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="4f80b-308">The database that is included with the application is a sqlite database.</span></span> <span data-ttu-id="4f80b-309">Fejlesztési célra ez egy kényelmes és hasznos alapértelmezett adatbázis, mivel nem igényel szinte semmilyen beállítást.</span><span class="sxs-lookup"><span data-stu-id="4f80b-309">This is a convenient and useful default database to use for development, as it requires almost no setup.</span></span> <span data-ttu-id="4f80b-310">Az adatbázis a projektmappa db.sqlite3 fájljában található.</span><span class="sxs-lookup"><span data-stu-id="4f80b-310">The database is stored in the db.sqlite3 file in the project folder.</span></span>

<span data-ttu-id="4f80b-311">Az Azure a Django-alkalmazásokból könnyen használható adatbázis-szolgáltatásokat kínál.</span><span class="sxs-lookup"><span data-stu-id="4f80b-311">Azure provides database services which are easy to use from a Django application.</span></span> <span data-ttu-id="4f80b-312">Az [SQL Database] és a [MySQL] Django-alkalmazásból történő használatához kapcsolódó oktatóanyagokban szerepelnek az adatbázis-szolgáltatás létrehozásához és az adatbázis beállításainak `DjangoWebProject/settings.py` elemben történő módosításához szükséges lépések, valamint a telepítéshez szükséges könyvtárak.</span><span class="sxs-lookup"><span data-stu-id="4f80b-312">Tutorials for using [SQL Database] and [MySQL] from a Django application show the steps necessary to create the database service, change the database settings in `DjangoWebProject/settings.py`, and the libraries required to install.</span></span>

<span data-ttu-id="4f80b-313">Természetesen ha Ön szeretné kezelni a saját adatbázis-kiszolgálóit, az Azure platformon futó Windows vagy Linux rendszerű virtuális gépek használatával megteheti azt.</span><span class="sxs-lookup"><span data-stu-id="4f80b-313">Of course, if you prefer to manage your own database servers, you can do so using Windows or Linux virtual machines running on Azure.</span></span>

## <a name="django-admin-interface"></a><span data-ttu-id="4f80b-314">A Django felügyeleti felülete</span><span class="sxs-lookup"><span data-stu-id="4f80b-314">Django Admin Interface</span></span>
<span data-ttu-id="4f80b-315">Amikor hozzákezd a modelljei felépítéséhez, fel fogja tölteni az adatbázist adatokkal.</span><span class="sxs-lookup"><span data-stu-id="4f80b-315">Once you start building your models, you'll want to populate the database with some data.</span></span> <span data-ttu-id="4f80b-316">A Django felügyeleti felületének használatával egyszerűen, interaktív módon vehet fel és szerkeszthet tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="4f80b-316">An easy way to do add and edit content interactively is to use the Django administration interface.</span></span>

<span data-ttu-id="4f80b-317">A felügyeleti felülethez tartozó kód az alkalmazásforrásokban megjegyzésként szerepel, de mivel egyértelműen meg van jelölve, könnyen engedélyezheti azt (keressen rá az „admin” kifejezésre).</span><span class="sxs-lookup"><span data-stu-id="4f80b-317">The code for the admin interface is commented out in the application sources, but it's clearly marked so you can easily enable it (search for 'admin').</span></span>

<span data-ttu-id="4f80b-318">Miután engedélyezte, szinkronizálja az adatbázist, futtassa az alkalmazást, majd lépjen a következő helyre: `/admin`.</span><span class="sxs-lookup"><span data-stu-id="4f80b-318">After it's enabled, synchronize the database, run the application and navigate to `/admin`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f80b-319">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f80b-319">Next Steps</span></span>
<span data-ttu-id="4f80b-320">Az alábbi hivatkozásokat követve tudhat meg többet a Django és a Python Tools for Visual Studio alkalmazásokról:</span><span class="sxs-lookup"><span data-stu-id="4f80b-320">Follow these links to learn more about Django and Python Tools for Visual Studio:</span></span>

* <span data-ttu-id="4f80b-321">[A Django dokumentációja]</span><span class="sxs-lookup"><span data-stu-id="4f80b-321">[Django Documentation]</span></span>
* <span data-ttu-id="4f80b-322">[a Python Tools for Visual Studio dokumentációjában]</span><span class="sxs-lookup"><span data-stu-id="4f80b-322">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="4f80b-323">Információk az SQL Database és a MySQL használatáról:</span><span class="sxs-lookup"><span data-stu-id="4f80b-323">For information on using SQL Database and MySQL:</span></span>

* <span data-ttu-id="4f80b-324">[Django and MySQL on Azure with Python Tools for Visual Studio] (Django és MySQL az Azure-ban, Python Tools for Visual Studio alkalmazással)</span><span class="sxs-lookup"><span data-stu-id="4f80b-324">[Django and MySQL on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="4f80b-325">[Django and SQL Database on Azure with Python Tools for Visual Studio] (Django és SQL Database az Azure-ban, Python Tools for Visual Studio alkalmazással)</span><span class="sxs-lookup"><span data-stu-id="4f80b-325">[Django and SQL Database on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="4f80b-326">További információ: [Python fejlesztői központban](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="4f80b-326">For more information, see the [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="4f80b-327">A változások</span><span class="sxs-lookup"><span data-stu-id="4f80b-327">What's changed</span></span>
* <span data-ttu-id="4f80b-328">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="4f80b-328">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!--Link references-->
[Django and MySQL on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-django-mysql.md (Django és MySQL az Azure-ban, Python Tools for Visual Studio alkalmazással)
[Django and SQL Database on Azure with Python Tools for Visual Studio]: web-sites-python-ptvs-django-sql.md (Django és SQL Database az Azure-ban, Python Tools for Visual Studio alkalmazással)
[SQL Database]: web-sites-python-ptvs-django-sql.md
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
[a Python Tools for Visual Studio dokumentációjában]: http://aka.ms/ptvsdocs
[A Django dokumentációja]: https://www.djangoproject.com/
