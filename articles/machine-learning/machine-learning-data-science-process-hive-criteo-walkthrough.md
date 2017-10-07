---
title: "művelet – 1 TB-os dataset Azure HDInsight Hadoop-fürtök használata a csapat az tudományos folyamata hello |} Microsoft Docs"
description: "Hello Team adatok tudományos folyamat használja egy végpont forgatókönyv egy HDInsight Hadoop alkalmazó toobuild fürt, és egy nagy méretű (1 TB) nyilvánosan elérhető adatkészlet a modell rendszerbe állítása"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 59b2af02e7840cb60a4b5b2f2c8ab0611df198ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a><span data-ttu-id="a3bd8-103">hello Team adatok tudományos folyamat művelet:-1 TB-os dataset Azure HDInsight Hadoop-fürtök használata</span><span class="sxs-lookup"><span data-stu-id="a3bd8-103">hello Team Data Science Process in action - Using an Azure HDInsight Hadoop Cluster on a 1 TB dataset</span></span>

<span data-ttu-id="a3bd8-104">Ebben a bemutatóban bemutatjuk használatával Team adatok tudományos folyamat hello egy végpont forgatókönyvet, és egy [Azure HDInsight Hadoop-fürt](https://azure.microsoft.com/services/hdinsight/) toostore, megismerkedhet a beállítást, a visszafejtés és nyilvánosan le a mintaadatokat egy hello rendelkezésre álló [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-104">In this walkthrough, we demonstrate using hello Team Data Science Process in an end-to-end scenario with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore, feature engineer, and down sample data from one of hello publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datasets.</span></span> <span data-ttu-id="a3bd8-105">A bináris osztályozási modell Azure Machine Learning toobuild ezeken az adatokon használjuk.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-105">We use Azure Machine Learning toobuild a binary classification model on this data.</span></span> <span data-ttu-id="a3bd8-106">Is megmutatjuk, hogyan toopublish ezek közül a modellek webszolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-106">We also show how toopublish one of these models as a Web service.</span></span>

<span data-ttu-id="a3bd8-107">Akkor is lehetséges toouse egy IPython notebook tooaccomplish hello feladatok jelenik meg ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-107">It is also possible toouse an IPython notebook tooaccomplish hello tasks presented in this walkthrough.</span></span> <span data-ttu-id="a3bd8-108">Felhasználók, akik volna, ez a megközelítés konzultáljon tootry például hello [Criteo forgatókönyv Hive ODBC-kapcsolat használatával](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) témakör.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-108">Users who would like tootry this approach should consult hello [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="a3bd8-109"><a name="dataset"></a>Criteo adatkészlet leírása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-109"><a name="dataset"></a>Criteo Dataset Description</span></span>
<span data-ttu-id="a3bd8-110">hello Criteo adata kattintson előrejelzés adatkészletre mutató körülbelül 370 GB tömörített gzip TSV fájlok (tömörítetlen ~1.3TB), amely több mint 4.3 milliárd rögzíti.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-110">hello Criteo data is a click prediction dataset that is approximately 370GB of gzip compressed TSV files (~1.3TB uncompressed), comprising more than 4.3 billion records.</span></span> <span data-ttu-id="a3bd8-111">A 24 napos származik kattintson által elérhetővé tett adatok [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-111">It is taken from 24 days of click data made available by [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span></span> <span data-ttu-id="a3bd8-112">Hello kényelmét szolgálja adatszakértőkön azt kell unzipped a toous tooexperiment elérhető adatokat.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-112">For hello convenience of data scientists, we have unzipped data available toous tooexperiment with.</span></span>

<span data-ttu-id="a3bd8-113">Ez az adatkészlet egyes rekord 40 oszlopokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-113">Each record in this dataset contains 40 columns:</span></span>

* <span data-ttu-id="a3bd8-114">hello első oszlopa egy címke oszlopot, amely azt jelzi, hogy a felhasználó rákattint egy **hozzáadása** (érték-1) vagy nem kattintson egy (a 0 érték)</span><span class="sxs-lookup"><span data-stu-id="a3bd8-114">hello first column is a label column that indicates whether a user clicks an **add** (value 1) or does not click one (value 0)</span></span>
* <span data-ttu-id="a3bd8-115">Ezután 13 oszlopok numerikus, és</span><span class="sxs-lookup"><span data-stu-id="a3bd8-115">next 13 columns are numeric, and</span></span>
* <span data-ttu-id="a3bd8-116">utolsó 26 kategorikus oszlopok</span><span class="sxs-lookup"><span data-stu-id="a3bd8-116">last 26 are categorical columns</span></span>

<span data-ttu-id="a3bd8-117">hello oszlopok vannak anonimizált adatokon alapul, és használja fel nevek sorozata: "Col1" (a címke oszlop hello) túl "Col40" (a hello utolsó kategorikus oszlop).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-117">hello columns are anonymized and use a series of enumerated names: "Col1" (for hello label column) too'Col40" (for hello last categorical column).</span></span>            

<span data-ttu-id="a3bd8-118">Íme hello kivonat első 20 oszlopok a dataset adatkészletből két megfigyelések (sorok):</span><span class="sxs-lookup"><span data-stu-id="a3bd8-118">Here is an excerpt of hello first 20 columns of two observations (rows) from this dataset:</span></span>

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

<span data-ttu-id="a3bd8-119">Nincsenek hiányzó értékek mindkét hello numerikus és kategorikus oszlopban ehhez az adatkészlethez.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-119">There are missing values in both hello numeric and categorical columns in this dataset.</span></span> <span data-ttu-id="a3bd8-120">Azt ismerteti, hogy egy egyszerű módszer a hiányzó értékek hello kezelése.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-120">We describe a simple method for handling hello missing values.</span></span> <span data-ttu-id="a3bd8-121">További részletek hello adatok írja során a Hive táblák tároljuk őket.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-121">Additional details of hello data are explored when we store them into Hive tables.</span></span>

<span data-ttu-id="a3bd8-122">**Definíciója:** *Átkattintós gyakorisága (Parancsra):* Ez az hello adatok kattintással hello százaléka.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-122">**Definition:** *Clickthrough rate (CTR):* This is hello percentage of clicks in hello data.</span></span> <span data-ttu-id="a3bd8-123">Criteo DataSet adatkészletben hello Parancsra, de hamarosan 3.3 % 0.033.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-123">In this Criteo dataset, hello CTR is about 3.3% or 0.033.</span></span>

## <span data-ttu-id="a3bd8-124"><a name="mltasks"></a>Előrejelzés feladatok példák</span><span class="sxs-lookup"><span data-stu-id="a3bd8-124"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="a3bd8-125">Ez a forgatókönyv két minta előrejelzés problémákat tárgyalja:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-125">Two sample prediction problems are addressed in this walkthrough:</span></span>

1. <span data-ttu-id="a3bd8-126">**Bináris osztályozási**: előrejelzi, hogy a felhasználó rákattint egy hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-126">**Binary classification**: Predicts whether a user clicked an add:</span></span>
   
   * <span data-ttu-id="a3bd8-127">Osztály: 0: Nincs kattintson</span><span class="sxs-lookup"><span data-stu-id="a3bd8-127">Class 0: No Click</span></span>
   * <span data-ttu-id="a3bd8-128">1 osztály: kattintson a</span><span class="sxs-lookup"><span data-stu-id="a3bd8-128">Class 1: Click</span></span>
2. <span data-ttu-id="a3bd8-129">**Regressziós**: hello valószínűségét, hogy egy felhasználó funkciókat ad kattintson előrejelzi.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-129">**Regression**: Predicts hello probability of an ad click from user features.</span></span>

## <span data-ttu-id="a3bd8-130"><a name="setup"></a>Tudományos adatok mentése beállítása egy HDInsight szabható Hadoop bemutatása-fürt</span><span class="sxs-lookup"><span data-stu-id="a3bd8-130"><a name="setup"></a>Set Up an HDInsight Hadoop cluster for data science</span></span>
<span data-ttu-id="a3bd8-131">**Megjegyzés:** Ez általában az egy **Admin** feladat.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-131">**Note:** This is typically an **Admin** task.</span></span>

<span data-ttu-id="a3bd8-132">Állítsa be a HDInsight-fürtökkel három lépésben prediktív elemzési megoldások létrehozásához Azure Adattudomány környezetében:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-132">Set up your Azure Data Science environment for building predictive analytics solutions with HDInsight clusters in three steps:</span></span>

1. <span data-ttu-id="a3bd8-133">[Hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md): ezt a tárfiókot az Azure Blob Storage tárolóban használt toostore adatai.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-133">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used toostore data in Azure Blob Storage.</span></span> <span data-ttu-id="a3bd8-134">a HDInsight-fürtök használt hello adatok itt tárolja.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-134">hello data used in HDInsight clusters is stored here.</span></span>
2. <span data-ttu-id="a3bd8-135">[Azure HDInsight Hadoop-fürtök testreszabása Adattudomány](machine-learning-data-science-customize-hadoop-cluster.md): Ezzel a lépéssel hoz létre egy Azure HDInsight Hadoop-fürt összes csomópontján telepítve 64 bites Anaconda Python 2.7.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-135">[Customize Azure HDInsight Hadoop Clusters for Data Science](machine-learning-data-science-customize-hadoop-cluster.md): This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="a3bd8-136">Nincsenek két fontos lépések az (a jelen témakörben ismertetett) toocomplete hello HDInsight-fürtök testreszabása.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-136">There are two important steps (described in this topic) toocomplete when customizing hello HDInsight cluster.</span></span>
   
   * <span data-ttu-id="a3bd8-137">Az 1. lépésben a HDInsight-fürthöz létrehozott létrehozásakor hello tárfiókban kell kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-137">You must link hello storage account created in step 1 with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="a3bd8-138">Ez a tárfiók hello fürtön belül feldolgozható adatainak eléréséhez használatos.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-138">This storage account is used for accessing data that can be processed within hello cluster.</span></span>
   * <span data-ttu-id="a3bd8-139">Létrehozás után engedélyeznie kell a távelérés toohello átjárócsomópont hello fürt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-139">You must enable Remote Access toohello head node of hello cluster after it is created.</span></span> <span data-ttu-id="a3bd8-140">Az itt megadott (eltérnek a saját létrehozásakor hello fürt számára megadott) hello távoli hozzáférési hitelesítő adatok megjegyzése: mintaadathalmazt toocomplete hello eljárásokat követve.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-140">Remember hello remote access credentials you specify here (different from those specified for hello cluster at its creation): you need them toocomplete hello following procedures.</span></span>
3. <span data-ttu-id="a3bd8-141">[Hozzon létre egy Azure ML munkaterületet](machine-learning-create-workspace.md): az Azure Machine Learning-munkaterület használata a machine learning modellek létrehozása után egy kezdeti adatok feltárása, és a mintavételi hello HDInsight-fürt le.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-141">[Create an Azure ML workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used for building machine learning models after an initial data exploration and down sampling on hello HDInsight cluster.</span></span>

## <span data-ttu-id="a3bd8-142"><a name="getdata"></a>GET és egy nyilvános forrásból származó adatokat</span><span class="sxs-lookup"><span data-stu-id="a3bd8-142"><a name="getdata"></a>Get and consume data from a public source</span></span>
<span data-ttu-id="a3bd8-143">Hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset hozzáférhet a hello hivatkozásra kattint, hello használati feltételek elfogadása és megadása.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-143">hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset can be accessed by clicking on hello link, accepting hello terms of use, and providing a name.</span></span> <span data-ttu-id="a3bd8-144">Pillanatkép készítése néz Ez itt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-144">A snapshot of what this looks like is shown here:</span></span>

![Criteo feltételek elfogadása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

<span data-ttu-id="a3bd8-146">Kattintson a **Folytatás tooDownload** tooread további hello dataset és elérhetőségét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-146">Click **Continue tooDownload** tooread more about hello dataset and its availability.</span></span>

<span data-ttu-id="a3bd8-147">hello adatok találhatók a nyilvános [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) helye: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-147">hello data resides in a public [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) location: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span></span> <span data-ttu-id="a3bd8-148">hello "wasb" tooAzure Blob Storage-helyre hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-148">hello "wasb" refers tooAzure Blob Storage location.</span></span> 

1. <span data-ttu-id="a3bd8-149">a nyilvános blob Storage tárolóban hello adatok három almappák tömörítetlen adatok állnak.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-149">hello data in this public blob storage consists of three subfolders of unzipped data.</span></span>
   
   1. <span data-ttu-id="a3bd8-150">almappa hello *nyers és száma/* tartalmaz hello első 21 napnyi adatot - nap után\_00 tooday\_20</span><span class="sxs-lookup"><span data-stu-id="a3bd8-150">hello subfolder *raw/count/* contains hello first 21 days of data - from day\_00 tooday\_20</span></span>
   2. <span data-ttu-id="a3bd8-151">almappa hello *nyers vonat / /* áll az adatok egy nap napi\_21</span><span class="sxs-lookup"><span data-stu-id="a3bd8-151">hello subfolder *raw/train/* consists of a single day of data, day\_21</span></span>
   3. <span data-ttu-id="a3bd8-152">almappa hello *nyers és tesztelési célú/* két napnyi adat, áll nap\_22-es és a nap\_23</span><span class="sxs-lookup"><span data-stu-id="a3bd8-152">hello subfolder *raw/test/* consists of two days of data, day\_22 and day\_23</span></span>
2. <span data-ttu-id="a3bd8-153">Azok számára, akik toostart hello nyers gzip adatokkal, ezek is hello fő mappában találhatók *nyers /* day_NN.gz, ahol NN 00 too23 tartalma szerint.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-153">For those who want toostart with hello raw gzip data, these are also available in hello main folder *raw/* as day_NN.gz, where NN goes from 00 too23.</span></span>

<span data-ttu-id="a3bd8-154">Egy másik módszert tooaccess vizsgálatát, és ezeket az adatokat bármely helyi letöltések kifejtett Ez az útmutató későbbi szakaszában Hive táblák létrehozásakor nem igénylő modell.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-154">An alternative approach tooaccess, explore, and model this data that does not require any local downloads is explained later in this walkthrough when we create Hive tables.</span></span>

## <span data-ttu-id="a3bd8-155"><a name="login"></a>Jelentkezzen be toohello fürt headnode</span><span class="sxs-lookup"><span data-stu-id="a3bd8-155"><a name="login"></a>Log in toohello cluster headnode</span></span>
<span data-ttu-id="a3bd8-156">a fürtbe hello használata hello toohello headnode toolog [Azure-portálon](https://ms.portal.azure.com) toolocate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-156">toolog in toohello headnode of hello cluster, use hello [Azure portal](https://ms.portal.azure.com) toolocate hello cluster.</span></span> <span data-ttu-id="a3bd8-157">Hello HDInsight elefánt ikonra a bal oldali hello, és kattintson duplán a fürt hello neve.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-157">Click hello HDInsight elephant icon on hello left and then double-click hello name of your cluster.</span></span> <span data-ttu-id="a3bd8-158">Keresse meg a toohello **konfigurációs** lapon hello CONNECT ikonra a hello hello lap aljára, és adja meg a távelérés hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-158">Navigate toohello **Configuration** tab, double-click hello CONNECT icon on hello bottom of hello page, and enter your remote access credentials when prompted.</span></span> <span data-ttu-id="a3bd8-159">Ezzel megnyitná toohello headnode hello fürt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-159">This takes you toohello headnode of hello cluster.</span></span>

<span data-ttu-id="a3bd8-160">Íme egy tipikus első bejelentkezéskor toohello fürt headnode néz:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-160">Here is what a typical first log in toohello cluster headnode looks like:</span></span>

![Jelentkezzen be toocluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

<span data-ttu-id="a3bd8-162">Hello bal oldali hello "Hadoop parancssori", amely a workhorse hello adatok feltárási látható.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-162">On hello left, we see hello "Hadoop Command Line", which is our workhorse for hello data exploration.</span></span> <span data-ttu-id="a3bd8-163">Azt is láthatja, két hasznos URL-címek – "Hadoop Yarn állapota" és "Hadoop neve csomópont".</span><span class="sxs-lookup"><span data-stu-id="a3bd8-163">We also see two useful URLs - "Hadoop Yarn Status" and "Hadoop Name Node".</span></span> <span data-ttu-id="a3bd8-164">hello csomópont URL-CÍMÉT hello fürtkonfiguráció részleteket nyújt és hello yarn állapot URL-CÍMÉT jeleníti meg a feladat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-164">hello yarn status URL shows job progress and hello name node URL gives details on hello cluster configuration.</span></span>

<span data-ttu-id="a3bd8-165">Most azt vannak beállítva, és kész toobegin első hello forgatókönyv része: Hive eszközzel és adatok Azure Machine Learning Felkészülés az adatok feltárása.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-165">Now we are set up and ready toobegin first part of hello walkthrough: data exploration using Hive and getting data ready for Azure Machine Learning.</span></span>

## <span data-ttu-id="a3bd8-166"><a name="hive-db-tables"></a>Hive-adatbázis és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-166"><a name="hive-db-tables"></a> Create Hive database and tables</span></span>
<span data-ttu-id="a3bd8-167">toocreate Hive táblák a Criteo az adatkészlethez, nyissa meg hello ***Hadoop parancssori*** a hello hello átjárócsomópont asztalát, és írja be a hello Hive directory hello parancs megadásával</span><span class="sxs-lookup"><span data-stu-id="a3bd8-167">toocreate Hive tables for our Criteo dataset, open hello ***Hadoop Command Line*** on hello desktop of hello head node, and enter hello Hive directory by entering hello command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="a3bd8-168">Ez a forgatókönyv minden Hive parancsot futtatja hello Hive bin / directory kérdés.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-168">Run all Hive commands in this walkthrough from hello Hive bin/ directory prompt.</span></span> <span data-ttu-id="a3bd8-169">Ezzel létrehozza az elérési út probléma merül fel automatikusan.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-169">This takes care of any path issues automatically.</span></span> <span data-ttu-id="a3bd8-170">"Struktúra directory prompt" hello kifejezéseket használjuk a "struktúra bin / directory prompt", és a "Hadoop parancssori" gombokra.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-170">We use hello terms "Hive directory prompt", "Hive bin/ directory prompt", and "Hadoop Command Line" interchangeably.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="a3bd8-171">tooexecute bármely Hive-lekérdezést egy mindig használhatja a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-171">tooexecute any Hive query, one can always use hello following commands:</span></span>
> 
> 

        cd %hive_home%\bin
        hive

<span data-ttu-id="a3bd8-172">Hello után Hive REPL jelenik meg, amelyen egy "struktúra >" aláírásához, egyszerűen Kivágás, és illessze be a hello lekérdezés tooexecute azt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-172">After hello Hive REPL appears with a "hive >"sign, simply cut and paste hello query tooexecute it.</span></span>

<span data-ttu-id="a3bd8-173">hello alábbi kód létrehoz egy "criteo" adatbázist és majd hoz létre 4 táblák:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-173">hello following code creates a database "criteo" and then generates 4 tables:</span></span>

* <span data-ttu-id="a3bd8-174">egy *számok generálásához tábla* nap napi épülő\_00 tooday\_20,</span><span class="sxs-lookup"><span data-stu-id="a3bd8-174">a *table for generating counts* built on days day\_00 tooday\_20,</span></span>
* <span data-ttu-id="a3bd8-175">egy *tábla használatra, vonat dataset hello* nap épülő\_21, és</span><span class="sxs-lookup"><span data-stu-id="a3bd8-175">a *table for use as hello train dataset* built on day\_21, and</span></span>
* <span data-ttu-id="a3bd8-176">két *hello használni táblák tesztelése adatkészletek* nap épülő\_22-es és a nap\_23 kulcsattribútumokkal.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-176">two *tables for use as hello test datasets* built on day\_22 and day\_23 respectively.</span></span>

<span data-ttu-id="a3bd8-177">A Microsoft ossza fel a tesztelési adatkészletnél két különböző tábla mert hello nap egyik Szabadnap, és toodetermine azt szeretnénk, ha hello modell észleli hello átkattintós sebessége szünnap és nem szünnap közötti különbségeket.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-177">We split our test dataset into two different tables because one of hello days is a holiday, and we want toodetermine if hello model can detect differences between a holiday and non-holiday from hello clickthrough rate.</span></span>

<span data-ttu-id="a3bd8-178">parancsfájl hello [minta &#95; hive &#95; hozzon létre &#95; criteo &#95; adatbázis &#95; és &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) kényelmi itt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-178">hello script [sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) is displayed here for convenience:</span></span>

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

<span data-ttu-id="a3bd8-179">A Microsoft ne feledje, hogy ezek a táblázatok külső, azt egyszerűen pont tooAzure Blob Storage (wasb) helyét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-179">We note that all these tables are external as we simply point tooAzure Blob Storage (wasb) locations.</span></span>

<span data-ttu-id="a3bd8-180">**Két módon tooexecute bármely Hive-lekérdezések, amelyek most említik van.**</span><span class="sxs-lookup"><span data-stu-id="a3bd8-180">**There are two ways tooexecute ANY Hive query that we now mention.**</span></span>

1. <span data-ttu-id="a3bd8-181">**Hive REPL parancssori használata hello**: hello először tooissue "hive" parancsot, és másolja és illessze be a lekérdezést a Hive REPL parancssori hello.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-181">**Using hello Hive REPL command-line**: hello first is tooissue a "hive" command and copy and paste a query at hello Hive REPL command-line.</span></span> <span data-ttu-id="a3bd8-182">toodo a, tegye:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-182">toodo this, do:</span></span>
   
        cd %hive_home%\bin
        hive
   
     <span data-ttu-id="a3bd8-183">Most, hello parancssori REPL, Kivágás és beillesztés a hello lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-183">Now at hello REPL command-line, cutting and pasting hello query executes it.</span></span>
2. <span data-ttu-id="a3bd8-184">**Mentés lekérdezi tooa fájl- és hello parancs végrehajtása**: hello második egy toosave hello lekérdezések tooa .hql fájl ([minta &#95; hive &#95; hozzon létre &#95; criteo &#95; adatbázis &#95; és &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) és arról, hogy a probléma hello következő majd parancsot a tooexecute hello lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-184">**Saving queries tooa file and executing hello command**: hello second is toosave hello queries tooa .hql file ([sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) and then issue hello following command tooexecute hello query:</span></span>
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a><span data-ttu-id="a3bd8-185">Erősítse meg az adatbázis és tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-185">Confirm database and table creation</span></span>
<span data-ttu-id="a3bd8-186">A következő Megerősítjük hello hello adatbázisának létrehozása a parancs követően – hello Hive bin hello / directory parancssorba:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-186">Next, we confirm hello creation of hello database with hello following command from hello Hive bin/ directory prompt:</span></span>

        hive -e "show databases;"

<span data-ttu-id="a3bd8-187">Ezen a következő:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-187">This gives:</span></span>

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

<span data-ttu-id="a3bd8-188">Ez megerősíti, hogy hello új adatbázis, "criteo" hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-188">This confirms hello creation of hello new database, "criteo".</span></span>

<span data-ttu-id="a3bd8-189">toosee milyen táblák létrehozott, egyszerűen kapni hello parancsot itt hello Hive bin / directory parancssorba:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-189">toosee what tables we created, we simply issue hello command here from hello Hive bin/ directory prompt:</span></span>

        hive -e "show tables in criteo;"

<span data-ttu-id="a3bd8-190">A következő kimeneti hello majd látható:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-190">We then see hello following output:</span></span>

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <span data-ttu-id="a3bd8-191"><a name="exploration"></a>Az adatok feltárása a Hive</span><span class="sxs-lookup"><span data-stu-id="a3bd8-191"><a name="exploration"></a> Data exploration in Hive</span></span>
<span data-ttu-id="a3bd8-192">Most a folyamatban van, kész toodo néhány alapvető adatok feltárása struktúrában.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-192">Now we are ready toodo some basic data exploration in Hive.</span></span> <span data-ttu-id="a3bd8-193">A Microsoft hello vonat szereplő példák hello száma alapján kezdődik, és adattáblák tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-193">We begin by counting hello number of examples in hello train and test data tables.</span></span>

### <a name="number-of-train-examples"></a><span data-ttu-id="a3bd8-194">Vonat példák száma</span><span class="sxs-lookup"><span data-stu-id="a3bd8-194">Number of train examples</span></span>
<span data-ttu-id="a3bd8-195">hello tartalmát [minta &#95; hive &#95; száma &#95; vonat &#95; tábla &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) itt látható:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-195">hello contents of [sample&#95;hive&#95;count&#95;train&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) are shown here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_train;

<span data-ttu-id="a3bd8-196">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-196">This yields:</span></span>

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

<span data-ttu-id="a3bd8-197">Azt is megteheti, egy lehetséges, hogy is kiadhatnak hello parancs követően – hello Hive bin / directory parancssorba:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-197">Alternatively, one may also issue hello following command from hello Hive bin/ directory prompt:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-hello-two-test-datasets"></a><span data-ttu-id="a3bd8-198">Tesztelési példák hello két teszt adatkészletek száma</span><span class="sxs-lookup"><span data-stu-id="a3bd8-198">Number of test examples in hello two test datasets</span></span>
<span data-ttu-id="a3bd8-199">A Microsoft most megadott számú hello hello két teszt adatkészletek példák.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-199">We now count hello number of examples in hello two test datasets.</span></span> <span data-ttu-id="a3bd8-200">hello tartalmát [minta &#95; hive &#95; száma &#95; criteo &#95; teszt &#95; nap &#95; 22 &#95; tábla &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) itt:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-200">hello contents of [sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;22&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) are here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

<span data-ttu-id="a3bd8-201">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-201">This yields:</span></span>

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

<span data-ttu-id="a3bd8-202">A szokásos módon, előfordulhat, hogy is nevezzük hello parancsfájl a hello Hive bin / könyvtár hello parancsot a parancssor:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-202">As usual, we may also call hello script from hello Hive bin/ directory prompt by issuing hello command:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

<span data-ttu-id="a3bd8-203">Végül azt vizsgálja meg az hello tesztelési adatkészletnél nap alapján tesztelési példák hello száma\_23.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-203">Finally, we examine hello number of test examples in hello test dataset based on day\_23.</span></span>

<span data-ttu-id="a3bd8-204">hello parancs toodo Ez az egyik csak látható hasonló toohello (tekintse meg a túl[minta &#95; hive &#95; száma &#95; criteo &#95; teszt &#95; nap &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span><span class="sxs-lookup"><span data-stu-id="a3bd8-204">hello command toodo this is similar toohello one just shown (refer too[sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;23&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

<span data-ttu-id="a3bd8-205">Ezen a következő:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-205">This gives:</span></span>

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-hello-train-dataset"></a><span data-ttu-id="a3bd8-206">Címke terjesztési hello vonat adatkészlet</span><span class="sxs-lookup"><span data-stu-id="a3bd8-206">Label distribution in hello train dataset</span></span>
<span data-ttu-id="a3bd8-207">hello címke terjesztési hello vonat adatkészlet jelentőséggel bír.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-207">hello label distribution in hello train dataset is of interest.</span></span> <span data-ttu-id="a3bd8-208">toosee, tartalmát megmutatjuk [minta &#95; hive &#95; criteo &#95; címke &#95; terjesztési &#95; vonat &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span><span class="sxs-lookup"><span data-stu-id="a3bd8-208">toosee this, we show contents of [sample&#95;hive&#95;criteo&#95;label&#95;distribution&#95;train&#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span></span>

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

<span data-ttu-id="a3bd8-209">Ez a hello címke terjesztési eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-209">This yields hello label distribution:</span></span>

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

<span data-ttu-id="a3bd8-210">Vegye figyelembe, hogy hello százalékos pozitív címkék készül 3.3 % (konzisztens hello eredeti adatkészlet).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-210">Note that hello percentage of positive labels is about 3.3% (consistent with hello original dataset).</span></span>

### <a name="histogram-distributions-of-some-numeric-variables-in-hello-train-dataset"></a><span data-ttu-id="a3bd8-211">Néhány numerikus változók hello hisztogram disztribúciók betanítása adatkészlet</span><span class="sxs-lookup"><span data-stu-id="a3bd8-211">Histogram distributions of some numeric variables in hello train dataset</span></span>
<span data-ttu-id="a3bd8-212">Használhatunk struktúrájának natív "hisztogram\_numerikus" milyen hello terjesztési hello numerikus változók a következőképpen néz ki toofind működik.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-212">We can use Hive's native "histogram\_numeric" function toofind out what hello distribution of hello numeric variables looks like.</span></span> <span data-ttu-id="a3bd8-213">Az alábbiakban hello tartalmát [minta &#95; hive &#95; criteo &#95; hisztogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span><span class="sxs-lookup"><span data-stu-id="a3bd8-213">Here are hello contents of [sample&#95;hive&#95;criteo&#95;histogram&#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span></span>

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

<span data-ttu-id="a3bd8-214">Ez adja eredményül a következő hello:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-214">This yields hello following:</span></span>

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

<span data-ttu-id="a3bd8-215">OLDALIRÁNYÚ NÉZET – hello felbontása Hive szolgál tooproduce hello szokásos lista helyett egy SQL-szerű kimeneti kombinációja.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-215">hello LATERAL VIEW - explode combination in Hive serves tooproduce a SQL-like output instead of hello usual list.</span></span> <span data-ttu-id="a3bd8-216">Vegye figyelembe, hogy a hello a tábla, hello első oszlop felel meg a toohello bin center és hello második toohello bin gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-216">Note that in hello this table, hello first column corresponds toohello bin center and hello second toohello bin frequency.</span></span>

### <a name="approximate-percentiles-of-some-numeric-variables-in-hello-train-dataset"></a><span data-ttu-id="a3bd8-217">Néhány numerikus változókra hello hozzávetőleges százalékos érték betanítása adatkészlet</span><span class="sxs-lookup"><span data-stu-id="a3bd8-217">Approximate percentiles of some numeric variables in hello train dataset</span></span>
<span data-ttu-id="a3bd8-218">A numerikus változókkal érdeklő a hello számítási hozzávetőleges százalékos érték a.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-218">Also of interest with numeric variables is hello computation of approximate percentiles.</span></span> <span data-ttu-id="a3bd8-219">Hive tartozó natív "PERCENTILIS\_hozzávetőleges" ezt az USA végzi.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-219">Hive's native "percentile\_approx" does this for us.</span></span> <span data-ttu-id="a3bd8-220">hello tartalmát [minta &#95; hive &#95; criteo &#95; hozzávetőleges &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) vannak:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-220">hello contents of [sample&#95;hive&#95;criteo&#95;approximate&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) are:</span></span>

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

<span data-ttu-id="a3bd8-221">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-221">This yields:</span></span>

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

<span data-ttu-id="a3bd8-222">A Microsoft remark, hogy százalékos érték hello terjesztése általában-e szorosan kapcsolódó toohello hisztogram terjesztési bármely numerikus változó.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-222">We remark that hello distribution of percentiles is closely related toohello histogram distribution of any numeric variable usually.</span></span>         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-hello-train-dataset"></a><span data-ttu-id="a3bd8-223">Hello vonat adatkészlet egyes kategorikus oszlopok egyedi értékek számának keresése</span><span class="sxs-lookup"><span data-stu-id="a3bd8-223">Find number of unique values for some categorical columns in hello train dataset</span></span>
<span data-ttu-id="a3bd8-224">A Folytatás hello adatok feltárása, most találtunk, néhány kategorikus oszlop hello tartanak egyedi értékek száma.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-224">Continuing hello data exploration, we now find, for some categorical columns, hello number of unique values they take.</span></span> <span data-ttu-id="a3bd8-225">toodo, tartalmát megmutatjuk [minta &#95; hive &#95; criteo & #95 egyedi; &#95; értékek &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span><span class="sxs-lookup"><span data-stu-id="a3bd8-225">toodo this, we show contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span></span>

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

<span data-ttu-id="a3bd8-226">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-226">This yields:</span></span>

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

<span data-ttu-id="a3bd8-227">Megjegyezzük, hogy rendelkezik-e Col15 19M egyedi értékeket!</span><span class="sxs-lookup"><span data-stu-id="a3bd8-227">We note that Col15 has 19M unique values!</span></span> <span data-ttu-id="a3bd8-228">Például a "egy közbeni kódolás" tooencode naïve technikák ilyen nagy dimenziós kategorikus változók használata lehessen hozni.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-228">Using naive techniques like "one-hot encoding" tooencode such high-dimensional categorical variables is infeasible.</span></span> <span data-ttu-id="a3bd8-229">Különösen azt ismertetik, és mutassa be nevű hatékony, hatékony módszer [tanulási a számlálás](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) a hatékony elleni probléma.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-229">In particular, we explain and demonstrate a powerful, robust technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) for tackling this problem efficiently.</span></span>

<span data-ttu-id="a3bd8-230">Ez alterület kategorikus oszlopokat, valamint az egyedi értékek számának hello megtekintésével végén azt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-230">We end this sub-section by looking at hello number of unique values for some other categorical columns as well.</span></span> <span data-ttu-id="a3bd8-231">hello tartalmát [minta &#95; hive &#95; criteo & #95 egyedi; &#95; értékek &#95; több &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) vannak:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-231">hello contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;multiple&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) are:</span></span>

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

<span data-ttu-id="a3bd8-232">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-232">This yields:</span></span>

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

<span data-ttu-id="a3bd8-233">Újra látható, hogy Col20, kivéve az összes hello más oszlopok számos egyedi értékeket tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-233">Again we see that except for Col20, all hello other columns have many unique values.</span></span>

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-hello-train-dataset"></a><span data-ttu-id="a3bd8-234">Közös előfordulási számát is párok hello vonat adatkészlet kategorikus változók</span><span class="sxs-lookup"><span data-stu-id="a3bd8-234">Co-occurrence counts of pairs of categorical variables in hello train dataset</span></span>

<span data-ttu-id="a3bd8-235">hello közös előfordulási számát is párok kategorikus változók az is fontos.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-235">hello co-occurrence counts of pairs of categorical variables is also of interest.</span></span> <span data-ttu-id="a3bd8-236">Ez lehet meghatározni a hello kóddal [minta &#95; hive &#95; criteo & #95 párosított; &#95; kategorikus &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span><span class="sxs-lookup"><span data-stu-id="a3bd8-236">This can be determined using hello code in [sample&#95;hive&#95;criteo&#95;paired&#95;categorical&#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span></span>

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

<span data-ttu-id="a3bd8-237">A Microsoft sorrend hello számok megfordíthatja a előfordulásainak kívánt számát, és tekintse meg hello felső 15 ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-237">We reverse order hello counts by their occurrence and look at hello top 15 in this case.</span></span> <span data-ttu-id="a3bd8-238">Biztosítanak számunkra:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-238">This gives us:</span></span>

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <span data-ttu-id="a3bd8-239"><a name="downsample"></a>Mintaként használható adathalmazt hello Azure Machine Learning le</span><span class="sxs-lookup"><span data-stu-id="a3bd8-239"><a name="downsample"></a> Down sample hello datasets for Azure Machine Learning</span></span>
<span data-ttu-id="a3bd8-240">Felderített hello adatkészleteket, és meg mutatja, hogyan lehet végezzük az ilyen típusú változó (többek között a következőket kombinációk), azt most le hello mintaadatkészletek feltárása, hogy azt az Azure Machine Learning modellek hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-240">Having explored hello datasets and demonstrated how we may do this type of exploration for any variables (including combinations), we now down sample hello data sets so that we can build models in Azure Machine Learning.</span></span> <span data-ttu-id="a3bd8-241">Adott azt összpontosítani hello probléma visszahívása: megadott példa attribútumok (Col2 - Col40 értékeinek szolgáltatás), azt megjósolható, hogy Oszlop1-e a 0 (nincs kattintson) vagy 1 (kattintson).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-241">Recall that hello problem we focus on is: given a set of example attributes (feature values from Col2 - Col40), we predict if Col1 is a 0 (no click) or a 1 (click).</span></span>

<span data-ttu-id="a3bd8-242">toodown a vonat sample, és tesztelje adatkészletek too1 % hello eredeti méret, struktúrájának natív RAND() függvény használjuk.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-242">toodown sample our train and test datasets too1% of hello original size, we use Hive's native RAND() function.</span></span> <span data-ttu-id="a3bd8-243">hello a következő parancsfájl [minta &#95; hive &#95; criteo &#95; felbontásának &#95; vonat &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) ezt hello vonat adatkészlet végzi:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-243">hello next script, [sample&#95;hive&#95;criteo&#95;downsample&#95;train&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) does this for hello train dataset:</span></span>

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

<span data-ttu-id="a3bd8-244">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-244">This yields:</span></span>

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

<span data-ttu-id="a3bd8-245">parancsfájl hello [minta &#95; hive &#95; criteo &#95; felbontásának &#95; teszt &#95; nap &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) minderre a tesztadatok, napi\_22:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-245">hello script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) does it for test data, day\_22:</span></span>

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

<span data-ttu-id="a3bd8-246">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-246">This yields:</span></span>

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


<span data-ttu-id="a3bd8-247">Végezetül, a parancsfájl hello [minta &#95; hive &#95; criteo &#95; felbontásának &#95; teszt &#95; nap &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) minderre a tesztadatok, napi\_23:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-247">Finally, hello script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) does it for test data, day\_23:</span></span>

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

<span data-ttu-id="a3bd8-248">Ez eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-248">This yields:</span></span>

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

<span data-ttu-id="a3bd8-249">Ennek, vagy folyamatban, a régebbi mintát készen toouse betanítása és felépítése az Azure Machine Learning modellek adathalmaz tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-249">With this, we are ready toouse our down sampled train and test datasets for building models in Azure Machine Learning.</span></span>

<span data-ttu-id="a3bd8-250">Összetevő végső fontos azt áthelyezni a Machine Learning, amely aggályokat hello száma tábla tooAzure előtt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-250">There is a final important component before we move on tooAzure Machine Learning, which is concerns hello count table.</span></span> <span data-ttu-id="a3bd8-251">Hello következő alárendelt szakaszban arról lesz szó ez részletesen bemutatható.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-251">In hello next sub-section, we discuss this in some detail.</span></span>

## <span data-ttu-id="a3bd8-252"><a name="count"></a>Hello száma tábla rövid döntéseken</span><span class="sxs-lookup"><span data-stu-id="a3bd8-252"><a name="count"></a> A brief discussion on hello count table</span></span>
<span data-ttu-id="a3bd8-253">Látott, mert több kategorikus változók nagyon magas granularitása adható meg.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-253">As we saw, several categorical variables have a very high dimensionality.</span></span> <span data-ttu-id="a3bd8-254">A forgatókönyv azt jelenleg egy hatékony módszer néven [tanulási a számlálás](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode egy hatékony és hatékony módon változókhoz.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-254">In our walkthrough, we present a powerful technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode these variables in an efficient, robust manner.</span></span> <span data-ttu-id="a3bd8-255">További információk ezzel a technikával hello hivatkozás van.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-255">More information on this technique is in hello link provided.</span></span>

[!NOTE]
><span data-ttu-id="a3bd8-256">A forgatókönyv azt összpontosítani száma táblák tooproduce kompakt ábrázolásai nagy dimenziós kategorikus szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-256">In this walkthrough, we focus on using count tables tooproduce compact representations of high-dimensional categorical features.</span></span> <span data-ttu-id="a3bd8-257">Ez nem az hello csak tooencode kategorikus szolgáltatások; egyéb technikák olvashat, érdekelt felhasználók lefoglalhatják [egy-közbeni-kódolás](http://en.wikipedia.org/wiki/One-hot) és [szolgáltatáskivonatolás](http://en.wikipedia.org/wiki/Feature_hashing).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-257">This is not hello only way tooencode categorical features; for more information on other techniques, interested users can check out [one-hot-encoding](http://en.wikipedia.org/wiki/One-hot) and [feature hashing](http://en.wikipedia.org/wiki/Feature_hashing).</span></span>
>

<span data-ttu-id="a3bd8-258">toobuild száma táblák hello számának adatai, a hello mappa nyers és száma használjuk hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-258">toobuild count tables on hello count data, we use hello data in hello folder raw/count.</span></span> <span data-ttu-id="a3bd8-259">A felhasználók megmutatjuk szakaszban modellezési hello hogyan toobuild ezek száma táblák teljesen, vagy másik lehetőségként toouse kategorikus szolgáltatások egy előre elkészített száma táblázatban azok explorations.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-259">In hello modeling section, we show users how toobuild these count tables for categorical features from scratch, or alternatively toouse a pre-built count table for their explorations.</span></span> <span data-ttu-id="a3bd8-260">Mi a következő, amikor irányítjuk túl "előzetesen elkészített száma táblák", azt jelenti, hogy általunk biztosított hello száma táblák használata.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-260">In what follows, when we refer too"pre-built count tables", we mean using hello count tables that we provide.</span></span> <span data-ttu-id="a3bd8-261">Hogyan tooaccess ezek a táblázatok biztosított hello a következő szakaszban részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-261">Detailed instructions on how tooaccess these tables are provided in hello next section.</span></span>

## <span data-ttu-id="a3bd8-262"><a name="aml"></a>A modell Azure Machine Learning segítségével létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-262"><a name="aml"></a> Build a model with Azure Machine Learning</span></span>
<span data-ttu-id="a3bd8-263">A modell létrehozása az Azure Machine Learning folyamatot követi ezeket a lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-263">Our model building process in Azure Machine Learning follows these steps:</span></span>

1. [<span data-ttu-id="a3bd8-264">Hello adatokat lekérni a Hive táblák az Azure gépi tanulás</span><span class="sxs-lookup"><span data-stu-id="a3bd8-264">Get hello data from Hive tables into Azure Machine Learning</span></span>](#step1)
2. [<span data-ttu-id="a3bd8-265">Hello kísérlet létrehozása: hello adatok és count táblákkal featurize törlése</span><span class="sxs-lookup"><span data-stu-id="a3bd8-265">Create hello experiment: clean hello data and featurize with count tables</span></span>](#step2)
3. [<span data-ttu-id="a3bd8-266">Build, tanítási és pontszám hello modell</span><span class="sxs-lookup"><span data-stu-id="a3bd8-266">Build, train, and score hello model</span></span>](#step3)
4. [<span data-ttu-id="a3bd8-267">Hello modell kiértékelése</span><span class="sxs-lookup"><span data-stu-id="a3bd8-267">Evaluate hello model</span></span>](#step4)
5. [<span data-ttu-id="a3bd8-268">Hello modell egy webszolgáltatás közzététele</span><span class="sxs-lookup"><span data-stu-id="a3bd8-268">Publish hello model as a web-service</span></span>](#step5)

<span data-ttu-id="a3bd8-269">Most már készen áll a toobuild modellek az Azure Machine Learning studio folyamatban.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-269">Now we are ready toobuild models in Azure Machine Learning studio.</span></span> <span data-ttu-id="a3bd8-270">A lefelé mintaadatokat Hive táblák hello fürt értékként menti.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-270">Our down sampled data is saved as Hive tables in hello cluster.</span></span> <span data-ttu-id="a3bd8-271">Hello Azure Machine Learning használjuk **és adatokat importálhat** modul tooread ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-271">We use hello Azure Machine Learning **Import Data** module tooread this data.</span></span> <span data-ttu-id="a3bd8-272">a következőkben hello hitelesítő adatok tooaccess hello tárfiók a fürt vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-272">hello credentials tooaccess hello storage account of this cluster are provided in what follows.</span></span>

### <span data-ttu-id="a3bd8-273"><a name="step1"></a>1. lépés: Adatokat lekérni a Hive táblák az Azure Machine Learning hello adatokat importálhat modullal, és válassza ki azt a tartalmazó gépi tanulási kísérlet</span><span class="sxs-lookup"><span data-stu-id="a3bd8-273"><a name="step1"></a> Step 1: Get data from Hive tables into Azure Machine Learning using hello Import Data module and select it for a machine learning experiment</span></span>
<span data-ttu-id="a3bd8-274">Kiválasztásával indítsa el a **+ új** -> **kísérlet** -> **üres kísérlet**.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-274">Start by selecting a **+NEW** -> **EXPERIMENT** -> **Blank Experiment**.</span></span> <span data-ttu-id="a3bd8-275">Ezt követően a hello **keresési** hello felül balra, keresse meg az "Adatok importálása" mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-275">Then, from hello **Search** box on hello top left, search for "Import Data".</span></span> <span data-ttu-id="a3bd8-276">A fogd és vidd hello **és adatokat importálhat** toohello kísérlet vászonra (hello üdvözlő képernyőt középső része) toouse hello modul az adatelérési modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-276">Drag and drop hello **Import Data** module on toohello experiment canvas (hello middle portion of hello screen) toouse hello module for data access.</span></span>

<span data-ttu-id="a3bd8-277">Ez az, hogy milyen hello **és adatokat importálhat** tűnik hello Hive tábla adatainak lekérése közben:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-277">This is what hello **Import Data** looks like while getting data from hello Hive table:</span></span>

![Adatok importálása adatok beolvasása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

<span data-ttu-id="a3bd8-279">A hello **és adatokat importálhat** modul, feltéve, hogy a hello kép csak példák hello rendezésére a értékekkel hello paramétereket hello értékének tooprovide kell.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-279">For hello **Import Data** module, hello values of hello parameters that are provided in hello graphic are just examples of hello sort of values you need tooprovide.</span></span> <span data-ttu-id="a3bd8-280">Íme néhány általános útmutatást ki hello paraméter toofill hogyan hello beállítani a **és adatokat importálhat** modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-280">Here is some general guidance on how toofill out hello parameter set for hello **Import Data** module.</span></span>

1. <span data-ttu-id="a3bd8-281">Válassza ki a "Hive-lekérdezések" a **adatforrás**</span><span class="sxs-lookup"><span data-stu-id="a3bd8-281">Choose "Hive query" for **Data Source**</span></span>
2. <span data-ttu-id="a3bd8-282">A hello **adatbázis-lekérdezés Hive** mezőbe egy egyszerű jelöljön ki * FROM < a\_adatbázis\_name.your\_tábla\_neve >-elegendő.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-282">In hello **Hive database query** box, a simple SELECT * FROM <your\_database\_name.your\_table\_name> - is enough.</span></span>
3. <span data-ttu-id="a3bd8-283">**Hcatalog kiszolgáló URI azonosítója**: Ha a fürt "abc", akkor ez az egyszerűen: https://abc.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="a3bd8-283">**Hcatalog server URI**: If your cluster is "abc", then this is simply: https://abc.azurehdinsight.net</span></span>
4. <span data-ttu-id="a3bd8-284">**Hadoop felhasználói fiók nevét**: hello helyreállításkor hello fürthöz beüzemelési kiválasztott hello felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-284">**Hadoop user account name**: hello user name chosen at hello time of commissioning hello cluster.</span></span> <span data-ttu-id="a3bd8-285">(Nem hello távelérési felhasználónév!)</span><span class="sxs-lookup"><span data-stu-id="a3bd8-285">(NOT hello Remote Access user name!)</span></span>
5. <span data-ttu-id="a3bd8-286">**Hadoop felhasználói fiók jelszavát**: hello helyreállításkor hello fürthöz beüzemelési kiválasztott hello felhasználónév hello jelszava.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-286">**Hadoop user account password**: hello password for hello user name chosen at hello time of commissioning hello cluster.</span></span> <span data-ttu-id="a3bd8-287">(Nem hello távelérési jelszó!)</span><span class="sxs-lookup"><span data-stu-id="a3bd8-287">(NOT hello Remote Access password!)</span></span>
6. <span data-ttu-id="a3bd8-288">**Az kimeneti adatok**: válassza az "Azure"</span><span class="sxs-lookup"><span data-stu-id="a3bd8-288">**Location of output data**: Choose "Azure"</span></span>
7. <span data-ttu-id="a3bd8-289">**Az Azure storage-fiók neve**: hello hello-fürthöz tartozó tárfiók</span><span class="sxs-lookup"><span data-stu-id="a3bd8-289">**Azure storage account name**: hello storage account associated with hello cluster</span></span>
8. <span data-ttu-id="a3bd8-290">**Azure storage-fiók kulcs**: hello hello-fürthöz tartozó hello tárfiók kulcsa.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-290">**Azure storage account key**: hello key of hello storage account associated with hello cluster.</span></span>
9. <span data-ttu-id="a3bd8-291">**Az Azure Tárolónév**: Ha hello fürtnév "abc", akkor egyszerűen "abc", ami általában.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-291">**Azure container name**: If hello cluster name is "abc", then this is simply "abc", usually.</span></span>

<span data-ttu-id="a3bd8-292">Egyszer hello **és adatokat importálhat** befejeződik (látható zöld hello osztásjelek hello modul) adatok, ezek az adatok mentése (a választott nevű) egy adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-292">Once hello **Import Data** finishes getting data (you see hello green tick on hello Module), save this data as a Dataset (with a name of your choice).</span></span> <span data-ttu-id="a3bd8-293">Mi a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-293">What this looks like:</span></span>

![Adatok importálása az adatok mentése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

<span data-ttu-id="a3bd8-295">Kattintson a jobb gombbal hello kimeneti portjára hello **és adatokat importálhat** modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-295">Right-click hello output port of hello **Import Data** module.</span></span> <span data-ttu-id="a3bd8-296">Ez azt jelzi egy **dataset elmentse** beállítás és egy **Visualize** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-296">This reveals a **Save as dataset** option and a **Visualize** option.</span></span> <span data-ttu-id="a3bd8-297">Hello **Visualize** lehetőséget, ha kattintott, jeleníti meg a hello adatok, és a jobb oldali panelen, amely akkor hasznos, ha néhány statisztikák 100 sorát.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-297">hello **Visualize** option, if clicked, displays 100 rows of hello data, along with a right panel that is useful for some summary statistics.</span></span> <span data-ttu-id="a3bd8-298">toosave adatok, egyszerűen válassza **dataset elmentse** , és kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-298">toosave data, simply select **Save as dataset** and follow instructions.</span></span>

<span data-ttu-id="a3bd8-299">tooselect mentett hello dataset használható a machine learning kísérletben, keresse meg a hello adatkészletekhez hello **keresési** mezőben hello a következő ábrán látható.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-299">tooselect hello saved dataset for use in a machine learning experiment, locate hello datasets using hello **Search** box shown in hello following figure.</span></span> <span data-ttu-id="a3bd8-300">Ezután egyszerűen ki adott hello név típus hello dataset részben, és húzza hello alakzatot dataset tooaccess hello fő panelje.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-300">Then simply type out hello name you gave hello dataset partially tooaccess it and drag hello dataset onto hello main panel.</span></span> <span data-ttu-id="a3bd8-301">Húzza azt hello fő panelen kijelöli a machine learning modellezési használatra.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-301">Dropping it onto hello main panel selects it for use in machine learning modeling.</span></span>

![Hello fő panelre Drage adatkészlet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> <span data-ttu-id="a3bd8-303">Ehhez a hello tanítási és a hello teszt adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-303">Do this for both hello train and hello test datasets.</span></span> <span data-ttu-id="a3bd8-304">Továbbá ne feledje, toouse hello adatbázis nevét és a táblaneveket, hogy a megadott, erre a célra.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-304">Also, remember toouse hello database name and table names that you gave for this purpose.</span></span> <span data-ttu-id="a3bd8-305">hello hello ábrán használt értékei kizárólag az ábra purposes.* *</span><span class="sxs-lookup"><span data-stu-id="a3bd8-305">hello values used in hello figure are solely for illustration purposes.**</span></span>
> 
> 

### <span data-ttu-id="a3bd8-306"><a name="step2"></a>2. lépés: Egy egyszerű kísérlet létrehozása az Azure Machine Learning toopredict kattintással / egyetlen kattintással</span><span class="sxs-lookup"><span data-stu-id="a3bd8-306"><a name="step2"></a> Step 2: Create a simple experiment in Azure Machine Learning toopredict clicks / no clicks</span></span>
<span data-ttu-id="a3bd8-307">Az Azure ML kísérlet így néz ki:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-307">Our Azure ML experiment looks like this:</span></span>

![Machine Learning kísérlet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

<span data-ttu-id="a3bd8-309">A Microsoft hello legfontosabb összetevők, ez a kísérlet most vizsgálja meg.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-309">We now examine hello key components of this experiment.</span></span> <span data-ttu-id="a3bd8-310">Ne feledje a mentett betanítása és adathalmazokat tooour kísérletvászonra először tesztelje toodrag kell.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-310">As a reminder, we need toodrag our saved train and test datasets on tooour experiment canvas first.</span></span>

#### <a name="clean-missing-data"></a><span data-ttu-id="a3bd8-311">Hiányzó adatok törlése</span><span class="sxs-lookup"><span data-stu-id="a3bd8-311">Clean Missing Data</span></span>
<span data-ttu-id="a3bd8-312">Hello **Clean Missing Data** modul does a neve alapján: törli a hiányzó adatokat, hogy a felhasználó által megadott lehet.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-312">hello **Clean Missing Data** module does what its name suggests:  it cleans missing data in ways that can be user-specified.</span></span> <span data-ttu-id="a3bd8-313">Ez a modul be keresése, azt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-313">Looking into this module, we see this:</span></span>

![Hiányzó adatok törlése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

<span data-ttu-id="a3bd8-315">Itt választottuk tooreplace összes hiányzó értéket 0-val.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-315">Here, we chose tooreplace all missing values with a 0.</span></span> <span data-ttu-id="a3bd8-316">Nincsenek, valamint egyéb beállításokat, amelyek hello legördülő listák megnyílásának hello modul megnézzük látható.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-316">There are other options as well, which can be seen by looking at hello dropdowns in hello module.</span></span>

#### <a name="feature-engineering-on-hello-data"></a><span data-ttu-id="a3bd8-317">Jellemzőkiemelés hello adatokon</span><span class="sxs-lookup"><span data-stu-id="a3bd8-317">Feature engineering on hello data</span></span>
<span data-ttu-id="a3bd8-318">Egyedi értékeket az egyes kategorikus szolgáltatások nagy adatkészletek több millió lehet.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-318">There can be millions of unique values for some categorical features of large datasets.</span></span> <span data-ttu-id="a3bd8-319">A natív módszerek, például egy közbeni kódolását jelző ilyen nagy dimenziós kategorikus funkciók használata teljesen amelyeknek.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-319">Using naive methods such as one-hot encoding for representing such high-dimensional categorical features is entirely unfeasible.</span></span> <span data-ttu-id="a3bd8-320">Ebben a bemutatóban bemutatjuk, hogyan toouse száma funkciók beépített Azure Machine Learning modulok toogenerate tömörítésével nagy dimenziós kategorikus változókhoz ábrázolásai.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-320">In this walkthrough, we demonstrate how toouse count features using built-in Azure Machine Learning modules toogenerate compact representations of these high-dimensional categorical variables.</span></span> <span data-ttu-id="a3bd8-321">hello end-eredménye modell mérete kisebb lesz, gyorsabb képzési és teljesítménymutatók, amelyek teljesen összehasonlítható toousing más módszereket.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-321">hello end-result is a smaller model size, faster training times, and performance metrics that are quite comparable toousing other techniques.</span></span>

##### <a name="building-counting-transforms"></a><span data-ttu-id="a3bd8-322">Leltár átalakítások felépítése</span><span class="sxs-lookup"><span data-stu-id="a3bd8-322">Building counting transforms</span></span>
<span data-ttu-id="a3bd8-323">hello használjuk toobuild száma funkciókat, **Build számbavételi átalakítási** elérhető az Azure Machine Learning modulban.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-323">toobuild count features, we use hello **Build Counting Transform** module that is available in Azure Machine Learning.</span></span> <span data-ttu-id="a3bd8-324">hello modul így néz ki:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-324">hello module looks like this:</span></span>

<span data-ttu-id="a3bd8-325">![Átalakítás számbavételi modul létrehozása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![Build számbavételi átalakító modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span><span class="sxs-lookup"><span data-stu-id="a3bd8-325">![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="a3bd8-326">A hello **oszlopok száma** mezőbe, hogy jelenítsük tooperform megjeleníti ilyen oszlopokat azt adja meg.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-326">In hello **Count columns** box, we enter those columns that we wish tooperform counts on.</span></span> <span data-ttu-id="a3bd8-327">Ezek rendszerint (említettek) nagy dimenziós kategorikus oszlopok.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-327">Typically, these are (as mentioned) high-dimensional categorical columns.</span></span> <span data-ttu-id="a3bd8-328">Hello indításkor azt említett adott hello Criteo dataset adatkészletben 26 kategorikus oszlopok: az Col15 tooCol40.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-328">At hello start, we mentioned that hello Criteo dataset has 26 categorical columns: from Col15 tooCol40.</span></span> <span data-ttu-id="a3bd8-329">Itt, hogy ezek száma és az indexek adjon (a 15 too40 látható vesszővel elválasztva).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-329">Here, we count on all of them and give their indices (from 15 too40 separated by commas as shown).</span></span>
> 

<span data-ttu-id="a3bd8-330">toouse hello moduljának hello MapReduce mód (megfelelő nagy adatkészletek), nem kell hozzáférni tooan HDInsight Hadoop-fürt (erre a célra egy szolgáltatás feltárása a felhasználhatók hello) és a hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-330">toouse hello module in hello MapReduce mode (appropriate for large datasets), we need access tooan HDInsight Hadoop cluster (hello one used for feature exploration can be reused for this purpose as well) and its credentials.</span></span> <span data-ttu-id="a3bd8-331">hello előző ábrák bemutatják, milyen hello kitöltött értékeket néz (cserélje le az azokat, a saját használati eset illusztrációs célokat hello értékek).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-331">hello  previous figures illustrate what hello filled-in values look like (replace hello values provided for illustration with those relevant for your own use-case).</span></span>

![Modul paraméterei](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

<span data-ttu-id="a3bd8-333">Hello a fenti ábrán szereplő megmutatjuk, hogyan tooenter hello bemeneti blob helyét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-333">In hello figure above, we show how tooenter hello input blob location.</span></span> <span data-ttu-id="a3bd8-334">Ezen a helyen száma táblák létrehozása fenntartott hello adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-334">This location has hello data reserved for building count tables on.</span></span>

<span data-ttu-id="a3bd8-335">Ez a modul végeztével is mentheti a hello átalakító később kattintson a jobb gombbal a hello modult, majd válassza a hello **átalakító elmentse** lehetőséget:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-335">After this module finishes running, we can save hello transform for later by right-clicking hello module and selecting hello **Save as Transform** option:</span></span>

!["Mentés másként átalakító" beállítás](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

<span data-ttu-id="a3bd8-337">A fenti kísérlet architektúra esetén hello dataset "ytransform2" pontosan száma átalakító mentett tooa felel meg.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-337">In our experiment architecture shown above, hello dataset "ytransform2" corresponds precisely tooa saved count transform.</span></span> <span data-ttu-id="a3bd8-338">Ehhez a kísérlethez hello hátralévő, feltételezzük, hogy a használt hello olvasó egy **Build számbavételi átalakítási** modul bizonyos adatok toogenerate számát, és majd is használja azokat számok toogenerate száma szolgáltatásait hello vonat, és tesztelje a adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-338">For hello remainder of this experiment, we assume that hello reader used a **Build Counting Transform** module on some data toogenerate counts, and can then use those counts toogenerate count features on hello train and test datasets.</span></span>

##### <a name="choosing-what-count-features-tooinclude-as-part-of-hello-train-and-test-datasets"></a><span data-ttu-id="a3bd8-339">Milyen funkciókat tooinclude száma hello vonat részeként és adatkészletek tesztelése kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-339">Choosing what count features tooinclude as part of hello train and test datasets</span></span>
<span data-ttu-id="a3bd8-340">Miután tudunk átalakítás kész számát, hello milyen funkciók tooinclude adja meg a tanítási és a felhasználónak adatkészletekhez hello tesztelése **száma tábla paraméterek módosítása** modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-340">Once we have a count transform ready, hello user can choose what features tooinclude in their train and test datasets using hello **Modify Count Table Parameters** module.</span></span> <span data-ttu-id="a3bd8-341">Ez a modul csak megmutatjuk Itt a teljesség, de az egyszerűség ténylegesen használja fel a kísérletben.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-341">We just show this module here for completeness, but in interests of simplicity do not actually use it in our experiment.</span></span>

![Count tábla paraméterek módosítása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

<span data-ttu-id="a3bd8-343">Ebben az esetben láthatók, mivel azt választotta, toouse csak hello napló-valószínűleg és tooignore hello biztonsági oszlop ki.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-343">In this case, as can be seen, we have chosen toouse just hello log-odds and tooignore hello back off column.</span></span> <span data-ttu-id="a3bd8-344">Azt is beállíthatja például hello szemétgyűjtési bin küszöbértéket, a program simítást, hogy hány pszeudo előzetes példák tooadd paramétereket, és hogy toouse bármely Laplacian zaj vagy nem.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-344">We can also set parameters such as hello garbage bin threshold, how many pseudo-prior examples tooadd for smoothing, and whether toouse any Laplacian noise or not.</span></span> <span data-ttu-id="a3bd8-345">A fenti speciális funkciók és toobe nincs jelezve, hogy hello alapértelmezett értékei a felhasználók számára szolgáltatás generációs típusú új toothis jó kiindulási pont.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-345">All these are advanced features and it is toobe noted that hello default values are a good starting point for users who are new toothis type of feature generation.</span></span>

##### <a name="data-transformation-before-generating-hello-count-features"></a><span data-ttu-id="a3bd8-346">Adatok átalakítása hello száma szolgáltatások létrehozása előtt</span><span class="sxs-lookup"><span data-stu-id="a3bd8-346">Data transformation before generating hello count features</span></span>
<span data-ttu-id="a3bd8-347">Most azt a vonat átalakítása kapcsolatos fontos megfontolandó összpontosít, és tesztelje a adatok előzetes tooactually száma szolgáltatások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-347">Now we focus on an important point about transforming our train and test data prior tooactually generating count features.</span></span> <span data-ttu-id="a3bd8-348">Vegye figyelembe, hogy nincsenek két **R-parancsfájl végrehajtása** előtt hello számának átalakító tooour adatai érvénybe lépni használt modulok.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-348">Note that there are two **Execute R Script** modules used before we apply hello count transform tooour data.</span></span>

![R-parancsfájl-modulokba végrehajtása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

<span data-ttu-id="a3bd8-350">Hello első R-parancsfájl a következő:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-350">Here is hello first R script:</span></span>

![Első R-parancsfájl](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

<span data-ttu-id="a3bd8-352">Az R-parancsfájl, a Microsoft nevezze át az oszlopok toonames "Oszlop1" túl "Col40".</span><span class="sxs-lookup"><span data-stu-id="a3bd8-352">In this R script, we rename our columns toonames "Col1" too"Col40".</span></span> <span data-ttu-id="a3bd8-353">Ennek az az oka hello száma átalakító vár ebben a formátumban nevét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-353">This is because hello count transform expects names of this format.</span></span>

<span data-ttu-id="a3bd8-354">Hello második R-parancsfájl, a Microsoft hello terjesztési pozitív és negatív osztályok között egyenleg (1 és 0 osztály rendre) egyszerűsítés hello negatív osztály.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-354">In hello second R script, we balance hello distribution between positive and negative classes (classes 1 and 0 respectively) by downsampling hello negative class.</span></span> <span data-ttu-id="a3bd8-355">hello R parancsfájl itt bemutatja, hogyan toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-355">hello R script here shows how toodo this:</span></span>

![Második R-parancsfájl](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

<span data-ttu-id="a3bd8-357">Ez egyszerű R-parancsfájl használjuk "pos\_Neg.\_arány" hello pozitív és negatív hello osztályok közötti egyensúly tooset hello mennyiségét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-357">In this simple R script, we use "pos\_neg\_ratio" tooset hello amount of balance between hello positive and hello negative classes.</span></span> <span data-ttu-id="a3bd8-358">Ez azért fontos, mivel osztály egyensúlyhiány javítása általában jobb teljesítményt nyújt a besorolási problémákat hello osztály terjesztési esetén toodo válik egyenetlenné (visszaírási, hogy ebben az esetben tudunk 3.3 % pozitív és negatív osztály 96.7 %).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-358">This is important toodo since improving class imbalance usually has performance benefits for classification problems where hello class distribution is skewed (recall that in our case, we have 3.3% positive class and 96.7% negative class).</span></span>

##### <a name="applying-hello-count-transformation-on-our-data"></a><span data-ttu-id="a3bd8-359">Az adatok a hello száma átalakítása alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-359">Applying hello count transformation on our data</span></span>
<span data-ttu-id="a3bd8-360">Végezetül használhatjuk hello **alkalmazása átalakítása** modul tooapply száma átalakítások a vonaton hello és adatkészletek tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-360">Finally, we can use hello **Apply Transformation** module tooapply hello count transforms on our train and test datasets.</span></span> <span data-ttu-id="a3bd8-361">Ez a modul egy bemeneti és a hello betanítása vagy adatkészletek tesztelése, egyéb beviteli hello vesz igénybe a mentett hello száma átalakító, és count szolgáltatásokkal rendelkező adatokat ad vissza.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-361">This module takes hello saved count transform as one input and hello train or test datasets as hello other input, and returns data with count features.</span></span> <span data-ttu-id="a3bd8-362">Az itt látható:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-362">It is shown here:</span></span>

![Átalakítás modul alkalmazása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-hello-count-features-look-like"></a><span data-ttu-id="a3bd8-364">Hogyan meg hello száma szolgáltatások kivonat</span><span class="sxs-lookup"><span data-stu-id="a3bd8-364">An excerpt of what hello count features look like</span></span>
<span data-ttu-id="a3bd8-365">Milyen hello száma funkciók jellegét esetünkben a fontos információkat tartalmazó toosee.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-365">It is instructive toosee what hello count features look like in our case.</span></span> <span data-ttu-id="a3bd8-366">Itt azt az a cikkből megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-366">Here we show an excerpt of this:</span></span>

![Szolgáltatások száma](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

<span data-ttu-id="a3bd8-368">A cikkből megmutatjuk, hogy hello olyan oszlopok esetében, azt a megszámlált, azt lekérése hello számát és bejelentkezés valószínűleg hozzáadása tooany vonatkozó backoffs.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-368">In this excerpt, we show that for hello columns that we counted on, we get hello counts and log odds in addition tooany relevant backoffs.</span></span>

<span data-ttu-id="a3bd8-369">Dolgozunk most már készen áll a toobuild az Azure Machine Learning modell ezen átalakított adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-369">We are now ready toobuild an Azure Machine Learning model using these transformed datasets.</span></span> <span data-ttu-id="a3bd8-370">A következő szakaszban hello megmutatjuk, hogyan ehhez.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-370">In hello next section, we show how this can be done.</span></span>

### <span data-ttu-id="a3bd8-371"><a name="step3"></a>3. lépés: Létrehozása, betanítását és pontozását hello modell.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-371"><a name="step3"></a> Step 3: Build, train, and score hello model</span></span>

#### <a name="choice-of-learner"></a><span data-ttu-id="a3bd8-372">Tanuló kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-372">Choice of learner</span></span>
<span data-ttu-id="a3bd8-373">Először igazolnia kell a tanuló toochoose.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-373">First, we need toochoose a learner.</span></span> <span data-ttu-id="a3bd8-374">Folyamatban, mint a tanuló folyamatos toouse egy két osztályú súlyozott döntési fa.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-374">We are going toouse a two class boosted decision tree as our learner.</span></span> <span data-ttu-id="a3bd8-375">Az alábbiakban a tanuló hello alapértelmezett beállítások:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-375">Here are hello default options for this learner:</span></span>

![Két osztályú súlyozott döntési fa paraméterek](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

<span data-ttu-id="a3bd8-377">A kísérleti fázisú funkciókat hogy folyamatos toochoose hello alapértelmezett értékei lesznek.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-377">For our experiment, we are going toochoose hello default values.</span></span> <span data-ttu-id="a3bd8-378">Megjegyezzük, hogy hello alapértelmezés szerint általában értelmezhető és egy jó módszer tooget gyors alaptervek a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-378">We note that hello defaults are usually meaningful and a good way tooget quick baselines on performance.</span></span> <span data-ttu-id="a3bd8-379">Abszolút paraméterek Ha úgy dönt, hogy tooonce alapterv javíthatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-379">You can improve on performance by sweeping parameters if you choose tooonce you have a baseline.</span></span>

#### <a name="train-hello-model"></a><span data-ttu-id="a3bd8-380">Hello tanítási</span><span class="sxs-lookup"><span data-stu-id="a3bd8-380">Train hello model</span></span>
<span data-ttu-id="a3bd8-381">Képzési, hogy egyszerűen meghívni egy **tanítási modell** modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-381">For training, we simply invoke a **Train Model** module.</span></span> <span data-ttu-id="a3bd8-382">hello két bemenet tooit a következők: hello két osztályú súlyozott döntési fa tanuló és az vonat adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-382">hello two inputs tooit are hello Two-Class Boosted Decision Tree learner and our train dataset.</span></span> <span data-ttu-id="a3bd8-383">Ez itt látható:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-383">This is shown here:</span></span>

![Train Model-modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-hello-model"></a><span data-ttu-id="a3bd8-385">Pontszám hello modell</span><span class="sxs-lookup"><span data-stu-id="a3bd8-385">Score hello model</span></span>
<span data-ttu-id="a3bd8-386">Miután a betanított modell tudunk, készen áll-e azt tooscore hello a DataSet adatkészlet és teszteléséhez tooevaluate a teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-386">Once we have a trained model, we are ready tooscore on hello test dataset and tooevaluate its performance.</span></span> <span data-ttu-id="a3bd8-387">Jelenleg ezt úgy teheti meg hello **Score Model** . ábrát, valamint a következő hello látható modul egy **modell kiértékelése** modul:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-387">We do this by using hello **Score Model** module shown in hello following figure, along with an **Evaluate Model** module:</span></span>

![A Score model (Modell montozása) modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <span data-ttu-id="a3bd8-389"><a name="step4"></a>4. lépés: Hello modell kiértékelése</span><span class="sxs-lookup"><span data-stu-id="a3bd8-389"><a name="step4"></a> Step 4: Evaluate hello model</span></span>
<span data-ttu-id="a3bd8-390">Végezetül tapasztalatairól tooanalyze modell teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-390">Finally, we would like tooanalyze model performance.</span></span> <span data-ttu-id="a3bd8-391">A két osztály (bináris) besorolás problémákat általában jó mérték hello AUC jelenti.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-391">Usually, for two class (binary) classification problems, a good measure is hello AUC.</span></span> <span data-ttu-id="a3bd8-392">toovisualize, azt a számítógéphez hello **Score Model** modul tooan **modell kiértékelése** a modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-392">toovisualize this, we hook up hello **Score Model** module tooan **Evaluate Model** module for this.</span></span> <span data-ttu-id="a3bd8-393">Kattintson a **Visualize** hello a **modell kiértékelése** modul például a következő egy hello képet eredményez:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-393">Clicking **Visualize** on hello **Evaluate Model** module yields a graphic like hello following one:</span></span>

![A modul táblázatos adatbázisba kerülő modell kiértékelése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

<span data-ttu-id="a3bd8-395">A bináris (vagy két osztály) besorolás problémák, az előrejelzés pontosságát jó mérték van hello terület a görbe (AUC).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-395">In binary (or two class) classification problems, a good measure of prediction accuracy is hello Area Under Curve (AUC).</span></span> <span data-ttu-id="a3bd8-396">A következő megmutatjuk, az eredményeket a tesztelési adatkészletnél ebben a modellben.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-396">In what follows, we show our results using this model on our test dataset.</span></span> <span data-ttu-id="a3bd8-397">tooget e, kattintson a jobb gombbal hello kimeneti portjára hello **modell kiértékelése** modult, majd **Visualize**.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-397">tooget this, right-click hello output port of hello **Evaluate Model** module and then **Visualize**.</span></span>

![Modell kiértékelése modul megjelenítése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <span data-ttu-id="a3bd8-399"><a name="step5"></a>5. lépés: Hello modell közzététele egy webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a3bd8-399"><a name="step5"></a> Step 5: Publish hello model as a Web service</span></span>
<span data-ttu-id="a3bd8-400">Adja meg a megfelelő széles körben elérhető értékes hello képességét toopublish webszolgáltatásként fuss legalább egy Azure Machine Learning-modell nem.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-400">hello ability toopublish an Azure Machine Learning model as web services with a minimum of fuss is a valuable feature for making it widely available.</span></span> <span data-ttu-id="a3bd8-401">Ezt követően, bárki, aki hívások toohello webszolgáltatás bemeneti adatokkal, hogy az előrejelzés van szükségük, és hello webszolgáltatás használja hello modell tooreturn e előrejelzéseket teheti meg.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-401">Once that is done, anyone can make calls toohello web service with input data that they need predictions for, and hello web service uses hello model tooreturn those predictions.</span></span>

<span data-ttu-id="a3bd8-402">toodo, először elmentjük a betanított modell Trained Model-objektumként.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-402">toodo this, we first save our trained model as a Trained Model object.</span></span> <span data-ttu-id="a3bd8-403">Ehhez kattintson a jobb gombbal a hello **tanítási modell** modul és a hello segítségével **Trained Model elmentse** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-403">This is done by right-clicking hello **Train Model** module and using hello **Save as Trained Model** option.</span></span>

<span data-ttu-id="a3bd8-404">A következő toocreate bemeneti és kimeneti kell a webszolgáltatás által használt portokat:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-404">Next, we need toocreate input and output ports for our web service:</span></span>

* <span data-ttu-id="a3bd8-405">egy bemeneti porthoz adatok az azonos alkotnak, igazolnia kell, hogy az előrejelzés hello adatként hello időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-405">an input port takes data in hello same form as hello data that we need predictions for</span></span>
* <span data-ttu-id="a3bd8-406">a kimeneti portra hello pontozott címkék és a kapcsolódó hello valószínűség adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-406">an output port returns hello Scored Labels and hello associated probabilities.</span></span>

#### <a name="select-a-few-rows-of-data-for-hello-input-port"></a><span data-ttu-id="a3bd8-407">Jelölje be néhány sornyi adatot hello bemeneti port</span><span class="sxs-lookup"><span data-stu-id="a3bd8-407">Select a few rows of data for hello input port</span></span>
<span data-ttu-id="a3bd8-408">Tetszés szerinti toouse egy **SQL-átalakítás alkalmazása** modul tooselect csak 10 sorok tooserve, hello bemeneti port adatai.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-408">It is convenient toouse an **Apply SQL Transformation** module tooselect just 10 rows tooserve as hello input port data.</span></span> <span data-ttu-id="a3bd8-409">Ezeknek az itt látható hello SQL lekérdezést használva bemeneti porthoz adatsorokat kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-409">Select just these rows of data for our input port using hello SQL query shown here:</span></span>

![Bemeneti porthoz adatok](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a><span data-ttu-id="a3bd8-411">Webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a3bd8-411">Web service</span></span>
<span data-ttu-id="a3bd8-412">Most a folyamatban van, egy kis kísérletet, amely használt toopublish toorun kész webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-412">Now we are ready toorun a small experiment that can be used toopublish our web service.</span></span>

#### <a name="generate-input-data-for-webservice"></a><span data-ttu-id="a3bd8-413">A webszolgáltatás bemeneti adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3bd8-413">Generate input data for webservice</span></span>
<span data-ttu-id="a3bd8-414">Zeroth lépésként hello száma tábla nagy, mivel azt igénybe vehet néhány sornyi Tesztadatok és hozhat létre a kimeneti adatokat, count szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-414">As a zeroth step, since hello count table is large, we take a few lines of test data and generate output data from it with count features.</span></span> <span data-ttu-id="a3bd8-415">Ez a szolgálhatnak a webservice hello bemeneti adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-415">This can serve as hello input data format for our webservice.</span></span> <span data-ttu-id="a3bd8-416">Ez itt látható:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-416">This is shown here:</span></span>

![A bemeneti adatok táblázatos adatbázisba kerülő létrehozása](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> <span data-ttu-id="a3bd8-418">A bemeneti adatok formátuma hello, most használjuk hello hello KIMENETE **száma Featurizer** modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-418">For hello input data format, we now use hello OUTPUT of hello **Count Featurizer** module.</span></span> <span data-ttu-id="a3bd8-419">Amennyiben ez kísérletezhet futtató befejeződik, mentése hello kimeneti hello **száma Featurizer** adatkészletként modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-419">Once this experiment finishes running, save hello output from hello **Count Featurizer** module as a Dataset.</span></span> <span data-ttu-id="a3bd8-420">Ez az adatkészlet hello bemeneti adatok hello webservice szolgál.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-420">This Dataset is used for hello input data in hello webservice.</span></span>
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a><span data-ttu-id="a3bd8-421">Kísérlet pontozás közzétételi webszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a3bd8-421">Scoring experiment for publishing webservice</span></span>
<span data-ttu-id="a3bd8-422">Első lépésként megmutatjuk néz ez.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-422">First, we show what this looks like.</span></span> <span data-ttu-id="a3bd8-423">hello alapvető struktúra egy **Score Model** modul, amely elfogadja a betanított modell objektum és a bemeneti adatok azt hello segítségével hello előző lépésekben létrehozott néhány sornyi **száma Featurizer** modul.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-423">hello essential structure is a **Score Model** module that accepts our trained model object and a few lines of input data that we generated in hello previous steps using hello **Count Featurizer** module.</span></span> <span data-ttu-id="a3bd8-424">"Válassza oszlopok a Dataset" tooproject hello Scored címkék és hello pontszám valószínűség használjuk.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-424">We use "Select Columns in Dataset" tooproject out hello Scored labels and hello Score probabilities.</span></span>

![Oszlopok kiválasztása az adathalmaz](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

<span data-ttu-id="a3bd8-426">Figyelje meg, hogyan hello **Select Columns in Dataset** modul "kiszűrése" adatok egy adatkészletből is használható.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-426">Notice how hello **Select Columns in Dataset** module can be used for 'filtering' data out from a dataset.</span></span> <span data-ttu-id="a3bd8-427">Megmutatjuk, itt hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-427">We show hello contents here:</span></span>

![A Dataset modul Select Columns hello szűrése](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

<span data-ttu-id="a3bd8-429">tooget hello kék bemeneti és kimeneti portok, egyszerűen kattint **készítse elő a webservice** jobb hello alján.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-429">tooget hello blue input and output ports, you simply click **prepare webservice** at hello bottom right.</span></span> <span data-ttu-id="a3bd8-430">Ez a kísérlet is lehetővé teszi a us toopublish hello webszolgáltatás: hello kattintson **WEBSZOLGÁLTATÁS közzététele** ikonra hello alsó jobb, látható itt:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-430">Running this experiment also allows us toopublish hello web service: click hello **PUBLISH WEB SERVICE** icon at hello bottom right, shown here:</span></span>

![Webszolgáltatás](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

<span data-ttu-id="a3bd8-432">Hello webservice van közzétéve, amint azt lekérése átirányított tooa lap, így a következőhöz:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-432">Once hello webservice is published, we get redirected tooa page that looks thus:</span></span>

![Webes szolgáltatás irányítópult](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

<span data-ttu-id="a3bd8-434">Két hivatkozások webservices hello bal oldalán látható:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-434">We see two links for webservices on hello left side:</span></span>

* <span data-ttu-id="a3bd8-435">Hello **kérelem/válasz** szolgáltatás (vagy RR-EKET) egyetlen előrejelzéseket készült, és mi azt használják a workshop a rendszer.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-435">hello **REQUEST/RESPONSE** Service (or RRS) is meant for single predictions and is what we utilize in this workshop.</span></span>
* <span data-ttu-id="a3bd8-436">Hello **KÖTEGELT végrehajtási** szolgáltatás (BES) szolgál a kötegelt előrejelzéseket, és megköveteli, hogy hello használt bemeneti adatok toomake előrejelzéseket találhatók az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-436">hello **BATCH EXECUTION** Service (BES) is used for batch predictions and requires that hello input data used toomake predictions reside in Azure Blob Storage.</span></span>

<span data-ttu-id="a3bd8-437">Hello hivatkozásra kattintva **kérelem/válasz** tooa vesz igénybe, olyan előre konzerv C#, python, és R. az Ezt a kódot, hogy a hívások toohello webservice kényelmesen használható.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-437">Clicking on hello link **REQUEST/RESPONSE** takes us tooa page that gives us pre-canned code in C#, python, and R. This code can be conveniently used for making calls toohello webservice.</span></span> <span data-ttu-id="a3bd8-438">Vegye figyelembe, hogy hello API-kulcs ezen az oldalon kell toobe hitelesítéshez használt.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-438">Note that hello API key on this page needs toobe used for authentication.</span></span>

<span data-ttu-id="a3bd8-439">A python-kód tooa új cellára hello IPython jegyzetfüzet kényelmes toocopy.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-439">It is convenient toocopy this python code over tooa new cell in hello IPython notebook.</span></span>

<span data-ttu-id="a3bd8-440">Itt megmutatjuk, python kódját egy szegmens hello megfelelő API-kulccsal.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-440">Here we show a segment of python code with hello correct API key.</span></span>

![Python-kód](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

<span data-ttu-id="a3bd8-442">Vegye figyelembe, hogy a Microsoft hello alapértelmezett API-kulcs helyére a problémák megoldásához segítséget az API-kulcs.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-442">Note that we replaced hello default API key with our webservices's API key.</span></span> <span data-ttu-id="a3bd8-443">Kattintson a **futtatása** ezen a cellán egy IPython a notebook adja eredményül a következő válasz hello:</span><span class="sxs-lookup"><span data-stu-id="a3bd8-443">Clicking **Run** on this cell in an IPython notebook yields hello following response:</span></span>

![IPython válasz](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

<span data-ttu-id="a3bd8-445">Látható, hogy a hello két vizsgálati példák azt ismételt (a python-parancsfájl hello hello JSON keretében), azt vissza választ kaphat hello formában "Scored valószínűség Scored címke".</span><span class="sxs-lookup"><span data-stu-id="a3bd8-445">We see that for hello two test examples we asked about (in hello JSON framework of hello python script), we get back answers in hello form "Scored Labels, Scored Probabilities".</span></span> <span data-ttu-id="a3bd8-446">Vegye figyelembe, hogy ebben az esetben választottuk hello alapértelmezett értékek hello előre előre összeállított kódot tartalmaz (0 összes numerikus oszlopok és hello karakterlánc "érték" minden kategorikus oszlopokhoz).</span><span class="sxs-lookup"><span data-stu-id="a3bd8-446">Note that in this case, we chose hello default values that hello pre-canned code provides (0's for all numeric columns and hello string "value" for all categorical columns).</span></span>

<span data-ttu-id="a3bd8-447">Ezzel befejezte a végpont forgatókönyv ábrázoló hogyan toohandle nagyméretű dataset Azure Machine Learning segítségével.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-447">This concludes our end-to-end walkthrough showing how toohandle large-scale dataset using Azure Machine Learning.</span></span> <span data-ttu-id="a3bd8-448">Azt a terabájt az adatok használatába, összeállított egy előrejelzési modellt, és telepítve lett egy webszolgáltatás hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="a3bd8-448">We started with a terabyte of data, constructed a prediction model and deployed it as a web service in hello cloud.</span></span>

