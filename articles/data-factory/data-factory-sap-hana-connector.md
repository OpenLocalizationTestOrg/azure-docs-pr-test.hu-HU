---
title: "Azure Data Factory használatával SAP HANA aaaMove adatait |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával SAP HANA toomove adatok."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: 5cefe4c8ed01ea4e86e02496b2f8a9083d0b949c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a><span data-ttu-id="9f8f9-103">Helyezze át az adatokat az SAP HANA Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="9f8f9-103">Move data From SAP HANA using Azure Data Factory</span></span>
<span data-ttu-id="9f8f9-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatait egy helyszíni SAP HANA a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises SAP HANA.</span></span> <span data-ttu-id="9f8f9-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="9f8f9-106">Egy helyszíni SAP HANA adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-106">You can copy data from an on-premises SAP HANA data store tooany supported sink data store.</span></span> <span data-ttu-id="9f8f9-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="9f8f9-108">Adat-előállító jelenleg csak áthelyezése egy SAP HANA-tooother adatok adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan SAP HANA tárolja.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-108">Data factory currently supports only moving data from an SAP HANA tooother data stores, but not for moving data from other data stores tooan SAP HANA.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="9f8f9-109">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="9f8f9-109">Supported versions and installation</span></span>
<span data-ttu-id="9f8f9-110">Ez az összekötő támogatja az SAP HANA-adatbázisból bármely verzióját.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-110">This connector supports any version of SAP HANA database.</span></span> <span data-ttu-id="9f8f9-111">Támogatja az adatok másolását a HANA információk modellek (például az elemzési és a számítási nézetek) és a sorhoz/oszlophoz táblák SQL-lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-111">It supports copying data from HANA information models (such as Analytic and Calculation views) and Row/Column tables using SQL queries.</span></span>

