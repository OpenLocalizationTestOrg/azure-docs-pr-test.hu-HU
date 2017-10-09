---
title: "egy helyi SQL Server tooSQL Azure szolgáltatásban az Azure Data Factory aaaMove adatait |} Microsoft Docs"
description: "Állítsa be az ADF-feldolgozási folyamat composes két együtt adatok áthelyezése a naponta helyszíni adatbázisok között, és hello felhőben található adatok áttelepítési tevékenységeket."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 36837c83-2015-48be-b850-c4346aa5ae8a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 7f7e78c7a84a259539221d3235b76bb5a3cf9866
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-an-on-premises-sql-server-toosql-azure-with-azure-data-factory"></a><span data-ttu-id="831b9-103">Adatok áthelyezése egy helyi SQL server tooSQL Azure az Azure Data Factoryvel</span><span class="sxs-lookup"><span data-stu-id="831b9-103">Move data from an on-premises SQL server tooSQL Azure with Azure Data Factory</span></span>
<span data-ttu-id="831b9-104">Ez a témakör bemutatja, hogyan toomove adatait egy helyi SQL Server-adatbázis tooa SQL Azure Database keresztül Azure Blob Storage használatával hello Azure Data Factory (ADF).</span><span class="sxs-lookup"><span data-stu-id="831b9-104">This topic shows how toomove data from an on-premises SQL Server Database tooa SQL Azure Database via Azure Blob Storage using hello Azure Data Factory (ADF).</span></span>

