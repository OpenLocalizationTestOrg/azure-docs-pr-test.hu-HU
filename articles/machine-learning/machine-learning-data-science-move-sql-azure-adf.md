---
title: "Adatok áthelyezése egy helyi SQL Server az SQL Azure-bA az Azure Data Factoryvel |} Microsoft Docs"
description: "Állítsa be az ADF-feldolgozási folyamat composes adatok áttelepítési két tevékenység váltó együtt adatok naponta helyszíni adatbázisok között, és a felhőben."
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
ms.openlocfilehash: 39fe26d3388be8b558f05063a8965889c013a41e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="move-data-from-an-on-premises-sql-server-to-sql-azure-with-azure-data-factory"></a><span data-ttu-id="3e798-103">Adatok áthelyezése egy helyi SQL server SQL Azure és az Azure Data Factory</span><span class="sxs-lookup"><span data-stu-id="3e798-103">Move data from an on-premises SQL server to SQL Azure with Azure Data Factory</span></span>
<span data-ttu-id="3e798-104">Ez a témakör bemutatja, hogyan tárolt adatok mozgatása egy helyi SQL Server-adatbázis SQL Azure adatbázishoz keresztül Azure Blob Storage használata az Azure Data Factory (ADF).</span><span class="sxs-lookup"><span data-stu-id="3e798-104">This topic shows how to move data from an on-premises SQL Server Database to a SQL Azure Database via Azure Blob Storage using the Azure Data Factory (ADF).</span></span>

