---
title: "Python Flask-webalkalmazási oktatóanyag az Azure Cosmos DB-hez | Microsoft Docs"
description: "Egy adatbázis-oktatóanyag áttekintésével megtudhatja, hogyan tárolhatja és érheti el az Azure-ban tárolt Python Flask-webalkalmazások adatait az Azure Cosmos DB használatával. Alkalmazásfejlesztési megoldások keresése."
keywords: "Alkalmazásfejlesztés, python flask, python-webalkalmazás, python-webfejlesztés"
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 20ebec18-67c2-4988-a760-be7c30cfb745
ms.service: cosmos-db
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 08/09/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed5284b5a265840c43dbc9890082a7c038d22975
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a><span data-ttu-id="6bfdd-105">Python Flask-webalkalmazás létrehozása az Azure Cosmos DB használatával</span><span class="sxs-lookup"><span data-stu-id="6bfdd-105">Build a Python Flask web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6bfdd-106">.NET</span><span class="sxs-lookup"><span data-stu-id="6bfdd-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="6bfdd-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="6bfdd-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="6bfdd-108">Java</span><span class="sxs-lookup"><span data-stu-id="6bfdd-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="6bfdd-109">Python</span><span class="sxs-lookup"><span data-stu-id="6bfdd-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="6bfdd-110">Ez az oktatóanyag bemutatja, hogyan tárolhatja és érheti el az Azure-ban tárolt Python-webalkalmazás adatait az Azure Cosmos DB használatával, valamint feltételezi, hogy már rendelkezik némi tapasztalattal a Python és az Azure Websites használatát illetően.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-110">This tutorial shows you how to use Azure Cosmos DB to store and access data from a Python web application hosted on Azure and presumes that you have some prior experience using Python and Azure websites.</span></span>

<span data-ttu-id="6bfdd-111">Az adatbázis-oktatóanyag az alábbiakat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-111">This database tutorial covers:</span></span>

1. <span data-ttu-id="6bfdd-112">Létrehozása, és üzembe helyezése egy Cosmos-DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-112">Creating and provisioning a Cosmos DB account.</span></span>
2. <span data-ttu-id="6bfdd-113">A Python Flask-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-113">Creating a Python Flask application.</span></span>
3. <span data-ttu-id="6bfdd-114">A Cosmos DB a webalkalmazásból történő használata, valamint az ahhoz való csatlakozás.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-114">Connecting to and using Cosmos DB from your web application.</span></span>
4. <span data-ttu-id="6bfdd-115">Az Azure webes alkalmazás központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-115">Deploying the web application to Azure.</span></span>

<span data-ttu-id="6bfdd-116">Az oktatóanyag utasításait követve egy egyszerű szavazóalkalmazást fog létrehozni, amely lehetővé teszi, hogy leadja a voksát egy szavazáson.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-116">By following this tutorial, you will build a simple voting application that allows you to vote for a poll.</span></span>

