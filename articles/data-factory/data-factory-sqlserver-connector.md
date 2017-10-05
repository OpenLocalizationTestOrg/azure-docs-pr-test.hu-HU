---
title: "Adatok áthelyezése, és az SQL-kiszolgáló |} Microsoft Docs"
description: "További tudnivalók áthelyezni az adatokat, vagy a helyszíni SQL Server-adatbázist vagy egy Azure virtuális gép Azure Data Factory használatával."
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
ms.openlocfilehash: 9cd2077d897631457925cda5ef5e6df3c0c33177
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-to-and-from-sql-server-on-premises-or-on-iaas-azure-vm-using-azure-data-factory"></a><span data-ttu-id="ca508-103">Azure Data Factory használatával történő és a helyszíni SQL Server vagy a IaaS (Azure virtuális gép) adatok áthelyezése</span><span class="sxs-lookup"><span data-stu-id="ca508-103">Move data to and from SQL Server on-premises or on IaaS (Azure VM) using Azure Data Factory</span></span>
<span data-ttu-id="ca508-104">Ez a cikk ismerteti, hogyan használható a másolási tevékenység során az Azure Data Factory adatok belőle egy helyi SQL Server-adatbázis áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="ca508-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from an on-premises SQL Server database.</span></span> <span data-ttu-id="ca508-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="ca508-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="ca508-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="ca508-106">Supported scenarios</span></span>
<span data-ttu-id="ca508-107">Adatokat másolhat **egy SQL Server-adatbázisból** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="ca508-107">You can copy data **from a SQL Server database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="ca508-108">Adatok másolása a következő adatokat tárolja **SQL Server-adatbázishoz**:</span><span class="sxs-lookup"><span data-stu-id="ca508-108">You can copy data from the following data stores **to a SQL Server database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-sql-server-versions"></a><span data-ttu-id="ca508-109">Támogatott SQL Server-verziók</span><span class="sxs-lookup"><span data-stu-id="ca508-109">Supported SQL Server versions</span></span>
<span data-ttu-id="ca508-110">Az SQL Server connector támogatása az adatok másolásának, vagy a következő verziók helyszínen üzemeltetett példányt, vagy az SQL-hitelesítést és a Windows-hitelesítést használó Azure IaaS: SQL Server 2016, az SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span><span class="sxs-lookup"><span data-stu-id="ca508-110">This SQL Server connector support copying data from/to the following versions of instance hosted on-premises or in Azure IaaS using both SQL authentication and Windows authentication: SQL Server 2016, SQL Server 2014, SQL Server 2012, SQL Server 2008 R2, SQL Server 2008, SQL Server 2005</span></span>

## <a name="enabling-connectivity"></a><span data-ttu-id="ca508-111">Kapcsolat engedélyezése</span><span class="sxs-lookup"><span data-stu-id="ca508-111">Enabling connectivity</span></span>
<span data-ttu-id="ca508-112">A fogalmakat és a helyszíni SQL Server által futtatott vagy Azure IaaS (infrastruktúra-,--szolgáltatás) virtuális gépeket a csatlakozáshoz szükséges lépések megegyeznek.</span><span class="sxs-lookup"><span data-stu-id="ca508-112">The concepts and steps needed for connecting with SQL Server hosted on-premises or in Azure IaaS (Infrastructure-as-a-Service) VMs are the same.</span></span> <span data-ttu-id="ca508-113">Mindkét esetben kell használnia az adatkezelési átjáró a hálózati kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="ca508-113">In both cases, you need to use Data Management Gateway for connectivity.</span></span>

<span data-ttu-id="ca508-114">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikkben tájékozódhat az adatkezelési átjáró és az átjáró beállításával kapcsolatos részletes útmutatás.</span><span class="sxs-lookup"><span data-stu-id="ca508-114">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway.</span></span> <span data-ttu-id="ca508-115">Kapcsolódás az SQL Server olyan előfeltételt egy átjárópéldányt beállítása.</span><span class="sxs-lookup"><span data-stu-id="ca508-115">Setting up a gateway instance is a pre-requisite for connecting with SQL Server.</span></span>

<span data-ttu-id="ca508-116">Amíg átjárót telepítheti az ugyanabban a helyi számítógépen vagy a felhő Virtuálisgép-példány és az SQL Server, a jobb teljesítmény, ajánlott külön gépeken telepíteni.</span><span class="sxs-lookup"><span data-stu-id="ca508-116">While you can install gateway on the same on-premises machine or cloud VM instance as the SQL Server for better performance, we recommended that you install them on separate machines.</span></span> <span data-ttu-id="ca508-117">Az átjáró és az SQL Server különböző gépeken csökkenti a Erőforrásverseny.</span><span class="sxs-lookup"><span data-stu-id="ca508-117">Having the gateway and SQL Server on separate machines reduces resource contention.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ca508-118">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="ca508-118">Getting started</span></span>
<span data-ttu-id="ca508-119">A másolási tevékenység, amely a különböző eszközök/API-k használatával helyezi át az adatokat belőle egy helyi SQL Server-adatbázis egy folyamat hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="ca508-119">You can create a pipeline with a copy activity that moves data to/from an on-premises SQL Server database by using different tools/APIs.</span></span>

<span data-ttu-id="ca508-120">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="ca508-120">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="ca508-121">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="ca508-121">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="ca508-122">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="ca508-122">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="ca508-123">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="ca508-123">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="ca508-124">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="ca508-124">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span> 

1. <span data-ttu-id="ca508-125">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="ca508-125">Create a **data factory**.</span></span> <span data-ttu-id="ca508-126">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="ca508-126">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="ca508-127">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="ca508-127">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="ca508-128">Például adatokat az SQL Server-adatbázis egy Azure blob Storage másolása, akkor két társított szolgáltatások létrehozásához az SQL Server-adatbázis és az Azure storage-fiók összekapcsolása a data factory.</span><span class="sxs-lookup"><span data-stu-id="ca508-128">For example, if you are copying data from a SQL Server database to an Azure blob storage, you create two linked services to link your SQL Server database and Azure storage account to your data factory.</span></span> <span data-ttu-id="ca508-129">SQL Server-adatbázis jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca508-129">For linked service properties that are specific to SQL Server database, see [linked service properties](#linked-service-properties) section.</span></span> 
3. <span data-ttu-id="ca508-130">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="ca508-130">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="ca508-131">A példa az előző lépésben említett Ha meg szeretné adni az SQL-tábla az SQL Server-adatbázis a bemeneti adatokat tartalmazó adatkészlet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ca508-131">In the example mentioned in the last step, you create a dataset to specify the SQL table in your SQL Server database that contains the input data.</span></span> <span data-ttu-id="ca508-132">Továbbá adja meg a blob-tároló és a mappa, amely tárolja az adatokat másolni az SQL Server-adatbázisból egy másik dataset létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="ca508-132">And, you create another dataset to specify the blob container and the folder that holds the data copied from the SQL Server database.</span></span> <span data-ttu-id="ca508-133">SQL Server-adatbázis adott adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca508-133">For dataset properties that are specific to SQL Server database, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="ca508-134">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="ca508-134">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="ca508-135">A korábban említett példában SqlSource forrás-és BlobSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="ca508-135">In the example mentioned earlier, you use SqlSource as a source and BlobSink as a sink for the copy activity.</span></span> <span data-ttu-id="ca508-136">Hasonlóképpen ha SQL Server-adatbázis másolása az Azure Blob-tárolóból, használható BlobSource és SqlSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="ca508-136">Similarly, if you are copying from Azure Blob Storage to SQL Server Database, you use BlobSource and SqlSink in the copy activity.</span></span> <span data-ttu-id="ca508-137">SQL Server-adatbázis adott tevékenység Tulajdonságok másolása, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca508-137">For copy activity properties that are specific to SQL Server Database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="ca508-138">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ca508-138">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span> 

