---
title: "Másolja az adatokat, az Oracle Data Factory használatával |} Microsoft Docs"
description: "Ismerje meg, és a Oracle adatbázis, amely a helyszíni Azure Data Factory használatával adatok másolása."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 3c20aa95-a8a1-4aae-9180-a6a16d64a109
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: bb6af719fe6f1a30c5933ce4342a4c0c072f3ff4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a><span data-ttu-id="89506-103">Adatok másolása az Azure Data Factory használatával a helyszíni Oracle és a</span><span class="sxs-lookup"><span data-stu-id="89506-103">Copy data to/from on-premises Oracle using Azure Data Factory</span></span>
<span data-ttu-id="89506-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factory áthelyezni az adatokat és a helyszíni Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="89506-104">This article explains how to use the Copy Activity in Azure Data Factory to move data to/from an on-premises Oracle database.</span></span> <span data-ttu-id="89506-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="89506-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="89506-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="89506-106">Supported scenarios</span></span>
<span data-ttu-id="89506-107">Adatokat másolhat **az Oracle-adatbázishoz** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="89506-107">You can copy data **from an Oracle database** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="89506-108">Adatok másolása a következő adatokat tárolja **Oracle-adatbázishoz**:</span><span class="sxs-lookup"><span data-stu-id="89506-108">You can copy data from the following data stores **to an Oracle database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a><span data-ttu-id="89506-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="89506-109">Prerequisites</span></span>
<span data-ttu-id="89506-110">Adat-előállító helyszíni Oracle-adatforrások az adatkezelési átjáró használatával történő csatlakozást támogatja.</span><span class="sxs-lookup"><span data-stu-id="89506-110">Data Factory supports connecting to on-premises Oracle sources using the Data Management Gateway.</span></span> <span data-ttu-id="89506-111">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) cikkben tájékozódhat az adatkezelési átjáró és [tárolt adatok mozgatása felhőbe helyszíni](data-factory-move-data-between-onprem-and-cloud.md) cikk lépésenkénti állít be, az átjáró adatok folyamat adatok áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="89506-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article to learn about Data Management Gateway and [Move data from on-premises to cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up the gateway a data pipeline to move data.</span></span>

<span data-ttu-id="89506-112">Átjáróra szükség, akkor is, ha az Oracle egy Azure IaaS virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="89506-112">Gateway is required even if the Oracle is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="89506-113">Telepítheti az átjáró adattárként ugyanazon infrastruktúra-szolgáltatási virtuális gép vagy egy másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="89506-113">You can install the gateway on the same IaaS VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>

> [!NOTE]
> <span data-ttu-id="89506-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="89506-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="89506-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="89506-115">Supported versions and installation</span></span>
<span data-ttu-id="89506-116">Az Oracle-összekötő illesztőprogramok két verziója támogatja:</span><span class="sxs-lookup"><span data-stu-id="89506-116">This Oracle connector support two versions of drivers:</span></span>

- <span data-ttu-id="89506-117">**Microsoft-illesztőprogram (ajánlott) Oracle**: az adatkezelési átjáró verziója 2.7, a Microsoft illesztőprogram az Oracle automatikusan telepítve van az átjárón, így nem kell továbbá kezelni annak érdekében, hogy az illesztőprogram-től kezdődő Oracle kapcsolatot létrehozni, és akkor is jelentkezhet jobb másolási teljesítményt az illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="89506-117">**Microsoft driver for Oracle (recommended)**: starting from Data Management Gateway version 2.7, a Microsoft driver for Oracle is automatically installed along with the gateway, so you don't need to additionally handle the driver in order to establish connectivity to Oracle, and you can also experience better copy performance using this driver.</span></span> <span data-ttu-id="89506-118">Oracle verziói alatti adatbázisok támogatottak:</span><span class="sxs-lookup"><span data-stu-id="89506-118">Below versions of Oracle databases are supported:</span></span>
    - <span data-ttu-id="89506-119">Oracle 12c R1 (12.1)</span><span class="sxs-lookup"><span data-stu-id="89506-119">Oracle 12c R1 (12.1)</span></span>
    - <span data-ttu-id="89506-120">Oracle 11g R1 vagy R2 (11.1, 11.2)</span><span class="sxs-lookup"><span data-stu-id="89506-120">Oracle 11g R1, R2 (11.1, 11.2)</span></span>
    - <span data-ttu-id="89506-121">Oracle 10g R1 vagy R2 (10.1, 10,2)</span><span class="sxs-lookup"><span data-stu-id="89506-121">Oracle 10g R1, R2 (10.1, 10.2)</span></span>
    - <span data-ttu-id="89506-122">Oracle 9i R1 vagy R2 (9.0.1, 9.2)</span><span class="sxs-lookup"><span data-stu-id="89506-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span></span>
    - <span data-ttu-id="89506-123">Oracle 8i R3 (8.1.7-es)</span><span class="sxs-lookup"><span data-stu-id="89506-123">Oracle 8i R3 (8.1.7)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="89506-124">Jelenleg Microsoft Oracle-illesztőprogram csak az adatok másolását a Oracle, de nincs írás Oracle támogatja.</span><span class="sxs-lookup"><span data-stu-id="89506-124">Currently Microsoft driver for Oracle only supports copying data from Oracle but not writing to Oracle.</span></span> <span data-ttu-id="89506-125">És jegyezze meg a teszt kapcsolat funkció adatok felügyeleti átjáró Diagnosztika lap nem támogatja az illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="89506-125">And note the test connection capability in Data Management Gateway Diagnostics tab does not support this driver.</span></span> <span data-ttu-id="89506-126">A varázsló segítségével azt is megteheti, ellenőrizze a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="89506-126">Alternatively, you can use the copy wizard to validate the connectivity.</span></span>
