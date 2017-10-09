---
title: "Azure Data Factory használatával SAP Business Warehouse aaaMove adatait |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával SAP Business Warehouse toomove adatok."
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
ms.date: 05/16/2017
ms.author: jingwang
ms.openlocfilehash: 85df16f4759a846f578cad301e3cf918179143d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a><span data-ttu-id="9c787-103">Helyezze át az adatokat az SAP Business Warehouse Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="9c787-103">Move data From SAP Business Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="9c787-104">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok egy helyszíni SAP Business adatraktár (BW) a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9c787-104">This article explains how toouse hello Copy Activity in Azure Data Factory toomove data from an on-premises SAP Business Warehouse (BW).</span></span> <span data-ttu-id="9c787-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="9c787-105">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="9c787-106">Egy helyszíni SAP Business Warehouse adatokat tároló támogatott tooany fogadó adatokat tároló adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="9c787-106">You can copy data from an on-premises SAP Business Warehouse data store tooany supported sink data store.</span></span> <span data-ttu-id="9c787-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="9c787-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="9c787-108">Adat-előállító jelenleg csak áthelyezése egy SAP Business Warehouse tooother adatok adatait tárolja, de nem támogatja az adatok áthelyezését más adatok tooan SAP Business Warehouse tárolja.</span><span class="sxs-lookup"><span data-stu-id="9c787-108">Data factory currently supports only moving data from an SAP Business Warehouse tooother data stores, but not for moving data from other data stores tooan SAP Business Warehouse.</span></span> 

## <a name="supported-versions-and-installation"></a><span data-ttu-id="9c787-109">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="9c787-109">Supported versions and installation</span></span>
<span data-ttu-id="9c787-110">Ez az összekötő támogatja az SAP Business Warehouse verzió 7.x.</span><span class="sxs-lookup"><span data-stu-id="9c787-110">This connector supports SAP Business Warehouse version 7.x.</span></span> <span data-ttu-id="9c787-111">Támogatja az adatok másolását előforduló infocubes-értékeket és QueryCubes (többek között a következőket BEx lekérdezések) segítségével MDX-lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="9c787-111">It supports copying data from InfoCubes and QueryCubes (including BEx queries) using MDX queries.</span></span>

