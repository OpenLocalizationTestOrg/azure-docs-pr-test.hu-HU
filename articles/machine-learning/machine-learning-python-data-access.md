---
title: "a Machine Learning Python ügyféloldali kódtár aaaAccess adatkészletek |} Microsoft Docs"
description: "Telepítési és hello Python ügyfél könyvtár tooaccess használja, és Azure Machine Learning adatok biztonságos kezelésére a helyi Python-környezetben."
services: machine-learning
documentationcenter: python
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 9ab42272-c30c-4b7e-8e66-d64eafef22d0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: huvalo;bradsev
ms.openlocfilehash: f55067118f13c52bf677930a20836ce6989f8187
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-datasets-with-python-using-hello-azure-machine-learning-python-client-library"></a><span data-ttu-id="cdd85-103">Hozzáférés adatkészletek Python hello Azure Machine Learning Python ügyféloldali kódtár használata</span><span class="sxs-lookup"><span data-stu-id="cdd85-103">Access datasets with Python using hello Azure Machine Learning Python client library</span></span>
<span data-ttu-id="cdd85-104">hello preview Microsoft Azure Machine Learning Python ügyféloldali kódtár engedélyezheti a biztonságos hozzáférést tooyour helyi Python-környezetben az Azure Machine Learning adatkészletek, és lehetővé teszi, hogy a hello létrehozását és kezelését egy munkaterület adathalmazok.</span><span class="sxs-lookup"><span data-stu-id="cdd85-104">hello preview of Microsoft Azure Machine Learning Python client library can enable secure access tooyour Azure Machine Learning datasets from a local Python environment and enables hello creation and management of datasets in a workspace.</span></span>

<span data-ttu-id="cdd85-105">Ez a témakör a kapcsolatos utasításokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="cdd85-105">This topic provides instructions on how to:</span></span>

* <span data-ttu-id="cdd85-106">hello Machine Learning Python ügyféloldali kódtár telepítése</span><span class="sxs-lookup"><span data-stu-id="cdd85-106">install hello Machine Learning Python client library</span></span> 
* <span data-ttu-id="cdd85-107">hozzáférés, és töltse fel az adatkészleteket, beleértve a útmutatást tooget engedélyezési tooaccess a helyi Python-környezet az Azure Machine Learning adatkészletek</span><span class="sxs-lookup"><span data-stu-id="cdd85-107">access and upload datasets, including instructions on how tooget authorization tooaccess Azure Machine Learning datasets from your local Python environment</span></span>
* <span data-ttu-id="cdd85-108">köztes adatkészletek elérje kísérletek</span><span class="sxs-lookup"><span data-stu-id="cdd85-108">access intermediate datasets from experiments</span></span>
* <span data-ttu-id="cdd85-109">hello Python ügyfél könyvtár tooenumerate adatkészletek használata, metaadatok elérése, olvassa el a DataSet adatkészlet hello tartalmát, hozzon létre új adatkészletek és meglévő adatkészletek módosítása</span><span class="sxs-lookup"><span data-stu-id="cdd85-109">use hello Python client library tooenumerate datasets, access metadata, read hello contents of a dataset, create new datasets and update existing datasets</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <span data-ttu-id="cdd85-110"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cdd85-110"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="cdd85-111">hello Python ügyféloldali kódtár tesztelték hello környezetben a következő alapján:</span><span class="sxs-lookup"><span data-stu-id="cdd85-111">hello Python client library has been tested under hello following environments:</span></span>

* <span data-ttu-id="cdd85-112">Windows, Mac és Linux</span><span class="sxs-lookup"><span data-stu-id="cdd85-112">Windows, Mac and Linux</span></span>
* <span data-ttu-id="cdd85-113">Python 2.7, 3.3 és 3.4</span><span class="sxs-lookup"><span data-stu-id="cdd85-113">Python 2.7, 3.3 and 3.4</span></span>

<span data-ttu-id="cdd85-114">A következő csomagok hello függőség van:</span><span class="sxs-lookup"><span data-stu-id="cdd85-114">It has a dependency on hello following packages:</span></span>

* <span data-ttu-id="cdd85-115">kérések</span><span class="sxs-lookup"><span data-stu-id="cdd85-115">requests</span></span>
* <span data-ttu-id="cdd85-116">Python-dateutil</span><span class="sxs-lookup"><span data-stu-id="cdd85-116">python-dateutil</span></span>
* <span data-ttu-id="cdd85-117">pandas</span><span class="sxs-lookup"><span data-stu-id="cdd85-117">pandas</span></span>