<span data-ttu-id="9f8f9-112">tooenable hello kapcsolat toohello SAP HANA-példány, a következő összetevők hello telepítése:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-112">tooenable hello connectivity toohello SAP HANA instance, install hello following components:</span></span>
- <span data-ttu-id="9f8f9-113">**Az adatkezelési átjáró**: összekötő tooon helyszíni adatok adat-előállító támogatja (beleértve az SAP HANA) az adatkezelési átjáró összetevő használatával hívása.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-113">**Data Management Gateway**: Data Factory service supports connecting tooon-premises data stores (including SAP HANA) using a component called Data Management Gateway.</span></span> <span data-ttu-id="9f8f9-114">Az adatkezelési átjáró és hello átjáró beállításának lépéseit toolearn lásd: [között a helyszíni adatok áthelyezése az adattároló toocloud adattár](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-114">toolearn about Data Management Gateway and step-by-step instructions for setting up hello gateway, see [Moving data between on-premises data store toocloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="9f8f9-115">Átjáróra szükség, akkor is, ha az SAP HANA hello Azure IaaS virtuális gépre (VM) tárolódik.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-115">Gateway is required even if hello SAP HANA is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="9f8f9-116">Hello ugyanazon VM hello adatként tanúsítványtárolójában, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-116">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>
- <span data-ttu-id="9f8f9-117">**SAP HANA ODBC-illesztőprogram** hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-117">**SAP HANA ODBC driver** on hello gateway machine.</span></span> <span data-ttu-id="9f8f9-118">Hello SAP HANA ODBC-illesztőprogram letölthető hello [SAP szoftverletöltő központból](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-118">You can download hello SAP HANA ODBC driver from hello [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="9f8f9-119">Hello kulcsszavas keresés **SAP HANA ügyfél Windows**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-119">Search with hello keyword **SAP HANA CLIENT for Windows**.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="9f8f9-120">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9f8f9-120">Getting started</span></span>
<span data-ttu-id="9f8f9-121">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-121">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="9f8f9-122">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-122">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="9f8f9-123">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="9f8f9-124">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-124">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="9f8f9-125">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="9f8f9-126">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-126">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="9f8f9-127">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-127">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="9f8f9-128">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-128">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="9f8f9-129">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="9f8f9-130">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-130">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="9f8f9-131">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="9f8f9-132">Az adat-előállító entitások, amelyek egy helyszíni SAP HANA használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-132">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises SAP HANA, see [JSON example: Copy data from SAP HANA tooAzure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="9f8f9-133">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooan SAP HANA adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-133">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooan SAP HANA data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="9f8f9-134">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="9f8f9-134">Linked service properties</span></span>
<span data-ttu-id="9f8f9-135">a következő táblázat hello biztosít a társított szolgáltatás JSON-elemek adott tooSAP HANA leírását.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-135">hello following table provides description for JSON elements specific tooSAP HANA linked service.</span></span>

<span data-ttu-id="9f8f9-136">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9f8f9-136">Property</span></span> | <span data-ttu-id="9f8f9-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="9f8f9-137">Description</span></span> | <span data-ttu-id="9f8f9-138">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9f8f9-138">Allowed values</span></span> | <span data-ttu-id="9f8f9-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9f8f9-139">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="9f8f9-140">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9f8f9-140">server</span></span> | <span data-ttu-id="9f8f9-141">Hello kiszolgálóra mely hello SAP HANA példány neve.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-141">Name of hello server on which hello SAP HANA instance resides.</span></span> <span data-ttu-id="9f8f9-142">Ha a kiszolgáló egy testreszabott portot használ, adja meg a `server:port`.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-142">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="9f8f9-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-143">string</span></span> | <span data-ttu-id="9f8f9-144">Igen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-144">Yes</span></span>
<span data-ttu-id="9f8f9-145">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="9f8f9-145">authenticationType</span></span> | <span data-ttu-id="9f8f9-146">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-146">Type of authentication.</span></span> | <span data-ttu-id="9f8f9-147">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-147">string.</span></span> <span data-ttu-id="9f8f9-148">"Basic" vagy "Windows"</span><span class="sxs-lookup"><span data-stu-id="9f8f9-148">"Basic" or "Windows"</span></span> | <span data-ttu-id="9f8f9-149">Igen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-149">Yes</span></span> 
<span data-ttu-id="9f8f9-150">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="9f8f9-150">username</span></span> | <span data-ttu-id="9f8f9-151">Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="9f8f9-151">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="9f8f9-152">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-152">string</span></span> | <span data-ttu-id="9f8f9-153">Igen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-153">Yes</span></span>
<span data-ttu-id="9f8f9-154">jelszó</span><span class="sxs-lookup"><span data-stu-id="9f8f9-154">password</span></span> | <span data-ttu-id="9f8f9-155">Hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-155">Password for hello user.</span></span> | <span data-ttu-id="9f8f9-156">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-156">string</span></span> | <span data-ttu-id="9f8f9-157">Igen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-157">Yes</span></span>
<span data-ttu-id="9f8f9-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="9f8f9-158">gatewayName</span></span> | <span data-ttu-id="9f8f9-159">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP HANA-példányt kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-159">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP HANA instance.</span></span> | <span data-ttu-id="9f8f9-160">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-160">string</span></span> | <span data-ttu-id="9f8f9-161">Igen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-161">Yes</span></span>
<span data-ttu-id="9f8f9-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="9f8f9-162">encryptedCredential</span></span> | <span data-ttu-id="9f8f9-163">hello titkosított hitelesítő adat karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-163">hello encrypted credential string.</span></span> | <span data-ttu-id="9f8f9-164">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-164">string</span></span> | <span data-ttu-id="9f8f9-165">Nem</span><span class="sxs-lookup"><span data-stu-id="9f8f9-165">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="9f8f9-166">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="9f8f9-166">Dataset properties</span></span>
<span data-ttu-id="9f8f9-167">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-167">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="9f8f9-168">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-168">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="9f8f9-169">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-169">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="9f8f9-170">Nincsenek hello SAP HANA-adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-170">There are no type-specific properties supported for hello SAP HANA dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="9f8f9-171">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="9f8f9-171">Copy activity properties</span></span>
<span data-ttu-id="9f8f9-172">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-172">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="9f8f9-173">A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-173">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="9f8f9-174">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-174">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="9f8f9-175">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-175">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="9f8f9-176">Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza a SAP HANA), a következő tulajdonságok hello typeProperties szakaszában érhetők:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-176">When source in copy activity is of type **RelationalSource** (which includes SAP HANA), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="9f8f9-177">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9f8f9-177">Property</span></span> | <span data-ttu-id="9f8f9-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="9f8f9-178">Description</span></span> | <span data-ttu-id="9f8f9-179">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9f8f9-179">Allowed values</span></span> | <span data-ttu-id="9f8f9-180">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9f8f9-180">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9f8f9-181">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="9f8f9-181">query</span></span> | <span data-ttu-id="9f8f9-182">Megadja a hello SQL lekérdezés tooread adatait hello SAP HANA-példányból.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-182">Specifies hello SQL query tooread data from hello SAP HANA instance.</span></span> | <span data-ttu-id="9f8f9-183">SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-183">SQL query.</span></span> | <span data-ttu-id="9f8f9-184">Igen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-184">Yes</span></span> |

## <a name="json-example-copy-data-from-sap-hana-tooazure-blob"></a><span data-ttu-id="9f8f9-185">JSON-példa: adatok másolása az SAP HANA tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="9f8f9-185">JSON example: Copy data from SAP HANA tooAzure Blob</span></span>
<span data-ttu-id="9f8f9-186">hello következő minta nyújt minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-186">hello following sample provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="9f8f9-187">Ez a példa bemutatja, hogyan egy helyszíni SAP HANA tooan Azure Blob Storage toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-187">This sample shows how toocopy data from an on-premises SAP HANA tooan Azure Blob Storage.</span></span> <span data-ttu-id="9f8f9-188">Azonban az adatok átmásolhatók **közvetlenül** felsorolt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-188">However, data can be copied **directly** tooany of hello sinks listed [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="9f8f9-189">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-189">This sample provides JSON snippets.</span></span> <span data-ttu-id="9f8f9-190">Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-190">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="9f8f9-191">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-191">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="9f8f9-192">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-192">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="9f8f9-193">A társított szolgáltatás típusa [SapHana](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-193">A linked service of type [SapHana](#linked-service-properties).</span></span>
2. <span data-ttu-id="9f8f9-194">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-194">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="9f8f9-195">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="9f8f9-196">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="9f8f9-197">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-197">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="9f8f9-198">hello minta másol adatokat egy SAP HANA-példány tooan Azure blob óránként.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-198">hello sample copies data from an SAP HANA instance tooan Azure blob hourly.</span></span> <span data-ttu-id="9f8f9-199">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-199">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="9f8f9-200">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-200">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="9f8f9-201">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-201">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-hana-linked-service"></a><span data-ttu-id="9f8f9-202">SAP HANA társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9f8f9-202">SAP HANA linked service</span></span>
<span data-ttu-id="9f8f9-203">Ez a SAP HANA-példány toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-203">This linked service links your SAP HANA instance toohello data factory.</span></span> <span data-ttu-id="9f8f9-204">hello type tulajdonság beállítása túl**SapHana**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-204">hello type property is set too**SapHana**.</span></span> <span data-ttu-id="9f8f9-205">hello typeProperties szakaszból hello SAP HANA-példány-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-205">hello typeProperties section provides connection information for hello SAP HANA instance.</span></span>

```json
{
    "name": "SapHanaLinkedService",
    "properties":
    {
        "type": "SapHana",
        "typeProperties":
        {
            "server": "<server name>",
            "authenticationType": "<Basic, or Windows>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}

```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="9f8f9-206">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9f8f9-206">Azure Storage linked service</span></span>
<span data-ttu-id="9f8f9-207">Ez az Azure Storage-fiók toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-207">This linked service links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="9f8f9-208">hello type tulajdonság beállítása túl**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-208">hello type property is set too**AzureStorage**.</span></span> <span data-ttu-id="9f8f9-209">hello typeProperties szakaszból hello Azure Storage-fiók kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-209">hello typeProperties section provides connection information for hello Azure Storage account.</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="sap-hana-input-dataset"></a><span data-ttu-id="9f8f9-210">SAP HANA-bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9f8f9-210">SAP HANA input dataset</span></span>

<span data-ttu-id="9f8f9-211">Ez az adatkészlet meghatározása hello SAP HANA-adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-211">This dataset defines hello SAP HANA dataset.</span></span> <span data-ttu-id="9f8f9-212">Hello típusú hello adat-előállító adatkészlet túl beállítása**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-212">You set hello type of hello Data Factory dataset too**RelationalTable**.</span></span> <span data-ttu-id="9f8f9-213">Jelenleg nincs megadva egy SAP HANA-adatkészlet típusra vonatkozó tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-213">Currently, you do not specify any type-specific properties for an SAP HANA dataset.</span></span> <span data-ttu-id="9f8f9-214">a másolási tevékenység definíciójának hello hello lekérdezés határozza meg, hogy milyen adatok tooread hello SAP HANA-példányból.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-214">hello query in hello Copy Activity definition specifies what data tooread from hello SAP HANA instance.</span></span> 

<span data-ttu-id="9f8f9-215">Hello Data Factory szolgáltatásnak beállítása external tulajdonság tootrue arról értesíti az adott hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-215">Setting external property tootrue informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="9f8f9-216">Gyakoriság és időköz tulajdonságok hello ütemezése határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-216">Frequency and interval properties defines hello schedule.</span></span> <span data-ttu-id="9f8f9-217">Ebben az esetben hello adatolvasás hello SAP HANA-példányból óránként.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-217">In this case, hello data is read from hello SAP HANA instance hourly.</span></span> 

```json
{
    "name": "SapHanaDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapHanaLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="9f8f9-218">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9f8f9-218">Azure Blob output dataset</span></span>
<span data-ttu-id="9f8f9-219">Ez az adatkészlet meghatározása hello kimeneti Azure Blob-adathalmazra.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-219">This dataset defines hello output Azure Blob dataset.</span></span> <span data-ttu-id="9f8f9-220">hello type tulajdonság beállítása tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-220">hello type property is set tooAzureBlob.</span></span> <span data-ttu-id="9f8f9-221">hello typeProperties rész másolta át hello SAP HANA-példányból hello adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-221">hello typeProperties section provides where hello data copied from hello SAP HANA instance is stored.</span></span> <span data-ttu-id="9f8f9-222">hello adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-222">hello data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="9f8f9-223">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-223">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="9f8f9-224">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-224">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/saphana/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="9f8f9-225">A másolási tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="9f8f9-225">Pipeline with Copy activity</span></span>

<span data-ttu-id="9f8f9-226">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-226">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="9f8f9-227">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** (az SAP HANA-forrás) és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-227">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** (for SAP HANA source) and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="9f8f9-228">hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-228">hello SQL query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopySapHanaToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<SQL Query for HANA>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapHanaDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SapHanaToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```


### <a name="type-mapping-for-sap-hana"></a><span data-ttu-id="9f8f9-229">SAP Hana leképezésének</span><span class="sxs-lookup"><span data-stu-id="9f8f9-229">Type mapping for SAP HANA</span></span>
<span data-ttu-id="9f8f9-230">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-230">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="9f8f9-231">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="9f8f9-231">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="9f8f9-232">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="9f8f9-232">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="9f8f9-233">Adatok áthelyezése SAP HANA, amikor a következő hozzárendelések hello SAP HANA-típusok too.NET típusok használtak.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-233">When moving data from SAP HANA, hello following mappings are used from SAP HANA types too.NET types.</span></span>

<span data-ttu-id="9f8f9-234">SAP HANA-típus</span><span class="sxs-lookup"><span data-stu-id="9f8f9-234">SAP HANA Type</span></span> | <span data-ttu-id="9f8f9-235">.NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="9f8f9-235">.NET Based Type</span></span>
------------- | ---------------
<span data-ttu-id="9f8f9-236">TINYINT</span><span class="sxs-lookup"><span data-stu-id="9f8f9-236">TINYINT</span></span> | <span data-ttu-id="9f8f9-237">Bájt</span><span class="sxs-lookup"><span data-stu-id="9f8f9-237">Byte</span></span>
<span data-ttu-id="9f8f9-238">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="9f8f9-238">SMALLINT</span></span> | <span data-ttu-id="9f8f9-239">Int16</span><span class="sxs-lookup"><span data-stu-id="9f8f9-239">Int16</span></span>
<span data-ttu-id="9f8f9-240">INT</span><span class="sxs-lookup"><span data-stu-id="9f8f9-240">INT</span></span> | <span data-ttu-id="9f8f9-241">Int32</span><span class="sxs-lookup"><span data-stu-id="9f8f9-241">Int32</span></span>
<span data-ttu-id="9f8f9-242">BIGINT</span><span class="sxs-lookup"><span data-stu-id="9f8f9-242">BIGINT</span></span> | <span data-ttu-id="9f8f9-243">Int64</span><span class="sxs-lookup"><span data-stu-id="9f8f9-243">Int64</span></span>
<span data-ttu-id="9f8f9-244">VALÓS</span><span class="sxs-lookup"><span data-stu-id="9f8f9-244">REAL</span></span> | <span data-ttu-id="9f8f9-245">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-245">Single</span></span>
<span data-ttu-id="9f8f9-246">DUPLA</span><span class="sxs-lookup"><span data-stu-id="9f8f9-246">DOUBLE</span></span> | <span data-ttu-id="9f8f9-247">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="9f8f9-247">Single</span></span>
<span data-ttu-id="9f8f9-248">DECIMÁLIS</span><span class="sxs-lookup"><span data-stu-id="9f8f9-248">DECIMAL</span></span> | <span data-ttu-id="9f8f9-249">Decimális</span><span class="sxs-lookup"><span data-stu-id="9f8f9-249">Decimal</span></span>
<span data-ttu-id="9f8f9-250">LOGIKAI ÉRTÉK</span><span class="sxs-lookup"><span data-stu-id="9f8f9-250">BOOLEAN</span></span> | <span data-ttu-id="9f8f9-251">Bájt</span><span class="sxs-lookup"><span data-stu-id="9f8f9-251">Byte</span></span>
<span data-ttu-id="9f8f9-252">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="9f8f9-252">VARCHAR</span></span> | <span data-ttu-id="9f8f9-253">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-253">String</span></span>
<span data-ttu-id="9f8f9-254">NVARCHAR</span><span class="sxs-lookup"><span data-stu-id="9f8f9-254">NVARCHAR</span></span> | <span data-ttu-id="9f8f9-255">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-255">String</span></span>
<span data-ttu-id="9f8f9-256">CLOB</span><span class="sxs-lookup"><span data-stu-id="9f8f9-256">CLOB</span></span> | <span data-ttu-id="9f8f9-257">Byte]</span><span class="sxs-lookup"><span data-stu-id="9f8f9-257">Byte[]</span></span>
<span data-ttu-id="9f8f9-258">ALPHANUM</span><span class="sxs-lookup"><span data-stu-id="9f8f9-258">ALPHANUM</span></span> | <span data-ttu-id="9f8f9-259">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9f8f9-259">String</span></span>
<span data-ttu-id="9f8f9-260">A BLOB</span><span class="sxs-lookup"><span data-stu-id="9f8f9-260">BLOB</span></span> | <span data-ttu-id="9f8f9-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="9f8f9-261">Byte[]</span></span>
<span data-ttu-id="9f8f9-262">DÁTUM</span><span class="sxs-lookup"><span data-stu-id="9f8f9-262">DATE</span></span> | <span data-ttu-id="9f8f9-263">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="9f8f9-263">DateTime</span></span>
<span data-ttu-id="9f8f9-264">IDŐ</span><span class="sxs-lookup"><span data-stu-id="9f8f9-264">TIME</span></span> | <span data-ttu-id="9f8f9-265">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="9f8f9-265">TimeSpan</span></span>
<span data-ttu-id="9f8f9-266">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="9f8f9-266">TIMESTAMP</span></span> | <span data-ttu-id="9f8f9-267">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="9f8f9-267">DateTime</span></span>
<span data-ttu-id="9f8f9-268">SECONDDATE</span><span class="sxs-lookup"><span data-stu-id="9f8f9-268">SECONDDATE</span></span> | <span data-ttu-id="9f8f9-269">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="9f8f9-269">DateTime</span></span>

## <a name="known-limitations"></a><span data-ttu-id="9f8f9-270">Ismert korlátozásai</span><span class="sxs-lookup"><span data-stu-id="9f8f9-270">Known limitations</span></span>
<span data-ttu-id="9f8f9-271">Ha az adatok másolása SAP HANA, van néhány ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="9f8f9-271">There are a few known limitations when copying data from SAP HANA:</span></span>

- <span data-ttu-id="9f8f9-272">NVARCHAR értékek a következők: 4000 Unicode-karaktereket csonkolt toomaximum hossza</span><span class="sxs-lookup"><span data-stu-id="9f8f9-272">NVARCHAR strings are truncated toomaximum length of 4000 Unicode characters</span></span>
- <span data-ttu-id="9f8f9-273">SMALLDECIMAL nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-273">SMALLDECIMAL is not supported</span></span>
- <span data-ttu-id="9f8f9-274">VARBINARY nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-274">VARBINARY is not supported</span></span>
- <span data-ttu-id="9f8f9-275">Érvényes dátumok csak közötti 1899 – 12/30 és 31-9999-12</span><span class="sxs-lookup"><span data-stu-id="9f8f9-275">Valid Dates are between 1899/12/30 and 9999/12/31</span></span>

## <a name="map-source-toosink-columns"></a><span data-ttu-id="9f8f9-276">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="9f8f9-276">Map source toosink columns</span></span>
<span data-ttu-id="9f8f9-277">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9f8f9-277">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="9f8f9-278">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="9f8f9-278">Repeatable read from relational sources</span></span>
<span data-ttu-id="9f8f9-279">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-279">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="9f8f9-280">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-280">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="9f8f9-281">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-281">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="9f8f9-282">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-282">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="9f8f9-283">Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="9f8f9-283">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="9f8f9-284">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="9f8f9-284">Performance and Tuning</span></span>
<span data-ttu-id="9f8f9-285">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="9f8f9-285">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
