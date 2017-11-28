---
title: "A Machine Learning Python ügyféloldali kódtár adatkészletek eléréséhez |} Microsoft Docs"
description: "Telepítheti és használhatja a Python ügyféloldali kódtár férhessen hozzá és felügyelhesse Azure Machine Learning adatok biztonságos helyen a helyi Python-környezetben."
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
ms.openlocfilehash: e3ae712e0f8d386f637520fbbff4b348bc86f32d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="access-datasets-with-python-using-the-azure-machine-learning-python-client-library"></a><span data-ttu-id="5008b-103">Hozzáférés az adathalmazokhoz Python segítségével, az Azure Machine Learning Python ügyfélkönyvtárat használva</span><span class="sxs-lookup"><span data-stu-id="5008b-103">Access datasets with Python using the Azure Machine Learning Python client library</span></span>
<span data-ttu-id="5008b-104">A Microsoft Azure Machine Learning Python ügyféloldali kódtár preview engedélyezheti a helyi Python-környezetben az Azure Machine Learning adatkészletekhez a biztonságos hozzáférést, és lehetővé teszi, hogy a létrehozását és kezelését egy munkaterület adathalmazok.</span><span class="sxs-lookup"><span data-stu-id="5008b-104">The preview of Microsoft Azure Machine Learning Python client library can enable secure access to your Azure Machine Learning datasets from a local Python environment and enables the creation and management of datasets in a workspace.</span></span>

<span data-ttu-id="5008b-105">Ez a témakör a kapcsolatos utasításokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5008b-105">This topic provides instructions on how to:</span></span>

* <span data-ttu-id="5008b-106">Telepítse a Machine Learning Python ügyféloldali kódtár</span><span class="sxs-lookup"><span data-stu-id="5008b-106">install the Machine Learning Python client library</span></span> 
* <span data-ttu-id="5008b-107">hozzáférés, és töltse fel az adatkészleteket, beleértve az beszerzése a helyi Python környezetből Azure Machine Learning adatkészletek hozzáférési útmutatást</span><span class="sxs-lookup"><span data-stu-id="5008b-107">access and upload datasets, including instructions on how to get authorization to access Azure Machine Learning datasets from your local Python environment</span></span>
* <span data-ttu-id="5008b-108">köztes adatkészletek elérje kísérletek</span><span class="sxs-lookup"><span data-stu-id="5008b-108">access intermediate datasets from experiments</span></span>
* <span data-ttu-id="5008b-109">használjon, Python adatkészletek számbavétele, metaadatok elérheti, olvassa el a dataset tartalmát, hozzon létre új adatkészletek és frissítheti a meglévő adatkészletek</span><span class="sxs-lookup"><span data-stu-id="5008b-109">use the Python client library to enumerate datasets, access metadata, read the contents of a dataset, create new datasets and update existing datasets</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <span data-ttu-id="5008b-110"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5008b-110"><a name="prerequisites"></a>Prerequisites</span></span>
<span data-ttu-id="5008b-111">A Python ügyféloldali kódtára a tesztek alapján a következő környezetekben:</span><span class="sxs-lookup"><span data-stu-id="5008b-111">The Python client library has been tested under the following environments:</span></span>

* <span data-ttu-id="5008b-112">Windows, Mac és Linux</span><span class="sxs-lookup"><span data-stu-id="5008b-112">Windows, Mac and Linux</span></span>
* <span data-ttu-id="5008b-113">Python 2.7, 3.3 és 3.4</span><span class="sxs-lookup"><span data-stu-id="5008b-113">Python 2.7, 3.3 and 3.4</span></span>

<span data-ttu-id="5008b-114">A következő csomag rendelkezik egy függőséget:</span><span class="sxs-lookup"><span data-stu-id="5008b-114">It has a dependency on the following packages:</span></span>

* <span data-ttu-id="5008b-115">kérések</span><span class="sxs-lookup"><span data-stu-id="5008b-115">requests</span></span>
* <span data-ttu-id="5008b-116">Python-dateutil</span><span class="sxs-lookup"><span data-stu-id="5008b-116">python-dateutil</span></span>
* <span data-ttu-id="5008b-117">pandas</span><span class="sxs-lookup"><span data-stu-id="5008b-117">pandas</span></span>

