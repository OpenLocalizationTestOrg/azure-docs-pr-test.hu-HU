---
title: "Speciális elemzés forgatókönyvek az Azure Machine Learning aaaIdentify |} Microsoft Docs"
description: "Válassza ki a megfelelő forgatókönyvek hello speciális hello csapat az tudományos folyamata a prediktív elemzés megteheti."
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
ms.openlocfilehash: 52c6bb10d6df4f640a4f66cf17cf4993cc1067b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scenarios-for-advanced-analytics-in-azure-machine-learning"></a><span data-ttu-id="865c6-103">Speciális elemzési forgatókönyvek az Azure Machine Learning rendszerben</span><span class="sxs-lookup"><span data-stu-id="865c6-103">Scenarios for advanced analytics in Azure Machine Learning</span></span>
<span data-ttu-id="865c6-104">Ez a cikk ismerteti a minta adatforrások és a cél-szolgáltatásokat, amelyek kezelhetik hello hello számos [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="865c6-104">This article outlines hello variety of sample data sources and target scenarios that can be handled by hello [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span> <span data-ttu-id="865c6-105">hello TDSP rendszeres megközelítését ismerteti a csapatok toocollaborate az intelligens alkalmazások létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="865c6-105">hello TDSP provides a systematic approach for teams toocollaborate on building intelligent applications.</span></span> <span data-ttu-id="865c6-106">Itt bemutatott hello forgatókönyvek bemutatására használható lehetőségekről hello adatfeldolgozási munkafolyamat szerelvényfájljaitól hello adatjellemzők, a Forráshelyek és a cél tárházak találhatók, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="865c6-106">hello scenarios presented here illustrate options available in hello data processing workflow that depend on hello data characteristics, source locations, and target repositories in Azure.</span></span>

<span data-ttu-id="865c6-107">Hello **döntési fa** a hello szituáció, amely megfelelő-e az adatok és a cél kiválasztása számára jelenik meg a hello utolsó szakaszában.</span><span class="sxs-lookup"><span data-stu-id="865c6-107">hello **decision tree** for selecting hello sample scenarios that is appropriate for your data and objective is presented in hello last section.</span></span>

> [!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
> 
> 

<span data-ttu-id="865c6-108">A következő szakaszok hello mindegyikét megadja egy mintaforgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="865c6-108">Each of hello following sections presents a sample scenario.</span></span> <span data-ttu-id="865c6-109">Az egyes forgatókönyvek esetében, egy lehetséges adattudomány vagy speciális elemzésekre folyamata és a támogató Azure-erőforrások találhatók.</span><span class="sxs-lookup"><span data-stu-id="865c6-109">For each scenario, a possible data science or advanced analytics flow and supporting Azure resources are listed.</span></span>

> [!NOTE]
> <span data-ttu-id="865c6-110">**Az összes hello a következő esetekben kell:**
> </span><span class="sxs-lookup"><span data-stu-id="865c6-110">**For all of hello following scenarios, you need to:**
</span></span><br/>
> 
> * <span data-ttu-id="865c6-111">[A storage-fiók létrehozása](../storage/common/storage-create-storage-account.md)
>   </span><span class="sxs-lookup"><span data-stu-id="865c6-111">[Create a storage account](../storage/common/storage-create-storage-account.md)
</span></span><br/>
> * [<span data-ttu-id="865c6-112">Az Azure Machine Learning-munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="865c6-112">Create an Azure Machine Learning workspace</span></span>](machine-learning-create-workspace.md)
> 
> 

## <span data-ttu-id="865c6-113"><a name="smalllocal"></a>A forgatókönyv \#1: kicsi toomedium táblázatos adatkészlet egy helyi fájlok</span><span class="sxs-lookup"><span data-stu-id="865c6-113"><a name="smalllocal"></a>Scenario \#1: Small toomedium tabular dataset in a local files</span></span>
![Kis toomedium helyi fájlok][1]

#### <a name="additional-azure-resources-none"></a><span data-ttu-id="865c6-115">További Azure-erőforrások: nincs</span><span class="sxs-lookup"><span data-stu-id="865c6-115">Additional Azure resources: None</span></span>
1. <span data-ttu-id="865c6-116">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-116">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
2. <span data-ttu-id="865c6-117">Töltse fel a DataSet adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="865c6-117">Upload a dataset.</span></span>
3. <span data-ttu-id="865c6-118">Hozzon létre egy Azure Machine Learning kísérlet folyamat feltöltött adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="865c6-118">Build an Azure Machine Learning experiment flow starting with uploaded dataset(s).</span></span>

## <span data-ttu-id="865c6-119"><a name="smalllocalprocess"></a>A forgatókönyv \#2: a helyi fájlok feldolgozást igénylő kis toomedium adatkészlet</span><span class="sxs-lookup"><span data-stu-id="865c6-119"><a name="smalllocalprocess"></a>Scenario \#2: Small toomedium dataset of local files that require processing</span></span>
![Kis toomedium helyi fájlok feldolgozása][2]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="865c6-121">További Azure-erőforrások: Azure virtuális gép (IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-121">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="865c6-122">Hozzon létre egy Azure virtuális gépen futó IPython Notebook.</span><span class="sxs-lookup"><span data-stu-id="865c6-122">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="865c6-123">Töltse fel az adatok tooan az Azure storage-tároló.</span><span class="sxs-lookup"><span data-stu-id="865c6-123">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="865c6-124">Előre feldolgozzák a, és az adatok elérése az Azure storage tárolóból IPython Notebook adatait.</span><span class="sxs-lookup"><span data-stu-id="865c6-124">Pre-process and clean data in IPython Notebook, accessing data from Azure storage container.</span></span>
4. <span data-ttu-id="865c6-125">Alakítsa át az adatokat toocleaned, táblázatos formában.</span><span class="sxs-lookup"><span data-stu-id="865c6-125">Transform data toocleaned, tabular form.</span></span>
5. <span data-ttu-id="865c6-126">Az Azure-blobokat átalakított adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="865c6-126">Save transformed data in Azure blobs.</span></span>
6. <span data-ttu-id="865c6-127">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-127">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
7. <span data-ttu-id="865c6-128">Hello adatokat olvasni az Azure BLOB hello segítségével [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="865c6-128">Read hello data from Azure blobs using hello [Import Data][import-data] module.</span></span>
8. <span data-ttu-id="865c6-129">Hozzon létre egy Azure Machine Learning kísérlet folyamat feldolgozott adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="865c6-129">Build an Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="865c6-130"><a name="largelocal"></a>A forgatókönyv \#3: a helyi fájloknak, Azure-Blobokkal célzó nagy adatkészlet</span><span class="sxs-lookup"><span data-stu-id="865c6-130"><a name="largelocal"></a>Scenario \#3: Large dataset of local files, targeting Azure Blobs</span></span>
![Nagy helyi fájlok][3]

#### <a name="additional-azure-resources-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="865c6-132">További Azure-erőforrások: Azure virtuális gép (IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-132">Additional Azure resources: Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="865c6-133">Hozzon létre egy Azure virtuális gépen futó IPython Notebook.</span><span class="sxs-lookup"><span data-stu-id="865c6-133">Create an Azure Virtual Machine running IPython Notebook.</span></span>
2. <span data-ttu-id="865c6-134">Töltse fel az adatok tooan az Azure storage-tároló.</span><span class="sxs-lookup"><span data-stu-id="865c6-134">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="865c6-135">Előre feldolgozzák, és az adatok elérése az Azure-blobokat IPython Notebook adatait.</span><span class="sxs-lookup"><span data-stu-id="865c6-135">Pre-process and clean data in IPython Notebook, accessing data from Azure blobs.</span></span>
4. <span data-ttu-id="865c6-136">Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.</span><span class="sxs-lookup"><span data-stu-id="865c6-136">Transform data toocleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="865c6-137">Adatokba, és szükség szerint funkciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-137">Explore data, and create features as needed.</span></span>
6. <span data-ttu-id="865c6-138">Bontsa ki a kis és közepes méretű mintáját.</span><span class="sxs-lookup"><span data-stu-id="865c6-138">Extract a small-to-medium data sample.</span></span>
7. <span data-ttu-id="865c6-139">Az Azure-blobokat mintát hello adatok mentése.</span><span class="sxs-lookup"><span data-stu-id="865c6-139">Save hello sampled data in Azure blobs.</span></span>
8. <span data-ttu-id="865c6-140">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-140">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="865c6-141">Hello adatokat olvasni az Azure BLOB hello segítségével [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="865c6-141">Read hello data from Azure blobs using hello [Import Data][import-data] module.</span></span>
10. <span data-ttu-id="865c6-142">Build Azure Machine Learning kísérlet folyamata feldolgozott adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="865c6-142">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="865c6-143"><a name="smalllocaltodb"></a>A forgatókönyv \#4: a helyi fájlok, SQL Server egy Azure virtuális gép célzó kis toomedium adatkészlet</span><span class="sxs-lookup"><span data-stu-id="865c6-143"><a name="smalllocaltodb"></a>Scenario \#4: Small toomedium dataset of local files, targeting SQL Server in an Azure Virtual Machine</span></span>
![Kis toomedium helyi fájlok tooSQL DB az Azure-ban][4]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="865c6-145">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-145">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="865c6-146">Hozzon létre egy Azure virtuális gépen fut az SQL-kiszolgáló + IPython Notebookot.</span><span class="sxs-lookup"><span data-stu-id="865c6-146">Create an Azure Virtual Machine running SQL Server + IPython Notebook.</span></span>
2. <span data-ttu-id="865c6-147">Töltse fel az adatok tooan az Azure storage-tároló.</span><span class="sxs-lookup"><span data-stu-id="865c6-147">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="865c6-148">Előre feldolgozzák, és az Azure storage tárolóban IPython Notebook használatával adatait.</span><span class="sxs-lookup"><span data-stu-id="865c6-148">Pre-process and clean data in Azure storage container using IPython Notebook.</span></span>
4. <span data-ttu-id="865c6-149">Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.</span><span class="sxs-lookup"><span data-stu-id="865c6-149">Transform data toocleaned, tabular form, if needed.</span></span>
5. <span data-ttu-id="865c6-150">Adatok tooVM helyi fájlokat (IPython Notebook fut a virtuális gép tudnivalókat a helyi meghajtókra tooVM meghajtókat).</span><span class="sxs-lookup"><span data-stu-id="865c6-150">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
6. <span data-ttu-id="865c6-151">Terheléselosztási adatok tooSQL Server-adatbázis egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="865c6-151">Load data tooSQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="865c6-152">A beállítás \#1: SQL Server Management Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="865c6-152">Option \#1: Using SQL Server Management Studio.</span></span>
   
   * <span data-ttu-id="865c6-153">Bejelentkezési tooSQL kiszolgálói virtuális gép</span><span class="sxs-lookup"><span data-stu-id="865c6-153">Login tooSQL Server VM</span></span>
   * <span data-ttu-id="865c6-154">Futtassa az SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="865c6-154">Run SQL Server Management Studio.</span></span>
   * <span data-ttu-id="865c6-155">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-155">Create database and target tables.</span></span>
   * <span data-ttu-id="865c6-156">Hello tömeges egyikét módszerek tooload hello adatok importálása a virtuális gép helyi fájlokból.</span><span class="sxs-lookup"><span data-stu-id="865c6-156">Use one of hello bulk import methods tooload hello data from VM-local files.</span></span>
   
   <span data-ttu-id="865c6-157">A beállítás \#2: használatával IPython Notebook – közepes és nagyobb adatkészletek esetében nem ajánlott</span><span class="sxs-lookup"><span data-stu-id="865c6-157">Option \#2: Using IPython Notebook – not advisable for medium and larger datasets</span></span>
   
   <!-- -->    
   * <span data-ttu-id="865c6-158">ODBC kapcsolati karakterlánc tooaccess SQL Server használata virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="865c6-158">Use ODBC connection string tooaccess SQL Server on VM.</span></span>
   * <span data-ttu-id="865c6-159">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-159">Create database and target tables.</span></span>
   * <span data-ttu-id="865c6-160">Hello tömeges egyikét módszerek tooload hello adatok importálása a virtuális gép helyi fájlokból.</span><span class="sxs-lookup"><span data-stu-id="865c6-160">Use one of hello bulk import methods tooload hello data from VM-local files.</span></span>
7. <span data-ttu-id="865c6-161">Adatokba, és szolgáltatások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="865c6-161">Explore data, create features as needed.</span></span> <span data-ttu-id="865c6-162">Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe.</span><span class="sxs-lookup"><span data-stu-id="865c6-162">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="865c6-163">Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.</span><span class="sxs-lookup"><span data-stu-id="865c6-163">Only note hello necessary query toocreate them.</span></span>
8. <span data-ttu-id="865c6-164">Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.</span><span class="sxs-lookup"><span data-stu-id="865c6-164">Decide on a data sample size, if needed and/or desired.</span></span>
9. <span data-ttu-id="865c6-165">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-165">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
10. <span data-ttu-id="865c6-166">Olvasási hello adatok közvetlenül a hello hello használata az SQL Server [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="865c6-166">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="865c6-167">Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="865c6-167">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
11. <span data-ttu-id="865c6-168">Build Azure Machine Learning kísérlet folyamata feldolgozott adatkészlet(ek) kezdve.</span><span class="sxs-lookup"><span data-stu-id="865c6-168">Build Azure Machine Learning experiment flow starting with ingested dataset(s).</span></span>

## <span data-ttu-id="865c6-169"><a name="largelocaltodb"></a>A forgatókönyv \#5: a helyi fájlok nagy dataset cél SQL Server Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="865c6-169"><a name="largelocaltodb"></a>Scenario \#5: Large dataset in a local files, target SQL Server in Azure VM</span></span>
![Nagy helyi fájlok tooSQL DB az Azure-ban][5]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="865c6-171">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-171">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="865c6-172">Hozzon létre egy Azure virtuális gépen futó SQL Server IPython Notebook kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="865c6-172">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="865c6-173">Töltse fel az adatok tooan az Azure storage-tároló.</span><span class="sxs-lookup"><span data-stu-id="865c6-173">Upload data tooan Azure storage container.</span></span>
3. <span data-ttu-id="865c6-174">(Választható) Előre feldolgozzák és adatait.</span><span class="sxs-lookup"><span data-stu-id="865c6-174">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="865c6-175">a.</span><span class="sxs-lookup"><span data-stu-id="865c6-175">a.</span></span>  <span data-ttu-id="865c6-176">Előre feldolgozzák és adatait IPython jegyzetfüzet adatok elérése az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="865c6-176">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="865c6-177">b.</span><span class="sxs-lookup"><span data-stu-id="865c6-177">b.</span></span>  <span data-ttu-id="865c6-178">Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.</span><span class="sxs-lookup"><span data-stu-id="865c6-178">Transform data toocleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="865c6-179">c.</span><span class="sxs-lookup"><span data-stu-id="865c6-179">c.</span></span>  <span data-ttu-id="865c6-180">Adatok tooVM helyi fájlokat (IPython Notebook fut a virtuális gép tudnivalókat a helyi meghajtókra tooVM meghajtókat).</span><span class="sxs-lookup"><span data-stu-id="865c6-180">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
4. <span data-ttu-id="865c6-181">Terheléselosztási adatok tooSQL Server-adatbázis egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="865c6-181">Load data tooSQL Server database running on an Azure VM.</span></span>
   
   <span data-ttu-id="865c6-182">a.</span><span class="sxs-lookup"><span data-stu-id="865c6-182">a.</span></span>  <span data-ttu-id="865c6-183">Bejelentkezési tooSQL kiszolgálói virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="865c6-183">Login tooSQL Server VM.</span></span>
   
   <span data-ttu-id="865c6-184">b.</span><span class="sxs-lookup"><span data-stu-id="865c6-184">b.</span></span>  <span data-ttu-id="865c6-185">Ha az adatok nem már mentve, adatfájlok le az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="865c6-185">If data not saved already, download data files from Azure</span></span>
   
       storage container toolocal-VM folder.
   
   <span data-ttu-id="865c6-186">c.</span><span class="sxs-lookup"><span data-stu-id="865c6-186">c.</span></span>  <span data-ttu-id="865c6-187">Futtassa az SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="865c6-187">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="865c6-188">d.</span><span class="sxs-lookup"><span data-stu-id="865c6-188">d.</span></span>  <span data-ttu-id="865c6-189">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-189">Create database and target tables.</span></span>
   
   <span data-ttu-id="865c6-190">e.</span><span class="sxs-lookup"><span data-stu-id="865c6-190">e.</span></span>  <span data-ttu-id="865c6-191">Hello tömeges egyikét módszerek tooload hello adatok importálása.</span><span class="sxs-lookup"><span data-stu-id="865c6-191">Use one of hello bulk import methods tooload hello data.</span></span>
   
   <span data-ttu-id="865c6-192">f.</span><span class="sxs-lookup"><span data-stu-id="865c6-192">f.</span></span>  <span data-ttu-id="865c6-193">Ha táblákra szükség, hozzon létre indexek tooexpedite illesztéseket.</span><span class="sxs-lookup"><span data-stu-id="865c6-193">If table joins are required, create indexes tooexpedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="865c6-194">Nagy adatmennyiség gyorsabb betöltését, ajánlott particionált táblák létrehozása, és tömeges hello adatokat importálhat párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="865c6-194">For faster loading of large data sizes, it is recommended that you create partitioned tables and bulk import hello data in parallel.</span></span> <span data-ttu-id="865c6-195">További információkért lásd: [párhuzamos importálhat tooSQL particionált táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="865c6-195">For more information, see [Parallel Data Import tooSQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="865c6-196">Adatokba, és szolgáltatások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="865c6-196">Explore data, create features as needed.</span></span> <span data-ttu-id="865c6-197">Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe.</span><span class="sxs-lookup"><span data-stu-id="865c6-197">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="865c6-198">Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.</span><span class="sxs-lookup"><span data-stu-id="865c6-198">Only note hello necessary query toocreate them.</span></span>
6. <span data-ttu-id="865c6-199">Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.</span><span class="sxs-lookup"><span data-stu-id="865c6-199">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="865c6-200">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-200">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="865c6-201">Olvasási hello adatok közvetlenül a hello hello használata az SQL Server [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="865c6-201">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="865c6-202">Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="865c6-202">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="865c6-203">Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdődően</span><span class="sxs-lookup"><span data-stu-id="865c6-203">Simple Azure Machine Learning experiment flow starting with uploaded dataset</span></span>

## <span data-ttu-id="865c6-204"><a name="largedbtodb"></a>A forgatókönyv \#6: nagy adatkészlet egy SQL Server adatbázis a helyszínen, célzás SQL Server egy Azure virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="865c6-204"><a name="largedbtodb"></a>Scenario \#6: Large dataset in a SQL Server database on-prem, targeting SQL Server in an Azure Virtual Machine</span></span>
![Nagy SQL-adatbázis a helyszíni tooSQL DB az Azure-ban][6]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="865c6-206">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-206">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
1. <span data-ttu-id="865c6-207">Hozzon létre egy Azure virtuális gépen futó SQL Server IPython Notebook kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="865c6-207">Create an Azure Virtual Machine running SQL Server and IPython Notebook server.</span></span>
2. <span data-ttu-id="865c6-208">Használja az adatok hello módszerek tooexport hello adatok exportálása az SQL Server toodump fájlok.</span><span class="sxs-lookup"><span data-stu-id="865c6-208">Use one of hello data export methods tooexport hello data from SQL Server toodump files.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="865c6-209">Ha úgy dönt, toomove hello helyszíni adatbázisban, egy másik (gyorsabb) metódus toomove hello adatbázis teljes toothe SQL Server-példányra az Azure-ban minden adatát.</span><span class="sxs-lookup"><span data-stu-id="865c6-209">If you decide toomove all data from hello on-prem database, an alternate (faster) method toomove hello full database toothe SQL Server instance in Azure.</span></span> <span data-ttu-id="865c6-210">Hello lépéseket tooexport adatok kihagyása, adatbázis, és a betöltés/importálási adatok toohello céladatbázis létrehozása, és kövesse a hello alternatív módszert.</span><span class="sxs-lookup"><span data-stu-id="865c6-210">Skip hello steps tooexport data, create database, and load/import data toohello target database and follow hello alternate method.</span></span>
   > 
   > 
3. <span data-ttu-id="865c6-211">Töltse fel a memóriakép fájlokhoz tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="865c6-211">Upload dump files tooAzure storage container.</span></span>
4. <span data-ttu-id="865c6-212">Betöltési hello adatok tooa SQL Server-adatbázis egy Azure virtuális gépet futtat.</span><span class="sxs-lookup"><span data-stu-id="865c6-212">Load hello data tooa SQL Server database running on an Azure Virtual Machine.</span></span>
   
   <span data-ttu-id="865c6-213">a.</span><span class="sxs-lookup"><span data-stu-id="865c6-213">a.</span></span>  <span data-ttu-id="865c6-214">Bejelentkezési toohello SQL Server virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="865c6-214">Login toohello SQL Server VM.</span></span>
   
   <span data-ttu-id="865c6-215">b.</span><span class="sxs-lookup"><span data-stu-id="865c6-215">b.</span></span>  <span data-ttu-id="865c6-216">Adatfájlok le az Azure storage tároló toohello helyi-VM mappából.</span><span class="sxs-lookup"><span data-stu-id="865c6-216">Download data files from an Azure storage container toohello local-VM folder.</span></span>
   
   <span data-ttu-id="865c6-217">c.</span><span class="sxs-lookup"><span data-stu-id="865c6-217">c.</span></span>  <span data-ttu-id="865c6-218">Futtassa az SQL Server Management Studio eszközt.</span><span class="sxs-lookup"><span data-stu-id="865c6-218">Run SQL Server Management Studio.</span></span>
   
   <span data-ttu-id="865c6-219">d.</span><span class="sxs-lookup"><span data-stu-id="865c6-219">d.</span></span>  <span data-ttu-id="865c6-220">Adatbázis és a célként megadott táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-220">Create database and target tables.</span></span>
   
   <span data-ttu-id="865c6-221">e.</span><span class="sxs-lookup"><span data-stu-id="865c6-221">e.</span></span>  <span data-ttu-id="865c6-222">Hello tömeges egyikét módszerek tooload hello adatok importálása.</span><span class="sxs-lookup"><span data-stu-id="865c6-222">Use one of hello bulk import methods tooload hello data.</span></span>
   
   <span data-ttu-id="865c6-223">f.</span><span class="sxs-lookup"><span data-stu-id="865c6-223">f.</span></span>  <span data-ttu-id="865c6-224">Ha táblákra szükség, hozzon létre indexek tooexpedite illesztéseket.</span><span class="sxs-lookup"><span data-stu-id="865c6-224">If table joins are required, create indexes tooexpedite joins.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="865c6-225">Nagy adatmennyiség gyorsabb betöltését, hozzon létre a particionált táblákat és toobulk hello és adatokat importálhat párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="865c6-225">For faster loading of large data sizes, create partitioned tables and toobulk import hello data in parallel.</span></span> <span data-ttu-id="865c6-226">További információkért lásd: [párhuzamos importálhat tooSQL particionált táblák](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span><span class="sxs-lookup"><span data-stu-id="865c6-226">For more information, see [Parallel Data Import tooSQL Partitioned Tables](machine-learning-data-science-parallel-load-sql-partitioned-tables.md).</span></span>
   > 
   > 
5. <span data-ttu-id="865c6-227">Adatokba, és szolgáltatások igény szerint.</span><span class="sxs-lookup"><span data-stu-id="865c6-227">Explore data, create features as needed.</span></span> <span data-ttu-id="865c6-228">Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe.</span><span class="sxs-lookup"><span data-stu-id="865c6-228">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="865c6-229">Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.</span><span class="sxs-lookup"><span data-stu-id="865c6-229">Only note hello necessary query toocreate them.</span></span>
6. <span data-ttu-id="865c6-230">Döntse el, az adatok mintájának méretét, ha szükséges, és/vagy a szükséges.</span><span class="sxs-lookup"><span data-stu-id="865c6-230">Decide on a data sample size, if needed and/or desired.</span></span>
7. <span data-ttu-id="865c6-231">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-231">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
8. <span data-ttu-id="865c6-232">Olvasási hello adatok közvetlenül a hello hello használata az SQL Server [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="865c6-232">Read hello data directly from hello SQL Server using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="865c6-233">Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="865c6-233">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
9. <span data-ttu-id="865c6-234">Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdve.</span><span class="sxs-lookup"><span data-stu-id="865c6-234">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

### <a name="alternate-method-toocopy-a-full-database-from-an-on-premises--sql-server-tooazure-sql-database"></a><span data-ttu-id="865c6-235">Alternatív módszert toocopy a teljes adatbázis egy helyi SQL Server tooAzure SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="865c6-235">Alternate method toocopy a full database from an on-premises  SQL Server tooAzure SQL Database</span></span>
![Válassza le a helyi adatbázis, és csatlakoztassa tooSQL DB az Azure-ban][7]

#### <a name="additional-azure-resources-azure-virtual-machine-sql-server--ipython-notebook-server"></a><span data-ttu-id="865c6-237">További Azure-erőforrások: Azure virtuális gép (SQL Server / IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-237">Additional Azure resources: Azure Virtual Machine (SQL Server / IPython Notebook server)</span></span>
<span data-ttu-id="865c6-238">tooreplicate hello teljes SQL Server-adatbázishoz az SQL Server virtuális gép, másolja egy adatbázist a hálózatihely-kiszolgáló egy tooanother, feltéve, hogy hello az adatbázis átmenetileg offline állapotú átvihető.</span><span class="sxs-lookup"><span data-stu-id="865c6-238">tooreplicate hello entire SQL Server database in your SQL Server VM, you should copy a database from one location/server tooanother, assuming that hello database can be taken temporarily offline.</span></span> <span data-ttu-id="865c6-239">Ez az SQL Server Management Studio Object Explorerben hello, vagy hello egyenértékű Transact-SQL-parancsok használatával teheti meg.</span><span class="sxs-lookup"><span data-stu-id="865c6-239">You do this in hello SQL Server Management Studio Object Explorer, or using hello equivalent Transact-SQL commands.</span></span>

1. <span data-ttu-id="865c6-240">Hello forrás helyen hello adatbázis leválasztásához.</span><span class="sxs-lookup"><span data-stu-id="865c6-240">Detach hello database at hello source location.</span></span> <span data-ttu-id="865c6-241">További információkért lásd: [egy adatbázis leválasztásához](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="865c6-241">For more information, see [Detach a database](https://technet.microsoft.com/library/ms191491\(v=sql.110\).aspx).</span></span>
2. <span data-ttu-id="865c6-242">Windows Explorer vagy a Windows parancssori ablakban a Másolás hello adatbázisfájl vagy a fájl és a naplófájl vagy a fájlok toohello célhelyet az SQL Server Azure-ban hello le.</span><span class="sxs-lookup"><span data-stu-id="865c6-242">In Windows Explorer or Windows Command Prompt window, copy hello detached database file or files and log file or files toohello target location on hello SQL Server VM in Azure.</span></span>
3. <span data-ttu-id="865c6-243">Hello másolt fájlok toohello cél SQL Server-példányhoz csatolja.</span><span class="sxs-lookup"><span data-stu-id="865c6-243">Attach hello copied files toohello target SQL Server instance.</span></span> <span data-ttu-id="865c6-244">További információkért lásd: [adatbázis csatolása](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span><span class="sxs-lookup"><span data-stu-id="865c6-244">For more information, see [Attach a Database](https://technet.microsoft.com/library/ms190209\(v=sql.110\).aspx).</span></span>

<span data-ttu-id="865c6-245">[Helyezze át egy adatbázis használatával válassza le, és csatolja (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span><span class="sxs-lookup"><span data-stu-id="865c6-245">[Move a Database Using Detach and Attach (Transact-SQL)](https://technet.microsoft.com/library/ms187858\(v=sql.110\).aspx)</span></span>

## <span data-ttu-id="865c6-246"><a name="largedbtohive"></a>A forgatókönyv \#7: helyi fájlok Big Data típusú adatok cél Hive-adatbázisban az Azure HDInsight Hadoop-fürtök</span><span class="sxs-lookup"><span data-stu-id="865c6-246"><a name="largedbtohive"></a>Scenario \#7: Big data in local files, target Hive database in Azure HDInsight Hadoop clusters</span></span>
![A helyi cél Hive big Data típusú adatok][9]

#### <a name="additional-azure-resources-azure-hdinsight-hadoop-cluster-and-azure-virtual-machine-ipython-notebook-server"></a><span data-ttu-id="865c6-248">További Azure-erőforrások: az Azure HDInsight Hadoop-fürt és az Azure virtuális gép (IPython Notebook kiszolgáló)</span><span class="sxs-lookup"><span data-stu-id="865c6-248">Additional Azure resources: Azure HDInsight Hadoop Cluster and Azure Virtual Machine (IPython Notebook server)</span></span>
1. <span data-ttu-id="865c6-249">Hozzon létre egy IPython Notebook Servert futtató Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="865c6-249">Create an Azure Virtual Machine running IPython Notebook server.</span></span>
2. <span data-ttu-id="865c6-250">Hozzon létre egy Azure HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="865c6-250">Create an Azure HDInsight Hadoop cluster.</span></span>
3. <span data-ttu-id="865c6-251">(Választható) Előre feldolgozzák és adatait.</span><span class="sxs-lookup"><span data-stu-id="865c6-251">(Optional) Pre-process and clean data.</span></span>
   
   <span data-ttu-id="865c6-252">a.</span><span class="sxs-lookup"><span data-stu-id="865c6-252">a.</span></span>  <span data-ttu-id="865c6-253">Előre feldolgozzák és adatait IPython jegyzetfüzet adatok elérése az Azure-ból</span><span class="sxs-lookup"><span data-stu-id="865c6-253">Pre-process and clean data in IPython Notebook, accessing data from Azure</span></span>
   
       blobs.
   
   <span data-ttu-id="865c6-254">b.</span><span class="sxs-lookup"><span data-stu-id="865c6-254">b.</span></span>  <span data-ttu-id="865c6-255">Ha szükséges. az átalakító adatok toocleaned, táblázatos formában.</span><span class="sxs-lookup"><span data-stu-id="865c6-255">Transform data toocleaned, tabular form, if needed.</span></span>
   
   <span data-ttu-id="865c6-256">c.</span><span class="sxs-lookup"><span data-stu-id="865c6-256">c.</span></span>  <span data-ttu-id="865c6-257">Adatok tooVM helyi fájlokat (IPython Notebook fut a virtuális gép tudnivalókat a helyi meghajtókra tooVM meghajtókat).</span><span class="sxs-lookup"><span data-stu-id="865c6-257">Save data tooVM-local files (IPython Notebook is running on VM, local drives refer tooVM drives).</span></span>
4. <span data-ttu-id="865c6-258">Töltse fel az adatok toohello alapértelmezett tároló hello Hadoop-fürt a hello 2.</span><span class="sxs-lookup"><span data-stu-id="865c6-258">Upload data toohello default container of hello Hadoop cluster selected in hello step 2.</span></span>
5. <span data-ttu-id="865c6-259">Terheléselosztási adatok tooHive adatbázis Azure HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="865c6-259">Load data tooHive database in Azure HDInsight Hadoop cluster.</span></span>
   
   <span data-ttu-id="865c6-260">a.</span><span class="sxs-lookup"><span data-stu-id="865c6-260">a.</span></span>  <span data-ttu-id="865c6-261">Jelentkezzen be toohello hello Hadoop-fürt átjárócsomópontjához</span><span class="sxs-lookup"><span data-stu-id="865c6-261">Log in toohello head node of hello Hadoop cluster</span></span>
   
   <span data-ttu-id="865c6-262">b.</span><span class="sxs-lookup"><span data-stu-id="865c6-262">b.</span></span>  <span data-ttu-id="865c6-263">Hello Hadoop parancssor megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="865c6-263">Open hello Hadoop Command Line.</span></span>
   
   <span data-ttu-id="865c6-264">c.</span><span class="sxs-lookup"><span data-stu-id="865c6-264">c.</span></span>  <span data-ttu-id="865c6-265">Adja meg a Hive gyökérkönyvtár hello parancs `cd %hive_home%\bin` Hadoop parancssor futtatása.</span><span class="sxs-lookup"><span data-stu-id="865c6-265">Enter hello Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="865c6-266">d.</span><span class="sxs-lookup"><span data-stu-id="865c6-266">d.</span></span>  <span data-ttu-id="865c6-267">Futtassa a hello Hive-lekérdezések toocreate adatbázis és a táblák, és adatok betöltése az blob storage-tooHive táblákat.</span><span class="sxs-lookup"><span data-stu-id="865c6-267">Run hello Hive queries toocreate database and tables, and load data from blob storage tooHive tables.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="865c6-268">Ha hello adatok nagy, a felhasználók létrehozhatják hello Hive tábla partíciókat.</span><span class="sxs-lookup"><span data-stu-id="865c6-268">If hello data is big, users can create hello Hive table with partitions.</span></span> <span data-ttu-id="865c6-269">Ezt követően a felhasználók használhatják a `for` hello átjárócsomópont tooload adatok hello Hive tábla particionálva partíció által a Hadoop parancssori hello hurkot.</span><span class="sxs-lookup"><span data-stu-id="865c6-269">Then, users can use a `for` loop in hello Hadoop Command Line on hello head node tooload data into hello Hive table partitioned by partition.</span></span>
   > 
   > 
6. <span data-ttu-id="865c6-270">Adatokba, és szükség esetén a Hadoop parancssori funkciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-270">Explore data and create features as needed in Hadoop Command Line.</span></span> <span data-ttu-id="865c6-271">Vegye figyelembe, hogy hello szolgáltatások nem szükséges a hello adatbázistáblák materializált toobe.</span><span class="sxs-lookup"><span data-stu-id="865c6-271">Note that hello features do not need toobe materialized in hello database tables.</span></span> <span data-ttu-id="865c6-272">Csak vegye figyelembe a hello szükséges lekérdezés toocreate őket.</span><span class="sxs-lookup"><span data-stu-id="865c6-272">Only note hello necessary query toocreate them.</span></span>
   
   <span data-ttu-id="865c6-273">a.</span><span class="sxs-lookup"><span data-stu-id="865c6-273">a.</span></span>  <span data-ttu-id="865c6-274">Jelentkezzen be toohello hello Hadoop-fürt átjárócsomópontjához</span><span class="sxs-lookup"><span data-stu-id="865c6-274">Log in toohello head node of hello Hadoop cluster</span></span>
   
   <span data-ttu-id="865c6-275">b.</span><span class="sxs-lookup"><span data-stu-id="865c6-275">b.</span></span>  <span data-ttu-id="865c6-276">Hello Hadoop parancssor megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="865c6-276">Open hello Hadoop Command Line.</span></span>
   
   <span data-ttu-id="865c6-277">c.</span><span class="sxs-lookup"><span data-stu-id="865c6-277">c.</span></span>  <span data-ttu-id="865c6-278">Adja meg a Hive gyökérkönyvtár hello parancs `cd %hive_home%\bin` Hadoop parancssor futtatása.</span><span class="sxs-lookup"><span data-stu-id="865c6-278">Enter hello Hive root directory by command `cd %hive_home%\bin` in Hadoop Command Line.</span></span>
   
   <span data-ttu-id="865c6-279">d.</span><span class="sxs-lookup"><span data-stu-id="865c6-279">d.</span></span>  <span data-ttu-id="865c6-280">A hello átjárócsomópontjához hello Hadoop-fürt tooexplore hello adatokat a Hadoop parancssor futtatása a hello Hive-lekérdezéseket, és igény szerint funkciók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="865c6-280">Run hello Hive queries in Hadoop Command Line on hello head node of hello Hadoop cluster tooexplore hello data and create features as needed.</span></span>
7. <span data-ttu-id="865c6-281">Ha szükséges, és/vagy szükséges, mintát hello adatok toofit az Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="865c6-281">If needed and/or desired, sample hello data toofit in Azure Machine Learning Studio.</span></span>
8. <span data-ttu-id="865c6-282">Jelentkezzen be toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="865c6-282">Sign in toohello [Azure Machine Learning Studio](https://studio.azureml.net/).</span></span>
9. <span data-ttu-id="865c6-283">Hello adatok közvetlenül olvassák be hello `Hive Queries` hello segítségével [és adatokat importálhat] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="865c6-283">Read hello data directly from hello `Hive Queries` using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="865c6-284">Beillesztés hello szükséges lekérdezés mezők, amely szolgáltatások hoz létre, és minták adatokat, ha közvetlenül a hello szükséges [és adatokat importálhat] [ import-data] lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="865c6-284">Paste hello necessary query which extracts fields, creates features, and samples data if needed directly in hello [Import Data][import-data] query.</span></span>
10. <span data-ttu-id="865c6-285">Egyszerű Azure Machine Learning kísérlet folyamata feltöltött dataset kezdve.</span><span class="sxs-lookup"><span data-stu-id="865c6-285">Simple Azure Machine Learning experiment flow starting with uploaded dataset.</span></span>

## <span data-ttu-id="865c6-286"><a name="decisiontree"></a>A forgatókönyv kiválasztása döntési fája</span><span class="sxs-lookup"><span data-stu-id="865c6-286"><a name="decisiontree"></a>Decision tree for scenario selection</span></span>
- - -
<span data-ttu-id="865c6-287">hello következő diagram foglalja össze a fent leírt hello forgatókönyvek és hello Advanced Analytics folyamat és a technológia választások tévő tooeach részletezett hello forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="865c6-287">hello following diagram summarizes hello scenarios described above and hello Advanced Analytics Process and Technology choices made that take you tooeach of hello itemized scenarios.</span></span> <span data-ttu-id="865c6-288">Vegye figyelembe, hogy az adatok feldolgozása, a feltárása, a szolgáltatás műszaki osztály és a mintavételi is igénybe vehet egy vagy több metódus/környezeti – hello forrás, a köztes, és/vagy a cél környezetekben – helyezze, és szükség szerint ismételt folytathatja.</span><span class="sxs-lookup"><span data-stu-id="865c6-288">Note that data processing, exploration, feature engineering, and sampling may take place in one or more method/environment -- at hello source, intermediate, and/or target environments – and may proceed iteratively as needed.</span></span> <span data-ttu-id="865c6-289">hello diagram csak néhány lehetséges adatfolyamok szemléltetésére szolgál, és nem biztosít teljes körű enumerálást.</span><span class="sxs-lookup"><span data-stu-id="865c6-289">hello diagram only serves as an illustration of some of possible flows and does not provide an exhaustive enumeration.</span></span>

![A minta DS folyamat bemutató forgatókönyvek][8]

### <a name="advanced-analytics-in-action-examples"></a><span data-ttu-id="865c6-291">Speciális elemzés a művelet példák</span><span class="sxs-lookup"><span data-stu-id="865c6-291">Advanced Analytics in action Examples</span></span>
<span data-ttu-id="865c6-292">A végpont Azure Machine Learning forgatókönyvek alkalmazó szoftverbiztonsági hello Advanced Analytics folyamat és a technológia használatával nyilvános adatkészleteket, lásd:</span><span class="sxs-lookup"><span data-stu-id="865c6-292">For end-to-end Azure Machine Learning walkthroughs that employ hello Advanced Analytics Process and Technology using public datasets, see:</span></span>

* <span data-ttu-id="865c6-293">[Vonja össze az adatokat tudományos folyamat működés közben: SQL Server használatával](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="865c6-293">[Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>
* <span data-ttu-id="865c6-294">[Vonja össze az adatokat tudományos folyamat működés közben: HDInsight Hadoop-fürtök használata](machine-learning-data-science-process-hive-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="865c6-294">[Team Data Science Process in action: using HDInsight Hadoop clusters](machine-learning-data-science-process-hive-walkthrough.md).</span></span>

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
