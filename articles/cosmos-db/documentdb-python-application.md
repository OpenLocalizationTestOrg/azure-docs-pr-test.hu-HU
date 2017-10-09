---
title: "aaaPython Flask webalkalmazásokra vonatkozó oktatóanyag az Azure Cosmos DB |} Microsoft Docs"
description: "Tekintse át egy adatbázis-oktatóanyag az Azure-platformon futó Python Flask-webalkalmazások toostore és a hozzáférési adatok Azure Cosmos DB használatával. Alkalmazásfejlesztési megoldások keresése."
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
ms.openlocfilehash: 87b73c656ed96a7efbd162843a1529d435f027f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a><span data-ttu-id="488f2-105">Python Flask-webalkalmazás létrehozása az Azure Cosmos DB használatával</span><span class="sxs-lookup"><span data-stu-id="488f2-105">Build a Python Flask web application using Azure Cosmos DB</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="488f2-106">.NET</span><span class="sxs-lookup"><span data-stu-id="488f2-106">.NET</span></span>](documentdb-dotnet-application.md)
> * [<span data-ttu-id="488f2-107">Node.js</span><span class="sxs-lookup"><span data-stu-id="488f2-107">Node.js</span></span>](documentdb-nodejs-application.md)
> * [<span data-ttu-id="488f2-108">Java</span><span class="sxs-lookup"><span data-stu-id="488f2-108">Java</span></span>](documentdb-java-application.md)
> * [<span data-ttu-id="488f2-109">Python</span><span class="sxs-lookup"><span data-stu-id="488f2-109">Python</span></span>](documentdb-python-application.md)
> 
> 

<span data-ttu-id="488f2-110">Az oktatóanyag bemutatja, hogyan toouse Azure Cosmos DB toostore és a hozzáférési adatok egy Python webes alkalmazás Azure-platformon futó, és feltételezi, hogy rendelkezik némi tapasztalattal a Python és az Azure-webhelyek használatát.</span><span class="sxs-lookup"><span data-stu-id="488f2-110">This tutorial shows you how toouse Azure Cosmos DB toostore and access data from a Python web application hosted on Azure and presumes that you have some prior experience using Python and Azure websites.</span></span>

<span data-ttu-id="488f2-111">Az adatbázis-oktatóanyag az alábbiakat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="488f2-111">This database tutorial covers:</span></span>

1. <span data-ttu-id="488f2-112">Létrehozása, és üzembe helyezése egy Cosmos-DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="488f2-112">Creating and provisioning a Cosmos DB account.</span></span>
2. <span data-ttu-id="488f2-113">A Python Flask-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="488f2-113">Creating a Python Flask application.</span></span>
3. <span data-ttu-id="488f2-114">Csatlakozás tooand Cosmos DB használatával a webalkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="488f2-114">Connecting tooand using Cosmos DB from your web application.</span></span>
4. <span data-ttu-id="488f2-115">Hello webes alkalmazás tooAzure telepítését.</span><span class="sxs-lookup"><span data-stu-id="488f2-115">Deploying hello web application tooAzure.</span></span>

<span data-ttu-id="488f2-116">Az oktatóanyag utasításait követve egy egyszerű szavazóalkalmazást, amely lehetővé teszi a voksát egy szavazáson toovote fog létrehozni.</span><span class="sxs-lookup"><span data-stu-id="488f2-116">By following this tutorial, you will build a simple voting application that allows you toovote for a poll.</span></span>