<span data-ttu-id="831b9-105">Az egy táblázatot, amely összefoglalja az áthelyezett adatok tooan Azure SQL Database különböző beállítások [adatok tooan Azure SQL-adatbázis áthelyezése az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="831b9-105">For a table that summarizes various options for moving data tooan Azure SQL Database, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

## <span data-ttu-id="831b9-106"><a name="intro"></a>Bevezetés: ADF és mikor lehet használt toomigrate adatokat?</span><span class="sxs-lookup"><span data-stu-id="831b9-106"><a name="intro"></a>Introduction: What is ADF and when should it be used toomigrate data?</span></span>
<span data-ttu-id="831b9-107">Az Azure Data Factory koordinálja, és automatikusan hello adatátviteli, valamint az adatok átalakítása felhőalapú teljes körűen felügyelt adatok integrációs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="831b9-107">Azure Data Factory is a fully managed cloud-based data integration service that orchestrates and automates hello movement and transformation of data.</span></span> <span data-ttu-id="831b9-108">hello kulcs hello ADF modellben fogalma folyamat.</span><span class="sxs-lookup"><span data-stu-id="831b9-108">hello key concept in hello ADF model is pipeline.</span></span> <span data-ttu-id="831b9-109">Egy folyamat tevékenységek, logikai csoportját, amelyek mindegyike meghatározza az hello műveletek tooperform adatkészletek szereplő hello adatok.</span><span class="sxs-lookup"><span data-stu-id="831b9-109">A pipeline is a logical grouping of Activities, each of which defines hello actions tooperform on hello data contained in Datasets.</span></span> <span data-ttu-id="831b9-110">Társított szolgáltatások olyan használt toodefine hello információk a Data Factory tooconnect toohello adatforrásaihoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="831b9-110">Linked services are used toodefine hello information needed for Data Factory tooconnect toohello data resources.</span></span>

<span data-ttu-id="831b9-111">Az ADF meglévő adatfeldolgozási szolgáltatások az adatok folyamatok, amelyek magas rendelkezésre állású és felügyelt hello felhőben állítható össze.</span><span class="sxs-lookup"><span data-stu-id="831b9-111">With ADF, existing data processing services can be composed into data pipelines that are highly available and managed in hello cloud.</span></span> <span data-ttu-id="831b9-112">Ezen adatok folyamatok is lehet ütemezett tooingest, előkészítése, átalakítására, elemzése és adatok közzététele, és ADF kezeli, és koordinálja a hello összetett adatokat és feldolgozási függőségek.</span><span class="sxs-lookup"><span data-stu-id="831b9-112">These data pipelines can be scheduled tooingest, prepare, transform, analyze, and publish data, and ADF manages and orchestrates hello complex data and processing dependencies.</span></span> <span data-ttu-id="831b9-113">Megoldások is gyorsan beépített és a telepített hello felhő csatlakozás helyszíni egyre több és a felhő tárolt adatforrások.</span><span class="sxs-lookup"><span data-stu-id="831b9-113">Solutions can be quickly built and deployed in hello cloud, connecting a growing number of on-premises and cloud data sources.</span></span>

<span data-ttu-id="831b9-114">Érdemes lehet ADF:</span><span class="sxs-lookup"><span data-stu-id="831b9-114">Consider using ADF:</span></span>

* <span data-ttu-id="831b9-115">Ha adatok igényeinek toobe folyamatosan át egy hibrid forgatókönyvekben, amelyek mindkettőt fér hozzá a helyszíni és felhőalapú erőforrások</span><span class="sxs-lookup"><span data-stu-id="831b9-115">when data needs toobe continually migrated in a hybrid scenario that accesses both on-premises and cloud resources</span></span>
* <span data-ttu-id="831b9-116">Ha hello adatok tranzakcióalapú van, vagy igények toobe módosítható vagy nem rendelkezik az üzleti logika hozzá tooit, amikor az áttelepíteni.</span><span class="sxs-lookup"><span data-stu-id="831b9-116">when hello data is transacted or needs toobe modified or have business logic added tooit when being migrated.</span></span>

<span data-ttu-id="831b9-117">ADF hello ütemezés és a feladatok által kezelt adatok rendszeres időközönként hello mozgása egyszerű JSON-parancsfájlok használata a figyelését teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="831b9-117">ADF allows for hello scheduling and monitoring of jobs using simple JSON scripts that manage hello movement of data on a periodic basis.</span></span> <span data-ttu-id="831b9-118">ADF is rendelkeznek egyéb képességeit, például a összetett műveletek támogatása.</span><span class="sxs-lookup"><span data-stu-id="831b9-118">ADF also has other capabilities such as support for complex operations.</span></span> <span data-ttu-id="831b9-119">Az ADF további információkért lásd: hello dokumentációját a [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="831b9-119">For more information on ADF, see hello documentation at [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span></span>

## <span data-ttu-id="831b9-120"><a name="scenario"></a>hello forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="831b9-120"><a name="scenario"></a>hello Scenario</span></span>
<span data-ttu-id="831b9-121">Az ADF-feldolgozási folyamat két adatok áttelepítési tevékenységek composes beállítjuk.</span><span class="sxs-lookup"><span data-stu-id="831b9-121">We set up an ADF pipeline that composes two data migration activities.</span></span> <span data-ttu-id="831b9-122">Együtt mozognak adatok naponta egy helyi SQL-adatbázis és az Azure SQL Database hello felhő között.</span><span class="sxs-lookup"><span data-stu-id="831b9-122">Together they move data on a daily basis between an on-premises SQL database and an Azure SQL Database in hello cloud.</span></span> <span data-ttu-id="831b9-123">hello két tevékenységek a következők:</span><span class="sxs-lookup"><span data-stu-id="831b9-123">hello two activities are:</span></span>

* <span data-ttu-id="831b9-124">adatok másolása egy helyi SQL Server adatbázis tooan Azure Blob Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="831b9-124">copy data from an on-premises SQL Server database tooan Azure Blob Storage account</span></span>
* <span data-ttu-id="831b9-125">adatok másolása az hello Azure Blob Storage-fiók tooan Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="831b9-125">copy data from hello Azure Blob Storage account tooan Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="831b9-126">hello látható itt volt módosítani a hello hello ADF csapat által biztosított oktatóanyag részletes lépései: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) toohello a témakör vonatkozó szakaszaihoz hivatkozik Ha a megfelelő biztosított.</span><span class="sxs-lookup"><span data-stu-id="831b9-126">hello steps shown here have been adapted from hello more detailed tutorial provided by hello ADF team: [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) References toohello relevant sections of that topic are provided when appropriate.</span></span>
>
>

## <span data-ttu-id="831b9-127"><a name="prereqs"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="831b9-127"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="831b9-128">Ez az oktatóanyag feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="831b9-128">This tutorial assumes you have:</span></span>

* <span data-ttu-id="831b9-129">Egy **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="831b9-129">An **Azure subscription**.</span></span> <span data-ttu-id="831b9-130">Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="831b9-130">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="831b9-131">Egy **Azure storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="831b9-131">An **Azure storage account**.</span></span> <span data-ttu-id="831b9-132">Egy Azure storage-fiók ebben az oktatóanyagban hello adatok tárolásához használja.</span><span class="sxs-lookup"><span data-stu-id="831b9-132">You use an Azure storage account for storing hello data in this tutorial.</span></span> <span data-ttu-id="831b9-133">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk.</span><span class="sxs-lookup"><span data-stu-id="831b9-133">If you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="831b9-134">Hello storage-fiók létrehozását követően használja a tooaccess hello tárolási tooobtain hello fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="831b9-134">After you have created hello storage account, you need tooobtain hello account key used tooaccess hello storage.</span></span> <span data-ttu-id="831b9-135">Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="831b9-135">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="831b9-136">Hozzáférés tooan **Azure SQL Database**.</span><span class="sxs-lookup"><span data-stu-id="831b9-136">Access tooan **Azure SQL Database**.</span></span> <span data-ttu-id="831b9-137">Ha be kell állítania egy Azure SQL Database, hello tpoic [Ismerkedés a Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) bemutatja, hogy miként tooprovision Azure SQL adatbázis új példányát.</span><span class="sxs-lookup"><span data-stu-id="831b9-137">If you must set up an Azure SQL Database, hello tpoic [Getting Started with Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) provides information on how tooprovision a new instance of an Azure SQL Database.</span></span>
* <span data-ttu-id="831b9-138">Telepített és konfigurált **Azure PowerShell** helyileg.</span><span class="sxs-lookup"><span data-stu-id="831b9-138">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="831b9-139">Útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="831b9-139">For instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!NOTE]
> <span data-ttu-id="831b9-140">Ez az eljárás használja hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="831b9-140">This procedure uses hello [Azure portal](https://portal.azure.com/).</span></span>
>
>

## <span data-ttu-id="831b9-141"><a name="upload-data"></a>Feltöltés hello adatok tooyour a helyszíni SQL Server</span><span class="sxs-lookup"><span data-stu-id="831b9-141"><a name="upload-data"></a> Upload hello data tooyour on-premises SQL Server</span></span>
<span data-ttu-id="831b9-142">Hello használjuk [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello áttelepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="831b9-142">We use hello [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) toodemonstrate hello migration process.</span></span> <span data-ttu-id="831b9-143">hello NYC Taxi adatkészlet érhető el, a feladás egy vagy több, az Azure blob storage leírtaknak megfelelően [NYC Taxi adatok](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="831b9-143">hello NYC Taxi dataset is available, as noted in that post, on Azure blob storage [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="831b9-144">hello adatok van két fájlt, hello trip_data.csv fájl, amely út adatokat tartalmaz, és hello trip_far.csv fájlt, amely minden út kifizette hello jegy ára részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="831b9-144">hello data has two files, hello trip_data.csv file, which contains trip details, and hello  trip_far.csv file, which contains details of hello fare paid for each trip.</span></span> <span data-ttu-id="831b9-145">A minta és az ezek a fájlok leírása szerepelnek [NYC Taxi Utazgatással adatkészlet leírása](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span><span class="sxs-lookup"><span data-stu-id="831b9-145">A sample and description of these files are provided in [NYC Taxi Trips Dataset Description](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span></span>

<span data-ttu-id="831b9-146">A saját adatok tooa készletét itt megadott hello eljárás igazítja, vagy hello lépésekkel hello NYC Taxi dataset használatával leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="831b9-146">You can either adapt hello procedure provided here tooa set of your own data or follow hello steps as described by using hello NYC Taxi dataset.</span></span> <span data-ttu-id="831b9-147">a helyszíni SQL Server adatbázisba NYC Taxi dataset tooupload hello eljárással hello leírt [tömeges adatok importálása az SQL Server-adatbázisba](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span><span class="sxs-lookup"><span data-stu-id="831b9-147">tooupload hello NYC Taxi dataset into your on-premises SQL Server database, follow hello procedure outlined in [Bulk Import Data into SQL Server Database](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span></span> <span data-ttu-id="831b9-148">Ezeket az utasításokat egy SQL Server egy Azure virtuális gépen, de a helyszíni SQL Server toohello feltöltése hello eljárását hello azonos.</span><span class="sxs-lookup"><span data-stu-id="831b9-148">These instructions are for a SQL Server on an Azure Virtual Machine, but hello procedure for uploading toohello on-premises SQL Server is hello same.</span></span>

## <span data-ttu-id="831b9-149"><a name="create-adf"></a>Egy Azure Data Factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="831b9-149"><a name="create-adf"></a> Create an Azure Data Factory</span></span>
<span data-ttu-id="831b9-150">egy új Azure Data Factory és az erőforráscsoport létrehozása a hello utasításokat hello [Azure-portálon](https://portal.azure.com/) biztosított [hozzon létre egy Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span><span class="sxs-lookup"><span data-stu-id="831b9-150">hello instructions for creating a new Azure Data Factory and a resource group in hello [Azure portal](https://portal.azure.com/) are provided [Create an Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span></span> <span data-ttu-id="831b9-151">Név hello új ADF példány *adfdsp* és name hello erőforráscsoport létrehozásánál *adfdsprg*.</span><span class="sxs-lookup"><span data-stu-id="831b9-151">Name hello new ADF instance *adfdsp* and name hello resource group created *adfdsprg*.</span></span>

## <a name="install-and-configure-up-hello-data-management-gateway"></a><span data-ttu-id="831b9-152">Telepítse és konfigurálja az adatkezelési átjáró hello mentése</span><span class="sxs-lookup"><span data-stu-id="831b9-152">Install and configure up hello Data Management Gateway</span></span>
<span data-ttu-id="831b9-153">tooenable a folyamatok egy az Azure data factory toowork egy helyi SQL Server, a tooadd szükség van rá a társított szolgáltatás toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="831b9-153">tooenable your pipelines in an Azure data factory toowork with an on-premises SQL Server, you need tooadd it as a Linked Service toohello data factory.</span></span> <span data-ttu-id="831b9-154">a csatolt szolgáltatása a helyszíni SQL Server kiszolgáló toocreate, kell:</span><span class="sxs-lookup"><span data-stu-id="831b9-154">toocreate a Linked Service for an on-premises SQL Server, you must:</span></span>

* <span data-ttu-id="831b9-155">Töltse le és telepítse a Microsoft adatkezelési átjáró hello a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="831b9-155">download and install Microsoft Data Management Gateway onto hello on-premises computer.</span></span>
* <span data-ttu-id="831b9-156">Konfigurálja a kapcsolódó hello szolgáltatást hello a helyszíni adatok forrás toouse hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="831b9-156">configure hello linked service for hello on-premises data source toouse hello gateway.</span></span>

<span data-ttu-id="831b9-157">Az adatkezelési átjáró hello rendezi sorba, és deserializes hello forrás és a fogadó adatokat hello számítógépen hol tárolja.</span><span class="sxs-lookup"><span data-stu-id="831b9-157">hello Data Management Gateway serializes and deserializes hello source and sink data on hello computer where it is hosted.</span></span>

<span data-ttu-id="831b9-158">Telepítési utasításokat és az adatkezelési átjáró részleteinek: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="831b9-158">For set-up instructions and details on Data Management Gateway, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span></span>

## <span data-ttu-id="831b9-159"><a name="adflinkedservices"></a>Hozzon létre csatolt szolgáltatások tooconnect toohello erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="831b9-159"><a name="adflinkedservices"></a>Create linked services tooconnect toohello data resources</span></span>
<span data-ttu-id="831b9-160">A társított szolgáltatás az Azure Data Factory tooconnect tooa adatforrás, melyhez a szükséges hello információkat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="831b9-160">A linked service defines hello information needed for Azure Data Factory tooconnect tooa data resource.</span></span> <span data-ttu-id="831b9-161">hello lépésről lépésre társított szolgáltatások létrehozásához megadott [társított szolgáltatások létrehozásához](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span><span class="sxs-lookup"><span data-stu-id="831b9-161">hello step-by-step procedure for creating linked services is provided in [Create linked services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span></span>

<span data-ttu-id="831b9-162">Három erőforrások van ebben a forgatókönyvben összekapcsolt szolgáltatások van szükség.</span><span class="sxs-lookup"><span data-stu-id="831b9-162">We have three resources in this scenario for which linked services are needed.</span></span>

1. [<span data-ttu-id="831b9-163">A helyszíni SQL Server társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="831b9-163">Linked service for on-premises SQL Server</span></span>](#adf-linked-service-onprem-sql)
2. [<span data-ttu-id="831b9-164">Az Azure Blob Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="831b9-164">Linked service for Azure Blob Storage</span></span>](#adf-linked-service-blob-store)
3. [<span data-ttu-id="831b9-165">Az Azure SQL database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="831b9-165">Linked service for Azure SQL database</span></span>](#adf-linked-service-azure-sql)

### <span data-ttu-id="831b9-166"><a name="adf-linked-service-onprem-sql"></a>A helyszíni SQL Server-adatbázis társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="831b9-166"><a name="adf-linked-service-onprem-sql"></a>Linked service for on-premises SQL Server database</span></span>
<span data-ttu-id="831b9-167">hello toocreate kapcsolódó hello szolgáltatást a helyi SQL Server:</span><span class="sxs-lookup"><span data-stu-id="831b9-167">toocreate hello linked service for hello on-premises SQL Server:</span></span>

* <span data-ttu-id="831b9-168">Kattintson a hello **adattár** a klasszikus Azure portál hello ADF kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="831b9-168">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="831b9-169">Válassza ki **SQL** , és írja be a hello *felhasználónév* és *jelszó* a hello a helyszíni SQL Server-felhasználó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="831b9-169">select **SQL** and enter hello *username* and *password* credentials for hello on-premises SQL Server.</span></span> <span data-ttu-id="831b9-170">Tooenter hello kiszolgálónév másként van szüksége egy **teljesen minősített kiszolgálónév fordított perjel példány neve (kiszolgáló_neve\példány_neve)**.</span><span class="sxs-lookup"><span data-stu-id="831b9-170">You need tooenter hello servername as a **fully qualified servername backslash instance name (servername\instancename)**.</span></span> <span data-ttu-id="831b9-171">A társított szolgáltatás neve hello *adfonpremsql*.</span><span class="sxs-lookup"><span data-stu-id="831b9-171">Name hello linked service *adfonpremsql*.</span></span>

### <span data-ttu-id="831b9-172"><a name="adf-linked-service-blob-store"></a>A Blob társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="831b9-172"><a name="adf-linked-service-blob-store"></a>Linked service for Blob</span></span>
<span data-ttu-id="831b9-173">toocreate hello hello Azure Blob Storage-fiókhoz társított szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="831b9-173">toocreate hello linked service for hello Azure Blob Storage account:</span></span>

* <span data-ttu-id="831b9-174">Kattintson a hello **adattár** a klasszikus Azure portál hello ADF kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="831b9-174">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="831b9-175">Válassza ki **Azure Storage-fiók**</span><span class="sxs-lookup"><span data-stu-id="831b9-175">select **Azure Storage Account**</span></span>
* <span data-ttu-id="831b9-176">Adja meg a hello Azure Blob Storage fiók kulcs és a tároló nevét.</span><span class="sxs-lookup"><span data-stu-id="831b9-176">enter hello Azure Blob Storage account key and container name.</span></span> <span data-ttu-id="831b9-177">Társított szolgáltatás neve hello *adfds*.</span><span class="sxs-lookup"><span data-stu-id="831b9-177">Name hello Linked Service *adfds*.</span></span>

### <span data-ttu-id="831b9-178"><a name="adf-linked-service-azure-sql"></a>Az Azure SQL database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="831b9-178"><a name="adf-linked-service-azure-sql"></a>Linked service for Azure SQL database</span></span>
<span data-ttu-id="831b9-179">hello Azure SQL Database toocreate kapcsolódó hello szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="831b9-179">toocreate hello linked service for hello Azure SQL Database:</span></span>

* <span data-ttu-id="831b9-180">Kattintson a hello **adattár** a klasszikus Azure portál hello ADF kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="831b9-180">click hello **Data Store** in hello ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="831b9-181">Válassza ki **Azure SQL** , és írja be a hello *felhasználónév* és *jelszó* hello Azure SQL adatbázis hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="831b9-181">select **Azure SQL** and enter hello *username* and *password* credentials for hello Azure SQL Database.</span></span> <span data-ttu-id="831b9-182">Hello *felhasználónév* kell megadni,  *user@servername* .</span><span class="sxs-lookup"><span data-stu-id="831b9-182">hello *username* must be specified as *user@servername*.</span></span>   

## <span data-ttu-id="831b9-183"><a name="adf-tables"></a>Adja meg, és hozzon létre táblák toospecify hogyan tooaccess hello adatkészletek</span><span class="sxs-lookup"><span data-stu-id="831b9-183"><a name="adf-tables"></a>Define and create tables toospecify how tooaccess hello datasets</span></span>
<span data-ttu-id="831b9-184">Hozzon létre táblákat, adja meg a következő eljárások parancsfájlalapú hello hello struktúra, helyét és hello adatkészletek rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="831b9-184">Create tables that specify hello structure, location, and availability of hello datasets with hello following script-based procedures.</span></span> <span data-ttu-id="831b9-185">JSON-fájlok használt toodefine hello táblákat.</span><span class="sxs-lookup"><span data-stu-id="831b9-185">JSON files are used toodefine hello tables.</span></span> <span data-ttu-id="831b9-186">Ezek a fájlok hello szerkezete további információkért lásd: [adatkészletek](../data-factory/data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="831b9-186">For more information on hello structure of these files, see [Datasets](../data-factory/data-factory-create-datasets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="831b9-187">Végre kell hajtani hello `Add-AzureAccount` parancsmag hello végrehajtása előtt [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) , amely közvetlenül az Azure-előfizetés hello parancsmag tooconfirm van kiválasztva a hello parancs végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="831b9-187">You should execute hello `Add-AzureAccount` cmdlet before executing hello [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet tooconfirm that hello right Azure subscription is selected for hello command execution.</span></span> <span data-ttu-id="831b9-188">Ez a parancsmag dokumentációjáért lásd: [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="831b9-188">For documentation of this cmdlet, see [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span></span>
>
>

<span data-ttu-id="831b9-189">hello JSON-alapú definíciók hello táblák neve a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="831b9-189">hello JSON-based definitions in hello tables use hello following names:</span></span>

* <span data-ttu-id="831b9-190">Hello **táblanév** hello a helyszíni SQL server esetében *nyctaxi_data*</span><span class="sxs-lookup"><span data-stu-id="831b9-190">hello **table name** in hello on-premises SQL server is *nyctaxi_data*</span></span>
* <span data-ttu-id="831b9-191">Hello **Tárolónév** hello Azure Blob Storage a fiók esetében *containername*</span><span class="sxs-lookup"><span data-stu-id="831b9-191">hello **container name** in hello Azure Blob Storage account is *containername*</span></span>  

<span data-ttu-id="831b9-192">Három definíciói az ADF adatcsatorna van szükség:</span><span class="sxs-lookup"><span data-stu-id="831b9-192">Three table definitions are needed for this ADF pipeline:</span></span>

1. [<span data-ttu-id="831b9-193">A helyszíni SQL-tábla</span><span class="sxs-lookup"><span data-stu-id="831b9-193">SQL on-premises Table</span></span>](#adf-table-onprem-sql)
2. [<span data-ttu-id="831b9-194">A BLOB tábla</span><span class="sxs-lookup"><span data-stu-id="831b9-194">Blob Table </span></span>](#adf-table-blob-store)
3. [<span data-ttu-id="831b9-195">Az SQL Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="831b9-195">SQL Azure Table</span></span>](#adf-table-azure-sql)

> [!NOTE]
> <span data-ttu-id="831b9-196">Ezek az eljárások Azure PowerShell toodefine használja, és hello ADF tevékenységek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="831b9-196">These procedures use Azure PowerShell toodefine and create hello ADF activities.</span></span> <span data-ttu-id="831b9-197">De ezek a feladatok hello Azure-portál használatával is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="831b9-197">But these tasks can also be accomplished using hello Azure portal.</span></span> <span data-ttu-id="831b9-198">További információkért lásd: [adatkészletek létrehozása](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span><span class="sxs-lookup"><span data-stu-id="831b9-198">For details, see [Create datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span></span>
>
>

### <span data-ttu-id="831b9-199"><a name="adf-table-onprem-sql"></a>A helyszíni SQL-tábla</span><span class="sxs-lookup"><span data-stu-id="831b9-199"><a name="adf-table-onprem-sql"></a>SQL on-premises Table</span></span>
<span data-ttu-id="831b9-200">hello hello-definíció a helyszíni SQL Server megadott JSON-fájl a következő hello:</span><span class="sxs-lookup"><span data-stu-id="831b9-200">hello table definition for hello on-premises SQL Server is specified in hello following JSON file:</span></span>

        {
            "name": "OnPremSQLTable",
            "properties":
            {
                "location":
                {
                "type": "OnPremisesSqlServerTableLocation",
                "tableName": "nyctaxi_data",
                "linkedServiceName": "adfonpremsql"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1,   
                "waitOnExternal":
                {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
                }

                }
            }
        }

<span data-ttu-id="831b9-201">hello oszlopnevek nem szerepeltek itt.</span><span class="sxs-lookup"><span data-stu-id="831b9-201">hello column names were not included here.</span></span> <span data-ttu-id="831b9-202">Részterv választhat a hello oszlopnevek itt többek között (részletekért ellenőrizze a hello [ADF dokumentáció](../data-factory/data-factory-data-movement-activities.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="831b9-202">You can sub-select on hello column names by including them here (for details check hello [ADF documentation](../data-factory/data-factory-data-movement-activities.md) topic.</span></span>

<span data-ttu-id="831b9-203">Hello JSON-definícióból hello tábla másolja egy nevű fájlba *onpremtabledef.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\onpremtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="831b9-203">Copy hello JSON definition of hello table into a file called *onpremtabledef.json* file and save it tooa known location (here assumed toobe *C:\temp\onpremtabledef.json*).</span></span> <span data-ttu-id="831b9-204">Hello tábla létrehozása az ADF hello Azure PowerShell-parancsmag a következő:</span><span class="sxs-lookup"><span data-stu-id="831b9-204">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <span data-ttu-id="831b9-205"><a name="adf-table-blob-store"></a>A BLOB tábla</span><span class="sxs-lookup"><span data-stu-id="831b9-205"><a name="adf-table-blob-store"></a>Blob Table</span></span>
<span data-ttu-id="831b9-206">Hello hello tábla definícióját kimeneti blob helyére hello alábbi (Ez leképezi a helyszíni tooAzure blob okozhatnak hello adatait):</span><span class="sxs-lookup"><span data-stu-id="831b9-206">Definition for hello table for hello output blob location is in hello following (this maps hello ingested data from on-premises tooAzure blob):</span></span>

        {
            "name": "OutputBlobTable",
            "properties":
            {
                "location":
                {
                "type": "AzureBlobLocation",
                "folderPath": "containername",
                "format":
                {
                "type": "TextFormat",
                "columnDelimiter": "\t"
                },
                "linkedServiceName": "adfds"
                },
                "availability":
                {
                "frequency": "Day",
                "interval": 1
                }
            }
        }

<span data-ttu-id="831b9-207">Hello JSON-definícióból hello tábla másolja egy nevű fájlba *bloboutputtabledef.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\bloboutputtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="831b9-207">Copy hello JSON definition of hello table into a file called *bloboutputtabledef.json* file and save it tooa known location (here assumed toobe *C:\temp\bloboutputtabledef.json*).</span></span> <span data-ttu-id="831b9-208">Hello tábla létrehozása az ADF hello Azure PowerShell-parancsmag a következő:</span><span class="sxs-lookup"><span data-stu-id="831b9-208">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <span data-ttu-id="831b9-209"><a name="adf-table-azure-sq"></a>Az SQL Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="831b9-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span></span>
<span data-ttu-id="831b9-210">Az SQL Azure kimeneti hello hello tábla definícióját hello következő (ebben a sémában leképezhető hello blob érkező hello adatok) kell megadni:</span><span class="sxs-lookup"><span data-stu-id="831b9-210">Definition for hello table for hello SQL Azure output is in hello following (this schema maps hello data coming from hello blob):</span></span>

    {
        "name": "OutputSQLAzureTable",
        "properties":
        {
            "structure":
            [
                { "name": "column1", type": "String"},
                { "name": "column2", type": "String"}                
            ],
            "location":
            {
                "type": "AzureSqlTableLocation",
                "tableName": "your_db_name",
                "linkedServiceName": "adfdssqlazure_linked_servicename"
            },
            "availability":
            {
                "frequency": "Day",
                "interval": 1            
            }
        }
    }

<span data-ttu-id="831b9-211">Hello JSON-definícióból hello tábla másolja egy nevű fájlba *AzureSqlTable.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\AzureSqlTable.json*).</span><span class="sxs-lookup"><span data-stu-id="831b9-211">Copy hello JSON definition of hello table into a file called *AzureSqlTable.json* file and save it tooa known location (here assumed toobe *C:\temp\AzureSqlTable.json*).</span></span> <span data-ttu-id="831b9-212">Hello tábla létrehozása az ADF hello Azure PowerShell-parancsmag a következő:</span><span class="sxs-lookup"><span data-stu-id="831b9-212">Create hello table in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <span data-ttu-id="831b9-213"><a name="adf-pipeline"></a>Adja meg, és hello folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="831b9-213"><a name="adf-pipeline"></a>Define and create hello pipeline</span></span>
<span data-ttu-id="831b9-214">Adja meg a toohello tartozó hello tevékenység a következő feldolgozási sorban, és hozzon létre hello folyamat a következő eljárások parancsfájlalapú hello.</span><span class="sxs-lookup"><span data-stu-id="831b9-214">Specify hello activities that belong toohello pipeline and create hello pipeline with hello following script-based procedures.</span></span> <span data-ttu-id="831b9-215">A JSON-fájl használt toodefine hello csővezeték tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="831b9-215">A JSON file is used toodefine hello pipeline properties.</span></span>

* <span data-ttu-id="831b9-216">hello parancsfájl feltételezi, hogy hello **adatcsatorna neve** van *AMLDSProcessPipeline*.</span><span class="sxs-lookup"><span data-stu-id="831b9-216">hello script assumes that hello **pipeline name** is *AMLDSProcessPipeline*.</span></span>
* <span data-ttu-id="831b9-217">Ne feledje, hogy hivatott hello periodikusságát hello adatcsatorna toobe végrehajtva a következő napi szinten és -felhasználási hello alapértelmezett végrehajtási idő hello feladat (12 óra UTC).</span><span class="sxs-lookup"><span data-stu-id="831b9-217">Also note that we set hello periodicity of hello pipeline toobe executed on daily basis and use hello default execution time for hello job (12 am UTC).</span></span>

> [!NOTE]
> <span data-ttu-id="831b9-218">hello alábbi eljárások használata az Azure PowerShell toodefine és hello ADF folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="831b9-218">hello following procedures use Azure PowerShell toodefine and create hello ADF pipeline.</span></span> <span data-ttu-id="831b9-219">Azonban ez a feladat az Azure portál használatával is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="831b9-219">But this task can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="831b9-220">További információkért lásd: [létrehozási folyamat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span><span class="sxs-lookup"><span data-stu-id="831b9-220">For details, see [Create pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span></span>
>
>

<span data-ttu-id="831b9-221">Hello definíciói használatával megadott korábban hello csővezeték definíciója hello ADF van megadva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="831b9-221">Using hello table definitions provided previously, hello pipeline definition for hello ADF is specified as follows:</span></span>

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL tooAzure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server tooblob",     
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OnPremSQLTable"} ],
                        "outputs": [ {"name": "OutputBlobTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "SqlSource",
                                "sqlReaderQuery": "select * from nyctaxi_data"
                            },
                            "sink":
                            {
                                "type": "BlobSink"
                            }   
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 0,
                            "timeout": "01:00:00"
                        }       

                     },

                    {
                        "name": "CopyFromBlobtoSQLAzure",
                        "description": "Push data tooSql Azure",        
                        "type": "CopyActivity",
                        "inputs": [ {"name": "OutputBlobTable"} ],
                        "outputs": [ {"name": "OutputSQLAzureTable"} ],
                        "transformation":
                        {
                            "source":
                            {                               
                                "type": "BlobSource"
                            },
                            "sink":
                            {
                                "type": "SqlSink",
                                "WriteBatchTimeout": "00:5:00",                
                            }            
                        },
                        "Policy":
                        {
                            "concurrency": 3,
                            "executionPriorityOrder": "NewestFirst",
                            "style": "StartOfInterval",
                            "retry": 2,
                            "timeout": "02:00:00"
                        }
                     }
                ]
            }
        }

<span data-ttu-id="831b9-222">Másolja a JSON-definícióból hello folyamatának nevű fájlba *pipelinedef.json* fájlt, és mentse ismert hely tooa (Itt feltételezett toobe *C:\temp\pipelinedef.json*).</span><span class="sxs-lookup"><span data-stu-id="831b9-222">Copy this JSON definition of hello pipeline into a file called *pipelinedef.json* file and save it tooa known location (here assumed toobe *C:\temp\pipelinedef.json*).</span></span> <span data-ttu-id="831b9-223">Hozzon létre hello folyamat az ADF hello Azure PowerShell-parancsmag a következő:</span><span class="sxs-lookup"><span data-stu-id="831b9-223">Create hello pipeline in ADF with hello following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

<span data-ttu-id="831b9-224">Ellenőrizze, hogy is látható hello csővezeték hello a klasszikus Azure portál hello ADF jelenik meg a következőképpen (kattintást hello diagram)</span><span class="sxs-lookup"><span data-stu-id="831b9-224">Confirm that you can see hello pipeline on hello ADF in hello Azure Classic Portal show up as following (when you click hello diagram)</span></span>

![Az ADF-feldolgozási folyamat](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <span data-ttu-id="831b9-226"><a name="adf-pipeline-start"></a>Hello folyamat elindítása</span><span class="sxs-lookup"><span data-stu-id="831b9-226"><a name="adf-pipeline-start"></a>Start hello Pipeline</span></span>
<span data-ttu-id="831b9-227">hello csővezeték futtatható a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="831b9-227">hello pipeline can now be run using hello following command:</span></span>

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

<span data-ttu-id="831b9-228">Hello *startdate* és *enddate* paraméterértékeket kell írni a hello tényleges dátumok, amelyek között hello adatcsatorna toorun kívánt toobe.</span><span class="sxs-lookup"><span data-stu-id="831b9-228">hello *startdate* and *enddate* parameter values need toobe replaced with hello actual dates between which you want hello pipeline toorun.</span></span>

<span data-ttu-id="831b9-229">Hello folyamat végrehajtása során, ha meg tudja toosee hello adatok megjelennek hello tárolóban hello BLOB, naponta egy fájl kiválasztott kell lennie.</span><span class="sxs-lookup"><span data-stu-id="831b9-229">Once hello pipeline executes, you should be able toosee hello data show up in hello container selected for hello blob, one file per day.</span></span>

<span data-ttu-id="831b9-230">Vegye figyelembe, hogy a jelenleg nem alkalmazhatók az hello funkcióit ADF toopipe adatok Növekményesen rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="831b9-230">Note that we have not leveraged hello functionality provided by ADF toopipe data incrementally.</span></span> <span data-ttu-id="831b9-231">További információ a hogyan toodo ezzel és más ADF, által biztosított képességek: hello [ADF dokumentáció](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="831b9-231">For more information on how toodo this and other capabilities provided by ADF, see hello [ADF documentation](https://azure.microsoft.com/services/data-factory/).</span></span>
