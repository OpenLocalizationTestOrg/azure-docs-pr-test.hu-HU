---
title: "Az Azure Machine Learning speciális elemzésekre forgatókönyveinek |} Microsoft Docs"
description: "Válassza ki a megfelelő forgatókönyvek speciális az Team tudományos folyamat a prediktív elemzés megteheti."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 53aecc1e-5089-42cf-8d44-77678653f92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fe4f74f2e0602d13eedb6ca186480291a9a5724f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a><span data-ttu-id="7cde7-103">Speciális elemzési forgatókönyvek az Azure Machine Learning rendszerben</span><span class="sxs-lookup"><span data-stu-id="7cde7-103">Scenarios for advanced analytics in Azure Machine Learning</span></span>
<span data-ttu-id="7cde7-104">Ez a cikk ismerteti a különböző minta adatforrások és a cél-szolgáltatásokat, amelyek kezelhetik a [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7cde7-104">This article outlines the variety of sample data sources and target scenarios that can be handled by the [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span> <span data-ttu-id="7cde7-105">A TDSP munkacsoportok számára az intelligens alkalmazások rendszeres megközelítését ismerteti.</span><span class="sxs-lookup"><span data-stu-id="7cde7-105">The TDSP provides a systematic approach for teams to collaborate on building intelligent applications.</span></span> <span data-ttu-id="7cde7-106">Az itt bemutatott esetekben bemutatják lehetőségeit adatok feldolgozása a munkafolyamatban, amely a adatjellemzők, az adatforrás helyének és a cél tárházak találhatók, az Azure-ban függ.</span><span class="sxs-lookup"><span data-stu-id="7cde7-106">The scenarios presented here illustrate options available in the data processing workflow that depend on the data characteristics, source locations, and target repositories in Azure.</span></span>

<span data-ttu-id="7cde7-107">A **döntési fa** a kiválasztása a mintaforgatókönyvek megfelelő az adatok és a cél számára jelenik meg az utolsó szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7cde7-107">The **decision tree** for selecting the sample scenarios that is appropriate for your data and objective is presented in the last section.</span></span>

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

<span data-ttu-id="7cde7-108">A következő szakaszok mindegyikének megadja egy mintaforgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="7cde7-108">Each of the following sections presents a sample scenario.</span></span> <span data-ttu-id="7cde7-109">Az egyes forgatókönyvek esetében, egy lehetséges adattudomány vagy speciális elemzésekre folyamata és a támogató Azure-erőforrások találhatók.</span><span class="sxs-lookup"><span data-stu-id="7cde7-109">For each scenario, a possible data science or advanced analytics flow and supporting Azure resources are listed.</span></span>

> [!NOTE]
> <span data-ttu-id="7cde7-110">**Az összes a következő esetekben kell:**
> </span><span class="sxs-lookup"><span data-stu-id="7cde7-110">**For all of the following scenarios, you need to:**
</span></span><br/>
> 
> * <span data-ttu-id="7cde7-111">[A storage-fiók létrehozása](../storage/common/storage-create-storage-account.md)
>   </span><span class="sxs-lookup"><span data-stu-id="7cde7-111">[Create a storage account](../storage/common/storage-create-storage-account.md)
</span></span><br/>
> * [<span data-ttu-id="7cde7-112">Az Azure Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="7cde7-112">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
> 
> 

## <span data-ttu-id="7cde7-113"><a name="smalllocal"></a>A forgatókönyv \#1: kicsi, közepes méretű táblázatos adatkészlet egy helyi fájlok</span><span class="sxs-lookup"><span data-stu-id="7cde7-113"><a name="smalllocal"></a>Scenario \#1: Small to medium tabular dataset in a local files</span></span>
![Kis-és közepes méretű helyi fájlok][1]

#### <a name="additional-azure-resources-none"></a><span data-ttu-id="7cde7-115">További Azure-erőforrások: nincs</span><span class="sxs-lookup"><span data-stu-id="7cde7-115">Additional Azure resources: None</span></span>
1. <span data-ttu-id="7cde7-116">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-116">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
2. <span data-ttu-id="7cde7-117">Töltse fel a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="7cde7-117">Upload a dataset.</span></span>
3. <span data-ttu-id="7cde7-118">Hozzon létre egy Azure Machine Learning kísérlet folyamat feltöltött adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="7cde7-118">Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).</span></span>

## <span data-ttu-id="7cde7-119"><a name="smalllocalprocess"></a>A forgatókönyv \#2: kis, közepes méretű DataSet feldolgozást igénylő helyi fájlok</span><span class="sxs-lookup"><span data-stu-id="7cde7-119"><a name="smalllocalprocess"></a>Scenario \#2: Small to medium dataset of local files that require processing</span></span>
![Kis-és közepes méretű helyi fájlok feldolgozása][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="7cde7-121">További Azure-erőforrások: Azure virtuális gép (IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-121">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="7cde7-122">Hozzon létre egy Azure virtuális gépen futó IPython Notebook.</span><span class="sxs-lookup"><span data-stu-id="7cde7-122">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="7cde7-123">Adatok feltöltése egy Azure storage-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="7cde7-123">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="7cde7-124">Előre feldolgozzák a, és az adatok elérése az Azure storage tárolóból IPython Notebook adatait.</span><span class="sxs-lookup"><span data-stu-id="7cde7-124">Pre-process and clean data in IPython Notebook, accessing data from Azure storage container.</span></span>
4. <span data-ttu-id="7cde7-125">Alakítsa át adatokat a tisztítás, táblázatos formában.</span><span class="sxs-lookup"><span data-stu-id="7cde7-125">Transform data to cleaned, tabular form.</span></span>
5. <span data-ttu-id="7cde7-126">Az Azure-blobokat átalakított adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="7cde7-126">Save transformed data in Azure blobs.</span></span>
6. <span data-ttu-id="7cde7-127">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-127">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
7. <span data-ttu-id="7cde7-128">Az adatokat olvasni az Azure-blobokat használ a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7cde7-128">Read the data from Azure blobs using the [Import Data][import-data] module.</span></span>
8. <span data-ttu-id="7cde7-129">Hozzon létre egy Azure Machine Learning kísérlet folyamat feldolgozott adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="7cde7-129">Build an Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="7cde7-130"><a name="largelocal"></a>A forgatókönyv \#3: a helyi fájloknak, Azure-Blobokkal célzó nagy adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7cde7-130"><a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs</span></span>
![Nagy helyi fájlok][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="7cde7-132">További Azure-erőforrások: Azure virtuális gép (IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-132">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="7cde7-133">Hozzon létre egy Azure virtuális gépen futó IPython Notebook.</span><span class="sxs-lookup"><span data-stu-id="7cde7-133">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="7cde7-134">Adatok feltöltése egy Azure storage-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="7cde7-134">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="7cde7-135">Előre feldolgozzák, és az adatok elérése az Azure-blobokat IPython Notebook adatait.</span><span class="sxs-lookup"><span data-stu-id="7cde7-135">Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.</span></span>
4. <span data-ttu-id="7cde7-136">Alakítsa át adatokat a tisztítás, táblázatos formában, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="7cde7-136">Transform data to cleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="7cde7-137">Adatokba, és szükség szerint funkciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-137">Explore data, and create features as needed.</span></span>
6. <span data-ttu-id="7cde7-138">Bontsa ki a kis és közepes méretű mintáját.</span><span class="sxs-lookup"><span data-stu-id="7cde7-138">Extract a small-to-medium data sample.</span></span>
7. <span data-ttu-id="7cde7-139">Mentse a mintaadatokat az Azure-blobokat.</span><span class="sxs-lookup"><span data-stu-id="7cde7-139">Save the sampled data in Azure blobs.</span></span>
8. <span data-ttu-id="7cde7-140">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-140">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="7cde7-141">Az adatokat olvasni az Azure-blobokat használ a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7cde7-141">Read the data from Azure blobs using the [Import Data][import-data] module.</span></span>
10. <span data-ttu-id="7cde7-142">Build Azure Machine Learning kísérlet folyamata feldolgozott adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="7cde7-142">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="7cde7-143"><a name="smalllocaltodb"></a>A forgatókönyv \#4: kis, közepes méretű DataSet helyi fájlok, SQL Server egy Azure virtuális gép célzó</span><span class="sxs-lookup"><span data-stu-id="7cde7-143"><a name="smalllocaltodb"></a>Scenario \#4: Small to medium dataset of local files, targeting SQL Server in an Azure Virtual Machine</span></span>
![Kis-és közepes méretű helyi fájlok az Azure SQL Adatbázishoz][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="7cde7-145">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-145">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="7cde7-146">Hozzon létre egy Azure virtuális gépen fut az SQL-kiszolgáló + IPython Notebookot.</span><span class="sxs-lookup"><span data-stu-id="7cde7-146">Create an Azure Virtual Machine running SQL Server + IPython Notebook.</span></span>
2. <span data-ttu-id="7cde7-147">Adatok feltöltése egy Azure storage-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="7cde7-147">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="7cde7-148">Előre feldolgozzák, és az Azure storage tárolóban IPython Notebook használatával adatait.</span><span class="sxs-lookup"><span data-stu-id="7cde7-148">Pre-process and clean data in Azure storage container using IPython Notebook.</span></span>
4. <span data-ttu-id="7cde7-149">Alakítsa át adatokat a tisztítás, táblázatos formában, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="7cde7-149">Transform data to cleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="7cde7-150">Adatok mentése a virtuális gép helyi fájlok (IPython Notebook a virtuális gép fut, helyi meghajtók tekintse meg a VM-meghajtók).</span><span class="sxs-lookup"><span data-stu-id="7cde7-150">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
6. <span data-ttu-id="7cde7-151">Egy Azure virtuális Gépen futó SQL Server-adatbázis az adatok betöltése.</span><span class="sxs-lookup"><span data-stu-id="7cde7-151">Load data to SQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="7cde7-152">A beállítás \#1: SQL Server Management Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="7cde7-152">Option \#1: Using SQL Server Management Studio.</span></span>
   
   * <span data-ttu-id="7cde7-153">Jelentkezzen be az SQL Server rendszerű virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="7cde7-153">Login to SQL Server VM</span></span>
   * <span data-ttu-id="7cde7-154">Futtassa az SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="7cde7-154">Run SQL Server Management Studio.</span></span>
   * <span data-ttu-id="7cde7-155">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-155">Create database and target tables.</span></span>
   * <span data-ttu-id="7cde7-156">Használja a tömeges importálásához módszerek a virtuális gép helyi fájlok az adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="7cde7-156">Use one of the bulk import methods to load the data from VM-local files.</span></span>
   
   <span data-ttu-id="7cde7-157">A beállítás \#2: használatával IPython Notebook – közepes és nagyobb adatkészletek esetében nem ajánlott</span><span class="sxs-lookup"><span data-stu-id="7cde7-157">Option \#2: Using IPython Notebook – not advisable for medium and larger datasets</span></span>
   
   <!-- -->    
   * <span data-ttu-id="7cde7-158">A virtuális gép az SQL Server eléréséhez használja az ODBC kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="7cde7-158">Use ODBC connection string to access SQL Server on VM.</span></span>
   * <span data-ttu-id="7cde7-159">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-159">Create database and target tables.</span></span>
   * <span data-ttu-id="7cde7-160">Használja a tömeges importálásához módszerek a virtuális gép helyi fájlok az adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="7cde7-160">Use one of the bulk import methods to load the data from VM-local files.</span></span>
7. <span data-ttu-id="7cde7-161">Adatokba, és szolgáltatások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="7cde7-161">Explore data, create features as needed.</span></span> <span data-ttu-id="7cde7-162">Ne feledje, hogy a szolgáltatások nem kell az adatbázis táblázatokban materializált.</span><span class="sxs-lookup"><span data-stu-id="7cde7-162">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="7cde7-163">Csak jegyezze fel a szükséges lekérdezést kell létrehoznia őket.</span><span class="sxs-lookup"><span data-stu-id="7cde7-163">Only note the necessary query to create them.</span></span>
8. <span data-ttu-id="7cde7-164">Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.</span><span class="sxs-lookup"><span data-stu-id="7cde7-164">Decide on a data sample size, if needed and/or desired.</span></span>
9. <span data-ttu-id="7cde7-165">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-165">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
10. <span data-ttu-id="7cde7-166">Közvetlenül olvassák be az adatokat az SQL Server használja a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7cde7-166">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="7cde7-167">Illessze be a szükséges lekérdezést, mely bontja ki a mezőket, szolgáltatások létrehozza, és minták adatok közvetlenül szükség esetén a [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7cde7-167">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
11. <span data-ttu-id="7cde7-168">Build Azure Machine Learning kísérlet folyamata feldolgozott adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="7cde7-168">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="7cde7-169"><a name="largelocaltodb"></a>A forgatókönyv \#5: a helyi fájlok nagy dataset cél SQL Server Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="7cde7-169"><a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM</span></span>
![Nagy helyi fájlok az Azure SQL Adatbázishoz][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="7cde7-171">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-171">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="7cde7-172">Hozzon létre egy Azure virtuális gépen futó SQL Server IPython Notebook kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="7cde7-172">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="7cde7-173">Adatok feltöltése egy Azure storage-tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="7cde7-173">Upload data to an Azure storage container.</span></span>
3. <span data-ttu-id="7cde7-174">(Választható) Előre feldolgozzák és adatait.</span><span class="sxs-lookup"><span data-stu-id="7cde7-174">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="7cde7-175">a.</span><span class="sxs-lookup"><span data-stu-id="7cde7-175">a.</span></span>  <span data-ttu-id="7cde7-176">Előre feldolgozzák és adatait IPython jegyzetfüzet adatok elérése az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="7cde7-176">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="7cde7-177">b.</span><span class="sxs-lookup"><span data-stu-id="7cde7-177">b.</span></span>  <span data-ttu-id="7cde7-178">Alakítsa át adatokat a tisztítás, táblázatos formában, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="7cde7-178">Transform data to cleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="7cde7-179">c.</span><span class="sxs-lookup"><span data-stu-id="7cde7-179">c.</span></span>  <span data-ttu-id="7cde7-180">Adatok mentése a virtuális gép helyi fájlok (IPython Notebook a virtuális gép fut, helyi meghajtók tekintse meg a VM-meghajtók).</span><span class="sxs-lookup"><span data-stu-id="7cde7-180">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
4. <span data-ttu-id="7cde7-181">Egy Azure virtuális Gépen futó SQL Server-adatbázis az adatok betöltése.</span><span class="sxs-lookup"><span data-stu-id="7cde7-181">Load data to SQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="7cde7-182">a.</span><span class="sxs-lookup"><span data-stu-id="7cde7-182">a.</span></span>  <span data-ttu-id="7cde7-183">Bejelentkezés az SQL Server rendszerű virtuális Géphez.</span><span class="sxs-lookup"><span data-stu-id="7cde7-183">Login to SQL Server VM.</span></span>
   
   <span data-ttu-id="7cde7-184">b.</span><span class="sxs-lookup"><span data-stu-id="7cde7-184">b.</span></span>  <span data-ttu-id="7cde7-185">Ha az adatok nem már mentve, adatfájlok le az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="7cde7-185">If data not saved already, download data files from Azure</span></span>
   
       storage container to local-VM folder.
   
   <span data-ttu-id="7cde7-186">c.</span><span class="sxs-lookup"><span data-stu-id="7cde7-186">c.</span></span>  <span data-ttu-id="7cde7-187">Futtassa az SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="7cde7-187">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="7cde7-188">d.</span><span class="sxs-lookup"><span data-stu-id="7cde7-188">d.</span></span>  <span data-ttu-id="7cde7-189">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-189">Create database and target tables.</span></span>
   
   <span data-ttu-id="7cde7-190">e.</span><span class="sxs-lookup"><span data-stu-id="7cde7-190">e.</span></span>  <span data-ttu-id="7cde7-191">Használja a tömeges importálásához módszerek az adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="7cde7-191">Use one of the bulk import methods to load the data.</span></span>
   
   <span data-ttu-id="7cde7-192">f.</span><span class="sxs-lookup"><span data-stu-id="7cde7-192">f.</span></span>  <span data-ttu-id="7cde7-193">Ha táblákra szükség, illesztések elősegítésére indexek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-193">If table joins are required, create indexes to expedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7cde7-194">Nagy adatmennyiség gyorsabb betöltését, javasolt, hogy a particionált táblák létrehozása, és tömeges az adatimportálás párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="7cde7-194">For faster loading of large data sizes, it is recommended that you create partitioned tables and bulk import the data in parallel.</span></span> <span data-ttu-id="7cde7-195">További információkért lásd: [párhuzamos importálhat SQL particionált táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="7cde7-195">For more information, see [Parallel Data Import to SQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="7cde7-196">Adatokba, és szolgáltatások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="7cde7-196">Explore data, create features as needed.</span></span> <span data-ttu-id="7cde7-197">Ne feledje, hogy a szolgáltatások nem kell az adatbázis táblázatokban materializált.</span><span class="sxs-lookup"><span data-stu-id="7cde7-197">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="7cde7-198">Csak jegyezze fel a szükséges lekérdezést kell létrehoznia őket.</span><span class="sxs-lookup"><span data-stu-id="7cde7-198">Only note the necessary query to create them.</span></span>
6. <span data-ttu-id="7cde7-199">Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.</span><span class="sxs-lookup"><span data-stu-id="7cde7-199">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="7cde7-200">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-200">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="7cde7-201">Közvetlenül olvassák be az adatokat az SQL Server használja a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7cde7-201">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="7cde7-202">Illessze be a szükséges lekérdezést, mely bontja ki a mezőket, szolgáltatások létrehozza, és minták adatok közvetlenül szükség esetén a [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7cde7-202">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="7cde7-203">Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdődően</span><span class="sxs-lookup"><span data-stu-id="7cde7-203">Simple Azure Machine Learning experiment flow starting with uploaded dataset</span></span>

## <span data-ttu-id="7cde7-204"><a name="largedbtodb"></a>A forgatókönyv \#6: nagy adatkészlet egy SQL Server adatbázis a helyszínen, célzás SQL Server egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="7cde7-204"><a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine</span></span>
![Nagy SQL DB a helyszínen az Azure SQL Adatbázishoz][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="7cde7-206">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-206">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="7cde7-207">Hozzon létre egy Azure virtuális gépen futó SQL Server IPython Notebook kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="7cde7-207">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="7cde7-208">Használja az adatok exportálása az adatok exportálása az SQL Server memóriaképek módszerek.</span><span class="sxs-lookup"><span data-stu-id="7cde7-208">Use one of the data export methods to export the data from SQL Server to dump files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7cde7-209">Ha úgy dönt, hogy minden adat áthelyezése a helyszíni adatbázis, a teljes adatbázis áthelyezése az Azure SQL Server-példány (gyorsabb) alternatív módszert.</span><span class="sxs-lookup"><span data-stu-id="7cde7-209">If you decide to move all data from the on-prem database, an alternate (faster) method to move the full database to the SQL Server instance in Azure.</span></span> <span data-ttu-id="7cde7-210">Hagyja ki a lépéseket, exportálhatja az adatokat hozzon létre adatbázis, és a céladatbázis adatok betöltése vagy importálása, és kövesse az alternatív módszert.</span><span class="sxs-lookup"><span data-stu-id="7cde7-210">Skip the steps to export data, create database, and load/import data to the target database and follow the alternate method.</span></span>
   > 
   > 
3. <span data-ttu-id="7cde7-211">Biztonsági másolat fájlok feltöltése az Azure storage tárolóba.</span><span class="sxs-lookup"><span data-stu-id="7cde7-211">Upload dump files to Azure storage container.</span></span>
4. <span data-ttu-id="7cde7-212">Egy Azure virtuális gépen futó SQL Server-adatbázishoz az adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="7cde7-212">Load the data to a SQL Server database running on an Azure Virtual Machine.</span></span>
   
   <span data-ttu-id="7cde7-213">a.</span><span class="sxs-lookup"><span data-stu-id="7cde7-213">a.</span></span>  <span data-ttu-id="7cde7-214">Bejelentkezés az SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="7cde7-214">Login to the SQL Server VM.</span></span>
   
   <span data-ttu-id="7cde7-215">b.</span><span class="sxs-lookup"><span data-stu-id="7cde7-215">b.</span></span>  <span data-ttu-id="7cde7-216">Adatfájlok le az Azure storage-tárolójából a helyi-VM mappába.</span><span class="sxs-lookup"><span data-stu-id="7cde7-216">Download data files from an Azure storage container to the local-VM folder.</span></span>
   
   <span data-ttu-id="7cde7-217">c.</span><span class="sxs-lookup"><span data-stu-id="7cde7-217">c.</span></span>  <span data-ttu-id="7cde7-218">Futtassa az SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="7cde7-218">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="7cde7-219">d.</span><span class="sxs-lookup"><span data-stu-id="7cde7-219">d.</span></span>  <span data-ttu-id="7cde7-220">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-220">Create database and target tables.</span></span>
   
   <span data-ttu-id="7cde7-221">e.</span><span class="sxs-lookup"><span data-stu-id="7cde7-221">e.</span></span>  <span data-ttu-id="7cde7-222">Használja a tömeges importálásához módszerek az adatok betöltésére.</span><span class="sxs-lookup"><span data-stu-id="7cde7-222">Use one of the bulk import methods to load the data.</span></span>
   
   <span data-ttu-id="7cde7-223">f.</span><span class="sxs-lookup"><span data-stu-id="7cde7-223">f.</span></span>  <span data-ttu-id="7cde7-224">Ha táblákra szükség, illesztések elősegítésére indexek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-224">If table joins are required, create indexes to expedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7cde7-225">A nagy méretű gyorsabb betöltése particionált táblák létrehozása, és tömeges adatimportálás párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="7cde7-225">For faster loading of large data sizes, create partitioned tables and to bulk import the data in parallel.</span></span> <span data-ttu-id="7cde7-226">További információkért lásd: [párhuzamos importálhat SQL particionált táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="7cde7-226">For more information, see [Parallel Data Import to SQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="7cde7-227">Adatokba, és szolgáltatások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="7cde7-227">Explore data, create features as needed.</span></span> <span data-ttu-id="7cde7-228">Ne feledje, hogy a szolgáltatások nem kell az adatbázis táblázatokban materializált.</span><span class="sxs-lookup"><span data-stu-id="7cde7-228">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="7cde7-229">Csak jegyezze fel a szükséges lekérdezést kell létrehoznia őket.</span><span class="sxs-lookup"><span data-stu-id="7cde7-229">Only note the necessary query to create them.</span></span>
6. <span data-ttu-id="7cde7-230">Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.</span><span class="sxs-lookup"><span data-stu-id="7cde7-230">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="7cde7-231">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-231">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="7cde7-232">Közvetlenül olvassák be az adatokat az SQL Server használja a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7cde7-232">Read the data directly from the SQL Server using the [Import Data][import-data] module.</span></span> <span data-ttu-id="7cde7-233">Illessze be a szükséges lekérdezést, mely bontja ki a mezőket, szolgáltatások létrehozza, és minták adatok közvetlenül szükség esetén a [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7cde7-233">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="7cde7-234">Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdve.</span><span class="sxs-lookup"><span data-stu-id="7cde7-234">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

### <a name="alternate-method-to-copy-a-full-database-from-an-on-premises--sql-server-to-azure-sql-database"></a><span data-ttu-id="7cde7-235">Alternatív módszert egy helyi SQL Server az Azure SQL Database egy teljes adatbázis másolása</span><span class="sxs-lookup"><span data-stu-id="7cde7-235">Alternate method to copy a full database from an on-premises  SQL Server to Azure SQL Database</span></span>
![Válassza le a helyi adatbázis és az Azure SQL-adatbázis csatlakoztatása][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="7cde7-237">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-237">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
<span data-ttu-id="7cde7-238">A teljes SQL Server-adatbázist az SQL Server virtuális gépen replikálni, másolja egy adatbázis egy hálózatihely-kiszolgáló a a másikra, feltéve, hogy az adatbázis átmenetileg offline állapotú lehet tenni.</span><span class="sxs-lookup"><span data-stu-id="7cde7-238">To replicate the entire SQL Server database in your SQL Server VM, you should copy a database from one location/server to another, assuming that the database can be taken temporarily offline.</span></span> <span data-ttu-id="7cde7-239">Ehhez az SQL Server Management Studio Object Explorer vagy a megfelelő Transact-SQL-parancsok használatával.</span><span class="sxs-lookup"><span data-stu-id="7cde7-239">You do this in the SQL Server Management Studio Object Explorer, or using the equivalent Transact-SQL commands.</span></span>

1. <span data-ttu-id="7cde7-240">Válassza le a forráshely adatbázisához.</span><span class="sxs-lookup"><span data-stu-id="7cde7-240">Detach the database at the source location.</span></span> <span data-ttu-id="7cde7-241">További információkért lásd: [egy adatbázis leválasztásához](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="7cde7-241">For more information, see [Detach a database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span></span>
2. <span data-ttu-id="7cde7-242">A Windows Explorer vagy a Windows parancssori ablakban másolja a adatbázisát fájl vagy fájlokat és a naplófájl vagy naplófájlok az SQL Server virtuális gépen, az Azure-ban a célhelyre.</span><span class="sxs-lookup"><span data-stu-id="7cde7-242">In Windows Explorer or Windows Command Prompt window, copy the detached database file or files and log file or files to the target location on the SQL Server VM in Azure.</span></span>
3. <span data-ttu-id="7cde7-243">A másolt fájlok csatolása a célként megadott SQL Server-példány.</span><span class="sxs-lookup"><span data-stu-id="7cde7-243">Attach the copied files to the target SQL Server instance.</span></span> <span data-ttu-id="7cde7-244">További információkért lásd: [adatbázis csatolása](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="7cde7-244">For more information, see [Attach a Database](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span></span>

<span data-ttu-id="7cde7-245">[Helyezze át egy adatbázis használatával válassza le, és csatolja (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span><span class="sxs-lookup"><span data-stu-id="7cde7-245">[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span></span>

## <span data-ttu-id="7cde7-246"><a name="largedbtohive"></a>A forgatókönyv \#7: helyi fájlok Big Data típusú adatok cél Hive-adatbázisban az Azure HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="7cde7-246"><a name="largedbtohive"></a>Scenario \#7: Big data in local files, target Hive database in Azure HDInsight Hadoop clusters</span></span>
![A helyi cél Hive big Data típusú adatok][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="7cde7-248">További Azure-erőforrások: az Azure HDInsight Hadoop-fürt és az Azure virtuális gép (IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="7cde7-248">Additional Azure resources: Azure HDInsight Hadoop Cluster and Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="7cde7-249">Hozzon létre egy IPython Notebook Servert futtató Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="7cde7-249">Create an Azure Virtual Machine running IPython Notebook server.</span></span>
2. <span data-ttu-id="7cde7-250">Hozzon létre egy Azure HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="7cde7-250">Create an Azure HDInsight Hadoop cluster.</span></span>
3. <span data-ttu-id="7cde7-251">(Választható) Előre feldolgozzák és adatait.</span><span class="sxs-lookup"><span data-stu-id="7cde7-251">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="7cde7-252">a.</span><span class="sxs-lookup"><span data-stu-id="7cde7-252">a.</span></span>  <span data-ttu-id="7cde7-253">Előre feldolgozzák és adatait IPython jegyzetfüzet adatok elérése az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="7cde7-253">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="7cde7-254">b.</span><span class="sxs-lookup"><span data-stu-id="7cde7-254">b.</span></span>  <span data-ttu-id="7cde7-255">Alakítsa át adatokat a tisztítás, táblázatos formában, szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="7cde7-255">Transform data to cleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="7cde7-256">c.</span><span class="sxs-lookup"><span data-stu-id="7cde7-256">c.</span></span>  <span data-ttu-id="7cde7-257">Adatok mentése a virtuális gép helyi fájlok (IPython Notebook a virtuális gép fut, helyi meghajtók tekintse meg a VM-meghajtók).</span><span class="sxs-lookup"><span data-stu-id="7cde7-257">Save data to VM-local files (IPython Notebook is running on VM, local drives refer to VM drives).</span></span>
4. <span data-ttu-id="7cde7-258">Az alapértelmezett tároló, a Hadoop-fürt a 2. lépésben kiválasztott feltölteni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="7cde7-258">Upload data to the default container of the Hadoop cluster selected in the step 2.</span></span>
5. <span data-ttu-id="7cde7-259">Az Azure HDInsight Hadoop-fürt Hive database adatok betöltése.</span><span class="sxs-lookup"><span data-stu-id="7cde7-259">Load data to Hive database in Azure HDInsight Hadoop cluster.</span></span>
   
   <span data-ttu-id="7cde7-260">a.</span><span class="sxs-lookup"><span data-stu-id="7cde7-260">a.</span></span>  <span data-ttu-id="7cde7-261">Jelentkezzen be a Hadoop-fürt átjárócsomópontjához</span><span class="sxs-lookup"><span data-stu-id="7cde7-261">Log in to the head node of the Hadoop cluster</span></span>
   
   <span data-ttu-id="7cde7-262">b.</span><span class="sxs-lookup"><span data-stu-id="7cde7-262">b.</span></span>  <span data-ttu-id="7cde7-263">Nyissa meg a Hadoop parancssort.</span><span class="sxs-lookup"><span data-stu-id="7cde7-263">Open the Hadoop Command Line.</span></span>
   
   <span data-ttu-id="7cde7-264">c.</span><span class="sxs-lookup"><span data-stu-id="7cde7-264">c.</span></span>  <span data-ttu-id="7cde7-265">Adja meg a Hive gyökérkönyvtár parancs `cd %hive_home%\bin` Hadoop parancssor futtatása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-265">Enter the Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="7cde7-266">d.</span><span class="sxs-lookup"><span data-stu-id="7cde7-266">d.</span></span>  <span data-ttu-id="7cde7-267">Adatbázis és a táblák létrehozásához a Hive-lekérdezések futtatása, és az adatok betöltése az blob storage Hive táblákat.</span><span class="sxs-lookup"><span data-stu-id="7cde7-267">Run the Hive queries to create database and tables, and load data from blob storage to Hive tables.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7cde7-268">Ha az adatok nagy, a felhasználók létrehozhatják a Hive tábla partíciókat.</span><span class="sxs-lookup"><span data-stu-id="7cde7-268">If the data is big, users can create the Hive table with partitions.</span></span> <span data-ttu-id="7cde7-269">Ezt követően a felhasználók használhatják a `for` hurok a Hadoop parancssori az átjárócsomópont az adatok betöltése a Hive tábla particionálva partíció által az.</span><span class="sxs-lookup"><span data-stu-id="7cde7-269">Then, users can use a `for` loop in the Hadoop Command Line on the head node to load data into the Hive table partitioned by partition.</span></span>
   > 
   > 
6. <span data-ttu-id="7cde7-270">Adatokba, és szükség esetén a Hadoop parancssori funkciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-270">Explore data and create features as needed in Hadoop Command Line.</span></span> <span data-ttu-id="7cde7-271">Ne feledje, hogy a szolgáltatások nem kell az adatbázis táblázatokban materializált.</span><span class="sxs-lookup"><span data-stu-id="7cde7-271">Note that the features do not need to be materialized in the database tables.</span></span> <span data-ttu-id="7cde7-272">Csak jegyezze fel a szükséges lekérdezést kell létrehoznia őket.</span><span class="sxs-lookup"><span data-stu-id="7cde7-272">Only note the necessary query to create them.</span></span>
   
   <span data-ttu-id="7cde7-273">a.</span><span class="sxs-lookup"><span data-stu-id="7cde7-273">a.</span></span>  <span data-ttu-id="7cde7-274">Jelentkezzen be a Hadoop-fürt átjárócsomópontjához</span><span class="sxs-lookup"><span data-stu-id="7cde7-274">Log in to the head node of the Hadoop cluster</span></span>
   
   <span data-ttu-id="7cde7-275">b.</span><span class="sxs-lookup"><span data-stu-id="7cde7-275">b.</span></span>  <span data-ttu-id="7cde7-276">Nyissa meg a Hadoop parancssort.</span><span class="sxs-lookup"><span data-stu-id="7cde7-276">Open the Hadoop Command Line.</span></span>
   
   <span data-ttu-id="7cde7-277">c.</span><span class="sxs-lookup"><span data-stu-id="7cde7-277">c.</span></span>  <span data-ttu-id="7cde7-278">Adja meg a Hive gyökérkönyvtár parancs `cd %hive_home%\bin` Hadoop parancssor futtatása.</span><span class="sxs-lookup"><span data-stu-id="7cde7-278">Enter the Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="7cde7-279">d.</span><span class="sxs-lookup"><span data-stu-id="7cde7-279">d.</span></span>  <span data-ttu-id="7cde7-280">A Hadoop parancssor az a Hive-lekérdezések futtatása az adatokba, és szükség szerint funkciók létrehozása a Hadoop-fürt központi csomópontján.</span><span class="sxs-lookup"><span data-stu-id="7cde7-280">Run the Hive queries in Hadoop Command Line on the head node of the Hadoop cluster to explore the data and create features as needed.</span></span>
7. <span data-ttu-id="7cde7-281">Ha szükséges, és/vagy szükséges, példa a az adatok az Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="7cde7-281">If needed and/or desired, sample the data to fit in Azure Machine Learning Studio.</span></span>
8. <span data-ttu-id="7cde7-282">Jelentkezzen be a [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="7cde7-282">Sign in to the [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="7cde7-283">Olvassa el az adatok közvetlenül a `Hive Queries` használatával a [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="7cde7-283">Read the data directly from the `Hive Queries` using the [Import Data][import-data] module.</span></span> <span data-ttu-id="7cde7-284">Illessze be a szükséges lekérdezést, mely bontja ki a mezőket, szolgáltatások létrehozza, és minták adatok közvetlenül szükség esetén a [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="7cde7-284">Paste the necessary query which extracts fields, creates features, and samples data if needed directly in the [Import Data][import-data] query.</span></span>
10. <span data-ttu-id="7cde7-285">Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdve.</span><span class="sxs-lookup"><span data-stu-id="7cde7-285">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

## <span data-ttu-id="7cde7-286"><a name="decisiontree"></a>A forgatókönyv kiválasztása döntési fája</span><span class="sxs-lookup"><span data-stu-id="7cde7-286"><a name="decisiontree"></a>Decision tree for scenario selection</span></span>
- - -
<span data-ttu-id="7cde7-287">Az alábbi ábra a fent leírt forgatókönyvek és a speciális Analytics folyamat és a technológia választások kattintva az egyes részletezett forgatókönyv foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="7cde7-287">The following diagram summarizes the scenarios described above and the Advanced Analytics Process and Technology choices made that take you to each of the itemized scenarios.</span></span> <span data-ttu-id="7cde7-288">Vegye figyelembe, hogy az adatok feldolgozása, a feltárása, a szolgáltatás műszaki osztály és a mintavételi is igénybe vehet egy vagy több módszer/környezet – a forrás, a köztes, és/vagy a cél környezetekben – helyezze, és szükség szerint ismételt folytathatja.</span><span class="sxs-lookup"><span data-stu-id="7cde7-288">Note that data processing, exploration, feature engineering, and sampling may take place in one or more method/environment -- at the source, intermediate, and/or target environments – and may proceed iteratively as needed.</span></span> <span data-ttu-id="7cde7-289">A diagram csak néhány lehetséges adatfolyamok szemléltetésére szolgál, és nem biztosít teljes körű enumerálást.</span><span class="sxs-lookup"><span data-stu-id="7cde7-289">The diagram only serves as an illustration of some of possible flows and does not provide an exhaustive enumeration.</span></span>

![A minta DS folyamat bemutató forgatókönyvek][8]

### <a name="advanced-analytics-in-action-examples"></a><span data-ttu-id="7cde7-291">Speciális elemzés a művelet példák</span><span class="sxs-lookup"><span data-stu-id="7cde7-291">Advanced Analytics in action Examples</span></span>
<span data-ttu-id="7cde7-292">Végpontok közötti Azure Machine Learning forgatókönyvek alkalmazó szoftverbiztonsági a Advanced Analytics folyamat és a nyilvános adatkészleteket használó technológia lásd:</span><span class="sxs-lookup"><span data-stu-id="7cde7-292">For end-to-end Azure Machine Learning walkthroughs that employ the Advanced Analytics Process and Technology using public datasets, see:</span></span>

* <span data-ttu-id="7cde7-293">[Vonja össze az adatokat tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="7cde7-293">[Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>
* <span data-ttu-id="7cde7-294">[Vonja össze az adatokat tudományos folyamat működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="7cde7-294">[Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-small-in-aml.png
[2]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-with-processing.png
[3]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-large.png
[4]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-db.png
[5]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-large-to-db.png
[6]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-db-to-db.png
[7]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-attach-db.png
[8]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-sample-scenarios.png
[9]: ./media/machine-learning-data-science-plan-sample-scenarios/dsp-plan-local-to-hive.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