<span data-ttu-id="5008b-118">Javasoljuk, használjon, mint a rendelkező Python elosztási [Anaconda](http://continuum.io/downloads#all) vagy [lombkoronaszint](https://store.enthought.com/downloads/), amelyek a Python, IPython származnak, és a fenti három csomagot telepítve.</span><span class="sxs-lookup"><span data-stu-id="5008b-118">We recommend using a Python distribution such as [Anaconda](http://continuum.io/downloads#all) or [Canopy](https://store.enthought.com/downloads/), which come with Python, IPython and the three packages listed above installed.</span></span> <span data-ttu-id="5008b-119">Bár IPython nem feltétlenül szükséges, kezelésére és adatok interaktív megjelenítése egy nagyszerű környezetet is.</span><span class="sxs-lookup"><span data-stu-id="5008b-119">Although IPython is not strictly required, it is a great environment for manipulating and visualizing data interactively.</span></span>

### <span data-ttu-id="5008b-120"><a name="installation"></a>Az Azure Machine Learning Python ügyféloldali kódtár telepítése</span><span class="sxs-lookup"><span data-stu-id="5008b-120"><a name="installation"></a>How to install the Azure Machine Learning Python client library</span></span>
<span data-ttu-id="5008b-121">Az Azure Machine Learning Python ügyféloldali kódtár is telepíteni kell a témakörben ismertetett feladatok végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="5008b-121">The Azure Machine Learning Python client library must also be installed to complete the tasks outlined in this topic.</span></span> <span data-ttu-id="5008b-122">Az elérhető a [Python-Csomagindexet](https://pypi.python.org/pypi/azureml).</span><span class="sxs-lookup"><span data-stu-id="5008b-122">It is available from the [Python Package Index](https://pypi.python.org/pypi/azureml).</span></span> <span data-ttu-id="5008b-123">A telepítéshez a Python-környezetben a következő parancsot a a helyi Python-környezetben:</span><span class="sxs-lookup"><span data-stu-id="5008b-123">To install it in your Python environment, run the following command from your local Python environment:</span></span>

    pip install azureml

<span data-ttu-id="5008b-124">Azt is megteheti, töltse le és telepítse a forrásból származó [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span><span class="sxs-lookup"><span data-stu-id="5008b-124">Alternatively, you can download and install from the sources on [github](https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python).</span></span>

    python setup.py install

<span data-ttu-id="5008b-125">Git a gépen telepítve van, a pip használatával közvetlenül telepítse a git-tárházba:</span><span class="sxs-lookup"><span data-stu-id="5008b-125">If you have git installed on your machine, you can use pip to install directly from the git repository:</span></span>

    pip install git+https://github.com/Azure/Azure-MachineLearning-ClientLibrary-Python.git


## <span data-ttu-id="5008b-126"><a name="datasetAccess"></a>Studio kódtöredékek adatkészletek eléréséhez használja</span><span class="sxs-lookup"><span data-stu-id="5008b-126"><a name="datasetAccess"></a>Use Studio Code snippets to access datasets</span></span>
<span data-ttu-id="5008b-127">A Python ügyféloldali kódtár programozott hozzáférést biztosít a meglévő adatkészletek az adott időpontig futtatott kísérletek.</span><span class="sxs-lookup"><span data-stu-id="5008b-127">The Python client library gives you programmatic access to your existing datasets from experiments that have been run.</span></span>

<span data-ttu-id="5008b-128">A Studio webes felhasználói felületen keresztüli kódrészletek, amelyek tartalmazzák a szükséges információkat, töltse le és adatkészletek deszerializálni a hely számítógépén Pandas DataFrame objektumként is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5008b-128">From the Studio web interface, you can generate code snippets that include all the necessary information to download and deserialize datasets as Pandas DataFrame objects on your location machine.</span></span>

### <span data-ttu-id="5008b-129"><a name="security"></a>Az adatelérési biztonsági</span><span class="sxs-lookup"><span data-stu-id="5008b-129"><a name="security"></a>Security for data access</span></span>
<span data-ttu-id="5008b-130">A Python ügyféloldali kódtár használható tartalmazza a munkaterület azonosítója és engedélyezési Studio által előírt kódrészletek token.</span><span class="sxs-lookup"><span data-stu-id="5008b-130">The code snippets provided by Studio for use with the Python client library includes your workspace id and authorization token.</span></span> <span data-ttu-id="5008b-131">Ezek a munkaterület teljes hozzáférést biztosítanak, és védeni kell, például a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5008b-131">These provide full access to your workspace and must be protected, like a password.</span></span>

<span data-ttu-id="5008b-132">Biztonsági okokból a kód részlet szolgáltatás működik, csak a felhasználók számára, amelyek rendelkeznek a szerepkörük állítja be **tulajdonos** a munkaterülethez.</span><span class="sxs-lookup"><span data-stu-id="5008b-132">For security reasons, the code snippet functionality is only available to users that have their role set as **Owner** for the workspace.</span></span> <span data-ttu-id="5008b-133">A szerepkör az Azure Machine Learning Studióban jelenik meg a **felhasználók** lapon az **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5008b-133">Your role is displayed in Azure Machine Learning Studio on the **USERS** page under **Settings**.</span></span>

![Biztonság][security]

<span data-ttu-id="5008b-135">Ha nincs beállítva az a szerepkör **tulajdonos**, vagy kérelem tulajdonos következőre, vagy kérje meg a munkaterület biztosítja, hogy a kódrészletet a tulajdonosa használhatja.</span><span class="sxs-lookup"><span data-stu-id="5008b-135">If your role is not set as **Owner**, you can either request to be reinvited as an owner, or ask the owner of the workspace to provide you with the code snippet.</span></span>

<span data-ttu-id="5008b-136">A hitelesítési jogkivonat beszerzése, tegye a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="5008b-136">To obtain the authorization token, you can do one of the following:</span></span>

* <span data-ttu-id="5008b-137">Kérje meg a jogkivonat a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="5008b-137">Ask for a token from an owner.</span></span> <span data-ttu-id="5008b-138">Tulajdonosok érhetik el a hitelesítési tokenek saját munkaterület Studio a beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="5008b-138">Owners can access their authorization tokens from the Settings page of their workspace in Studio.</span></span> <span data-ttu-id="5008b-139">Válassza ki **beállítások** a bal oldali ablaktáblán, majd kattintson a **engedélyezési JOGKIVONATOK** az elsődleges és másodlagos jogkivonatok megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5008b-139">Select **Settings** from the left pane and click **AUTHORIZATION TOKENS** to see the primary and secondary tokens.</span></span>  <span data-ttu-id="5008b-140">Bár az elsődleges vagy a másodlagos engedélyezési jogkivonatok használható a kódrészletet, azt javasoljuk, hogy a tulajdonosok csak használ-e a másodlagos engedélyezési jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="5008b-140">Although either the primary or the secondary authorization tokens can be used in the code snippet, it is recommended that owners only share the secondary authorization tokens.</span></span>

![Engedélyezési jogkivonatok](./media/machine-learning-python-data-access/ml-python-access-settings-tokens.png)

* <span data-ttu-id="5008b-142">Kérje meg, hogy a szerepkör tulajdonosa előléptetni.</span><span class="sxs-lookup"><span data-stu-id="5008b-142">Ask to be promoted to role of owner.</span></span>  <span data-ttu-id="5008b-143">Ehhez a munkaterület aktuális tulajdonosának kell először távolítsa el azt a munkaterületet, majd újra hívni Önt, tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="5008b-143">To do this, a current owner of the workspace needs to first remove you from the workspace then re-invite you to it as an owner.</span></span>

<span data-ttu-id="5008b-144">Miután a fejlesztők beszerezte a munkaterület azonosítója és engedélyezési jogkivonat, képesek hozzáférni a munkaterület használatával a kódrészletet, függetlenül azok szerepét.</span><span class="sxs-lookup"><span data-stu-id="5008b-144">Once developers have obtained the workspace id and authorization token, they are able to access the workspace using the code snippet regardless of their role.</span></span>

<span data-ttu-id="5008b-145">A hitelesítési tokenek kezelt a **engedélyezési JOGKIVONATOK** lapon az **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="5008b-145">Authorization tokens are managed on the **AUTHORIZATION TOKENS** page under **SETTINGS**.</span></span> <span data-ttu-id="5008b-146">Újragenerálás őket, de ez az eljárás visszavonja az előző lexikális elem a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="5008b-146">You can regenerate them, but this procedure revokes access to the previous token.</span></span>

### <span data-ttu-id="5008b-147"><a name="accessingDatasets"></a>Hozzáférés adatkészletek helyi Python-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5008b-147"><a name="accessingDatasets"></a>Access datasets from a local Python application</span></span>
1. <span data-ttu-id="5008b-148">A Machine Learning Studióban, kattintson a **ADATKÉSZLETEK** a bal oldali navigációs sávon.</span><span class="sxs-lookup"><span data-stu-id="5008b-148">In Machine Learning Studio, click **DATASETS** in the navigation bar on the left.</span></span>
2. <span data-ttu-id="5008b-149">Válassza ki a szeretne hozzáférni adathalmaz.</span><span class="sxs-lookup"><span data-stu-id="5008b-149">Select the dataset you would like to access.</span></span> <span data-ttu-id="5008b-150">Az adathalmaz bármelyikét kiválaszthatja a **saját ADATKÉSZLETEK** lista vagy a **minták** listája.</span><span class="sxs-lookup"><span data-stu-id="5008b-150">You can select any of the datasets from the **MY DATASETS** list or from the **SAMPLES** list.</span></span>
3. <span data-ttu-id="5008b-151">Kattintson az alsó eszköztár **adatok hozzáférési kód generálása**.</span><span class="sxs-lookup"><span data-stu-id="5008b-151">From the bottom toolbar, click **Generate Data Access Code**.</span></span> <span data-ttu-id="5008b-152">Ha az adatok formátuma nem kompatibilis a Python ügyféloldali kódtár, ez a gomb le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="5008b-152">If the data is in a format incompatible with the Python client library, this button is disabled.</span></span>
   
    ![Adathalmazok][datasets]
4. <span data-ttu-id="5008b-154">Válassza ki a kódrészletet a ablakban jelenik meg, és másolja a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="5008b-154">Select the code snippet from the window that appears and copy it to your clipboard.</span></span>
   
    ![Hozzáférési kód][dataset-access-code]
5. <span data-ttu-id="5008b-156">Illessze be a notebook a helyi Python-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5008b-156">Paste the code into the notebook of your local Python application.</span></span>
   
    ![Notebook][ipython-dataset]

## <span data-ttu-id="5008b-158"><a name="accessingIntermediateDatasets"></a>A Machine Learning kísérleteket a hozzáférés köztes adatkészletek</span><span class="sxs-lookup"><span data-stu-id="5008b-158"><a name="accessingIntermediateDatasets"></a>Access intermediate datasets from Machine Learning experiments</span></span>
<span data-ttu-id="5008b-159">Egy kísérletben a Machine Learning Studio futtatása után is lehet a modulok kimeneti csomópontról hozzáférhetnek a köztes adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="5008b-159">After an experiment is run in the Machine Learning Studio, it is possible to access the intermediate datasets from the output nodes of modules.</span></span> <span data-ttu-id="5008b-160">Köztes adatkészletek olyan adatok, amelyek a létrehozott és használt köztes lépések egy modell eszköz futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="5008b-160">Intermediate datasets are data that has been created and used for intermediate steps when a model tool has been run.</span></span>

<span data-ttu-id="5008b-161">Mindaddig, amíg a adatok formátuma nem kompatibilis a Python ügyféloldali kódtára a köztes adatkészletek érhető el.</span><span class="sxs-lookup"><span data-stu-id="5008b-161">Intermediate datasets can be accessed as long as the data format is compatible with the Python client library.</span></span>

<span data-ttu-id="5008b-162">A következő formátum támogatott (Ezek állandók szerepelnek a `azureml.DataTypeIds` osztály):</span><span class="sxs-lookup"><span data-stu-id="5008b-162">The following formats are supported (constants for these are in the `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="5008b-163">Egyszerű szöveg</span><span class="sxs-lookup"><span data-stu-id="5008b-163">PlainText</span></span>
* <span data-ttu-id="5008b-164">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="5008b-164">GenericCSV</span></span>
* <span data-ttu-id="5008b-165">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="5008b-165">GenericTSV</span></span>
* <span data-ttu-id="5008b-166">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="5008b-166">GenericCSVNoHeader</span></span>
* <span data-ttu-id="5008b-167">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="5008b-167">GenericTSVNoHeader</span></span>

<span data-ttu-id="5008b-168">A formátum azt is meghatározhatja, hogy egy modul kimeneti csomópont fölött.</span><span class="sxs-lookup"><span data-stu-id="5008b-168">You can determine the format by hovering over a module output node.</span></span> <span data-ttu-id="5008b-169">A csomópont neve eszközleírásként együtt jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="5008b-169">It is displayed along with the node name, in a tooltip.</span></span>

<span data-ttu-id="5008b-170">Néhány a modulok, mint a [vegyes] [ split] modul nevű formátumra kimeneti `Dataset`, amely nem támogatja a Python ügyféloldali kódtár.</span><span class="sxs-lookup"><span data-stu-id="5008b-170">Some of the modules, such as the [Split][split] module, output to a format named `Dataset`, which is not supported by the Python client library.</span></span>

![A DataSet formátumban][dataset-format]

<span data-ttu-id="5008b-172">Kell használnia, mint a konverziós modul [CSV átalakítása][convert-to-csv], a kimenetnek feltölti egy támogatott formátumra.</span><span class="sxs-lookup"><span data-stu-id="5008b-172">You need to use a conversion module, such as [Convert to CSV][convert-to-csv], to get an output into a supported format.</span></span>

![GenericCSV formátum][csv-format]

<span data-ttu-id="5008b-174">A következő lépések bemutatják egy példa, amely hoz létre a kísérlet, futtatja, és a köztes dataset fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="5008b-174">The following steps show an example that creates an experiment, runs it and accesses the intermediate dataset.</span></span>

1. <span data-ttu-id="5008b-175">Hozzon létre egy új kísérlet.</span><span class="sxs-lookup"><span data-stu-id="5008b-175">Create a new experiment.</span></span>
2. <span data-ttu-id="5008b-176">Helyezze be egy **bináris osztályozási felnőtt nyilvántartásba bevétel dataset** modul.</span><span class="sxs-lookup"><span data-stu-id="5008b-176">Insert an **Adult Census Income Binary Classification dataset** module.</span></span>
3. <span data-ttu-id="5008b-177">Helyezze be a [vegyes] [ split] modul, és csatlakoztassa a bemeneti adatkészlet modul kimenetével.</span><span class="sxs-lookup"><span data-stu-id="5008b-177">Insert a [Split][split] module, and connect its input to the dataset module output.</span></span>
4. <span data-ttu-id="5008b-178">Helyezze be a [CSV átalakítása] [ convert-to-csv] modul, és csatlakoztassa a bemeneti egy a [vegyes] [ split] modul kimenete.</span><span class="sxs-lookup"><span data-stu-id="5008b-178">Insert a [Convert to CSV][convert-to-csv] module and connect its input to one of the [Split][split] module outputs.</span></span>
5. <span data-ttu-id="5008b-179">A kísérlet mentéséhez, majd futtassa és várja meg, amíg a futása befejeződik.</span><span class="sxs-lookup"><span data-stu-id="5008b-179">Save the experiment, run it, and wait for it to finish running.</span></span>
6. <span data-ttu-id="5008b-180">A kimeneti csomóponton kattintson a a [CSV átalakítása] [ convert-to-csv] modul.</span><span class="sxs-lookup"><span data-stu-id="5008b-180">Click the output node on the [Convert to CSV][convert-to-csv] module.</span></span>
7. <span data-ttu-id="5008b-181">Ha a helyi menü megjelenik, válassza ki a **adatok hozzáférési kód generálása**.</span><span class="sxs-lookup"><span data-stu-id="5008b-181">When the context menu appears, select **Generate Data Access Code**.</span></span>
   
    ![A helyi menü][experiment]
8. <span data-ttu-id="5008b-183">Válassza ki a kódrészletet, és a megjelenő ablakban másolja a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="5008b-183">Select the code snippet and copy it to your clipboard from the window that appears.</span></span>
   
    ![Hozzáférési kód][intermediate-dataset-access-code]
9. <span data-ttu-id="5008b-185">Illessze be a kódját a notebook.</span><span class="sxs-lookup"><span data-stu-id="5008b-185">Paste the code in your notebook.</span></span>
   
    ![Notebook][ipython-intermediate-dataset]
10. <span data-ttu-id="5008b-187">Az adatok matplotlib használatával jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="5008b-187">You can visualize the data using matplotlib.</span></span> <span data-ttu-id="5008b-188">Ez a kor oszlop hisztogram jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="5008b-188">This displays in a histogram for the age column:</span></span>
    
    ![Hisztogram][ipython-histogram]

## <span data-ttu-id="5008b-190"><a name="clientApis"></a>A Machine Learning Python ügyféloldali kódtár segítségével hozzáférni, olvasását, létrehozását és adatkészletek kezelése</span><span class="sxs-lookup"><span data-stu-id="5008b-190"><a name="clientApis"></a>Use the Machine Learning Python client library to access, read, create, and manage datasets</span></span>
### <a name="workspace"></a><span data-ttu-id="5008b-191">Munkaterület</span><span class="sxs-lookup"><span data-stu-id="5008b-191">Workspace</span></span>
<span data-ttu-id="5008b-192">A munkaterületen a belépési pont, a Python ügyféloldali kódtára a rendszer.</span><span class="sxs-lookup"><span data-stu-id="5008b-192">The workspace is the entry point for the Python client library.</span></span> <span data-ttu-id="5008b-193">Adja meg a `Workspace` osztály a munkaterület azonosítója és engedélyezési jogkivonatot példány létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="5008b-193">Provide the `Workspace` class with your workspace id and authorization token to create an instance:</span></span>

    ws = Workspace(workspace_id='4c29e1adeba2e5a7cbeb0e4f4adfb4df',
                   authorization_token='f4f3ade2c6aefdb1afb043cd8bcf3daf')


### <a name="enumerate-datasets"></a><span data-ttu-id="5008b-194">Adatkészletek számbavétele</span><span class="sxs-lookup"><span data-stu-id="5008b-194">Enumerate datasets</span></span>
<span data-ttu-id="5008b-195">Operációs rendszer egy adott munkaterület minden adathalmazok:</span><span class="sxs-lookup"><span data-stu-id="5008b-195">To enumerate all datasets in a given workspace:</span></span>

    for ds in ws.datasets:
        print(ds.name)

<span data-ttu-id="5008b-196">Operációs rendszer csak a felhasználó által létrehozott adatkészletek:</span><span class="sxs-lookup"><span data-stu-id="5008b-196">To enumerate just the user-created datasets:</span></span>

    for ds in ws.user_datasets:
        print(ds.name)

<span data-ttu-id="5008b-197">Operációs rendszer csak a például adatkészleteket:</span><span class="sxs-lookup"><span data-stu-id="5008b-197">To enumerate just the example datasets:</span></span>

    for ds in ws.example_datasets:
        print(ds.name)

<span data-ttu-id="5008b-198">A DataSet adatkészlet neve (kis-és nagybetűket) érhető el:</span><span class="sxs-lookup"><span data-stu-id="5008b-198">You can access a dataset by name (which is case-sensitive):</span></span>

    ds = ws.datasets['my dataset name']

<span data-ttu-id="5008b-199">Vagy hozzá tud férni az index szerinti:</span><span class="sxs-lookup"><span data-stu-id="5008b-199">Or you can access it by index:</span></span>

    ds = ws.datasets[0]


### <a name="metadata"></a><span data-ttu-id="5008b-200">Metaadatok</span><span class="sxs-lookup"><span data-stu-id="5008b-200">Metadata</span></span>
<span data-ttu-id="5008b-201">Adatkészletek metaadatok mellett tartalom van.</span><span class="sxs-lookup"><span data-stu-id="5008b-201">Datasets have metadata, in addition to content.</span></span> <span data-ttu-id="5008b-202">(Köztes adatkészletek Ez a szabály alól kivételt, és minden metaadatot.)</span><span class="sxs-lookup"><span data-stu-id="5008b-202">(Intermediate datasets are an exception to this rule and do not have any metadata.)</span></span>

<span data-ttu-id="5008b-203">Néhány metaadatok értéket rendeli hozzá a felhasználó a létrehozás időpontjában:</span><span class="sxs-lookup"><span data-stu-id="5008b-203">Some metadata values are assigned by the user at creation time:</span></span>

    print(ds.name)
    print(ds.description)
    print(ds.family_id)
    print(ds.data_type_id)

<span data-ttu-id="5008b-204">Azure ML által hozzárendelt értékek:</span><span class="sxs-lookup"><span data-stu-id="5008b-204">Others are values assigned by Azure ML:</span></span>

    print(ds.id)
    print(ds.created_date)
    print(ds.size)

<span data-ttu-id="5008b-205">Tekintse meg a `SourceDataset` tudhat meg a rendelkezésre álló metaadatok osztály.</span><span class="sxs-lookup"><span data-stu-id="5008b-205">See the `SourceDataset` class for more on the available metadata.</span></span>

### <a name="read-contents"></a><span data-ttu-id="5008b-206">Tartalmának olvasása</span><span class="sxs-lookup"><span data-stu-id="5008b-206">Read contents</span></span>
<span data-ttu-id="5008b-207">A Machine Learning Studio által biztosított automatikusan kódtöredékek töltse le, és az adatkészlet egy Pandas DataFrame objektum deszerializálása.</span><span class="sxs-lookup"><span data-stu-id="5008b-207">The code snippets provided by Machine Learning Studio automatically download and deserialize the dataset to a Pandas DataFrame object.</span></span> <span data-ttu-id="5008b-208">Ez a lépés a `to_dataframe` módszert:</span><span class="sxs-lookup"><span data-stu-id="5008b-208">This is done with the `to_dataframe` method:</span></span>

    frame = ds.to_dataframe()

<span data-ttu-id="5008b-209">Töltse le a nyers adatokat, és hajtsa végre a deszerializálás, saját kezűleg szeretné, ha ez egy lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5008b-209">If you prefer to download the raw data, and perform the deserialization yourself, that is an option.</span></span> <span data-ttu-id="5008b-210">A jelenleg ez a lehetőség csak formátum például "ARFF", amely a Python ügyféloldali kódtár nem deszerializálható.</span><span class="sxs-lookup"><span data-stu-id="5008b-210">At the moment, this is the only option for formats such as 'ARFF', which the Python client library cannot deserialize.</span></span>

<span data-ttu-id="5008b-211">Tartalmának olvasása szövegként:</span><span class="sxs-lookup"><span data-stu-id="5008b-211">To read the contents as text:</span></span>

    text_data = ds.read_as_text()

<span data-ttu-id="5008b-212">Tartalmának olvasása bináris:</span><span class="sxs-lookup"><span data-stu-id="5008b-212">To read the contents as binary:</span></span>

    binary_data = ds.read_as_binary()

<span data-ttu-id="5008b-213">Nyissa meg a tartalmát egy stream csak is:</span><span class="sxs-lookup"><span data-stu-id="5008b-213">You can also just open a stream to the contents:</span></span>

    with ds.open() as file:
        binary_data_chunk = file.read(1000)


### <a name="create-a-new-dataset"></a><span data-ttu-id="5008b-214">Hozzon létre egy új adatkészlet</span><span class="sxs-lookup"><span data-stu-id="5008b-214">Create a new dataset</span></span>
<span data-ttu-id="5008b-215">A Python ügyféloldali kódtár lehetővé teszi, hogy adatkészletek a Python-programból feltölthetők.</span><span class="sxs-lookup"><span data-stu-id="5008b-215">The Python client library allows you to upload datasets from your Python program.</span></span> <span data-ttu-id="5008b-216">Ezek az adatkészletek állnak majd a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="5008b-216">These datasets are then available for use in your workspace.</span></span>

<span data-ttu-id="5008b-217">Ha az adatok egy Pandas DataFrame van, használja a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="5008b-217">If you have your data in a Pandas DataFrame, use the following code:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_dataframe(
        dataframe=frame,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="5008b-218">Ha az adatok már tartozik, akkor használhatja:</span><span class="sxs-lookup"><span data-stu-id="5008b-218">If your data is already serialized, you can use:</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets.add_from_raw_data(
        raw_data=raw_data,
        data_type_id=DataTypeIds.GenericCSV,
        name='my new dataset',
        description='my description'
    )

<span data-ttu-id="5008b-219">A Python ügyféloldali kódtár képes szerializálni a Pandas DataFrame a következő formátumban (Ezek állandók szerepelnek a `azureml.DataTypeIds` osztály):</span><span class="sxs-lookup"><span data-stu-id="5008b-219">The Python client library is able to serialize a Pandas DataFrame to the following formats (constants for these are in the `azureml.DataTypeIds` class):</span></span>

* <span data-ttu-id="5008b-220">Egyszerű szöveg</span><span class="sxs-lookup"><span data-stu-id="5008b-220">PlainText</span></span>
* <span data-ttu-id="5008b-221">GenericCSV</span><span class="sxs-lookup"><span data-stu-id="5008b-221">GenericCSV</span></span>
* <span data-ttu-id="5008b-222">GenericTSV</span><span class="sxs-lookup"><span data-stu-id="5008b-222">GenericTSV</span></span>
* <span data-ttu-id="5008b-223">GenericCSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="5008b-223">GenericCSVNoHeader</span></span>
* <span data-ttu-id="5008b-224">GenericTSVNoHeader</span><span class="sxs-lookup"><span data-stu-id="5008b-224">GenericTSVNoHeader</span></span>

### <a name="update-an-existing-dataset"></a><span data-ttu-id="5008b-225">Egy meglévő adatkészlet frissítése</span><span class="sxs-lookup"><span data-stu-id="5008b-225">Update an existing dataset</span></span>
<span data-ttu-id="5008b-226">Ha megpróbálja feltölteni egy meglévő adatkészlet egyező nevű új adatkészlet, egy ütközés hiba szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="5008b-226">If you try to upload a new dataset with a name that matches an existing dataset, you should get a conflict error.</span></span>

<span data-ttu-id="5008b-227">Egy meglévő adatkészlet frissítéséhez először kell a meglévő adatkészletet hivatkozás:</span><span class="sxs-lookup"><span data-stu-id="5008b-227">To update an existing dataset, you first need to get a reference to the existing dataset:</span></span>

    dataset = ws.datasets['existing dataset']

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="5008b-228">Ezután `update_from_dataframe` szerializálni, és cserélje ki annak tartalmát a DataSet adatkészlet Azure:</span><span class="sxs-lookup"><span data-stu-id="5008b-228">Then use `update_from_dataframe` to serialize and replace the contents of the dataset on Azure:</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(frame2)

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="5008b-229">Ha szeretné szerializálni az adatokat, más formátumba, adjon meg egy értéket a választható `data_type_id` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5008b-229">If you want to serialize the data to a different format, specify a value for the optional `data_type_id` parameter.</span></span>

    from azureml import DataTypeIds

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        data_type_id=DataTypeIds.GenericTSV,
    )

    print(dataset.data_type_id) # 'GenericTSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to jan 2015'

<span data-ttu-id="5008b-230">Meg nem kötelezően leírást adhat meg új értéket ad a `description` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5008b-230">You can optionally set a new description by specifying a value for the `description` parameter.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        description='data up to feb 2015',
    )

    print(dataset.data_type_id) # 'GenericCSV'
    print(dataset.name)         # 'existing dataset'
    print(dataset.description)  # 'data up to feb 2015'

<span data-ttu-id="5008b-231">Opcionálisan megadhatja egy új nevet értéket ad a `name` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5008b-231">You can optionally set a new name by specifying a value for the `name` parameter.</span></span> <span data-ttu-id="5008b-232">Ettől kezdve a fogja lekérni az adatkészlet csak az új névre.</span><span class="sxs-lookup"><span data-stu-id="5008b-232">From now on, you'll retrieve the dataset using the new name only.</span></span> <span data-ttu-id="5008b-233">Az alábbi kód frissíti az adatokat, nevét és leírását.</span><span class="sxs-lookup"><span data-stu-id="5008b-233">The following code updates the data, name and description.</span></span>

    dataset = ws.datasets['existing dataset']

    dataset.update_from_dataframe(
        dataframe=frame2,
        name='existing dataset v2',
        description='data up to feb 2015',
    )

    print(dataset.data_type_id)                    # 'GenericCSV'
    print(dataset.name)                            # 'existing dataset v2'
    print(dataset.description)                     # 'data up to feb 2015'

    print(ws.datasets['existing dataset v2'].name) # 'existing dataset v2'
    print(ws.datasets['existing dataset'].name)    # IndexError

<span data-ttu-id="5008b-234">A `data_type_id`, `name` és `description` paraméterek megadása nem kötelező, és alapértelmezés szerint az előző értéket.</span><span class="sxs-lookup"><span data-stu-id="5008b-234">The `data_type_id`, `name` and `description` parameters are optional and default to their previous value.</span></span> <span data-ttu-id="5008b-235">A `dataframe` paraméter megadása mindig kötelező.</span><span class="sxs-lookup"><span data-stu-id="5008b-235">The `dataframe` parameter is always required.</span></span>

<span data-ttu-id="5008b-236">Ha az adatok már szerializált, `update_from_raw_data` helyett `update_from_dataframe`.</span><span class="sxs-lookup"><span data-stu-id="5008b-236">If your data is already serialized, use `update_from_raw_data` instead of `update_from_dataframe`.</span></span> <span data-ttu-id="5008b-237">Ha a átadni `raw_data` helyett `dataframe`, hasonló módon működik.</span><span class="sxs-lookup"><span data-stu-id="5008b-237">If you just pass in `raw_data` instead of  `dataframe`, it works in a similar way.</span></span>

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