<span data-ttu-id="cdd85-118">Javasoljuk, használjon, mint a rendelkező Python elosztási [Anaconda](http://continuum.io/downloads#all) vagy [lombkoronaszint](https://store.enthought.com/downloads/), amelyek származnak, Python, IPython és a fenti három hello csomagot telepítve.</span><span class="sxs-lookup"><span data-stu-id="cdd85-118">We recommend using a Python distribution such as [Anaconda](http://continuum.io/downloads#all) or [Canopy](https://store.enthought.com/downloads/), which come with Python, IPython and hello three packages listed above installed.</span></span> <span data-ttu-id="cdd85-119">Bár IPython nem feltétlenül szükséges, kezelésére és adatok interaktív megjelenítése egy nagyszerű környezetet is.</span><span class="sxs-lookup"><span data-stu-id="cdd85-119">Although IPython is not strictly required, it is a great environment for manipulating and visualizing data interactively.</span></span>

### <span data-ttu-id="cdd85-120"><a name="installation"></a>Hogyan tooinstall hello Azure Machine Learning Python ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="cdd85-120"><a name="installation"></a>How tooinstall hello Azure Machine Learning Python client library</span></span>
<span data-ttu-id="cdd85-121">hello Azure Machine Learning Python ügyféloldali kódtár kell telepített toocomplete hello feladatok az ebben a témakörben ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="cdd85-121">hello Azure Machine Learning Python client library must also be installed toocomplete hello tasks outlined in this topic.</span></span> <span data-ttu-id="cdd85-122">Elérhető a hello [Python-Csomagindexet](https://pypi.python.org/pypi/azureml).</span><span class="sxs-lookup"><span data-stu-id="cdd85-122">It is available from hello [Python Package Index](https://pypi.python.org/pypi/azureml).</span></span> <span data-ttu-id="cdd85-123">tooinstall a Python-környezetben, akkor futtassa hello parancsot a helyi Python-környezetben a következő:</span><span class="sxs-lookup"><span data-stu-id="cdd85-123">tooinstall it in your Python environment, run hello following command from your local Python environment:</span></span>

    pip install azureml

<span data-ttu-id="cdd85-124">Azt is megteheti, töltse le és telepítse az hello forrásból származó [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span><span class="sxs-lookup"><span data-stu-id="cdd85-124">Alternatively, you can download and install from hello sources on [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span></span>

    python setup.py install

<span data-ttu-id="cdd85-125">Ha git a gépen telepítve van, a pip tooinstall közvetlenül a git-tárház hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="cdd85-125">If you have git installed on your machine, you can use pip tooinstall directly from hello git repository:</span></span>

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <span data-ttu-id="cdd85-126"><a name="datasetAccess"></a>Studio Code kódtöredékek tooaccess adatkészletek használata</span><span class="sxs-lookup"><span data-stu-id="cdd85-126"><a name="datasetAccess"></a>Use Studio Code snippets tooaccess datasets</span></span>
<span data-ttu-id="cdd85-127">hello Python ügyféloldali kódtár lehetővé teszi az programozott hozzáférés tooyour meglévő adatkészletek az adott időpontig futtatott kísérletek.</span><span class="sxs-lookup"><span data-stu-id="cdd85-127">hello Python client library gives you programmatic access tooyour existing datasets from experiments that have been run.</span></span>

<span data-ttu-id="cdd85-128">Hello Studio webes felhasználói felületen keresztüli kódrészletek, amelyek közé tartozik az összes hello szükséges információkat toodownload és adatkészletek deszerializálni a hely számítógépén Pandas DataFrame objektumként is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="cdd85-128">From hello Studio web interface, you can generate code snippets that include all hello necessary information toodownload and deserialize datasets as Pandas DataFrame objects on your location machine.</span></span>

### <span data-ttu-id="cdd85-129"><a name="security"></a>Az adatelérési biztonsági</span><span class="sxs-lookup"><span data-stu-id="cdd85-129"><a name="security"></a>Security for data access</span></span>
<span data-ttu-id="cdd85-130">hello Python ügyféloldali kódtár használatára is tartalmaz, a munkaterület azonosítója és engedélyezési Studio által előírt kódtöredékek hello token.</span><span class="sxs-lookup"><span data-stu-id="cdd85-130">hello code snippets provided by Studio for use with hello Python client library includes your workspace id and authorization token.</span></span> <span data-ttu-id="cdd85-131">Ezek adja meg a teljes körű hozzáférési tooyour munkaterület, és védeni kell, például a jelszót.</span><span class="sxs-lookup"><span data-stu-id="cdd85-131">These provide full access tooyour workspace and must be protected, like a password.</span></span>

<span data-ttu-id="cdd85-132">Biztonsági okokból hello kód részlet funkció csak érhető el, amelyek rendelkeznek a szerepkörük állítja be toousers **tulajdonos** hello munkaterülethez.</span><span class="sxs-lookup"><span data-stu-id="cdd85-132">For security reasons, hello code snippet functionality is only available toousers that have their role set as **Owner** for hello workspace.</span></span> <span data-ttu-id="cdd85-133">A szerepkör hello megjelenik az Azure Machine Learning Studióban **felhasználók** lapon az **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="cdd85-133">Your role is displayed in Azure Machine Learning Studio on hello **USERS** page under **Settings**.</span></span>

![Biztonság][security]

<span data-ttu-id="cdd85-135">Ha nincs beállítva az a szerepkör **tulajdonos**, vagy kérheti tulajdonos következőre toobe, vagy kérje meg a hello munkaterület tooprovide hello tulajdonosának hello kódrészlettel meg.</span><span class="sxs-lookup"><span data-stu-id="cdd85-135">If your role is not set as **Owner**, you can either request toobe reinvited as an owner, or ask hello owner of hello workspace tooprovide you with hello code snippet.</span></span>

<span data-ttu-id="cdd85-136">tooobtain hello engedélyezési jogkivonat teheti hello a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="cdd85-136">tooobtain hello authorization token, you can do one of hello following:</span></span>

* <span data-ttu-id="cdd85-137">Kérje meg a jogkivonat a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="cdd85-137">Ask for a token from an owner.</span></span> <span data-ttu-id="cdd85-138">Tulajdonosok érhetik el a hitelesítési tokenek saját munkaterület Studio hello-beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="cdd85-138">Owners can access their authorization tokens from hello Settings page of their workspace in Studio.</span></span> <span data-ttu-id="cdd85-139">Válassza ki **beállítások** hello bal oldali ablaktáblán, és kattintson a **engedélyezési JOGKIVONATOK** toosee hello elsődleges és másodlagos jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="cdd85-139">Select **Settings** from hello left pane and click **AUTHORIZATION TOKENS** toosee hello primary and secondary tokens.</span></span>  <span data-ttu-id="cdd85-140">Bár a hello elsődleges vagy másodlagos engedélyezési jogkivonatok hello használható hello kódrészletet, azt javasoljuk, hogy a tulajdonosok csak közös hello másodlagos engedélyezési jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="cdd85-140">Although either hello primary or hello secondary authorization tokens can be used in hello code snippet, it is recommended that owners only share hello secondary authorization tokens.</span></span>

![Engedélyezési jogkivonatok](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* <span data-ttu-id="cdd85-142">Kérje meg a tulajdonosa előléptetni toobe toorole.</span><span class="sxs-lookup"><span data-stu-id="cdd85-142">Ask toobe promoted toorole of owner.</span></span>  <span data-ttu-id="cdd85-143">toodo ezt, a jelenlegi tulajdonos hello munkaterület igények toofirst távolítsa el, hello munkaterületen, majd újra meghívhatjuk tooit tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="cdd85-143">toodo this, a current owner of hello workspace needs toofirst remove you from hello workspace then re-invite you tooit as an owner.</span></span>

<span data-ttu-id="cdd85-144">A fejlesztők beszerezte hello munkaterület azonosítója és az engedélyezés után token,-e képes tooaccess hello munkaterület hello kódrészletet, függetlenül azok szerepkör használatával.</span><span class="sxs-lookup"><span data-stu-id="cdd85-144">Once developers have obtained hello workspace id and authorization token, they are able tooaccess hello workspace using hello code snippet regardless of their role.</span></span>

<span data-ttu-id="cdd85-145">Engedélyezési jogkivonatok objektumainak kezelése az hello **engedélyezési JOGKIVONATOK** lapon az **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="cdd85-145">Authorization tokens are managed on hello **AUTHORIZATION TOKENS** page under **SETTINGS**.</span></span> <span data-ttu-id="cdd85-146">Újragenerálás őket, de ez az eljárás jogkivonat toohello előző visszavonja.</span><span class="sxs-lookup"><span data-stu-id="cdd85-146">You can regenerate them, but this procedure revokes access toohello previous token.</span></span>

### <span data-ttu-id="cdd85-147"><a name="accessingDatasets"></a>Hozzáférés adatkészletek helyi Python-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cdd85-147"><a name="accessingDatasets"></a>Access datasets from a local Python application</span></span>
1. <span data-ttu-id="cdd85-148">A Machine Learning Studióban, kattintson a **ADATKÉSZLETEK** hello bal oldali navigációs sávján hello.</span><span class="sxs-lookup"><span data-stu-id="cdd85-148">In Machine Learning Studio, click **DATASETS** in hello navigation bar on hello left.</span></span>
2. <span data-ttu-id="cdd85-149">Válassza ki azt szeretné, hogy tooaccess hello adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="cdd85-149">Select hello dataset you would like tooaccess.</span></span> <span data-ttu-id="cdd85-150">Hello adatkészletek bármelyikét kiválaszthatja hello **saját ADATKÉSZLETEK** lista vagy a hello **minták** listája.</span><span class="sxs-lookup"><span data-stu-id="cdd85-150">You can select any of hello datasets from hello **MY DATASETS** list or from hello **SAMPLES** list.</span></span>
3. <span data-ttu-id="cdd85-151">Kattintson az alsó eszköztár hello, **adatok hozzáférési kód generálása**.</span><span class="sxs-lookup"><span data-stu-id="cdd85-151">From hello bottom toolbar, click **Generate Data Access Code**.</span></span> <span data-ttu-id="cdd85-152">Ha hello adatok formátuma nem kompatibilis a hello Python ügyféloldali kódtár, ez a gomb le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="cdd85-152">If hello data is in a format incompatible with hello Python client library, this button is disabled.</span></span>
   
    ![Adathalmazok][datasets]
4. <span data-ttu-id="cdd85-154">A hello megjelenő ablakban válassza ki a hello kódrészletet, és tooyour vágólapra másolásához.</span><span class="sxs-lookup"><span data-stu-id="cdd85-154">Select hello code snippet from hello window that appears and copy it tooyour clipboard.</span></span>
   
    ![Hozzáférési kód][dataset-access-code]
5. <span data-ttu-id="cdd85-156">Hello kód beillesztése hello notebook a helyi Python-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="cdd85-156">Paste hello code into hello notebook of your local Python application.</span></span>
   
    ![Notebook][ipython-dataset]

## <span data-ttu-id="cdd85-158"><a name="accessingIntermediateDatasets"></a>A Machine Learning kísérleteket a hozzáférés köztes adatkészletek</span><span class="sxs-lookup"><span data-stu-id="cdd85-158"><a name="accessingIntermediateDatasets"></a>Access intermediate datasets from Machine Learning experiments</span></span>
<span data-ttu-id="cdd85-159">Egy kísérletben a Machine Learning Studio hello futtatása után is lehetséges tooaccess hello köztes adatkészletek modulok hello kimeneti csomópontjából.</span><span class="sxs-lookup"><span data-stu-id="cdd85-159">After an experiment is run in hello Machine Learning Studio, it is possible tooaccess hello intermediate datasets from hello output nodes of modules.</span></span> <span data-ttu-id="cdd85-160">Köztes adatkészletek olyan adatok, amelyek a létrehozott és használt köztes lépések egy modell eszköz futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="cdd85-160">Intermediate datasets are data that has been created and used for intermediate steps when a model tool has been run.</span></span>

<span data-ttu-id="cdd85-161">Mindaddig, amíg hello adatformátum összeegyeztethető hello Python ügyféloldali kódtára a köztes adatkészletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="cdd85-161">Intermediate datasets can be accessed as long as hello data format is compatible with hello Python client library.</span></span>

<span data-ttu-id="cdd85-162">hello következő formátum támogatott (Ezek állandók szerepelnek hello `azureml.DataTypeIds` osztály):</span><span class="sxs-lookup"><span data-stu-id="cdd85-162">hello following formats are supported (constants for these are in hello `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="cdd85-163">Egyszerű szöveg</span><span class="sxs-lookup"><span data-stu-id="cdd85-163">PlainText</span></span>
* <span data-ttu-id="cdd85-164">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="cdd85-164">GenericCSV</span></span>
* <span data-ttu-id="cdd85-165">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="cdd85-165">GenericTSV</span></span>
* <span data-ttu-id="cdd85-166">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="cdd85-166">GenericCSVNoHeader</span></span>
* <span data-ttu-id="cdd85-167">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="cdd85-167">GenericTSVNoHeader</span></span>

<span data-ttu-id="cdd85-168">Hello formátum azt is meghatározhatja, hogy egy modul kimeneti csomópont fölött.</span><span class="sxs-lookup"><span data-stu-id="cdd85-168">You can determine hello format by hovering over a module output node.</span></span> <span data-ttu-id="cdd85-169">Hello csomópontnév eszközleírásként együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="cdd85-169">It is displayed along with hello node name, in a tooltip.</span></span>

<span data-ttu-id="cdd85-170">Néhány hello modulok, például hello [vegyes] [ split] modul, kimeneti tooa formátum nevű `Dataset`, hello Python ügyféloldali kódtár által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="cdd85-170">Some of hello modules, such as hello [Split][split] module, output tooa format named `Dataset`, which is not supported by hello Python client library.</span></span>

![A DataSet formátumban][dataset-format]

<span data-ttu-id="cdd85-172">Toouse átalakítás modul, például a kell [tooCSV átalakítása][convert-to-csv], tooget egy kimeneti egy támogatott formátumra.</span><span class="sxs-lookup"><span data-stu-id="cdd85-172">You need toouse a conversion module, such as [Convert tooCSV][convert-to-csv], tooget an output into a supported format.</span></span>

![GenericCSV formátum][csv-format]

<span data-ttu-id="cdd85-174">hello következő lépések bemutatják egy példa, amely létrehozza a kísérlet, futtatja, és hello köztes dataset hozzáfér.</span><span class="sxs-lookup"><span data-stu-id="cdd85-174">hello following steps show an example that creates an experiment, runs it and accesses hello intermediate dataset.</span></span>

1. <span data-ttu-id="cdd85-175">Hozzon létre egy új kísérlet.</span><span class="sxs-lookup"><span data-stu-id="cdd85-175">Create a new experiment.</span></span>
2. <span data-ttu-id="cdd85-176">Helyezze be egy **bináris osztályozási felnőtt nyilvántartásba bevétel dataset** modul.</span><span class="sxs-lookup"><span data-stu-id="cdd85-176">Insert an **Adult Census Income Binary Classification dataset** module.</span></span>
3. <span data-ttu-id="cdd85-177">Helyezze be egy [vegyes] [ split] modul, és csatlakoztassa a bemeneti toohello dataset modul kimenetét.</span><span class="sxs-lookup"><span data-stu-id="cdd85-177">Insert a [Split][split] module, and connect its input toohello dataset module output.</span></span>
4. <span data-ttu-id="cdd85-178">Helyezze be egy [tooCSV átalakítása] [ convert-to-csv] modul, és csatlakoztassa a bemeneti tooone hello a [vegyes] [ split] modul kimenete.</span><span class="sxs-lookup"><span data-stu-id="cdd85-178">Insert a [Convert tooCSV][convert-to-csv] module and connect its input tooone of hello [Split][split] module outputs.</span></span>
5. <span data-ttu-id="cdd85-179">Hello kísérlet mentse, majd futtassa, és várjon, amíg az futtató toofinish.</span><span class="sxs-lookup"><span data-stu-id="cdd85-179">Save hello experiment, run it, and wait for it toofinish running.</span></span>
6. <span data-ttu-id="cdd85-180">Hello kimeneti csomópontot a hello [tooCSV átalakítása] [ convert-to-csv] modul.</span><span class="sxs-lookup"><span data-stu-id="cdd85-180">Click hello output node on hello [Convert tooCSV][convert-to-csv] module.</span></span>
7. <span data-ttu-id="cdd85-181">Amikor hello helyi menü megjelenik, válassza ki a **adatok hozzáférési kód generálása**.</span><span class="sxs-lookup"><span data-stu-id="cdd85-181">When hello context menu appears, select **Generate Data Access Code**.</span></span>
   
    ![A helyi menü][experiment]
8. <span data-ttu-id="cdd85-183">Válassza ki a hello kódrészletet, és a megjelenő ablakban hello tooyour vágólapra másolja.</span><span class="sxs-lookup"><span data-stu-id="cdd85-183">Select hello code snippet and copy it tooyour clipboard from hello window that appears.</span></span>
   
    ![Hozzáférési kód][intermediate-dataset-access-code]
9. <span data-ttu-id="cdd85-185">Illessze be a notebook hello kódot.</span><span class="sxs-lookup"><span data-stu-id="cdd85-185">Paste hello code in your notebook.</span></span>
   
    ![Notebook][ipython-intermediate-dataset]
10. <span data-ttu-id="cdd85-187">Hello adatok matplotlib használatával jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="cdd85-187">You can visualize hello data using matplotlib.</span></span> <span data-ttu-id="cdd85-188">Ez a hisztogram hello kor oszlop jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="cdd85-188">This displays in a histogram for hello age column:</span></span>
    
    ![Hisztogram][ipython-histogram]

## <span data-ttu-id="cdd85-190"><a name="clientApis"></a>Hello Machine Learning Python ügyfél könyvtár tooaccess használja, olvasását, létrehozását és adatkészletek kezelése</span><span class="sxs-lookup"><span data-stu-id="cdd85-190"><a name="clientApis"></a>Use hello Machine Learning Python client library tooaccess, read, create, and manage datasets</span></span>
### <a name="workspace"></a><span data-ttu-id="cdd85-191">Munkaterület</span><span class="sxs-lookup"><span data-stu-id="cdd85-191">Workspace</span></span>
<span data-ttu-id="cdd85-192">hello munkaterület hello belépési pont hello Python ügyféloldali kódtára a rendszer.</span><span class="sxs-lookup"><span data-stu-id="cdd85-192">hello workspace is hello entry point for hello Python client library.</span></span> <span data-ttu-id="cdd85-193">Adja meg a hello `Workspace` a munkaterület azonosítója és engedélyezési jogkivonat toocreate az osztály egy példányát:</span><span class="sxs-lookup"><span data-stu-id="cdd85-193">Provide hello `Workspace` class with your workspace id and authorization token toocreate an instance:</span></span>

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a><span data-ttu-id="cdd85-194">Adatkészletek számbavétele</span><span class="sxs-lookup"><span data-stu-id="cdd85-194">Enumerate datasets</span></span>
<span data-ttu-id="cdd85-195">tooenumerate egy adott munkaterület minden adathalmazok:</span><span class="sxs-lookup"><span data-stu-id="cdd85-195">tooenumerate all datasets in a given workspace:</span></span>

    for ds in ws.datasets:
        print(ds.name)

<span data-ttu-id="cdd85-196">tooenumerate csak hello felhasználó által létrehozott adatkészletek:</span><span class="sxs-lookup"><span data-stu-id="cdd85-196">tooenumerate just hello user-created datasets:</span></span>

    for ds in ws.user_datasets:
        print(ds.name)

<span data-ttu-id="cdd85-197">tooenumerate csak hello példa adatkészletek:</span><span class="sxs-lookup"><span data-stu-id="cdd85-197">tooenumerate just hello example datasets:</span></span>

    for ds in ws.example_datasets:
        print(ds.name)

<span data-ttu-id="cdd85-198">A DataSet adatkészlet neve (kis-és nagybetűket) érhető el:</span><span class="sxs-lookup"><span data-stu-id="cdd85-198">You can access a dataset by name (which is case-sensitive):</span></span>

    ds = ws.datasets['my dataset name']

<span data-ttu-id="cdd85-199">Vagy hozzá tud férni az index szerinti:</span><span class="sxs-lookup"><span data-stu-id="cdd85-199">Or you can access it by index:</span></span>

    ds = ws.datasets[0]


### <a name="metadata"></a><span data-ttu-id="cdd85-200">Metaadatok</span><span class="sxs-lookup"><span data-stu-id="cdd85-200">Metadata</span></span>
<span data-ttu-id="cdd85-201">Adatkészletek metaadatok, továbbá toocontent rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cdd85-201">Datasets have metadata, in addition toocontent.</span></span> <span data-ttu-id="cdd85-202">(Köztes adatkészletek toothis kivételi szabályt, és minden metaadatot.)</span><span class="sxs-lookup"><span data-stu-id="cdd85-202">(Intermediate datasets are an exception toothis rule and do not have any metadata.)</span></span>

<span data-ttu-id="cdd85-203">Néhány metaadatok érték hello felhasználói rendeli hozzá a létrehozás időpontjában:</span><span class="sxs-lookup"><span data-stu-id="cdd85-203">Some metadata values are assigned by hello user at creation time:</span></span>

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

<span data-ttu-id="cdd85-204">Azure ML által hozzárendelt értékek:</span><span class="sxs-lookup"><span data-stu-id="cdd85-204">Others are values assigned by Azure ML:</span></span>

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

<span data-ttu-id="cdd85-205">Lásd: hello `SourceDataset` osztály több on hello elérhető metaadatok.</span><span class="sxs-lookup"><span data-stu-id="cdd85-205">See hello `SourceDataset` class for more on hello available metadata.</span></span>

### <a name="read-contents"></a><span data-ttu-id="cdd85-206">Tartalmának olvasása</span><span class="sxs-lookup"><span data-stu-id="cdd85-206">Read contents</span></span>
<span data-ttu-id="cdd85-207">a Machine Learning Studio által biztosított automatikusan hello kódtöredékek töltse le és hello dataset tooa Pandas DataFrame objektum deszerializálása.</span><span class="sxs-lookup"><span data-stu-id="cdd85-207">hello code snippets provided by Machine Learning Studio automatically download and deserialize hello dataset tooa Pandas DataFrame object.</span></span> <span data-ttu-id="cdd85-208">Ez a lépés hello `to_dataframe` módszert:</span><span class="sxs-lookup"><span data-stu-id="cdd85-208">This is done with hello `to_dataframe` method:</span></span>

    frame = ds.to_dataframe()

<span data-ttu-id="cdd85-209">Ha inkább toodownload hello nyers adatokat, és hajtsa végre hello deszerializálás, saját magának, ez egy lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cdd85-209">If you prefer toodownload hello raw data, and perform hello deserialization yourself, that is an option.</span></span> <span data-ttu-id="cdd85-210">Hello jelenleg ez a lehetőség hello csak formátum például "ARFF", mely hello Python ügyféloldali kódtár nem deszerializálható.</span><span class="sxs-lookup"><span data-stu-id="cdd85-210">At hello moment, this is hello only option for formats such as 'ARFF', which hello Python client library cannot deserialize.</span></span>

<span data-ttu-id="cdd85-211">szövegként tooread a hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="cdd85-211">tooread hello contents as text:</span></span>

    text_data = ds.read_as_text()

<span data-ttu-id="cdd85-212">bináris tooread a hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="cdd85-212">tooread hello contents as binary:</span></span>

    binary_data = ds.read_as_binary()

<span data-ttu-id="cdd85-213">Egy adatfolyam toohello tartalmát is ugyanúgy nyithatja meg:</span><span class="sxs-lookup"><span data-stu-id="cdd85-213">You can also just open a stream toohello contents:</span></span>

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a><span data-ttu-id="cdd85-214">Hozzon létre egy új adatkészlet</span><span class="sxs-lookup"><span data-stu-id="cdd85-214">Create a new dataset</span></span>
<span data-ttu-id="cdd85-215">hello Python ügyféloldali kódtár lehetővé teszi a Python programból tooupload adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="cdd85-215">hello Python client library allows you tooupload datasets from your Python program.</span></span> <span data-ttu-id="cdd85-216">Ezek az adatkészletek állnak majd a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="cdd85-216">These datasets are then available for use in your workspace.</span></span>

<span data-ttu-id="cdd85-217">Ha az adatok egy Pandas DataFrame van, használja a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="cdd85-217">If you have your data in a Pandas DataFrame, use hello following code:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="cdd85-218">Ha az adatok már tartozik, akkor használhatja:</span><span class="sxs-lookup"><span data-stu-id="cdd85-218">If your data is already serialized, you can use:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="cdd85-219">hello Python ügyféloldali kódtár egy Pandas DataFrame toohello következő formátumokat tudja tooserialize (Ezek állandók szerepelnek hello `azureml.DataTypeIds` osztály):</span><span class="sxs-lookup"><span data-stu-id="cdd85-219">hello Python client library is able tooserialize a Pandas DataFrame toohello following formats (constants for these are in hello `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="cdd85-220">Egyszerű szöveg</span><span class="sxs-lookup"><span data-stu-id="cdd85-220">PlainText</span></span>
* <span data-ttu-id="cdd85-221">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="cdd85-221">GenericCSV</span></span>
* <span data-ttu-id="cdd85-222">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="cdd85-222">GenericTSV</span></span>
* <span data-ttu-id="cdd85-223">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="cdd85-223">GenericCSVNoHeader</span></span>
* <span data-ttu-id="cdd85-224">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="cdd85-224">GenericTSVNoHeader</span></span>

### <a name="update-an-existing-dataset"></a><span data-ttu-id="cdd85-225">Egy meglévő adatkészlet frissítése</span><span class="sxs-lookup"><span data-stu-id="cdd85-225">Update an existing dataset</span></span>
<span data-ttu-id="cdd85-226">Ha egy meglévő adatkészlet egyező nevű új adatkészlet tooupload, egy ütközés hiba szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="cdd85-226">If you try tooupload a new dataset with a name that matches an existing dataset, you should get a conflict error.</span></span>

<span data-ttu-id="cdd85-227">tooupdate egyik meglévő adatkészletét, először kell tooget hivatkozás toohello meglévő adatkészlet:</span><span class="sxs-lookup"><span data-stu-id="cdd85-227">tooupdate an existing dataset, you first need tooget a reference toohello existing dataset:</span></span>

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="cdd85-228">Ezután `update_from_dataframe` hello dataset Azure tooserialize és csere hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="cdd85-228">Then use `update_from_dataframe` tooserialize and replace hello contents of hello dataset on Azure:</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="cdd85-229">Ha azt szeretné, hogy tooserialize hello tooa különböző adatformátum, adja meg a választható hello értékét `data_type_id` paraméter.</span><span class="sxs-lookup"><span data-stu-id="cdd85-229">If you want tooserialize hello data tooa different format, specify a value for hello optional `data_type_id` parameter.</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toojan 2015'

<span data-ttu-id="cdd85-230">Meg nem kötelezően leírást adhat meg új hello értékének megadásával `description` paraméter.</span><span class="sxs-lookup"><span data-stu-id="cdd85-230">You can optionally set a new description by specifying a value for hello `description` parameter.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up toofeb 2015'

<span data-ttu-id="cdd85-231">Opcionálisan megadhatja egy új nevet hello értékének megadásával `name` paraméter.</span><span class="sxs-lookup"><span data-stu-id="cdd85-231">You can optionally set a new name by specifying a value for hello `name` parameter.</span></span> <span data-ttu-id="cdd85-232">Ettől kezdve a be fogja olvasni hello adatkészlet az új néven hello.</span><span class="sxs-lookup"><span data-stu-id="cdd85-232">From now on, you'll retrieve hello dataset using hello new name only.</span></span> <span data-ttu-id="cdd85-233">a következő kód hello frissíti hello adatok, nevét és leírását.</span><span class="sxs-lookup"><span data-stu-id="cdd85-233">hello following code updates hello data, name and description.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up toofeb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up toofeb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

<span data-ttu-id="cdd85-234">Hello `data_type_id`, `name` és `description` paraméterek megadása nem kötelező, és az alapértelmezett tootheir előző érték.</span><span class="sxs-lookup"><span data-stu-id="cdd85-234">hello `data_type_id`, `name` and `description` parameters are optional and default tootheir previous value.</span></span> <span data-ttu-id="cdd85-235">Hello `dataframe` paraméter megadása mindig kötelező.</span><span class="sxs-lookup"><span data-stu-id="cdd85-235">hello `dataframe` parameter is always required.</span></span>

<span data-ttu-id="cdd85-236">Ha az adatok már szerializált, `update_from_raw_data` helyett `update_from_dataframe`.</span><span class="sxs-lookup"><span data-stu-id="cdd85-236">If your data is already serialized, use `update_from_raw_data` instead of `update_from_dataframe`.</span></span> <span data-ttu-id="cdd85-237">Ha a átadni `raw_data` helyett `dataframe`, hasonló módon működik.</span><span class="sxs-lookup"><span data-stu-id="cdd85-237">If you just pass in `raw_data` instead of  `dataframe`, it works in a similar way.</span></span>

<!-- Images -->
[security]:./media/machine-learning-python-data-access/security.png
[dataset-format]:./media/machine-learning-python-data-access/dataset-format.png
[csv-format]:./media/machine-learning-python-data-access/csv-format.png
[datasets]:./media/machine-learning-python-data-access/datasets.png
[dataset-access-code]:./media/machine-learning-python-data-access/dataset-access-code.png
[ipython-dataset]:./media/machine-learning-python-data-access/ipython-dataset.png
[experiment]:./media/machine-learning-python-data-access/experiment.png
[intermediate-dataset-access-code]:./media/machine-learning-python-data-access/intermediate-dataset-access-code.png
[ipython-intermediate-dataset]:./media/machine-learning-python-data-access/ipython-intermediate-dataset.png
[ipython-histogram]:./media/machine-learning-python-data-access/ipython-histogram.png


<!-- Module References -->
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/

