---
title: az SQL Server aaaMove adatok tooand |} Microsoft Docs
description: "Ismerje meg, hogyan toomove adatokat az SQL Server-adatbázis, amely a helyszíni vagy egy Azure virtuális gép Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 864ece28-93b5-4309-9873-b095bbe6fedd
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jingwang
ms.openlocfilehash: f0cccf56a670e62ec893d75052a81eb26d562050
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a><span data-ttu-id="b553d-103">Helyezze át az adatokat tooand a helyszíni SQL Server vagy az infrastruktúra-szolgáltatási (Azure virtuális gép), Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="b553d-103">Move data tooand from SQL Server on-premises or on IaaS (Azure VM) using Azure Data Factory</span></span>
<span data-ttu-id="b553d-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok belőle egy helyi SQL Server-adatbázis a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b553d-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from an on-premises SQL Server database.</span></span> <span data-ttu-id="b553d-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="b553d-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="b553d-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="b553d-106">Supported scenarios</span></span>
<span data-ttu-id="b553d-107">Adatokat másolhat **egy SQL Server-adatbázisból** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="b553d-107">You can copy data **from a SQL Server database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="b553d-108">Adatok másolása a következő adatokat tárolja hello **tooa SQL Server-adatbázis**:</span><span class="sxs-lookup"><span data-stu-id="b553d-108">You can copy data from hello following data stores **tooa SQL Server database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a><span data-ttu-id="b553d-109">Támogatott SQL Server-verziók</span><span class="sxs-lookup"><span data-stu-id="b553d-109">Supported SQL Server versions</span></span>
<span data-ttu-id="b553d-110">Az SQL Server connector támogatása az adatok másolásának / toohello következő helyszínen üzemeltetett példányt, vagy az SQL-hitelesítést és a Windows-hitelesítést használó Azure IaaS verziók: SQL Server 2016, az SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span><span class="sxs-lookup"><span data-stu-id="b553d-110">This SQL Server connector support copying data from/toohello following versions of instance hosted on-premises or in Azure IaaS using both SQL authentication and Windows authentication: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="b553d-111">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b553d-111">Enabling connectivity</span></span>
<span data-ttu-id="b553d-112">hello fogalmakat és a helyszíni SQL Server által futtatott vagy az Azure infrastruktúra-szolgáltatási (infrastruktúra-,--szolgáltatás) virtuális gépek csatlakoztatásához szükséges lépéseket hello azonos történik.</span><span class="sxs-lookup"><span data-stu-id="b553d-112">hello concepts and steps needed for connecting with SQL Server hosted on-premises or in Azure IaaS (Infrastructure-as-a-Service) VMs are hello same.</span></span> <span data-ttu-id="b553d-113">Mindkét esetben szükséges toouse az adatkezelési átjáró kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b553d-113">In both cases, you need toouse Data Management Gateway for connectivity.</span></span>

<span data-ttu-id="b553d-114">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk toolearn az adatkezelési átjáró és hello átjáró beállításával kapcsolatos részletes utasításokat.</span><span class="sxs-lookup"><span data-stu-id="b553d-114">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article toolearn about Data Management Gateway and step-by-step instructions on setting up hello gateway.</span></span> <span data-ttu-id="b553d-115">Kapcsolódás az SQL Server olyan előfeltételt egy átjárópéldányt beállítása.</span><span class="sxs-lookup"><span data-stu-id="b553d-115">Setting up a gateway instance is a pre-requisite for connecting with SQL Server.</span></span>

<span data-ttu-id="b553d-116">Amíg átjárót telepítheti hello megegyezik a helyszíni gépen vagy a felhő Virtuálisgép-példány SQL Server hello a jobb teljesítmény, javasoljuk, hogy külön gépeken telepíteni.</span><span class="sxs-lookup"><span data-stu-id="b553d-116">While you can install gateway on hello same on-premises machine or cloud VM instance as hello SQL Server for better performance, we recommended that you install them on separate machines.</span></span> <span data-ttu-id="b553d-117">Hello átjáró és az SQL Server különböző gépeken csökkenti a Erőforrásverseny.</span><span class="sxs-lookup"><span data-stu-id="b553d-117">Having hello gateway and SQL Server on separate machines reduces resource contention.</span></span>

## <a name="getting-started"></a><span data-ttu-id="b553d-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b553d-118">Getting started</span></span>
<span data-ttu-id="b553d-119">A másolási tevékenység, amely a különböző eszközök/API-k használatával helyezi át az adatokat belőle egy helyi SQL Server-adatbázis egy folyamat hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="b553d-119">You can create a pipeline with a copy activity that moves data to/from an on-premises SQL Server database by using different tools/APIs.</span></span>

<span data-ttu-id="b553d-120">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="b553d-120">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="b553d-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="b553d-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="b553d-122">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="b553d-122">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b553d-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="b553d-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="b553d-124">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="b553d-124">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span> 