![Az adatbázis-oktatóprogram során létrehozott szavazóalkalmazást képernyőfelvétele](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a><span data-ttu-id="6bfdd-118">Az adatbázis-oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="6bfdd-118">Database tutorial prerequisites</span></span>
<span data-ttu-id="6bfdd-119">A jelen cikkben lévő utasítások követése előtt rendelkeznie kell a következőkkel:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-119">Before following the instructions in this article, you should ensure that you have the following installed:</span></span>

* <span data-ttu-id="6bfdd-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-120">An active Azure account.</span></span> <span data-ttu-id="6bfdd-121">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="6bfdd-122">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
 
    <span data-ttu-id="6bfdd-123">VAGY</span><span class="sxs-lookup"><span data-stu-id="6bfdd-123">OR</span></span> 

    <span data-ttu-id="6bfdd-124">Az [Azure Cosmos DB Emulator](local-emulator.md) helyi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-124">A local installation of the [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="6bfdd-125">[A Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="6bfdd-126">[A Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-126">[Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span></span>  
* <span data-ttu-id="6bfdd-127">[Python 2.7-hez készült Microsoft Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-127">[Microsoft Azure SDK for Python 2.7](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="6bfdd-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6bfdd-129">Ha először telepíti a Python 2.7, győződjön meg arról, hogy testreszabása Python 2.7.13 képernyőjén választja **python.exe hozzáadása az elérési út**.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-129">If you are installing Python 2.7 for the first time, ensure that in the Customize Python 2.7.13 screen, you select **Add python.exe to Path**.</span></span>
> 
> ![Képernyőfelvétel a Customize Python 2.7.11 (Python 2.7.11 testreszabása) képernyőről, ahol be az Add python.exe to Path (Python.exe hozzáadása az útvonalhoz) lehetőséget ki kell választania.](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* <span data-ttu-id="6bfdd-131">[A Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="6bfdd-132">1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-132">Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="6bfdd-133">Először hozzon létre egy Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-133">Let's start by creating an Cosmos DB account.</span></span> <span data-ttu-id="6bfdd-134">Ha már rendelkezik fiókkal, vagy az oktatóanyagban az Azure Cosmos DB Emulatort használja, továbbléphet a [2. lépés: Új Python Flask-webalkalmazás létrehozása](#step-2-create-a-new-python-flask-web-application) című lépésre.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-134">If you already have an account or if you are using the Azure Cosmos DB Emulator for this tutorial, you can skip to [Step 2: Create a new Python Flask web application](#step-2-create-a-new-python-flask-web-application).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
<span data-ttu-id="6bfdd-135">Most végigvezetjük azon, hogyan hozhat létre új Python Flask-webalkalmazást az alapoktól kezdve.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-135">We will now walk through how to create a new Python Flask web application from the ground up.</span></span>

## <a name="step-2-create-a-new-python-flask-web-application"></a><span data-ttu-id="6bfdd-136">2. lépés: Új Python Flask-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-136">Step 2: Create a new Python Flask web application</span></span>
1. <span data-ttu-id="6bfdd-137">A Visual Studio programban, a **File** (Fájl) menüben mutasson a **New** (Új) elemre, majd kattintson a **Project** (Projekt) elemre.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-137">In Visual Studio, on the **File** menu, point to **New**, and then click **Project**.</span></span>
   
    <span data-ttu-id="6bfdd-138">Megjelenik a **New project** (Új projekt) párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-138">The **New Project** dialog box appears.</span></span>
2. <span data-ttu-id="6bfdd-139">A bal oldali ablaktáblán bontsa ki a **Templates** (Sablonok), majd a **Python** elemet, és kattintson a **Web** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-139">In the left pane, expand **Templates** and then **Python**, and then click **Web**.</span></span> 
3. <span data-ttu-id="6bfdd-140">Válassza ki a **Flask Web Project** (Flask webes projekt) lehetőséget a középső ablaktáblán, majd a **Name** (Név) mezőbe írja be a **tutorial** nevet, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-140">Select **Flask  Web Project** in the center pane, then in the **Name** box type **tutorial**, and then click **OK**.</span></span> <span data-ttu-id="6bfdd-141">Ne feledje, hogy a Python-csomagok nevében csak kisbetű szerepelhet, ahogyan ezt a [Stílusmutató a Python-kódokhoz](https://www.python.org/dev/peps/pep-0008/#package-and-module-names) című útmutató is részletezi.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-141">Remember that Python package names should be all lowercase, as described in the [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span></span>
   
    <span data-ttu-id="6bfdd-142">Azok számára, akik még nem ismernék, a Python Flask egy webalkalmazás-fejlesztési keretrendszer, amely lehetővé teszi a webalkalmazások Pythonban történő gyorsabb létrehozását.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-142">For those new to Python Flask, it is a web application development framework that helps you build web applications in Python faster.</span></span>
   
    ![Képernyőfelvétel a Visual Studio New Project (Új projekt) ablakáról, amely bal oldalán ki van emelve a Python, középen ki van választva a Python Flask webes projekt elem, a Name (Név) mezőben pedig meg van adva a tutorial név.](./media/documentdb-python-application/image9.png)
4. <span data-ttu-id="6bfdd-144">A **Python Tools for Visual Studio** ablakban kattintson az **Install into a virtual environment** (Telepítés virtuális környezetbe) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-144">In the **Python Tools for Visual Studio** window, click **Install into a virtual environment**.</span></span> 
   
    ![Képernyőfelvétel az adatbázisról-oktatóanyagról – Python Tools for Visual Studio](./media/documentdb-python-application/python-install-virtual-environment.png)
5. <span data-ttu-id="6bfdd-146">Az **Add Virtual Environment** (Virtuális környezet hozzáadása) ablakban elfogadhatja az alapértelmezett értékeket, és a Python 2.7-es verziót használhatja alapkörnyezetként, mivel a PyDocumentDB jelenleg nem támogatja a Python 3.x-es verzióit. Végül kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-146">In the **Add Virtual Environment** window, you can accept the defaults and use Python 2.7 as the base environment because PyDocumentDB does not currently support Python 3.x, and then click **Create**.</span></span> <span data-ttu-id="6bfdd-147">Ezzel beállítja a projekthez szükséges Python virtuális környezetet.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-147">This sets up the required Python virtual environment for your project.</span></span>
   
    ![Képernyőfelvétel az adatbázisról-oktatóanyagról – Python Tools for Visual Studio](./media/documentdb-python-application/image10_A.png)
   
    <span data-ttu-id="6bfdd-149">A környezet sikeres telepítését követően a következőt látja majd a kimeneti ablakban: `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-149">The output window displays `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` when the environment is successfully installed.</span></span>

## <a name="step-3-modify-the-python-flask-web-application"></a><span data-ttu-id="6bfdd-150">3. lépés: A Python Flask-webalkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-150">Step 3: Modify the Python Flask web application</span></span>
### <a name="add-the-python-flask-packages-to-your-project"></a><span data-ttu-id="6bfdd-151">A Python Flask-csomagok hozzáadása a projekthez</span><span class="sxs-lookup"><span data-stu-id="6bfdd-151">Add the Python Flask packages to your project</span></span>
<span data-ttu-id="6bfdd-152">A projekt beállítását követően hozzá kell adnia a szükséges Flask-csomagokat a projekthez, beleértve a pydocumentdb csomagot is, amely a DocumentDB-hez szükséges Python-csomag.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-152">After your project is set up, you'll need to add the required Flask packages to your project, including pydocumentdb, the Python package for DocumentDB.</span></span>

1. <span data-ttu-id="6bfdd-153">A Solution Explorer (Megoldáskezelő) nézetben nyissa meg a **requirements.txt** fájlt, majd cserélje ki annak tartalmát a következőre:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-153">In Solution Explorer, open the file named **requirements.txt** and replace the contents with the following:</span></span>
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. <span data-ttu-id="6bfdd-154">Mentse a **requirements.txt** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-154">Save the **requirements.txt** file.</span></span> 
3. <span data-ttu-id="6bfdd-155">A Solution Explorer (Megoldáskezelő) nézetben kattintson a jobb gombbal az **env** elemre, majd kattintson az **Install from requirements.txt** (Telepítés a requirements.txt fájlból) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-155">In Solution Explorer, right-click **env** and click **Install from requirements.txt**.</span></span>
   
    ![A képernyőfelvétel az env elem (Python 2.7) kiválasztását, valamint az Install from requirements.txt (Telepítés a requirements.txt fájlból) lehetőséget mutatja be.](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    <span data-ttu-id="6bfdd-157">A sikeres telepítés után a kimeneti ablak a következőt jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-157">After successful installation, the output window displays the following:</span></span>
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > <span data-ttu-id="6bfdd-158">Ritka esetekben előfordulhat, hogy egy hibaüzenet jelenik meg a kimeneti ablakban.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-158">In rare cases, you might see a failure in the output window.</span></span> <span data-ttu-id="6bfdd-159">Ebben az esetben ellenőrizze, hogy a hiba a tisztítással kapcsolatos-e.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-159">If this happens, check if the error is related to cleanup.</span></span> <span data-ttu-id="6bfdd-160">Előfordul, hogy a tisztítás sikertelen, de a telepítés sikeres (ennek ellenőrzéséhez görgessen felfelé a kimeneti ablakban).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-160">Sometimes the cleanup fails, but the installation will still be successful (scroll up in the output window to verify this).</span></span> <span data-ttu-id="6bfdd-161">A telepítés állapotát a [virtuális környezet ellenőrzésével](#verify-the-virtual-environment) vizsgálhatja meg.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-161">You can check your installation by [Verifying the virtual environment](#verify-the-virtual-environment).</span></span> <span data-ttu-id="6bfdd-162">Ha a telepítés sikertelen volt, de a megerősítés sikeres, akkor továbbléphet.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-162">If the installation failed but the verification is successful, it's OK to continue.</span></span>
   > 
   > 

### <a name="verify-the-virtual-environment"></a><span data-ttu-id="6bfdd-163">A virtuális környezet ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="6bfdd-163">Verify the virtual environment</span></span>
<span data-ttu-id="6bfdd-164">Ellenőrizzük, hogy minden megfelelően telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-164">Let's make sure that everything is installed correctly.</span></span>

1. <span data-ttu-id="6bfdd-165">Fordítsa le a megoldást a **Ctrl**+**Shift**+**B** billentyűkombináció lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-165">Build the solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="6bfdd-166">A sikeres fordítás után indítsa el a webhelyet az **F5** billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-166">Once the build succeeds, start the website by pressing **F5**.</span></span> <span data-ttu-id="6bfdd-167">Ez elindítja a Flask fejlesztési kiszolgálót és a webböngészőt.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-167">This launches the Flask development server and starts your web browser.</span></span> <span data-ttu-id="6bfdd-168">A következő lapnak kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-168">You should see the following page.</span></span>
   
    ![A böngészőben megjelenített üres Python Flask webes fejlesztési projekt](./media/documentdb-python-application/image12.png)
3. <span data-ttu-id="6bfdd-170">Nyomja le a **Shift**+**F5** billentyűkombinációt a Visual Studio alkalmazásban a webhely hibakeresésének leállításához.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-170">Stop debugging the website by pressing **Shift**+**F5** in Visual Studio.</span></span>

### <a name="create-database-collection-and-document-definitions"></a><span data-ttu-id="6bfdd-171">Adatbázis-, gyűjtemény- és dokumentum-definíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-171">Create database, collection, and document definitions</span></span>
<span data-ttu-id="6bfdd-172">Ideje létrehozni a szavazóalkalmazást az új fájlok hozzáadásával, valamint a többi fájl frissítésével.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-172">Now let's create your voting application by adding new files and updating others.</span></span>

1. <span data-ttu-id="6bfdd-173">A Solution Explorer (Megoldáskezelő) nézetben kattintson a jobb gombbal a **tutorial** nevű projektre, kattintson az **Add** (Hozzáadás), majd a **New Item** (Új elem) gombra.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-173">In Solution Explorer, right-click the **tutorial** project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="6bfdd-174">Válassza az **Empty Python File** (Üres Python-fájl) lehetőséget, és adja neki a **forms.py** nevet.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-174">Select **Empty Python File** and name the file **forms.py**.</span></span>  
2. <span data-ttu-id="6bfdd-175">Adja hozzá a következő kódot a forms.py fájlhoz, majd mentse azt.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-175">Add the following code to the forms.py file, and then save the file.</span></span>

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-the-required-imports-to-viewspy"></a><span data-ttu-id="6bfdd-176">A szükséges importálások hozzáadása a views.py fájlhoz</span><span class="sxs-lookup"><span data-stu-id="6bfdd-176">Add the required imports to views.py</span></span>
1. <span data-ttu-id="6bfdd-177">A Solution Explorer (Megoldáskezelő) nézetben bontsa ki a **tutorial** mappát, majd nyissa meg a **views.py** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-177">In Solution Explorer, expand the **tutorial** folder, and open the **views.py** file.</span></span> 
2. <span data-ttu-id="6bfdd-178">Adja hozzá a következő importálási utasításokat a **views.py** fájl elejéhez, majd mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-178">Add the following import statements to the top of the **views.py** file, then save the file.</span></span> <span data-ttu-id="6bfdd-179">Ezek importálják majd a Cosmos DB Python SDK-it és a Flask-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-179">These import Cosmos DB's PythonSDK and the Flask packages.</span></span>
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a><span data-ttu-id="6bfdd-180">Adatbázisok, gyűjtemények és dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-180">Create database, collection, and document</span></span>
* <span data-ttu-id="6bfdd-181">Adja hozzá az alábbi kódot a **views.py** fájl végéhez.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-181">Still in **views.py**, add the following code to the end of the file.</span></span> <span data-ttu-id="6bfdd-182">Ezzel létrehozza az űrlap által használt adatbázist.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-182">This takes care of creating the database used by the form.</span></span> <span data-ttu-id="6bfdd-183">Ne töröljön semmit a **views.py** fájl meglévő kódjából.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-183">Do not delete any of the existing code in **views.py**.</span></span> <span data-ttu-id="6bfdd-184">Egyszerűen csak fűzze hozzá a kódot a fájl végéhez.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-184">Simply append this to the end.</span></span>

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config.DOCUMENTDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config.DOCUMENTDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config.DOCUMENTDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config.DOCUMENTDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a><span data-ttu-id="6bfdd-185">Adatbázis, gyűjtemény és dokumentum beolvasása, valamint az űrlap elküldése</span><span class="sxs-lookup"><span data-stu-id="6bfdd-185">Read database, collection, document, and submit form</span></span>
* <span data-ttu-id="6bfdd-186">Adja hozzá az alábbi kódot a **views.py** fájl végéhez.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-186">Still in **views.py**, add the following code to the end of the file.</span></span> <span data-ttu-id="6bfdd-187">Ezzel létrehozza az űrlapot, beolvassa az adatbázist, a gyűjteményt és a dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-187">This takes care of setting up the form, reading the database, collection, and document.</span></span> <span data-ttu-id="6bfdd-188">Ne töröljön semmit a **views.py** fájl meglévő kódjából.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-188">Do not delete any of the existing code in **views.py**.</span></span> <span data-ttu-id="6bfdd-189">Egyszerűen csak fűzze hozzá a kódot a fájl végéhez.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-189">Simply append this to the end.</span></span>

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config.DOCUMENTDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config.DOCUMENTDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config.DOCUMENTDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-the-html-files"></a><span data-ttu-id="6bfdd-190">A HTML-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-190">Create the HTML files</span></span>
1. <span data-ttu-id="6bfdd-191">A Solution Explorer (Megoldáskezelő) nézetben, a **tutorial** mappában kattintson a jobb gombbal a **Templates** (Sablonok) mappára, kattintson az **Add** (Hozzáadás), majd a **New Item** (Új elem) elemre.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-191">In Solution Explorer, in the **tutorial** folder, right click the **templates** folder, click **Add**, and then click **New Item**.</span></span> 
2. <span data-ttu-id="6bfdd-192">Válassza ki a **HTML Page** (HTML-oldal) lehetőséget, majd a Name (Név) mezőbe írja be a **create.html** nevet.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-192">Select **HTML Page**, and then in the name box type **create.html**.</span></span> 
3. <span data-ttu-id="6bfdd-193">Ismételje meg az 1. és 2. lépést, és adjon hozzá további kettő HTML-fájlt: ezek a results.html és a vote.html.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-193">Repeat steps 1 and 2 to create two additional HTML files: results.html and vote.html.</span></span>
4. <span data-ttu-id="6bfdd-194">Adja hozzá a következő kódot a **create.html** fájl `<body>` szakaszához.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-194">Add the following code to **create.html** in the `<body>` element.</span></span> <span data-ttu-id="6bfdd-195">Ez megjelenít egy üzenetet, miszerint sikeresen létrehozott egy új adatbázist, gyűjteményt és dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-195">It displays a message stating that we created a new database, collection, and document.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. <span data-ttu-id="6bfdd-196">Adja hozzá a következő kódot a **results.html** fájl `<body`> szakaszához.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-196">Add the following code to **results.html** in the `<body`> element.</span></span> <span data-ttu-id="6bfdd-197">Ez megjeleníti a szavazás eredményét.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-197">It displays the results of the poll.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of the vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. <span data-ttu-id="6bfdd-198">Adja hozzá a következő kódot a **vote.html** fájl `<body`> szakaszához.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-198">Add the following code to **vote.html** in the `<body`> element.</span></span> <span data-ttu-id="6bfdd-199">Ez megjeleníti a szavazást, és fogadja a szavazatokat.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-199">It displays the poll and accepts the votes.</span></span> <span data-ttu-id="6bfdd-200">A szavazatok regisztrálása után a vezérlést a views.py fájl veszi át, ahol feldolgozhatjuk a leadott szavazatot, és annak megfelelően hozzáfűzhetjük a szükséges dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-200">On registering the votes, the control is passed over to views.py where we will recognize the vote cast and append the document accordingly.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way to host an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. <span data-ttu-id="6bfdd-201">A **templates** mappában cserélje ki az **index.html** fájl tartalmát az alábbira.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-201">In the **templates** folder, replace the contents of **index.html** with the following.</span></span> <span data-ttu-id="6bfdd-202">Ez lesz az alkalmazás kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-202">This serves as the landing page for your application.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-initpy"></a><span data-ttu-id="6bfdd-203">Konfigurációs fájl hozzáadása és az \_\_init\_\_.py fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-203">Add a configuration file and change the \_\_init\_\_.py</span></span>
1. <span data-ttu-id="6bfdd-204">A Solution Explorer (Megoldáskezelő) nézetben kattintson a jobb gombbal a **tutorial** nevű projektre, kattintson az **Add** (Hozzáadás), majd a **New Item** (Új elem) gombra, válassza az **Empty Python File** (Üres Python-fájl) lehetőséget, és a fájlnak adja a **config.py** nevet.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-204">In Solution Explorer, right-click the **tutorial** project, click **Add**, click **New Item**, select **Empty Python File**, and then name the file **config.py**.</span></span> <span data-ttu-id="6bfdd-205">A Flask űrlapjainak szüksége van erre a konfigurációs fájlra.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-205">This config file is required by forms in Flask.</span></span> <span data-ttu-id="6bfdd-206">Ezzel a fájllal egy titkos kulcsot is megadhat.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-206">You can use it to provide a secret key as well.</span></span> <span data-ttu-id="6bfdd-207">A jelen oktatóanyaghoz azonban nincs szükség ilyen kulcsra.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-207">This key is not needed for this tutorial though.</span></span>
2. <span data-ttu-id="6bfdd-208">Adja hozzá a következő kódot a config.py fájlhoz, és a következő lépésben módosítsa a **DOCUMENTDB\_HOST** és **DOCUMENTDB\_KEY** paraméterek értékét.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-208">Add the following code to config.py, you'll need to alter the values of **DOCUMENTDB\_HOST** and **DOCUMENTDB\_KEY** in the next step.</span></span>
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. <span data-ttu-id="6bfdd-209">Az [Azure Portalon](https://portal.azure.com/) navigáljon a **Kulcsok** panelre. Ehhez kattintson a **Tallózás**, majd az **Azure Cosmos DB-fiókok** lehetőségre, kattintson duplán a használni kívánt fiók nevére, és végül kattintson a **Kulcsok** gombra az **Alapvető erőforrások** területen.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-209">In the [Azure portal](https://portal.azure.com/), navigate to the **Keys** blade by clicking **Browse**, **Azure Cosmos DB Accounts**, double-click the name of the account to use, and then click the **Keys** button in the **Essentials** area.</span></span> <span data-ttu-id="6bfdd-210">A **Kulcsok** panelen másolja ki az **URI** mező értékét, és illessze be azt a **config.py** fájlba a **DOCUMENTDB\_HOST** paraméter értéke helyére.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-210">In the **Keys** blade, copy the **URI** value and paste it into the **config.py** file, as the value for the **DOCUMENTDB\_HOST** property.</span></span> 
4. <span data-ttu-id="6bfdd-211">Ismét az Azure Portalon, a **Kulcsok** panelen másolja ki az **Elsődleges kulcs** vagy **Másodlagos kulcs** mező értékét, és illessze be azt a **config.py** fájlba a **DOCUMENTDB\_KEY** paraméter értéke helyére.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-211">Back in the Azure portal, in the **Keys** blade, copy the value of the **Primary Key** or the **Secondary Key**, and paste it into the **config.py** file, as the value for the **DOCUMENTDB\_KEY** property.</span></span>
5. <span data-ttu-id="6bfdd-212">Adja hozzá a következő sort az **\_\_init\_\_.py** fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-212">In the **\_\_init\_\_.py** file, add the following line.</span></span> 
   
        app.config.from_object('config')
   
    <span data-ttu-id="6bfdd-213">Tehát a fájl tartalma a következő legyen:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-213">So that the content of the file is:</span></span>
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. <span data-ttu-id="6bfdd-214">Az összes fájl hozzáadása után a Solution Explorer (Megoldáskezelő) nézetnek az alábbi módon kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-214">After adding all the files, Solution Explorer should look like this:</span></span>
   
    ![Képernyőfelvétel a Visual Studio Solution Explorer (Megoldáskezelő) ablakáról](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a><span data-ttu-id="6bfdd-216">4. lépés: A webalkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="6bfdd-216">Step 4: Run your web application locally</span></span>
1. <span data-ttu-id="6bfdd-217">Fordítsa le a megoldást a **Ctrl**+**Shift**+**B** billentyűkombináció lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-217">Build the solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="6bfdd-218">A sikeres fordítás után indítsa el a webhelyet az **F5** billentyű lenyomásával.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-218">Once the build succeeds, start the website by pressing **F5**.</span></span> <span data-ttu-id="6bfdd-219">A következőnek kell megjelennie a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-219">You should see the following on your screen.</span></span>
   
    ![Képernyőfelvétel a webböngészőben megjelenített Python + Azure Cosmos DB szavazóalkalmazásról](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. <span data-ttu-id="6bfdd-221">Kattintson a **Create/Clear the Voting Database** (A szavazóadatbázis létrehozása/törlése) lehetőségre az adatbázis létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-221">Click **Create/Clear the Voting Database** to generate the database.</span></span>
   
    ![Képernyőfelvétel a webalkalmazás Create (Létrehozás) lapjáról – fejlesztési részletek](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. <span data-ttu-id="6bfdd-223">Ezután kattintson a **Vote** (Szavazás) elemre, és válassza ki a kívánt elemet.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-223">Then, click **Vote** and select your option.</span></span>
   
    ![Képernyőfelvétel a webalkalmazásról és a szavazási kérdés feltételéről](./media/documentdb-python-application/cosmos-db-vote.png)
5. <span data-ttu-id="6bfdd-225">Minden leadott szavazattal az annak megfelelő számlálót növeli.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-225">For every vote you cast, it increments the appropriate counter.</span></span>
   
    ![Képernyőfelvétel a szavazás oldalának Results (Eredmények) lapjáról](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. <span data-ttu-id="6bfdd-227">A projekt hibakeresésének leállításához nyomja le a Shift+F5 billentyűkombinációt.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-227">Stop debugging the project by pressing Shift+F5.</span></span>

## <a name="step-5-deploy-the-web-application-to-azure"></a><span data-ttu-id="6bfdd-228">5. lépés: A webalkalmazás az Azure-bA telepítése</span><span class="sxs-lookup"><span data-stu-id="6bfdd-228">Step 5: Deploy the web application to Azure</span></span>
<span data-ttu-id="6bfdd-229">Most, hogy a teljes alkalmazás megfelelően működik-e Cosmos DB ellen, fogjuk központilag telepítheti az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-229">Now that you have the complete application working correctly against Cosmos DB, we're going to deploy this to Azure.</span></span>

1. <span data-ttu-id="6bfdd-230">Kattintson a jobb gombbal a projektre a Solution Explorer (Megoldáskezelő) nézetben (győződjön meg arról, hogy helyileg már nem futtatja azt), és válassza a **Publish** (Közzététel) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-230">Right-click the project in Solution Explorer (make sure you're not still running it locally) and select **Publish**.</span></span>  
   
     ![Képernyőfelvétel a kiválasztott „tutorial” projektről a Solution Explorer (Megoldáskezelő) nézetben, a kiemelt Publish (Közzététel) lehetőséggel](./media/documentdb-python-application/image20.png)
2. <span data-ttu-id="6bfdd-232">Az a **közzététel** párbeszédpanelen jelölje ki **Microsoft Azure App Service**, jelölje be **hozzon létre új**, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-232">In the **Publish** dialog box, select **Microsoft Azure App Service**, select **Create New**, and then click **Publish**.</span></span>
   
    ![Képernyőfelvétel a Microsoft Azure App Service a kijelölt webhely közzététele ablak](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. <span data-ttu-id="6bfdd-234">Az a **App Service létrehozása** párbeszédpanel mezőben adja meg a nevet a webalkalmazás, valamint a **előfizetés**, **erőforráscsoport**, és **App Service-csomag**, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-234">In the **Create App Service** dialog box, enter the name for your web app along with your **Subscription**, **Resource Group**, and **App Service Plan**, then click **Create**.</span></span>
   
    ![Képernyőfelvétel a Microsoft Azure Web Apps (Microsoft Azure-webalkalmazások) ablakról](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. <span data-ttu-id="6bfdd-236">Néhány másodpercen belül a Visual Studio befejezi az app service közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!</span><span class="sxs-lookup"><span data-stu-id="6bfdd-236">In a few seconds, Visual Studio will finish publishing your app service and launch a browser where you can see your handiwork running in Azure!</span></span>

    ![Képernyőfelvétel a Microsoft Azure Web Apps (Microsoft Azure-webalkalmazások) ablakról](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a><span data-ttu-id="6bfdd-238">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6bfdd-238">Troubleshooting</span></span>
<span data-ttu-id="6bfdd-239">Ha ez az első Python-alkalmazás, amelyet számítógépén futtat, győződjön meg arról, hogy a következő mappák (vagy az azokkal egyenértékű telepítési helyek) szerepelnek a PATH változóban:</span><span class="sxs-lookup"><span data-stu-id="6bfdd-239">If this is the first Python app you've run on your computer, ensure that the following folders (or the equivalent installation locations) are included in your PATH variable:</span></span>

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

<span data-ttu-id="6bfdd-240">Ha hibába ütközik a szavazási lapon, és a projektet nem **tutorial** néven hozta létre, győződjön meg arról, hogy az **\_\_init\_\_.py** fájl a megfelelő projektnévre hivatkozik a következő sorban: `import tutorial.view`.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-240">If you receive an error on your vote page, and you named your project something other than **tutorial**, make sure that **\_\_init\_\_.py** references the correct project name in the line: `import tutorial.view`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6bfdd-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bfdd-241">Next steps</span></span>
<span data-ttu-id="6bfdd-242">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="6bfdd-242">Congratulations!</span></span> <span data-ttu-id="6bfdd-243">Ebben az esetben az első Python webes alkalmazás Cosmos DB használatával befejeződött, és közzétette azt Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-243">You have just completed your first Python web application using Cosmos DB and published it to Azure.</span></span>

<span data-ttu-id="6bfdd-244">Gyakran frissítjük és javítjuk a jelen témakört a visszajelzések alapján.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-244">We update and improve this topic frequently based on your feedback.</span></span>  <span data-ttu-id="6bfdd-245">Az oktatóanyag befejezése után a lap tetején vagy alján található szavazógomb használatával küldhet visszajelzést. A visszajelzésbe azt is foglalja bele, hogy milyen javításokat szeretne látni.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-245">Once you've completed the tutorial, please using the voting buttons at the top and bottom of this page, and be sure to include your feedback on what improvements you want to see made.</span></span> <span data-ttu-id="6bfdd-246">Ha szeretne közvetlenül kapcsolatba lépni velünk, a hozzászólásaiban tüntesse fel az e-mail-címét.</span><span class="sxs-lookup"><span data-stu-id="6bfdd-246">If you'd like us to contact you directly, feel free to include your email address in your comments.</span></span>

<span data-ttu-id="6bfdd-247">További funkciókat szeretne az alkalmazáshoz adni, tekintse át az elérhető API-kat a [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-247">To add additional functionality to your web application, review the APIs available in the [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span></span>

<span data-ttu-id="6bfdd-248">Az Azure-ra, a Visual Studióval és a Pythonnal kapcsolatos további információkért lásd: [Python fejlesztői központ](https://azure.microsoft.com/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="6bfdd-248">For more information about Azure, Visual Studio, and Python, see the [Python Developer Center](https://azure.microsoft.com/develop/python/).</span></span> 

<span data-ttu-id="6bfdd-249">További Python Flask-oktatóanyagok: [The Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) (A Flask óriási oktatóanyaga – 1. rész: Hello, World!)</span><span class="sxs-lookup"><span data-stu-id="6bfdd-249">For additional Python Flask tutorials, see [The Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span></span> 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
