---
title: "aaaTen lehetősége van az adatok tudományos virtuális gép hello |} Microsoft Docs"
description: "Hajtsa végre különböző adatok feltárása és modellezési feladat hello adattudomány virtuális gépet."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 145dfe3e-2bd2-478f-9b6e-99d97d789c62
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: gokuma;weig;bradsev
ms.openlocfilehash: 4dfe22f14f00208c63e26ce44b05123c9ac4b850
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ten-things-you-can-do-on-hello-data-science-virtual-machine"></a><span data-ttu-id="220cd-103">Tíz végezhető műveletek hello adatok tudományos virtuális gép</span><span class="sxs-lookup"><span data-stu-id="220cd-103">Ten things you can do on hello Data science Virtual Machine</span></span>
<span data-ttu-id="220cd-104">hello Microsoft adatok tudományos virtuális gép (DSVM) olyan hatékony tudományos fejlesztési környezet, amely lehetővé teszi az Ön tooperform különböző adatok feltárása és modellezési feladatokat.</span><span class="sxs-lookup"><span data-stu-id="220cd-104">hello Microsoft Data Science Virtual Machine (DSVM) is a powerful data science development environment that enables you tooperform various data exploration and modeling tasks.</span></span> <span data-ttu-id="220cd-105">hello környezet származik, már beépített csomagolt számos népszerű adatokkal, melyek könnyen tooget elemzőeszközök használatának gyors megkezdése az elemzési helyszíni, felhőalapú vagy hibrid telepítések.</span><span class="sxs-lookup"><span data-stu-id="220cd-105">hello environment comes already built and bundled with several popular data analytics tools that make it easy tooget started quickly with your analysis for On-premises, Cloud or hybrid deployments.</span></span> <span data-ttu-id="220cd-106">hello DSVM szorosan együttműködik olyan sok Azure-szolgáltatást, és képes tooread és a folyamat a Azure-ban az Azure SQL Data Warehouse, az Azure Data Lake, az Azure Storage, vagy az Azure Cosmos Adatbázisba már tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="220cd-106">hello DSVM works closely with many Azure services and is able tooread and process data that is already stored on Azure, in Azure SQL Data Warehouse, Azure Data Lake, Azure Storage, or in Azure Cosmos DB.</span></span> <span data-ttu-id="220cd-107">Azt is kihasználhatják a más analytics eszközök, például az Azure Machine Learning és az Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="220cd-107">It can also leverage other analytics tools such as Azure Machine Learning and Azure Data Factory.</span></span>

<span data-ttu-id="220cd-108">Ebben a cikkben azt végigvezetik Önt toouse a DSVM tooperform különböző adattudomány feladatokat, és más Azure-szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="220cd-108">In this article we walk you through how toouse your DSVM tooperform various data science tasks and interact with other Azure services.</span></span> <span data-ttu-id="220cd-109">Az alábbiakban néhány hello DSVM elvégezhető hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="220cd-109">Here are some of hello things you can do on hello DSVM:</span></span>

1. <span data-ttu-id="220cd-110">Adatok vizsgálatát, és helyileg az hello segítségével a Microsoft R Server, Python DSVM modellek fejlesztése</span><span class="sxs-lookup"><span data-stu-id="220cd-110">Explore data and develop models locally on hello DSVM using Microsoft R Server, Python</span></span>
2. <span data-ttu-id="220cd-111">A Jupyter notebook tooexperiment használ az adatok egy böngésző Python 2, Python 3, Microsoft R méretezhetőség és teljesítmény készült R készen áll a vállalati verziójának használata</span><span class="sxs-lookup"><span data-stu-id="220cd-111">Use a Jupyter notebook tooexperiment with your data on a browser using Python 2, Python 3, Microsoft R an enterprise ready version of R designed for scalability and performance</span></span>
3. <span data-ttu-id="220cd-112">Azok a modellek használatával készített R és Python az Azure Machine Learning szolgáltatást az ügyfélalkalmazások számára a szolgáltatások egyszerű webes felületen keresztül modellek</span><span class="sxs-lookup"><span data-stu-id="220cd-112">Operationalize models built using R and Python on Azure Machine Learning so client applications can access your models using a simple web services interface</span></span>
4. <span data-ttu-id="220cd-113">Felügyelheti az Azure-erőforrások Azure-portálon vagy a Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="220cd-113">Administer your Azure resources using  Azure portal or Powershell</span></span>
5. <span data-ttu-id="220cd-114">A tárhely kiterjesztése és nagy méretű adatkészletek megosztása / hozzon létre egy Azure File storage a DSVM csatlakoztatható meghajtóként között a teljes csoport kód</span><span class="sxs-lookup"><span data-stu-id="220cd-114">Extend your storage space and share large-scale datasets / code across your whole team by creating an Azure File storage as a mountable drive on your DSVM</span></span>
6. <span data-ttu-id="220cd-115">Kód megoszthatja a Githubon használatával munkatársaival, és hozzáférni a tárházat hello előre telepített Git ügyfelek - Git Bash Git grafikus felhasználói felület használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-115">Share code with your team using GitHub and access your repository using hello pre-installed Git clients - Git Bash, Git GUI.</span></span>
7. <span data-ttu-id="220cd-116">Hozzáférjen a különböző Azure adatok és analitikák szolgáltatásokhoz hasonlóan az Azure blob storage Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & adatbázisok</span><span class="sxs-lookup"><span data-stu-id="220cd-116">Access various Azure data and analytics services like Azure blob storage, Azure Data Lake, Azure HDInsight (Hadoop), Azure Cosmos DB, Azure SQL Data Warehouse & databases</span></span>
8. <span data-ttu-id="220cd-117">Jelentéseket és hello előtelepített hello DSVM a Power BI Desktop használatával hozhat létre és telepíthet hello felhő</span><span class="sxs-lookup"><span data-stu-id="220cd-117">Build reports and dashboard using hello Power BI Desktop pre-installed on hello DSVM and deploy them on hello cloud</span></span>
9. <span data-ttu-id="220cd-118">A DSVM toomeet a projektet dinamikusan méretezése</span><span class="sxs-lookup"><span data-stu-id="220cd-118">Dynamically scale your DSVM toomeet your project needs</span></span>
10. <span data-ttu-id="220cd-119">A virtuális gép további eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="220cd-119">Install additional tools on your virtual machine</span></span>   

