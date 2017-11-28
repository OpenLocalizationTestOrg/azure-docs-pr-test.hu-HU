---
title: "az Oracle Data Factory használatával aaaCopy adatok |} Microsoft Docs"
description: "Ismerje meg, hogyan toocopy való/Oracle-adatbázis adatait, amely a helyszíni Azure Data Factory használatával."
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
ms.openlocfilehash: adb6d5fbe38e18791616ac77e8179970bbea37fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tofrom-on-premises-oracle-using-azure-data-factory"></a><span data-ttu-id="40f26-103">Adatok másolása az Azure Data Factory használatával a helyszíni Oracle és a</span><span class="sxs-lookup"><span data-stu-id="40f26-103">Copy data to/from on-premises Oracle using Azure Data Factory</span></span>
<span data-ttu-id="40f26-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok helyszíni Oracle-adatbázishoz és a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="40f26-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data to/from an on-premises Oracle database.</span></span> <span data-ttu-id="40f26-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="40f26-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="40f26-106">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="40f26-106">Supported scenarios</span></span>
<span data-ttu-id="40f26-107">Adatokat másolhat **az Oracle-adatbázishoz** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="40f26-107">You can copy data **from an Oracle database** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="40f26-108">Adatok másolása a következő adatokat tárolja hello **tooan Oracle-adatbázishoz**:</span><span class="sxs-lookup"><span data-stu-id="40f26-108">You can copy data from hello following data stores **tooan Oracle database**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="prerequisites"></a><span data-ttu-id="40f26-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="40f26-109">Prerequisites</span></span>
<span data-ttu-id="40f26-110">Adat-előállító csatlakozó tooon helyszíni Oracle adatforrások az adatkezelési átjáró hello segítségével támogatja.</span><span class="sxs-lookup"><span data-stu-id="40f26-110">Data Factory supports connecting tooon-premises Oracle sources using hello Data Management Gateway.</span></span> <span data-ttu-id="40f26-111">Lásd: [az adatkezelési átjáró](data-factory-data-management-gateway.md) az adatkezelési átjáró kapcsolatos cikk toolearn és [tárolt adatok mozgatása a helyszíni toocloud](data-factory-move-data-between-onprem-and-cloud.md) cikk lépésenkénti adatok adatcsatorna hello átjáró beállítása toomove adatokat.</span><span class="sxs-lookup"><span data-stu-id="40f26-111">See [Data Management Gateway](data-factory-data-management-gateway.md) article toolearn about Data Management Gateway and [Move data from on-premises toocloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions on setting up hello gateway a data pipeline toomove data.</span></span>

<span data-ttu-id="40f26-112">Átjáróra szükség, akkor is, ha hello Oracle van tárolva az Azure infrastruktúra-szolgáltatási virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="40f26-112">Gateway is required even if hello Oracle is hosted in an Azure IaaS VM.</span></span> <span data-ttu-id="40f26-113">Ugyanaz, infrastruktúra-szolgáltatási virtuális gép hello adatként tárolja, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="40f26-113">You can install hello gateway on hello same IaaS VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>

> [!NOTE]
> <span data-ttu-id="40f26-114">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="40f26-114">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="40f26-115">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="40f26-115">Supported versions and installation</span></span>
<span data-ttu-id="40f26-116">Az Oracle-összekötő illesztőprogramok két verziója támogatja:</span><span class="sxs-lookup"><span data-stu-id="40f26-116">This Oracle connector support two versions of drivers:</span></span>

- <span data-ttu-id="40f26-117">**Microsoft-illesztőprogram (ajánlott) Oracle**: kiindulva az adatkezelési átjáró verziója 2.7, a Microsoft illesztőprogram hello átjáró együtt automatikusan telepíti a Oracle, így nem kell tooadditionally leíró hello illesztőprogram sorrendben tooestablish kapcsolat tooOracle, és is tapasztalhat, az illesztőprogram jobb másolási teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="40f26-117">**Microsoft driver for Oracle (recommended)**: starting from Data Management Gateway version 2.7, a Microsoft driver for Oracle is automatically installed along with hello gateway, so you don't need tooadditionally handle hello driver in order tooestablish connectivity tooOracle, and you can also experience better copy performance using this driver.</span></span> <span data-ttu-id="40f26-118">Oracle verziói alatti adatbázisok támogatottak:</span><span class="sxs-lookup"><span data-stu-id="40f26-118">Below versions of Oracle databases are supported:</span></span>
    - <span data-ttu-id="40f26-119">Oracle 12c R1 (12.1)</span><span class="sxs-lookup"><span data-stu-id="40f26-119">Oracle 12c R1 (12.1)</span></span>
    - <span data-ttu-id="40f26-120">Oracle 11g R1 vagy R2 (11.1, 11.2)</span><span class="sxs-lookup"><span data-stu-id="40f26-120">Oracle 11g R1, R2 (11.1, 11.2)</span></span>
    - <span data-ttu-id="40f26-121">Oracle 10g R1 vagy R2 (10.1, 10,2)</span><span class="sxs-lookup"><span data-stu-id="40f26-121">Oracle 10g R1, R2 (10.1, 10.2)</span></span>
    - <span data-ttu-id="40f26-122">Oracle 9i R1 vagy R2 (9.0.1, 9.2)</span><span class="sxs-lookup"><span data-stu-id="40f26-122">Oracle 9i R1, R2 (9.0.1, 9.2)</span></span>
    - <span data-ttu-id="40f26-123">Oracle 8i R3 (8.1.7-es)</span><span class="sxs-lookup"><span data-stu-id="40f26-123">Oracle 8i R3 (8.1.7)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40f26-124">Jelenleg Microsoft Oracle-illesztőprogram csak az adatok másolását a Oracle, de nincs írás tooOracle támogatja.</span><span class="sxs-lookup"><span data-stu-id="40f26-124">Currently Microsoft driver for Oracle only supports copying data from Oracle but not writing tooOracle.</span></span> <span data-ttu-id="40f26-125">És megjegyzés hello teszt kapcsolat funkció adatok felügyeleti átjáró Diagnosztika lap nem támogatja az illesztőprogramot.</span><span class="sxs-lookup"><span data-stu-id="40f26-125">And note hello test connection capability in Data Management Gateway Diagnostics tab does not support this driver.</span></span> <span data-ttu-id="40f26-126">Másik lehetőségként hello másolása varázsló toovalidate hello kapcsolatot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="40f26-126">Alternatively, you can use hello copy wizard toovalidate hello connectivity.</span></span>
>

- <span data-ttu-id="40f26-127">**.NET-keretrendszerhez készült Oracle-adatszolgáltatóban:** toouse Oracle-adatszolgáltatóban toocopy adatait is beállíthatja / tooOracle.</span><span class="sxs-lookup"><span data-stu-id="40f26-127">**Oracle Data Provider for .NET:** you can also choose toouse Oracle Data Provider toocopy data from/tooOracle.</span></span> <span data-ttu-id="40f26-128">Ez az összetevő megtalálható [Oracle Data Access Windows összetevők](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span><span class="sxs-lookup"><span data-stu-id="40f26-128">This component is included in [Oracle Data Access Components for Windows](http://www.oracle.com/technetwork/topics/dotnet/downloads/).</span></span> <span data-ttu-id="40f26-129">Telepítse a hello megfelelő verziója (32 vagy 64 bit) hello hello átjárót futtató gépen.</span><span class="sxs-lookup"><span data-stu-id="40f26-129">Install hello appropriate version (32/64 bit) on hello machine where hello gateway is installed.</span></span> <span data-ttu-id="40f26-130">[Oracle-adatszolgáltatóban .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) tooOracle Database 10 g, 2 vagy újabb kiadás hozzáférhet.</span><span class="sxs-lookup"><span data-stu-id="40f26-130">[Oracle Data Provider .NET 12.1](http://docs.oracle.com/database/121/ODPNT/InstallSystemRequirements.htm#ODPNT149) can access tooOracle Database 10g Release 2 or later.</span></span>

    <span data-ttu-id="40f26-131">Ha úgy dönt, hogy a "XCopy telepítés", hajtsa végre a hello readme.htm.</span><span class="sxs-lookup"><span data-stu-id="40f26-131">If you choose “XCopy Installation”, follow steps in hello readme.htm.</span></span> <span data-ttu-id="40f26-132">Azt javasoljuk, hogy úgy dönt, hogy hello telepítőjét a felhasználói felület (nem-XCopy egy).</span><span class="sxs-lookup"><span data-stu-id="40f26-132">We recommend you choose hello installer with UI (non-XCopy one).</span></span>

    <span data-ttu-id="40f26-133">Hello szolgáltató telepítése után **indítsa újra a** hello az adatkezelési átjáró gazdaszolgáltatás a gépen a szolgáltatások kisalkalmazásával (vagy) az adatkezelési átjáró konfigurációkezelőjének használatával.</span><span class="sxs-lookup"><span data-stu-id="40f26-133">After installing hello provider, **restart** hello Data Management Gateway host service on your machine using Services applet (or) Data Management Gateway Configuration Manager.</span></span>  

<span data-ttu-id="40f26-134">Ha másolása varázsló tooauthor hello másolási folyamat használja, a hello illesztőprogram típus lesz automatikus határozza meg.</span><span class="sxs-lookup"><span data-stu-id="40f26-134">If you use copy wizard tooauthor hello copy pipeline, hello driver type will be auto-determined.</span></span> <span data-ttu-id="40f26-135">Microsoft illesztőprogram által használható alapértelmezett, kivéve, ha az átjáró verziója alacsonyabb, mint 2.7, vagy ha úgy dönt, Oracle, a fogadó.</span><span class="sxs-lookup"><span data-stu-id="40f26-135">Microsoft driver will be used by default, unless your gateway version is lower than 2.7 or you choose Oracle as sink.</span></span>

## <a name="getting-started"></a><span data-ttu-id="40f26-136">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="40f26-136">Getting started</span></span>
<span data-ttu-id="40f26-137">A másolási tevékenység, amely helyezi át az adatokat a helyszíni Oracle-adatbázishoz és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="40f26-137">You can create a pipeline with a copy activity that moves data to/from an on-premises Oracle database by using different tools/APIs.</span></span>

<span data-ttu-id="40f26-138">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="40f26-138">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="40f26-139">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="40f26-139">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span>

<span data-ttu-id="40f26-140">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="40f26-140">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="40f26-141">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="40f26-141">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="40f26-142">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="40f26-142">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="40f26-143">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="40f26-143">Create a **data factory**.</span></span> <span data-ttu-id="40f26-144">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="40f26-144">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="40f26-145">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="40f26-145">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="40f26-146">Adatok másolása az egy Oralce adatbázis tooan Azure blob Storage tárolóban, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Oracle-adatbázishoz és az Azure storage tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="40f26-146">For example, if you are copying data from an Oralce database tooan Azure blob storage, you create two linked services toolink your Oracle database and Azure storage account tooyour data factory.</span></span> <span data-ttu-id="40f26-147">Csatolt szolgáltatás tulajdonságait, amelyek adott tooOracle, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="40f26-147">For linked service properties that are specific tooOracle, see [linked service properties](#linked-service-properties) section.</span></span>
3. <span data-ttu-id="40f26-148">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="40f26-148">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="40f26-149">Hello utolsó lépésében említett hello példában az Oracle-adatbázis hello bemeneti adatokat tartalmazó hoz létre egy adatkészlet toospecify hello tábla.</span><span class="sxs-lookup"><span data-stu-id="40f26-149">In hello example mentioned in hello last step, you create a dataset toospecify hello table in your Oracle database that contains hello input data.</span></span> <span data-ttu-id="40f26-150">Egy másik dataset toospecify hello blob tároló létrehozása és a hello adatokat tartalmazó hello mappába másolta át hello Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="40f26-150">And, you create another dataset toospecify hello blob container and hello folder that holds hello data copied from hello Oracle database.</span></span> <span data-ttu-id="40f26-151">Adatkészlet tulajdonságai, amelyek adott tooOracle, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="40f26-151">For dataset properties that are specific tooOracle, see [dataset properties](#dataset-properties) section.</span></span>
4. <span data-ttu-id="40f26-152">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="40f26-152">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="40f26-153">A korábban említett hello példában OracleSource forrás-és BlobSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="40f26-153">In hello example mentioned earlier, you use OracleSource as a source and BlobSink as a sink for hello copy activity.</span></span> <span data-ttu-id="40f26-154">Ehhez hasonlóan az Azure Blob Storage tooOracle adatbázis másolása, használható BlobSource és OracleSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="40f26-154">Similarly, if you are copying from Azure Blob Storage tooOracle Database, you use BlobSource and OracleSink in hello copy activity.</span></span> <span data-ttu-id="40f26-155">Másolási tevékenység tulajdonságok adott tooOracle adatbázis esetében, tekintse meg a [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="40f26-155">For copy activity properties that are specific tooOracle database, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="40f26-156">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="40f26-156">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span> 

<span data-ttu-id="40f26-157">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="40f26-157">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="40f26-158">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="40f26-158">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="40f26-159">Az adat-előállító entitások, amelyek az egy helyszíni Oracle-adatbázishoz használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-oracle-database) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="40f26-159">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an on-premises Oracle database, see [JSON examples](#json-examples-for-copying-data-to-and-from-oracle-database) section of this article.</span></span>

<span data-ttu-id="40f26-160">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="40f26-160">hello following sections provide details about JSON properties that are used toodefine Data Factory entities:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="40f26-161">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="40f26-161">Linked service properties</span></span>
<span data-ttu-id="40f26-162">a következő táblázat hello biztosít JSON elemek adott tooOracle kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="40f26-162">hello following table provides description for JSON elements specific tooOracle linked service.</span></span>

| <span data-ttu-id="40f26-163">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="40f26-163">Property</span></span> | <span data-ttu-id="40f26-164">Leírás</span><span class="sxs-lookup"><span data-stu-id="40f26-164">Description</span></span> | <span data-ttu-id="40f26-165">Szükséges</span><span class="sxs-lookup"><span data-stu-id="40f26-165">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="40f26-166">type</span><span class="sxs-lookup"><span data-stu-id="40f26-166">type</span></span> |<span data-ttu-id="40f26-167">hello type tulajdonságot kell beállítani: **OnPremisesOracle**</span><span class="sxs-lookup"><span data-stu-id="40f26-167">hello type property must be set to: **OnPremisesOracle**</span></span> |<span data-ttu-id="40f26-168">Igen</span><span class="sxs-lookup"><span data-stu-id="40f26-168">Yes</span></span> |
| <span data-ttu-id="40f26-169">driverType</span><span class="sxs-lookup"><span data-stu-id="40f26-169">driverType</span></span> | <span data-ttu-id="40f26-170">Adja meg, mely illesztőprogram toouse toocopy adatait / tooOracle adatbázis.</span><span class="sxs-lookup"><span data-stu-id="40f26-170">Specify which driver toouse toocopy data from/tooOracle Database.</span></span> <span data-ttu-id="40f26-171">Két érték engedélyezett **Microsoft** vagy **ODP** (alapértelmezett).</span><span class="sxs-lookup"><span data-stu-id="40f26-171">Allowed values are **Microsoft** or **ODP** (default).</span></span> <span data-ttu-id="40f26-172">Lásd: [verziójától és a telepítés támogatott](#supported-versions-and-installation) illesztőprogram adatai szakaszban.</span><span class="sxs-lookup"><span data-stu-id="40f26-172">See [Supported version and installation](#supported-versions-and-installation) section on driver details.</span></span> | <span data-ttu-id="40f26-173">Nem</span><span class="sxs-lookup"><span data-stu-id="40f26-173">No</span></span> |
| <span data-ttu-id="40f26-174">connectionString</span><span class="sxs-lookup"><span data-stu-id="40f26-174">connectionString</span></span> | <span data-ttu-id="40f26-175">Adjon meg információt tooconnect toohello Oracle adatbázispéldány hello connectionString tulajdonság szükséges.</span><span class="sxs-lookup"><span data-stu-id="40f26-175">Specify information needed tooconnect toohello Oracle Database instance for hello connectionString property.</span></span> | <span data-ttu-id="40f26-176">Igen</span><span class="sxs-lookup"><span data-stu-id="40f26-176">Yes</span></span> |
| <span data-ttu-id="40f26-177">gatewayName</span><span class="sxs-lookup"><span data-stu-id="40f26-177">gatewayName</span></span> | <span data-ttu-id="40f26-178">Hello átjáró, amely használt tooconnect toohello helyszíni Oracle-kiszolgáló neve</span><span class="sxs-lookup"><span data-stu-id="40f26-178">Name of hello gateway that that is used tooconnect toohello on-premises Oracle server</span></span> |<span data-ttu-id="40f26-179">Igen</span><span class="sxs-lookup"><span data-stu-id="40f26-179">Yes</span></span> |

<span data-ttu-id="40f26-180">**Példa: Microsoft-illesztőprogramot használ:**</span><span class="sxs-lookup"><span data-stu-id="40f26-180">**Example: using Microsoft driver:**</span></span>
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

<span data-ttu-id="40f26-181">**Példa: ODP illesztőprogramot használja.**</span><span class="sxs-lookup"><span data-stu-id="40f26-181">**Example: using ODP driver**</span></span>

<span data-ttu-id="40f26-182">Tekintse meg a túl[ezen a helyen](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) az engedélyezett formátumokat hello.</span><span class="sxs-lookup"><span data-stu-id="40f26-182">Refer too[this site](https://www.connectionstrings.com/oracle-data-provider-for-net-odp-net/) for hello allowed formats.</span></span>

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

## <a name="dataset-properties"></a><span data-ttu-id="40f26-183">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="40f26-183">Dataset properties</span></span>
<span data-ttu-id="40f26-184">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="40f26-184">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="40f26-185">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Oracle, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="40f26-185">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Oracle, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="40f26-186">hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="40f26-186">hello typeProperties section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="40f26-187">hello typeProperties szakasz hello adatkészlet OracleTable típus következő tulajdonságai hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="40f26-187">hello typeProperties section for hello dataset of type OracleTable has hello following properties:</span></span>

| <span data-ttu-id="40f26-188">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="40f26-188">Property</span></span> | <span data-ttu-id="40f26-189">Leírás</span><span class="sxs-lookup"><span data-stu-id="40f26-189">Description</span></span> | <span data-ttu-id="40f26-190">Szükséges</span><span class="sxs-lookup"><span data-stu-id="40f26-190">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="40f26-191">tableName</span><span class="sxs-lookup"><span data-stu-id="40f26-191">tableName</span></span> |<span data-ttu-id="40f26-192">Oracle-adatbázishoz kapcsolódó szolgáltatás hello hello hello tábla neve hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="40f26-192">Name of hello table in hello Oracle Database that hello linked service refers to.</span></span> |<span data-ttu-id="40f26-193">Nem (Ha **oracleReaderQuery** a **OracleSource** van megadva)</span><span class="sxs-lookup"><span data-stu-id="40f26-193">No (if **oracleReaderQuery** of **OracleSource** is specified)</span></span> |

## <a name="copy-activity-properties"></a><span data-ttu-id="40f26-194">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="40f26-194">Copy activity properties</span></span>
<span data-ttu-id="40f26-195">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="40f26-195">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="40f26-196">Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="40f26-196">Properties such as name, description, input and output tables, and policy are available for all types of activities.</span></span>

> [!NOTE]
> <span data-ttu-id="40f26-197">hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.</span><span class="sxs-lookup"><span data-stu-id="40f26-197">hello Copy Activity takes only one input and produces only one output.</span></span>

<span data-ttu-id="40f26-198">Mivel a hello hello tevékenység részében typeProperties rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="40f26-198">Whereas, properties available in hello typeProperties section of hello activity vary with each activity type.</span></span> <span data-ttu-id="40f26-199">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="40f26-199">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

### <a name="oraclesource"></a><span data-ttu-id="40f26-200">OracleSource</span><span class="sxs-lookup"><span data-stu-id="40f26-200">OracleSource</span></span>
<span data-ttu-id="40f26-201">A másolási tevékenység, ha hello adatforrás típusú **OracleSource** hello a következő tulajdonságok érhetők el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="40f26-201">In Copy activity, when hello source is of type **OracleSource** hello following properties are available in **typeProperties** section:</span></span>

| <span data-ttu-id="40f26-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="40f26-202">Property</span></span> | <span data-ttu-id="40f26-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="40f26-203">Description</span></span> | <span data-ttu-id="40f26-204">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="40f26-204">Allowed values</span></span> | <span data-ttu-id="40f26-205">Szükséges</span><span class="sxs-lookup"><span data-stu-id="40f26-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="40f26-206">oracleReaderQuery</span><span class="sxs-lookup"><span data-stu-id="40f26-206">oracleReaderQuery</span></span> |<span data-ttu-id="40f26-207">Hello egyéni lekérdezés tooread adatok felhasználásával.</span><span class="sxs-lookup"><span data-stu-id="40f26-207">Use hello custom query tooread data.</span></span> |<span data-ttu-id="40f26-208">SQL-lekérdezési karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="40f26-208">SQL query string.</span></span> <span data-ttu-id="40f26-209">Például: Válasszon * from tábla</span><span class="sxs-lookup"><span data-stu-id="40f26-209">For example: select * from MyTable</span></span> <br/><br/><span data-ttu-id="40f26-210">Ha nincs megadva, az SQL-utasítás végrehajtása hello: Válasszon * from tábla</span><span class="sxs-lookup"><span data-stu-id="40f26-210">If not specified, hello SQL statement that is executed: select * from MyTable</span></span> |<span data-ttu-id="40f26-211">Nem (Ha **tableName** a **dataset** van megadva)</span><span class="sxs-lookup"><span data-stu-id="40f26-211">No (if **tableName** of **dataset** is specified)</span></span> |

### <a name="oraclesink"></a><span data-ttu-id="40f26-212">OracleSink</span><span class="sxs-lookup"><span data-stu-id="40f26-212">OracleSink</span></span>
<span data-ttu-id="40f26-213">**OracleSink** következő tulajdonságai hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="40f26-213">**OracleSink** supports hello following properties:</span></span>

| <span data-ttu-id="40f26-214">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="40f26-214">Property</span></span> | <span data-ttu-id="40f26-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="40f26-215">Description</span></span> | <span data-ttu-id="40f26-216">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="40f26-216">Allowed values</span></span> | <span data-ttu-id="40f26-217">Szükséges</span><span class="sxs-lookup"><span data-stu-id="40f26-217">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="40f26-218">writeBatchTimeout</span><span class="sxs-lookup"><span data-stu-id="40f26-218">writeBatchTimeout</span></span> |<span data-ttu-id="40f26-219">Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot.</span><span class="sxs-lookup"><span data-stu-id="40f26-219">Wait time for hello batch insert operation toocomplete before it times out.</span></span> |<span data-ttu-id="40f26-220">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="40f26-220">timespan</span></span><br/><br/> <span data-ttu-id="40f26-221">. Példa: 00:30:00 (30 perc).</span><span class="sxs-lookup"><span data-stu-id="40f26-221">Example: 00:30:00 (30 minutes).</span></span> |<span data-ttu-id="40f26-222">Nem</span><span class="sxs-lookup"><span data-stu-id="40f26-222">No</span></span> |
| <span data-ttu-id="40f26-223">WriteBatchSize</span><span class="sxs-lookup"><span data-stu-id="40f26-223">writeBatchSize</span></span> |<span data-ttu-id="40f26-224">Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat.</span><span class="sxs-lookup"><span data-stu-id="40f26-224">Inserts data into hello SQL table when hello buffer size reaches writeBatchSize.</span></span> |<span data-ttu-id="40f26-225">Egész szám (sorok száma)</span><span class="sxs-lookup"><span data-stu-id="40f26-225">Integer (number of rows)</span></span> |<span data-ttu-id="40f26-226">Nem (alapértelmezett: 100)</span><span class="sxs-lookup"><span data-stu-id="40f26-226">No (default: 100)</span></span> |
| <span data-ttu-id="40f26-227">sqlWriterCleanupScript</span><span class="sxs-lookup"><span data-stu-id="40f26-227">sqlWriterCleanupScript</span></span> |<span data-ttu-id="40f26-228">Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait.</span><span class="sxs-lookup"><span data-stu-id="40f26-228">Specify a query for Copy Activity tooexecute such that data of a specific slice is cleaned up.</span></span> |<span data-ttu-id="40f26-229">A lekérdezési utasítást.</span><span class="sxs-lookup"><span data-stu-id="40f26-229">A query statement.</span></span> |<span data-ttu-id="40f26-230">Nem</span><span class="sxs-lookup"><span data-stu-id="40f26-230">No</span></span> |
| <span data-ttu-id="40f26-231">sliceIdentifierColumnName</span><span class="sxs-lookup"><span data-stu-id="40f26-231">sliceIdentifierColumnName</span></span> |<span data-ttu-id="40f26-232">Adja meg, a másolási tevékenység toofill oszlopnév automatikusan létrejön szelet azonosítóval, amely adatokat egy adott szelet, amikor futtassa újra a használt tooclean.</span><span class="sxs-lookup"><span data-stu-id="40f26-232">Specify column name for Copy Activity toofill with auto generated slice identifier, which is used tooclean up data of a specific slice when rerun.</span></span> |<span data-ttu-id="40f26-233">Egy oszlop binary(32) adattípusú oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="40f26-233">Column name of a column with data type of binary(32).</span></span> |<span data-ttu-id="40f26-234">Nem</span><span class="sxs-lookup"><span data-stu-id="40f26-234">No</span></span> |

## <a name="json-examples-for-copying-data-tooand-from-oracle-database"></a><span data-ttu-id="40f26-235">JSON Példák adatok tooand másolását Oracle-adatbázishoz</span><span class="sxs-lookup"><span data-stu-id="40f26-235">JSON examples for copying data tooand from Oracle database</span></span>
<span data-ttu-id="40f26-236">hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="40f26-236">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="40f26-237">Azok hogyan toocopy adatait / tooan Oracle adatbázis, az Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="40f26-237">They show how toocopy data from/tooan Oracle database to/from Azure Blob Storage.</span></span> <span data-ttu-id="40f26-238">Adatok azonban nem közölt hello nyelő másolt tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="40f26-238">However, data can be copied tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>   

## <a name="example-copy-data-from-oracle-tooazure-blob"></a><span data-ttu-id="40f26-239">Példa: Adatok másolása az Oracle tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="40f26-239">Example: Copy data from Oracle tooAzure Blob</span></span>

<span data-ttu-id="40f26-240">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="40f26-240">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="40f26-241">A társított szolgáltatás típusa [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-241">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="40f26-242">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-242">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="40f26-243">Bemeneti [dataset](data-factory-create-datasets.md) típusú [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-243">An input [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="40f26-244">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-244">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="40f26-245">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) forrásként és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) mint fogadó.</span><span class="sxs-lookup"><span data-stu-id="40f26-245">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [OracleSource](data-factory-onprem-oracle-connector.md#copy-activity-properties) as source and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="40f26-246">hello minta másol adatokat egy helyszíni Oracle adatbázis tooa blob táblához óránként.</span><span class="sxs-lookup"><span data-stu-id="40f26-246">hello sample copies data from a table in an on-premises Oracle database tooa blob hourly.</span></span> <span data-ttu-id="40f26-247">További információ a különböző hello minta használt tulajdonságok hello mintát a következő szakaszok dokumentációjában olvasható.</span><span class="sxs-lookup"><span data-stu-id="40f26-247">For more information on various properties used in hello sample, see documentation in sections following hello samples.</span></span>

<span data-ttu-id="40f26-248">**Oracle kapcsolódó szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="40f26-248">**Oracle linked service:**</span></span>

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

<span data-ttu-id="40f26-249">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="40f26-249">**Azure Blob storage linked service:**</span></span>

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

<span data-ttu-id="40f26-250">**Oracle bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="40f26-250">**Oracle input dataset:**</span></span>

<span data-ttu-id="40f26-251">hello példa azt feltételezi, hogy létrehozott egy tábla "MyTable" Oracle és egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="40f26-251">hello sample assumes you have created a table “MyTable” in Oracle and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="40f26-252">"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.</span><span class="sxs-lookup"><span data-stu-id="40f26-252">Setting “external”: ”true” informs hello Data Factory service that hello dataset is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="40f26-253">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="40f26-253">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="40f26-254">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="40f26-254">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="40f26-255">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="40f26-255">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="40f26-256">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="40f26-256">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="40f26-257">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="40f26-257">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="40f26-258">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="40f26-258">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun hourly.</span></span> <span data-ttu-id="40f26-259">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**OracleSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="40f26-259">In hello pipeline JSON definition, hello **source** type is set too**OracleSource** and **sink** type is set too**BlobSink**.</span></span>  <span data-ttu-id="40f26-260">hello SQL-lekérdezésben megadott **oracleReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="40f26-260">hello SQL query specified with **oracleReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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

## <a name="example-copy-data-from-azure-blob-toooracle"></a><span data-ttu-id="40f26-261">Példa: Adatok másolása az Azure Blob tooOracle</span><span class="sxs-lookup"><span data-stu-id="40f26-261">Example: Copy data from Azure Blob tooOracle</span></span>
<span data-ttu-id="40f26-262">Ez a példa bemutatja, hogyan toocopy adatait az Azure Blob Storage tooan helyszíni Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="40f26-262">This sample shows how toocopy data from an Azure Blob Storage tooan on-premises Oracle database.</span></span> <span data-ttu-id="40f26-263">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello források [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="40f26-263">However, data can be copied **directly** from any of hello sources stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

<span data-ttu-id="40f26-264">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="40f26-264">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="40f26-265">A társított szolgáltatás típusa [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-265">A linked service of type [OnPremisesOracle](data-factory-onprem-oracle-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="40f26-266">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-266">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="40f26-267">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-267">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="40f26-268">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="40f26-268">An output [dataset](data-factory-create-datasets.md) of type [OracleTable](data-factory-onprem-oracle-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="40f26-269">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) forrásaként [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) mint fogadó.</span><span class="sxs-lookup"><span data-stu-id="40f26-269">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) as source [OracleSink](data-factory-onprem-oracle-connector.md#copy-activity-properties) as sink.</span></span>

<span data-ttu-id="40f26-270">hello minta másol adatokat a helyszíni Oracle-adatbázisban egy blob tooa tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="40f26-270">hello sample copies data from a blob tooa table in an on-premises Oracle database every hour.</span></span> <span data-ttu-id="40f26-271">További információ a különböző hello minta használt tulajdonságok hello mintát a következő szakaszok dokumentációjában olvasható.</span><span class="sxs-lookup"><span data-stu-id="40f26-271">For more information on various properties used in hello sample, see documentation in sections following hello samples.</span></span>

<span data-ttu-id="40f26-272">**Oracle kapcsolódó szolgáltatás:**</span><span class="sxs-lookup"><span data-stu-id="40f26-272">**Oracle linked service:**</span></span>
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

<span data-ttu-id="40f26-273">**Az Azure Blob storage társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="40f26-273">**Azure Blob storage linked service:**</span></span>
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

<span data-ttu-id="40f26-274">**Az Azure Blob bemeneti adatkészlet**</span><span class="sxs-lookup"><span data-stu-id="40f26-274">**Azure Blob input dataset**</span></span>

<span data-ttu-id="40f26-275">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="40f26-275">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="40f26-276">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="40f26-276">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="40f26-277">hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello.</span><span class="sxs-lookup"><span data-stu-id="40f26-277">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="40f26-278">"external": "true" beállítás arról értesíti az, hogy ez a táblázat külső toohello adat-előállító és hello adat-előállítóban tevékenység nem hozzák hello Data Factory szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="40f26-278">“external”: “true” setting informs hello Data Factory service that this table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="40f26-279">**Oracle kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="40f26-279">**Oracle output dataset:**</span></span>

<span data-ttu-id="40f26-280">hello példa feltételezi, hogy létrehozott egy "MyTable" táblát az Oracle.</span><span class="sxs-lookup"><span data-stu-id="40f26-280">hello sample assumes you have created a table “MyTable” in Oracle.</span></span> <span data-ttu-id="40f26-281">Hello tábla létrehozása az Oracle azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt.</span><span class="sxs-lookup"><span data-stu-id="40f26-281">Create hello table in Oracle with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="40f26-282">Új sorok hozzáadásakor toohello tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="40f26-282">New rows are added toohello table every hour.</span></span>

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

<span data-ttu-id="40f26-283">**A másolási tevékenység során a következő feldolgozási sorban:**</span><span class="sxs-lookup"><span data-stu-id="40f26-283">**Pipeline with Copy activity:**</span></span>

<span data-ttu-id="40f26-284">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="40f26-284">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="40f26-285">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és hello **fogadó** típusuk értéke túl**OracleSink**.</span><span class="sxs-lookup"><span data-stu-id="40f26-285">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and hello **sink** type is set too**OracleSink**.</span></span>  

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


## <a name="troubleshooting-tips"></a><span data-ttu-id="40f26-286">Hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="40f26-286">Troubleshooting tips</span></span>
### <a name="problem-1-net-framework-data-provider"></a><span data-ttu-id="40f26-287">1. hiba: A .NET-keretrendszer adatszolgáltatója</span><span class="sxs-lookup"><span data-stu-id="40f26-287">Problem 1: .NET Framework Data Provider</span></span>

<span data-ttu-id="40f26-288">Lásd a következő hello **hibaüzenet**:</span><span class="sxs-lookup"><span data-stu-id="40f26-288">You see hello following **error message**:</span></span>

    Copy activity met invalid parameters: 'UnknownParameterName', Detailed message: Unable toofind hello requested .Net Framework Data Provider. It may not be installed”.  

<span data-ttu-id="40f26-289">**Lehetséges okok:**</span><span class="sxs-lookup"><span data-stu-id="40f26-289">**Possible causes:**</span></span>

1. <span data-ttu-id="40f26-290">.NET Framework Data Provider – Oracle hello nem lett telepítve.</span><span class="sxs-lookup"><span data-stu-id="40f26-290">hello .NET Framework Data Provider for Oracle was not installed.</span></span>
2. <span data-ttu-id="40f26-291">.NET Framework Data Provider – Oracle hello telepített too.NET keretrendszer 2.0-s volt, és nem található a .NET Framework 4.0 hello mappákban.</span><span class="sxs-lookup"><span data-stu-id="40f26-291">hello .NET Framework Data Provider for Oracle was installed too.NET Framework 2.0 and is not found in hello .NET Framework 4.0 folders.</span></span>

<span data-ttu-id="40f26-292">**Megoldás vagy megoldás:**</span><span class="sxs-lookup"><span data-stu-id="40f26-292">**Resolution/Workaround:**</span></span>

1. <span data-ttu-id="40f26-293">Ha nem telepített .NET-szolgáltató az Oracle, hello [telepítse](http://www.oracle.com/technetwork/topics/dotnet/downloads/) , majd próbálja megismételni a hello forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="40f26-293">If you haven't installed hello .NET Provider for Oracle, [install it](http://www.oracle.com/technetwork/topics/dotnet/downloads/) and retry hello scenario.</span></span>
2. <span data-ttu-id="40f26-294">Ha hibaüzenet hello hello szolgáltató telepítése után is, hello lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="40f26-294">If you get hello error message even after installing hello provider, do hello following steps:</span></span>
   1. <span data-ttu-id="40f26-295">Nyissa meg a .NET 2.0 gép config hello mappából: <system disk>: \Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span><span class="sxs-lookup"><span data-stu-id="40f26-295">Open machine config of .NET 2.0 from hello folder: <system disk>:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG\machine.config.</span></span>
   2. <span data-ttu-id="40f26-296">Keresse meg **.NET-keretrendszerhez készült Oracle-adatszolgáltatóban**, és képes toofind bejegyzést kell lennie, ahogy az a következő minta alapján hello **system.data** -> **DbProviderFactories**: "<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description=".NET oracle-adatszolgáltatója</span><span class="sxs-lookup"><span data-stu-id="40f26-296">Search for **Oracle Data Provider for .NET**, and you should be able toofind an entry as shown in hello following sample under **system.data** -> **DbProviderFactories**: “<add name="Oracle Data Provider for .NET" invariant="Oracle.DataAccess.Client" description="Oracle Data Provider for .NET</span></span>" type="Oracle.DataAccess.Client.OracleClientFactory, Oracle.DataAccess, Version=2.112.3.0, Culture=neutral, PublicKeyToken=89b483f429c47342" /><span data-ttu-id="40f26-297">”</span><span class="sxs-lookup"><span data-stu-id="40f26-297">”</span></span>
3. <span data-ttu-id="40f26-298">A bejegyzés toohello machine.config fájl másolása a következő v4.0 mappa hello: <system disk>: \Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config és módosítási hello verzió too4.xxx.x.x.</span><span class="sxs-lookup"><span data-stu-id="40f26-298">Copy this entry toohello machine.config file in hello following v4.0 folder: <system disk>:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config, and change hello version too4.xxx.x.x.</span></span>
4. <span data-ttu-id="40f26-299">"< ODP.NET telepített elérési útja > \11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll" hello globális szerelvény-gyorsítótárban (GAC) futtatásával telepítse `gacutil /i [provider path]`. ## hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="40f26-299">Install “<ODP.NET Installed Path>\11.2.0\client_1\odp.net\bin\4\Oracle.DataAccess.dll” into hello global assembly cache (GAC) by running `gacutil /i [provider path]`.## Troubleshooting tips</span></span>

### <a name="problem-2-datetime-formatting"></a><span data-ttu-id="40f26-300">2. hiba: dátum és idő formázása</span><span class="sxs-lookup"><span data-stu-id="40f26-300">Problem 2: datetime formatting</span></span>

<span data-ttu-id="40f26-301">Lásd a következő hello **hibaüzenet**:</span><span class="sxs-lookup"><span data-stu-id="40f26-301">You see hello following **error message**:</span></span>

    Message=Operation failed in Oracle Database with hello following error: 'ORA-01861: literal does not match format string'.,Source=,''Type=Oracle.DataAccess.Client.OracleException,Message=ORA-01861: literal does not match format string,Source=Oracle Data Provider for .NET,'.

<span data-ttu-id="40f26-302">**Megoldás vagy megoldás:**</span><span class="sxs-lookup"><span data-stu-id="40f26-302">**Resolution/Workaround:**</span></span>

<span data-ttu-id="40f26-303">A másolási tevékenység alapján dátumok hogyan vannak konfigurálva az Oracle-adatbázishoz, ahogy az alábbi hello esetleg tooadjust hello lekérdezési karakterlánc minta (hello to_date függvény használatával):</span><span class="sxs-lookup"><span data-stu-id="40f26-303">You may need tooadjust hello query string in your copy activity based on how dates are configured in your Oracle database, as shown in hello following sample (using hello to_date function):</span></span>

    "oracleReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= to_date(\\'{0:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\')  AND timestampcolumn < to_date(\\'{1:MM-dd-yyyy HH:mm}\\',\\'MM/DD/YYYY HH24:MI\\') ', WindowStart, WindowEnd)"


## <a name="type-mapping-for-oracle"></a><span data-ttu-id="40f26-304">Oracle típusú leképezése</span><span class="sxs-lookup"><span data-stu-id="40f26-304">Type mapping for Oracle</span></span>
<span data-ttu-id="40f26-305">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="40f26-305">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article Copy activity performs automatic type conversions from source types toosink types with hello following 2-step approach:</span></span>

1. <span data-ttu-id="40f26-306">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="40f26-306">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="40f26-307">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="40f26-307">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="40f26-308">Ha adatok áthelyezése az Oracle, leképezéseket a következő hello használ az Oracle típusú too.NET típusánál, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="40f26-308">When moving data from Oracle, hello following mappings are used from Oracle data type too.NET type and vice versa.</span></span>

| <span data-ttu-id="40f26-309">Oracle-adattípusra</span><span class="sxs-lookup"><span data-stu-id="40f26-309">Oracle data type</span></span> | <span data-ttu-id="40f26-310">.NET-keretrendszer adattípus</span><span class="sxs-lookup"><span data-stu-id="40f26-310">.NET Framework data type</span></span> |
| --- | --- |
| <span data-ttu-id="40f26-311">BFÁJL</span><span class="sxs-lookup"><span data-stu-id="40f26-311">BFILE</span></span> |<span data-ttu-id="40f26-312">Byte]</span><span class="sxs-lookup"><span data-stu-id="40f26-312">Byte[]</span></span> |
| <span data-ttu-id="40f26-313">A BLOB</span><span class="sxs-lookup"><span data-stu-id="40f26-313">BLOB</span></span> |<span data-ttu-id="40f26-314">Byte]</span><span class="sxs-lookup"><span data-stu-id="40f26-314">Byte[]</span></span> |
| <span data-ttu-id="40f26-315">KARAKTER</span><span class="sxs-lookup"><span data-stu-id="40f26-315">CHAR</span></span> |<span data-ttu-id="40f26-316">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-316">String</span></span> |
| <span data-ttu-id="40f26-317">CLOB</span><span class="sxs-lookup"><span data-stu-id="40f26-317">CLOB</span></span> |<span data-ttu-id="40f26-318">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-318">String</span></span> |
| <span data-ttu-id="40f26-319">DÁTUM</span><span class="sxs-lookup"><span data-stu-id="40f26-319">DATE</span></span> |<span data-ttu-id="40f26-320">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="40f26-320">DateTime</span></span> |
| <span data-ttu-id="40f26-321">LEBEGŐPONTOS</span><span class="sxs-lookup"><span data-stu-id="40f26-321">FLOAT</span></span> |<span data-ttu-id="40f26-322">Decimális, karakterlánc (Ha pontosság > 28)</span><span class="sxs-lookup"><span data-stu-id="40f26-322">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="40f26-323">EGÉSZ SZÁM</span><span class="sxs-lookup"><span data-stu-id="40f26-323">INTEGER</span></span> |<span data-ttu-id="40f26-324">Decimális, karakterlánc (Ha pontosság > 28)</span><span class="sxs-lookup"><span data-stu-id="40f26-324">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="40f26-325">IDŐKÖZ év tooMONTH</span><span class="sxs-lookup"><span data-stu-id="40f26-325">INTERVAL YEAR tooMONTH</span></span> |<span data-ttu-id="40f26-326">Int32</span><span class="sxs-lookup"><span data-stu-id="40f26-326">Int32</span></span> |
| <span data-ttu-id="40f26-327">IDŐKÖZ nap tooSECOND</span><span class="sxs-lookup"><span data-stu-id="40f26-327">INTERVAL DAY tooSECOND</span></span> |<span data-ttu-id="40f26-328">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="40f26-328">TimeSpan</span></span> |
| <span data-ttu-id="40f26-329">HOSSZÚ</span><span class="sxs-lookup"><span data-stu-id="40f26-329">LONG</span></span> |<span data-ttu-id="40f26-330">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-330">String</span></span> |
| <span data-ttu-id="40f26-331">HOSSZÚ NYERS</span><span class="sxs-lookup"><span data-stu-id="40f26-331">LONG RAW</span></span> |<span data-ttu-id="40f26-332">Byte]</span><span class="sxs-lookup"><span data-stu-id="40f26-332">Byte[]</span></span> |
| <span data-ttu-id="40f26-333">NCHAR</span><span class="sxs-lookup"><span data-stu-id="40f26-333">NCHAR</span></span> |<span data-ttu-id="40f26-334">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-334">String</span></span> |
| <span data-ttu-id="40f26-335">NCLOB</span><span class="sxs-lookup"><span data-stu-id="40f26-335">NCLOB</span></span> |<span data-ttu-id="40f26-336">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-336">String</span></span> |
| <span data-ttu-id="40f26-337">SZÁM</span><span class="sxs-lookup"><span data-stu-id="40f26-337">NUMBER</span></span> |<span data-ttu-id="40f26-338">Decimális, karakterlánc (Ha pontosság > 28)</span><span class="sxs-lookup"><span data-stu-id="40f26-338">Decimal, String (if precision > 28)</span></span> |
| <span data-ttu-id="40f26-339">NVARCHAR2</span><span class="sxs-lookup"><span data-stu-id="40f26-339">NVARCHAR2</span></span> |<span data-ttu-id="40f26-340">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-340">String</span></span> |
| <span data-ttu-id="40f26-341">NYERS</span><span class="sxs-lookup"><span data-stu-id="40f26-341">RAW</span></span> |<span data-ttu-id="40f26-342">Byte]</span><span class="sxs-lookup"><span data-stu-id="40f26-342">Byte[]</span></span> |
| <span data-ttu-id="40f26-343">ROWID</span><span class="sxs-lookup"><span data-stu-id="40f26-343">ROWID</span></span> |<span data-ttu-id="40f26-344">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-344">String</span></span> |
| <span data-ttu-id="40f26-345">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="40f26-345">TIMESTAMP</span></span> |<span data-ttu-id="40f26-346">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="40f26-346">DateTime</span></span> |
| <span data-ttu-id="40f26-347">A HELYI IDŐZÓNÁRA IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="40f26-347">TIMESTAMP WITH LOCAL TIME ZONE</span></span> |<span data-ttu-id="40f26-348">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="40f26-348">DateTime</span></span> |
| <span data-ttu-id="40f26-349">AZ IDŐZÓNA IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="40f26-349">TIMESTAMP WITH TIME ZONE</span></span> |<span data-ttu-id="40f26-350">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="40f26-350">DateTime</span></span> |
| <span data-ttu-id="40f26-351">ELŐJEL NÉLKÜLI EGÉSZKÉNT.</span><span class="sxs-lookup"><span data-stu-id="40f26-351">UNSIGNED INTEGER</span></span> |<span data-ttu-id="40f26-352">Szám</span><span class="sxs-lookup"><span data-stu-id="40f26-352">Number</span></span> |
| <span data-ttu-id="40f26-353">VARCHAR2</span><span class="sxs-lookup"><span data-stu-id="40f26-353">VARCHAR2</span></span> |<span data-ttu-id="40f26-354">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-354">String</span></span> |
| <span data-ttu-id="40f26-355">XML</span><span class="sxs-lookup"><span data-stu-id="40f26-355">XML</span></span> |<span data-ttu-id="40f26-356">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="40f26-356">String</span></span> |

> [!NOTE]
> <span data-ttu-id="40f26-357">Adattípus **IDŐKÖZ év tooMONTH** és **IDŐKÖZ nap tooSECOND** Microsoft illesztőprogram használata esetén nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="40f26-357">Data type **INTERVAL YEAR tooMONTH** and **INTERVAL DAY tooSECOND** are not supported when using Microsoft driver.</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="40f26-358">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="40f26-358">Map source toosink columns</span></span>
<span data-ttu-id="40f26-359">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="40f26-359">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="40f26-360">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="40f26-360">Repeatable read from relational sources</span></span>
<span data-ttu-id="40f26-361">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="40f26-361">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="40f26-362">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="40f26-362">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="40f26-363">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="40f26-363">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="40f26-364">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="40f26-364">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="40f26-365">Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span><span class="sxs-lookup"><span data-stu-id="40f26-365">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="40f26-366">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="40f26-366">Performance and Tuning</span></span>
<span data-ttu-id="40f26-367">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="40f26-367">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