1. <span data-ttu-id="b553d-125">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="b553d-125">Create a **data factory**.</span></span> <span data-ttu-id="b553d-126">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="b553d-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="b553d-127">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b553d-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="b553d-128">Például egy SQL Server adatbázis tooan Azure blob-tároló az adatok másolása, létrehozhat két összekapcsolt szolgáltatások toolink az SQL Server-adatbázis és az Azure storage tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b553d-128">For example, if you are copying data from a SQL Server database tooan Azure blob storage, you create two linked services toolink your SQL Server database and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="b553d-129">Csatolt szolgáltatás tulajdonságait, amelyek adott tooSQL Server-adatbázis, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b553d-129">For linked service properties that are specific tooSQL Server database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="b553d-130">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="b553d-130">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="b553d-131">Hello utolsó lépésében említett hello példában dataset toospecify hello SQL tábla hello bemeneti adatokat tartalmazó az SQL Server-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b553d-131">In hello example mentioned in hello last step, you create a dataset toospecify hello SQL table in your SQL Server database that contains hello input data.</span></span> <span data-ttu-id="b553d-132">Egy másik dataset toospecify hello blob tároló létrehozása és a hello adatokat tartalmazó hello mappába másolta át hello SQL Server-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b553d-132">And, you create another dataset toospecify hello blob container and hello folder that holds hello data copied from hello SQL Server database.</span></span> <span data-ttu-id="b553d-133">Adatkészlet tulajdonságai, amelyek adott tooSQL Server-adatbázis, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b553d-133">For dataset properties that are specific tooSQL Server database, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="b553d-134">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="b553d-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="b553d-135">A korábban említett hello példában SqlSource forrás-és BlobSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="b553d-135">In hello example mentioned earlier, you use SqlSource as a source and BlobSink as a sink for hello copy activity.</span></span> <span data-ttu-id="b553d-136">Ehhez hasonlóan az Azure Blob Storage tooSQL Server-adatbázis másolása, használható BlobSource és SqlSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="b553d-136">Similarly, if you are copying from Azure Blob Storage tooSQL Server Database, you use BlobSource and SqlSink in hello copy activity.</span></span> <span data-ttu-id="b553d-137">A másolási tevékenység tulajdonságait, amelyek adott tooSQL Server-adatbázis, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b553d-137">For copy activity properties that are specific tooSQL Server Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="b553d-138">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="b553d-138">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span> 

<span data-ttu-id="b553d-139">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="b553d-139">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="b553d-140">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="b553d-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="b553d-141">Minták használt toocopy adatok egy helyi SQL Server-adatbázis az adat-előállító entitások JSON-definíciók, lásd: [JSON példák](#json-examples-for-copying-data-from-and-to-sql-server) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="b553d-141">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an on-premises SQL Server database, see [JSON examples](#json-examples-for-copying-data-from-and-to-sql-server) section of this article.</span></span> 

<span data-ttu-id="b553d-142">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooSQL Server részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="b553d-142">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooSQL Server:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="b553d-143">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="b553d-143">Linked service properties</span></span>
<span data-ttu-id="b553d-144">Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** toolink egy helyi SQL Server adatbázis tooa adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b553d-144">You create a linked service of type **OnPremisesSqlServer** toolink an on-premises SQL Server database tooa data factory.</span></span> <span data-ttu-id="b553d-145">a következő táblázat hello biztosít JSON elemek adott tooon-hez kapcsolódó SQL Server szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="b553d-145">hello following table provides description for JSON elements specific tooon-premises SQL Server linked service.</span></span>

<span data-ttu-id="b553d-146">a következő táblázat hello biztosít JSON-elemek adott tooSQL csatolt kiszolgáló szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="b553d-146">hello following table provides description for JSON elements specific tooSQL Server linked service.</span></span>

| <span data-ttu-id="b553d-147">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b553d-147">Property</span></span> | <span data-ttu-id="b553d-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="b553d-148">Description</span></span> | <span data-ttu-id="b553d-149">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b553d-149">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b553d-150">type</span><span class="sxs-lookup"><span data-stu-id="b553d-150">type</span></span> |<span data-ttu-id="b553d-151">hello tulajdonságra kell megadni: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="b553d-151">hello type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="b553d-152">Igen</span><span class="sxs-lookup"><span data-stu-id="b553d-152">Yes</span></span> |
| <span data-ttu-id="b553d-153">connectionString</span><span class="sxs-lookup"><span data-stu-id="b553d-153">connectionString</span></span> |<span data-ttu-id="b553d-154">Adja meg a connectionString információ tooconnect toohello a helyszíni SQL Server-adatbázis SQL-hitelesítéssel vagy a Windows-hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="b553d-154">Specify connectionString information needed tooconnect toohello on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="b553d-155">Igen</span><span class="sxs-lookup"><span data-stu-id="b553d-155">Yes</span></span> |
| <span data-ttu-id="b553d-156">gatewayName</span><span class="sxs-lookup"><span data-stu-id="b553d-156">gatewayName</span></span> |<span data-ttu-id="b553d-157">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello a helyszíni SQL Server-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="b553d-157">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SQL Server database.</span></span> |<span data-ttu-id="b553d-158">Igen</span><span class="sxs-lookup"><span data-stu-id="b553d-158">Yes</span></span> |
| <span data-ttu-id="b553d-159">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="b553d-159">username</span></span> |<span data-ttu-id="b553d-160">Adja meg a felhasználónevet, ha a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="b553d-160">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="b553d-161">Példa: **tartománynév\\felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="b553d-161">Example: **domainname\\username**.</span></span> |<span data-ttu-id="b553d-162">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-162">No</span></span> |
| <span data-ttu-id="b553d-163">jelszó</span><span class="sxs-lookup"><span data-stu-id="b553d-163">password</span></span> |<span data-ttu-id="b553d-164">Adja meg a megadott felhasználónévhez hello hello felhasználói fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="b553d-164">Specify password for hello user account you specified for hello username.</span></span> |<span data-ttu-id="b553d-165">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-165">No</span></span> |

<span data-ttu-id="b553d-166">Hitelesítő adatok hello segítségével titkosíthatja **New-AzureRmDataFactoryEncryptValue** parancsmag, amelyekkel hello kapcsolati karakterlánc, ahogy az alábbi példa hello (**EncryptedCredential** tulajdonság):</span><span class="sxs-lookup"><span data-stu-id="b553d-166">You can encrypt credentials using hello **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in hello connection string as shown in hello following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a><span data-ttu-id="b553d-167">Példák</span><span class="sxs-lookup"><span data-stu-id="b553d-167">Samples</span></span>
<span data-ttu-id="b553d-168">**Az SQL-hitelesítéssel JSON**</span><span class="sxs-lookup"><span data-stu-id="b553d-168">**JSON for using SQL Authentication**</span></span>

```json
{
    "name": "MyOnPremisesSQLDB",
    "properties":
    {
        "type": "OnPremisesSqlServer",
        "typeProperties": {
            "connectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=False;User ID=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```
<span data-ttu-id="b553d-169">**A Windows-hitelesítést használó JSON**</span><span class="sxs-lookup"><span data-stu-id="b553d-169">**JSON for using Windows Authentication**</span></span>