<span data-ttu-id="ca508-139">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="ca508-139">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="ca508-140">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="ca508-140">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="ca508-141">JSON-definíciók, amely segítségével másolja az adatokat a helyszíni SQL Server adatbázis az adat-előállító entitások minták, lásd: [JSON példák](#json-examples-for-copying-data-from-and-to-sql-server) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="ca508-141">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an on-premises SQL Server database, see [JSON examples](#json-examples-for-copying-data-from-and-to-sql-server) section of this article.</span></span> 

<span data-ttu-id="ca508-142">A következő szakaszok részletesen bemutatják az SQL Server Data Factory tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="ca508-142">The following sections provide details about JSON properties that are used to define Data Factory entities specific to SQL Server:</span></span> 

## <a name="linked-service-properties"></a><span data-ttu-id="ca508-143">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="ca508-143">Linked service properties</span></span>
<span data-ttu-id="ca508-144">Típusú társított szolgáltatás létrehozása **OnPremisesSqlServer** a helyszíni SQL Server adatbázis összekapcsolása egy adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="ca508-144">You create a linked service of type **OnPremisesSqlServer** to link an on-premises SQL Server database to a data factory.</span></span> <span data-ttu-id="ca508-145">A következő táblázat a JSON-elemek szerepelnek a helyszíni SQL Server kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="ca508-145">The following table provides description for JSON elements specific to on-premises SQL Server linked service.</span></span>

<span data-ttu-id="ca508-146">A következő táblázat a JSON-elemek szerepelnek kapcsolódó SQL Server-szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="ca508-146">The following table provides description for JSON elements specific to SQL Server linked service.</span></span>

| <span data-ttu-id="ca508-147">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ca508-147">Property</span></span> | <span data-ttu-id="ca508-148">Leírás</span><span class="sxs-lookup"><span data-stu-id="ca508-148">Description</span></span> | <span data-ttu-id="ca508-149">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ca508-149">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ca508-150">type</span><span class="sxs-lookup"><span data-stu-id="ca508-150">type</span></span> |<span data-ttu-id="ca508-151">A type tulajdonságot kell megadni: **OnPremisesSqlServer**.</span><span class="sxs-lookup"><span data-stu-id="ca508-151">The type property should be set to: **OnPremisesSqlServer**.</span></span> |<span data-ttu-id="ca508-152">Igen</span><span class="sxs-lookup"><span data-stu-id="ca508-152">Yes</span></span> |
| <span data-ttu-id="ca508-153">connectionString</span><span class="sxs-lookup"><span data-stu-id="ca508-153">connectionString</span></span> |<span data-ttu-id="ca508-154">Az SQL-hitelesítéssel vagy a Windows-hitelesítés a helyszíni SQL Server adatbázishoz való kapcsolódáshoz szükséges connectionString információkat adják meg.</span><span class="sxs-lookup"><span data-stu-id="ca508-154">Specify connectionString information needed to connect to the on-premises SQL Server database using either SQL authentication or Windows authentication.</span></span> |<span data-ttu-id="ca508-155">Igen</span><span class="sxs-lookup"><span data-stu-id="ca508-155">Yes</span></span> |
| <span data-ttu-id="ca508-156">gatewayName</span><span class="sxs-lookup"><span data-stu-id="ca508-156">gatewayName</span></span> |<span data-ttu-id="ca508-157">Neve az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia kell a helyszíni SQL Server adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="ca508-157">Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database.</span></span> |<span data-ttu-id="ca508-158">Igen</span><span class="sxs-lookup"><span data-stu-id="ca508-158">Yes</span></span> |
| <span data-ttu-id="ca508-159">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="ca508-159">username</span></span> |<span data-ttu-id="ca508-160">Adja meg a felhasználónevet, ha a Windows-hitelesítést használ.</span><span class="sxs-lookup"><span data-stu-id="ca508-160">Specify user name if you are using Windows Authentication.</span></span> <span data-ttu-id="ca508-161">Példa: **tartománynév\\felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="ca508-161">Example: **domainname\\username**.</span></span> |<span data-ttu-id="ca508-162">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-162">No</span></span> |
| <span data-ttu-id="ca508-163">jelszó</span><span class="sxs-lookup"><span data-stu-id="ca508-163">password</span></span> |<span data-ttu-id="ca508-164">Adja meg a felhasználónévhez megadott felhasználói fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ca508-164">Specify password for the user account you specified for the username.</span></span> |<span data-ttu-id="ca508-165">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-165">No</span></span> |

<span data-ttu-id="ca508-166">Hitelesítő adatok használatával titkosíthatja az **New-AzureRmDataFactoryEncryptValue** parancsmag és a következő példában látható módon használhatja őket a kapcsolódási karakterláncban (**EncryptedCredential** tulajdonság):</span><span class="sxs-lookup"><span data-stu-id="ca508-166">You can encrypt credentials using the **New-AzureRmDataFactoryEncryptValue** cmdlet and use them in the connection string as shown in the following example (**EncryptedCredential** property):</span></span>  

```JSON
"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;EncryptedCredential=<encrypted credential>",
```

### <a name="samples"></a><span data-ttu-id="ca508-167">Példák</span><span class="sxs-lookup"><span data-stu-id="ca508-167">Samples</span></span>
<span data-ttu-id="ca508-168">**Az SQL-hitelesítéssel JSON**</span><span class="sxs-lookup"><span data-stu-id="ca508-168">**JSON for using SQL Authentication**</span></span>

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
<span data-ttu-id="ca508-169">**A Windows-hitelesítést használó JSON**</span><span class="sxs-lookup"><span data-stu-id="ca508-169">**JSON for using Windows Authentication**</span></span>

<span data-ttu-id="ca508-170">Az adatkezelési átjáró fog megszemélyesíteni a megadott felhasználói fióknak a helyi SQL Server-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="ca508-170">Data Management Gateway will impersonate the specified user account to connect to the on-premises SQL Server database.</span></span> 

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

## <a name="dataset-properties"></a><span data-ttu-id="ca508-171">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ca508-171">Dataset properties</span></span>
<span data-ttu-id="ca508-172">A minták használja egy adatkészlet típusú **SqlServerTable** képviselő egy SQL Server adatbázis egyik táblája.</span><span class="sxs-lookup"><span data-stu-id="ca508-172">In the samples, you have used a dataset of type **SqlServerTable** to represent a table in a SQL Server database.</span></span>  

<span data-ttu-id="ca508-173">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ca508-173">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="ca508-174">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (SQL Server, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="ca508-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (SQL Server, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="ca508-175">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="ca508-175">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="ca508-176">A **typeProperties** szakasz az adatkészlet típusú **SqlServerTable** tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="ca508-176">The **typeProperties** section for the dataset of type **SqlServerTable** has the following properties:</span></span>

| <span data-ttu-id="ca508-177">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ca508-177">Property</span></span> | <span data-ttu-id="ca508-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="ca508-178">Description</span></span> | <span data-ttu-id="ca508-179">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ca508-179">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ca508-180">tableName</span><span class="sxs-lookup"><span data-stu-id="ca508-180">tableName</span></span> |<span data-ttu-id="ca508-181">A tábla vagy nézet, amelyre a társított szolgáltatás SQL Server adatbázis-példány neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ca508-181">Name of the table or view in the SQL Server Database instance that linked service refers to.</span></span> |<span data-ttu-id="ca508-182">Igen</span><span class="sxs-lookup"><span data-stu-id="ca508-182">Yes</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="ca508-183">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="ca508-183">Copy activity properties</span></span>
<span data-ttu-id="ca508-184">Ha adatokat egy SQL Server-adatbázisból, a másolási tevékenység beállítása a forrástípus **SqlSource**.</span><span class="sxs-lookup"><span data-stu-id="ca508-184">If you are moving data from a SQL Server database, you set the source type in the copy activity to **SqlSource**.</span></span> <span data-ttu-id="ca508-185">Hasonlóképpen, ha adatok SQL Server-adatbázishoz, beállítása a fogadó típusa a másolási tevékenység **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="ca508-185">Similarly, if you are moving data to a SQL Server database, you set the sink type in the copy activity to **SqlSink**.</span></span> <span data-ttu-id="ca508-186">Ez a témakör SqlSource és SqlSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="ca508-186">This section provides a list of properties supported by SqlSource and SqlSink.</span></span>

<span data-ttu-id="ca508-187">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ca508-187">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="ca508-188">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="ca508-188">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="ca508-189">A másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="ca508-189">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="ca508-190">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="ca508-190">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="ca508-191">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="ca508-191">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="sqlsource"></a><span data-ttu-id="ca508-192">SqlSource</span><span class="sxs-lookup"><span data-stu-id="ca508-192">SqlSource</span></span>
<span data-ttu-id="ca508-193">Ha a másolási tevékenység során a forrás típusa nem **SqlSource**, a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="ca508-193">When source in a copy activity is of type **SqlSource**, the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="ca508-194">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ca508-194">Property</span></span> | <span data-ttu-id="ca508-195">Leírás</span><span class="sxs-lookup"><span data-stu-id="ca508-195">Description</span></span> | <span data-ttu-id="ca508-196">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="ca508-196">Allowed values</span></span> | <span data-ttu-id="ca508-197">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ca508-197">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ca508-198">sqlReaderQuery</span><span class="sxs-lookup"><span data-stu-id="ca508-198">sqlReaderQuery</span></span> |<span data-ttu-id="ca508-199">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="ca508-199">Use the custom query to read data.</span></span> |<span data-ttu-id="ca508-200">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="ca508-200">SQL query string.</span></span> <span data-ttu-id="ca508-201">Például: Válasszon * from tábla.</span><span class="sxs-lookup"><span data-stu-id="ca508-201">For example: select * from MyTable.</span></span> <span data-ttu-id="ca508-202">Előfordulhat, hogy a bemeneti adatkészlet által hivatkozott adatbázishoz több táblát is hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ca508-202">May reference multiple tables from the database referenced by the input dataset.</span></span> <span data-ttu-id="ca508-203">Ha nincs megadva, az SQL-utasítás végrehajtott: táblanév kiválaszthatja.</span><span class="sxs-lookup"><span data-stu-id="ca508-203">If not specified, the SQL statement that is executed: select from MyTable.</span></span> |<span data-ttu-id="ca508-204">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-204">No</span></span> |
| <span data-ttu-id="ca508-205">sqlReaderStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="ca508-205">sqlReaderStoredProcedureName</span></span> |<span data-ttu-id="ca508-206">A tárolt eljárás, amely adatokat olvas a forrástábla neve.</span><span class="sxs-lookup"><span data-stu-id="ca508-206">Name of the stored procedure that reads data from the source table.</span></span> |<span data-ttu-id="ca508-207">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="ca508-207">Name of the stored procedure.</span></span> <span data-ttu-id="ca508-208">Az utolsó SQL-utasítás a következő tárolt eljárást a SELECT utasítással kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ca508-208">The last SQL statement must be a SELECT statement in the stored procedure.</span></span> |<span data-ttu-id="ca508-209">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-209">No</span></span> |
| <span data-ttu-id="ca508-210">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="ca508-210">storedProcedureParameters</span></span> |<span data-ttu-id="ca508-211">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ca508-211">Parameters for the stored procedure.</span></span> |<span data-ttu-id="ca508-212">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="ca508-212">Name/value pairs.</span></span> <span data-ttu-id="ca508-213">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ca508-213">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="ca508-214">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-214">No</span></span> |

<span data-ttu-id="ca508-215">Ha a **sqlReaderQuery** van megadva a SqlSource, a másolási tevékenység során ez a lekérdezés fut az SQL Server-adatbázis forrás az adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ca508-215">If the **sqlReaderQuery** is specified for the SqlSource, the Copy Activity runs this query against the SQL Server Database source to get the data.</span></span>

<span data-ttu-id="ca508-216">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="ca508-216">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span>

<span data-ttu-id="ca508-217">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, struktúra szakaszában meghatározott oszlopokat válassza futtatni az SQL Server adatbázis-lekérdezés összeállításához használt.</span><span class="sxs-lookup"><span data-stu-id="ca508-217">If you do not specify either sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="ca508-218">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="ca508-218">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

> [!NOTE]
> <span data-ttu-id="ca508-219">Amikor **sqlReaderStoredProcedureName**, továbbra is meg kell adnia egy értéket a **tableName** az adatkészlet JSON tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="ca508-219">When you use **sqlReaderStoredProcedureName**, you still need to specify a value for the **tableName** property in the dataset JSON.</span></span> <span data-ttu-id="ca508-220">Nincs érvényesítést hajt végre ezt a táblázatot, ha van.</span><span class="sxs-lookup"><span data-stu-id="ca508-220">There are no validations performed against this table though.</span></span>

### <a name="sqlsink"></a><span data-ttu-id="ca508-221">SqlSink</span><span class="sxs-lookup"><span data-stu-id="ca508-221">SqlSink</span></span>
<span data-ttu-id="ca508-222">**SqlSink** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="ca508-222">**SqlSink** supports the following properties:</span></span>

| <span data-ttu-id="ca508-223">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="ca508-223">Property</span></span> | <span data-ttu-id="ca508-224">Leírás</span><span class="sxs-lookup"><span data-stu-id="ca508-224">Description</span></span> | <span data-ttu-id="ca508-225">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="ca508-225">Allowed values</span></span> | <span data-ttu-id="ca508-226">Szükséges</span><span class="sxs-lookup"><span data-stu-id="ca508-226">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="ca508-227">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="ca508-227">writeBatchTimeout</span></span> |<span data-ttu-id="ca508-228">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="ca508-228">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="ca508-229">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ca508-229">timespan</span></span><br/><br/> <span data-ttu-id="ca508-230">Példa: "00: 30:00" (30 perc).</span><span class="sxs-lookup"><span data-stu-id="ca508-230">Example: “00:30:00” (30 minutes).</span></span> |<span data-ttu-id="ca508-231">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-231">No</span></span> |
| <span data-ttu-id="ca508-232">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="ca508-232">writeBatchSize</span></span> |<span data-ttu-id="ca508-233">Szúr be az SQL-tábla adatokat, amikor a puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="ca508-233">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="ca508-234">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="ca508-234">Integer (number of rows)</span></span> |<span data-ttu-id="ca508-235">Nem (alapértelmezett: 10000)</span><span class="sxs-lookup"><span data-stu-id="ca508-235">No (default: 10000)</span></span> |
| <span data-ttu-id="ca508-236">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="ca508-236">sqlWriterCleanupScript</span></span> |<span data-ttu-id="ca508-237">Adja meg a lekérdezést úgy, hogy egy adott szelet adatait végrehajtásához másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="ca508-237">Specify query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> <span data-ttu-id="ca508-238">További információkért lásd: [ismételhető másolási](#repeatable-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca508-238">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="ca508-239">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="ca508-239">A query statement.</span></span> |<span data-ttu-id="ca508-240">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-240">No</span></span> |
| <span data-ttu-id="ca508-241">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="ca508-241">sliceIdentifierColumnName</span></span> |<span data-ttu-id="ca508-242">Adja meg a másolási tevékenység során automatikusan létrejön szelet azonosító, amely segítségével távolítja el az adatokat egy adott szelet, amikor futtassa újra a töltse ki az oszlopnevet.</span><span class="sxs-lookup"><span data-stu-id="ca508-242">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> <span data-ttu-id="ca508-243">További információkért lásd: [ismételhető másolási](#repeatable-copy) szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca508-243">For more information, see [repeatable copy](#repeatable-copy) section.</span></span> |<span data-ttu-id="ca508-244">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="ca508-244">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="ca508-245">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-245">No</span></span> |
| <span data-ttu-id="ca508-246">sqlWriterStoredProcedureName</span><span class="sxs-lookup"><span data-stu-id="ca508-246">sqlWriterStoredProcedureName</span></span> |<span data-ttu-id="ca508-247">A tárolt eljárás neve a cél táblázatba upserts (frissítés/Beszúrás) adatok.</span><span class="sxs-lookup"><span data-stu-id="ca508-247">Name of the stored procedure that upserts (updates/inserts) data into the target table.</span></span> |<span data-ttu-id="ca508-248">A tárolt eljárás neve.</span><span class="sxs-lookup"><span data-stu-id="ca508-248">Name of the stored procedure.</span></span> |<span data-ttu-id="ca508-249">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-249">No</span></span> |
| <span data-ttu-id="ca508-250">storedProcedureParameters</span><span class="sxs-lookup"><span data-stu-id="ca508-250">storedProcedureParameters</span></span> |<span data-ttu-id="ca508-251">A tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ca508-251">Parameters for the stored procedure.</span></span> |<span data-ttu-id="ca508-252">A név/érték párok.</span><span class="sxs-lookup"><span data-stu-id="ca508-252">Name/value pairs.</span></span> <span data-ttu-id="ca508-253">Nevét és a kis-és a paraméterek meg kell egyeznie a nevek és a kis-és nagybetűhasználat a tárolt eljárás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="ca508-253">Names and casing of parameters must match the names and casing of the stored procedure parameters.</span></span> |<span data-ttu-id="ca508-254">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-254">No</span></span> |
| <span data-ttu-id="ca508-255">sqlWriterTableType</span><span class="sxs-lookup"><span data-stu-id="ca508-255">sqlWriterTableType</span></span> |<span data-ttu-id="ca508-256">Adja meg a tárolt eljárásban használandó tábla neve.</span><span class="sxs-lookup"><span data-stu-id="ca508-256">Specify table type name to be used in the stored procedure.</span></span> <span data-ttu-id="ca508-257">Másolási tevékenység elérhetővé teszi az adatok áthelyezése egy ideiglenes táblát, amely a táblatípus.</span><span class="sxs-lookup"><span data-stu-id="ca508-257">Copy activity makes the data being moved available in a temp table with this table type.</span></span> <span data-ttu-id="ca508-258">Tárolt eljárás kódot is majd egyesítheti az adatokat, a meglévő adatok másolásának.</span><span class="sxs-lookup"><span data-stu-id="ca508-258">Stored procedure code can then merge the data being copied with existing data.</span></span> |<span data-ttu-id="ca508-259">Egy tábla környezettípus nevét.</span><span class="sxs-lookup"><span data-stu-id="ca508-259">A table type name.</span></span> |<span data-ttu-id="ca508-260">Nem</span><span class="sxs-lookup"><span data-stu-id="ca508-260">No</span></span> |


## <a name="json-examples-for-copying-data-from-and-to-sql-server"></a><span data-ttu-id="ca508-261">Adatok másolása a kezdő és a SQL Server JSON példák</span><span class="sxs-lookup"><span data-stu-id="ca508-261">JSON examples for copying data from and to SQL Server</span></span>
<span data-ttu-id="ca508-262">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="ca508-262">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="ca508-263">A következő minták adatok másolása az SQL Server és az Azure Blob Storage megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="ca508-263">The following samples show how to copy data to and from SQL Server and Azure Blob Storage.</span></span> <span data-ttu-id="ca508-264">Azonban az adatok átmásolhatók **közvetlenül** a forrásokban, sem a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="ca508-264">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>     

## <a name="example-copy-data-from-sql-server-to-azure-blob"></a><span data-ttu-id="ca508-265">Példa: Adatok másolása az SQL Server az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="ca508-265">Example: Copy data from SQL Server to Azure Blob</span></span>
<span data-ttu-id="ca508-266">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="ca508-266">The following sample shows:</span></span>

1. <span data-ttu-id="ca508-267">A társított szolgáltatás típusa [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-267">A linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="ca508-268">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-268">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="ca508-269">Bemeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-269">An input [dataset](data-factory-create-datasets.md) of type [SqlServerTable](#dataset-properties).</span></span>
4. <span data-ttu-id="ca508-270">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-270">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="ca508-271">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-271">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="ca508-272">A minta-idősoros adatok egy SQL Server tábla másolja az Azure blob minden órában.</span><span class="sxs-lookup"><span data-stu-id="ca508-272">The sample copies time-series data from a SQL Server table to an Azure blob every hour.</span></span> <span data-ttu-id="ca508-273">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ca508-273">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="ca508-274">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="ca508-274">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="ca508-275">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="ca508-275">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

<span data-ttu-id="ca508-276">**Kapcsolódó SQL Server szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="ca508-276">**SQL Server linked service**</span></span>
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
<span data-ttu-id="ca508-277">**Az Azure Blob storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="ca508-277">**Azure Blob storage linked service**</span></span>

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
<span data-ttu-id="ca508-278">**SQL Server bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="ca508-278">**SQL Server input dataset**</span></span>

<span data-ttu-id="ca508-279">A minta azt feltételezi, hogy létrehozott egy tábla "MyTable" SQL Server és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ca508-279">The sample assumes you have created a table “MyTable” in SQL Server and it contains a column called “timestampcolumn” for time series data.</span></span> <span data-ttu-id="ca508-280">Lekérheti az egyetlen adatkészlet ugyanazon adatbázis több tábla keresztül, de egyetlen tábla a dataset tableName typeProperty kell használni.</span><span class="sxs-lookup"><span data-stu-id="ca508-280">You can query over multiple tables within the same database using a single dataset, but a single table must be used for the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="ca508-281">"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="ca508-281">Setting “external”: ”true” informs Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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
<span data-ttu-id="ca508-282">**Az Azure Blob kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="ca508-282">**Azure Blob output dataset**</span></span>

<span data-ttu-id="ca508-283">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="ca508-283">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ca508-284">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="ca508-284">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="ca508-285">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="ca508-285">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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
<span data-ttu-id="ca508-286">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="ca508-286">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="ca508-287">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="ca508-287">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="ca508-288">Az adatcsatorna JSON-definícióból a **forrás** típusúra **SqlSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="ca508-288">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="ca508-289">A megadott SQL-lekérdezést a **SqlReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="ca508-289">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

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
<span data-ttu-id="ca508-290">Ebben a példában **sqlReaderQuery** a SqlSource van megadva.</span><span class="sxs-lookup"><span data-stu-id="ca508-290">In this example, **sqlReaderQuery** is specified for the SqlSource.</span></span> <span data-ttu-id="ca508-291">A másolási tevékenység során ez a lekérdezés fut az SQL Server-adatbázis forrás az adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="ca508-291">The Copy Activity runs this query against the SQL Server Database source to get the data.</span></span> <span data-ttu-id="ca508-292">Másik lehetőségként megadhat tárolt eljárás megadásával a **sqlReaderStoredProcedureName** és **storedProcedureParameters** (Ha a tárolt eljárás paraméterek fogadja el).</span><span class="sxs-lookup"><span data-stu-id="ca508-292">Alternatively, you can specify a stored procedure by specifying the **sqlReaderStoredProcedureName** and **storedProcedureParameters** (if the stored procedure takes parameters).</span></span> <span data-ttu-id="ca508-293">A sqlReaderQuery hivatkozhat több táblák az adatbázisban a következő bemeneti adatkészlet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="ca508-293">The sqlReaderQuery can reference multiple tables within the database referenced by the input dataset.</span></span> <span data-ttu-id="ca508-294">Nincs korlátozva csak a tábla a dataset tableName typeProperty állítja be.</span><span class="sxs-lookup"><span data-stu-id="ca508-294">It is not limited to only the table set as the dataset's tableName typeProperty.</span></span>

<span data-ttu-id="ca508-295">Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, struktúra szakaszában meghatározott oszlopokat válassza futtatni az SQL Server adatbázis-lekérdezés összeállításához használt.</span><span class="sxs-lookup"><span data-stu-id="ca508-295">If you do not specify sqlReaderQuery or sqlReaderStoredProcedureName, the columns defined in the structure section are used to build a select query to run against the SQL Server Database.</span></span> <span data-ttu-id="ca508-296">Az adatkészlet-definícióban nem rendelkezik a struktúra, ha minden kiválasztott oszlop. a táblából.</span><span class="sxs-lookup"><span data-stu-id="ca508-296">If the dataset definition does not have the structure, all columns are selected from the table.</span></span>

<span data-ttu-id="ca508-297">Tekintse meg a [Sql-forrás](#sqlsource) szakasz és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource és BlobSink által támogatott tulajdonságok listája.</span><span class="sxs-lookup"><span data-stu-id="ca508-297">See the [Sql Source](#sqlsource) section and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) for the list of properties supported by SqlSource and BlobSink.</span></span>

## <a name="example-copy-data-from-azure-blob-to-sql-server"></a><span data-ttu-id="ca508-298">Példa: Adatok másolása az Azure Blob az SQL Server</span><span class="sxs-lookup"><span data-stu-id="ca508-298">Example: Copy data from Azure Blob to SQL Server</span></span>
<span data-ttu-id="ca508-299">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="ca508-299">The following sample shows:</span></span>

1. <span data-ttu-id="ca508-300">A társított szolgáltatás típusa [OnPremisesSqlServer](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-300">The linked service of type [OnPremisesSqlServer](#linked-service-properties).</span></span>
2. <span data-ttu-id="ca508-301">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-301">The linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="ca508-302">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-302">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="ca508-303">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-303">An output [dataset](data-factory-create-datasets.md) of type [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="ca508-304">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlSink](#sql-server-copy-activity-type-properties).</span><span class="sxs-lookup"><span data-stu-id="ca508-304">The [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) and [SqlSink](#sql-server-copy-activity-type-properties).</span></span>

<span data-ttu-id="ca508-305">A minta másolatok idősorozat adatokat az Azure blob-egy SQL Server minden órában tábla.</span><span class="sxs-lookup"><span data-stu-id="ca508-305">The sample copies time-series data from an Azure blob to a SQL Server table every hour.</span></span> <span data-ttu-id="ca508-306">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="ca508-306">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="ca508-307">**Kapcsolódó SQL Server szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="ca508-307">**SQL Server linked service**</span></span>

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
<span data-ttu-id="ca508-308">**Az Azure Blob storage társított szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="ca508-308">**Azure Blob storage linked service**</span></span>

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
<span data-ttu-id="ca508-309">**Az Azure Blob bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="ca508-309">**Azure Blob input dataset**</span></span>

<span data-ttu-id="ca508-310">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="ca508-310">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="ca508-311">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="ca508-311">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="ca508-312">A mappa elérési útját használja év, hónap és nap részét kezdési idejét, valamint fájl nevét a kezdő időpontja óra részét.</span><span class="sxs-lookup"><span data-stu-id="ca508-312">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="ca508-313">"external": "true" beállítás arról értesíti az, hogy az adatkészlet data factoryval való külső, és egy tevékenység adat-előállító nem hozzák a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="ca508-313">“external”: “true” setting informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

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
<span data-ttu-id="ca508-314">**SQL Server kimeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="ca508-314">**SQL Server output dataset**</span></span>

<span data-ttu-id="ca508-315">A minta másolja az adatokat az SQL Server "MyTable" nevű tábla.</span><span class="sxs-lookup"><span data-stu-id="ca508-315">The sample copies data to a table named “MyTable” in SQL Server.</span></span> <span data-ttu-id="ca508-316">A tábla létrehozása az SQL Server azonos számú oszlopot a Blob CSV-fájl tartalmazza a várt módon.</span><span class="sxs-lookup"><span data-stu-id="ca508-316">Create the table in SQL Server with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="ca508-317">Új sorok hozzáadásakor a tábla minden órában.</span><span class="sxs-lookup"><span data-stu-id="ca508-317">New rows are added to the table every hour.</span></span>

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
<span data-ttu-id="ca508-318">**A másolási tevékenység-feldolgozási folyamat**</span><span class="sxs-lookup"><span data-stu-id="ca508-318">**Pipeline with Copy activity**</span></span>

<span data-ttu-id="ca508-319">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="ca508-319">The pipeline contains a Copy Activity that is configured to use these input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="ca508-320">Az adatcsatorna JSON-definícióból a **forrás** típusúra **BlobSource** és **fogadó** típusúra **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="ca508-320">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

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

## <a name="troubleshooting-connection-issues"></a><span data-ttu-id="ca508-321">Kapcsolati problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="ca508-321">Troubleshooting connection issues</span></span>
1. <span data-ttu-id="ca508-322">Konfigurálja az SQL Server távoli kapcsolatokat fogadjon.</span><span class="sxs-lookup"><span data-stu-id="ca508-322">Configure your SQL Server to accept remote connections.</span></span> <span data-ttu-id="ca508-323">Indítsa el **SQL Server Management Studio**, kattintson a jobb gombbal **server**, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="ca508-323">Launch **SQL Server Management Studio**, right-click **server**, and click **Properties**.</span></span> <span data-ttu-id="ca508-324">Válassza ki **kapcsolatok** csoportot a listából, és ellenőrzés **a kiszolgáló távoli kapcsolatok engedélyezése a**.</span><span class="sxs-lookup"><span data-stu-id="ca508-324">Select **Connections** from the list and check **Allow remote connections to the server**.</span></span>

    ![Távoli kapcsolatok engedélyezése](./media/data-factory-sqlserver-connector/AllowRemoteConnections.png)

    <span data-ttu-id="ca508-326">Lásd: [konfigurálja a távelérési kiszolgálói konfigurációs beállítás megadása](https://msdn.microsoft.com/library/ms191464.aspx) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ca508-326">See [Configure the remote access Server Configuration Option](https://msdn.microsoft.com/library/ms191464.aspx) for detailed steps.</span></span>
2. <span data-ttu-id="ca508-327">Indítsa el **SQL Server Konfigurációkezelő**.</span><span class="sxs-lookup"><span data-stu-id="ca508-327">Launch **SQL Server Configuration Manager**.</span></span> <span data-ttu-id="ca508-328">Bontsa ki a **SQL Server hálózati konfigurációja** , és válassza ki a példány **MSSQLSERVER protokolljai**.</span><span class="sxs-lookup"><span data-stu-id="ca508-328">Expand **SQL Server Network Configuration** for the instance you want, and select **Protocols for MSSQLSERVER**.</span></span> <span data-ttu-id="ca508-329">A jobb oldali ablaktáblában protokollok kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="ca508-329">You should see protocols in the right-pane.</span></span> <span data-ttu-id="ca508-330">Engedélyezze a TCP/IP protokollt kattintson a jobb gombbal **TCP/IP** elemre kattintva **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="ca508-330">Enable TCP/IP by right-clicking **TCP/IP** and clicking **Enable**.</span></span>

    ![Engedélyezze a TCP/IP protokollt](./media/data-factory-sqlserver-connector/EnableTCPProptocol.png)

    <span data-ttu-id="ca508-332">Lásd: [engedélyezheti vagy tilthatja le a hálózati protokoll](https://msdn.microsoft.com/library/ms191294.aspx) részleteit és más módon, hogy a TCP/IP-protokoll.</span><span class="sxs-lookup"><span data-stu-id="ca508-332">See [Enable or Disable a Server Network Protocol](https://msdn.microsoft.com/library/ms191294.aspx) for details and alternate ways of enabling TCP/IP protocol.</span></span>
3. <span data-ttu-id="ca508-333">Az azonos ablakban kattintson duplán **TCP/IP** indítása **TCP/IP-tulajdonságok** ablak.</span><span class="sxs-lookup"><span data-stu-id="ca508-333">In the same window, double-click **TCP/IP** to launch **TCP/IP Properties** window.</span></span>
4. <span data-ttu-id="ca508-334">Váltás a **IP-címek** fülre.</span><span class="sxs-lookup"><span data-stu-id="ca508-334">Switch to the **IP Addresses** tab.</span></span> <span data-ttu-id="ca508-335">Görgessen le lásd: **IPAll** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca508-335">Scroll down to see **IPAll** section.</span></span> <span data-ttu-id="ca508-336">Jegyezze fel a ** TCP-Port ** (alapértelmezett érték a **1433**).</span><span class="sxs-lookup"><span data-stu-id="ca508-336">Note down the **TCP Port **(default is **1433**).</span></span>
5. <span data-ttu-id="ca508-337">Hozzon létre egy **szabály a Windows tűzfal** ezen a porton keresztül bejövő adatforgalmat engedélyezi a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ca508-337">Create a **rule for the Windows Firewall** on the machine to allow incoming traffic through this port.</span></span>  
6. <span data-ttu-id="ca508-338">**Ellenőrizze a kapcsolat**: teljesen minősített nevet az SQL Serverhez való kapcsolódáshoz használja az SQL Server Management Studio egy másik gépről.</span><span class="sxs-lookup"><span data-stu-id="ca508-338">**Verify connection**: To connect to the SQL Server using fully qualified name, use SQL Server Management Studio from a different machine.</span></span> <span data-ttu-id="ca508-339">Például: "<machine>.<domain>. Corp.<company>.com, 1433. "</span><span class="sxs-lookup"><span data-stu-id="ca508-339">For example: "<machine>.<domain>.corp.<company>.com,1433."</span></span>

   > [!IMPORTANT]

   > <span data-ttu-id="ca508-340">Lásd: [helyezze át az adatokat a helyszíni adatforrások és az adatkezelési átjáró a felhő közötti](data-factory-move-data-between-onprem-and-cloud.md) részletes információkat.</span><span class="sxs-lookup"><span data-stu-id="ca508-340">See [Move data between on-premises sources and the cloud with Data Management Gateway](data-factory-move-data-between-onprem-and-cloud.md) for detailed information.</span></span>
   >
   > <span data-ttu-id="ca508-341">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="ca508-341">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>
   >
   >


## <a name="identity-columns-in-the-target-database"></a><span data-ttu-id="ca508-342">A céladatbázis azonosító oszlop</span><span class="sxs-lookup"><span data-stu-id="ca508-342">Identity columns in the target database</span></span>
<span data-ttu-id="ca508-343">Ez a szakasz azt mutatja, hogy adatokat másol a forrástábla nem azonosító oszlop az azonosító oszlopot tartalmazó táblát.</span><span class="sxs-lookup"><span data-stu-id="ca508-343">This section provides an example that copies data from a source table with no identity column to a destination table with an identity column.</span></span>

<span data-ttu-id="ca508-344">**Forrástábla:**</span><span class="sxs-lookup"><span data-stu-id="ca508-344">**Source table:**</span></span>

```sql
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
<span data-ttu-id="ca508-345">**Céltábla:**</span><span class="sxs-lookup"><span data-stu-id="ca508-345">**Destination table:**</span></span>

```sql
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```

<span data-ttu-id="ca508-346">Figyelje meg, hogy a céltábla rendelkezik-e az azonosító oszlop.</span><span class="sxs-lookup"><span data-stu-id="ca508-346">Notice that the target table has an identity column.</span></span>

<span data-ttu-id="ca508-347">**Forrás adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="ca508-347">**Source dataset JSON definition**</span></span>

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
<span data-ttu-id="ca508-348">**Cél adatkészlet JSON-definícióból**</span><span class="sxs-lookup"><span data-stu-id="ca508-348">**Destination dataset JSON definition**</span></span>

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

<span data-ttu-id="ca508-349">Figyelje meg, hogy a forrás és cél táblázatként különböző sémája (cél rendelkezik egy olyan további oszlop identitású).</span><span class="sxs-lookup"><span data-stu-id="ca508-349">Notice that as your source and target table have different schema (target has an additional column with identity).</span></span> <span data-ttu-id="ca508-350">Ilyen esetben meg kell adnia **struktúra** tulajdonság az a tároló adatkészlet-definícióban, amely nem tartalmazza az identitásoszlop.</span><span class="sxs-lookup"><span data-stu-id="ca508-350">In this scenario, you need to specify **structure** property in the target dataset definition, which doesn’t include the identity column.</span></span>

## <a name="invoke-stored-procedure-from-sql-sink"></a><span data-ttu-id="ca508-351">A fogadó SQL tárolt eljárás meghívása</span><span class="sxs-lookup"><span data-stu-id="ca508-351">Invoke stored procedure from SQL sink</span></span>
<span data-ttu-id="ca508-352">Lásd: [fogadó SQL tárolt eljárás meghívása a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md) cikk SQL fogadó a folyamat a másolási tevékenység a tárolt eljárás meghívása példát.</span><span class="sxs-lookup"><span data-stu-id="ca508-352">See [Invoke stored procedure for SQL sink in copy activity](data-factory-invoke-stored-procedure-from-copy-activity.md) article for an example of invoking a stored procedure from SQL sink in a copy activity of a pipeline.</span></span>

## <a name="type-mapping-for-sql-server"></a><span data-ttu-id="ca508-353">Az SQL server leképezésének</span><span class="sxs-lookup"><span data-stu-id="ca508-353">Type mapping for SQL server</span></span>
<span data-ttu-id="ca508-354">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység során hajtja végre a módszert használja a következő 2. lépés típusok gyűjtése eseményforrás-típusnak automatikus típuskonverziók:</span><span class="sxs-lookup"><span data-stu-id="ca508-354">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, the Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="ca508-355">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="ca508-355">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="ca508-356">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="ca508-356">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="ca508-357">Ha adatok áthelyezése az & SQL Server, a következő megfeleltetéseket használ az SQL-típus a .NET-típus, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="ca508-357">When moving data to & from SQL server, the following mappings are used from SQL type to .NET type and vice versa.</span></span>

<span data-ttu-id="ca508-358">Leképezése nem ugyanaz, mint az SQL Server adattípus-hozzárendelése az ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="ca508-358">The mapping is same as the SQL Server Data Type Mapping for ADO.NET.</span></span>

| <span data-ttu-id="ca508-359">SQL Server adatbázismotor típusa</span><span class="sxs-lookup"><span data-stu-id="ca508-359">SQL Server Database Engine type</span></span> | <span data-ttu-id="ca508-360">.NET-keretrendszer típusa</span><span class="sxs-lookup"><span data-stu-id="ca508-360">.NET Framework type</span></span> |
| --- | --- |
| <span data-ttu-id="ca508-361">bigint</span><span class="sxs-lookup"><span data-stu-id="ca508-361">bigint</span></span> |<span data-ttu-id="ca508-362">Int64</span><span class="sxs-lookup"><span data-stu-id="ca508-362">Int64</span></span> |
| <span data-ttu-id="ca508-363">Bináris</span><span class="sxs-lookup"><span data-stu-id="ca508-363">binary</span></span> |<span data-ttu-id="ca508-364">Byte]</span><span class="sxs-lookup"><span data-stu-id="ca508-364">Byte[]</span></span> |
| <span data-ttu-id="ca508-365">bit</span><span class="sxs-lookup"><span data-stu-id="ca508-365">bit</span></span> |<span data-ttu-id="ca508-366">Logikai érték</span><span class="sxs-lookup"><span data-stu-id="ca508-366">Boolean</span></span> |
| <span data-ttu-id="ca508-367">Karakter</span><span class="sxs-lookup"><span data-stu-id="ca508-367">char</span></span> |<span data-ttu-id="ca508-368">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ca508-368">String, Char[]</span></span> |
| <span data-ttu-id="ca508-369">Dátum</span><span class="sxs-lookup"><span data-stu-id="ca508-369">date</span></span> |<span data-ttu-id="ca508-370">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ca508-370">DateTime</span></span> |
| <span data-ttu-id="ca508-371">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ca508-371">Datetime</span></span> |<span data-ttu-id="ca508-372">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ca508-372">DateTime</span></span> |
| <span data-ttu-id="ca508-373">datetime2</span><span class="sxs-lookup"><span data-stu-id="ca508-373">datetime2</span></span> |<span data-ttu-id="ca508-374">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ca508-374">DateTime</span></span> |
| <span data-ttu-id="ca508-375">datetimeoffset</span><span class="sxs-lookup"><span data-stu-id="ca508-375">Datetimeoffset</span></span> |<span data-ttu-id="ca508-376">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ca508-376">DateTimeOffset</span></span> |
| <span data-ttu-id="ca508-377">Decimális</span><span class="sxs-lookup"><span data-stu-id="ca508-377">Decimal</span></span> |<span data-ttu-id="ca508-378">Decimális</span><span class="sxs-lookup"><span data-stu-id="ca508-378">Decimal</span></span> |
| <span data-ttu-id="ca508-379">A FILESTREAM attribútum (varbinary(max))</span><span class="sxs-lookup"><span data-stu-id="ca508-379">FILESTREAM attribute (varbinary(max))</span></span> |<span data-ttu-id="ca508-380">Byte]</span><span class="sxs-lookup"><span data-stu-id="ca508-380">Byte[]</span></span> |
| <span data-ttu-id="ca508-381">Lebegőpontos</span><span class="sxs-lookup"><span data-stu-id="ca508-381">Float</span></span> |<span data-ttu-id="ca508-382">Dupla</span><span class="sxs-lookup"><span data-stu-id="ca508-382">Double</span></span> |
| <span data-ttu-id="ca508-383">Kép</span><span class="sxs-lookup"><span data-stu-id="ca508-383">image</span></span> |<span data-ttu-id="ca508-384">Byte]</span><span class="sxs-lookup"><span data-stu-id="ca508-384">Byte[]</span></span> |
| <span data-ttu-id="ca508-385">int</span><span class="sxs-lookup"><span data-stu-id="ca508-385">int</span></span> |<span data-ttu-id="ca508-386">Int32</span><span class="sxs-lookup"><span data-stu-id="ca508-386">Int32</span></span> |
| <span data-ttu-id="ca508-387">pénz</span><span class="sxs-lookup"><span data-stu-id="ca508-387">money</span></span> |<span data-ttu-id="ca508-388">Decimális</span><span class="sxs-lookup"><span data-stu-id="ca508-388">Decimal</span></span> |
| <span data-ttu-id="ca508-389">nchar</span><span class="sxs-lookup"><span data-stu-id="ca508-389">nchar</span></span> |<span data-ttu-id="ca508-390">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ca508-390">String, Char[]</span></span> |
| <span data-ttu-id="ca508-391">ntext</span><span class="sxs-lookup"><span data-stu-id="ca508-391">ntext</span></span> |<span data-ttu-id="ca508-392">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ca508-392">String, Char[]</span></span> |
| <span data-ttu-id="ca508-393">Numerikus</span><span class="sxs-lookup"><span data-stu-id="ca508-393">numeric</span></span> |<span data-ttu-id="ca508-394">Decimális</span><span class="sxs-lookup"><span data-stu-id="ca508-394">Decimal</span></span> |
| <span data-ttu-id="ca508-395">nvarchar</span><span class="sxs-lookup"><span data-stu-id="ca508-395">nvarchar</span></span> |<span data-ttu-id="ca508-396">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ca508-396">String, Char[]</span></span> |
| <span data-ttu-id="ca508-397">valós</span><span class="sxs-lookup"><span data-stu-id="ca508-397">real</span></span> |<span data-ttu-id="ca508-398">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="ca508-398">Single</span></span> |
| <span data-ttu-id="ca508-399">ROWVERSION</span><span class="sxs-lookup"><span data-stu-id="ca508-399">rowversion</span></span> |<span data-ttu-id="ca508-400">Byte]</span><span class="sxs-lookup"><span data-stu-id="ca508-400">Byte[]</span></span> |
| <span data-ttu-id="ca508-401">smalldatetime</span><span class="sxs-lookup"><span data-stu-id="ca508-401">smalldatetime</span></span> |<span data-ttu-id="ca508-402">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="ca508-402">DateTime</span></span> |
| <span data-ttu-id="ca508-403">smallint</span><span class="sxs-lookup"><span data-stu-id="ca508-403">smallint</span></span> |<span data-ttu-id="ca508-404">Int16</span><span class="sxs-lookup"><span data-stu-id="ca508-404">Int16</span></span> |
| <span data-ttu-id="ca508-405">kis pénz típusú értéknél</span><span class="sxs-lookup"><span data-stu-id="ca508-405">smallmoney</span></span> |<span data-ttu-id="ca508-406">Decimális</span><span class="sxs-lookup"><span data-stu-id="ca508-406">Decimal</span></span> |
| <span data-ttu-id="ca508-407">sql_variant</span><span class="sxs-lookup"><span data-stu-id="ca508-407">sql_variant</span></span> |<span data-ttu-id="ca508-408">Objektum *</span><span class="sxs-lookup"><span data-stu-id="ca508-408">Object *</span></span> |
| <span data-ttu-id="ca508-409">Szöveg</span><span class="sxs-lookup"><span data-stu-id="ca508-409">text</span></span> |<span data-ttu-id="ca508-410">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ca508-410">String, Char[]</span></span> |
| <span data-ttu-id="ca508-411">time</span><span class="sxs-lookup"><span data-stu-id="ca508-411">time</span></span> |<span data-ttu-id="ca508-412">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ca508-412">TimeSpan</span></span> |
| <span data-ttu-id="ca508-413">időbélyeg</span><span class="sxs-lookup"><span data-stu-id="ca508-413">timestamp</span></span> |<span data-ttu-id="ca508-414">Byte]</span><span class="sxs-lookup"><span data-stu-id="ca508-414">Byte[]</span></span> |
| <span data-ttu-id="ca508-415">tinyint</span><span class="sxs-lookup"><span data-stu-id="ca508-415">tinyint</span></span> |<span data-ttu-id="ca508-416">Bájt</span><span class="sxs-lookup"><span data-stu-id="ca508-416">Byte</span></span> |
| <span data-ttu-id="ca508-417">egyedi azonosító</span><span class="sxs-lookup"><span data-stu-id="ca508-417">uniqueidentifier</span></span> |<span data-ttu-id="ca508-418">GUID</span><span class="sxs-lookup"><span data-stu-id="ca508-418">Guid</span></span> |
| <span data-ttu-id="ca508-419">varbinary</span><span class="sxs-lookup"><span data-stu-id="ca508-419">varbinary</span></span> |<span data-ttu-id="ca508-420">Byte]</span><span class="sxs-lookup"><span data-stu-id="ca508-420">Byte[]</span></span> |
| <span data-ttu-id="ca508-421">varchar</span><span class="sxs-lookup"><span data-stu-id="ca508-421">varchar</span></span> |<span data-ttu-id="ca508-422">Karakterlánc, Char]</span><span class="sxs-lookup"><span data-stu-id="ca508-422">String, Char[]</span></span> |
| <span data-ttu-id="ca508-423">xml</span><span class="sxs-lookup"><span data-stu-id="ca508-423">xml</span></span> |<span data-ttu-id="ca508-424">XML</span><span class="sxs-lookup"><span data-stu-id="ca508-424">Xml</span></span> |

## <a name="mapping-source-to-sink-columns"></a><span data-ttu-id="ca508-425">Leképezési forráshoz oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="ca508-425">Mapping source to sink columns</span></span>
<span data-ttu-id="ca508-426">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="ca508-426">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-copy"></a><span data-ttu-id="ca508-427">Ismételhető másolása</span><span class="sxs-lookup"><span data-stu-id="ca508-427">Repeatable copy</span></span>
<span data-ttu-id="ca508-428">Amikor adat másolása az SQL Server-adatbázis, a másolási tevékenység hozzáfűzi adatokat a fogadó tábla alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="ca508-428">When copying data to SQL Server Database, the copy activity appends data to the sink table by default.</span></span> <span data-ttu-id="ca508-429">Ehelyett egy UPSERT végrehajtásához tekintse meg [Repeatable írni SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) cikk.</span><span class="sxs-lookup"><span data-stu-id="ca508-429">To perform an UPSERT instead,  See [Repeatable write to SqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) article.</span></span> 

<span data-ttu-id="ca508-430">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ca508-430">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="ca508-431">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="ca508-431">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="ca508-432">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="ca508-432">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="ca508-433">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="ca508-433">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="ca508-434">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="ca508-434">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="ca508-435">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="ca508-435">Performance and Tuning</span></span>
<span data-ttu-id="ca508-436">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="ca508-436">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
