---
title: "az Azure-ban Flask aaaCreating webalkalmazások"
description: "Ez az oktatóanyag bemutatja a toorunning a Python webalkalmazás az Azure-on."
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
ms.openlocfilehash: 3362047b3106b4380b5971e47cbd8042d38a792b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-web-apps-with-flask-in-azure"></a><span data-ttu-id="de981-103">Webalkalmazások létrehozása a Flask az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="de981-103">Creating web apps with Flask in Azure</span></span>
<span data-ttu-id="de981-104">Ez az oktatóanyag leírja, hogyan tooget indulása Python [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="de981-104">This tutorial describes how tooget started running Python in [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>  <span data-ttu-id="de981-105">A Web Apps korlátozott ingyenes üzemeltetést és gyors üzembe helyezést kínál, ráadásul a Python alkalmazást is használhatja!</span><span class="sxs-lookup"><span data-stu-id="de981-105">Web Apps provides limited free hosting and rapid deployment, and you can use Python!</span></span>  <span data-ttu-id="de981-106">Az alkalmazás forgalmához igazítható, megváltoztathatja az toopaid üzemeltető, és integrálhatja az összes hello más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="de981-106">As your app grows, you can switch toopaid hosting, and you can also integrate with all of hello other Azure services.</span></span>

<span data-ttu-id="de981-107">Létre fog hozni egy alkalmazást, hello Flask webes keretrendszer használatával (lásd az oktatóanyag alternatív verzióit [Django](web-sites-python-create-deploy-django-app.md) és [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span><span class="sxs-lookup"><span data-stu-id="de981-107">You will create an application using hello Flask web framework (see alternate versions of this tutorial for [Django](web-sites-python-create-deploy-django-app.md) and [Bottle](web-sites-python-create-deploy-bottle-app.md)).</span></span>  <span data-ttu-id="de981-108">Az Azure katalógusában hello hello webhely létrehoz, Git-telepítésének beállítása, és helyileg hello tárház klónozása.</span><span class="sxs-lookup"><span data-stu-id="de981-108">You will create hello website from hello Azure gallery, set up Git deployment, and clone hello repository locally.</span></span>  <span data-ttu-id="de981-109">Ezután lesz hello alkalmazás helyileg történő futtatása, módosításokat, véglegesítse és küldje le őket tooAzure.</span><span class="sxs-lookup"><span data-stu-id="de981-109">Then you will run hello application locally, make changes, commit and push them tooAzure.</span></span>  <span data-ttu-id="de981-110">Hogyan hello oktatóanyag azt mutatja be toodo ezt a Windows vagy Mac/Linux.</span><span class="sxs-lookup"><span data-stu-id="de981-110">hello tutorial shows how toodo this from Windows or Mac/Linux.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

> [!NOTE]
> <span data-ttu-id="de981-111">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="de981-111">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="de981-112">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="de981-112">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="de981-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de981-113">Prerequisites</span></span>
* <span data-ttu-id="de981-114">Windows, Mac vagy Linux</span><span class="sxs-lookup"><span data-stu-id="de981-114">Windows, Mac or Linux</span></span>
* <span data-ttu-id="de981-115">Python 2.7 vagy 3.4</span><span class="sxs-lookup"><span data-stu-id="de981-115">Python 2.7 or 3.4</span></span>
* <span data-ttu-id="de981-116">setuptools, pip, virtualenv (csak Python 2.7)</span><span class="sxs-lookup"><span data-stu-id="de981-116">setuptools, pip, virtualenv (Python 2.7 only)</span></span>
* <span data-ttu-id="de981-117">Git</span><span class="sxs-lookup"><span data-stu-id="de981-117">Git</span></span>
* <span data-ttu-id="de981-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) – Megjegyzés: szabadon választható</span><span class="sxs-lookup"><span data-stu-id="de981-118">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) - Note: this is optional</span></span>

<span data-ttu-id="de981-119">**Megjegyzés:** A TFS-közzététel a Python-projektek esetében jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="de981-119">**Note**: TFS publishing is currently not supported for Python projects.</span></span>

### <a name="windows"></a><span data-ttu-id="de981-120">Windows</span><span class="sxs-lookup"><span data-stu-id="de981-120">Windows</span></span>
<span data-ttu-id="de981-121">Ha még nem telepítette a Python 2.7-es vagy 3.4-es (32 bites) verzióját, javasoljuk a [Python 2.7-hez készült Azure SDK] vagy a [Python 3.4-hez készült Azure SDK] Webplatform-telepítővel történő telepítését.</span><span class="sxs-lookup"><span data-stu-id="de981-121">If you don't already have Python 2.7 or 3.4 installed (32-bit), we recommend installing [Azure SDK for Python 2.7] or [Azure SDK for Python 3.4] using Web Platform Installer.</span></span>  <span data-ttu-id="de981-122">Ez a Python, setuptools, pip, virtualenv stb (az Azure-gazdagépeken hello telepített Python 32 bites) hello 32 bites verzióját telepíti.</span><span class="sxs-lookup"><span data-stu-id="de981-122">This installs hello 32-bit version of Python, setuptools, pip, virtualenv, etc (32-bit Python is what's installed on hello Azure host machines).</span></span>  <span data-ttu-id="de981-123">Alternatív megoldásként a Python eszközt a [python.org] webhelyről is beszerezheti.</span><span class="sxs-lookup"><span data-stu-id="de981-123">Alternatively, you can get Python from [python.org].</span></span>

<span data-ttu-id="de981-124">A Git esetében a [Git for Windows] vagy a [GitHub for Windows] használatát javasoljuk.</span><span class="sxs-lookup"><span data-stu-id="de981-124">For Git, we recommend [Git for Windows] or [GitHub for Windows].</span></span>  <span data-ttu-id="de981-125">Ha a Visual Studio használata esetén használhatja hello integrált Git-támogatást.</span><span class="sxs-lookup"><span data-stu-id="de981-125">If you use Visual Studio, you can use hello integrated Git support.</span></span>

<span data-ttu-id="de981-126">Szintén javasoljuk a [Python Tools 2.2 for Visual Studio] telepítését.</span><span class="sxs-lookup"><span data-stu-id="de981-126">We also recommend installing [Python Tools 2.2 for Visual Studio].</span></span>  <span data-ttu-id="de981-127">Ez nem kötelező, de ha rendelkezik [Visual Studio], beleértve a hello ingyenes Visual Studio Community 2013 vagy Visual Studio Express 2013 for Web alkalmazásokat, és ekkor kap egy nagyszerű Python IDE.</span><span class="sxs-lookup"><span data-stu-id="de981-127">This is optional, but if you have [Visual Studio], including hello free Visual Studio Community 2013 or Visual Studio Express 2013 for Web, then this will give you a great Python IDE.</span></span>

### <a name="maclinux"></a><span data-ttu-id="de981-128">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="de981-128">Mac/Linux</span></span>
<span data-ttu-id="de981-129">A Python és a Git alkalmazásoknak már telepítve kell lenniük, de győződjön meg arról, hogy a Python verziószáma 2.7-es vagy 3.4-es.</span><span class="sxs-lookup"><span data-stu-id="de981-129">You should have Python and Git already installed, but make sure you have either Python 2.7 or 3.4.</span></span>

## <a name="web-app-create-on-hello-azure-portal"></a><span data-ttu-id="de981-130">A webalkalmazás létrehozása hello Azure portál</span><span class="sxs-lookup"><span data-stu-id="de981-130">Web app create on hello Azure Portal</span></span>
<span data-ttu-id="de981-131">hello saját alkalmazása létrehozásának első lépése az toocreate hello hello webalkalmazás [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="de981-131">hello first step in creating your app is toocreate hello web app via hello [Azure Portal](https://portal.azure.com).</span></span> 

1. <span data-ttu-id="de981-132">Jelentkezzen be hello Azure Portal, majd kattintson a hello **új** hello bal alsó sarokban gombjára.</span><span class="sxs-lookup"><span data-stu-id="de981-132">Log into hello Azure Portal and click hello **NEW** button in hello bottom left corner.</span></span> 
2. <span data-ttu-id="de981-133">Kattintson a **Web + mobil** elemre.</span><span class="sxs-lookup"><span data-stu-id="de981-133">Click **Web + Mobile**.</span></span>
3. <span data-ttu-id="de981-134">Hello a keresőmezőbe írja be a "python".</span><span class="sxs-lookup"><span data-stu-id="de981-134">In hello search box, type "python".</span></span>
4. <span data-ttu-id="de981-135">Hello keresési eredmények között, válassza ki a **Flask**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="de981-135">In hello search results, select **Flask**, then click **Create**.</span></span>
5. <span data-ttu-id="de981-136">Hello új Flask alkalmazás, például egy új App Service-csomag és egy új erőforráscsoport létrehozásával az konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="de981-136">Configure hello new Flask app, such as creating a new App Service plan and a new resource group for it.</span></span> <span data-ttu-id="de981-137">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="de981-137">Then, click **Create**.</span></span>
6. <span data-ttu-id="de981-138">Az újonnan létrehozott webalkalmazáshoz tartozó Git-közzététel konfigurálása: hello utasításokat követve [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="de981-138">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

## <a name="application-overview"></a><span data-ttu-id="de981-139">Az alkalmazás áttekintése</span><span class="sxs-lookup"><span data-stu-id="de981-139">Application Overview</span></span>
### <a name="git-repository-contents"></a><span data-ttu-id="de981-140">A Git-tárház tartalma</span><span class="sxs-lookup"><span data-stu-id="de981-140">Git repository contents</span></span>
<span data-ttu-id="de981-141">Itt a hello kezdeti Git-tárházban, amely klónozását fogjuk elvégezni hello a következő szakaszban találhat hello fájlok nyújt áttekintést.</span><span class="sxs-lookup"><span data-stu-id="de981-141">Here's an overview of hello files you'll find in hello initial Git repository, which we'll clone in hello next section.</span></span>

    \FlaskWebProject\__init__.py
    \FlaskWebProject\views.py
    \FlaskWebProject\static\content\
    \FlaskWebProject\static\fonts\
    \FlaskWebProject\static\scripts\
    \FlaskWebProject\templates\about.html
    \FlaskWebProject\templates\contact.html
    \FlaskWebProject\templates\index.html
    \FlaskWebProject\templates\layout.html

<span data-ttu-id="de981-142">Hello alkalmazás fő forrásai.</span><span class="sxs-lookup"><span data-stu-id="de981-142">Main sources for hello application.</span></span>  <span data-ttu-id="de981-143">3 oldalból (index, névjegy, kapcsolat) áll egy fő elrendezéssel.</span><span class="sxs-lookup"><span data-stu-id="de981-143">Consists of 3 pages (index, about, contact) with a master layout.</span></span>  <span data-ttu-id="de981-144">Példák statikus tartalomra és parancsfájlokra: bootstrap, jquery, modernizr és respond.</span><span class="sxs-lookup"><span data-stu-id="de981-144">Static content and scripts include bootstrap, jquery, modernizr and respond.</span></span>

    \runserver.py

<span data-ttu-id="de981-145">Helyi fejlesztési kiszolgáló támogatása.</span><span class="sxs-lookup"><span data-stu-id="de981-145">Local development server support.</span></span> <span data-ttu-id="de981-146">Az toorun hello alkalmazás helyileg használatához.</span><span class="sxs-lookup"><span data-stu-id="de981-146">Use this toorun hello application locally.</span></span>

    \FlaskWebProject.pyproj
    \FlaskWebProject.sln

<span data-ttu-id="de981-147">A [Python Tools for Visual Studio] alkalmazással használható projektfájlok.</span><span class="sxs-lookup"><span data-stu-id="de981-147">Project files for use with [Python Tools for Visual Studio].</span></span>

    \ptvs_virtualenv_proxy.py

<span data-ttu-id="de981-148">IIS-proxy a virtuális környezetekhez, valamint távoli hibaelhárítási támogatás a PTVS-hez.</span><span class="sxs-lookup"><span data-stu-id="de981-148">IIS proxy for virtual environments and PTVS remote debugging support.</span></span>

    \requirements.txt

<span data-ttu-id="de981-149">Az alkalmazás számára szükséges külső csomagok.</span><span class="sxs-lookup"><span data-stu-id="de981-149">External packages needed by this application.</span></span> <span data-ttu-id="de981-150">hello telepítési parancsfájl elvégzi az ebben a fájlban felsorolt csomagok telepítése hello.</span><span class="sxs-lookup"><span data-stu-id="de981-150">hello deployment script will pip install hello packages listed in this file.</span></span>

    \web.2.7.config
    \web.3.4.config

<span data-ttu-id="de981-151">IIS-konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="de981-151">IIS configuration files.</span></span>  <span data-ttu-id="de981-152">hello telepítési parancsfájl hello megfelelő web.x.y.config használja, és web.config fájlként másolásához.</span><span class="sxs-lookup"><span data-stu-id="de981-152">hello deployment script will use hello appropriate web.x.y.config and copy it as web.config.</span></span>

### <a name="optional-files---customizing-deployment"></a><span data-ttu-id="de981-153">Opcionális fájlok – A telepítés testre szabása</span><span class="sxs-lookup"><span data-stu-id="de981-153">Optional files - Customizing deployment</span></span>
[!INCLUDE [web-sites-python-customizing-deployment](../../includes/web-sites-python-customizing-deployment.md)]

### <a name="optional-files---python-runtime"></a><span data-ttu-id="de981-154">Opcionális fájlok – Python-futtatókörnyezet</span><span class="sxs-lookup"><span data-stu-id="de981-154">Optional files - Python runtime</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

### <a name="additional-files-on-server"></a><span data-ttu-id="de981-155">További fájlok a kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="de981-155">Additional files on server</span></span>
<span data-ttu-id="de981-156">Egyes fájlok hello kiszolgálón léteznek, de nem kerülnek toohello git-tárház.</span><span class="sxs-lookup"><span data-stu-id="de981-156">Some files exist on hello server but are not added toohello git repository.</span></span>  <span data-ttu-id="de981-157">Ezek hello telepítési parancsfájl hozza létre.</span><span class="sxs-lookup"><span data-stu-id="de981-157">These are created by hello deployment script.</span></span>

    \web.config

<span data-ttu-id="de981-158">IIS-konfigurációs fájl.</span><span class="sxs-lookup"><span data-stu-id="de981-158">IIS configuration file.</span></span>  <span data-ttu-id="de981-159">Minden telepítéshez a web.x.y.config fájlból jön létre.</span><span class="sxs-lookup"><span data-stu-id="de981-159">Created from web.x.y.config on every deployment.</span></span>

    \env\

<span data-ttu-id="de981-160">Python virtuális környezet.</span><span class="sxs-lookup"><span data-stu-id="de981-160">Python virtual environment.</span></span>  <span data-ttu-id="de981-161">Telepítése során létrehozott Ha kompatibilis virtuális környezet már nem létezik a hello alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="de981-161">Created during deployment if a compatible virtual environment doesn't already exist on hello app.</span></span>  <span data-ttu-id="de981-162">A requirements.txt fájlban felsorolt csomagok telepítése a pip, de a pip kihagyja a telepítést, ha már telepítve vannak a hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="de981-162">Packages listed in requirements.txt are pip installed, but pip will skip installation if hello packages are already installed.</span></span>

<span data-ttu-id="de981-163">hello következő 3 szakaszok mutatják be, hogyan tooproceed hello a webalkalmazás-e a fejlesztés alatt 3 különböző környezetekben:</span><span class="sxs-lookup"><span data-stu-id="de981-163">hello next 3 sections describe how tooproceed with hello web app development under 3 different environments:</span></span>

* <span data-ttu-id="de981-164">Windows, Python Tools for Visual Studio alkalmazással</span><span class="sxs-lookup"><span data-stu-id="de981-164">Windows, with Python Tools for Visual Studio</span></span>
* <span data-ttu-id="de981-165">Windows, parancssorral</span><span class="sxs-lookup"><span data-stu-id="de981-165">Windows, with command line</span></span>
* <span data-ttu-id="de981-166">Mac/Linux, parancssorral</span><span class="sxs-lookup"><span data-stu-id="de981-166">Mac/Linux, with command line</span></span>

## <a name="web-app-development---windows---python-tools-for-visual-studio"></a><span data-ttu-id="de981-167">Webalkalmazás-fejlesztés – Windows – Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de981-167">Web app development - Windows - Python Tools for Visual Studio</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="de981-168">Hello tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="de981-168">Clone hello repository</span></span>
<span data-ttu-id="de981-169">Első lépésben klónozza hello tárházat hello található URL-cím hello Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="de981-169">First, clone hello repository using hello URL provided on hello Azure Portal.</span></span> <span data-ttu-id="de981-170">További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="de981-170">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

<span data-ttu-id="de981-171">Nyissa meg a hello megoldásfájlt (.sln), hello hello tárház gyökérkönyvtárában található.</span><span class="sxs-lookup"><span data-stu-id="de981-171">Open hello solution file (.sln) that is included in hello root of hello repository.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-solution-flask.png)

### <a name="create-virtual-environment"></a><span data-ttu-id="de981-172">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="de981-172">Create virtual environment</span></span>
<span data-ttu-id="de981-173">Most helyi telepítéshez tartozó virtuális környezetet hozunk létre.</span><span class="sxs-lookup"><span data-stu-id="de981-173">Now we'll create a virtual environment for local development.</span></span>  <span data-ttu-id="de981-174">Kattintson a jobb gombbal a **Python Environments** (Python-környezetek) elemre, majd válassza az **Add Virtual Environment...** (Virtuális környezet hozzáadása...) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="de981-174">Right-click on **Python Environments** select **Add Virtual Environment...**.</span></span>

* <span data-ttu-id="de981-175">Ellenőrizze, hogy hello név hello környezet `env`.</span><span class="sxs-lookup"><span data-stu-id="de981-175">Make sure hello name of hello environment is `env`.</span></span>
* <span data-ttu-id="de981-176">Válassza ki a hello alapszintű értelmezőt.</span><span class="sxs-lookup"><span data-stu-id="de981-176">Select hello base interpreter.</span></span>  <span data-ttu-id="de981-177">Győződjön meg arról, hogy toouse hello a webalkalmazás kiválasztott Python ugyanazon verzióját (a runtime.txt vagy hello **Alkalmazásbeállítások** panelen található webalkalmazását az Azure portál hello).</span><span class="sxs-lookup"><span data-stu-id="de981-177">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>
* <span data-ttu-id="de981-178">Ellenőrizze, hogy hello beállítás toodownload és telepítési csomagok be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="de981-178">Make sure hello option toodownload and install packages is checked.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-add-virtual-env-27.png)

<span data-ttu-id="de981-179">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="de981-179">Click **Create**.</span></span>  <span data-ttu-id="de981-180">Ezzel hello virtuális környezet létrehozása, és telepítse a Requirements.txt fájlban található függőségek.</span><span class="sxs-lookup"><span data-stu-id="de981-180">This will create hello virtual environment, and install dependencies listed in requirements.txt.</span></span>

### <a name="run-using-development-server"></a><span data-ttu-id="de981-181">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="de981-181">Run using development server</span></span>
<span data-ttu-id="de981-182">Nyomja le az F5 toostart Hibakeresés és a webböngészőjében automatikusan toohello helyileg futtatott lap ekkor megnyílik.</span><span class="sxs-lookup"><span data-stu-id="de981-182">Press F5 toostart debugging, and your web browser will open automatically toohello page running locally.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

<span data-ttu-id="de981-183">Töréspontokat állíthat hello adatforrások, tekintse meg a windows hello, stb.  Lásd: hello [Python Tools for Visual Studio dokumentációjában] hello további információt a különböző szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="de981-183">You can set breakpoints in hello sources, use hello watch windows, etc.  See hello [Python Tools for Visual Studio Documentation] for more information on hello various features.</span></span>

### <a name="make-changes"></a><span data-ttu-id="de981-184">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="de981-184">Make changes</span></span>
<span data-ttu-id="de981-185">Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.</span><span class="sxs-lookup"><span data-stu-id="de981-185">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="de981-186">A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="de981-186">After you've tested your changes, commit them toohello Git repository:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-commit-flask.png)

### <a name="install-more-packages"></a><span data-ttu-id="de981-187">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-187">Install more packages</span></span>
<span data-ttu-id="de981-188">Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="de981-188">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="de981-189">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="de981-189">You can install additional packages using pip.</span></span>  <span data-ttu-id="de981-190">tooinstall egy csomagot, kattintson a jobb gombbal a hello virtuális környezetet, és válassza **Python-csomag telepítése**.</span><span class="sxs-lookup"><span data-stu-id="de981-190">tooinstall a package, right-click on hello virtual environment and select **Install Python Package**.</span></span>

<span data-ttu-id="de981-191">Például tooinstall hello hozzáférést tud biztosítani tooAzure storage, service bus és további Azure-szolgáltatásokhoz, írja be Pythonhoz készült Azure SDK `azure`:</span><span class="sxs-lookup"><span data-stu-id="de981-191">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, enter `azure`:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-install-package-dialog.png)

<span data-ttu-id="de981-192">Kattintson a jobb gombbal a hello virtuális környezetre, és válassza ki **készítése a requirements.txt** tooupdate Requirements.txt fájlt.</span><span class="sxs-lookup"><span data-stu-id="de981-192">Right-click on hello virtual environment and select **Generate requirements.txt** tooupdate requirements.txt.</span></span>

<span data-ttu-id="de981-193">Ezt követően véglegesítse hello módosítások toorequirements.txt toohello Git-tárházba.</span><span class="sxs-lookup"><span data-stu-id="de981-193">Then, commit hello changes toorequirements.txt toohello Git repository.</span></span>

### <a name="deploy-tooazure"></a><span data-ttu-id="de981-194">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-194">Deploy tooAzure</span></span>
<span data-ttu-id="de981-195">tootrigger a telepítést, kattintson a **szinkronizálási** vagy **leküldéses**.</span><span class="sxs-lookup"><span data-stu-id="de981-195">tootrigger a deployment, click on **Sync** or **Push**.</span></span>  <span data-ttu-id="de981-196">A szinkronizálás lekérési és leküldési funkcióval is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="de981-196">Sync does both a push and a pull.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/ptvs-git-push.png)

<span data-ttu-id="de981-197">hello első központi telepítési időt vesz igénybe, mivel ekkor létrehoz egy virtuális környezethez, a csomagok telepítése, stb.</span><span class="sxs-lookup"><span data-stu-id="de981-197">hello first deployment will take some time, as it will create a virtual environment, install packages, etc.</span></span>

<span data-ttu-id="de981-198">A Visual Studio nem jeleníti meg hello hello központi telepítés végrehajtási állapotát.</span><span class="sxs-lookup"><span data-stu-id="de981-198">Visual Studio doesn't show hello progress of hello deployment.</span></span>  <span data-ttu-id="de981-199">Ha szeretné tooreview hello kimeneti, ld. hello [hibaelhárítás – üzembe helyezés](#troubleshooting-deployment).</span><span class="sxs-lookup"><span data-stu-id="de981-199">If you'd like tooreview hello output, see hello section on [Troubleshooting - Deployment](#troubleshooting-deployment).</span></span>

<span data-ttu-id="de981-200">Keresse meg a toohello Azure URL-cím tooview a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="de981-200">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---windows---command-line"></a><span data-ttu-id="de981-201">Webalkalmazás-fejlesztés – Windows – parancssor</span><span class="sxs-lookup"><span data-stu-id="de981-201">Web app development - Windows - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="de981-202">Hello tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="de981-202">Clone hello repository</span></span>
<span data-ttu-id="de981-203">Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="de981-203">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="de981-204">További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="de981-204">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="de981-205">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="de981-205">Create virtual environment</span></span>
<span data-ttu-id="de981-206">Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház).</span><span class="sxs-lookup"><span data-stu-id="de981-206">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span>  <span data-ttu-id="de981-207">Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.</span><span class="sxs-lookup"><span data-stu-id="de981-207">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="de981-208">Győződjön meg arról, hogy toouse hello a webalkalmazás kiválasztott Python ugyanazon verzióját (a runtime.txt vagy hello **Alkalmazásbeállítások** panelen található webalkalmazását az Azure portál hello).</span><span class="sxs-lookup"><span data-stu-id="de981-208">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="de981-209">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="de981-209">For Python 2.7:</span></span>

    c:\python27\python.exe -m virtualenv env

<span data-ttu-id="de981-210">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="de981-210">For Python 3.4:</span></span>

    c:\python34\python.exe -m venv env

<span data-ttu-id="de981-211">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="de981-211">Install any external packages required by your application.</span></span> <span data-ttu-id="de981-212">A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:</span><span class="sxs-lookup"><span data-stu-id="de981-212">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env\scripts\pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="de981-213">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="de981-213">Run using development server</span></span>
<span data-ttu-id="de981-214">A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="de981-214">You can launch hello application under a development server with hello following command:</span></span>

    env\scripts\python runserver.py

<span data-ttu-id="de981-215">hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="de981-215">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-run-local-flask.png)

<span data-ttu-id="de981-216">Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="de981-216">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/windows-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="de981-217">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="de981-217">Make changes</span></span>
<span data-ttu-id="de981-218">Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.</span><span class="sxs-lookup"><span data-stu-id="de981-218">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="de981-219">A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="de981-219">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="de981-220">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-220">Install more packages</span></span>
<span data-ttu-id="de981-221">Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="de981-221">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="de981-222">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="de981-222">You can install additional packages using pip.</span></span>  <span data-ttu-id="de981-223">Például tooinstall hello Azure SDK for Python, amely lehetővé teszi az elérhető tooAzure storage, service bus és további Azure-szolgáltatásokhoz, típus:</span><span class="sxs-lookup"><span data-stu-id="de981-223">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env\scripts\pip install azure

<span data-ttu-id="de981-224">Győződjön meg arról, hogy tooupdate Requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="de981-224">Make sure tooupdate requirements.txt:</span></span>

    env\scripts\pip freeze > requirements.txt

<span data-ttu-id="de981-225">Hello változtatások véglegesítése a határidő:</span><span class="sxs-lookup"><span data-stu-id="de981-225">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="de981-226">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-226">Deploy tooAzure</span></span>
<span data-ttu-id="de981-227">a központi telepítés tootrigger, leküldéses hello tooAzure módosítja:</span><span class="sxs-lookup"><span data-stu-id="de981-227">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="de981-228">Hello a telepítési parancsfájl, beleértve a virtuális környezet létrehozását, a csomagok telepítését és a Web.config fájl létrehozását hello kimenetet fog látni.</span><span class="sxs-lookup"><span data-stu-id="de981-228">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="de981-229">Keresse meg a toohello Azure URL-cím tooview a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="de981-229">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="web-app-development---maclinux---command-line"></a><span data-ttu-id="de981-230">Webalkalmazás-fejlesztés – Mac/Linux – parancssor</span><span class="sxs-lookup"><span data-stu-id="de981-230">Web app development - Mac/Linux - command line</span></span>
### <a name="clone-hello-repository"></a><span data-ttu-id="de981-231">Hello tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="de981-231">Clone hello repository</span></span>
<span data-ttu-id="de981-232">Először klónozza hello tárházat hello található URL-cím hello Azure portál használatával, és adja hozzá a távoli hello Azure-tárházat.</span><span class="sxs-lookup"><span data-stu-id="de981-232">First, clone hello repository using hello URL provided on hello Azure Portal, and add hello Azure repository as a remote.</span></span> <span data-ttu-id="de981-233">További információkért lásd: [helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="de981-233">For more information, see [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span>

    git clone <repo-url>
    cd <repo-folder>
    git remote add azure <repo-url> 

### <a name="create-virtual-environment"></a><span data-ttu-id="de981-234">Virtuális környezet létrehozása</span><span class="sxs-lookup"><span data-stu-id="de981-234">Create virtual environment</span></span>
<span data-ttu-id="de981-235">Létrehozunk egy új virtuális környezet fejlesztési célra (nem adja hozzá azt toohello tárház).</span><span class="sxs-lookup"><span data-stu-id="de981-235">We'll create a new virtual environment for development purposes (do not add it toohello repository).</span></span>  <span data-ttu-id="de981-236">Így hello alkalmazás dolgozó összes fejlesztő hoz létre a saját helyi, amelyek Python virtuális környezetei nem helyezhetőek át.</span><span class="sxs-lookup"><span data-stu-id="de981-236">Virtual environments in Python are not relocatable, so every developer working on hello application will create their own locally.</span></span>

<span data-ttu-id="de981-237">Győződjön meg arról, hogy toouse hello a webalkalmazás kiválasztott Python ugyanazon verzióját (a runtime.txt vagy hello **Alkalmazásbeállítások** panelen található webalkalmazását az Azure portál hello).</span><span class="sxs-lookup"><span data-stu-id="de981-237">Make sure toouse hello same version of Python that is selected for your web app (in runtime.txt or hello **Application Settings** blade of your web app in hello Azure Portal).</span></span>

<span data-ttu-id="de981-238">Python 2.7 esetén:</span><span class="sxs-lookup"><span data-stu-id="de981-238">For Python 2.7:</span></span>

    python -m virtualenv env

<span data-ttu-id="de981-239">Python 3.4 esetén:</span><span class="sxs-lookup"><span data-stu-id="de981-239">For Python 3.4:</span></span>

    python -m venv env
<span data-ttu-id="de981-240">vagy pyvenv env</span><span class="sxs-lookup"><span data-stu-id="de981-240">or pyvenv env</span></span>

<span data-ttu-id="de981-241">Telepítse az alkalmazáshoz esetlegesen szükséges külső csomagokat.</span><span class="sxs-lookup"><span data-stu-id="de981-241">Install any external packages required by your application.</span></span> <span data-ttu-id="de981-242">A virtuális környezetben használható hello a requirements.txt fájl hello gyökerében hello tárház tooinstall hello csomagok:</span><span class="sxs-lookup"><span data-stu-id="de981-242">You can use hello requirements.txt file at hello root of hello repository tooinstall hello packages in your virtual environment:</span></span>

    env/bin/pip install -r requirements.txt

### <a name="run-using-development-server"></a><span data-ttu-id="de981-243">Futtatás fejlesztési kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="de981-243">Run using development server</span></span>
<span data-ttu-id="de981-244">A parancs a következő hello indítja el hello alkalmazást egy fejlesztési kiszolgáló alatt:</span><span class="sxs-lookup"><span data-stu-id="de981-244">You can launch hello application under a development server with hello following command:</span></span>

    env/bin/python runserver.py

<span data-ttu-id="de981-245">hello konzol megjeleníti hello URL-cím és port hello kiszolgáló figyel:</span><span class="sxs-lookup"><span data-stu-id="de981-245">hello console will display hello URL and port hello server listens to:</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-run-local-flask.png)

<span data-ttu-id="de981-246">Ezután nyissa meg a webes böngésző toothat URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="de981-246">Then, open your web browser toothat URL.</span></span>

![](./media/web-sites-python-create-deploy-flask-app/mac-browser-flask.png)

### <a name="make-changes"></a><span data-ttu-id="de981-247">Módosítások végrehajtása</span><span class="sxs-lookup"><span data-stu-id="de981-247">Make changes</span></span>
<span data-ttu-id="de981-248">Most kísérletezhet azáltal, hogy a módosítások toohello alkalmazásforrásokon és sablonok.</span><span class="sxs-lookup"><span data-stu-id="de981-248">Now you can experiment by making changes toohello application sources and/or templates.</span></span>

<span data-ttu-id="de981-249">A módosítások tesztelését, követően véglegesítse azokat toohello Git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="de981-249">After you've tested your changes, commit them toohello Git repository:</span></span>

    git add <modified-file>
    git commit -m "<commit-comment>"

### <a name="install-more-packages"></a><span data-ttu-id="de981-250">További csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-250">Install more packages</span></span>
<span data-ttu-id="de981-251">Az alkalmazás a Python és a Flask kívüli függőségekkel is rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="de981-251">Your application may have dependencies beyond Python and Flask.</span></span>

<span data-ttu-id="de981-252">A pip használatával további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="de981-252">You can install additional packages using pip.</span></span>  <span data-ttu-id="de981-253">Például tooinstall hello Azure SDK for Python, amely lehetővé teszi az elérhető tooAzure storage, service bus és további Azure-szolgáltatásokhoz, típus:</span><span class="sxs-lookup"><span data-stu-id="de981-253">For example, tooinstall hello Azure SDK for Python, which gives you access tooAzure storage, service bus and other Azure services, type:</span></span>

    env/bin/pip install azure

<span data-ttu-id="de981-254">Győződjön meg arról, hogy tooupdate Requirements.txt fájlt:</span><span class="sxs-lookup"><span data-stu-id="de981-254">Make sure tooupdate requirements.txt:</span></span>

    env/bin/pip freeze > requirements.txt

<span data-ttu-id="de981-255">Hello változtatások véglegesítése a határidő:</span><span class="sxs-lookup"><span data-stu-id="de981-255">Commit hello changes:</span></span>

    git add requirements.txt
    git commit -m "Added azure package"

### <a name="deploy-tooazure"></a><span data-ttu-id="de981-256">TooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-256">Deploy tooAzure</span></span>
<span data-ttu-id="de981-257">a központi telepítés tootrigger, leküldéses hello tooAzure módosítja:</span><span class="sxs-lookup"><span data-stu-id="de981-257">tootrigger a deployment, push hello changes tooAzure:</span></span>

    git push azure master

<span data-ttu-id="de981-258">Hello a telepítési parancsfájl, beleértve a virtuális környezet létrehozását, a csomagok telepítését és a Web.config fájl létrehozását hello kimenetet fog látni.</span><span class="sxs-lookup"><span data-stu-id="de981-258">You will see hello output of hello deployment script, including virtual environment creation, installation of packages, creation of web.config.</span></span>

<span data-ttu-id="de981-259">Keresse meg a toohello Azure URL-cím tooview a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="de981-259">Browse toohello Azure URL tooview your changes.</span></span>

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="de981-260">Hibaelhárítás – Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="de981-260">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="de981-261">Hibaelhárítás – Virtuális környezet</span><span class="sxs-lookup"><span data-stu-id="de981-261">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="de981-262">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de981-262">Next Steps</span></span>
<span data-ttu-id="de981-263">Hajtsa végre a Flask és a Python Tools kapcsolatos további hivatkozások toolearn Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="de981-263">Follow these links toolearn more about Flask and Python Tools for Visual Studio:</span></span> 

* <span data-ttu-id="de981-264">[Flask-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="de981-264">[Flask Documentation]</span></span>
* <span data-ttu-id="de981-265">[Python Tools for Visual Studio dokumentációjában]</span><span class="sxs-lookup"><span data-stu-id="de981-265">[Python Tools for Visual Studio Documentation]</span></span>

<span data-ttu-id="de981-266">Információk az Azure Table Storage és a MongoDB használatával:</span><span class="sxs-lookup"><span data-stu-id="de981-266">For information on using Azure Table Storage and MongoDB:</span></span>

* <span data-ttu-id="de981-267">[Flask és a Python Tools for Visual Studio Azure MongoDB]</span><span class="sxs-lookup"><span data-stu-id="de981-267">[Flask and MongoDB on Azure with Python Tools for Visual Studio]</span></span>
* <span data-ttu-id="de981-268">[Flask és az Azure Table Storage Azure Python Tools for Visual Studio]</span><span class="sxs-lookup"><span data-stu-id="de981-268">[Flask and Azure Table Storage on Azure with Python Tools for Visual Studio]</span></span>

<span data-ttu-id="de981-269">További információkért lásd még: hello [Python fejlesztői központ](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="de981-269">For more information, see also hello [Python Developer Center](/develop/python/).</span></span>

## <a name="whats-changed"></a><span data-ttu-id="de981-270">A változások</span><span class="sxs-lookup"><span data-stu-id="de981-270">What's changed</span></span>
* <span data-ttu-id="de981-271">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="de981-271">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
[Python Tools for Visual Studio dokumentációjában]: http://aka.ms/ptvsdocs
[Flask-dokumentáció]: http://flask.pocoo.org/ 