<span data-ttu-id="b553d-170">Az adatkezelési átjáró fog megszemélyesíteni hello megadott felhasználói fiók tooconnect toohello a helyszíni SQL Server-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="b553d-170">Data Management Gateway will impersonate hello specified user account tooconnect toohello on-premises SQL Server database.</span></span> 

```json
{
     "Name": " MyOnPremisesSQLDB",
     "Properties":
     {
         "type": "OnPremisesSqlServer",
         "typeProperties": {
             "ConnectionString": "Data Source=<servername>;Initial Catalog=MarketingCampaigns;Integrated Security=True;",
             "username": "<domain\\username>",
             "password": "<password>",
             "gatewayName": "<gateway name>"
        }
     }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="b553d-171">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b553d-171">Dataset properties</span></span>
<span data-ttu-id="b553d-172">A hello mintában használt típusú dataset **SqlServerTable** toorepresent egy SQL Server adatbázis egyik táblája.</span><span class="sxs-lookup"><span data-stu-id="b553d-172">In hello samples, you have used a dataset of type **SqlServerTable** toorepresent a table in a SQL Server database.</span></span>  

<span data-ttu-id="b553d-173">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b553d-173">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="b553d-174">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (SQL Server, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="b553d-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (SQL Server, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="b553d-175">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b553d-175">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="b553d-176">Hello **typeProperties** típusú hello adatkészlet szakasz **SqlServerTable** rendelkezik hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="b553d-176">hello **typeProperties** section for hello dataset of type **SqlServerTable** has hello following properties:</span></span>

| <span data-ttu-id="b553d-177">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b553d-177">Property</span></span> | <span data-ttu-id="b553d-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="b553d-178">Description</span></span> | <span data-ttu-id="b553d-179">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b553d-179">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b553d-180">tableName</span><span class="sxs-lookup"><span data-stu-id="b553d-180">tableName</span></span> |<span data-ttu-id="b553d-181">Hello tábla vagy nézet hello SQL Server-adatbázispéldány, amelyre a társított szolgáltatás neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b553d-181">Name of hello table or view in hello SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="b553d-182">Igen</span><span class="sxs-lookup"><span data-stu-id="b553d-182">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="b553d-183">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b553d-183">Copy activity properties</span></span>
<span data-ttu-id="b553d-184">Ha adatokat egy SQL Server-adatbázisból, beállítása hello forrástípus hello másolási tevékenység túl**SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="b553d-184">If you are moving data from a SQL Server database, you set hello source type in hello copy activity too**SqlSource**.</span></span> <span data-ttu-id="b553d-185">Hasonlóképpen, ha az tooa SQL Server-adatbázist, beállítása hello a fogadó típusa hello másolási tevékenység túl**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="b553d-185">Similarly, if you are moving data tooa SQL Server database, you set hello sink type in hello copy activity too**SqlSink**.</span></span> <span data-ttu-id="b553d-186">Ez a témakör SqlSource és SqlSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="b553d-186">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

<span data-ttu-id="b553d-187">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b553d-187">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="b553d-188">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b553d-188">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="b553d-189">hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="b553d-189">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="b553d-190">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="b553d-190">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="b553d-191">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="b553d-191">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="b553d-192">SqlSource</span><span class="sxs-lookup"><span data-stu-id="b553d-192">SqlSource</span></span>
<span data-ttu-id="b553d-193">Ha a másolási tevékenység során a forrás típusa nem **SqlSource**, hello a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b553d-193">When source in a copy activity is of type **SqlSource**, hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="b553d-194">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b553d-194">Property</span></span> | <span data-ttu-id="b553d-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="b553d-195">Description</span></span> | <span data-ttu-id="b553d-196">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b553d-196">Allowed values</span></span> | <span data-ttu-id="b553d-197">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b553d-197">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b553d-198">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="b553d-198">sqlReaderQuery</span></span> |<span data-ttu-id="b553d-199">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="b553d-199">Use hello custom query tooread data.</span></span> |<span data-ttu-id="b553d-200">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="b553d-200">SQL query string.</span></span> <span data-ttu-id="b553d-201">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="b553d-201">For example: select * from MyTable.</span></span> <span data-ttu-id="b553d-202">Hivatkozhatnak hello bemeneti adatkészlet által hivatkozott hello adatbázisból táblákat.</span><span class="sxs-lookup"><span data-stu-id="b553d-202">May reference multiple tables from hello database referenced by hello input dataset.</span></span> <span data-ttu-id="b553d-203">Ha nincs megadva, az SQL-utasítás végrehajtása hello: táblanév kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="b553d-203">If not specified, hello SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="b553d-204">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-204">No</span></span> |
| <span data-ttu-id="b553d-205">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b553d-205">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="b553d-206">Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b553d-206">Name of hello stored procedure that reads data from hello source table.</span></span> |<span data-ttu-id="b553d-207">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b553d-207">Name of hello stored procedure.</span></span> <span data-ttu-id="b553d-208">hello utolsó SQL-utasítás hello tárolt eljárás SELECT utasítással kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b553d-208">hello last SQL statement must be a SELECT statement in hello stored procedure.</span></span> |<span data-ttu-id="b553d-209">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-209">No</span></span> |
| <span data-ttu-id="b553d-210">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b553d-210">storedProcedureParameters</span></span> |<span data-ttu-id="b553d-211">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b553d-211">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b553d-212">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b553d-212">Name/value pairs.</span></span> <span data-ttu-id="b553d-213">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b553d-213">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b553d-214">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-214">No</span></span> |

<span data-ttu-id="b553d-215">Ha hello **sqlReaderQuery** megadott hello SqlSource, hello másolási tevékenység során ez a lekérdezés futtatása hello SQL Server-adatbázis forrás tooget hello adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="b553d-215">If hello **sqlReaderQuery** is specified for hello SqlSource, hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span>

<span data-ttu-id="b553d-216">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="b553d-216">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span>

<span data-ttu-id="b553d-217">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b553d-217">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="b553d-218">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="b553d-218">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="b553d-219">Használata esetén **sqlReaderStoredProcedureName**, továbbra is szükséges toospecify értéket hello **tableName** hello adatkészlet JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b553d-219">When you use **sqlReaderStoredProcedureName**, you still need toospecify a value for hello **tableName** property in hello dataset JSON.</span></span> <span data-ttu-id="b553d-220">Nincs érvényesítést hajt végre ezt a táblázatot, ha van.</span><span class="sxs-lookup"><span data-stu-id="b553d-220">There are no validations performed against this table though.</span></span>

### <a name="sqlsink"></a><span data-ttu-id="b553d-221">SqlSink</span><span class="sxs-lookup"><span data-stu-id="b553d-221">SqlSink</span></span>
<span data-ttu-id="b553d-222">**SqlSink** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="b553d-222">**SqlSink** supports hello following properties:</span></span>

| <span data-ttu-id="b553d-223">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b553d-223">Property</span></span> | <span data-ttu-id="b553d-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="b553d-224">Description</span></span> | <span data-ttu-id="b553d-225">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b553d-225">Allowed values</span></span> | <span data-ttu-id="b553d-226">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b553d-226">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b553d-227">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="b553d-227">writeBatchTimeout</span></span> |<span data-ttu-id="b553d-228">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="b553d-228">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="b553d-229">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b553d-229">timespan</span></span><br/><br/> <span data-ttu-id="b553d-230">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="b553d-230">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="b553d-231">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-231">No</span></span> |
| <span data-ttu-id="b553d-232">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="b553d-232">writeBatchSize</span></span> |<span data-ttu-id="b553d-233">Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="b553d-233">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="b553d-234">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="b553d-234">Integer (number of rows)</span></span> |<span data-ttu-id="b553d-235">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="b553d-235">No (default: 10000)</span></span> |
| <span data-ttu-id="b553d-236">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="b553d-236">sqlWriterCleanupScript</span></span> |<span data-ttu-id="b553d-237">Adja meg a másolási tevékenység tooexecute lekérdezést, úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="b553d-237">Specify query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="b553d-238">További információkért lásd: [ismételhető másolási](#repeatable-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b553d-238">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="b553d-239">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="b553d-239">A query statement.</span></span> |<span data-ttu-id="b553d-240">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-240">No</span></span> |
| <span data-ttu-id="b553d-241">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="b553d-241">sliceIdentifierColumnName</span></span> |<span data-ttu-id="b553d-242">Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean.</span><span class="sxs-lookup"><span data-stu-id="b553d-242">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> <span data-ttu-id="b553d-243">További információkért lásd: [ismételhető másolási](#repeatable-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b553d-243">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="b553d-244">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="b553d-244">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="b553d-245">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-245">No</span></span> |
| <span data-ttu-id="b553d-246">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="b553d-246">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="b553d-247">Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába.</span><span class="sxs-lookup"><span data-stu-id="b553d-247">Name of hello stored procedure that upserts (updates/inserts) data into hello target table.</span></span> |<span data-ttu-id="b553d-248">Hello neve tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b553d-248">Name of hello stored procedure.</span></span> |<span data-ttu-id="b553d-249">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-249">No</span></span> |
| <span data-ttu-id="b553d-250">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="b553d-250">storedProcedureParameters</span></span> |<span data-ttu-id="b553d-251">Hello paramétereinek tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="b553d-251">Parameters for hello stored procedure.</span></span> |<span data-ttu-id="b553d-252">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="b553d-252">Name/value pairs.</span></span> <span data-ttu-id="b553d-253">Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b553d-253">Names and casing of parameters must match hello names and casing of hello stored procedure parameters.</span></span> |<span data-ttu-id="b553d-254">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-254">No</span></span> |
| <span data-ttu-id="b553d-255">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="b553d-255">sqlWriterTableType</span></span> |<span data-ttu-id="b553d-256">Adja meg a tábla Típus neve toobe hello tárolt eljárásban használt.</span><span class="sxs-lookup"><span data-stu-id="b553d-256">Specify table type name toobe used in hello stored procedure.</span></span> <span data-ttu-id="b553d-257">Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="b553d-257">Copy activity makes hello data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="b553d-258">Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="b553d-258">Stored procedure code can then merge hello data being copied with existing data.</span></span> |<span data-ttu-id="b553d-259">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="b553d-259">A table type name.</span></span> |<span data-ttu-id="b553d-260">Nem</span><span class="sxs-lookup"><span data-stu-id="b553d-260">No</span></span> |


## <a name="json-examples-for-copying-data-from-and-toosql-server"></a><span data-ttu-id="b553d-261">Az adatok és a kiszolgáló tooSQL másolására JSON példák</span><span class="sxs-lookup"><span data-stu-id="b553d-261">JSON examples for copying data from and tooSQL Server</span></span>
<span data-ttu-id="b553d-262">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b553d-262">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="b553d-263">a következő minták megjelenítése hogyan hello toocopy adatok tooand az SQL Server és az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b553d-263">hello following samples show how toocopy data tooand from SQL Server and Azure Blob Storage.</span></span> <span data-ttu-id="b553d-264">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="b553d-264">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>     

## <a name="example-copy-data-from-sql-server-tooazure-blob"></a><span data-ttu-id="b553d-265">Példa: Adatok másolása az SQL Server tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="b553d-265">Example: Copy data from SQL Server tooAzure Blob</span></span>
<span data-ttu-id="b553d-266">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="b553d-266">hello following sample shows:</span></span>

1. <span data-ttu-id="b553d-267">A társított szolgáltatás típusa [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-267">A linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="b553d-268">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-268">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b553d-269">Bemeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-269">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](#dataset-properties).</span></span>
4. <span data-ttu-id="b553d-270">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-270">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b553d-271">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-271">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b553d-272">hello minta másol idősorozat adatokat egy SQL Server tábla tooan Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="b553d-272">hello sample copies time-series data from a SQL Server table tooan Azure blob every hour.</span></span> <span data-ttu-id="b553d-273">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="b553d-273">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="b553d-274">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="b553d-274">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="b553d-275">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="b553d-275">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="b553d-276">**Kapcsolódó SQL Server szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="b553d-276">**SQL Server linked service**</span></span>
```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
<span data-ttu-id="b553d-277">**Az Azure Blob storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="b553d-277">**Azure Blob storage linked service**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="b553d-278">**SQL Server bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="b553d-278">**SQL Server input dataset**</span></span>