![Képernyőfelvétel a hello szavazóalkalmazást adatbázis-oktatóprogram során létrehozott](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a><span data-ttu-id="488f2-118">Az adatbázis-oktatóanyag előfeltételei</span><span class="sxs-lookup"><span data-stu-id="488f2-118">Database tutorial prerequisites</span></span>
<span data-ttu-id="488f2-119">Ez a cikk hello utasításait követve, előtt győződjön meg, hogy rendelkezik a következőkkel hello:</span><span class="sxs-lookup"><span data-stu-id="488f2-119">Before following hello instructions in this article, you should ensure that you have hello following installed:</span></span>

* <span data-ttu-id="488f2-120">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="488f2-120">An active Azure account.</span></span> <span data-ttu-id="488f2-121">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="488f2-121">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="488f2-122">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="488f2-122">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
 
    <span data-ttu-id="488f2-123">VAGY</span><span class="sxs-lookup"><span data-stu-id="488f2-123">OR</span></span> 

    <span data-ttu-id="488f2-124">Egy helyi telepítését teszi hello [Azure Cosmos DB emulátor](local-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="488f2-124">A local installation of hello [Azure Cosmos DB Emulator](local-emulator.md).</span></span>
* <span data-ttu-id="488f2-125">[A Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="488f2-125">[Microsoft Visual Studio Community 2017](http://www.visualstudio.com/).</span></span>  
* <span data-ttu-id="488f2-126">[A Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span><span class="sxs-lookup"><span data-stu-id="488f2-126">[Python Tools for Visual Studio](https://github.com/Microsoft/PTVS/).</span></span>  
* <span data-ttu-id="488f2-127">[Python 2.7-hez készült Microsoft Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="488f2-127">[Microsoft Azure SDK for Python 2.7](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="488f2-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span><span class="sxs-lookup"><span data-stu-id="488f2-128">[Python 2.7.13](https://www.python.org/downloads/windows/).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="488f2-129">Ha telepíti Python 2.7 hello először, győződjön meg arról, hogy hello testreszabása Python 2.7.13 képernyőjén választja **python.exe tooPath hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="488f2-129">If you are installing Python 2.7 for hello first time, ensure that in hello Customize Python 2.7.13 screen, you select **Add python.exe tooPath**.</span></span>
> 
> ![Képernyőfelvétel a hello testreszabása Python 2.7.11 képernyő, ahol tooselect Add python.exe tooPath kell](./media/documentdb-python-application/cosmos-db-python-install.png)
> 
> 

* <span data-ttu-id="488f2-131">[A Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span><span class="sxs-lookup"><span data-stu-id="488f2-131">[Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).</span></span>

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a><span data-ttu-id="488f2-132">1. lépés: Azure Cosmos DB-adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="488f2-132">Step 1: Create an Azure Cosmos DB database account</span></span>
<span data-ttu-id="488f2-133">Először hozzon létre egy Cosmos DB-fiókot.</span><span class="sxs-lookup"><span data-stu-id="488f2-133">Let's start by creating an Cosmos DB account.</span></span> <span data-ttu-id="488f2-134">Ha már rendelkezik fiókkal, vagy használatakor hello Azure Cosmos DB emulátor ehhez az oktatóanyaghoz, ugorjon túl[2. lépés: új Python Flask-webalkalmazás létrehozása](#step-2-create-a-new-python-flask-web-application).</span><span class="sxs-lookup"><span data-stu-id="488f2-134">If you already have an account or if you are using hello Azure Cosmos DB Emulator for this tutorial, you can skip too[Step 2: Create a new Python Flask web application](#step-2-create-a-new-python-flask-web-application).</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
<span data-ttu-id="488f2-135">Most végigvezetjük hogyan toocreate egy új Python Flask-webalkalmazás a hello szabad-e.</span><span class="sxs-lookup"><span data-stu-id="488f2-135">We will now walk through how toocreate a new Python Flask web application from hello ground up.</span></span>

## <a name="step-2-create-a-new-python-flask-web-application"></a><span data-ttu-id="488f2-136">2. lépés: Új Python Flask-webalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="488f2-136">Step 2: Create a new Python Flask web application</span></span>
1. <span data-ttu-id="488f2-137">A Visual Studio, a hello **fájl** menüben mutasson túl**új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="488f2-137">In Visual Studio, on hello **File** menu, point too**New**, and then click **Project**.</span></span>
   
    <span data-ttu-id="488f2-138">Hello **új projekt** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="488f2-138">hello **New Project** dialog box appears.</span></span>
2. <span data-ttu-id="488f2-139">Hello bal oldali ablaktáblán bontsa ki a **sablonok** , majd **Python**, és kattintson a **webes**.</span><span class="sxs-lookup"><span data-stu-id="488f2-139">In hello left pane, expand **Templates** and then **Python**, and then click **Web**.</span></span> 
3. <span data-ttu-id="488f2-140">Válassza ki **Flask webes projekt** hello középső ablaktáblába, majd a hello **neve** mezőbe írja be **oktatóanyag**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="488f2-140">Select **Flask  Web Project** in hello center pane, then in hello **Name** box type **tutorial**, and then click **OK**.</span></span> <span data-ttu-id="488f2-141">Ne feledje, hogy Python-csomagok nevében csak kisbetű szerepelhet, a hello [Stílusútmutató Python kód](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span><span class="sxs-lookup"><span data-stu-id="488f2-141">Remember that Python package names should be all lowercase, as described in hello [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names).</span></span>
   
    <span data-ttu-id="488f2-142">Ezen új tooPython Flask egy webalkalmazás-fejlesztési keretrendszer, amely segít a webalkalmazások pythonban gyorsabban esetén.</span><span class="sxs-lookup"><span data-stu-id="488f2-142">For those new tooPython Flask, it is a web application development framework that helps you build web applications in Python faster.</span></span>
   
    ![Képernyőfelvétel a Visual Studio és Python lévő balra, a Python Flask webes projekt hello középső és hello neve oktatóanyag hello név mezőben kiválasztott hello hello új projekt ablakról](./media/documentdb-python-application/image9.png)
4. <span data-ttu-id="488f2-144">A hello **a Python Tools for Visual Studio** ablak, kattintson a **a virtuális környezetbe telepítése**.</span><span class="sxs-lookup"><span data-stu-id="488f2-144">In hello **Python Tools for Visual Studio** window, click **Install into a virtual environment**.</span></span> 
   
    ![Képernyőfelvétel a hello adatbázisról-oktatóanyagról – Python Tools for Visual Studio ablak](./media/documentdb-python-application/python-install-virtual-environment.png)
5. <span data-ttu-id="488f2-146">A hello **virtuális környezet hozzáadása** ablakban hello alapértelmezések elfogadásához és a Python 2.7-es alapszintű hello környezetben használható, mert a PyDocumentDB jelenleg nem támogatja a Python 3.x, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="488f2-146">In hello **Add Virtual Environment** window, you can accept hello defaults and use Python 2.7 as hello base environment because PyDocumentDB does not currently support Python 3.x, and then click **Create**.</span></span> <span data-ttu-id="488f2-147">Hello szükséges Python virtuális környezetet a projekt állít be.</span><span class="sxs-lookup"><span data-stu-id="488f2-147">This sets up hello required Python virtual environment for your project.</span></span>
   
    ![Képernyőfelvétel a hello adatbázisról-oktatóanyagról – Python Tools for Visual Studio ablak](./media/documentdb-python-application/image10_A.png)
   
    <span data-ttu-id="488f2-149">a kimeneti ablakban jelennek meg hello `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` amikor hello környezet sikeres telepítését követően.</span><span class="sxs-lookup"><span data-stu-id="488f2-149">hello output window displays `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.` when hello environment is successfully installed.</span></span>

## <a name="step-3-modify-hello-python-flask-web-application"></a><span data-ttu-id="488f2-150">3. lépés: Hello Python Flask-webalkalmazás módosítása</span><span class="sxs-lookup"><span data-stu-id="488f2-150">Step 3: Modify hello Python Flask web application</span></span>
### <a name="add-hello-python-flask-packages-tooyour-project"></a><span data-ttu-id="488f2-151">Hello Python Flask-csomagok tooyour projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="488f2-151">Add hello Python Flask packages tooyour project</span></span>
<span data-ttu-id="488f2-152">A projekt beállítását követően tooadd hello szükséges Flask csomagok tooyour projekt, beleértve a pydocumentdb hello Python-csomag a DocumentDB lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="488f2-152">After your project is set up, you'll need tooadd hello required Flask packages tooyour project, including pydocumentdb, hello Python package for DocumentDB.</span></span>

1. <span data-ttu-id="488f2-153">A Solution Explorerben nyissa meg a hello fájlt nevű **requirements.txt** , és cserélje ki hello tartalmát hello következőre:</span><span class="sxs-lookup"><span data-stu-id="488f2-153">In Solution Explorer, open hello file named **requirements.txt** and replace hello contents with hello following:</span></span>
   
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
2. <span data-ttu-id="488f2-154">Mentse a hello **requirements.txt** fájlt.</span><span class="sxs-lookup"><span data-stu-id="488f2-154">Save hello **requirements.txt** file.</span></span> 
3. <span data-ttu-id="488f2-155">A Solution Explorer (Megoldáskezelő) nézetben kattintson a jobb gombbal az **env** elemre, majd kattintson az **Install from requirements.txt** (Telepítés a requirements.txt fájlból) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="488f2-155">In Solution Explorer, right-click **env** and click **Install from requirements.txt**.</span></span>
   
    ![Képernyőfelvétel az ENV elem (Python 2.7) ki a telepítés a requirements.txt hello listában kijelölt](./media/documentdb-python-application/cosmos-db-python-install-from-requirements.png)
   
    <span data-ttu-id="488f2-157">A sikeres telepítés után hello a kimeneti ablakban hello következő:</span><span class="sxs-lookup"><span data-stu-id="488f2-157">After successful installation, hello output window displays hello following:</span></span>
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > <span data-ttu-id="488f2-158">Ritka esetekben hello kimeneti ablakban hiba jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="488f2-158">In rare cases, you might see a failure in hello output window.</span></span> <span data-ttu-id="488f2-159">Ha ez történik, ellenőrizze, hogy hello hiba-e a kapcsolódó toocleanup.</span><span class="sxs-lookup"><span data-stu-id="488f2-159">If this happens, check if hello error is related toocleanup.</span></span> <span data-ttu-id="488f2-160">Egyes esetekben nem sikerül hello karbantartása, de hello telepítése akkor is sikeres (görgessen fel a hello kimeneti ablak tooverify ez).</span><span class="sxs-lookup"><span data-stu-id="488f2-160">Sometimes hello cleanup fails, but hello installation will still be successful (scroll up in hello output window tooverify this).</span></span> <span data-ttu-id="488f2-161">Ellenőrizheti a telepítés állapotát [ellenőrzése hello virtuális környezet](#verify-the-virtual-environment).</span><span class="sxs-lookup"><span data-stu-id="488f2-161">You can check your installation by [Verifying hello virtual environment](#verify-the-virtual-environment).</span></span> <span data-ttu-id="488f2-162">Ha hello telepítése sikertelen volt, de hello ellenőrzés sikeres, akkor OK toocontinue.</span><span class="sxs-lookup"><span data-stu-id="488f2-162">If hello installation failed but hello verification is successful, it's OK toocontinue.</span></span>
   > 
   > 

### <a name="verify-hello-virtual-environment"></a><span data-ttu-id="488f2-163">Hello virtuális környezet ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="488f2-163">Verify hello virtual environment</span></span>
<span data-ttu-id="488f2-164">Ellenőrizzük, hogy minden megfelelően telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="488f2-164">Let's make sure that everything is installed correctly.</span></span>

1. <span data-ttu-id="488f2-165">Hello megoldás kiépítését, billentyűkombináció lenyomásával **Ctrl**+**Shift**+**B**.</span><span class="sxs-lookup"><span data-stu-id="488f2-165">Build hello solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="488f2-166">Sikeres hello fordítás után indítsa el a webhelyet hello billentyűkombináció lenyomásával **F5**.</span><span class="sxs-lookup"><span data-stu-id="488f2-166">Once hello build succeeds, start hello website by pressing **F5**.</span></span> <span data-ttu-id="488f2-167">Ez elindítja a hello Flask fejlesztési kiszolgálót, és a webböngészőt.</span><span class="sxs-lookup"><span data-stu-id="488f2-167">This launches hello Flask development server and starts your web browser.</span></span> <span data-ttu-id="488f2-168">A következő lap hello kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="488f2-168">You should see hello following page.</span></span>
   
    ![hello üres Python Flask webes fejlesztési projekt a böngészőben megjelenített](./media/documentdb-python-application/image12.png)
3. <span data-ttu-id="488f2-170">Nyomja le a hello webhely hibakeresésének leállításához **Shift**+**F5** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="488f2-170">Stop debugging hello website by pressing **Shift**+**F5** in Visual Studio.</span></span>

### <a name="create-database-collection-and-document-definitions"></a><span data-ttu-id="488f2-171">Adatbázis-, gyűjtemény- és dokumentum-definíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="488f2-171">Create database, collection, and document definitions</span></span>
<span data-ttu-id="488f2-172">Ideje létrehozni a szavazóalkalmazást az új fájlok hozzáadásával, valamint a többi fájl frissítésével.</span><span class="sxs-lookup"><span data-stu-id="488f2-172">Now let's create your voting application by adding new files and updating others.</span></span>

1. <span data-ttu-id="488f2-173">A Megoldáskezelőben kattintson a jobb gombbal hello **oktatóanyag** projektre, kattintson **Hozzáadás**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="488f2-173">In Solution Explorer, right-click hello **tutorial** project, click **Add**, and then click **New Item**.</span></span> <span data-ttu-id="488f2-174">Válassza ki **üres Python-fájl** és nevű hello fájl **forms.py**.</span><span class="sxs-lookup"><span data-stu-id="488f2-174">Select **Empty Python File** and name hello file **forms.py**.</span></span>  
2. <span data-ttu-id="488f2-175">Adja hozzá a következő kód toohello forms.py fájl hello, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="488f2-175">Add hello following code toohello forms.py file, and then save hello file.</span></span>

```python
from flask.ext.wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-hello-required-imports-tooviewspy"></a><span data-ttu-id="488f2-176">Adja hozzá a szükséges hello importálja tooviews.py</span><span class="sxs-lookup"><span data-stu-id="488f2-176">Add hello required imports tooviews.py</span></span>
1. <span data-ttu-id="488f2-177">A Megoldáskezelőben bontsa ki a hello **oktatóanyag** mappára, majd nyissa meg hello **views.py** fájlt.</span><span class="sxs-lookup"><span data-stu-id="488f2-177">In Solution Explorer, expand hello **tutorial** folder, and open hello **views.py** file.</span></span> 
2. <span data-ttu-id="488f2-178">Adja hozzá a következő importálási utasítások toohello felső részén hello hello **views.py** fájlt, majd mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="488f2-178">Add hello following import statements toohello top of hello **views.py** file, then save hello file.</span></span> <span data-ttu-id="488f2-179">Ezek importálása Cosmos DB Python SDK-IT és hello Flask-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="488f2-179">These import Cosmos DB's PythonSDK and hello Flask packages.</span></span>
   
    ```python
    from forms import VoteForm
    import config
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a><span data-ttu-id="488f2-180">Adatbázisok, gyűjtemények és dokumentumok létrehozása</span><span class="sxs-lookup"><span data-stu-id="488f2-180">Create database, collection, and document</span></span>
* <span data-ttu-id="488f2-181">Még mindig **views.py**, adja hozzá a következő kód toohello hello fájl vége hello.</span><span class="sxs-lookup"><span data-stu-id="488f2-181">Still in **views.py**, add hello following code toohello end of hello file.</span></span> <span data-ttu-id="488f2-182">Ezzel létrehozza hello űrlap által használt hello adatbázist hoz létre.</span><span class="sxs-lookup"><span data-stu-id="488f2-182">This takes care of creating hello database used by hello form.</span></span> <span data-ttu-id="488f2-183">Ne törölje a meglévő kód hello **views.py**.</span><span class="sxs-lookup"><span data-stu-id="488f2-183">Do not delete any of hello existing code in **views.py**.</span></span> <span data-ttu-id="488f2-184">Egyszerűen csak fűzze hozzá toohello ennek.</span><span class="sxs-lookup"><span data-stu-id="488f2-184">Simply append this toohello end.</span></span>

```python
@app.route('/create')
def create():
    """Renders hello contact page."""
    client = document_client.DocumentClient(config.DOCUMENTDB_HOST, {'masterKey': config.DOCUMENTDB_KEY})

    # Attempt toodelete hello database.  This allows this toobe used toorecreate as well as create
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


### <a name="read-database-collection-document-and-submit-form"></a><span data-ttu-id="488f2-185">Adatbázis, gyűjtemény és dokumentum beolvasása, valamint az űrlap elküldése</span><span class="sxs-lookup"><span data-stu-id="488f2-185">Read database, collection, document, and submit form</span></span>
* <span data-ttu-id="488f2-186">Még mindig **views.py**, adja hozzá a következő kód toohello hello fájl vége hello.</span><span class="sxs-lookup"><span data-stu-id="488f2-186">Still in **views.py**, add hello following code toohello end of hello file.</span></span> <span data-ttu-id="488f2-187">Ezzel létrehozza hello űrlap hello adatbázis, gyűjtemény és dokumentum olvasása beállítása.</span><span class="sxs-lookup"><span data-stu-id="488f2-187">This takes care of setting up hello form, reading hello database, collection, and document.</span></span> <span data-ttu-id="488f2-188">Ne törölje a meglévő kód hello **views.py**.</span><span class="sxs-lookup"><span data-stu-id="488f2-188">Do not delete any of hello existing code in **views.py**.</span></span> <span data-ttu-id="488f2-189">Egyszerűen csak fűzze hozzá toohello ennek.</span><span class="sxs-lookup"><span data-stu-id="488f2-189">Simply append this toohello end.</span></span>

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

        # Take hello data from hello deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model toopass tooresults.html
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


### <a name="create-hello-html-files"></a><span data-ttu-id="488f2-190">Hello HTML-fájlok létrehozása</span><span class="sxs-lookup"><span data-stu-id="488f2-190">Create hello HTML files</span></span>
1. <span data-ttu-id="488f2-191">A Megoldáskezelőben a hello **oktatóanyag** mappa, a jobb oldali kattintson hello **sablonok** mappát, kattintson a **hozzáadása**, és kattintson a **új elem**.</span><span class="sxs-lookup"><span data-stu-id="488f2-191">In Solution Explorer, in hello **tutorial** folder, right click hello **templates** folder, click **Add**, and then click **New Item**.</span></span> 
2. <span data-ttu-id="488f2-192">Válassza ki **HTML-weblap**, majd a hello név mezőbe írja be a **create.html**.</span><span class="sxs-lookup"><span data-stu-id="488f2-192">Select **HTML Page**, and then in hello name box type **create.html**.</span></span> 
3. <span data-ttu-id="488f2-193">Ismételje meg az 1. és 2 toocreate két további HTML-fájlok: ezek a results.html és a vote.html.</span><span class="sxs-lookup"><span data-stu-id="488f2-193">Repeat steps 1 and 2 toocreate two additional HTML files: results.html and vote.html.</span></span>
4. <span data-ttu-id="488f2-194">Adja hozzá a következő kód túl hello**create.html** a hello `<body>` elemet.</span><span class="sxs-lookup"><span data-stu-id="488f2-194">Add hello following code too**create.html** in hello `<body>` element.</span></span> <span data-ttu-id="488f2-195">Ez megjelenít egy üzenetet, miszerint sikeresen létrehozott egy új adatbázist, gyűjteményt és dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="488f2-195">It displays a message stating that we created a new database, collection, and document.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. <span data-ttu-id="488f2-196">Adja hozzá a következő kód túl hello**results.html** a hello `<body`> elemet.</span><span class="sxs-lookup"><span data-stu-id="488f2-196">Add hello following code too**results.html** in hello `<body`> element.</span></span> <span data-ttu-id="488f2-197">Hello hello lekérdezési eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="488f2-197">It displays hello results of hello poll.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of hello vote</h2>
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
6. <span data-ttu-id="488f2-198">Adja hozzá a következő kód túl hello**vote.html** a hello `<body`> elemet.</span><span class="sxs-lookup"><span data-stu-id="488f2-198">Add hello following code too**vote.html** in hello `<body`> element.</span></span> <span data-ttu-id="488f2-199">Hello lekérdezési jeleníti meg, és elfogadja hello szavazatot.</span><span class="sxs-lookup"><span data-stu-id="488f2-199">It displays hello poll and accepts hello votes.</span></span> <span data-ttu-id="488f2-200">Hello szavazatok regisztrálása, hello vezérlő tooviews.py, ahol rendszer hello leadott ismeri fel és ennek megfelelően hozzáfűzése hello dokumentum keresztül lett átadva.</span><span class="sxs-lookup"><span data-stu-id="488f2-200">On registering hello votes, hello control is passed over tooviews.py where we will recognize hello vote cast and append hello document accordingly.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way toohost an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. <span data-ttu-id="488f2-201">A hello **sablonok** mappa, a név felülírandó hello tartalmát **index.html** hello következőre.</span><span class="sxs-lookup"><span data-stu-id="488f2-201">In hello **templates** folder, replace hello contents of **index.html** with hello following.</span></span> <span data-ttu-id="488f2-202">Ez az alkalmazás kezdőlapján hello funkcionál.</span><span class="sxs-lookup"><span data-stu-id="488f2-202">This serves as hello landing page for your application.</span></span>
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear hello Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-hello-initpy"></a><span data-ttu-id="488f2-203">Konfigurációs fájl felvétele és módosítása hello \_ \_init\_\_.py</span><span class="sxs-lookup"><span data-stu-id="488f2-203">Add a configuration file and change hello \_\_init\_\_.py</span></span>
1. <span data-ttu-id="488f2-204">A Megoldáskezelőben kattintson a jobb gombbal hello **oktatóanyag** projektre, kattintson **hozzáadása**, kattintson **új elem**, jelölje be **üres Python-fájl**, majd nevű hello fájl **config.py**.</span><span class="sxs-lookup"><span data-stu-id="488f2-204">In Solution Explorer, right-click hello **tutorial** project, click **Add**, click **New Item**, select **Empty Python File**, and then name hello file **config.py**.</span></span> <span data-ttu-id="488f2-205">A Flask űrlapjainak szüksége van erre a konfigurációs fájlra.</span><span class="sxs-lookup"><span data-stu-id="488f2-205">This config file is required by forms in Flask.</span></span> <span data-ttu-id="488f2-206">Használat tooprovide egy titkos kulcsot is.</span><span class="sxs-lookup"><span data-stu-id="488f2-206">You can use it tooprovide a secret key as well.</span></span> <span data-ttu-id="488f2-207">A jelen oktatóanyaghoz azonban nincs szükség ilyen kulcsra.</span><span class="sxs-lookup"><span data-stu-id="488f2-207">This key is not needed for this tutorial though.</span></span>
2. <span data-ttu-id="488f2-208">Adja hozzá a következő hello kód tooconfig.py, szüksége lesz tooalter hello értékének **DOCUMENTDB\_állomás** és **DOCUMENTDB\_kulcs** hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="488f2-208">Add hello following code tooconfig.py, you'll need tooalter hello values of **DOCUMENTDB\_HOST** and **DOCUMENTDB\_KEY** in hello next step.</span></span>
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    DOCUMENTDB_HOST = 'https://YOUR_DOCUMENTDB_NAME.documents.azure.com:443/'
    DOCUMENTDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    DOCUMENTDB_DATABASE = 'voting database'
    DOCUMENTDB_COLLECTION = 'voting collection'
    DOCUMENTDB_DOCUMENT = 'voting document'
    ```
3. <span data-ttu-id="488f2-209">Hello a [Azure-portálon](https://portal.azure.com/), keresse meg a toohello **kulcsok** panelre. Ehhez kattintson **Tallózás**, **Azure Cosmos DB fiókok**, kattintson duplán a hello neve a hello toouse fiókra, majd hello **kulcsok** hello gombjára **Essentials** területen.</span><span class="sxs-lookup"><span data-stu-id="488f2-209">In hello [Azure portal](https://portal.azure.com/), navigate toohello **Keys** blade by clicking **Browse**, **Azure Cosmos DB Accounts**, double-click hello name of hello account toouse, and then click hello **Keys** button in hello **Essentials** area.</span></span> <span data-ttu-id="488f2-210">A hello **kulcsok** panelen, a Másolás hello **URI** értékét, és illessze be hello **config.py** fájl hello hello értékként **DOCUMENTDB\_állomás**  tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="488f2-210">In hello **Keys** blade, copy hello **URI** value and paste it into hello **config.py** file, as hello value for hello **DOCUMENTDB\_HOST** property.</span></span> 
4. <span data-ttu-id="488f2-211">Vissza az Azure portálra, az hello hello **kulcsok** panelen másolási hello értékének hello **elsődleges kulcs** vagy hello **másodlagos kulcs**, és illessze be hello **config.py**  fájl hello hello értékként **DOCUMENTDB\_kulcs** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="488f2-211">Back in hello Azure portal, in hello **Keys** blade, copy hello value of hello **Primary Key** or hello **Secondary Key**, and paste it into hello **config.py** file, as hello value for hello **DOCUMENTDB\_KEY** property.</span></span>
5. <span data-ttu-id="488f2-212">A hello  **\_ \_init\_\_.py** fájlt, adja hozzá a következő sor hello.</span><span class="sxs-lookup"><span data-stu-id="488f2-212">In hello **\_\_init\_\_.py** file, add hello following line.</span></span> 
   
        app.config.from_object('config')
   
    <span data-ttu-id="488f2-213">Így hello hello fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="488f2-213">So that hello content of hello file is:</span></span>
   
    ```python
    from flask import Flask
    app = Flask(__name__)
    app.config.from_object('config')
    import tutorial.views
    ```
6. <span data-ttu-id="488f2-214">A felvett összes hello fájlok, Megoldáskezelőben kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="488f2-214">After adding all hello files, Solution Explorer should look like this:</span></span>
   
    ![Képernyőfelvétel a hello Visual Studio Solution Explorer ablak](./media/documentdb-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a><span data-ttu-id="488f2-216">4. lépés: A webalkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="488f2-216">Step 4: Run your web application locally</span></span>
1. <span data-ttu-id="488f2-217">Hello megoldás kiépítését, billentyűkombináció lenyomásával **Ctrl**+**Shift**+**B**.</span><span class="sxs-lookup"><span data-stu-id="488f2-217">Build hello solution by pressing **Ctrl**+**Shift**+**B**.</span></span>
2. <span data-ttu-id="488f2-218">Sikeres hello fordítás után indítsa el a webhelyet hello billentyűkombináció lenyomásával **F5**.</span><span class="sxs-lookup"><span data-stu-id="488f2-218">Once hello build succeeds, start hello website by pressing **F5**.</span></span> <span data-ttu-id="488f2-219">Hello következő kell megjelennie a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="488f2-219">You should see hello following on your screen.</span></span>
   
    ![Képernyőfelvétel a hello Python + Azure Cosmos DB Szavazóalkalmazásról megjelenik a webböngészőben](./media/documentdb-python-application/cosmos-db-pythonr-run-application.png)
3. <span data-ttu-id="488f2-221">Kattintson a **Létrehozás/törlés hello szavazás adatbázis** toogenerate hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="488f2-221">Click **Create/Clear hello Voting Database** toogenerate hello database.</span></span>
   
    ![Képernyőfelvétel a hello létrehozása lap hello webalkalmazás – fejlesztési részletek](./media/documentdb-python-application/cosmos-db-python-run-create-page.png)
4. <span data-ttu-id="488f2-223">Ezután kattintson a **Vote** (Szavazás) elemre, és válassza ki a kívánt elemet.</span><span class="sxs-lookup"><span data-stu-id="488f2-223">Then, click **Vote** and select your option.</span></span>
   
    ![Képernyőfelvétel a hello webalkalmazásról és a szavazási kérdés feltételéről](./media/documentdb-python-application/cosmos-db-vote.png)
5. <span data-ttu-id="488f2-225">Minden leadott szavazattal az hello megfelelő számlálót növeli.</span><span class="sxs-lookup"><span data-stu-id="488f2-225">For every vote you cast, it increments hello appropriate counter.</span></span>
   
    ![Képernyőfelvétel a hello hello szavazási lapon látható eredményei](./media/documentdb-python-application/cosmos-db-voting-results.png)
6. <span data-ttu-id="488f2-227">Nyomja le a Shift + F5 hello projekt hibakeresésének leállításához.</span><span class="sxs-lookup"><span data-stu-id="488f2-227">Stop debugging hello project by pressing Shift+F5.</span></span>

## <a name="step-5-deploy-hello-web-application-tooazure"></a><span data-ttu-id="488f2-228">5. lépés: Hello webes alkalmazás tooAzure telepítése</span><span class="sxs-lookup"><span data-stu-id="488f2-228">Step 5: Deploy hello web application tooAzure</span></span>
<span data-ttu-id="488f2-229">Most, hogy hello teljes alkalmazás megfelelően működik-e Cosmos DB szemben, az oktatóanyagban módosítjuk toodeploy a tooAzure.</span><span class="sxs-lookup"><span data-stu-id="488f2-229">Now that you have hello complete application working correctly against Cosmos DB, we're going toodeploy this tooAzure.</span></span>

1. <span data-ttu-id="488f2-230">Kattintson a jobb gombbal hello projektre a Solution Explorer (Győződjön meg arról, hogy Ön nem továbbra is helyben fut), majd **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="488f2-230">Right-click hello project in Solution Explorer (make sure you're not still running it locally) and select **Publish**.</span></span>  
   
     ![Képernyőfelvétel a hello "tutorial" projektről a Solution Explorer hello közzététel lehetőséggel kiemelve](./media/documentdb-python-application/image20.png)
2. <span data-ttu-id="488f2-232">A hello **közzététel** párbeszédpanelen jelölje ki **Microsoft Azure App Service**, jelölje be **hozzon létre új**, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="488f2-232">In hello **Publish** dialog box, select **Microsoft Azure App Service**, select **Create New**, and then click **Publish**.</span></span>
   
    ![Képernyőfelvétel a hello webhely közzététele ablak kiemelt Microsoft Azure App Service szolgáltatással](./media/documentdb-python-application/cosmos-db-python-publish.png)
3. <span data-ttu-id="488f2-234">A hello **létrehozása az App Service** párbeszédpanelen adja meg a webalkalmazás, valamint hello nevét a **előfizetés**, **erőforráscsoport**, és **App Service-csomag** , majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="488f2-234">In hello **Create App Service** dialog box, enter hello name for your web app along with your **Subscription**, **Resource Group**, and **App Service Plan**, then click **Create**.</span></span>
   
    ![Képernyőfelvétel a hello Microsoft Azure Web Apps ablak ablak](./media/documentdb-python-application/cosmos-db-python-create-app-service.png)
4. <span data-ttu-id="488f2-236">Néhány másodpercen belül a Visual Studio befejezi az app service közzétételét, és elindít egy böngészőt, ahol láthatja az Azure-beli handiwork!</span><span class="sxs-lookup"><span data-stu-id="488f2-236">In a few seconds, Visual Studio will finish publishing your app service and launch a browser where you can see your handiwork running in Azure!</span></span>

    ![Képernyőfelvétel a hello Microsoft Azure Web Apps ablak ablak](./media/documentdb-python-application/cosmos-db-python-appservice-created.png)

## <a name="troubleshooting"></a><span data-ttu-id="488f2-238">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="488f2-238">Troubleshooting</span></span>
<span data-ttu-id="488f2-239">Ha ez hello első Python-alkalmazás futtatását a számítógépen, győződjön meg arról, hogy hello következő mappák (vagy hello azokkal egyenértékű telepítési helyek) szerepelnek a PATH változóban:</span><span class="sxs-lookup"><span data-stu-id="488f2-239">If this is hello first Python app you've run on your computer, ensure that hello following folders (or hello equivalent installation locations) are included in your PATH variable:</span></span>

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

<span data-ttu-id="488f2-240">Ha hibaüzenetet kap a szavazási lapon, és elnevezett a projekt valami eltérő **oktatóanyag**, győződjön meg arról, hogy  **\_ \_init\_\_.py** hivatkozások hello megfelelő projektnévre hello sorban: `import tutorial.view`.</span><span class="sxs-lookup"><span data-stu-id="488f2-240">If you receive an error on your vote page, and you named your project something other than **tutorial**, make sure that **\_\_init\_\_.py** references hello correct project name in hello line: `import tutorial.view`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="488f2-241">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="488f2-241">Next steps</span></span>
<span data-ttu-id="488f2-242">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="488f2-242">Congratulations!</span></span> <span data-ttu-id="488f2-243">Ebben az esetben az első Python webes alkalmazás Cosmos DB használatával befejeződött, és közzétette azt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="488f2-243">You have just completed your first Python web application using Cosmos DB and published it tooAzure.</span></span>

<span data-ttu-id="488f2-244">Gyakran frissítjük és javítjuk a jelen témakört a visszajelzések alapján.</span><span class="sxs-lookup"><span data-stu-id="488f2-244">We update and improve this topic frequently based on your feedback.</span></span>  <span data-ttu-id="488f2-245">Egyszer, elsajátította hello oktatóanyagban hello szavazás hello felső és a lap alján gombok használatával, és lehet, hogy tooinclude Várjuk visszajelzését a végrehajtott toosee kívánt milyen fejlesztéseket.</span><span class="sxs-lookup"><span data-stu-id="488f2-245">Once you've completed hello tutorial, please using hello voting buttons at hello top and bottom of this page, and be sure tooinclude your feedback on what improvements you want toosee made.</span></span> <span data-ttu-id="488f2-246">Ha szeretné toocontact úgy közvetlenül, érzi, hogy az e-mail cím szabad tooinclude a megjegyzéseit.</span><span class="sxs-lookup"><span data-stu-id="488f2-246">If you'd like us toocontact you directly, feel free tooinclude your email address in your comments.</span></span>

<span data-ttu-id="488f2-247">tooadd további funkciók tooyour webalkalmazás, tekintse át hello hello elérhető API-k [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span><span class="sxs-lookup"><span data-stu-id="488f2-247">tooadd additional functionality tooyour web application, review hello APIs available in hello [Azure Cosmos DB Python SDK](documentdb-sdk-python.md).</span></span>

<span data-ttu-id="488f2-248">Azure, a Visual Studio és a Pythonnal kapcsolatos további információkért lásd: hello [Python fejlesztői központ](https://azure.microsoft.com/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="488f2-248">For more information about Azure, Visual Studio, and Python, see hello [Python Developer Center](https://azure.microsoft.com/develop/python/).</span></span> 

<span data-ttu-id="488f2-249">További további Python Flask-oktatóanyagok: [hello Flask Mega-oktatóanyagban rész I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span><span class="sxs-lookup"><span data-stu-id="488f2-249">For additional Python Flask tutorials, see [hello Flask Mega-Tutorial, Part I: Hello, World!](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world).</span></span> 

[Visual Studio Express]: http://www.visualstudio.com/products/visual-studio-express-vs.aspx
[2]: https://www.python.org/downloads/windows/
[3]: https://www.microsoft.com/download/details.aspx?id=44266
[Microsoft Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[Azure portal]: http://portal.azure.com