<span data-ttu-id="9c787-112">tooenable hello kapcsolat toohello SAP BW-példány, a következő összetevők hello telepítése:</span><span class="sxs-lookup"><span data-stu-id="9c787-112">tooenable hello connectivity toohello SAP BW instance, install hello following components:</span></span>
- <span data-ttu-id="9c787-113">**Az adatkezelési átjáró**: összekötő tooon helyszíni adatok adat-előállító támogatja (beleértve az SAP Business Warehouse) az adatkezelési átjáró összetevő használatával hívása.</span><span class="sxs-lookup"><span data-stu-id="9c787-113">**Data Management Gateway**: Data Factory service supports connecting tooon-premises data stores (including SAP Business Warehouse) using a component called Data Management Gateway.</span></span> <span data-ttu-id="9c787-114">Az adatkezelési átjáró és hello átjáró beállításának lépéseit toolearn lásd: [között a helyszíni adatok áthelyezése az adattároló toocloud adattár](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9c787-114">toolearn about Data Management Gateway and step-by-step instructions for setting up hello gateway, see [Moving data between on-premises data store toocloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="9c787-115">Átjáróra szükség, akkor is, ha az SAP Business Warehouse hello Azure IaaS virtuális gépként (VM) van tárolva.</span><span class="sxs-lookup"><span data-stu-id="9c787-115">Gateway is required even if hello SAP Business Warehouse is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="9c787-116">Hello ugyanazon VM hello adatként tanúsítványtárolójában, vagy egy másik virtuális gép mindaddig hello átjáró kapcsolódhatnak toohello adatbázis hello átjáró telepíthető.</span><span class="sxs-lookup"><span data-stu-id="9c787-116">You can install hello gateway on hello same VM as hello data store or on a different VM as long as hello gateway can connect toohello database.</span></span>
- <span data-ttu-id="9c787-117">**SAP NetWeaver könyvtár** hello-átjáró számítógépén.</span><span class="sxs-lookup"><span data-stu-id="9c787-117">**SAP NetWeaver library** on hello gateway machine.</span></span> <span data-ttu-id="9c787-118">Az SAP rendszergazdájától, vagy közvetlenül a hello kaphat hello SAP Netweaver könyvtár [SAP szoftverletöltő központból](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="9c787-118">You can get hello SAP Netweaver library from your SAP administrator, or directly from hello [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="9c787-119">Keresse meg a hello **SAP Megjegyzés #1025361** tooget hello letöltési helyét hello alkalmazás legújabb verziójára.</span><span class="sxs-lookup"><span data-stu-id="9c787-119">Search for hello **SAP Note #1025361** tooget hello download location for hello most recent version.</span></span> <span data-ttu-id="9c787-120">Győződjön meg arról, hogy hello architektúra hello SAP NetWeaver szalagtár (32 bites vagy 64 bites) megegyezik-e az átjáró telepítése.</span><span class="sxs-lookup"><span data-stu-id="9c787-120">Make sure that hello architecture for hello SAP NetWeaver library (32-bit or 64-bit) matches your gateway installation.</span></span> <span data-ttu-id="9c787-121">Ezután telepítse hello SAP NetWeaver RFC SDK függően toohello SAP Megjegyzés szereplő összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="9c787-121">Then install all files included in hello SAP NetWeaver RFC SDK according toohello SAP Note.</span></span> <span data-ttu-id="9c787-122">hello SAP NetWeaver könyvtár hello SAP Client Tools telepítését is megtalálhatók.</span><span class="sxs-lookup"><span data-stu-id="9c787-122">hello SAP NetWeaver library is also included in hello SAP Client Tools installation.</span></span>

> [!TIP]
> <span data-ttu-id="9c787-123">Hello DLL-ek kibontani a NetWeaver RFC SDK hello a system32 mappába helyezze el.</span><span class="sxs-lookup"><span data-stu-id="9c787-123">Put hello dlls extracted from hello NetWeaver RFC SDK into system32 folder.</span></span>

## <a name="getting-started"></a><span data-ttu-id="9c787-124">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9c787-124">Getting started</span></span>
<span data-ttu-id="9c787-125">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="9c787-125">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="9c787-126">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="9c787-126">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="9c787-127">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="9c787-127">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using hello Copy data wizard.</span></span> 
- <span data-ttu-id="9c787-128">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="9c787-128">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="9c787-129">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="9c787-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span> 

<span data-ttu-id="9c787-130">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="9c787-130">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="9c787-131">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="9c787-131">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="9c787-132">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="9c787-132">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> 
3. <span data-ttu-id="9c787-133">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="9c787-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="9c787-134">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="9c787-134">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="9c787-135">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="9c787-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="9c787-136">Az adat-előállító entitások, amelyek egy helyszíni SAP Business Warehouse használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="9c787-136">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an on-premises SAP Business Warehouse, see [JSON example: Copy data from SAP Business Warehouse tooAzure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="9c787-137">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooan SAP BW adattár részleteit tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="9c787-137">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooan SAP BW data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="9c787-138">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="9c787-138">Linked service properties</span></span>
<span data-ttu-id="9c787-139">a következő táblázat hello biztosít JSON-elemek adott tooSAP Business Warehouse (BW) kapcsolódó szolgáltatás leírását.</span><span class="sxs-lookup"><span data-stu-id="9c787-139">hello following table provides description for JSON elements specific tooSAP Business Warehouse (BW) linked service.</span></span>

<span data-ttu-id="9c787-140">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9c787-140">Property</span></span> | <span data-ttu-id="9c787-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="9c787-141">Description</span></span> | <span data-ttu-id="9c787-142">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9c787-142">Allowed values</span></span> | <span data-ttu-id="9c787-143">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9c787-143">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="9c787-144">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9c787-144">server</span></span> | <span data-ttu-id="9c787-145">Hello kiszolgálóra mely hello SAP BW példány neve.</span><span class="sxs-lookup"><span data-stu-id="9c787-145">Name of hello server on which hello SAP BW instance resides.</span></span> | <span data-ttu-id="9c787-146">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-146">string</span></span> | <span data-ttu-id="9c787-147">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-147">Yes</span></span>
<span data-ttu-id="9c787-148">systemNumber</span><span class="sxs-lookup"><span data-stu-id="9c787-148">systemNumber</span></span> | <span data-ttu-id="9c787-149">Hello SAP BW rendszer rendszer száma.</span><span class="sxs-lookup"><span data-stu-id="9c787-149">System number of hello SAP BW system.</span></span> | <span data-ttu-id="9c787-150">Kétjegyű tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="9c787-150">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="9c787-151">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-151">Yes</span></span>
<span data-ttu-id="9c787-152">clientId</span><span class="sxs-lookup"><span data-stu-id="9c787-152">clientId</span></span> | <span data-ttu-id="9c787-153">Hello ügyfél hello SAP W rendszer ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9c787-153">Client ID of hello client in hello SAP W system.</span></span> | <span data-ttu-id="9c787-154">Három számjegyből tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="9c787-154">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="9c787-155">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-155">Yes</span></span>
<span data-ttu-id="9c787-156">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="9c787-156">username</span></span> | <span data-ttu-id="9c787-157">Hello toohello SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="9c787-157">Name of hello user who has access toohello SAP server</span></span> | <span data-ttu-id="9c787-158">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-158">string</span></span> | <span data-ttu-id="9c787-159">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-159">Yes</span></span>
<span data-ttu-id="9c787-160">jelszó</span><span class="sxs-lookup"><span data-stu-id="9c787-160">password</span></span> | <span data-ttu-id="9c787-161">Hello felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9c787-161">Password for hello user.</span></span> | <span data-ttu-id="9c787-162">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-162">string</span></span> | <span data-ttu-id="9c787-163">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-163">Yes</span></span>
<span data-ttu-id="9c787-164">gatewayName</span><span class="sxs-lookup"><span data-stu-id="9c787-164">gatewayName</span></span> | <span data-ttu-id="9c787-165">Hello átjáró, amely a Data Factory szolgáltatásnak hello neve tooconnect toohello helyszíni SAP BW példányát kell használnia.</span><span class="sxs-lookup"><span data-stu-id="9c787-165">Name of hello gateway that hello Data Factory service should use tooconnect toohello on-premises SAP BW instance.</span></span> | <span data-ttu-id="9c787-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-166">string</span></span> | <span data-ttu-id="9c787-167">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-167">Yes</span></span>
<span data-ttu-id="9c787-168">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="9c787-168">encryptedCredential</span></span> | <span data-ttu-id="9c787-169">hello titkosított hitelesítő adat karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="9c787-169">hello encrypted credential string.</span></span> | <span data-ttu-id="9c787-170">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-170">string</span></span> | <span data-ttu-id="9c787-171">Nem</span><span class="sxs-lookup"><span data-stu-id="9c787-171">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="9c787-172">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="9c787-172">Dataset properties</span></span>
<span data-ttu-id="9c787-173">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9c787-173">For a full list of sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="9c787-174">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="9c787-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="9c787-175">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="9c787-175">hello **typeProperties** section is different for each type of dataset and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="9c787-176">Nincsenek hello SAP BW adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9c787-176">There are no type-specific properties supported for hello SAP BW dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="9c787-177">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="9c787-177">Copy activity properties</span></span>
<span data-ttu-id="9c787-178">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9c787-178">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="9c787-179">A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9c787-179">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="9c787-180">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="9c787-180">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="9c787-181">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="9c787-181">For Copy activity, they vary depending on hello types of sources and sinks.</span></span>

<span data-ttu-id="9c787-182">Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza az SAP BW), typeProperties szakaszában érhetők hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="9c787-182">When source in copy activity is of type **RelationalSource** (which includes SAP BW), hello following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="9c787-183">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9c787-183">Property</span></span> | <span data-ttu-id="9c787-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="9c787-184">Description</span></span> | <span data-ttu-id="9c787-185">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9c787-185">Allowed values</span></span> | <span data-ttu-id="9c787-186">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9c787-186">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9c787-187">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="9c787-187">query</span></span> | <span data-ttu-id="9c787-188">Megadja a hello MDX tooread adatait hello SAP BW-példányból.</span><span class="sxs-lookup"><span data-stu-id="9c787-188">Specifies hello MDX query tooread data from hello SAP BW instance.</span></span> | <span data-ttu-id="9c787-189">MDX-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="9c787-189">MDX query.</span></span> | <span data-ttu-id="9c787-190">Igen</span><span class="sxs-lookup"><span data-stu-id="9c787-190">Yes</span></span> |


## <a name="json-example-copy-data-from-sap-business-warehouse-tooazure-blob"></a><span data-ttu-id="9c787-191">JSON-példa: adatok másolása az SAP Business Warehouse tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="9c787-191">JSON example: Copy data from SAP Business Warehouse tooAzure Blob</span></span>
<span data-ttu-id="9c787-192">hello alábbi példa minta JSON definícióit tartalmazza használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9c787-192">hello following example provides sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="9c787-193">Ez a példa bemutatja, hogyan egy helyszíni SAP Business Warehouse tooan Azure Blob Storage toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="9c787-193">This sample shows how toocopy data from an on-premises SAP Business Warehouse tooan Azure Blob Storage.</span></span> <span data-ttu-id="9c787-194">Azonban az adatok átmásolhatók **közvetlenül** közölt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="9c787-194">However, data can be copied **directly** tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="9c787-195">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="9c787-195">This sample provides JSON snippets.</span></span> <span data-ttu-id="9c787-196">Nem tartalmazza a hello adat-előállító létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="9c787-196">It does not include step-by-step instructions for creating hello data factory.</span></span> <span data-ttu-id="9c787-197">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="9c787-197">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="9c787-198">hello minta a következő data factory entitások hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="9c787-198">hello sample has hello following data factory entities:</span></span>

1. <span data-ttu-id="9c787-199">A társított szolgáltatás típusa [SapBw](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9c787-199">A linked service of type [SapBw](#linked-service-properties).</span></span>
2. <span data-ttu-id="9c787-200">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9c787-200">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="9c787-201">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9c787-201">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="9c787-202">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9c787-202">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="9c787-203">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="9c787-203">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="9c787-204">hello minta másol adatokat az SAP Business Warehouse példány tooan Azure blob óránként.</span><span class="sxs-lookup"><span data-stu-id="9c787-204">hello sample copies data from an SAP Business Warehouse instance tooan Azure blob hourly.</span></span> <span data-ttu-id="9c787-205">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="9c787-205">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="9c787-206">Első lépésként a telepítő hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="9c787-206">As a first step, setup hello data management gateway.</span></span> <span data-ttu-id="9c787-207">hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9c787-207">hello instructions are in hello [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-business-warehouse-linked-service"></a><span data-ttu-id="9c787-208">SAP Business Warehouse társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9c787-208">SAP Business Warehouse linked service</span></span>
<span data-ttu-id="9c787-209">Ez a SAP BW példány toohello data factory kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="9c787-209">This linked service links your SAP BW instance toohello data factory.</span></span> <span data-ttu-id="9c787-210">hello type tulajdonság beállítása túl**SapBw**.</span><span class="sxs-lookup"><span data-stu-id="9c787-210">hello type property is set too**SapBw**.</span></span> <span data-ttu-id="9c787-211">hello typeProperties szakaszból hello SAP BW példány-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="9c787-211">hello typeProperties section provides connection information for hello SAP BW instance.</span></span> 

```json
{
    "name": "SapBwLinkedService",
    "properties":
    {
        "type": "SapBw",
        "typeProperties":
        {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "username": "<SAP user>",
            "password": "<Password for SAP user>",
            "gatewayName": "<gateway name>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="9c787-212">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9c787-212">Azure Storage linked service</span></span>
<span data-ttu-id="9c787-213">Ez az Azure Storage-fiók toohello adat-előállító kapcsolódó szolgáltatás hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="9c787-213">This linked service links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="9c787-214">hello type tulajdonság beállítása túl**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="9c787-214">hello type property is set too**AzureStorage**.</span></span> <span data-ttu-id="9c787-215">hello typeProperties szakaszból hello Azure Storage-fiók kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="9c787-215">hello typeProperties section provides connection information for hello Azure Storage account.</span></span>

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

### <a name="sap-bw-input-dataset"></a><span data-ttu-id="9c787-216">SAP BW bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9c787-216">SAP BW input dataset</span></span>
<span data-ttu-id="9c787-217">Ez az adatkészlet hello SAP Business Warehouse adatkészlet meghatározása.</span><span class="sxs-lookup"><span data-stu-id="9c787-217">This dataset defines hello SAP Business Warehouse dataset.</span></span> <span data-ttu-id="9c787-218">Hello típusú hello adat-előállító adatkészlet túl beállítása**RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9c787-218">You set hello type of hello Data Factory dataset too**RelationalTable**.</span></span> <span data-ttu-id="9c787-219">Jelenleg nincs megadva az SAP BW adatkészlethez típusra vonatkozó tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="9c787-219">Currently, you do not specify any type-specific properties for an SAP BW dataset.</span></span> <span data-ttu-id="9c787-220">a másolási tevékenység definíciójának hello hello lekérdezés határozza meg, milyen adatok tooread hello SAP BW-példányból.</span><span class="sxs-lookup"><span data-stu-id="9c787-220">hello query in hello Copy Activity definition specifies what data tooread from hello SAP BW instance.</span></span> 

<span data-ttu-id="9c787-221">Hello Data Factory szolgáltatásnak beállítása external tulajdonság tootrue arról értesíti az adott hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9c787-221">Setting external property tootrue informs hello Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

<span data-ttu-id="9c787-222">Gyakoriság és időköz tulajdonságok hello ütemezése határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9c787-222">Frequency and interval properties defines hello schedule.</span></span> <span data-ttu-id="9c787-223">Ebben az esetben hello adatolvasás hello SAP BW példányból óránként.</span><span class="sxs-lookup"><span data-stu-id="9c787-223">In this case, hello data is read from hello SAP BW instance hourly.</span></span> 

```json
{
    "name": "SapBwDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "SapBwLinkedService",
        "typeProperties": {},
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```



### <a name="azure-blob-output-dataset"></a><span data-ttu-id="9c787-224">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9c787-224">Azure Blob output dataset</span></span>
<span data-ttu-id="9c787-225">Ez az adatkészlet meghatározása hello kimeneti Azure Blob-adathalmazra.</span><span class="sxs-lookup"><span data-stu-id="9c787-225">This dataset defines hello output Azure Blob dataset.</span></span> <span data-ttu-id="9c787-226">hello type tulajdonság beállítása tooAzureBlob.</span><span class="sxs-lookup"><span data-stu-id="9c787-226">hello type property is set tooAzureBlob.</span></span> <span data-ttu-id="9c787-227">hello typeProperties rész másolta át hello SAP BW-példányból hello adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="9c787-227">hello typeProperties section provides where hello data copied from hello SAP BW instance is stored.</span></span> <span data-ttu-id="9c787-228">hello adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="9c787-228">hello data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="9c787-229">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="9c787-229">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="9c787-230">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="9c787-230">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/sapbw/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="9c787-231">A másolási tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="9c787-231">Pipeline with Copy activity</span></span>
<span data-ttu-id="9c787-232">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="9c787-232">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="9c787-233">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** (az SAP BW forrás) és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="9c787-233">In hello pipeline JSON definition, hello **source** type is set too**RelationalSource** (for SAP BW source) and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="9c787-234">hello megadott hello lekérdezés **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="9c787-234">hello query specified for hello **query** property selects hello data in hello past hour toocopy.</span></span>

```json
{
    "name": "CopySapBwToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "<MDX query for SAP BW>"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "SapBwDataset"
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
                "name": "SapBwToBlob"
            }
        ],
        "start": "2017-03-01T18:00:00Z",
        "end": "2017-03-01T19:00:00Z"
    }
}
```



### <a name="type-mapping-for-sap-bw"></a><span data-ttu-id="9c787-235">Az SAP BW Programhoz leképezésének</span><span class="sxs-lookup"><span data-stu-id="9c787-235">Type mapping for SAP BW</span></span>
<span data-ttu-id="9c787-236">A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:</span><span class="sxs-lookup"><span data-stu-id="9c787-236">As mentioned in hello [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types toosink types with hello following two-step approach:</span></span>

1. <span data-ttu-id="9c787-237">Natív típusok too.NET forrástípus konvertálása</span><span class="sxs-lookup"><span data-stu-id="9c787-237">Convert from native source types too.NET type</span></span>
2. <span data-ttu-id="9c787-238">.NET típusú toonative a fogadó típusa konvertálása</span><span class="sxs-lookup"><span data-stu-id="9c787-238">Convert from .NET type toonative sink type</span></span>

<span data-ttu-id="9c787-239">Adatok áthelyezése az SAP BW Programhoz, amikor a következő hozzárendelések hello SAP BW típusok too.NET típusok használtak.</span><span class="sxs-lookup"><span data-stu-id="9c787-239">When moving data from SAP BW, hello following mappings are used from SAP BW types too.NET types.</span></span>

<span data-ttu-id="9c787-240">Hello ABAP szótár típusú adatok</span><span class="sxs-lookup"><span data-stu-id="9c787-240">Data type in hello ABAP Dictionary</span></span> | <span data-ttu-id="9c787-241">.NET-adattípus</span><span class="sxs-lookup"><span data-stu-id="9c787-241">.Net Data Type</span></span>
-------------------------------- | --------------
<span data-ttu-id="9c787-242">ACCP</span><span class="sxs-lookup"><span data-stu-id="9c787-242">ACCP</span></span> |  <span data-ttu-id="9c787-243">int</span><span class="sxs-lookup"><span data-stu-id="9c787-243">Int</span></span>
<span data-ttu-id="9c787-244">KARAKTER</span><span class="sxs-lookup"><span data-stu-id="9c787-244">CHAR</span></span> | <span data-ttu-id="9c787-245">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-245">String</span></span>
<span data-ttu-id="9c787-246">CLNT</span><span class="sxs-lookup"><span data-stu-id="9c787-246">CLNT</span></span> | <span data-ttu-id="9c787-247">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-247">String</span></span>
<span data-ttu-id="9c787-248">PÉNZNEM</span><span class="sxs-lookup"><span data-stu-id="9c787-248">CURR</span></span> | <span data-ttu-id="9c787-249">Decimális</span><span class="sxs-lookup"><span data-stu-id="9c787-249">Decimal</span></span>
<span data-ttu-id="9c787-250">CUKY</span><span class="sxs-lookup"><span data-stu-id="9c787-250">CUKY</span></span> | <span data-ttu-id="9c787-251">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-251">String</span></span>
<span data-ttu-id="9c787-252">DEC</span><span class="sxs-lookup"><span data-stu-id="9c787-252">DEC</span></span> | <span data-ttu-id="9c787-253">Decimális</span><span class="sxs-lookup"><span data-stu-id="9c787-253">Decimal</span></span>
<span data-ttu-id="9c787-254">FLTP</span><span class="sxs-lookup"><span data-stu-id="9c787-254">FLTP</span></span> | <span data-ttu-id="9c787-255">Dupla</span><span class="sxs-lookup"><span data-stu-id="9c787-255">Double</span></span>
<span data-ttu-id="9c787-256">INT1</span><span class="sxs-lookup"><span data-stu-id="9c787-256">INT1</span></span> | <span data-ttu-id="9c787-257">Bájt</span><span class="sxs-lookup"><span data-stu-id="9c787-257">Byte</span></span>
<span data-ttu-id="9c787-258">INT2</span><span class="sxs-lookup"><span data-stu-id="9c787-258">INT2</span></span> | <span data-ttu-id="9c787-259">Int16</span><span class="sxs-lookup"><span data-stu-id="9c787-259">Int16</span></span>
<span data-ttu-id="9c787-260">INT4</span><span class="sxs-lookup"><span data-stu-id="9c787-260">INT4</span></span> | <span data-ttu-id="9c787-261">int</span><span class="sxs-lookup"><span data-stu-id="9c787-261">Int</span></span>
<span data-ttu-id="9c787-262">LANG</span><span class="sxs-lookup"><span data-stu-id="9c787-262">LANG</span></span> | <span data-ttu-id="9c787-263">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-263">String</span></span>
<span data-ttu-id="9c787-264">LCHR</span><span class="sxs-lookup"><span data-stu-id="9c787-264">LCHR</span></span> | <span data-ttu-id="9c787-265">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-265">String</span></span>
<span data-ttu-id="9c787-266">LRAW</span><span class="sxs-lookup"><span data-stu-id="9c787-266">LRAW</span></span> | <span data-ttu-id="9c787-267">Byte]</span><span class="sxs-lookup"><span data-stu-id="9c787-267">Byte[]</span></span>
<span data-ttu-id="9c787-268">PREC</span><span class="sxs-lookup"><span data-stu-id="9c787-268">PREC</span></span> | <span data-ttu-id="9c787-269">Int16</span><span class="sxs-lookup"><span data-stu-id="9c787-269">Int16</span></span>
<span data-ttu-id="9c787-270">QUAN</span><span class="sxs-lookup"><span data-stu-id="9c787-270">QUAN</span></span> | <span data-ttu-id="9c787-271">Decimális</span><span class="sxs-lookup"><span data-stu-id="9c787-271">Decimal</span></span>
<span data-ttu-id="9c787-272">NYERS</span><span class="sxs-lookup"><span data-stu-id="9c787-272">RAW</span></span> | <span data-ttu-id="9c787-273">Byte]</span><span class="sxs-lookup"><span data-stu-id="9c787-273">Byte[]</span></span>
<span data-ttu-id="9c787-274">RAWSTRING</span><span class="sxs-lookup"><span data-stu-id="9c787-274">RAWSTRING</span></span> | <span data-ttu-id="9c787-275">Byte]</span><span class="sxs-lookup"><span data-stu-id="9c787-275">Byte[]</span></span>
<span data-ttu-id="9c787-276">KARAKTERLÁNC</span><span class="sxs-lookup"><span data-stu-id="9c787-276">STRING</span></span> | <span data-ttu-id="9c787-277">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-277">String</span></span>
<span data-ttu-id="9c787-278">EGYSÉG</span><span class="sxs-lookup"><span data-stu-id="9c787-278">UNIT</span></span> | <span data-ttu-id="9c787-279">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-279">String</span></span>
<span data-ttu-id="9c787-280">DATS</span><span class="sxs-lookup"><span data-stu-id="9c787-280">DATS</span></span> | <span data-ttu-id="9c787-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-281">String</span></span>
<span data-ttu-id="9c787-282">NUMC</span><span class="sxs-lookup"><span data-stu-id="9c787-282">NUMC</span></span> | <span data-ttu-id="9c787-283">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-283">String</span></span>
<span data-ttu-id="9c787-284">TIMS</span><span class="sxs-lookup"><span data-stu-id="9c787-284">TIMS</span></span> | <span data-ttu-id="9c787-285">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9c787-285">String</span></span>

> [!NOTE]
> <span data-ttu-id="9c787-286">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9c787-286">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="map-source-toosink-columns"></a><span data-ttu-id="9c787-287">A forrásoszlopokat toosink leképezése</span><span class="sxs-lookup"><span data-stu-id="9c787-287">Map source toosink columns</span></span>
<span data-ttu-id="9c787-288">toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9c787-288">toolearn about mapping columns in source dataset toocolumns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="9c787-289">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="9c787-289">Repeatable read from relational sources</span></span>
<span data-ttu-id="9c787-290">Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények.</span><span class="sxs-lookup"><span data-stu-id="9c787-290">When copying data from relational data stores, keep repeatability in mind tooavoid unintended outcomes.</span></span> <span data-ttu-id="9c787-291">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="9c787-291">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="9c787-292">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="9c787-292">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="9c787-293">A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása.</span><span class="sxs-lookup"><span data-stu-id="9c787-293">When a slice is rerun in either way, you need toomake sure that hello same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="9c787-294">Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="9c787-294">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="9c787-295">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="9c787-295">Performance and Tuning</span></span>
<span data-ttu-id="9c787-296">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="9c787-296">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