<span data-ttu-id="b553d-279">hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" SQL Server és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b553d-279">hello sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="b553d-280">Több tábla belül azonos adatbázist egyetlen dataset, de egy táblát kell használni az hello dataset tableName typeProperty hello keresztül kérdezheti le.</span><span class="sxs-lookup"><span data-stu-id="b553d-280">You can query over multiple tables within hello same database using a single dataset, but a single table must be used for hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="b553d-281">"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="b553d-281">Setting “external”: ”true” informs Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
  "name": "SqlServerInput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
<span data-ttu-id="b553d-282">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="b553d-282">**Azure Blob output dataset**</span></span>

<span data-ttu-id="b553d-283">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="b553d-283">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b553d-284">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="b553d-284">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="b553d-285">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="b553d-285">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="b553d-286">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="b553d-286">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="b553d-287">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="b553d-287">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="b553d-288">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b553d-288">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="b553d-289">hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="b553d-289">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2016-06-01T18:00:00",
    "end":"2016-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
<span data-ttu-id="b553d-290">Ebben a példában **sqlReaderQuery** hello SqlSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="b553d-290">In this example, **sqlReaderQuery** is specified for hello SqlSource.</span></span> <span data-ttu-id="b553d-291">hello másolási tevékenység fut ez a lekérdezés hello hello tooget SQL Server-adatbázis forrásadatot.</span><span class="sxs-lookup"><span data-stu-id="b553d-291">hello Copy Activity runs this query against hello SQL Server Database source tooget hello data.</span></span> <span data-ttu-id="b553d-292">Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="b553d-292">Alternatively, you can specify a stored procedure by specifying hello **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if hello stored procedure takes parameters).</span></span> <span data-ttu-id="b553d-293">hello sqlReaderQuery hello bemeneti adatkészlet által hivatkozott hello adatbázison belül több táblát is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b553d-293">hello sqlReaderQuery can reference multiple tables within hello database referenced by hello input dataset.</span></span> <span data-ttu-id="b553d-294">Már nem korlátozott tooonly hello tábla adatkészlet tableName typeProperty hello beállítani.</span><span class="sxs-lookup"><span data-stu-id="b553d-294">It is not limited tooonly hello table set as hello dataset's tableName typeProperty.</span></span>