<span data-ttu-id="3e798-105">Az egy táblázatot, amely összefoglalja az adatok áthelyezése az Azure SQL Database különböző beállítások [adatok áthelyezése az Azure SQL Database az Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3e798-105">For a table that summarizes various options for moving data to an Azure SQL Database, see [Move data to an Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md).</span></span>

## <span data-ttu-id="3e798-106"><a name="intro"></a>Bemutatása: Mi az az ADF, és amikor azt használatával telepítse át az adatokat?</span><span class="sxs-lookup"><span data-stu-id="3e798-106"><a name="intro"></a>Introduction: What is ADF and when should it be used to migrate data?</span></span>
<span data-ttu-id="3e798-107">Az Azure Data Factory koordinálja és automatizálja az adatátviteli és az adatok átalakítása felhőalapú teljes körűen felügyelt adatok integrációs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="3e798-107">Azure Data Factory is a fully managed cloud-based data integration service that orchestrates and automates the movement and transformation of data.</span></span> <span data-ttu-id="3e798-108">A kulcs az ADF modell fogalma folyamat.</span><span class="sxs-lookup"><span data-stu-id="3e798-108">The key concept in the ADF model is pipeline.</span></span> <span data-ttu-id="3e798-109">Egy folyamat tevékenységek, logikai csoportját, amelyek mindegyike meghatározza, hogy a műveletek végrehajtását az adatkészletek szereplő adatokat.</span><span class="sxs-lookup"><span data-stu-id="3e798-109">A pipeline is a logical grouping of Activities, each of which defines the actions to perform on the data contained in Datasets.</span></span> <span data-ttu-id="3e798-110">Társított szolgáltatások segítségével határozza meg a Data Factory az adatok erőforrások eléréséhez szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="3e798-110">Linked services are used to define the information needed for Data Factory to connect to the data resources.</span></span>

<span data-ttu-id="3e798-111">Az ADF meglévő adatfeldolgozási szolgáltatások összeállítható az adatok folyamatok, amelyek magas rendelkezésre állású és kezelt a felhőben.</span><span class="sxs-lookup"><span data-stu-id="3e798-111">With ADF, existing data processing services can be composed into data pipelines that are highly available and managed in the cloud.</span></span> <span data-ttu-id="3e798-112">Ezen adatok folyamatok ütemezhető a betöltési, előkészítése, átalakítására, elemzése és adatok közzétételére, és az ADF kezeli, és az összetett adatokat és a feldolgozási függőségek koordinálja.</span><span class="sxs-lookup"><span data-stu-id="3e798-112">These data pipelines can be scheduled to ingest, prepare, transform, analyze, and publish data, and ADF manages and orchestrates the complex data and processing dependencies.</span></span> <span data-ttu-id="3e798-113">Megoldások gyorsan kell a beépített és csatlakozás helyszíni egyre több telepített a felhőben és a felhő tárolt adatforrások.</span><span class="sxs-lookup"><span data-stu-id="3e798-113">Solutions can be quickly built and deployed in the cloud, connecting a growing number of on-premises and cloud data sources.</span></span>

<span data-ttu-id="3e798-114">Érdemes lehet ADF:</span><span class="sxs-lookup"><span data-stu-id="3e798-114">Consider using ADF:</span></span>

* <span data-ttu-id="3e798-115">Ha folyamatosan áttelepítendő adatokat egy hibrid forgatókönyvben, amely mindkettőt fér hozzá a helyszíni és felhőalapú erőforrások</span><span class="sxs-lookup"><span data-stu-id="3e798-115">when data needs to be continually migrated in a hybrid scenario that accesses both on-premises and cloud resources</span></span>
* <span data-ttu-id="3e798-116">Ha az adatok tranzakcióalapú van, vagy módosítani vagy üzleti logikát, ha az áttelepítés alatt álló hozzáadott rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3e798-116">when the data is transacted or needs to be modified or have business logic added to it when being migrated.</span></span>

<span data-ttu-id="3e798-117">ADF lehetővé teszi, hogy az ütemezés és rendszeres időközönként adatok mozgása kezelő egyszerű JSON-parancsfájlokat használó feladatok figyelését.</span><span class="sxs-lookup"><span data-stu-id="3e798-117">ADF allows for the scheduling and monitoring of jobs using simple JSON scripts that manage the movement of data on a periodic basis.</span></span> <span data-ttu-id="3e798-118">ADF is rendelkeznek egyéb képességeit, például a összetett műveletek támogatása.</span><span class="sxs-lookup"><span data-stu-id="3e798-118">ADF also has other capabilities such as support for complex operations.</span></span> <span data-ttu-id="3e798-119">Az ADF további információkért lásd a dokumentációban a [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="3e798-119">For more information on ADF, see the documentation at [Azure Data Factory (ADF)](https://azure.microsoft.com/services/data-factory/).</span></span>

## <span data-ttu-id="3e798-120"><a name="scenario"></a>A forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="3e798-120"><a name="scenario"></a>The Scenario</span></span>
<span data-ttu-id="3e798-121">Az ADF-feldolgozási folyamat két adatok áttelepítési tevékenységek composes beállítjuk.</span><span class="sxs-lookup"><span data-stu-id="3e798-121">We set up an ADF pipeline that composes two data migration activities.</span></span> <span data-ttu-id="3e798-122">Együtt mozognak adatok naponta egy helyi SQL-adatbázis és a felhőben Azure SQL adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="3e798-122">Together they move data on a daily basis between an on-premises SQL database and an Azure SQL Database in the cloud.</span></span> <span data-ttu-id="3e798-123">A két tevékenység a következők:</span><span class="sxs-lookup"><span data-stu-id="3e798-123">The two activities are:</span></span>

* <span data-ttu-id="3e798-124">a helyszíni SQL Server adatbázisból származó adatok másolása az Azure Blob Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="3e798-124">copy data from an on-premises SQL Server database to an Azure Blob Storage account</span></span>
* <span data-ttu-id="3e798-125">adatok másolása az Azure Blob Storage-fiók egy Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="3e798-125">copy data from the Azure Blob Storage account to an Azure SQL Database.</span></span>

> [!NOTE]
> <span data-ttu-id="3e798-126">Itt volt módosítani a részletesebb oktatóanyagot, az ADF-csapat által biztosított bemutatott lépések: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) témakör vonatkozó szakaszaihoz-hivatkozások megadott, amikor szükséges.</span><span class="sxs-lookup"><span data-stu-id="3e798-126">The steps shown here have been adapted from the more detailed tutorial provided by the ADF team: [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md) References to the relevant sections of that topic are provided when appropriate.</span></span>
>
>

## <span data-ttu-id="3e798-127"><a name="prereqs"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3e798-127"><a name="prereqs"></a>Prerequisites</span></span>
<span data-ttu-id="3e798-128">Ez az oktatóanyag feltételezi, hogy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="3e798-128">This tutorial assumes you have:</span></span>

* <span data-ttu-id="3e798-129">Egy **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="3e798-129">An **Azure subscription**.</span></span> <span data-ttu-id="3e798-130">Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3e798-130">If you do not have a subscription, you can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="3e798-131">Egy **Azure storage-fiók**.</span><span class="sxs-lookup"><span data-stu-id="3e798-131">An **Azure storage account**.</span></span> <span data-ttu-id="3e798-132">Egy Azure storage-fiókot használ az adatok tárolása az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="3e798-132">You use an Azure storage account for storing the data in this tutorial.</span></span> <span data-ttu-id="3e798-133">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk.</span><span class="sxs-lookup"><span data-stu-id="3e798-133">If you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article.</span></span> <span data-ttu-id="3e798-134">Miután létrehozta a tárfiókot, be kell szereznie a tároló elérésére használt fiókot kulcs.</span><span class="sxs-lookup"><span data-stu-id="3e798-134">After you have created the storage account, you need to obtain the account key used to access the storage.</span></span> <span data-ttu-id="3e798-135">Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span><span class="sxs-lookup"><span data-stu-id="3e798-135">See [Manage your storage access keys](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).</span></span>
* <span data-ttu-id="3e798-136">A hozzáférést egy **Azure SQL adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="3e798-136">Access to an **Azure SQL Database**.</span></span> <span data-ttu-id="3e798-137">Ha be kell állítania egy Azure SQL Database, a tpoic [Ismerkedés a Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) bemutatja, hogy miként lehet kiépíteni egy Azure SQL adatbázis új példányát.</span><span class="sxs-lookup"><span data-stu-id="3e798-137">If you must set up an Azure SQL Database, the tpoic [Getting Started with Microsoft Azure SQL Database ](../sql-database/sql-database-get-started.md) provides information on how to provision a new instance of an Azure SQL Database.</span></span>
* <span data-ttu-id="3e798-138">Telepített és konfigurált **Azure PowerShell** helyileg.</span><span class="sxs-lookup"><span data-stu-id="3e798-138">Installed and configured **Azure PowerShell** locally.</span></span> <span data-ttu-id="3e798-139">Útmutatásért lásd: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3e798-139">For instructions, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

> [!NOTE]
> <span data-ttu-id="3e798-140">Ez az eljárás használja a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="3e798-140">This procedure uses the [Azure portal](https://portal.azure.com/).</span></span>
>
>

## <span data-ttu-id="3e798-141"><a name="upload-data"></a>Az adatok feltöltése a helyszíni SQL Server</span><span class="sxs-lookup"><span data-stu-id="3e798-141"><a name="upload-data"></a> Upload the data to your on-premises SQL Server</span></span>
<span data-ttu-id="3e798-142">Használjuk a [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) az áttelepítési folyamat bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="3e798-142">We use the [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/) to demonstrate the migration process.</span></span> <span data-ttu-id="3e798-143">A következőt: Taxi adatkészlet érhető el, a feladás egy vagy több, az Azure blob storage leírtaknak megfelelően [NYC Taxi adatok](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="3e798-143">The NYC Taxi dataset is available, as noted in that post, on Azure blob storage [NYC Taxi Data](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="3e798-144">Az adatok van két fájlt, a trip_data.csv fájlt, amely tartalmazza-e vissza adatokat, és a trip_far.csv fájlt, amely a jegy minden út kifizette ára részleteit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3e798-144">The data has two files, the trip_data.csv file, which contains trip details, and the  trip_far.csv file, which contains details of the fare paid for each trip.</span></span> <span data-ttu-id="3e798-145">A minta és az ezek a fájlok leírása szerepelnek [NYC Taxi Utazgatással adatkészlet leírása](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span><span class="sxs-lookup"><span data-stu-id="3e798-145">A sample and description of these files are provided in [NYC Taxi Trips Dataset Description](machine-learning-data-science-process-sql-walkthrough.md#dataset).</span></span>

<span data-ttu-id="3e798-146">Az itt megadott saját adatok eljárás igazítja, vagy hajtsa végre a következőt: Taxi adatkészlet szerint.</span><span class="sxs-lookup"><span data-stu-id="3e798-146">You can either adapt the procedure provided here to a set of your own data or follow the steps as described by using the NYC Taxi dataset.</span></span> <span data-ttu-id="3e798-147">Töltse fel a következőt: Taxi dataset a helyszíni SQL Server-adatbázisba, kövesse az ismertetett eljárás [tömeges adatok importálása az SQL Server-adatbázisba](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span><span class="sxs-lookup"><span data-stu-id="3e798-147">To upload the NYC Taxi dataset into your on-premises SQL Server database, follow the procedure outlined in [Bulk Import Data into SQL Server Database](machine-learning-data-science-process-sql-walkthrough.md#dbload).</span></span> <span data-ttu-id="3e798-148">Ezek az utasítások az Azure virtuális gép az SQL Server, de a helyszíni SQL Server feltöltését eljárás megegyezik.</span><span class="sxs-lookup"><span data-stu-id="3e798-148">These instructions are for a SQL Server on an Azure Virtual Machine, but the procedure for uploading to the on-premises SQL Server is the same.</span></span>

## <span data-ttu-id="3e798-149"><a name="create-adf"></a>Egy Azure Data Factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e798-149"><a name="create-adf"></a> Create an Azure Data Factory</span></span>
<span data-ttu-id="3e798-150">Egy új Azure Data Factory és az erőforráscsoport létrehozására vonatkozó utasításokat a [Azure-portálon](https://portal.azure.com/) biztosított [hozzon létre egy Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span><span class="sxs-lookup"><span data-stu-id="3e798-150">The instructions for creating a new Azure Data Factory and a resource group in the [Azure portal](https://portal.azure.com/) are provided [Create an Azure Data Factory](../data-factory/data-factory-build-your-first-pipeline-using-editor.md#create-data-factory).</span></span> <span data-ttu-id="3e798-151">Az új ADF-példány neve *adfdsp* és az erőforráscsoport létrehozásánál *adfdsprg*.</span><span class="sxs-lookup"><span data-stu-id="3e798-151">Name the new ADF instance *adfdsp* and name the resource group created *adfdsprg*.</span></span>

## <a name="install-and-configure-up-the-data-management-gateway"></a><span data-ttu-id="3e798-152">Telepítse és konfigurálja az adatkezelési átjáró mentése</span><span class="sxs-lookup"><span data-stu-id="3e798-152">Install and configure up the Data Management Gateway</span></span>
<span data-ttu-id="3e798-153">Ahhoz, hogy az egy az Azure data factory szeretne dolgozni egy helyi SQL Server adatcsatornák, szüksége csatolt szolgáltatásként hozzáadása az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="3e798-153">To enable your pipelines in an Azure data factory to work with an on-premises SQL Server, you need to add it as a Linked Service to the data factory.</span></span> <span data-ttu-id="3e798-154">A társított szolgáltatás létrehozása egy helyi SQL Server, a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="3e798-154">To create a Linked Service for an on-premises SQL Server, you must:</span></span>

* <span data-ttu-id="3e798-155">Töltse le és telepítse a Microsoft adatkezelési átjáró a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="3e798-155">download and install Microsoft Data Management Gateway onto the on-premises computer.</span></span>
* <span data-ttu-id="3e798-156">a társított szolgáltatás a helyszíni adatforráshoz az átjáró használatára konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="3e798-156">configure the linked service for the on-premises data source to use the gateway.</span></span>

<span data-ttu-id="3e798-157">Az adatkezelési átjáró rendezi sorba, és a forrás és a fogadó adatokat azon a számítógépen, amelyen található deserializes.</span><span class="sxs-lookup"><span data-stu-id="3e798-157">The Data Management Gateway serializes and deserializes the source and sink data on the computer where it is hosted.</span></span>

<span data-ttu-id="3e798-158">Telepítési utasításokat és az adatkezelési átjáró részleteinek: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró felhő között](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="3e798-158">For set-up instructions and details on Data Management Gateway, see [Move data between on-premises sources and cloud with Data Management Gateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md)</span></span>

## <span data-ttu-id="3e798-159"><a name="adflinkedservices"></a>Az adatok erőforrásokhoz való társított szolgáltatások létrehozásához</span><span class="sxs-lookup"><span data-stu-id="3e798-159"><a name="adflinkedservices"></a>Create linked services to connect to the data resources</span></span>
<span data-ttu-id="3e798-160">A társított szolgáltatás határozza meg az Azure Data Factory egy adatforrás, melyhez való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="3e798-160">A linked service defines the information needed for Azure Data Factory to connect to a data resource.</span></span> <span data-ttu-id="3e798-161">Lépésről lépésre társított szolgáltatások létrehozásához megadott [társított szolgáltatások létrehozásához](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span><span class="sxs-lookup"><span data-stu-id="3e798-161">The step-by-step procedure for creating linked services is provided in [Create linked services](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-linked-services).</span></span>

<span data-ttu-id="3e798-162">Három erőforrások van ebben a forgatókönyvben összekapcsolt szolgáltatások van szükség.</span><span class="sxs-lookup"><span data-stu-id="3e798-162">We have three resources in this scenario for which linked services are needed.</span></span>

1. [<span data-ttu-id="3e798-163">A helyszíni SQL Server társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e798-163">Linked service for on-premises SQL Server</span></span>](#adf-linked-service-onprem-sql)
2. [<span data-ttu-id="3e798-164">Az Azure Blob Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e798-164">Linked service for Azure Blob Storage</span></span>](#adf-linked-service-blob-store)
3. [<span data-ttu-id="3e798-165">Az Azure SQL database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e798-165">Linked service for Azure SQL database</span></span>](#adf-linked-service-azure-sql)

### <span data-ttu-id="3e798-166"><a name="adf-linked-service-onprem-sql"></a>A helyszíni SQL Server-adatbázis társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e798-166"><a name="adf-linked-service-onprem-sql"></a>Linked service for on-premises SQL Server database</span></span>
<span data-ttu-id="3e798-167">A helyszíni SQL Server a társított szolgáltatás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3e798-167">To create the linked service for the on-premises SQL Server:</span></span>

* <span data-ttu-id="3e798-168">Kattintson a **adattár** a klasszikus Azure portálon az ADF kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="3e798-168">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="3e798-169">Válassza ki **SQL** , és írja be a *felhasználónév* és *jelszó* a helyszíni SQL-kiszolgálóhoz tartozó hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="3e798-169">select **SQL** and enter the *username* and *password* credentials for the on-premises SQL Server.</span></span> <span data-ttu-id="3e798-170">Meg kell adnia a kiszolgálónév, mint egy **teljesen minősített kiszolgálónév fordított perjel példány neve (kiszolgáló_neve\példány_neve)**.</span><span class="sxs-lookup"><span data-stu-id="3e798-170">You need to enter the servername as a **fully qualified servername backslash instance name (servername\instancename)**.</span></span> <span data-ttu-id="3e798-171">A társított szolgáltatás neve *adfonpremsql*.</span><span class="sxs-lookup"><span data-stu-id="3e798-171">Name the linked service *adfonpremsql*.</span></span>

### <span data-ttu-id="3e798-172"><a name="adf-linked-service-blob-store"></a>A Blob társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e798-172"><a name="adf-linked-service-blob-store"></a>Linked service for Blob</span></span>
<span data-ttu-id="3e798-173">A társított szolgáltatás az Azure Blob Storage-fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3e798-173">To create the linked service for the Azure Blob Storage account:</span></span>

* <span data-ttu-id="3e798-174">Kattintson a **adattár** a klasszikus Azure portálon az ADF kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="3e798-174">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="3e798-175">Válassza ki **Azure Storage-fiók**</span><span class="sxs-lookup"><span data-stu-id="3e798-175">select **Azure Storage Account**</span></span>
* <span data-ttu-id="3e798-176">Írja be a Azure Blob Storage-fiók és tároló neve.</span><span class="sxs-lookup"><span data-stu-id="3e798-176">enter the Azure Blob Storage account key and container name.</span></span> <span data-ttu-id="3e798-177">A társított szolgáltatás neve *adfds*.</span><span class="sxs-lookup"><span data-stu-id="3e798-177">Name the Linked Service *adfds*.</span></span>

### <span data-ttu-id="3e798-178"><a name="adf-linked-service-azure-sql"></a>Az Azure SQL database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3e798-178"><a name="adf-linked-service-azure-sql"></a>Linked service for Azure SQL database</span></span>
<span data-ttu-id="3e798-179">A társított szolgáltatás az Azure SQL-adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3e798-179">To create the linked service for the Azure SQL Database:</span></span>

* <span data-ttu-id="3e798-180">Kattintson a **adattár** a klasszikus Azure portálon az ADF kezdőlapja</span><span class="sxs-lookup"><span data-stu-id="3e798-180">click the **Data Store** in the ADF landing page on Azure Classic Portal</span></span>
* <span data-ttu-id="3e798-181">Válassza ki **Azure SQL** , és írja be a *felhasználónév* és *jelszó* az Azure SQL-adatbázishoz tartozó hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="3e798-181">select **Azure SQL** and enter the *username* and *password* credentials for the Azure SQL Database.</span></span> <span data-ttu-id="3e798-182">A *felhasználónév* kell megadni,  *user@servername* .</span><span class="sxs-lookup"><span data-stu-id="3e798-182">The *username* must be specified as *user@servername*.</span></span>   

## <span data-ttu-id="3e798-183"><a name="adf-tables"></a>Adja meg, és adja meg, hogyan férhet hozzá az adatkészletek táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e798-183"><a name="adf-tables"></a>Define and create tables to specify how to access the datasets</span></span>
<span data-ttu-id="3e798-184">Adja meg a struktúra, helyét és az adatkészletek rendelkezésre állását az alábbi parancsfájl-alapú eljárások táblák létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3e798-184">Create tables that specify the structure, location, and availability of the datasets with the following script-based procedures.</span></span> <span data-ttu-id="3e798-185">JSON-fájlokat a táblák meghatározásához használják.</span><span class="sxs-lookup"><span data-stu-id="3e798-185">JSON files are used to define the tables.</span></span> <span data-ttu-id="3e798-186">Ezek a fájlok szerkezetének további információkért lásd: [adatkészletek](../data-factory/data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="3e798-186">For more information on the structure of these files, see [Datasets](../data-factory/data-factory-create-datasets.md).</span></span>

> [!NOTE]
> <span data-ttu-id="3e798-187">Végre kell hajtani a `Add-AzureAccount` parancsmag végrehajtása előtt a [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) parancsmag segítségével győződjön meg arról, hogy a jobb oldali Azure-előfizetés parancs végrehajtásának van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="3e798-187">You should execute the `Add-AzureAccount` cmdlet before executing the [New-AzureDataFactoryTable](https://msdn.microsoft.com/library/azure/dn835096.aspx) cmdlet to confirm that the right Azure subscription is selected for the command execution.</span></span> <span data-ttu-id="3e798-188">Ez a parancsmag dokumentációjáért lásd: [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span><span class="sxs-lookup"><span data-stu-id="3e798-188">For documentation of this cmdlet, see [Add-AzureAccount](/powershell/module/azure/add-azureaccount?view=azuresmps-3.7.0).</span></span>
>
>

<span data-ttu-id="3e798-189">A JSON-alapú definíciók táblázatokban az alábbi neveket használja:</span><span class="sxs-lookup"><span data-stu-id="3e798-189">The JSON-based definitions in the tables use the following names:</span></span>

* <span data-ttu-id="3e798-190">a **táblanév** a helyszíni SQL server rendszer *nyctaxi_data*</span><span class="sxs-lookup"><span data-stu-id="3e798-190">the **table name** in the on-premises SQL server is *nyctaxi_data*</span></span>
* <span data-ttu-id="3e798-191">a **Tárolónév** az Azure Blob Storage fiók van *containername*</span><span class="sxs-lookup"><span data-stu-id="3e798-191">the **container name** in the Azure Blob Storage account is *containername*</span></span>  

<span data-ttu-id="3e798-192">Három definíciói az ADF adatcsatorna van szükség:</span><span class="sxs-lookup"><span data-stu-id="3e798-192">Three table definitions are needed for this ADF pipeline:</span></span>

1. [<span data-ttu-id="3e798-193">A helyszíni SQL-tábla</span><span class="sxs-lookup"><span data-stu-id="3e798-193">SQL on-premises Table</span></span>](#adf-table-onprem-sql)
2. [<span data-ttu-id="3e798-194">A BLOB tábla</span><span class="sxs-lookup"><span data-stu-id="3e798-194">Blob Table </span></span>](#adf-table-blob-store)
3. [<span data-ttu-id="3e798-195">Az SQL Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="3e798-195">SQL Azure Table</span></span>](#adf-table-azure-sql)

> [!NOTE]
> <span data-ttu-id="3e798-196">Ezek az eljárások Azure PowerShell használatával határozza meg, és az ADF tevékenységek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3e798-196">These procedures use Azure PowerShell to define and create the ADF activities.</span></span> <span data-ttu-id="3e798-197">De ezek a feladatok az Azure portál használatával is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="3e798-197">But these tasks can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="3e798-198">További információkért lásd: [adatkészletek létrehozása](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span><span class="sxs-lookup"><span data-stu-id="3e798-198">For details, see [Create datasets](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-datasets).</span></span>
>
>

### <span data-ttu-id="3e798-199"><a name="adf-table-onprem-sql"></a>A helyszíni SQL-tábla</span><span class="sxs-lookup"><span data-stu-id="3e798-199"><a name="adf-table-onprem-sql"></a>SQL on-premises Table</span></span>
<span data-ttu-id="3e798-200">A helyszíni SQL Server-definíció van megadva a következő JSON-fájlban:</span><span class="sxs-lookup"><span data-stu-id="3e798-200">The table definition for the on-premises SQL Server is specified in the following JSON file:</span></span>

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

<span data-ttu-id="3e798-201">Az oszlopnevek nem szerepeltek itt.</span><span class="sxs-lookup"><span data-stu-id="3e798-201">The column names were not included here.</span></span> <span data-ttu-id="3e798-202">Részterv jelölheti meg az oszlopok neveit itt többek között (a részleteket tekintse meg a [ADF dokumentáció](../data-factory/data-factory-data-movement-activities.md) témakör.</span><span class="sxs-lookup"><span data-stu-id="3e798-202">You can sub-select on the column names by including them here (for details check the [ADF documentation](../data-factory/data-factory-data-movement-activities.md) topic.</span></span>

<span data-ttu-id="3e798-203">Másolás fájlba a tábla JSON-definícióból nevű *onpremtabledef.json* fájlt, és mentse egy ismert helyre (Itt feltételezhető *C:\temp\onpremtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="3e798-203">Copy the JSON definition of the table into a file called *onpremtabledef.json* file and save it to a known location (here assumed to be *C:\temp\onpremtabledef.json*).</span></span> <span data-ttu-id="3e798-204">A tábla létrehozása az ADF a következő Azure PowerShell-parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="3e798-204">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp –File C:\temp\onpremtabledef.json


### <span data-ttu-id="3e798-205"><a name="adf-table-blob-store"></a>A BLOB tábla</span><span class="sxs-lookup"><span data-stu-id="3e798-205"><a name="adf-table-blob-store"></a>Blob Table</span></span>
<span data-ttu-id="3e798-206">A következő táblázatban a kimeneti blob helyére vonatkozó definíciójának szerepel (Ez leképezi a feldolgozott adatokat a helyszíni Azure blob) a következő:</span><span class="sxs-lookup"><span data-stu-id="3e798-206">Definition for the table for the output blob location is in the following (this maps the ingested data from on-premises to Azure blob):</span></span>

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

<span data-ttu-id="3e798-207">Másolás fájlba a tábla JSON-definícióból nevű *bloboutputtabledef.json* fájlt, és mentse egy ismert helyre (Itt feltételezhető *C:\temp\bloboutputtabledef.json*).</span><span class="sxs-lookup"><span data-stu-id="3e798-207">Copy the JSON definition of the table into a file called *bloboutputtabledef.json* file and save it to a known location (here assumed to be *C:\temp\bloboutputtabledef.json*).</span></span> <span data-ttu-id="3e798-208">A tábla létrehozása az ADF a következő Azure PowerShell-parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="3e798-208">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\bloboutputtabledef.json  

### <span data-ttu-id="3e798-209"><a name="adf-table-azure-sq"></a>Az SQL Azure-tábla</span><span class="sxs-lookup"><span data-stu-id="3e798-209"><a name="adf-table-azure-sq"></a>SQL Azure Table</span></span>
<span data-ttu-id="3e798-210">Definíciója a következő táblázatban az SQL Azure kimeneti (ebben a sémában az adatokat a blob érkező képezi le) a következőket:</span><span class="sxs-lookup"><span data-stu-id="3e798-210">Definition for the table for the SQL Azure output is in the following (this schema maps the data coming from the blob):</span></span>

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

<span data-ttu-id="3e798-211">Másolás fájlba a tábla JSON-definícióból nevű *AzureSqlTable.json* fájlt, és mentse egy ismert helyre (Itt feltételezhető *C:\temp\AzureSqlTable.json*).</span><span class="sxs-lookup"><span data-stu-id="3e798-211">Copy the JSON definition of the table into a file called *AzureSqlTable.json* file and save it to a known location (here assumed to be *C:\temp\AzureSqlTable.json*).</span></span> <span data-ttu-id="3e798-212">A tábla létrehozása az ADF a következő Azure PowerShell-parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="3e798-212">Create the table in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryTable -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\AzureSqlTable.json  


## <span data-ttu-id="3e798-213"><a name="adf-pipeline"></a>Adja meg, és a folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e798-213"><a name="adf-pipeline"></a>Define and create the pipeline</span></span>
<span data-ttu-id="3e798-214">Adja meg az adatcsatornához tartozó, és hozzon létre a folyamat a következő parancsprogram-alapú eljárásokkal a tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="3e798-214">Specify the activities that belong to the pipeline and create the pipeline with the following script-based procedures.</span></span> <span data-ttu-id="3e798-215">A JSON-fájl folyamat tulajdonságainak definiálásához szolgál.</span><span class="sxs-lookup"><span data-stu-id="3e798-215">A JSON file is used to define the pipeline properties.</span></span>

* <span data-ttu-id="3e798-216">A parancsfájl feltételezi, hogy a **adatcsatorna neve** van *AMLDSProcessPipeline*.</span><span class="sxs-lookup"><span data-stu-id="3e798-216">The script assumes that the **pipeline name** is *AMLDSProcessPipeline*.</span></span>
* <span data-ttu-id="3e798-217">Ne feledje, hogy be van állítva az ismétlődési gyakoriságára vonatkozóan a feldolgozási sor napi szinten hajtható végre, és az alapértelmezett végrehajtási idő használata a feladathoz (12 óra UTC).</span><span class="sxs-lookup"><span data-stu-id="3e798-217">Also note that we set the periodicity of the pipeline to be executed on daily basis and use the default execution time for the job (12 am UTC).</span></span>

> [!NOTE]
> <span data-ttu-id="3e798-218">Az alábbi eljárások Azure PowerShell használatával határozza meg, és hozzon létre a ADF-feldolgozási folyamat.</span><span class="sxs-lookup"><span data-stu-id="3e798-218">The following procedures use Azure PowerShell to define and create the ADF pipeline.</span></span> <span data-ttu-id="3e798-219">Azonban ez a feladat az Azure portál használatával is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="3e798-219">But this task can also be accomplished using the Azure portal.</span></span> <span data-ttu-id="3e798-220">További információkért lásd: [létrehozási folyamat](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span><span class="sxs-lookup"><span data-stu-id="3e798-220">For details, see [Create pipeline](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#create-pipeline).</span></span>
>
>

<span data-ttu-id="3e798-221">A korábban megadott definíciói használ, az ADF csővezeték definíciója van megadva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="3e798-221">Using the table definitions provided previously, the pipeline definition for the ADF is specified as follows:</span></span>

        {
            "name": "AMLDSProcessPipeline",
            "properties":
            {
                "description" : "This pipeline has one Copy activity that copies data from an on-premises SQL to Azure blob",
                 "activities":
                [
                    {
                        "name": "CopyFromSQLtoBlob",
                        "description": "Copy data from on-premises SQL server to blob",     
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
                        "description": "Push data to Sql Azure",        
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

<span data-ttu-id="3e798-222">A JSON-definícióból a folyamat egy fájlba nevű példány *pipelinedef.json* fájlt, és mentse egy ismert helyre (Itt feltételezhető *C:\temp\pipelinedef.json*).</span><span class="sxs-lookup"><span data-stu-id="3e798-222">Copy this JSON definition of the pipeline into a file called *pipelinedef.json* file and save it to a known location (here assumed to be *C:\temp\pipelinedef.json*).</span></span> <span data-ttu-id="3e798-223">Hozzon létre a folyamat a következő Azure PowerShell-parancsmaggal ADF:</span><span class="sxs-lookup"><span data-stu-id="3e798-223">Create the pipeline in ADF with the following Azure PowerShell cmdlet:</span></span>

    New-AzureDataFactoryPipeline  -ResourceGroupName adfdsprg -DataFactoryName adfdsp -File C:\temp\pipelinedef.json

<span data-ttu-id="3e798-224">Győződjön meg arról, hogy látja-e a folyamat az ADF a klasszikus Azure-portálon a megjelennek, a következő, (Ha a diagram kattint)</span><span class="sxs-lookup"><span data-stu-id="3e798-224">Confirm that you can see the pipeline on the ADF in the Azure Classic Portal show up as following (when you click the diagram)</span></span>

![Az ADF-feldolgozási folyamat](media/machine-learning-data-science-move-sql-azure-adf/DJP1kji.png)

## <span data-ttu-id="3e798-226"><a name="adf-pipeline-start"></a>A folyamat elindítása</span><span class="sxs-lookup"><span data-stu-id="3e798-226"><a name="adf-pipeline-start"></a>Start the Pipeline</span></span>
<span data-ttu-id="3e798-227">A folyamat futtatható a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="3e798-227">The pipeline can now be run using the following command:</span></span>

    Set-AzureDataFactoryPipelineActivePeriod -ResourceGroupName ADFdsprg -DataFactoryName ADFdsp -StartDateTime startdateZ –EndDateTime enddateZ –Name AMLDSProcessPipeline

<span data-ttu-id="3e798-228">A *startdate* és *enddate* paraméterértékeket kell cserélni a tényleges dátum között, amelyek a folyamatot futtatni szeretné.</span><span class="sxs-lookup"><span data-stu-id="3e798-228">The *startdate* and *enddate* parameter values need to be replaced with the actual dates between which you want the pipeline to run.</span></span>

<span data-ttu-id="3e798-229">Ha a feldolgozási sor végrehajtása során, kell megjeleníti őket a tárolóban, a BLOB, naponta egy fájl kiválasztott adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e798-229">Once the pipeline executes, you should be able to see the data show up in the container selected for the blob, one file per day.</span></span>

<span data-ttu-id="3e798-230">Vegye figyelembe, hogy a jelenleg nem alkalmazhatók az által biztosított funkcióknak ADF cső adatok Növekményesen rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3e798-230">Note that we have not leveraged the functionality provided by ADF to pipe data incrementally.</span></span> <span data-ttu-id="3e798-231">Ez, és más ADF által biztosított képességek módjáról további információkért lásd: a [ADF dokumentáció](https://azure.microsoft.com/services/data-factory/).</span><span class="sxs-lookup"><span data-stu-id="3e798-231">For more information on how to do this and other capabilities provided by ADF, see the [ADF documentation](https://azure.microsoft.com/services/data-factory/).</span></span>