> [!NOTE]
> <span data-ttu-id="220cd-120">További használati díjak hello további adatok tárolási és elemzési szolgáltatások a cikkben szereplő számos vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="220cd-120">Additional usage charges apply for many of hello additional data storage and analytics services listed in this article.</span></span> <span data-ttu-id="220cd-121">Tekintse meg a toohello [Azure díjszabása](https://azure.microsoft.com/pricing/) a lapot.</span><span class="sxs-lookup"><span data-stu-id="220cd-121">Please refer toohello [Azure Pricing](https://azure.microsoft.com/pricing/) page for details.</span></span>
> 
> 

<span data-ttu-id="220cd-122">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="220cd-122">**Prerequisites**</span></span>

* <span data-ttu-id="220cd-123">Azure-előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="220cd-123">You need an Azure subscription.</span></span> <span data-ttu-id="220cd-124">Regisztrálhat egy ingyenes próbaverzióra [Itt](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="220cd-124">You can sign up for a free trial [here](https://azure.microsoft.com/free/).</span></span>
* <span data-ttu-id="220cd-125">Hello Azure-portálon található adatok tudományos virtuális géphez történő üzembe helyezéséhez utasításokat érhetők el [egy virtuális gép létrehozása](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span><span class="sxs-lookup"><span data-stu-id="220cd-125">Instructions for provisioning a Data Science Virtual Machine on hello Azure portal are available at [Creating a virtual machine](https://portal.azure.com/#create/microsoft-ads.standard-data-science-vmstandard-data-science-vm).</span></span>

## <a name="1-explore-data-and-develop-models-using-microsoft-r-server-or-python"></a><span data-ttu-id="220cd-126">1. Adatok vizsgálatát, és a Microsoft R Server vagy a Python modellek fejlesztése</span><span class="sxs-lookup"><span data-stu-id="220cd-126">1. Explore data and develop models using Microsoft R Server or Python</span></span>
<span data-ttu-id="220cd-127">Használhat olyan nyelveket R és Python toodo hasonlóan a adatelemzés jogosultsággal a hello DSVM.</span><span class="sxs-lookup"><span data-stu-id="220cd-127">You can use languages like R and Python toodo your data analytics right on hello DSVM.</span></span>

<span data-ttu-id="220cd-128">Az R egy IDE "8.0 fordulat R vállalati" nevű hello start menüből vagy az asztal hello található is használhatja.</span><span class="sxs-lookup"><span data-stu-id="220cd-128">For R, you can use an IDE called "Revolution R Enterprise 8.0" that can be found on hello start menu or hello desktop.</span></span> <span data-ttu-id="220cd-129">A Microsoft közzétett további könyvtárak hello nyitott forrás/CRAN-R tooenable méretezhető elemzés és hello képességét tooanalyze adatok párhuzamos darabolt elemzés végrehajtásával engedélyezett hello memória mérete nagyobb felett.</span><span class="sxs-lookup"><span data-stu-id="220cd-129">Microsoft has provided additional libraries on top of hello Open source/CRAN-R tooenable scalable analytics and hello ability tooanalyze data larger than hello memory size allowed by doing parallel chunked analysis.</span></span> <span data-ttu-id="220cd-130">A choice hasonló egy R IDE is telepíthet [Rstudióból](https://www.rstudio.com/products/rstudio-desktop/).</span><span class="sxs-lookup"><span data-stu-id="220cd-130">You can also install an R IDE of your choice like [RStudio](https://www.rstudio.com/products/rstudio-desktop/).</span></span>

<span data-ttu-id="220cd-131">A Python például a Visual Studio Community Edition hello Python Tools előre telepített Visual Studio (PTVS) kiterjesztés rendelkező egy IDE használhatják.</span><span class="sxs-lookup"><span data-stu-id="220cd-131">For Python, you can use an IDE like Visual Studio Community Edition which has hello Python Tools for Visual Studio (PTVS) extension pre-installed.</span></span> <span data-ttu-id="220cd-132">Alapértelmezés szerint csak egy alapszintű Python 2.7 van konfigurálva a PTVS (nélkül analytics függvénytárat, például a SciKit, Pandas).</span><span class="sxs-lookup"><span data-stu-id="220cd-132">By default, only a basic Python 2.7 is configured on PTVS (without any analytics library like SciKit, Pandas).</span></span> <span data-ttu-id="220cd-133">A sorrend tooenable Anaconda Python 2.7-es és 3.5-ös verzióját toodo hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="220cd-133">In order tooenable Anaconda Python 2.7 and 3.5, you need toodo hello following:</span></span>

* <span data-ttu-id="220cd-134">Hozzon létre egyéni környezetek egyes verzióihoz útvonalon túl**eszközök** -> **Python Tools** -> **Python-környezetek** , majd kattintson " **+ Egyéni**"a Visual Studio 2015 Community Edition hello</span><span class="sxs-lookup"><span data-stu-id="220cd-134">Create custom environments for each version by navigating too**Tools** -> **Python Tools** -> **Python Environments** and then clicking "**+ Custom**" in hello Visual Studio 2015 Community Edition</span></span>
* <span data-ttu-id="220cd-135">Adjon meg leírást, majd állítsa be a hello környezet előtag elérési utakat *c:\anaconda* Anaconda Python 2.7 vagy *c:\anaconda\envs\py35* a Anaconda Python 3.5</span><span class="sxs-lookup"><span data-stu-id="220cd-135">Give a description and set hello environment prefix paths as *c:\anaconda* for Anaconda Python 2.7 OR *c:\anaconda\envs\py35* for Anaconda Python 3.5</span></span>
* <span data-ttu-id="220cd-136">Kattintson a **az automatikus észlelés** , majd **alkalmaz** toosave hello környezetben.</span><span class="sxs-lookup"><span data-stu-id="220cd-136">Click **Auto Detect** and then **Apply** toosave hello environment.</span></span>

<span data-ttu-id="220cd-137">Ez milyen hello egyéni környezet telepítése a Visual Studio tűnik.</span><span class="sxs-lookup"><span data-stu-id="220cd-137">Here is what hello custom environment setup looks like in Visual Studio.</span></span>

![PTVS-telepítő](./media/machine-learning-data-science-vm-do-ten-things/PTVSSetup.png)

<span data-ttu-id="220cd-139">Lásd: hello [PVTS dokumentációban](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) a további részleteket a Python-környezetek toocreate.</span><span class="sxs-lookup"><span data-stu-id="220cd-139">See hello [PTVS documentation](https://github.com/Microsoft/PTVS/wiki/Selecting-and-Installing-Python-Interpreters#hey-i-already-have-an-interpreter-on-my-machine-but-ptvs-doesnt-seem-to-know-about-it) for additional details on how toocreate Python Environments.</span></span>

<span data-ttu-id="220cd-140">Most már elkészült toocreate egy új Python-projektet.</span><span class="sxs-lookup"><span data-stu-id="220cd-140">Now you are set up toocreate a new Python project.</span></span> <span data-ttu-id="220cd-141">Keresse meg a túl**fájl** -> **új** -> **projekt** -> **Python** és hello típusának kiválasztása Python alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="220cd-141">Navigate too**File** -> **New** -> **Project** -> **Python** and select hello type of Python application you are building.</span></span> <span data-ttu-id="220cd-142">Hello Python-környezetben hello aktuális projekt toohello kívánt verziójával (Anaconda 2.7 vagy 3.5-ös) állíthatja be: kattintson a jobb gombbal hello **Python-környezetben**, jelölje be **hozzáadása Python-környezetek**, és Ezután válassza hello szükséges környezet tooassociate hello projekt esetében.</span><span class="sxs-lookup"><span data-stu-id="220cd-142">You can set hello Python environment for hello current project toohello desired version (Anaconda 2.7 or 3.5): right-click hello **Python environment**, select **Add/Remove Python Environments**, and then select hello desired environment tooassociate with hello project.</span></span> <span data-ttu-id="220cd-143">PTVS hello termék kezelésével kapcsolatos további információ található [dokumentáció](https://github.com/Microsoft/PTVS/wiki) lap.</span><span class="sxs-lookup"><span data-stu-id="220cd-143">You can find more information about working with PTVS on hello product [documentation](https://github.com/Microsoft/PTVS/wiki) page.</span></span>

## <a name="2-using-a-jupyter-notebook-tooexplore-and-model-your-data-with-python-or-r"></a><span data-ttu-id="220cd-144">2. A Jupyter Notebook tooexplore és a modell adatait a Python vagy használó R</span><span class="sxs-lookup"><span data-stu-id="220cd-144">2. Using a Jupyter Notebook tooexplore and model your data with Python or R</span></span>
<span data-ttu-id="220cd-145">Jupyter Notebook hello egy hatékony környezetben, amely egy webböngésző-alapú "IDE" biztosít az adatok feltárása és modellezési.</span><span class="sxs-lookup"><span data-stu-id="220cd-145">hello Jupyter Notebook is a powerful environment that provides a browser-based "IDE" for data exploration and modeling.</span></span> <span data-ttu-id="220cd-146">Jupyter Notebook Python 2, Python 3 vagy R (nyílt forráskódú és hello Microsoft R Server) használhatja.</span><span class="sxs-lookup"><span data-stu-id="220cd-146">You can use Python 2, Python 3 or R (both Open Source and hello Microsoft R Server) in a Jupyter Notebook.</span></span>

<span data-ttu-id="220cd-147">Jupyter Notebook toolaunch hello kattintson a hello start menü ikonra / asztali ikon című **Jupyter Notebook**.</span><span class="sxs-lookup"><span data-stu-id="220cd-147">toolaunch hello Jupyter Notebook click on hello start menu icon / desktop icon titled **Jupyter Notebook**.</span></span> <span data-ttu-id="220cd-148">A hello DSVM is megkeresheti túl "https://localhost:9999 /" tooaccess hello Jupiter Notebookot.</span><span class="sxs-lookup"><span data-stu-id="220cd-148">On hello DSVM you can also browse too"https://localhost:9999/" tooaccess hello Jupiter Notebook.</span></span> <span data-ttu-id="220cd-149">Ha azt kéri a jelszót, használja a hello utasításokat ***hogyan toocreate hello Jupyter notebook kiszolgálón egy erős jelszót*** hello szakasza [rendelkezés hello Microsoft adatok tudományos virtuális gép](machine-learning-data-science-provision-vm.md)témakör toocreate erős jelszó tooaccess hello Jupyter notebook.</span><span class="sxs-lookup"><span data-stu-id="220cd-149">If it prompts you for a password, use instructions provided in hello ***How toocreate a strong password on hello Jupyter notebook server*** section of hello [Provision hello Microsoft Data Science Virtual Machine](machine-learning-data-science-provision-vm.md) topic toocreate a strong password tooaccess hello Jupyter notebook.</span></span> 

<span data-ttu-id="220cd-150">Hello notebook megnyitása után meg kell jelennie egy néhány példa notebookok, amelyek a hello DSVM előcsomagolt tartalmazó könyvtár.</span><span class="sxs-lookup"><span data-stu-id="220cd-150">Once you have opened hello notebook, you should see a directory that contains a few example notebooks that are pre-packaged into hello DSVM.</span></span> <span data-ttu-id="220cd-151">Most már a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="220cd-151">Now you can:</span></span>

* <span data-ttu-id="220cd-152">Kattintson a hello notebook toosee hello kódot.</span><span class="sxs-lookup"><span data-stu-id="220cd-152">click on hello notebook toosee hello code.</span></span>
* <span data-ttu-id="220cd-153">Minden cella végrehajtása billentyűkombináció lenyomásával **a SHIFT + ENTER**.</span><span class="sxs-lookup"><span data-stu-id="220cd-153">execute each cell by pressing **SHIFT-ENTER**.</span></span>
* <span data-ttu-id="220cd-154">hello teljes jegyzetfüzet kattintva elindítja **cella** -> **futtatása**</span><span class="sxs-lookup"><span data-stu-id="220cd-154">run hello entire notebook by clicking on **Cell** -> **Run**</span></span>
* <span data-ttu-id="220cd-155">Hozzon létre egy új notebook hello Jupyter ikon (bal felső sarokban) parancsra, majd **új** hello megfelelő, és kattintson a hello notebook nyelvi (más néven kernelek) gombra.</span><span class="sxs-lookup"><span data-stu-id="220cd-155">create a new notebook by clicking on hello Jupyter Icon (left top corner) and then clicking **New** button on hello right and then choosing hello notebook language (also known as kernels).</span></span>   

> [!NOTE]
> <span data-ttu-id="220cd-156">Jelenleg támogatjuk a Python 2.7, Python 3.5-ös és R. hello R kernel programozási támogatja mind nyílt forráskódú R, valamint hello vállalati méretezhető Microsoft R Server.</span><span class="sxs-lookup"><span data-stu-id="220cd-156">Currently we support Python 2.7, Python 3.5 and R. hello R kernel supports programming in both Open source R as well as hello enterprise scalable Microsoft R Server.</span></span>   
> 
> 

<span data-ttu-id="220cd-157">Miután belépett hello notebook az adatokba, hello modell létrehozása, szalagtárak tetszőleges hello modell teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="220cd-157">Once you are in hello notebook you can explore your data, build hello model, test hello model using your choice of libraries.</span></span>

## <a name="3-build-models-using-r-or-python-and-operationalize-them-using-azure-machine-learning"></a><span data-ttu-id="220cd-158">3. R vagy Python és használatával Operationalize őket az Azure Machine Learning modellek létrehozása</span><span class="sxs-lookup"><span data-stu-id="220cd-158">3. Build models using R or Python and Operationalize them using Azure Machine Learning</span></span>
<span data-ttu-id="220cd-159">Miután létrehozott és érvényesített hello következő lépésként modell általában toodeploy az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="220cd-159">Once you have built and validated your model hello next step is usually toodeploy it into production.</span></span> <span data-ttu-id="220cd-160">Ez lehetővé teszi az ügyfél alkalmazások tooinvoke hello modell előrejelzéseket egy valós idejű vagy kötegelt módban alapon.</span><span class="sxs-lookup"><span data-stu-id="220cd-160">This allows your client applications tooinvoke hello model predictions on a real time or on a batch mode basis.</span></span> <span data-ttu-id="220cd-161">Az Azure Machine Learning a beépített R vagy Python modellt biztosít egy mechanizmus toooperationalize.</span><span class="sxs-lookup"><span data-stu-id="220cd-161">Azure Machine Learning provides a mechanism toooperationalize a model built in either R or Python.</span></span>

<span data-ttu-id="220cd-162">Azok az az Azure Machine Learning modellje, amikor egy webszolgáltatás-bővítmény, amely lehetővé teszi az ügyfelek toomake REST-hívások adjon át bemeneti paraméterekkel és előrejelzéseket fogadása hello modellből kimenetek van közzétéve.</span><span class="sxs-lookup"><span data-stu-id="220cd-162">When you operationalize your model in Azure Machine Learning, a web service is exposed that allows clients toomake REST calls that pass in input parameters and receive predictions from hello model as outputs.</span></span>   

> [!NOTE]
> <span data-ttu-id="220cd-163">Ha nem még feliratkozott az Azure Machine Learning, ezt úgy szerezheti be egy szabad munkaterület vagy egy szabványos munkaterület hello felkeresésével [Azure Machine Learning Studio](https://studio.azureml.net/) kezdőlapját, majd kattintson az "első lépések".</span><span class="sxs-lookup"><span data-stu-id="220cd-163">If you have not yet signed up for Azure Machine Learning, you can obtain a free workspace or a standard workspace by visiting hello [Azure Machine Learning Studio](https://studio.azureml.net/) home page and clicking on "Get Started".</span></span>   
> 
> 

### <a name="build-and-operationalize-python-models"></a><span data-ttu-id="220cd-164">Buildelés és azok Python modellek</span><span class="sxs-lookup"><span data-stu-id="220cd-164">Build and Operationalize Python models</span></span>
<span data-ttu-id="220cd-165">Íme egy hello SciKit további szalagtárat használó egyszerű modell épülő Python Jupyter Notebook létre kódrészletét.</span><span class="sxs-lookup"><span data-stu-id="220cd-165">Here is a snippet of code developed in a Python Jupyter Notebook that builds a simple model using hello SciKit-learn library.</span></span>

    #IRIS classification
    from sklearn import datasets
    from sklearn import svm
    clf = svm.SVC()
    iris = datasets.load_iris()
    X, y = iris.data, iris.target
    clf.fit(X, y)

<span data-ttu-id="220cd-166">hello metódus használt toodeploy a python-modellek tooAzure Machine Learning becsomagolja hello előrejelzését hello modell egy függvénynek, és azt hello előre telepített Azure Machine Learning python kódtár által biztosított attribútumokkal, az Azure Machine jelöl decorates Learning munkaterületének Azonosítóját, az API-kulcs és hello adjon meg, és térjen vissza a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="220cd-166">hello method used toodeploy your python models tooAzure Machine Learning wraps hello prediction of hello model into a function and decorates it with attributes provided by hello pre-installed Azure Machine Learning python library that denote your Azure Machine Learning workspace ID, API Key, and hello input and return parameters.</span></span>  

    from azureml import services
    @services.publish(workspaceid, auth_token)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(int) #0, or 1, or 2
    def predictIris(sep_l, sep_w, pet_l, pet_w):
     inputArray = [sep_l, sep_w, pet_l, pet_w]
    return clf.predict(inputArray)

<span data-ttu-id="220cd-167">Egy ügyfél hajtsa végre hívások toohello webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="220cd-167">A client can now make calls toohello web service.</span></span> <span data-ttu-id="220cd-168">Nincsenek kényelmi burkolók, amely a REST API-kérés hello összeállításához.</span><span class="sxs-lookup"><span data-stu-id="220cd-168">There are convenience wrappers that construct hello REST API requests.</span></span> <span data-ttu-id="220cd-169">Íme egy minta kód tooconsume hello webszolgáltatás-bővítmény.</span><span class="sxs-lookup"><span data-stu-id="220cd-169">Here is a sample code tooconsume hello web service.</span></span>

    # Consume through web service URL and keys
    from azureml import services
    @services.service(url, api_key)
    @services.types(sep_l = float, sep_w = float, pet_l=float, pet_w=float)
    @services.returns(float)
    def IrisPredictor(sep_l, sep_w, pet_l, pet_w):
    pass

    IrisPredictor(3,2,3,4)


> [!NOTE]
> <span data-ttu-id="220cd-170">hello Azure Machine Learning könyvtár csak jelenleg támogatott a Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="220cd-170">hello Azure Machine Learning library is only supported on Python 2.7 currently.</span></span>   
> 
> 

### <a name="build-and-operationalize-r-models"></a><span data-ttu-id="220cd-171">Buildelés és azok R modellek</span><span class="sxs-lookup"><span data-stu-id="220cd-171">Build and Operationalize R models</span></span>
<span data-ttu-id="220cd-172">Hello adatok tudományos virtuális gép vagy máshová helyezik Azure Machine Learning oly módon, amely hasonló toohow Python elkészült a beépített R modellek is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="220cd-172">You can deploy R models built on hello Data Science Virtual Machine or elsewhere onto Azure Machine Learning in a manner that is similar toohow it is done for Python.</span></span> <span data-ttu-id="220cd-173">Saját hello lépést:</span><span class="sxs-lookup"><span data-stu-id="220cd-173">Her are hello steps:</span></span>

* <span data-ttu-id="220cd-174">Hozzon létre egy settings.json fájl tooprovide a munkaterület azonosítója és a hitelesítési token, ahogy az alábbi példakód hello.</span><span class="sxs-lookup"><span data-stu-id="220cd-174">create a settings.json file tooprovide your workspace ID and auth token as shown in hello following code sample.</span></span>
* <span data-ttu-id="220cd-175">a burkoló írni, ehhez a hello modell előrejelzése függvény.</span><span class="sxs-lookup"><span data-stu-id="220cd-175">write a wrapper for hello model's predict function.</span></span>
* <span data-ttu-id="220cd-176">hívás ```publishWebService``` a hello Azure Machine Learning könyvtár toopass a hello függvény burkoló.</span><span class="sxs-lookup"><span data-stu-id="220cd-176">call ```publishWebService``` in hello Azure Machine Learning library toopass in hello function wrapper.</span></span>  

<span data-ttu-id="220cd-177">Itt található hello eljárás és kód kódtöredékek használt tooset mentése, felépítéséhez, közzététele, és egy modellt használ az Azure Machine Learning webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="220cd-177">Here is hello procedure and code snippets that can be used tooset up, build, publish, and consume a model as a web service in Azure Machine Learning.</span></span>

#### <a name="setup"></a><span data-ttu-id="220cd-178">Beállítás</span><span class="sxs-lookup"><span data-stu-id="220cd-178">Setup</span></span>
1. <span data-ttu-id="220cd-179">Hello Machine Learning R telepítéséhez írja be a ```install.packages("AzureML")``` fordulat R vállalati 8.0 IDE vagy az R IDE.</span><span class="sxs-lookup"><span data-stu-id="220cd-179">Install hello Machine Learning R package by typing ```install.packages("AzureML")``` in Revolution R Enterprise 8.0 IDE or your R IDE.</span></span>
2. <span data-ttu-id="220cd-180">Töltse le a RTools [Itt](https://cran.r-project.org/bin/windows/Rtools/).</span><span class="sxs-lookup"><span data-stu-id="220cd-180">Download RTools from [here](https://cran.r-project.org/bin/windows/Rtools/).</span></span> <span data-ttu-id="220cd-181">Kell hello zip hello elérési útja (és a nevesített zip.exe) segédprogram toooperationalize az R csomag Machine Learning be.</span><span class="sxs-lookup"><span data-stu-id="220cd-181">You need hello zip utility in hello path (and named zip.exe) toooperationalize your R package into Machine Learning.</span></span>
3. <span data-ttu-id="220cd-182">Hozzon létre egy settings.json fájlt nevű alatt ```.azureml``` a kezdőkönyvtár alatt, és adjon meg hello paraméterek az Azure Machine Learning-munkaterület:</span><span class="sxs-lookup"><span data-stu-id="220cd-182">Create a settings.json file under a directory called ```.azureml``` under your home directory and enter hello parameters from your Azure Machine Learning workspace:</span></span>

<span data-ttu-id="220cd-183">Settings.JSON fájlstruktúra:</span><span class="sxs-lookup"><span data-stu-id="220cd-183">settings.json File structure:</span></span>

    {"workspace":{
    "id"                  : "ENTER YOUR AZUREML WORKSPACE ID",
    "authorization_token" : "ENTER YOUR AZUREML AUTH TOKEN"
    }}


#### <a name="build-a-model-in-r-and-publish-it-in-azure-machine-learning"></a><span data-ttu-id="220cd-184">Az R egy modell létrehozása, és tegye közzé az Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="220cd-184">Build a model in R and publish it in Azure Machine Learning</span></span>
    library(AzureML)
    ws <- workspace(config="~/.azureml/settings.json")

    if(!require("lme4")) install.packages("lme4")
    library(lme4)
    set.seed(1)
    train <- sleepstudy[sample(nrow(sleepstudy), 120),]
    m <- lm(Reaction ~ Days + Subject, data = train)

    # Define a prediction function toopublish based on hello model:
    sleepyPredict <- function(newdata){
          predict(m, newdata=newdata)
    }

    ep <- publishWebService(ws, fun = sleepyPredict, name="sleepy lm", inputSchema = sleepstudy, data.frame=TRUE)

#### <a name="consume-hello-model-deployed-in-azure-machine-learning"></a><span data-ttu-id="220cd-185">Az Azure Machine Learning telepített hello modell felhasználása</span><span class="sxs-lookup"><span data-stu-id="220cd-185">Consume hello model deployed in Azure Machine Learning</span></span>
<span data-ttu-id="220cd-186">hello Azure Machine Learning könyvtár toolook mentése hello használjuk tooconsume hello modell a ügyfélalkalmazás webszolgáltatás közzétevő neve hello segítségével `services` API-hívás toodetermine hello végpont.</span><span class="sxs-lookup"><span data-stu-id="220cd-186">tooconsume hello model from a client application, we use hello Azure Machine Learning library toolook up hello published web service by name using hello `services` API call toodetermine hello endpoint.</span></span> <span data-ttu-id="220cd-187">Ezt követően csak hívja hello `consume` működik, és adja át a hello adatok keret toobe előre jelezni.</span><span class="sxs-lookup"><span data-stu-id="220cd-187">Then you just call hello `consume` function and pass in hello data frame toobe predicted.</span></span>
<span data-ttu-id="220cd-188">a következő kód hello használt tooconsume hello modellt közzé az Azure Machine Learning webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="220cd-188">hello following code is used tooconsume hello model published as an Azure Machine Learning web service.</span></span>

    library(AzureML)
    library(lme4)
    ws <- workspace(config="~/.azureml/settings.json")

    s <-  services(ws, name = "sleepy lm")
    s <- tail(s, 1) # use hello last published function, in case of duplicate function names

    ep <- endpoints(ws, s)

    # OK, try this out, and compare with raw data
    ans = consume(ep, sleepstudy)$ans

<span data-ttu-id="220cd-189">További információ a hello Azure Machine Learning R könyvtárban található [Itt](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span><span class="sxs-lookup"><span data-stu-id="220cd-189">More information about hello Azure Machine Learning R library can be found [here](https://cran.r-project.org/web/packages/AzureML/AzureML.pdf).</span></span>

## <a name="4-administer-your-azure-resources-using-azure-portal-or-powershell"></a><span data-ttu-id="220cd-190">4. Felügyelheti az Azure-erőforrások Azure-portálon vagy a Powershell használatával</span><span class="sxs-lookup"><span data-stu-id="220cd-190">4. Administer your Azure resources using Azure portal or Powershell</span></span>
<span data-ttu-id="220cd-191">hello DSVM csak nem teszi lehetővé toobuild a elemzési megoldások helyben hello a virtuális gép, de emellett lehetővé teszi a Microsoft Azure felhőbe tooaccess szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="220cd-191">hello DSVM not only allows you toobuild your analytics solution locally on hello virtual machine, but also allows you tooaccess services on Microsoft's Azure cloud.</span></span> <span data-ttu-id="220cd-192">Azure több számítási, tárolási, analytics adatszolgáltatások és egyéb szolgáltatások, amelyek felügyeletéhez, és a DSVM hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="220cd-192">Azure provides several compute, storage, data analytics services and other services that you can administer and access from your DSVM.</span></span>

<span data-ttu-id="220cd-193">tooadminister az Azure előfizetés és a felhő-erőforrások használhatja a böngésző és a pont toothe [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="220cd-193">tooadminister your Azure subscription and cloud resources you can use your browser and point toothe [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="220cd-194">Használhatja az Azure Powershell tooadminister, az Azure-előfizetés és az erőforrások egy parancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="220cd-194">You can also use Azure Powershell tooadminister your Azure subscription and resources via a script.</span></span>
<span data-ttu-id="220cd-195">Azure Powershell parancsikon hello asztali környezetben futnak, vagy hello a start menü "Microsoft Azure Powershell" című témakört.</span><span class="sxs-lookup"><span data-stu-id="220cd-195">You can run Azure Powershell from a shortcut on hello desktop or from hello start menu titled "Microsoft Azure Powershell".</span></span> <span data-ttu-id="220cd-196">Tekintse meg [Microsoft Azure Powershell dokumentációs](../powershell-azure-resource-manager.md) hogyan felügyelhető az Azure-előfizetés és a Windows Powershell-parancsfájlokkal erőforrások olvashat.</span><span class="sxs-lookup"><span data-stu-id="220cd-196">Refer to [Microsoft Azure Powershell documentation](../powershell-azure-resource-manager.md) for more information on how you can administer your Azure subscription and resources using Windows Powershell scripts.</span></span>

## <a name="5-extend-your-storage-space-with-a-shared-file-system"></a><span data-ttu-id="220cd-197">5. A tárolóhely egy megosztott fájlrendszerrel kiterjesztése</span><span class="sxs-lookup"><span data-stu-id="220cd-197">5. Extend your storage space with a shared file system</span></span>
<span data-ttu-id="220cd-198">Adatszakértőkön megoszthatja a nagy adatkészleteket, kódot, valamint más hello munkacsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="220cd-198">Data scientists can share large datasets, code or other resources within hello team.</span></span> <span data-ttu-id="220cd-199">hello maga DSVM készül 70GB szabad lemezterület van.</span><span class="sxs-lookup"><span data-stu-id="220cd-199">hello DSVM itself has about 70GB of space available.</span></span> <span data-ttu-id="220cd-200">tooextend tárhelyét, használhat hello Azure szolgáltatás, és csatlakoztassa azt hello DSVM vagy-e férni a REST API-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="220cd-200">tooextend your storage, you can use hello Azure File Service and either mount it on hello DSVM or access it via a REST API.</span></span>   

> [!NOTE]
> <span data-ttu-id="220cd-201">hello maximális lemezterület hello Azure Fájlszolgáltatás megosztás 5TB, és egyes fájl maximális mérete 1TB.</span><span class="sxs-lookup"><span data-stu-id="220cd-201">hello maximum space of hello Azure File Service share is 5TB and individual file size limit is 1TB.</span></span>   
> 
> 

<span data-ttu-id="220cd-202">Használhatja az Azure Powershell toocreate egy Azure Fájlszolgáltatás megosztás.</span><span class="sxs-lookup"><span data-stu-id="220cd-202">You can use Azure Powershell toocreate an Azure File Service share.</span></span> <span data-ttu-id="220cd-203">Ez az Azure PowerShell toocreate egy Azure szolgáltatás fájlmegosztás hello parancsfájl toorun.</span><span class="sxs-lookup"><span data-stu-id="220cd-203">Here is hello script toorun under Azure PowerShell toocreate an Azure File service share.</span></span>

    # Authenticate tooAzure.
    Login-AzureRmAccount
    # Select your subscription
    Get-AzureRmSubscription –SubscriptionName "<your subscription name>" | Select-AzureRmSubscription
    # Create a new resource group.
    New-AzureRmResourceGroup -Name <dsvmdatarg>
    # Create a new storage account. You can reuse existing storage account if you wish.
    New-AzureRmStorageAccount -Name <mydatadisk> -ResourceGroupName <dsvmdatarg> -Location "<Azure Data Center Name For eg. South Central US>" -Type "Standard_LRS"
    # Set your current working storage account
    Set-AzureRmCurrentStorageAccount –ResourceGroupName "<dsvmdatarg>" –StorageAccountName <mydatadisk>

    # Create a Azure File Service Share
    $s = New-AzureStorageShare <<teamsharename>>
    # Create a directory under hello FIle share. You can give it any name
    New-AzureStorageDirectory -Share $s -Path <directory name>
    # List hello share tooconfirm that everything worked
    Get-AzureStorageFile -Share $s


<span data-ttu-id="220cd-204">Most, hogy létrehozta az Azure fájlmegosztások, a virtuális gép az Azure-ban csatlakoztathatja azt.</span><span class="sxs-lookup"><span data-stu-id="220cd-204">Now that you have created an Azure file share, you can mount it in any virtual machine in Azure.</span></span> <span data-ttu-id="220cd-205">Lehetőleg ugyanazon Azure adatközpontba, ahol hello tárolási fiók tooavoid késést és az adatok adatátviteli díjakkal adott hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="220cd-205">It is highly recommended that hello VM is in same Azure data center as hello storage account tooavoid latency and data transfer charges.</span></span> <span data-ttu-id="220cd-206">Az Azure Powershell futtatható DSVM hello az alábbiakban hello parancsok toomount hello meghajtó.</span><span class="sxs-lookup"><span data-stu-id="220cd-206">Here are hello commands toomount hello drive on hello DSVM that you can run on Azure Powershell.</span></span>

    # Get storage key of hello storage account that has hello Azure file share from Azure portal. Store it securely on hello VM tooavoid prompted in next command.
    cmdkey /add:<<mydatadisk>>.file.core.windows.net /user:<<mydatadisk>> /pass:<storage key>

    # Mount hello Azure file share as Z: drive on hello VM. You can chose another drive letter if you wish
    net use z:  \\<mydatadisk>.file.core.windows.net\<<teamsharename>>


<span data-ttu-id="220cd-207">Most már hozzáférhet a meghajtó mint bármely virtuális gép hello normál meghajtóján.</span><span class="sxs-lookup"><span data-stu-id="220cd-207">Now you can access this drive as you would any normal drive on hello VM.</span></span>

## <a name="6-share-code-with-your-team-using-github"></a><span data-ttu-id="220cd-208">6. Kód megosztása a csapat GitHub használatával</span><span class="sxs-lookup"><span data-stu-id="220cd-208">6. Share code with your team using GitHub</span></span>
<span data-ttu-id="220cd-209">GitHub tárháza kód megkeresésének mintakód és a források számos különböző eszközök különböző technológiákkal hello fejlesztői Közösség által megosztott használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-209">GitHub is a code repository where you can find a lot of sample code and sources for different tools using various technologies shared by hello developer community.</span></span> <span data-ttu-id="220cd-210">Hello technológia tootrack és a tároló verziója hello kódfájlok Git azt használja.</span><span class="sxs-lookup"><span data-stu-id="220cd-210">It uses Git as hello technology tootrack and store versions of hello code files.</span></span> <span data-ttu-id="220cd-211">GitHub egyben a platform ahol létrehozhat saját tárház toostore a csoport megosztott kód és dokumentáció, verziókezelést és megvalósításához is vezérlő akik hozzáférés tooview és kód közre.</span><span class="sxs-lookup"><span data-stu-id="220cd-211">GitHub is also a platform where you can create your own repository toostore your team's shared code and documentation, implement version control and also control who have access tooview and contribute code.</span></span> <span data-ttu-id="220cd-212">Látogasson el a hello [GitHub súgóoldalak](https://help.github.com/) további információt a Git használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-212">Please visit hello [GitHub help pages](https://help.github.com/) for more information on using Git.</span></span> <span data-ttu-id="220cd-213">Használhatja a GitHub hello módon toocollaborate rendelkezésre álló munkatársaival, hello közösségi által fejlesztett kód használata és kód hátsó toohello közösségi közreműködés.</span><span class="sxs-lookup"><span data-stu-id="220cd-213">You can use GitHub as one of hello ways toocollaborate with your team, use code developed by hello community and contribute code back toohello community.</span></span>

<span data-ttu-id="220cd-214">hello DSVM már előre betöltött az ügyféleszközök elől mindkét parancssori és grafikus felhasználói Felülettel tooaccess GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="220cd-214">hello DSVM already comes loaded with client tools on both command-line as well GUI tooaccess GitHub repository.</span></span> <span data-ttu-id="220cd-215">a Git és GitHub hello parancssori eszköz toowork nevezik Git bash eszközt.</span><span class="sxs-lookup"><span data-stu-id="220cd-215">hello command-line tool toowork with Git and GitHub is called Git Bash.</span></span> <span data-ttu-id="220cd-216">Visual Studio hello DSVM telepítve van a hello Git extensions.</span><span class="sxs-lookup"><span data-stu-id="220cd-216">Visual Studio installed on hello DSVM has hello Git extensions.</span></span> <span data-ttu-id="220cd-217">Ezek az eszközök hello start menüben és hello asztali kezdeti ikonok találhat meg.</span><span class="sxs-lookup"><span data-stu-id="220cd-217">You can find start-up icons for these tools on hello start menu and hello desktop.</span></span>

<span data-ttu-id="220cd-218">toodownload kódját a GitHub-tárházban hello használata ```git clone``` parancsot.</span><span class="sxs-lookup"><span data-stu-id="220cd-218">toodownload code from a GitHub repository you use hello ```git clone``` command.</span></span> <span data-ttu-id="220cd-219">Például toodownload tudományos adattárház hello aktuális könyvtárba Microsoft által kiadott futtathatja a következő parancsot, Miután belépett hello ```git-bash```.</span><span class="sxs-lookup"><span data-stu-id="220cd-219">For example toodownload data science repository published by Microsoft into hello current directory you can run hello following command once you are in ```git-bash```.</span></span>

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

<span data-ttu-id="220cd-220">A Visual Studio, megteheti ugyanazon Klónozás hello.</span><span class="sxs-lookup"><span data-stu-id="220cd-220">In Visual Studio, you can do hello same clone operation.</span></span> <span data-ttu-id="220cd-221">a következő képernyőfelvétel hello bemutatja, hogyan tooaccess a Git szoftver, a GitHub a Visual Studio eszközök.</span><span class="sxs-lookup"><span data-stu-id="220cd-221">hello  following screen-shot shows how tooaccess Git and GitHub tools in Visual Studio.</span></span>

![A Visual Studio Git](./media/machine-learning-data-science-vm-do-ten-things/VSGit.PNG)

<span data-ttu-id="220cd-223">További tájékoztatást Git toowork a github.com több erőforrást a GitHub-tárházban található. Hello [adatlap](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) hasznos hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="220cd-223">You can find more information on using Git toowork with your GitHub repository from several resources available on github.com. hello [cheat sheet](https://training.github.com/kit/downloads/github-git-cheat-sheet.pdf) is a useful reference.</span></span>

## <a name="7-access-various-azure-data-and-analytics-services"></a><span data-ttu-id="220cd-224">7. Hozzáférjen a különböző Azure adatok és analitikák szolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="220cd-224">7. Access various Azure data and analytics services</span></span>
### <a name="azure-blob"></a><span data-ttu-id="220cd-225">Azure-blob</span><span class="sxs-lookup"><span data-stu-id="220cd-225">Azure Blob</span></span>
<span data-ttu-id="220cd-226">Az Azure blob a kis és nagy adatok megbízható, gazdaságos felhőtárhelyére.</span><span class="sxs-lookup"><span data-stu-id="220cd-226">Azure blob is a reliable, economical cloud storage for data big and small.</span></span> <span data-ttu-id="220cd-227">Ossza meg velünk tekintse meg hogyan adatok tooAzure Blob és a hozzáférési adatok tárolódnak az Azure Blob helyezheti.</span><span class="sxs-lookup"><span data-stu-id="220cd-227">Let us look at how you can move data tooAzure Blob and access data stored in an Azure Blob.</span></span>

<span data-ttu-id="220cd-228">**Előfeltétel**</span><span class="sxs-lookup"><span data-stu-id="220cd-228">**Prerequisite**</span></span>

* <span data-ttu-id="220cd-229">**Az az Azure Blob storage-fiók létrehozása [Azure-portálon](https://portal.azure.com).**</span><span class="sxs-lookup"><span data-stu-id="220cd-229">**Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).**</span></span>

![Create_Azure_Blob](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="220cd-231">Győződjön meg arról, hogy hello előre telepített parancssori AzCopy eszköz a következő címen található ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span><span class="sxs-lookup"><span data-stu-id="220cd-231">Confirm that hello pre-installed command-line AzCopy tool is found at ```C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy.exe```.</span></span> <span data-ttu-id="220cd-232">Hello tartalmazó hello azcopy.exe tooyour elérési út környezeti változó tooavoid gépelési hello teljes parancs könyvtárútvonal adhat hozzá, az eszköz futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="220cd-232">You can add hello directory containing hello azcopy.exe tooyour PATH environment variable tooavoid typing hello full command path when running this tool.</span></span> <span data-ttu-id="220cd-233">AzCopy eszközről további információért tekintse meg túl[AzCopy dokumentációját](../storage/common/storage-use-azcopy.md)</span><span class="sxs-lookup"><span data-stu-id="220cd-233">For more info on AzCopy tool please refer too[AzCopy documentation](../storage/common/storage-use-azcopy.md)</span></span>
* <span data-ttu-id="220cd-234">Indítsa el a hello Azure Tártallózó eszközt.</span><span class="sxs-lookup"><span data-stu-id="220cd-234">Start hello Azure Storage Explorer tool.</span></span> <span data-ttu-id="220cd-235">Letölthető a [Microsoft Azure Tártallózó](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="220cd-235">It can be downloaded from [Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span> 

![AzureStorageExplorer_v4](./media/machine-learning-data-science-vm-do-ten-things/AzureStorageExplorer_v4.png)

<span data-ttu-id="220cd-237">**Adatok áthelyezése VM tooAzure Blob: AzCopy**</span><span class="sxs-lookup"><span data-stu-id="220cd-237">**Move data from VM tooAzure Blob: AzCopy**</span></span>

<span data-ttu-id="220cd-238">toomove adatok között a helyi fájlokat és a blob storage segítségével is AzCopy parancssori vagy a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="220cd-238">toomove data between your local files and blob storage, you can use AzCopy in command-line or PowerShell:</span></span>

    AzCopy /Source:C:\myfolder /Dest:https://<mystorageaccount>.blob.core.windows.net/<mycontainer> /DestKey:<storage account key> /Pattern:abc.txt

<span data-ttu-id="220cd-239">Cserélje le **C:\myfolder** toohello elérési útra, ahol a fájl található, **mystorageaccount** tooyour blob storage-fiók neve, **mycontainer** toohello Tárolónév esetén **tárfiók kulcsa** tooyour blob tárelérési kulcs.</span><span class="sxs-lookup"><span data-stu-id="220cd-239">Replace **C:\myfolder** toohello path where your file is stored, **mystorageaccount** tooyour blob storage account name, **mycontainer** toohello container name, **storage account key** tooyour blob storage access key.</span></span> <span data-ttu-id="220cd-240">A tárfiók hitelesítő adatainak a található [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="220cd-240">You can find your storage account credentials in [Azure portal](https://portal.azure.com).</span></span>

![StorageAccountCredential_v2](./media/machine-learning-data-science-vm-do-ten-things/StorageAccountCredential_v2.png)

<span data-ttu-id="220cd-242">AzCopy parancs futtatása, a PowerShell vagy a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="220cd-242">Run AzCopy command in PowerShell or from a command prompt.</span></span> <span data-ttu-id="220cd-243">Íme néhány példa használati AzCopy parancs:</span><span class="sxs-lookup"><span data-stu-id="220cd-243">Here is some example usage of AzCopy command:</span></span>

    # Copy *.sql from local machine tooa Azure Blob
    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:"c:\Aaqs\Data Science Scripts" /Dest:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /DestKey:[ENTER STORAGE KEY] /S /Pattern:*.sql

    # Copy back all files from Azure Blob container tooLocal machine

    "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Dest:"c:\Aaqs\Data Science Scripts\temp" /Source:https://[ENTER STORAGE ACCOUNT].blob.core.windows.net/[ENTER CONTAINER] /SourceKey:[ENTER STORAGE KEY] /S



<span data-ttu-id="220cd-244">Az AzCopy parancs toocopy tooan látja, hogy a fájl megjelennek Azure Tártallózó hamarosan Azure blob futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="220cd-244">Once you run your AzCopy command toocopy tooan Azure blob you see your file shows up in Azure Storage Explorer shortly.</span></span>

![AzCopy_run_finshed_Storage_Explorer_v3](./media/machine-learning-data-science-vm-do-ten-things/AzCopy_run_finshed_Storage_Explorer_v3.png)

<span data-ttu-id="220cd-246">**Adatok áthelyezése VM tooAzure Blob: Azure Tártallózó**</span><span class="sxs-lookup"><span data-stu-id="220cd-246">**Move data from VM tooAzure Blob: Azure Storage Explorer**</span></span>

<span data-ttu-id="220cd-247">A virtuális gép Azure Tártallózó is feltöltheti hello helyi fájl adatait:</span><span class="sxs-lookup"><span data-stu-id="220cd-247">You can also upload data from hello local file in your VM using Azure Storage Explorer:</span></span>

* <span data-ttu-id="220cd-248">tooupload adatok tooa tároló, jelölje be hello cél konténert, és kattintson hello **feltöltése** gomb.![ A Tártallózó feltöltése](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span><span class="sxs-lookup"><span data-stu-id="220cd-248">tooupload data tooa container, select hello target container and click hello **Upload** button.![Upload in Storage Explorer](./media/machine-learning-data-science-vm-do-ten-things/storage-accounts.png)</span></span>
* <span data-ttu-id="220cd-249">Kattintson a hello **...**  sarkában hello toohello **fájlok** mezőben, jelöljön ki egy vagy több fájlok tooupload hello fájlrendszer majd **feltöltése** hello fájlok feltöltése toobegin.![ Fájlok tooblob feltöltése](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span><span class="sxs-lookup"><span data-stu-id="220cd-249">Click on hello **...** toohello right of hello **Files** box, select one or multiple files tooupload from hello file system and click **Upload** toobegin uploading hello files.![Upload files tooblob](./media/machine-learning-data-science-vm-do-ten-things/upload-files-to-blob.png)</span></span>

<span data-ttu-id="220cd-250">**Adatokat olvasni az Azure Blob: gépi tanulás olvasó modul**</span><span class="sxs-lookup"><span data-stu-id="220cd-250">**Read data from Azure Blob: Machine Learning reader module**</span></span>

<span data-ttu-id="220cd-251">Az Azure Machine Learning Studióban használhatja egy **és adatokat importálhat modul** tooread adatokat a blobból.</span><span class="sxs-lookup"><span data-stu-id="220cd-251">In Azure Machine Learning Studio you can use an **Import Data module** tooread data from your blob.</span></span>

![AML_ReaderBlob_Module_v3](./media/machine-learning-data-science-vm-do-ten-things/AML_ReaderBlob_Module_v3.png)

<span data-ttu-id="220cd-253">**Adatokat olvasni az Azure Blob: Python ODBC**</span><span class="sxs-lookup"><span data-stu-id="220cd-253">**Read data from Azure Blob: Python ODBC**</span></span>

<span data-ttu-id="220cd-254">Használhat **BlobService** könyvtár tooread adatok közvetlenül a Jupyter Notebook vagy Python programban blob.</span><span class="sxs-lookup"><span data-stu-id="220cd-254">You can use **BlobService** library tooread data directly from blob in a Jupyter Notebook or Python program.</span></span>

<span data-ttu-id="220cd-255">Először importálja a szükséges csomagokat:</span><span class="sxs-lookup"><span data-stu-id="220cd-255">First, import required packages:</span></span>

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random

<span data-ttu-id="220cd-256">Majd a beépülő modul az Azure Blob-fiók hitelesítő adatait, és adatokat olvasni Blob:</span><span class="sxs-lookup"><span data-stu-id="220cd-256">Then plug in your Azure Blob account credentials and read data from Blob:</span></span>

    CONTAINERNAME = 'xxx'
    STORAGEACCOUNTNAME = 'xxxx'
    STORAGEACCOUNTKEY = 'xxxxxxxxxxxxxxxx'
    BLOBNAME = 'nyctaxidataset/nyctaxitrip/trip_data_1.csv'
    localfilename = 'trip_data_1.csv'
    LOCALDIRECTORY = os.getcwd()
    LOCALFILE =  os.path.join(LOCALDIRECTORY, localfilename)

    #download from blob
    t1 = time.time()
    blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILE)
    t2 = time.time()
    print(("It takes %s seconds toodownload "+BLOBNAME) % (t2 - t1))

    #unzipping downloaded files if needed
    #with zipfile.ZipFile(ZIPPEDLOCALFILE, "r") as z:
    #    z.extractall(LOCALDIRECTORY)

    df1 = pd.read_csv(LOCALFILE, header=0)
    df1.columns = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime','passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude']
    print 'hello size of hello data is: %d rows and  %d columns' % df1.shape

<span data-ttu-id="220cd-257">hello adatolvasás az adatok keretként:</span><span class="sxs-lookup"><span data-stu-id="220cd-257">hello data is read in as a data frame:</span></span>

![IPNB_data_readin](./media/machine-learning-data-science-vm-do-ten-things/IPNB_data_readin.PNG)

### <a name="azure-data-lake"></a><span data-ttu-id="220cd-259">Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="220cd-259">Azure Data Lake</span></span>
<span data-ttu-id="220cd-260">Az Azure Data Lake Storage egy kapacitású adattár a big data elemzés számítási feladatokat és a Hadoop elosztott fájlrendszerrel (HDFS) kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="220cd-260">Azure Data Lake Storage is a hyper-scale repository for big data analytics workloads and compatible with Hadoop Distributed File System (HDFS).</span></span> <span data-ttu-id="220cd-261">Hello Hadoop ökoszisztémájának és hello Azure Data Lake Analytics is működik.</span><span class="sxs-lookup"><span data-stu-id="220cd-261">It works with both hello Hadoop ecosystem and hello Azure Data Lake Analytics.</span></span> <span data-ttu-id="220cd-262">Megmutatjuk, hogyan adatok áthelyezése az Azure Data Lake Store hello rendszerbe, és futtassa a analytics az Azure Data Lake Analytics használatának.</span><span class="sxs-lookup"><span data-stu-id="220cd-262">We show how you can move data into hello Azure Data Lake Store and run analytics using Azure Data Lake Analytics.</span></span>

<span data-ttu-id="220cd-263">**Előfeltétel**</span><span class="sxs-lookup"><span data-stu-id="220cd-263">**Prerequisite**</span></span>

* <span data-ttu-id="220cd-264">Hozzon létre az Azure Data Lake Analytics a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="220cd-264">Create your Azure Data Lake Analytics in [Azure portal](https://portal.azure.com).</span></span>

![Azure_Data_Lake_Create_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_Create_v2.png)

* <span data-ttu-id="220cd-266">Hello **Azure Data Lake Tools** a **Visual Studio** ezzel található [hivatkozás](https://www.microsoft.com/download/details.aspx?id=49504) hello amelyek hello virtuális gép a Visual Studio Community Edition már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="220cd-266">hello  **Azure Data Lake Tools** in **Visual Studio** found at this  [link](https://www.microsoft.com/download/details.aspx?id=49504) is already installed on hello Visual Studio Community Edition which is on hello virtual machine.</span></span> <span data-ttu-id="220cd-267">Visual Studio kezdő, és az Azure-előfizetése van bejelentkezve, megtekintheti az Azure Data Analytics-fiók és a tárolási hello bal oldali panel a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="220cd-267">After starting Visual Studio and logging in your Azure subscription, you should see your Azure Data Analytics account and storage in hello left panel of Visual Studio.</span></span>

![Azure_Data_Lake_PlugIn_v2](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_PlugIn_v2.PNG)

<span data-ttu-id="220cd-269">**Adatok áthelyezése VM tooData Lake: Azure Data Lake-kezelővel**</span><span class="sxs-lookup"><span data-stu-id="220cd-269">**Move data from VM tooData Lake: Azure Data Lake Explorer**</span></span>

<span data-ttu-id="220cd-270">Használhat **Azure Data Lake Explorer** tooupload adatokat a virtuális gép tooData Lake tárolóban hello helyi fájlokból.</span><span class="sxs-lookup"><span data-stu-id="220cd-270">You can use **Azure Data Lake Explorer** tooupload data from hello local files in your Virtual Machine tooData Lake storage.</span></span>

![Azure_Data_Lake_UploadData](./media/machine-learning-data-science-vm-do-ten-things/Azure_Data_Lake_UploadData.PNG)

<span data-ttu-id="220cd-272">Is létrehozható egy adatok adatcsatorna tooproductionize az adatok mozgása tooor az Azure Data Lake hello segítségével [Azure adatok Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="220cd-272">You can also build a data pipeline tooproductionize your data movement tooor from Azure Data Lake using hello [Azure Data Factory(ADF)](https://azure.microsoft.com/services/data-factory/).</span></span> <span data-ttu-id="220cd-273">Azt tekintse meg, hogy toothis [cikk](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide hello lépéseket toobuild hello adatokat nyújt a folyamatok.</span><span class="sxs-lookup"><span data-stu-id="220cd-273">We refer you toothis [article](https://azure.microsoft.com/blog/creating-big-data-pipelines-using-azure-data-lake-and-azure-data-factory/) tooguide you through hello steps toobuild hello data pipelines.</span></span>

<span data-ttu-id="220cd-274">**Adatokat olvasni az Azure Blob tooData Lake: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="220cd-274">**Read data from Azure Blob tooData Lake: U-SQL**</span></span>

<span data-ttu-id="220cd-275">Ha az adatok Azure Blob Storage tárolóban található, közvetlenül olvashatja adatokat az Azure storage-blobból az U-SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="220cd-275">If your data resides in Azure Blob storage, you can directly read data from Azure storage blob in U-SQL query.</span></span> <span data-ttu-id="220cd-276">A U-SQL-lekérdezés létrehozása, előtt győződjön meg arról a blob storage-fiók csatolt tooyour Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="220cd-276">Before composing your U-SQL query, make sure your blob storage account is linked tooyour Azure Data Lake.</span></span> <span data-ttu-id="220cd-277">Nyissa meg túl**Azure-portálon**, az Azure Data Lake Analytics irányítópulton található, majd **adatforrás hozzáadása**, tárolási típusa túl**Azure Storage** , és csatlakoztassa az Azure Storage Fiók neve és kulcsa.</span><span class="sxs-lookup"><span data-stu-id="220cd-277">Go too**Azure portal**, find your Azure Data Lake Analytics dashboard, click **Add Data Source**, select storage type too**Azure Storage** and plug in your Azure Storage Account Name and Key.</span></span> <span data-ttu-id="220cd-278">Esetén a rendszer képes tooreference hello adatok hello tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="220cd-278">Then you are able tooreference hello data stored in hello storage account.</span></span>

![Adja meg a tárfiók és a kulcs](./media/machine-learning-data-science-vm-do-ten-things/Link_Blob_to_ADLA_v2.PNG)

<span data-ttu-id="220cd-280">A Visual Studio-adatok olvasása a blob-tároló, hajtson végre néhány adatkezelési, a szolgáltatás mérnöki csapathoz, és kimeneti hello eredményül kapott adatok tooeither Azure Data Lake vagy Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="220cd-280">In Visual Studio, you can read data from blob storage, do some data manipulation, feature engineering, and output hello resulting data tooeither Azure Data Lake or Azure Blob Storage.</span></span> <span data-ttu-id="220cd-281">Ha hello adatokat a blob Storage tárolóban hivatkozik, **wasb: / /**; Ha hello adatok Azure Data Lake, használjon hivatkozik **swbhdfs: / /**</span><span class="sxs-lookup"><span data-stu-id="220cd-281">When you reference hello data in blob storage, use **wasb://**; when you reference hello data in Azure Data Lake, use **swbhdfs://**</span></span>

![Adatok keret](./media/machine-learning-data-science-vm-do-ten-things/USQL_Read_Blob_v2.PNG)

<span data-ttu-id="220cd-283">A következő U-SQL-lekérdezések Visual Studio hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="220cd-283">You may use hello following U-SQL queries in Visual Studio:</span></span>

    @a =
        EXTRACT medallion string,
                hack_license string,
                vendor_id string,
                rate_code string,
                store_and_fwd_flag string,
                pickup_datetime string,
                dropoff_datetime string,
                passenger_count int,
                trip_time_in_secs double,
                trip_distance double,
                pickup_longitude string,
                pickup_latitude string,
                dropoff_longitude string,
                dropoff_latitude string

        FROM "wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Input Data File Name>"
        USING Extractors.Csv();

    @b =
        SELECT vendor_id,
        COUNT(medallion) AS cnt_medallion,
        SUM(passenger_count) AS cnt_passenger,
        AVG(trip_distance) AS avg_trip_dist,
        MIN(trip_distance) AS min_trip_dist,
        MAX(trip_distance) AS max_trip_dist,
        AVG(trip_time_in_secs) AS avg_trip_time
        FROM @a
        GROUP BY vendor_id;

    OUTPUT @b   
    too"swebhdfs://<Azure Data Lake Storage Account Name>.azuredatalakestore.net/<Folder Name>/<Output Data File Name>"
    USING Outputters.Csv();

    OUTPUT @b   
    too"wasb://<Container name>@<Azure Blob Storage Account Name>.blob.core.windows.net/<Output Data File Name>"
    USING Outputters.Csv();



<span data-ttu-id="220cd-284">Miután a lekérdezés elküldött toohello kiszolgáló, ábra: a feladat állapotának hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="220cd-284">After your query is submitted toohello server, a diagram showing hello status of your job is displayed.</span></span>

![Feladat állapota diagramja](./media/machine-learning-data-science-vm-do-ten-things/USQL_Job_Status.PNG)

<span data-ttu-id="220cd-286">**A Data Lake adatait: U-SQL**</span><span class="sxs-lookup"><span data-stu-id="220cd-286">**Query data in Data Lake: U-SQL**</span></span>

<span data-ttu-id="220cd-287">Miután az Azure Data Lake hello dataset van okozhatnak, [U-SQL nyelv](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery és hello adatokba.</span><span class="sxs-lookup"><span data-stu-id="220cd-287">After hello dataset is ingested into Azure Data Lake, you can use [U-SQL language](../data-lake-analytics/data-lake-analytics-u-sql-get-started.md) tooquery and explore hello data.</span></span> <span data-ttu-id="220cd-288">U-SQL nyelv hasonló eszköz SQL, de egyesíti az egyes szolgáltatások, a C#, hogy a felhasználók írhat, egyéni modulokat, felhasználó által definiált függvények és stb. Az előző lépésben hello hello parancsfájlokat használhat.</span><span class="sxs-lookup"><span data-stu-id="220cd-288">U-SQL language is similar tooT-SQL, but combines some features from C# so that users can write customized modules, user-defined functions, and etc. You can use hello scripts in hello previous step.</span></span>

<span data-ttu-id="220cd-289">Miután hello lekérdezés elküldött tooserver, tripdata_summary. Fürt megosztott kötetei szolgáltatás található hamarosan **Azure Data Lake Explorer**, előfordulhat, hogy előzetes megtekintéséhez kattintson a jobb gombbal hello fájl hello adatait.</span><span class="sxs-lookup"><span data-stu-id="220cd-289">After hello query is submitted tooserver, tripdata_summary.CSV can be found shortly in **Azure Data Lake Explorer**, you may preview hello data by right-click hello file.</span></span>

![Az Azure Data Lake Explorer fájl](./media/machine-learning-data-science-vm-do-ten-things/USQL_create_summary.png)

<span data-ttu-id="220cd-291">toosee hello fájlinformációt:</span><span class="sxs-lookup"><span data-stu-id="220cd-291">toosee hello file information:</span></span>

![Fájl összegzése](./media/machine-learning-data-science-vm-do-ten-things/USQL_tripdata_summary.png)

### <a name="hdinsight-hadoop-clusters"></a><span data-ttu-id="220cd-293">HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="220cd-293">HDInsight Hadoop Clusters</span></span>
<span data-ttu-id="220cd-294">Az Azure HDInsight egy olyan felügyelt Apache Hadoop, a Spark, a HBase és a Storm szolgáltatás hello felhő.</span><span class="sxs-lookup"><span data-stu-id="220cd-294">Azure HDInsight is a managed Apache Hadoop, Spark, HBase, and Storm service on hello cloud.</span></span> <span data-ttu-id="220cd-295">Azure HDInsight-fürtök hello adatok tudományos virtuális gépről könnyen dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="220cd-295">You can work easily with Azure HDInsight clusters from hello data science virtual machine.</span></span>

<span data-ttu-id="220cd-296">**Előfeltétel**</span><span class="sxs-lookup"><span data-stu-id="220cd-296">**Prerequisite**</span></span>

* <span data-ttu-id="220cd-297">Az az Azure Blob storage-fiók létrehozása [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="220cd-297">Create your Azure Blob storage account from [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="220cd-298">Ez a tárfiók nem HDInsight-fürtök használt toostore adatok.</span><span class="sxs-lookup"><span data-stu-id="220cd-298">This storage account is used toostore data for HDInsight clusters.</span></span>

![Azure Blob storage-fiók létrehozása](./media/machine-learning-data-science-vm-do-ten-things/Create_Azure_Blob.PNG)

* <span data-ttu-id="220cd-300">Testre szabhatja az Azure HDInsight Hadoop-fürtök a [Azure-portálon](machine-learning-data-science-customize-hadoop-cluster.md)</span><span class="sxs-lookup"><span data-stu-id="220cd-300">Customize Azure HDInsight Hadoop Clusters from [Azure portal](machine-learning-data-science-customize-hadoop-cluster.md)</span></span>
  
  * <span data-ttu-id="220cd-301">Hello storage-fiók létrehozásakor a HDInsight-fürthöz létrehozott kell kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="220cd-301">You must link hello storage account created with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="220cd-302">Ez a tárfiók hello fürtön belül feldolgozható adatainak eléréséhez használatos.</span><span class="sxs-lookup"><span data-stu-id="220cd-302">This storage account is used for accessing data that can be processed within hello cluster.</span></span>

![HDInsight-fürthöz létrehozott hivatkozás toostorage fiók](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_v4.PNG)

* <span data-ttu-id="220cd-304">Engedélyeznie kell az **távelérési** toohello átjárócsomópont hello fürt létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="220cd-304">You must enable **Remote Access** toohello head node of hello cluster after it is created.</span></span> <span data-ttu-id="220cd-305">Az itt megadott (eltérnek a saját létrehozásakor hello fürt számára megadott) hello távoli hozzáférési hitelesítő adatok megjegyzése: hello későbbi eljárásban kell.</span><span class="sxs-lookup"><span data-stu-id="220cd-305">Remember hello remote access credentials you specify here (different from those specified for hello cluster at its creation): you need them in hello subsequent procedure.</span></span>

![Távelérés engedélyezése](./media/machine-learning-data-science-vm-do-ten-things/Create_HDI_dashboard_v3.PNG)

* <span data-ttu-id="220cd-307">Hozzon létre egy Azure Machine Learning munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="220cd-307">Create an Azure Machine Learning workspace.</span></span> <span data-ttu-id="220cd-308">A gépi tanulási kísérletekhez a Machine Learning-munkaterület vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="220cd-308">Your Machine Learning Experiments are stored in this Machine Learning workspace.</span></span> <span data-ttu-id="220cd-309">A kijelölt hello-beállítások kiválasztása a portálon, ahogy az alábbi képernyőfelvétel a hello:</span><span class="sxs-lookup"><span data-stu-id="220cd-309">Select hello highlighted options in Portal as shown in hello following screenshot:</span></span>

![Azure Machine Learning-munkaterület létrehozása](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space.PNG)

* <span data-ttu-id="220cd-311">Ezután írja be a hello paraméterek a munkaterület</span><span class="sxs-lookup"><span data-stu-id="220cd-311">Then enter hello parameters for your workspace</span></span>

![Adja meg a Machine Learning-munkaterület paraméterek](./media/machine-learning-data-science-vm-do-ten-things/Create_ML_Space_step2_v2.PNG)

* <span data-ttu-id="220cd-313">Töltse fel az adatok IPython Notebook használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-313">Upload data using IPython Notebook.</span></span> <span data-ttu-id="220cd-314">Először importálja a szükséges csomagokat, a beépülő modul hitelesítő adatokat, egy adatbázis létrehozása a tárfiókban lévő, majd adatok tooHDI fürtök betölteni.</span><span class="sxs-lookup"><span data-stu-id="220cd-314">First import required packages, plug in credentials, create a db in your storage account, then load data tooHDI clusters.</span></span>

        #Import required Packages
        import pyodbc
        import time as time
        import json
        import os
        import urllib
        import urllib2
        import warnings
        import re
        import pandas as pd
        import matplotlib.pyplot as plt
        from azure.storage.blob import BlobService
        warnings.filterwarnings("ignore", category=UserWarning, module='urllib2')


        #Create hello connection tooHive using ODBC
        SERVER_NAME='xxx.azurehdinsight.net'
        DATABASE_NAME='nyctaxidb'
        USERID='xxx'
        PASSWORD='xxxx'
        DB_DRIVER='Microsoft Hive ODBC Driver'
        driver = 'DRIVER={' + DB_DRIVER + '}'
        server = 'Host=' + SERVER_NAME + ';Port=443'
        database = 'Schema=' + DATABASE_NAME
        hiveserv = 'HiveServerType=2'
        auth = 'AuthMech=6'
        uid = 'UID=' + USERID
        pwd = 'PWD=' + PASSWORD
        CONNECTION_STRING = ';'.join([driver,server,database,hiveserv,auth,uid,pwd])
        connection = pyodbc.connect(CONNECTION_STRING, autocommit=True)
        cursor=connection.cursor()


        #Create Hive database and tables
        queryString = "create database if not exists nyctaxidb;"
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.trip
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            rate_code string,
                            store_and_fwd_flag string,
                            pickup_datetime string,
                            dropoff_datetime string,
                            passenger_count int,
                            trip_time_in_secs double,
                            trip_distance double,
                            pickup_longitude double,
                            pickup_latitude double,
                            dropoff_longitude double,
                            dropoff_latitude double)  
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)

        queryString = """
                        create external table if not exists nyctaxidb.fare
                        (
                            medallion string,
                            hack_license string,
                            vendor_id string,
                            pickup_datetime string,
                            payment_type string,
                            fare_amount double,
                            surcharge double,
                            mta_tax double,
                            tip_amount double,
                            tolls_amount double,
                            total_amount double)
                        PARTITIONED BY (month int)
                        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\\n'
                        STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');
                    """
        cursor.execute(queryString)


        #Upload data from blob storage tooHDI cluster
        for i in range(1,13):
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxitripraw2/trip_data_%d.csv' INTO TABLE nyctaxidb2.trip PARTITION (month=%d);"%(i,i)
            cursor.execute(queryString)
            queryString = "LOAD DATA INPATH 'wasb:///nyctaxifareraw2/trip_fare_%d.csv' INTO TABLE nyctaxidb2.fare PARTITION (month=%d);"%(i,i)  
            cursor.execute(queryString)


* <span data-ttu-id="220cd-315">Ehelyett kövesse a [forgatókönyv](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi adatok tooHDI fürt.</span><span class="sxs-lookup"><span data-stu-id="220cd-315">Alternately,  you can follow this [walkthrough](machine-learning-data-science-process-hive-walkthrough.md) tooupload NYC Taxi data tooHDI cluster.</span></span> <span data-ttu-id="220cd-316">Fő lépések az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="220cd-316">Major steps include:</span></span>
  
  * <span data-ttu-id="220cd-317">AzCopy: Töltse le a tömörített CSV nyilvános blob tooyour helyi mappából</span><span class="sxs-lookup"><span data-stu-id="220cd-317">AzCopy: download zipped CSV's from public blob tooyour local folder</span></span>
  * <span data-ttu-id="220cd-318">AzCopy: töltse fel a tömörítetlen fürt megosztott kötetei szolgáltatás a helyi mappa tooHDI fürtből</span><span class="sxs-lookup"><span data-stu-id="220cd-318">AzCopy: upload unzipped CSV's from local folder tooHDI cluster</span></span>
  * <span data-ttu-id="220cd-319">Hadoop-fürt átjárócsomópontjához hello bejelentkezni, és készítse elő a felderítő adatelemzés</span><span class="sxs-lookup"><span data-stu-id="220cd-319">Log into hello head node of Hadoop cluster and prepare for exploratory data analysis</span></span>

<span data-ttu-id="220cd-320">Miután hello adatok betöltött tooHDI fürthöz, ellenőrizheti a Azure Tártallózó adatai.</span><span class="sxs-lookup"><span data-stu-id="220cd-320">After hello data is loaded tooHDI cluster, you can check your data in Azure Storage Explorer.</span></span> <span data-ttu-id="220cd-321">És egy adatbázis nyctaxidb HDI-fürtöt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="220cd-321">And you have a database nyctaxidb created in HDI cluster.</span></span>

<span data-ttu-id="220cd-322">**Az adatok feltárása: a Python Hive-lekérdezések**</span><span class="sxs-lookup"><span data-stu-id="220cd-322">**Data exploration: Hive Queries in Python**</span></span>

<span data-ttu-id="220cd-323">Mivel hello adatokat a Hadoop-fürttel, használhatja a hello pyodbc csomag tooconnect tooHadoop fürtök és az adatbázis lekérdezése Hive toodo feltárására és a szolgáltatás műszaki osztály használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-323">Since hello data is in Hadoop cluster, you can use hello pyodbc package tooconnect tooHadoop Clusters and query database using Hive toodo exploration and feature engineering.</span></span> <span data-ttu-id="220cd-324">Hello hello előfeltétel lépésben létrehozott meglévő táblák tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="220cd-324">You can view hello existing tables we created in hello prerequisite step.</span></span>

    queryString = """
        show tables in nyctaxidb2;
        """
    pd.read_sql(queryString,connection)


![Meglévő táblák megtekintése](./media/machine-learning-data-science-vm-do-ten-things/Python_View_Existing_Tables_Hive_v3.PNG)

<span data-ttu-id="220cd-326">Nézzük hello bejegyzések száma a minden hónap és hello gyakoriságát Formabontó, vagy nem található a hello út táblában:</span><span class="sxs-lookup"><span data-stu-id="220cd-326">Let's look at hello number of records in each month and hello frequencies of tipped or not in hello trip table:</span></span>

    queryString = """
        select month, count(*) from nyctaxidb.trip group by month;
        """
    results = pd.read_sql(queryString,connection)

    %matplotlib inline

    results.columns = ['month', 'trip_count']
    df = results.copy()
    df.index = df['month']
    df['trip_count'].plot(kind='bar')


![Bejegyzések száma havonta a ábrázolása](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Number_Records_by_Month_v3.PNG)

    queryString = """
        SELECT tipped, COUNT(*) AS tip_freq
        FROM
        (
            SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
            FROM nyctaxidb.fare
        )tc
        GROUP BY tipped;
        """
    results = pd.read_sql(queryString,connection)

    results.columns = ['tipped', 'trip_count']
    df = results.copy()
    df.index = df['tipped']
    df['trip_count'].plot(kind='bar')


![Tipp gyakoriságot ábrázolása](./media/machine-learning-data-science-vm-do-ten-things/Exploration_Frequency_tip_or_not_v3.PNG)

<span data-ttu-id="220cd-329">Azt is számítási hello felvételi helye és dropoff helye közötti távolság szerint, és hasonlítsa össze toohello út távolságot.</span><span class="sxs-lookup"><span data-stu-id="220cd-329">We can also compute hello distance between pickup location and dropoff location and then compare it toohello trip distance.</span></span>

    queryString = """
                    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
                        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
                        *radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
                        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
                        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
                        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*
                        pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance
                        from nyctaxidb.trip
                        where month=1
                            and pickup_longitude between -90 and -30
                            and pickup_latitude between 30 and 90
                            and dropoff_longitude between -90 and -30
                            and dropoff_latitude between 30 and 90;
                """
    results = pd.read_sql(queryString,connection)
    results.head(5)


![A felvételi és dropoff tábla](./media/machine-learning-data-science-vm-do-ten-things/Exploration_compute_pickup_dropoff_distance_v2.PNG)

    results.columns = ['pickup_longitude', 'pickup_latitude', 'dropoff_longitude',
                       'dropoff_latitude', 'trip_distance', 'trip_time_in_secs', 'direct_distance']
    df = results.loc[results['trip_distance']<=100] #remove outliers
    df = df.loc[df['direct_distance']<=100] #remove outliers
    plt.scatter(df['direct_distance'], df['trip_distance'])


![A felvételi/dropoff távolság tootrip távolság ábrázolása](./media/machine-learning-data-science-vm-do-ten-things/Exploration_direct_distance_trip_distance_v2.PNG)

<span data-ttu-id="220cd-332">Most tegyük készítse el lefelé-mintát (1 %) adatokat a modellezési.</span><span class="sxs-lookup"><span data-stu-id="220cd-332">Now let's prepare a down-sampled (1%) set of data for modeling.</span></span> <span data-ttu-id="220cd-333">A Machine Learning-olvasó modul használhatjuk ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="220cd-333">We can use this data in Machine Learning reader module.</span></span>

        queryString = """
        create  table if not exists nyctaxi_downsampled_dataset_testNEW (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\\n'
        stored as textfile;
        """
        cursor.execute(queryString)

        --- now insert contents of hello join into hello preceding internal table

        queryString = """
        insert overwrite table nyctaxi_downsampled_dataset_testNEW
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class
        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        3959*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        radians(180)/180/2),2)-cos(pickup_latitude*radians(180)/180)
        *cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*radians(180)/180/2),2)
        +cos(pickup_latitude*radians(180)/180)*cos(dropoff_latitude*radians(180)/180)*pow(sin((dropoff_longitude-pickup_longitude)*radians(180)/180/2),2))) as direct_distance,
        rand() as sample_key

        from trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01
        """
        cursor.execute(queryString)

<span data-ttu-id="220cd-334">Egy kis idő után látható hello adatok betöltése a Hadoop-fürtök:</span><span class="sxs-lookup"><span data-stu-id="220cd-334">After a while, you can see hello data has been loaded in Hadoop clusters:</span></span>

    queryString = """
        select * from nyctaxi_downsampled_dataset limit 10;
        """
    cursor.execute(queryString)
    pd.read_sql(queryString,connection)


![Az adatok tábla](./media/machine-learning-data-science-vm-do-ten-things/DownSample_Data_For_Modeling_v2.PNG)

<span data-ttu-id="220cd-336">**A Machine Learning segítségével HDI-adatok olvasása: olvasó modul**</span><span class="sxs-lookup"><span data-stu-id="220cd-336">**Read data from HDI using Machine Learning: reader module**</span></span>

<span data-ttu-id="220cd-337">Is használhatja hello **olvasó** modul a Machine Learning Studio tooaccess hello adatbázisban az Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="220cd-337">You may also use hello **reader** module in Machine Learning Studio tooaccess hello database in Hadoop cluster.</span></span> <span data-ttu-id="220cd-338">Csatlakoztassa a HDI-fürtökhöz hello hitelesítő adatait, és Azure Storage-fiók tooenable ing gépi tanulási modellek, adatbázist használ a HDI-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="220cd-338">Plug in hello credentials of your HDI clusters and Azure Storage Account tooenable build ing machine learning models using database in HDI clusters.</span></span>

![Olvasó modul tulajdonságai](./media/machine-learning-data-science-vm-do-ten-things/AML_Reader_Hive.PNG)

<span data-ttu-id="220cd-340">hello pontozott adatkészletet is megtekinthetők:</span><span class="sxs-lookup"><span data-stu-id="220cd-340">hello scored dataset can then be viewed:</span></span>

![Pontozott dataset megtekintése](./media/machine-learning-data-science-vm-do-ten-things/AML_Model_Results.PNG)

### <a name="azure-sql-data-warehouse--databases"></a><span data-ttu-id="220cd-342">Az Azure SQL Data Warehouse & adatbázisok</span><span class="sxs-lookup"><span data-stu-id="220cd-342">Azure SQL Data Warehouse & databases</span></span>
<span data-ttu-id="220cd-343">Az SQL Data Warehouse az egy rugalmas data warehouse az SQL Server révén vállalati szintű szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="220cd-343">Azure SQL Data Warehouse is an elastic data warehouse as a service with enterprise-class SQL Server experience.</span></span>

<span data-ttu-id="220cd-344">Jelen hello utasításokat követve építhető ki az Azure SQL Data Warehouse [cikk](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span><span class="sxs-lookup"><span data-stu-id="220cd-344">You can provision your Azure SQL Data Warehouse by following hello instructions provided in this [article](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md).</span></span> <span data-ttu-id="220cd-345">Miután az Azure SQL Data Warehouse kiépítése ezzel [forgatókönyv](machine-learning-data-science-process-sqldw-walkthrough.md) toodo adatok feltöltése, áttekintését és modellezési hello SQL Data Warehouse adatainak használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-345">Once you provision your Azure SQL Data Warehouse, you can use this [walkthrough](machine-learning-data-science-process-sqldw-walkthrough.md) toodo data upload, exploration and modeling using data within hello SQL Data Warehouse.</span></span>

#### <a name="azure-cosmos-db"></a><span data-ttu-id="220cd-346">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="220cd-346">Azure Cosmos DB</span></span>
<span data-ttu-id="220cd-347">Azure Cosmos-adatbázis egy NoSQL-adatbázis hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="220cd-347">Azure Cosmos DB is a NoSQL database in hello cloud.</span></span> <span data-ttu-id="220cd-348">Lehetővé teszi a dokumentumok például JSON toowork, és lehetővé teszi a toostore és lekérdezés hello dokumentumokat.</span><span class="sxs-lookup"><span data-stu-id="220cd-348">It allows you toowork with documents like JSON and allows you toostore and query hello documents.</span></span>

<span data-ttu-id="220cd-349">Egy szükséges lépéseket tooaccess Azure Cosmos DB követően – hello DSVM toodo hello van szüksége.</span><span class="sxs-lookup"><span data-stu-id="220cd-349">You need toodo hello following per-requisites steps tooaccess Azure Cosmos DB from hello DSVM.</span></span>

1. <span data-ttu-id="220cd-350">A DocumentDB Python SDK telepítése (Futtatás ```pip install pydocumentdb``` parancssorból)</span><span class="sxs-lookup"><span data-stu-id="220cd-350">Install DocumentDB Python SDK (Run ```pip install pydocumentdb``` from command prompt)</span></span>
2. <span data-ttu-id="220cd-351">Hozzon létre egy Azure Cosmos DB fiókot és az adatbázis [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="220cd-351">Create an Azure Cosmos DB account and a database from [Azure portal](https://portal.azure.com)</span></span>
3. <span data-ttu-id="220cd-352">"Azure Cosmos DB áttelepítési eszköz" letölthető [Itt](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) és bontsa ki az Ön által választott tooa könyvtár</span><span class="sxs-lookup"><span data-stu-id="220cd-352">Download "Azure Cosmos DB Migration Tool" from [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) and extract tooa directory of your choice</span></span>
4. <span data-ttu-id="220cd-353">Adatimportálás JSON (mexikói adatok) tárolja egy [nyilvános blob](https://cahandson.blob.core.windows.net/samples/volcano.json) be a következő parancs paraméterei toohello áttelepítési eszköz (dtui.exe hello Cosmos DB áttelepítési eszköz telepítési hello könyvtárból) Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="220cd-353">Import JSON data (volcano data) stored on a [public blob](https://cahandson.blob.core.windows.net/samples/volcano.json) into Cosmos DB with following command parameters toohello migration tool (dtui.exe from hello directory where you installed hello Cosmos DB Migration Tool).</span></span> <span data-ttu-id="220cd-354">Adja meg a hello forrása és célja helyet ezekkel a paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="220cd-354">Enter hello source and target location with these parameters:</span></span>
   
    <span data-ttu-id="220cd-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/; AccountKey = [[kulcs]; adatbázis mexikói /t.Collection:volcano1 =</span><span class="sxs-lookup"><span data-stu-id="220cd-355">/s:JsonFile /s.Files:https://cahandson.blob.core.windows.net/samples/volcano.json /t:DocumentDBBulk /t.ConnectionString:AccountEndpoint=https://[DocDBAccountName].documents.azure.com:443/;AccountKey=[[KEY];Database=volcano /t.Collection:volcano1</span></span>

<span data-ttu-id="220cd-356">Miután hello adatokat importál, elvégezheti a tooJupyter és a nyitott hello notebook című *DocumentDBSample* python, amely tartalmazza a DocumentDB tooaccess code, és tegye a néhány alapvető lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="220cd-356">Once you import hello data, you can go tooJupyter and open hello notebook titled *DocumentDBSample* which contains python code tooaccess DocumentDB and do some basic querying.</span></span> <span data-ttu-id="220cd-357">További kapcsolatos Cosmos DB hello szolgáltatás felkeresésével [dokumentációs oldal](https://docs.microsoft.com/azure/cosmos-db/).</span><span class="sxs-lookup"><span data-stu-id="220cd-357">You can learn more about Cosmos DB by visiting hello service [documentation page](https://docs.microsoft.com/azure/cosmos-db/).</span></span>

## <a name="8-build-reports-and-dashboard-using-hello-power-bi-desktop"></a><span data-ttu-id="220cd-358">8. Jelentések és hello Power BI Desktop használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="220cd-358">8. Build reports and dashboard using hello Power BI Desktop</span></span>
<span data-ttu-id="220cd-359">Ossza meg velünk megjelenítheti a Power BI toogain visual insightsban hello adatokká Cosmos DB példa megelőző hello bekerül hello mexikói JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="220cd-359">Let us visualize hello Volcano JSON file that we saw in hello preceding Cosmos DB example in Power BI toogain visual insights into hello data.</span></span> <span data-ttu-id="220cd-360">Részletes utasítások találhatók hello [Power BI-cikk](../cosmos-db/powerbi-visualize.md).</span><span class="sxs-lookup"><span data-stu-id="220cd-360">Detailed steps are available in hello [Power BI article](../cosmos-db/powerbi-visualize.md).</span></span> <span data-ttu-id="220cd-361">Az alábbiakban hello magas szintű lépéseket:</span><span class="sxs-lookup"><span data-stu-id="220cd-361">Here are hello high-level steps:</span></span>

1. <span data-ttu-id="220cd-362">Nyissa meg a Power BI Desktopban, és hajtsa végre az "Adatok beolvasása".</span><span class="sxs-lookup"><span data-stu-id="220cd-362">Open Power BI Desktop and do "Get Data".</span></span> <span data-ttu-id="220cd-363">Adja meg a hello az URL-címet: https://cahandson.blob.core.windows.net/samples/volcano.json</span><span class="sxs-lookup"><span data-stu-id="220cd-363">Specify hello URL as: https://cahandson.blob.core.windows.net/samples/volcano.json</span></span>
2. <span data-ttu-id="220cd-364">Megtekintheti az importált listaként hello JSON bejegyzések</span><span class="sxs-lookup"><span data-stu-id="220cd-364">You should see hello JSON records imported as a list</span></span>
3. <span data-ttu-id="220cd-365">Táblázatból hello lista tooa, a Power BI együttműködhet hello azonos</span><span class="sxs-lookup"><span data-stu-id="220cd-365">Convert hello list tooa table so Power BI can work with hello same</span></span>
4. <span data-ttu-id="220cd-366">Bontsa ki a hello oszlopok hello kattintva bontsa ki a ikon (hello egy hello "balra nyíl billentyűt és a jobbra mutató nyílra" ikon sarkában hello oszlop hello)</span><span class="sxs-lookup"><span data-stu-id="220cd-366">Expand hello columns by clicking on hello expand icon (hello one with hello "left arrow and a right arrow" icon on hello right of hello column)</span></span>
5. <span data-ttu-id="220cd-367">Figyelje meg, hogy a helye "Rekord" mező.</span><span class="sxs-lookup"><span data-stu-id="220cd-367">Notice that location is a "Record" field.</span></span> <span data-ttu-id="220cd-368">Bontsa ki a hello rekord, és csak hello koordináták válassza.</span><span class="sxs-lookup"><span data-stu-id="220cd-368">Expand hello record and select only hello coordinates.</span></span> <span data-ttu-id="220cd-369">Koordináta egy listaoszlop</span><span class="sxs-lookup"><span data-stu-id="220cd-369">Coordinate is a list column</span></span>
6. <span data-ttu-id="220cd-370">Vegyen fel egy új oszlop tooconvert hello lista koordináta oszlopot vesszővel külön LatLong oszlopba hozzáfűzésével hello két elemeinek hello koordináta listamező hello képlet szerint ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span><span class="sxs-lookup"><span data-stu-id="220cd-370">Add a new column tooconvert hello list coordinate column into a comma separate LatLong column concatenating hello two elements in hello coordinate list field using hello formula ```Text.From([coordinates]{1})&","&Text.From([coordinates]{0})```.</span></span>
7. <span data-ttu-id="220cd-371">Végül átalakítása hello ```Elevation``` oszlop tooDecimal és select hello **Bezárás** és **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="220cd-371">Finally convert hello ```Elevation``` column tooDecimal and select hello **Close** and **Apply**.</span></span>

<span data-ttu-id="220cd-372">Helyett az előző lépésekben, illessze be a következő kódot, amely a kimenő használt hello lépéseket hello hello speciális szerkesztő a Power bi-ban, amely lehetővé teszi egy lekérdezési nyelv a toowrite hello adatátalakítást.</span><span class="sxs-lookup"><span data-stu-id="220cd-372">Instead of preceding steps, you can paste hello following code that scripts out hello steps used in hello Advanced Editor in Power BI that allows you toowrite hello data transformations in a query language.</span></span>

    let
        Source = Json.Document(Web.Contents("https://cahandson.blob.core.windows.net/samples/volcano.json")),
        #"Converted tooTable" = Table.FromList(Source, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
        #"Expanded Column1" = Table.ExpandRecordColumn(#"Converted tooTable", "Column1", {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}, {"Volcano Name", "Country", "Region", "Location", "Elevation", "Type", "Status", "Last Known Eruption", "id"}),
        #"Expanded Location" = Table.ExpandRecordColumn(#"Expanded Column1", "Location", {"coordinates"}, {"coordinates"}),
        #"Added Custom" = Table.AddColumn(#"Expanded Location", "LatLong", each Text.From([coordinates]{1})&","&Text.From([coordinates]{0})),
        #"Changed Type" = Table.TransformColumnTypes(#"Added Custom",{{"Elevation", type number}})
    in
        #"Changed Type"



<span data-ttu-id="220cd-373">Most már rendelkezik hello adatokat a Power BI adatmodell.</span><span class="sxs-lookup"><span data-stu-id="220cd-373">You now have hello data in your Power BI data model.</span></span> <span data-ttu-id="220cd-374">A Power BI desktop megjelenjen-e az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="220cd-374">Your Power BI desktop should appear as follows:</span></span>

![Power BI Desktop](./media/machine-learning-data-science-vm-do-ten-things/PowerBIVolcanoData.png)

<span data-ttu-id="220cd-376">Megkezdheti a jelentések és a képi megjelenítések hello adatok modell használatával.</span><span class="sxs-lookup"><span data-stu-id="220cd-376">You can start building reports and visualizations using hello data model.</span></span> <span data-ttu-id="220cd-377">A lépésekkel hello ezen [Power BI-cikk](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild jelentést.</span><span class="sxs-lookup"><span data-stu-id="220cd-377">You can follow hello steps in this [Power BI article](../cosmos-db/powerbi-visualize.md#build-the-reports) toobuild a report.</span></span> <span data-ttu-id="220cd-378">hello end eredménye egy jelentést, amely a következőhöz hasonló hello.</span><span class="sxs-lookup"><span data-stu-id="220cd-378">hello end result is a report that looks like hello following.</span></span>

![A Power BI Desktop jelentés nézet – Power BI connector](./media/machine-learning-data-science-vm-do-ten-things/power_bi_connector_pbireportview2.png)

## <a name="9-dynamically-scale-your-dsvm-toomeet-your-project-needs"></a><span data-ttu-id="220cd-380">9. A DSVM toomeet a projektet dinamikusan méretezése</span><span class="sxs-lookup"><span data-stu-id="220cd-380">9. Dynamically scale your DSVM toomeet your project needs</span></span>
<span data-ttu-id="220cd-381">A projekt kell hello DSVM toomeet fel és le méretezheti.</span><span class="sxs-lookup"><span data-stu-id="220cd-381">You can scale up and down hello DSVM toomeet your project needs.</span></span> <span data-ttu-id="220cd-382">Ha nem kívánja toouse hello VM hello este vagy a hétvégeken, csak leállíthat hello a virtuális gép hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="220cd-382">If you don't need toouse hello VM in hello evening or weekends, you can just shut down hello VM from hello [Azure portal](https://portal.azure.com).</span></span>

> [!NOTE]
> <span data-ttu-id="220cd-383">Ön tudomásával költségeket, ha csupán hello operációs rendszer Leállítás gombját hello virtuális gép használ.</span><span class="sxs-lookup"><span data-stu-id="220cd-383">You incur compute charges if you use just hello Operating system shutdown button on hello VM.</span></span>  
> 
> 

<span data-ttu-id="220cd-384">Ha meg kell toohandle néhány nagy méretű elemzési és több Processzor és/vagy a memóriát és/vagy a lemez kapacitását CPU mag, memóriakapacitása és lemeztípusok (beleértve az SSD-meghajtókon) a számítási és a költségvetési igényeinek megfelelő Virtuálisgép-méretek széles választéka találja.</span><span class="sxs-lookup"><span data-stu-id="220cd-384">If you need toohandle some large-scale analysis and need more CPU and/or memory and/or disk capacity you can find a large choice of VM sizes in terms of CPU cores, memory capacity and disk types (including Solid state drives) that meet your compute and budgetary needs.</span></span> <span data-ttu-id="220cd-385">hello az óránkénti számítási árképzési együtt a virtuális gépek teljes listája megtalálható hello [Azure Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/) lap.</span><span class="sxs-lookup"><span data-stu-id="220cd-385">hello full list of VMs along with their hourly compute pricing is available on hello [Azure Virtual Machines Pricing](https://azure.microsoft.com/pricing/details/virtual-machines/) page.</span></span>

<span data-ttu-id="220cd-386">Hasonlóképpen ha csökkenti a virtuális gép feldolgozási kapacitás szükség (például: egy nagyobb munkaterhelés tooa Hadoop vagy egy Spark-fürt helyezte), lefelé hello hello fürt méretezheti [Azure-portálon](https://portal.azure.com) toohello beállításokat a virtuális gép, és a példány.</span><span class="sxs-lookup"><span data-stu-id="220cd-386">Similarly, if your need for VM processing capacity reduces (for example: you moved a major workload tooa Hadoop or a Spark cluster), you can scale down hello cluster from hello [Azure portal](https://portal.azure.com) and going toohello settings of your VM instance.</span></span> <span data-ttu-id="220cd-387">Íme egy képernyőkép.</span><span class="sxs-lookup"><span data-stu-id="220cd-387">Here is a screenshot.</span></span>

![Virtuálisgép-példány beállítások](./media/machine-learning-data-science-vm-do-ten-things/VMScaling.PNG)

## <a name="10-install-additional-tools-on-your-virtual-machine"></a><span data-ttu-id="220cd-389">10. A virtuális gép további eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="220cd-389">10. Install additional tools on your virtual machine</span></span>
<span data-ttu-id="220cd-390">A Microsoft csomagolását biztosak vagyunk abban, hogy a rendszer képes tooaddress hello közös adatok analytics igényeinek, és számos takaríthat meg kerülése tooinstall alkalommal és állítsa be a környezetek egyenként és takaríthat meg, hogy pénzt fizető csak olyan erőforrásokat, amikor több eszközt használjon.</span><span class="sxs-lookup"><span data-stu-id="220cd-390">We have packaged several tools that we believe are able tooaddress many of hello common data analytics needs and that should save you time by avoiding having tooinstall and configure your environments one by one and save you money by paying only for resources that you use.</span></span>

<span data-ttu-id="220cd-391">Kihasználhatja a más az Azure data, és ez a cikk tooenhance analytics környezetét analytics szolgáltatások csatolást.</span><span class="sxs-lookup"><span data-stu-id="220cd-391">You can leverage other Azure data and analytics services profiled in this article tooenhance your analytics environment.</span></span> <span data-ttu-id="220cd-392">Tudjuk, hogy bizonyos esetekben az igényeinek szükség lehet további eszközeit, beleértve az egyes védett külső eszközök.</span><span class="sxs-lookup"><span data-stu-id="220cd-392">We understand that in some cases your needs may require additional tools, including some proprietary third-party tools.</span></span> <span data-ttu-id="220cd-393">Teljes rendszergazdai hozzáféréssel rendelkezzen hello virtuális gép tooinstall új eszközök van szüksége.</span><span class="sxs-lookup"><span data-stu-id="220cd-393">You have full administrative access on hello virtual machine tooinstall new tools you need.</span></span> <span data-ttu-id="220cd-394">A Python és előre nem telepített R további csomagokat is telepíthet.</span><span class="sxs-lookup"><span data-stu-id="220cd-394">You can also install additional packages in Python and R that are not pre-installed.</span></span> <span data-ttu-id="220cd-395">Python-hez is használhatja ```conda``` vagy ```pip```.</span><span class="sxs-lookup"><span data-stu-id="220cd-395">For Python you can use either ```conda``` or ```pip```.</span></span> <span data-ttu-id="220cd-396">Az R hello használható ```install.packages()``` hello R-konzol vagy hello IDE használja, és válassza a "**csomagok** -> **csomagok telepítése...** ".</span><span class="sxs-lookup"><span data-stu-id="220cd-396">For R you can use hello ```install.packages()``` in hello R console or use hello IDE and choose "**Packages** -> **Install Packages...**".</span></span>

## <a name="summary"></a><span data-ttu-id="220cd-397">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="220cd-397">Summary</span></span>
<span data-ttu-id="220cd-398">Ezek a néhány hello műveleteket végezhet el hello Microsoft adatok tudományos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="220cd-398">These are just some of hello things you can do on hello Microsoft Data Science Virtual Machine.</span></span> <span data-ttu-id="220cd-399">Számos dolgot további toomake teheti azt egy hatékony analytics környezetben.</span><span class="sxs-lookup"><span data-stu-id="220cd-399">There are many more things you can do toomake it an effective analytics environment.</span></span>