<span data-ttu-id="b553d-295">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello struktúra szakaszban meghatározott hello oszlopok használt toobuild a select lekérdezés toorun elleni hello SQL Server-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b553d-295">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, hello columns defined in hello structure section are used toobuild a select query toorun against hello SQL Server Database.</span></span> <span data-ttu-id="b553d-296">Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.</span><span class="sxs-lookup"><span data-stu-id="b553d-296">If hello dataset definition does not have hello structure, all columns are selected from hello table.</span></span>

<span data-ttu-id="b553d-297">Lásd: hello [Sql-forrás](#sqlsource) szakasz és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource és BlobSink által támogatott tulajdonságokról hello listáját.</span><span class="sxs-lookup"><span data-stu-id="b553d-297">See hello [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for hello list of properties supported by SqlSource and BlobSink.</span></span>

## <a name="example-copy-data-from-azure-blob-toosql-server"></a><span data-ttu-id="b553d-298">Példa: Adatok másolása az Azure Blob tooSQL kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="b553d-298">Example: Copy data from Azure Blob tooSQL Server</span></span>
<span data-ttu-id="b553d-299">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="b553d-299">hello following sample shows:</span></span>

1. <span data-ttu-id="b553d-300">hello társított szolgáltatás típusa [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-300">hello linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="b553d-301">hello társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-301">hello linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="b553d-302">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-302">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="b553d-303">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-303">An output [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="b553d-304">Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlSink](#sql-server-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="b553d-304">hello [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#sql-server-copy-activity-type-properties).</span></span>

<span data-ttu-id="b553d-305">hello minta idősorozat adatainak másolása az Azure blob tooa SQL Server táblából óránként.</span><span class="sxs-lookup"><span data-stu-id="b553d-305">hello sample copies time-series data from an Azure blob tooa SQL Server table every hour.</span></span> <span data-ttu-id="b553d-306">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="b553d-306">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="b553d-307">**Kapcsolódó SQL Server szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="b553d-307">**SQL Server linked service**</span></span>

```json
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```
<span data-ttu-id="b553d-308">**Az Azure Blob storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="b553d-308">**Azure Blob storage linked service**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="b553d-309">**Az Azure Blob bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="b553d-309">**Azure Blob input dataset**</span></span>

<span data-ttu-id="b553d-310">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="b553d-310">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b553d-311">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="b553d-311">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="b553d-312">hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello.</span><span class="sxs-lookup"><span data-stu-id="b553d-312">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="b553d-313">"external": "true" beállítás arról értesíti az adott hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="b553d-313">“external”: “true” setting informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
<span data-ttu-id="b553d-314">**SQL Server kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="b553d-314">**SQL Server output dataset**</span></span>

<span data-ttu-id="b553d-315">hello minta másolja át az SQL Server "MyTable" nevű tooa adattábla.</span><span class="sxs-lookup"><span data-stu-id="b553d-315">hello sample copies data tooa table named “MyTable” in SQL Server.</span></span> <span data-ttu-id="b553d-316">Hozzon létre hello táblát az SQL Server azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt.</span><span class="sxs-lookup"><span data-stu-id="b553d-316">Create hello table in SQL Server with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="b553d-317">Új sorok hozzáadásakor toohello tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="b553d-317">New rows are added toohello table every hour.</span></span>

```json
{
  "name": "SqlServerOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="b553d-318">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="b553d-318">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="b553d-319">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse ezen bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="b553d-319">hello pipeline contains a Copy Activity that is configured toouse these input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="b553d-320">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="b553d-320">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": " SqlServerOutput "
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```

## <a name="troubleshooting-connection-issues"></a><span data-ttu-id="b553d-321">Kapcsolati problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="b553d-321">Troubleshooting connection issues</span></span>
1. <span data-ttu-id="b553d-322">Az SQL Server tooaccept távoli kapcsolatok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="b553d-322">Configure your SQL Server tooaccept remote connections.</span></span> <span data-ttu-id="b553d-323">Indítsa el **SQL Server Management Studio**, kattintson a jobb gombbal **server**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="b553d-323">Launch **SQL Server Management Studio**, right-click **server**, and click **Properties**.</span></span> <span data-ttu-id="b553d-324">Válassza ki **kapcsolatok** hello listája és ellenőrzés **engedélyezés távoli kapcsolatok toohello server**.</span><span class="sxs-lookup"><span data-stu-id="b553d-324">Select **Connections** from hello list and check **Allow remote connections toohello server**.</span></span>

    ![Távoli kapcsolatok engedélyezése](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    <span data-ttu-id="b553d-326">Lásd: [hello távelérési kiszolgáló konfigurációs beállítás konfigurálása](https://msdn.microsoft.com/library/ms191464.aspx) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b553d-326">See [Configure hello remote access Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) for detailed steps.</span></span>
2. <span data-ttu-id="b553d-327">Indítsa el **SQL Server Konfigurációkezelő**.</span><span class="sxs-lookup"><span data-stu-id="b553d-327">Launch **SQL Server Configuration Manager**.</span></span> <span data-ttu-id="b553d-328">Bontsa ki a **SQL Server hálózati konfigurációja** hello a példány akkor kívánja, majd válassza **MSSQLSERVER protokolljai**.</span><span class="sxs-lookup"><span data-stu-id="b553d-328">Expand **SQL Server Network Configuration** for hello instance you want, and select **Protocols for MSSQLSERVER**.</span></span> <span data-ttu-id="b553d-329">Meg kell jelennie protokollok hello jobb oldali ablaktáblában.</span><span class="sxs-lookup"><span data-stu-id="b553d-329">You should see protocols in hello right-pane.</span></span> <span data-ttu-id="b553d-330">Engedélyezze a TCP/IP protokollt kattintson a jobb gombbal **TCP/IP** elemre kattintva **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="b553d-330">Enable TCP/IP by right-clicking **TCP/IP** and clicking **Enable**.</span></span>

    ![Engedélyezze a TCP/IP protokollt](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    <span data-ttu-id="b553d-332">Lásd: [engedélyezheti vagy tilthatja le a hálózati protokoll](https://msdn.microsoft.com/library/ms191294.aspx) részleteit és más módon, hogy a TCP/IP-protokoll.</span><span class="sxs-lookup"><span data-stu-id="b553d-332">See [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) for details and alternate ways of enabling TCP/IP protocol.</span></span>
3. <span data-ttu-id="b553d-333">A hello azonos ablak, kattintson duplán a **TCP/IP** toolaunch **TCP/IP-tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="b553d-333">In hello same window, double-click **TCP/IP** toolaunch **TCP/IP Properties** window.</span></span>
4. <span data-ttu-id="b553d-334">Váltás toohello **IP-címek** fülre. Görgessen lefelé toosee **IPAll** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b553d-334">Switch toohello **IP Addresses** tab. Scroll down toosee **IPAll** section.</span></span> <span data-ttu-id="b553d-335">Jegyezze fel a hello ** TCP-Port ** (alapértelmezett érték a **1433**).</span><span class="sxs-lookup"><span data-stu-id="b553d-335">Note down hello **TCP Port **(default is **1433**).</span></span>
5. <span data-ttu-id="b553d-336">Hozzon létre egy **szabály a Windows tűzfal hello** hello gép tooallow érkező forgalmat ezen a porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="b553d-336">Create a **rule for hello Windows Firewall** on hello machine tooallow incoming traffic through this port.</span></span>  
6. <span data-ttu-id="b553d-337">**Ellenőrizze a kapcsolat**: tooconnect toohello teljesen minősített nevet használó SQL Server SQL Server Management Studio használja egy másik gépről.</span><span class="sxs-lookup"><span data-stu-id="b553d-337">**Verify connection**: tooconnect toohello SQL Server using fully qualified name, use SQL Server Management Studio from a different machine.</span></span> <span data-ttu-id="b553d-338">Például: "<machine>.<domain>. Corp.<company>.com, 1433. "</span><span class="sxs-lookup"><span data-stu-id="b553d-338">For example: "<machine>.<domain>.corp.<company>.com,1433."</span></span>

   > [!IMPORTANT]

   > <span data-ttu-id="b553d-339">Lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró hello felhő között](data-factory-move-data-between-onprem-and-cloud.md) részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="b553d-339">See [Move data between on-premises sources and hello cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for detailed information.</span></span>
   >
   > <span data-ttu-id="b553d-340">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="b553d-340">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>
   >
   >


## <a name="identity-columns-in-hello-target-database"></a><span data-ttu-id="b553d-341">Azonosító oszlop hello céladatbázis</span><span class="sxs-lookup"><span data-stu-id="b553d-341">Identity columns in hello target database</span></span>
<span data-ttu-id="b553d-342">Ez a szakasz azt mutatja, hogy az adatok átmásolja a forrás nincs azonosító oszlop tooa céltábla azonosító oszlopot tartalmazó tábla.</span><span class="sxs-lookup"><span data-stu-id="b553d-342">This section provides an example that copies data from a source table with no identity column tooa destination table with an identity column.</span></span>

<span data-ttu-id="b553d-343">**Forrástábla:**</span><span class="sxs-lookup"><span data-stu-id="b553d-343">**Source table:**</span></span>

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="b553d-344">**Céltábla:**</span><span class="sxs-lookup"><span data-stu-id="b553d-344">**Destination table:**</span></span>

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

<span data-ttu-id="b553d-345">Figyelje meg, hogy hello céltábla tartalmaz azonosító oszlopot.</span><span class="sxs-lookup"><span data-stu-id="b553d-345">Notice that hello target table has an identity column.</span></span>

<span data-ttu-id="b553d-346">**Forrás adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="b553d-346">**Source dataset JSON definition**</span></span>

```json
{
    "name": "SampleSource",
    "properties": {
        "published": false,
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
<span data-ttu-id="b553d-347">**Cél adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="b553d-347">**Destination dataset JSON definition**</span></span>

```json
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

<span data-ttu-id="b553d-348">Figyelje meg, hogy a forrás és cél táblázatként különböző sémája (cél rendelkezik egy olyan további oszlop identitású).</span><span class="sxs-lookup"><span data-stu-id="b553d-348">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="b553d-349">Ebben az esetben szüksége toospecify **struktúra** hello tároló adatkészlet-definícióban, amely nem tartalmazza a hello azonosító oszlop tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="b553d-349">In this scenario, you need toospecify **structure** property in hello target dataset definition, which doesn’t include hello identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="b553d-350">A fogadó SQL tárolt eljárás meghívása</span><span class="sxs-lookup"><span data-stu-id="b553d-350">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="b553d-351">Lásd: [fogadó SQL tárolt eljárás meghívása a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md) cikk SQL fogadó a folyamat a másolási tevékenység a tárolt eljárás meghívása példát.</span><span class="sxs-lookup"><span data-stu-id="b553d-351">See [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article for an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline.</span></span>

## <a name="type-mapping-for-sql-server"></a><span data-ttu-id="b553d-352">Az SQL server leképezésének</span><span class="sxs-lookup"><span data-stu-id="b553d-352">Type mapping for SQL server</span></span>
<span data-ttu-id="b553d-353">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk hello másolási tevékenység hajt végre automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello:</span><span class="sxs-lookup"><span data-stu-id="b553d-353">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, hello Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="b553d-354">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="b553d-354">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="b553d-355">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="b553d-355">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="b553d-356">Ha áthelyezése adatok túl & SQL server, a hello következő megfeleltetéseket használ SQL too.NET típusának, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="b553d-356">When moving data too& from SQL server, hello following mappings are used from SQL type too.NET type and vice versa.</span></span>

<span data-ttu-id="b553d-357">hello ugyanaz, mint az SQL Server adattípus-hozzárendelése az ADO.NET hello lesz.</span><span class="sxs-lookup"><span data-stu-id="b553d-357">hello mapping is same as hello SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="b553d-358">SQL Server adatbázismotor típusa</span><span class="sxs-lookup"><span data-stu-id="b553d-358">SQL Server Database Engine type</span></span> | <span data-ttu-id="b553d-359">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="b553d-359">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="b553d-360">bigint</span><span class="sxs-lookup"><span data-stu-id="b553d-360">bigint</span></span> |<span data-ttu-id="b553d-361">Int64</span><span class="sxs-lookup"><span data-stu-id="b553d-361">Int64</span></span> |
| <span data-ttu-id="b553d-362">Bináris</span><span class="sxs-lookup"><span data-stu-id="b553d-362">binary</span></span> |<span data-ttu-id="b553d-363">Byte]</span><span class="sxs-lookup"><span data-stu-id="b553d-363">Byte[]</span></span> |
| <span data-ttu-id="b553d-364">bit</span><span class="sxs-lookup"><span data-stu-id="b553d-364">bit</span></span> |<span data-ttu-id="b553d-365">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="b553d-365">Boolean</span></span> |
| <span data-ttu-id="b553d-366">Karakter</span><span class="sxs-lookup"><span data-stu-id="b553d-366">char</span></span> |<span data-ttu-id="b553d-367">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="b553d-367">String, Char[]</span></span> |
| <span data-ttu-id="b553d-368">Dátum</span><span class="sxs-lookup"><span data-stu-id="b553d-368">date</span></span> |<span data-ttu-id="b553d-369">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b553d-369">DateTime</span></span> |
| <span data-ttu-id="b553d-370">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b553d-370">Datetime</span></span> |<span data-ttu-id="b553d-371">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b553d-371">DateTime</span></span> |
| <span data-ttu-id="b553d-372">datetime2</span><span class="sxs-lookup"><span data-stu-id="b553d-372">datetime2</span></span> |<span data-ttu-id="b553d-373">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b553d-373">DateTime</span></span> |
| <span data-ttu-id="b553d-374">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="b553d-374">Datetimeoffset</span></span> |<span data-ttu-id="b553d-375">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="b553d-375">DateTimeOffset</span></span> |
| <span data-ttu-id="b553d-376">Decimális</span><span class="sxs-lookup"><span data-stu-id="b553d-376">Decimal</span></span> |<span data-ttu-id="b553d-377">Decimális</span><span class="sxs-lookup"><span data-stu-id="b553d-377">Decimal</span></span> |
| <span data-ttu-id="b553d-378">A FILESTREAM attribútum (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="b553d-378">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="b553d-379">Byte]</span><span class="sxs-lookup"><span data-stu-id="b553d-379">Byte[]</span></span> |
| <span data-ttu-id="b553d-380">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="b553d-380">Float</span></span> |<span data-ttu-id="b553d-381">Dupla</span><span class="sxs-lookup"><span data-stu-id="b553d-381">Double</span></span> |
| <span data-ttu-id="b553d-382">Kép</span><span class="sxs-lookup"><span data-stu-id="b553d-382">image</span></span> |<span data-ttu-id="b553d-383">Byte]</span><span class="sxs-lookup"><span data-stu-id="b553d-383">Byte[]</span></span> |
| <span data-ttu-id="b553d-384">int</span><span class="sxs-lookup"><span data-stu-id="b553d-384">int</span></span> |<span data-ttu-id="b553d-385">Int32</span><span class="sxs-lookup"><span data-stu-id="b553d-385">Int32</span></span> |
| <span data-ttu-id="b553d-386">pénz</span><span class="sxs-lookup"><span data-stu-id="b553d-386">money</span></span> |<span data-ttu-id="b553d-387">Decimális</span><span class="sxs-lookup"><span data-stu-id="b553d-387">Decimal</span></span> |
| <span data-ttu-id="b553d-388">nchar</span><span class="sxs-lookup"><span data-stu-id="b553d-388">nchar</span></span> |<span data-ttu-id="b553d-389">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="b553d-389">String, Char[]</span></span> |
| <span data-ttu-id="b553d-390">ntext</span><span class="sxs-lookup"><span data-stu-id="b553d-390">ntext</span></span> |<span data-ttu-id="b553d-391">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="b553d-391">String, Char[]</span></span> |
| <span data-ttu-id="b553d-392">Numerikus</span><span class="sxs-lookup"><span data-stu-id="b553d-392">numeric</span></span> |<span data-ttu-id="b553d-393">Decimális</span><span class="sxs-lookup"><span data-stu-id="b553d-393">Decimal</span></span> |
| <span data-ttu-id="b553d-394">nvarchar</span><span class="sxs-lookup"><span data-stu-id="b553d-394">nvarchar</span></span> |<span data-ttu-id="b553d-395">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="b553d-395">String, Char[]</span></span> |
| <span data-ttu-id="b553d-396">valós</span><span class="sxs-lookup"><span data-stu-id="b553d-396">real</span></span> |<span data-ttu-id="b553d-397">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="b553d-397">Single</span></span> |
| <span data-ttu-id="b553d-398">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="b553d-398">rowversion</span></span> |<span data-ttu-id="b553d-399">Byte]</span><span class="sxs-lookup"><span data-stu-id="b553d-399">Byte[]</span></span> |
| <span data-ttu-id="b553d-400">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="b553d-400">smalldatetime</span></span> |<span data-ttu-id="b553d-401">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="b553d-401">DateTime</span></span> |
| <span data-ttu-id="b553d-402">smallint</span><span class="sxs-lookup"><span data-stu-id="b553d-402">smallint</span></span> |<span data-ttu-id="b553d-403">Int16</span><span class="sxs-lookup"><span data-stu-id="b553d-403">Int16</span></span> |
| <span data-ttu-id="b553d-404">kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="b553d-404">smallmoney</span></span> |<span data-ttu-id="b553d-405">Decimális</span><span class="sxs-lookup"><span data-stu-id="b553d-405">Decimal</span></span> |
| <span data-ttu-id="b553d-406">sql_variant</span><span class="sxs-lookup"><span data-stu-id="b553d-406">sql_variant</span></span> |<span data-ttu-id="b553d-407">Objektum *</span><span class="sxs-lookup"><span data-stu-id="b553d-407">Object *</span></span> |
| <span data-ttu-id="b553d-408">Szöveg</span><span class="sxs-lookup"><span data-stu-id="b553d-408">text</span></span> |<span data-ttu-id="b553d-409">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="b553d-409">String, Char[]</span></span> |
| <span data-ttu-id="b553d-410">time</span><span class="sxs-lookup"><span data-stu-id="b553d-410">time</span></span> |<span data-ttu-id="b553d-411">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="b553d-411">TimeSpan</span></span> |
| <span data-ttu-id="b553d-412">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="b553d-412">timestamp</span></span> |<span data-ttu-id="b553d-413">Byte]</span><span class="sxs-lookup"><span data-stu-id="b553d-413">Byte[]</span></span> |
| <span data-ttu-id="b553d-414">tinyint</span><span class="sxs-lookup"><span data-stu-id="b553d-414">tinyint</span></span> |<span data-ttu-id="b553d-415">Bájt</span><span class="sxs-lookup"><span data-stu-id="b553d-415">Byte</span></span> |
| <span data-ttu-id="b553d-416">egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="b553d-416">uniqueidentifier</span></span> |<span data-ttu-id="b553d-417">GUID</span><span class="sxs-lookup"><span data-stu-id="b553d-417">Guid</span></span> |
| <span data-ttu-id="b553d-418">varbinary</span><span class="sxs-lookup"><span data-stu-id="b553d-418">varbinary</span></span> |<span data-ttu-id="b553d-419">Byte]</span><span class="sxs-lookup"><span data-stu-id="b553d-419">Byte[]</span></span> |
| <span data-ttu-id="b553d-420">varchar</span><span class="sxs-lookup"><span data-stu-id="b553d-420">varchar</span></span> |<span data-ttu-id="b553d-421">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="b553d-421">String, Char[]</span></span> |
| <span data-ttu-id="b553d-422">xml</span><span class="sxs-lookup"><span data-stu-id="b553d-422">xml</span></span> |<span data-ttu-id="b553d-423">XML</span><span class="sxs-lookup"><span data-stu-id="b553d-423">Xml</span></span> |

## <a name="mapping-source-toosink-columns"></a><span data-ttu-id="b553d-424">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="b553d-424">Mapping source toosink columns</span></span>
<span data-ttu-id="b553d-425">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b553d-425">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="b553d-426">Ismételhető másolása</span><span class="sxs-lookup"><span data-stu-id="b553d-426">Repeatable copy</span></span>
<span data-ttu-id="b553d-427">Adatok tooSQL Server-adatbázis másolásakor hello másolási tevékenység hozzáfűzi toohello fogadó adattábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b553d-427">When copying data tooSQL Server Database, hello copy activity appends data toohello sink table by default.</span></span> <span data-ttu-id="b553d-428">egy UPSERT tooperform helyett, lásd: [ismételhető írási tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) cikk.</span><span class="sxs-lookup"><span data-stu-id="b553d-428">tooperform an UPSERT instead,  See [Repeatable write tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="b553d-429">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="b553d-429">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="b553d-430">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="b553d-430">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="b553d-431">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="b553d-431">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="b553d-432">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="b553d-432">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="b553d-433">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="b553d-433">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="b553d-434">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="b553d-434">Performance and Tuning</span></span>
<span data-ttu-id="b553d-435">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="b553d-435">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