>

- <span data-ttu-id="89506-127">**.NET-keretrendszerhez készült Oracle-adatszolgáltatóban:** másolja az adatokat, Oracle vagy Oracle-adatszolgáltatóban segítségével is beállíthatja.</span><span class="sxs-lookup"><span data-stu-id="89506-127">**Oracle Data Provider for .NET:** you can also choose to use Oracle Data Provider to copy data from/to Oracle.</span></span> <span data-ttu-id="89506-128">Ez az összetevő megtalálható [Oracle Data Access Windows összetevők](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span><span class="sxs-lookup"><span data-stu-id="89506-128">This component is included in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span></span> <span data-ttu-id="89506-129">Telepítse a megfelelő verzióját (32 vagy 64 bites) a számítógépen, amelyen az átjáró telepítve van.</span><span class="sxs-lookup"><span data-stu-id="89506-129">Install the appropriate version (32/64 bit) on the machine where the gateway is installed.</span></span> <span data-ttu-id="89506-130">[Oracle-adatszolgáltatóban .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) férhetnek hozzá Oracle Database 10 g, 2 vagy újabb kiadás.</span><span class="sxs-lookup"><span data-stu-id="89506-130">[Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) can access to Oracle Database 10g Release 2 or later.</span></span>

    <span data-ttu-id="89506-131">Ha úgy dönt, hogy a "XCopy telepítés", kövesse a readme.htm.</span><span class="sxs-lookup"><span data-stu-id="89506-131">If you choose “XCopy Installation”, follow steps in the readme.htm.</span></span> <span data-ttu-id="89506-132">Azt javasoljuk, hogy úgy dönt, hogy a telepítő felhasználói felületen (nem-XCopy egy).</span><span class="sxs-lookup"><span data-stu-id="89506-132">We recommend you choose the installer with UI (non-XCopy one).</span></span>

    <span data-ttu-id="89506-133">A szolgáltató telepítése után **indítsa újra a** az adatkezelési átjáró gazdaszolgáltatás a gépen a szolgáltatások kisalkalmazásával (vagy) az adatkezelési átjáró konfigurációkezelőjének használatával.</span><span class="sxs-lookup"><span data-stu-id="89506-133">After installing the provider, **restart** the Data Management Gateway host service on your machine using Services applet (or) Data Management Gateway Configuration Manager.</span></span>  

<span data-ttu-id="89506-134">Ahhoz, hogy a másolási folyamat másolása varázslót használja, ha az illesztőprogram-típus lesz automatikusan határozza meg.</span><span class="sxs-lookup"><span data-stu-id="89506-134">If you use copy wizard to author the copy pipeline, the driver type will be auto-determined.</span></span> <span data-ttu-id="89506-135">Microsoft illesztőprogram által használható alapértelmezett, kivéve, ha az átjáró verziója alacsonyabb, mint 2.7, vagy ha úgy dönt, Oracle, a fogadó.</span><span class="sxs-lookup"><span data-stu-id="89506-135">Microsoft driver will be used by default, unless your gateway version is lower than 2.7 or you choose Oracle as sink.</span></span>

## <a name="getting-started"></a><span data-ttu-id="89506-136">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="89506-136">Getting started</span></span>
<span data-ttu-id="89506-137">A másolási tevékenység, amely helyezi át az adatokat a helyszíni Oracle-adatbázishoz és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="89506-137">You can create a pipeline with a copy activity that moves data to/from an on-premises Oracle database by using different tools/APIs.</span></span>

<span data-ttu-id="89506-138">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="89506-138">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="89506-139">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="89506-139">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span>

<span data-ttu-id="89506-140">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="89506-140">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="89506-141">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="89506-141">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="89506-142">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="89506-142">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="89506-143">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="89506-143">Create a **data factory**.</span></span> <span data-ttu-id="89506-144">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="89506-144">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="89506-145">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="89506-145">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="89506-146">Adatok Oralce adatbázisból az Azure blob Storage másolása, akkor hozzon létre például az Oracle-adatbázishoz és az Azure storage-fiók összekapcsolása a data factory két társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="89506-146">For example, if you are copying data from an Oralce database to an Azure blob storage, you create two linked services to link your Oracle database and Azure storage account to your data factory.</span></span> <span data-ttu-id="89506-147">Oracle jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="89506-147">For linked service properties that are specific to Oracle, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="89506-148">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="89506-148">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="89506-149">A példa az előző lépésben említett Ha meg szeretné adni a tábla az Oracle-adatbázishoz a bemeneti adatokat tartalmazó adatkészlet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="89506-149">In the example mentioned in the last step, you create a dataset to specify the table in your Oracle database that contains the input data.</span></span> <span data-ttu-id="89506-150">Továbbá adja meg a blob-tároló és a mappa, amely tárolja az adatokat másolni az Oracle-adatbázisból egy másik dataset létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="89506-150">And, you create another dataset to specify the blob container and the folder that holds the data copied from the Oracle database.</span></span> <span data-ttu-id="89506-151">Oracle adott adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="89506-151">For dataset properties that are specific to Oracle, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="89506-152">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="89506-152">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="89506-153">A korábban említett példában OracleSource forrás-és BlobSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="89506-153">In the example mentioned earlier, you use OracleSource as a source and BlobSink as a sink for the copy activity.</span></span> <span data-ttu-id="89506-154">Ehhez hasonlóan az Azure Blob Storage Oracle-adatbázishoz való másolása, használható BlobSource és OracleSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="89506-154">Similarly, if you are copying from Azure Blob Storage to Oracle Database, you use BlobSource and OracleSink in the copy activity.</span></span> <span data-ttu-id="89506-155">Oracle-adatbázishoz adott tevékenység Tulajdonságok másolása, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="89506-155">For copy activity properties that are specific to Oracle database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="89506-156">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="89506-156">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span> 

<span data-ttu-id="89506-157">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="89506-157">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="89506-158">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="89506-158">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="89506-159">JSON-definíciók, amely segítségével másolja az adatokat a helyszíni Oracle-adatbázishoz az adat-előállító entitások minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-oracle-database) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="89506-159">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an on-premises Oracle database, see [JSON examples](#json-examples-for-copying-data-to-and-from-oracle-database) section of this article.</span></span>

<span data-ttu-id="89506-160">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory entitások JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="89506-160">The following sections provide details about JSON properties that are used to define Data Factory entities:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="89506-161">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="89506-161">Linked service properties</span></span>
<span data-ttu-id="89506-162">A következő táblázat a JSON-elemek szerepelnek Oracle kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="89506-162">The following table provides description for JSON elements specific to Oracle linked service.</span></span>

| <span data-ttu-id="89506-163">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="89506-163">Property</span></span> | <span data-ttu-id="89506-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="89506-164">Description</span></span> | <span data-ttu-id="89506-165">Szükséges</span><span class="sxs-lookup"><span data-stu-id="89506-165">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="89506-166">type</span><span class="sxs-lookup"><span data-stu-id="89506-166">type</span></span> |<span data-ttu-id="89506-167">A type tulajdonságot kell beállítani: **OnPremisesOracle**</span><span class="sxs-lookup"><span data-stu-id="89506-167">The type property must be set to: **OnPremisesOracle**</span></span> |<span data-ttu-id="89506-168">Igen</span><span class="sxs-lookup"><span data-stu-id="89506-168">Yes</span></span> |
| <span data-ttu-id="89506-169">driverType</span><span class="sxs-lookup"><span data-stu-id="89506-169">driverType</span></span> | <span data-ttu-id="89506-170">Adja meg, melyik illesztőprogram használatával másolja az adatokat, vagy Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="89506-170">Specify which driver to use to copy data from/to Oracle Database.</span></span> <span data-ttu-id="89506-171">Két érték engedélyezett **Microsoft** vagy **ODP** (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="89506-171">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="89506-172">Lásd: [verziójától és a telepítés támogatott](#supported-versions-and-installation) illesztőprogram adatai szakaszban.</span><span class="sxs-lookup"><span data-stu-id="89506-172">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="89506-173">Nem</span><span class="sxs-lookup"><span data-stu-id="89506-173">No</span></span> |
| <span data-ttu-id="89506-174">connectionString</span><span class="sxs-lookup"><span data-stu-id="89506-174">connectionString</span></span> | <span data-ttu-id="89506-175">Adja meg az Oracle adatbázispéldányt a connectionString tulajdonság való kapcsolódáshoz szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="89506-175">Specify information needed to connect to the Oracle Database instance for the connectionString property.</span></span> | <span data-ttu-id="89506-176">Igen</span><span class="sxs-lookup"><span data-stu-id="89506-176">Yes</span></span> |
| <span data-ttu-id="89506-177">gatewayName</span><span class="sxs-lookup"><span data-stu-id="89506-177">gatewayName</span></span> | <span data-ttu-id="89506-178">Azon átjáró neve, amely a helyszíni Oracle-kiszolgálóhoz való csatlakozáshoz használt</span><span class="sxs-lookup"><span data-stu-id="89506-178">Name of the gateway that that is used to connect to the on-premises Oracle server</span></span> |<span data-ttu-id="89506-179">Igen</span><span class="sxs-lookup"><span data-stu-id="89506-179">Yes</span></span> |

<span data-ttu-id="89506-180">**Példa: Microsoft-illesztőprogramot használ:**</span><span class="sxs-lookup"><span data-stu-id="89506-180">**Example: using Microsoft driver:**</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="89506-181">**Példa: ODP illesztőprogramot használja.**</span><span class="sxs-lookup"><span data-stu-id="89506-181">**Example: using ODP driver**</span></span>

<span data-ttu-id="89506-182">Tekintse meg [ezen a helyen](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) az engedélyezett formátumokat.</span><span class="sxs-lookup"><span data-stu-id="89506-182">Refer to [this site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) for the allowed formats.</span></span>

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="89506-183">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="89506-183">Dataset properties</span></span>
<span data-ttu-id="89506-184">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="89506-184">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="89506-185">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Oracle, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="89506-185">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Oracle, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="89506-186">A typeProperties szakasz más adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="89506-186">The typeProperties section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="89506-187">A typeProperties szakasz az adatkészlet OracleTable típusú tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="89506-187">The typeProperties section for the dataset of type OracleTable has the following properties:</span></span>

| <span data-ttu-id="89506-188">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="89506-188">Property</span></span> | <span data-ttu-id="89506-189">Leírás</span><span class="sxs-lookup"><span data-stu-id="89506-189">Description</span></span> | <span data-ttu-id="89506-190">Szükséges</span><span class="sxs-lookup"><span data-stu-id="89506-190">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="89506-191">tableName</span><span class="sxs-lookup"><span data-stu-id="89506-191">tableName</span></span> |<span data-ttu-id="89506-192">A tábla az Oracle-adatbázishoz, amely hivatkozik a társított szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="89506-192">Name of the table in the Oracle Database that the linked service refers to.</span></span> |<span data-ttu-id="89506-193">Nem (Ha **oracleReaderQuery** a **OracleSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="89506-193">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="89506-194">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="89506-194">Copy activity properties</span></span>
<span data-ttu-id="89506-195">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="89506-195">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="89506-196">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="89506-196">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="89506-197">A másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="89506-197">The Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="89506-198">Mivel a tevékenység typeProperties szakaszában elérhető tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="89506-198">Whereas, properties available in the typeProperties section of the activity vary with each activity type.</span></span> <span data-ttu-id="89506-199">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="89506-199">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

### <a name="oraclesource"></a><span data-ttu-id="89506-200">OracleSource</span><span class="sxs-lookup"><span data-stu-id="89506-200">OracleSource</span></span>
<span data-ttu-id="89506-201">A másolási tevékenység, ha az adatforrás típusú **OracleSource** a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="89506-201">In Copy activity, when the source is of type **OracleSource** the following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="89506-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="89506-202">Property</span></span> | <span data-ttu-id="89506-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="89506-203">Description</span></span> | <span data-ttu-id="89506-204">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="89506-204">Allowed values</span></span> | <span data-ttu-id="89506-205">Szükséges</span><span class="sxs-lookup"><span data-stu-id="89506-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="89506-206">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="89506-206">oracleReaderQuery</span></span> |<span data-ttu-id="89506-207">Az egyéni lekérdezés segítségével adatokat olvasni.</span><span class="sxs-lookup"><span data-stu-id="89506-207">Use the custom query to read data.</span></span> |<span data-ttu-id="89506-208">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="89506-208">SQL query string.</span></span> <span data-ttu-id="89506-209">Például: Válasszon * from tábla</span><span class="sxs-lookup"><span data-stu-id="89506-209">For example: select * from MyTable</span></span> <br/><br/><span data-ttu-id="89506-210">Ha nincs megadva, az SQL-utasítás végrehajtott: Válasszon * from tábla</span><span class="sxs-lookup"><span data-stu-id="89506-210">If not specified, the SQL statement that is executed: select * from MyTable</span></span> |<span data-ttu-id="89506-211">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="89506-211">No (if **tableName** of **dataset** is specified)</span></span> |

### <a name="oraclesink"></a><span data-ttu-id="89506-212">OracleSink</span><span class="sxs-lookup"><span data-stu-id="89506-212">OracleSink</span></span>
<span data-ttu-id="89506-213">**OracleSink** támogatja a következő tulajdonságokkal:</span><span class="sxs-lookup"><span data-stu-id="89506-213">**OracleSink** supports the following properties:</span></span>

| <span data-ttu-id="89506-214">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="89506-214">Property</span></span> | <span data-ttu-id="89506-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="89506-215">Description</span></span> | <span data-ttu-id="89506-216">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="89506-216">Allowed values</span></span> | <span data-ttu-id="89506-217">Szükséges</span><span class="sxs-lookup"><span data-stu-id="89506-217">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="89506-218">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="89506-218">writeBatchTimeout</span></span> |<span data-ttu-id="89506-219">Várakozási idő a kötegelt beszúrási művelet befejezését, mielőtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="89506-219">Wait time for the batch insert operation to complete before it times out.</span></span> |<span data-ttu-id="89506-220">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="89506-220">timespan</span></span><br/><br/> <span data-ttu-id="89506-221">. Példa: 00:30:00 (30 perc).</span><span class="sxs-lookup"><span data-stu-id="89506-221">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="89506-222">Nem</span><span class="sxs-lookup"><span data-stu-id="89506-222">No</span></span> |
| <span data-ttu-id="89506-223">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="89506-223">writeBatchSize</span></span> |<span data-ttu-id="89506-224">Szúr be az SQL-tábla adatokat, amikor a puffer mérete eléri writeBatchSize.</span><span class="sxs-lookup"><span data-stu-id="89506-224">Inserts data into the SQL table when the buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="89506-225">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="89506-225">Integer (number of rows)</span></span> |<span data-ttu-id="89506-226">Nem (alapértelmezett: 100)</span><span class="sxs-lookup"><span data-stu-id="89506-226">No (default: 100)</span></span> |
| <span data-ttu-id="89506-227">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="89506-227">sqlWriterCleanupScript</span></span> |<span data-ttu-id="89506-228">Adja meg egy lekérdezést a másolási tevékenység végrehajtása úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="89506-228">Specify a query for Copy Activity to execute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="89506-229">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="89506-229">A query statement.</span></span> |<span data-ttu-id="89506-230">Nem</span><span class="sxs-lookup"><span data-stu-id="89506-230">No</span></span> |
| <span data-ttu-id="89506-231">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="89506-231">sliceIdentifierColumnName</span></span> |<span data-ttu-id="89506-232">Adja meg a másolási tevékenység során automatikusan létrejön szelet azonosító, amely segítségével távolítja el az adatokat egy adott szelet, amikor futtassa újra a töltse ki az oszlopnevet.</span><span class="sxs-lookup"><span data-stu-id="89506-232">Specify column name for Copy Activity to fill with auto generated slice identifier, which is used to clean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="89506-233">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="89506-233">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="89506-234">Nem</span><span class="sxs-lookup"><span data-stu-id="89506-234">No</span></span> |

## <a name="json-examples-for-copying-data-to-and-from-oracle-database"></a><span data-ttu-id="89506-235">Adatok másolása, és az Oracle-adatbázisból JSON példák</span><span class="sxs-lookup"><span data-stu-id="89506-235">JSON examples for copying data to and from Oracle database</span></span>
<span data-ttu-id="89506-236">Az alábbi példa minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="89506-236">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="89506-237">Ezek szemléltetik adatok másolása az / / az Azure Blob Storage Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="89506-237">They show how to copy data from/to an Oracle database to/from Azure Blob Storage.</span></span> <span data-ttu-id="89506-238">Azonban adatok átmásolhatók a megadott mosdók bármelyikét [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="89506-238">However, data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>   

## <a name="example-copy-data-from-oracle-to-azure-blob"></a><span data-ttu-id="89506-239">Példa: Adatok másolása az Oracle az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="89506-239">Example: Copy data from Oracle to Azure Blob</span></span>

<span data-ttu-id="89506-240">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="89506-240">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="89506-241">A társított szolgáltatás típusa [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-241">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="89506-242">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-242">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="89506-243">Bemeneti [dataset](data-factory-create-datasets.md) típusú [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-243">An input [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="89506-244">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-244">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="89506-245">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) forrásként és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) mint fogadó.</span><span class="sxs-lookup"><span data-stu-id="89506-245">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) as source and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="89506-246">A minta másol adatokat egy helyszíni Oracle adatbázis egyik táblája blob óránként.</span><span class="sxs-lookup"><span data-stu-id="89506-246">The sample copies data from a table in an on-premises Oracle database to a blob hourly.</span></span> <span data-ttu-id="89506-247">További információ a különböző, a mintában használt tulajdonságok a mintákat a következő szakaszok dokumentációjában olvasható.</span><span class="sxs-lookup"><span data-stu-id="89506-247">For more information on various properties used in the sample, see documentation in sections following the samples.</span></span>

<span data-ttu-id="89506-248">**Oracle kapcsolódó szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="89506-248">**Oracle linked service:**</span></span>

```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "driverType": "Microsoft",
            "connectionString":"Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="89506-249">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="89506-249">**Azure Blob storage linked service:**</span></span>

```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

<span data-ttu-id="89506-250">**Oracle bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="89506-250">**Oracle input dataset:**</span></span>

<span data-ttu-id="89506-251">A példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Oracle és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="89506-251">The sample assumes you have created a table “MyTable” in Oracle and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="89506-252">"External" beállítása: "true" arról tájékoztatja a Data Factory szolgáltatásnak, hogy az adatkészlet külső data factoryval való és adat-előállító tevékenység nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="89506-252">Setting “external”: ”true” informs the Data Factory service that the dataset is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "OracleInput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "external": true,
        "availability": {
            "offset": "01:00:00",
            "interval": "1",
            "anchorDateTime": "2014-02-27T12:00:00",
            "frequency": "Hour"
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

<span data-ttu-id="89506-253">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="89506-253">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="89506-254">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="89506-254">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="89506-255">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="89506-255">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="89506-256">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="89506-256">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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

<span data-ttu-id="89506-257">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="89506-257">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="89506-258">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra van ütemezve.</span><span class="sxs-lookup"><span data-stu-id="89506-258">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run hourly.</span></span> <span data-ttu-id="89506-259">Az adatcsatorna JSON-definícióból a **forrás** típusúra **OracleSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="89506-259">In the pipeline JSON definition, the **source** type is set to **OracleSource** and **sink** type is set to **BlobSink**.</span></span>  <span data-ttu-id="89506-260">A megadott SQL-lekérdezés **oracleReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="89506-260">The SQL query specified with **oracleReaderQuery** property selects the data in the past hour to copy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "OracletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": " OracleInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "OracleSource",
                        "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
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

## <a name="example-copy-data-from-azure-blob-to-oracle"></a><span data-ttu-id="89506-261">Példa: Adatok másolása az Azure Blob az Oracle</span><span class="sxs-lookup"><span data-stu-id="89506-261">Example: Copy data from Azure Blob to Oracle</span></span>
<span data-ttu-id="89506-262">Ez a példa bemutatja az adatok másolása egy Azure Blob Storage-ból a helyszíni Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="89506-262">This sample shows how to copy data from an Azure Blob Storage to an on-premises Oracle database.</span></span> <span data-ttu-id="89506-263">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott forrás [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="89506-263">However, data can be copied **directly** from any of the sources stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="89506-264">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="89506-264">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="89506-265">A társított szolgáltatás típusa [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-265">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="89506-266">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-266">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="89506-267">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-267">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="89506-268">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="89506-268">An output [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="89506-269">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) forrásaként [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) mint fogadó.</span><span class="sxs-lookup"><span data-stu-id="89506-269">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) as source [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="89506-270">A minta másol adatokat egy blobot egy helyszíni Oracle adatbázis egyik táblája óránként.</span><span class="sxs-lookup"><span data-stu-id="89506-270">The sample copies data from a blob to a table in an on-premises Oracle database every hour.</span></span> <span data-ttu-id="89506-271">További információ a különböző, a mintában használt tulajdonságok a mintákat a következő szakaszok dokumentációjában olvasható.</span><span class="sxs-lookup"><span data-stu-id="89506-271">For more information on various properties used in the sample, see documentation in sections following the samples.</span></span>

<span data-ttu-id="89506-272">**Oracle kapcsolódó szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="89506-272">**Oracle linked service:**</span></span>
```json
{
    "name": "OnPremisesOracleLinkedService",
    "properties": {
        "type": "OnPremisesOracle",
        "typeProperties": {
            "connectionString": "Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<hostname>)(PORT=<port number>))(CONNECT_DATA=(SERVICE_NAME=<SID>)));
            User Id=<username>;Password=<password>;",
            "gatewayName": "<gateway name>"
        }
    }
}
```

<span data-ttu-id="89506-273">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="89506-273">**Azure Blob storage linked service:**</span></span>
```json
{
    "name": "StorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<Account key>"
        }
    }
}
```

<span data-ttu-id="89506-274">**Az Azure Blob bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="89506-274">**Azure Blob input dataset**</span></span>

<span data-ttu-id="89506-275">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="89506-275">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="89506-276">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="89506-276">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="89506-277">A mappa elérési útját használja év, hónap és nap részét kezdési idejét, valamint fájl nevét a kezdő időpontja óra részét.</span><span class="sxs-lookup"><span data-stu-id="89506-277">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="89506-278">"external": "true" beállítás arról értesíti az, hogy ezt a táblázatot az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák a Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="89506-278">“external”: “true” setting informs the Data Factory service that this table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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
            "frequency": "Day",
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

<span data-ttu-id="89506-279">**Oracle kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="89506-279">**Oracle output dataset:**</span></span>

<span data-ttu-id="89506-280">A példa feltételezi, hogy létrehozott egy "MyTable" táblát az Oracle.</span><span class="sxs-lookup"><span data-stu-id="89506-280">The sample assumes you have created a table “MyTable” in Oracle.</span></span> <span data-ttu-id="89506-281">A tábla létrehozása az azonos számú oszlopot az Oracle a Blob CSV-fájl tartalmazza a várt módon.</span><span class="sxs-lookup"><span data-stu-id="89506-281">Create the table in Oracle with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="89506-282">Új sorok hozzáadásakor a tábla minden órában.</span><span class="sxs-lookup"><span data-stu-id="89506-282">New rows are added to the table every hour.</span></span>

```json
{
    "name": "OracleOutput",
    "properties": {
        "type": "OracleTable",
        "linkedServiceName": "OnPremisesOracleLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Day",
            "interval": "1"
        }
    }
}
```

<span data-ttu-id="89506-283">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="89506-283">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="89506-284">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="89506-284">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="89506-285">Az adatcsatorna JSON-definícióból a **forrás** típusúra **BlobSource** és a **fogadó** típusúra **OracleSink**.</span><span class="sxs-lookup"><span data-stu-id="89506-285">In the pipeline JSON definition, the **source** type is set to **BlobSource** and the **sink** type is set to **OracleSink**.</span></span>  

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-05T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
            {
                "name": "AzureBlobtoOracle",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                    {
                        "name": "AzureBlobInput"
                    }
                ],
                "outputs": [
                    {
                        "name": "OracleOutput"
                    }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "OracleSink"
                    }
                },
                "scheduler": {
                    "frequency": "Day",
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


## <a name="troubleshooting-tips"></a><span data-ttu-id="89506-286">Hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="89506-286">Troubleshooting tips</span></span>
### <a name="problem-1-net-framework-data-provider"></a><span data-ttu-id="89506-287">1. hiba: A .NET-keretrendszer adatszolgáltatója</span><span class="sxs-lookup"><span data-stu-id="89506-287">Problem 1: .NET Framework Data Provider</span></span>

<span data-ttu-id="89506-288">Lásd a következő **hibaüzenet**:</span><span class="sxs-lookup"><span data-stu-id="89506-288">You see the following **error message**:</span></span>

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable to find the requested .Net Framework Data Provider. It may not be installed”.  

<span data-ttu-id="89506-289">**Lehetséges okok:**</span><span class="sxs-lookup"><span data-stu-id="89506-289">**Possible causes:**</span></span>

1. <span data-ttu-id="89506-290">A .NET Framework Data Provider – Oracle nem lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="89506-290">The .NET Framework Data Provider for Oracle was not installed.</span></span>
2. <span data-ttu-id="89506-291">A .NET Framework Data Provider – Oracle lett telepítve a .NET-keretrendszer 2.0, és nem található a .NET Framework 4.0 mappákban.</span><span class="sxs-lookup"><span data-stu-id="89506-291">The .NET Framework Data Provider for Oracle was installed to .NET Framework 2.0 and is not found in the .NET Framework 4.0 folders.</span></span>

<span data-ttu-id="89506-292">**Megoldás vagy megoldás:**</span><span class="sxs-lookup"><span data-stu-id="89506-292">**Resolution/Workaround:**</span></span>

1. <span data-ttu-id="89506-293">Ha még nem telepítette a .NET-szolgáltató az Oracle rendszerhez, [telepítse](http://www.oracle.com/technetwork/topics/dotnet/downloads/) , majd próbálja megismételni a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="89506-293">If you haven't installed the .NET Provider for Oracle, [install it](http://www.oracle.com/technetwork/topics/dotnet/downloads/) and retry the scenario.</span></span>
2. <span data-ttu-id="89506-294">Ha a hiba jelenik meg a szolgáltató telepítése után is, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="89506-294">If you get the error message even after installing the provider, do the following steps:</span></span>
   1. <span data-ttu-id="89506-295">Nyissa meg a .NET 2.0 gép config a mappából: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span><span class="sxs-lookup"><span data-stu-id="89506-295">Open machine config of .NET 2.0 from the folder: <system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span></span>
   2. <span data-ttu-id="89506-296">Keresse meg **.NET-keretrendszerhez készült Oracle-adatszolgáltatóban**, és meg kell található bejegyzés a következő mintában látható módon **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description=".NET oracle-adatszolgáltatója</span><span class="sxs-lookup"><span data-stu-id="89506-296">Search for **Oracle Data Provider for .NET**, and you should be able to find an entry as shown in the following sample under **system.data** -> **DbProviderFactories**: “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET</span></span>" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" /><span data-ttu-id="89506-297">”</span><span class="sxs-lookup"><span data-stu-id="89506-297">”</span></span>
3. <span data-ttu-id="89506-298">Ez a bejegyzés másolja a machine.config fájlban a következő v4.0: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, és módosítsa a 4.xxx.x.x verzióra.</span><span class="sxs-lookup"><span data-stu-id="89506-298">Copy this entry to the machine.config file in the following v4.0 folder: <system disk>:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, and change the version to 4.xxx.x.x.</span></span>
4. <span data-ttu-id="89506-299">A globális szerelvény-gyorsítótárban (GAC) "< ODP.NET telepített elérési útja > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" futtatásával telepítse `gacutil /i [provider path]`. ## hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="89506-299">Install “<ODP.NET Installed Path>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” into the global assembly cache (GAC) by running `gacutil /i [provider path]`.## Troubleshooting tips</span></span>

### <a name="problem-2-datetime-formatting"></a><span data-ttu-id="89506-300">2. hiba: dátum és idő formázása</span><span class="sxs-lookup"><span data-stu-id="89506-300">Problem 2: datetime formatting</span></span>

<span data-ttu-id="89506-301">Lásd a következő **hibaüzenet**:</span><span class="sxs-lookup"><span data-stu-id="89506-301">You see the following **error message**:</span></span>

    Message=Operation failed in Oracle Database with the following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

<span data-ttu-id="89506-302">**Megoldás vagy megoldás:**</span><span class="sxs-lookup"><span data-stu-id="89506-302">**Resolution/Workaround:**</span></span>

<span data-ttu-id="89506-303">Szükség lehet úgy, hogy a lekérdezési karakterláncok a másolási tevékenység alapján dátumok hogyan vannak konfigurálva az Oracle-adatbázis (a to_date függvény használatával) a következő mintában látható módon:</span><span class="sxs-lookup"><span data-stu-id="89506-303">You may need to adjust the query string in your copy activity based on how dates are configured in your Oracle database, as shown in the following sample (using the to_date function):</span></span>

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a><span data-ttu-id="89506-304">Oracle típusú leképezése</span><span class="sxs-lookup"><span data-stu-id="89506-304">Type mapping for Oracle</span></span>
<span data-ttu-id="89506-305">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az eseményforrás-típusnak gyűjtése módszert használja a következő 2. lépés típusok automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="89506-305">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types to sink types with the following 2-step approach:</span></span>

1. <span data-ttu-id="89506-306">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="89506-306">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="89506-307">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="89506-307">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="89506-308">Ha az adatok áthelyezése az Oracle, a következő megfeleltetéseket használtak Oracle-adattípusra .NET-típus, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="89506-308">When moving data from Oracle, the following mappings are used from Oracle data type to .NET type and vice versa.</span></span>

| <span data-ttu-id="89506-309">Oracle-adattípusra</span><span class="sxs-lookup"><span data-stu-id="89506-309">Oracle data type</span></span> | <span data-ttu-id="89506-310">.NET-keretrendszer adattípus</span><span class="sxs-lookup"><span data-stu-id="89506-310">.NET Framework data type</span></span> |
| --- | --- |
| <span data-ttu-id="89506-311">BFÁJL</span><span class="sxs-lookup"><span data-stu-id="89506-311">BFILE</span></span> |<span data-ttu-id="89506-312">Byte]</span><span class="sxs-lookup"><span data-stu-id="89506-312">Byte[]</span></span> |
| <span data-ttu-id="89506-313">A BLOB</span><span class="sxs-lookup"><span data-stu-id="89506-313">BLOB</span></span> |<span data-ttu-id="89506-314">Byte]</span><span class="sxs-lookup"><span data-stu-id="89506-314">Byte[]</span></span> |
| <span data-ttu-id="89506-315">KARAKTER</span><span class="sxs-lookup"><span data-stu-id="89506-315">CHAR</span></span> |<span data-ttu-id="89506-316">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-316">String</span></span> |
| <span data-ttu-id="89506-317">CLOB</span><span class="sxs-lookup"><span data-stu-id="89506-317">CLOB</span></span> |<span data-ttu-id="89506-318">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-318">String</span></span> |
| <span data-ttu-id="89506-319">DÁTUM</span><span class="sxs-lookup"><span data-stu-id="89506-319">DATE</span></span> |<span data-ttu-id="89506-320">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="89506-320">DateTime</span></span> |
| <span data-ttu-id="89506-321">LEBEGŐPONTOS</span><span class="sxs-lookup"><span data-stu-id="89506-321">FLOAT</span></span> |<span data-ttu-id="89506-322">Decimális, karakterlánc (Ha pontosság > 28)</span><span class="sxs-lookup"><span data-stu-id="89506-322">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="89506-323">EGÉSZ SZÁM</span><span class="sxs-lookup"><span data-stu-id="89506-323">INTEGER</span></span> |<span data-ttu-id="89506-324">Decimális, karakterlánc (Ha pontosság > 28)</span><span class="sxs-lookup"><span data-stu-id="89506-324">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="89506-325">IDŐKÖZ HÓNAP ÉV</span><span class="sxs-lookup"><span data-stu-id="89506-325">INTERVAL YEAR TO MONTH</span></span> |<span data-ttu-id="89506-326">Int32</span><span class="sxs-lookup"><span data-stu-id="89506-326">Int32</span></span> |
| <span data-ttu-id="89506-327">MÁSODIK INTERVALLUM NAPONTA</span><span class="sxs-lookup"><span data-stu-id="89506-327">INTERVAL DAY TO SECOND</span></span> |<span data-ttu-id="89506-328">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="89506-328">TimeSpan</span></span> |
| <span data-ttu-id="89506-329">HOSSZÚ</span><span class="sxs-lookup"><span data-stu-id="89506-329">LONG</span></span> |<span data-ttu-id="89506-330">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-330">String</span></span> |
| <span data-ttu-id="89506-331">HOSSZÚ NYERS</span><span class="sxs-lookup"><span data-stu-id="89506-331">LONG RAW</span></span> |<span data-ttu-id="89506-332">Byte]</span><span class="sxs-lookup"><span data-stu-id="89506-332">Byte[]</span></span> |
| <span data-ttu-id="89506-333">NCHAR</span><span class="sxs-lookup"><span data-stu-id="89506-333">NCHAR</span></span> |<span data-ttu-id="89506-334">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-334">String</span></span> |
| <span data-ttu-id="89506-335">NCLOB</span><span class="sxs-lookup"><span data-stu-id="89506-335">NCLOB</span></span> |<span data-ttu-id="89506-336">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-336">String</span></span> |
| <span data-ttu-id="89506-337">SZÁM</span><span class="sxs-lookup"><span data-stu-id="89506-337">NUMBER</span></span> |<span data-ttu-id="89506-338">Decimális, karakterlánc (Ha pontosság > 28)</span><span class="sxs-lookup"><span data-stu-id="89506-338">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="89506-339">NVARCHAR2</span><span class="sxs-lookup"><span data-stu-id="89506-339">NVARCHAR2</span></span> |<span data-ttu-id="89506-340">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-340">String</span></span> |
| <span data-ttu-id="89506-341">NYERS</span><span class="sxs-lookup"><span data-stu-id="89506-341">RAW</span></span> |<span data-ttu-id="89506-342">Byte]</span><span class="sxs-lookup"><span data-stu-id="89506-342">Byte[]</span></span> |
| <span data-ttu-id="89506-343">ROWID</span><span class="sxs-lookup"><span data-stu-id="89506-343">ROWID</span></span> |<span data-ttu-id="89506-344">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-344">String</span></span> |
| <span data-ttu-id="89506-345">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="89506-345">TIMESTAMP</span></span> |<span data-ttu-id="89506-346">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="89506-346">DateTime</span></span> |
| <span data-ttu-id="89506-347">A HELYI IDŐZÓNÁRA IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="89506-347">TIMESTAMP WITH LOCAL TIME ZONE</span></span> |<span data-ttu-id="89506-348">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="89506-348">DateTime</span></span> |
| <span data-ttu-id="89506-349">AZ IDŐZÓNA IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="89506-349">TIMESTAMP WITH TIME ZONE</span></span> |<span data-ttu-id="89506-350">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="89506-350">DateTime</span></span> |
| <span data-ttu-id="89506-351">ELŐJEL NÉLKÜLI EGÉSZKÉNT.</span><span class="sxs-lookup"><span data-stu-id="89506-351">UNSIGNED INTEGER</span></span> |<span data-ttu-id="89506-352">Szám</span><span class="sxs-lookup"><span data-stu-id="89506-352">Number</span></span> |
| <span data-ttu-id="89506-353">VARCHAR2</span><span class="sxs-lookup"><span data-stu-id="89506-353">VARCHAR2</span></span> |<span data-ttu-id="89506-354">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-354">String</span></span> |
| <span data-ttu-id="89506-355">XML</span><span class="sxs-lookup"><span data-stu-id="89506-355">XML</span></span> |<span data-ttu-id="89506-356">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="89506-356">String</span></span> |

> [!NOTE]
> <span data-ttu-id="89506-357">Adattípus **IDŐKÖZ év TO hónap** és **IDŐKÖZ nap TO második** Microsoft illesztőprogram használata esetén nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="89506-357">Data type **INTERVAL YEAR TO MONTH** and **INTERVAL DAY TO SECOND** are not supported when using Microsoft driver.</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="89506-358">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="89506-358">Map source to sink columns</span></span>
<span data-ttu-id="89506-359">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="89506-359">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="89506-360">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="89506-360">Repeatable read from relational sources</span></span>
<span data-ttu-id="89506-361">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="89506-361">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="89506-362">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="89506-362">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="89506-363">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="89506-363">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="89506-364">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="89506-364">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="89506-365">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="89506-365">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="89506-366">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="89506-366">Performance and Tuning</span></span>
<span data-ttu-id="89506-367">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="89506-367">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
