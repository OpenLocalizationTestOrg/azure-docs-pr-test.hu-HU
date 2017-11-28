---
title: "Adatok áthelyezése az Azure Data Factory használatával SAP Business Warehouse |} Microsoft Docs"
description: "További tudnivalók az SAP Business Warehouse Azure Data Factory használatával áthelyezni az adatokat."
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
ms.openlocfilehash: 220ccc8b94797880d335385046001c5f3b17c862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-sap-business-warehouse-using-azure-data-factory"></a><span data-ttu-id="9d1d5-103">Helyezze át az adatokat az SAP Business Warehouse Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="9d1d5-103">Move data From SAP Business Warehouse using Azure Data Factory</span></span>
<span data-ttu-id="9d1d5-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factory áthelyezni az adatokat a egy helyszíni SAP Business adatraktár (BW).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises SAP Business Warehouse (BW).</span></span> <span data-ttu-id="9d1d5-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="9d1d5-106">Egy helyszíni SAP Business Warehouse adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-106">You can copy data from an on-premises SAP Business Warehouse data store to any supported sink data store.</span></span> <span data-ttu-id="9d1d5-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="9d1d5-108">Adat-előállító jelenleg csak áthelyezése adatait egy SAP Business Warehouse egyéb adattárakhoz, de nem az egyéb adattárakhoz adatok áthelyezése egy SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-108">Data factory currently supports only moving data from an SAP Business Warehouse to other data stores, but not for moving data from other data stores to an SAP Business Warehouse.</span></span> 

## <a name="supported-versions-and-installation"></a><span data-ttu-id="9d1d5-109">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="9d1d5-109">Supported versions and installation</span></span>
<span data-ttu-id="9d1d5-110">Ez az összekötő támogatja az SAP Business Warehouse verzió 7.x.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-110">This connector supports SAP Business Warehouse version 7.x.</span></span> <span data-ttu-id="9d1d5-111">Támogatja az adatok másolását előforduló infocubes-értékeket és QueryCubes (többek között a következőket BEx lekérdezések) segítségével MDX-lekérdezésekben.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-111">It supports copying data from InfoCubes and QueryCubes (including BEx queries) using MDX queries.</span></span>

<span data-ttu-id="9d1d5-112">Ahhoz, hogy a kapcsolat az SAP BW-példányra, a következő összetevők telepítése:</span><span class="sxs-lookup"><span data-stu-id="9d1d5-112">To enable the connectivity to the SAP BW instance, install the following components:</span></span>
- <span data-ttu-id="9d1d5-113">**Az adatkezelési átjáró**: Data Factory támogatja a helyszíni adatokhoz való kapcsolódásról (beleértve az SAP Business Warehouse) az adatkezelési átjáró összetevő használatával nevezik.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-113">**Data Management Gateway**: Data Factory service supports connecting to on-premises data stores (including SAP Business Warehouse) using a component called Data Management Gateway.</span></span> <span data-ttu-id="9d1d5-114">Az adatkezelési átjáró és az átjáró beállításának lépéseit, lásd: [áthelyezése a helyszíni adatok között adattároló adattár felhőbe](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-114">To learn about Data Management Gateway and step-by-step instructions for setting up the gateway, see [Moving data between on-premises data store to cloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="9d1d5-115">Átjáróra szükség, akkor is, ha az SAP Business Warehouse Azure IaaS virtuális gépként (VM) van tárolva.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-115">Gateway is required even if the SAP Business Warehouse is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="9d1d5-116">Az átjárót telepítheti a adattárként azonos virtuális Gépen vagy a másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-116">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>
- <span data-ttu-id="9d1d5-117">**SAP NetWeaver könyvtár** az átjárót működtető gépen.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-117">**SAP NetWeaver library** on the gateway machine.</span></span> <span data-ttu-id="9d1d5-118">A SAP rendszergazdájától, vagy közvetlenül az SAP Netweaver könyvtár kaphat a [SAP szoftverletöltő központból](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-118">You can get the SAP Netweaver library from your SAP administrator, or directly from the [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="9d1d5-119">Keresse meg a **SAP Megjegyzés #1025361** lekérni a legújabb verziót a letöltési helyét.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-119">Search for the **SAP Note #1025361** to get the download location for the most recent version.</span></span> <span data-ttu-id="9d1d5-120">Győződjön meg arról, hogy a SAP NetWeaver könyvtárban (32 bites vagy 64 bites) architektúrája megegyezik-e az átjáró telepítése.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-120">Make sure that the architecture for the SAP NetWeaver library (32-bit or 64-bit) matches your gateway installation.</span></span> <span data-ttu-id="9d1d5-121">Telepítse az SAP NetWeaver RFC SDK az SAP Megjegyzés szereplő összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-121">Then install all files included in the SAP NetWeaver RFC SDK according to the SAP Note.</span></span> <span data-ttu-id="9d1d5-122">Az SAP NetWeaver könyvtár is szerepel a SAP Client Tools telepítését.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-122">The SAP NetWeaver library is also included in the SAP Client Tools installation.</span></span>

> [!TIP]
> <span data-ttu-id="9d1d5-123">Helyezze a NetWeaver RFC SDK kinyert a system32 mappába a dll-fájl.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-123">Put the dlls extracted from the NetWeaver RFC SDK into system32 folder.</span></span>

## <a name="getting-started"></a><span data-ttu-id="9d1d5-124">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9d1d5-124">Getting started</span></span>
<span data-ttu-id="9d1d5-125">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-125">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="9d1d5-126">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-126">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="9d1d5-127">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-127">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="9d1d5-128">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-128">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="9d1d5-129">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-129">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="9d1d5-130">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="9d1d5-130">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="9d1d5-131">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-131">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="9d1d5-132">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-132">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="9d1d5-133">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-133">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="9d1d5-134">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-134">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="9d1d5-135">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-135">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="9d1d5-136">Adatok másolása egy helyszíni SAP Business Warehouse használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az SAP Business Warehouse az Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-136">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises SAP Business Warehouse, see [JSON example: Copy data from SAP Business Warehouse to Azure Blob](#json-example-copy-data-from-sap-business-warehouse-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="9d1d5-137">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory tartozó entitások SAP BW adattárolóihoz JSON-tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="9d1d5-137">The following sections provide details about JSON properties that are used to define Data Factory entities specific to an SAP BW data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="9d1d5-138">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="9d1d5-138">Linked service properties</span></span>
<span data-ttu-id="9d1d5-139">A következő táblázat a JSON-elemek szerepelnek SAP Business Warehouse (BW) kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-139">The following table provides description for JSON elements specific to SAP Business Warehouse (BW) linked service.</span></span>

<span data-ttu-id="9d1d5-140">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9d1d5-140">Property</span></span> | <span data-ttu-id="9d1d5-141">Leírás</span><span class="sxs-lookup"><span data-stu-id="9d1d5-141">Description</span></span> | <span data-ttu-id="9d1d5-142">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9d1d5-142">Allowed values</span></span> | <span data-ttu-id="9d1d5-143">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9d1d5-143">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="9d1d5-144">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="9d1d5-144">server</span></span> | <span data-ttu-id="9d1d5-145">A kiszolgálóra az SAP BW-példány neve.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-145">Name of the server on which the SAP BW instance resides.</span></span> | <span data-ttu-id="9d1d5-146">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-146">string</span></span> | <span data-ttu-id="9d1d5-147">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-147">Yes</span></span>
<span data-ttu-id="9d1d5-148">systemNumber</span><span class="sxs-lookup"><span data-stu-id="9d1d5-148">systemNumber</span></span> | <span data-ttu-id="9d1d5-149">Az SAP BW rendszer rendszer száma.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-149">System number of the SAP BW system.</span></span> | <span data-ttu-id="9d1d5-150">Kétjegyű tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-150">Two-digit decimal number represented as a string.</span></span> | <span data-ttu-id="9d1d5-151">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-151">Yes</span></span>
<span data-ttu-id="9d1d5-152">clientId</span><span class="sxs-lookup"><span data-stu-id="9d1d5-152">clientId</span></span> | <span data-ttu-id="9d1d5-153">Az ügyfél számára a SAP W rendszer ügyfél-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-153">Client ID of the client in the SAP W system.</span></span> | <span data-ttu-id="9d1d5-154">Három számjegyből tizedes tört karakterláncból.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-154">Three-digit decimal number represented as a string.</span></span> | <span data-ttu-id="9d1d5-155">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-155">Yes</span></span>
<span data-ttu-id="9d1d5-156">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="9d1d5-156">username</span></span> | <span data-ttu-id="9d1d5-157">Az SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="9d1d5-157">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="9d1d5-158">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-158">string</span></span> | <span data-ttu-id="9d1d5-159">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-159">Yes</span></span>
<span data-ttu-id="9d1d5-160">jelszó</span><span class="sxs-lookup"><span data-stu-id="9d1d5-160">password</span></span> | <span data-ttu-id="9d1d5-161">A felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-161">Password for the user.</span></span> | <span data-ttu-id="9d1d5-162">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-162">string</span></span> | <span data-ttu-id="9d1d5-163">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-163">Yes</span></span>
<span data-ttu-id="9d1d5-164">gatewayName</span><span class="sxs-lookup"><span data-stu-id="9d1d5-164">gatewayName</span></span> | <span data-ttu-id="9d1d5-165">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia a helyszíni SAP BW-példányra neve.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-165">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP BW instance.</span></span> | <span data-ttu-id="9d1d5-166">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-166">string</span></span> | <span data-ttu-id="9d1d5-167">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-167">Yes</span></span>
<span data-ttu-id="9d1d5-168">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="9d1d5-168">encryptedCredential</span></span> | <span data-ttu-id="9d1d5-169">A titkosított hitelesítő adatokban karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-169">The encrypted credential string.</span></span> | <span data-ttu-id="9d1d5-170">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-170">string</span></span> | <span data-ttu-id="9d1d5-171">Nem</span><span class="sxs-lookup"><span data-stu-id="9d1d5-171">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="9d1d5-172">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="9d1d5-172">Dataset properties</span></span>
<span data-ttu-id="9d1d5-173">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-173">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="9d1d5-174">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-174">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="9d1d5-175">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-175">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="9d1d5-176">Nincsenek az SAP BW adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-176">There are no type-specific properties supported for the SAP BW dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="9d1d5-177">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="9d1d5-177">Copy activity properties</span></span>
<span data-ttu-id="9d1d5-178">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-178">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="9d1d5-179">A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-179">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="9d1d5-180">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-180">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="9d1d5-181">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-181">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="9d1d5-182">Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza az SAP BW), a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="9d1d5-182">When source in copy activity is of type **RelationalSource** (which includes SAP BW), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="9d1d5-183">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="9d1d5-183">Property</span></span> | <span data-ttu-id="9d1d5-184">Leírás</span><span class="sxs-lookup"><span data-stu-id="9d1d5-184">Description</span></span> | <span data-ttu-id="9d1d5-185">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="9d1d5-185">Allowed values</span></span> | <span data-ttu-id="9d1d5-186">Szükséges</span><span class="sxs-lookup"><span data-stu-id="9d1d5-186">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="9d1d5-187">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="9d1d5-187">query</span></span> | <span data-ttu-id="9d1d5-188">Meghatározza az MDX-lekérdezés adatokat olvasni az SAP BW-példány.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-188">Specifies the MDX query to read data from the SAP BW instance.</span></span> | <span data-ttu-id="9d1d5-189">MDX-lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-189">MDX query.</span></span> | <span data-ttu-id="9d1d5-190">Igen</span><span class="sxs-lookup"><span data-stu-id="9d1d5-190">Yes</span></span> |


## <a name="json-example-copy-data-from-sap-business-warehouse-to-azure-blob"></a><span data-ttu-id="9d1d5-191">JSON-példa: adatok másolása az SAP Business Warehouse az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="9d1d5-191">JSON example: Copy data from SAP Business Warehouse to Azure Blob</span></span>
<span data-ttu-id="9d1d5-192">Az alábbi példa minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-192">The following example provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="9d1d5-193">Ez a példa bemutatja, hogyan adatok másolása az Azure Blob Storage egy helyszíni SAP Business Warehouse.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-193">This sample shows how to copy data from an on-premises SAP Business Warehouse to an Azure Blob Storage.</span></span> <span data-ttu-id="9d1d5-194">Azonban az adatok átmásolhatók **közvetlenül** bármely, a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-194">However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="9d1d5-195">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-195">This sample provides JSON snippets.</span></span> <span data-ttu-id="9d1d5-196">Nem tartalmazza az adat-előállítóban létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-196">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="9d1d5-197">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-197">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="9d1d5-198">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="9d1d5-198">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="9d1d5-199">A társított szolgáltatás típusa [SapBw](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-199">A linked service of type [SapBw](#linked-service-properties).</span></span>
2. <span data-ttu-id="9d1d5-200">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-200">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="9d1d5-201">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-201">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="9d1d5-202">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-202">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="9d1d5-203">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-203">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="9d1d5-204">A minta adatokat másolja át egy SAP Business Warehouse-példányból egy Azure blob óránként.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-204">The sample copies data from an SAP Business Warehouse instance to an Azure blob hourly.</span></span> <span data-ttu-id="9d1d5-205">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-205">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="9d1d5-206">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-206">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="9d1d5-207">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-207">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-business-warehouse-linked-service"></a><span data-ttu-id="9d1d5-208">SAP Business Warehouse társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9d1d5-208">SAP Business Warehouse linked service</span></span>
<span data-ttu-id="9d1d5-209">A kapcsolódó szolgáltatás hivatkozások az SAP BW-példányt az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-209">This linked service links your SAP BW instance to the data factory.</span></span> <span data-ttu-id="9d1d5-210">A type tulajdonság beállítása **SapBw**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-210">The type property is set to **SapBw**.</span></span> <span data-ttu-id="9d1d5-211">A typeProperties a témakör az SAP BW-példány-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-211">The typeProperties section provides connection information for the SAP BW instance.</span></span> 

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="9d1d5-212">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9d1d5-212">Azure Storage linked service</span></span>
<span data-ttu-id="9d1d5-213">A kapcsolódó szolgáltatás hivatkozások az Azure Storage-fiókban az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-213">This linked service links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="9d1d5-214">A type tulajdonság beállítása **AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-214">The type property is set to **AzureStorage**.</span></span> <span data-ttu-id="9d1d5-215">A typeProperties a témakör az Azure Storage-fiók kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-215">The typeProperties section provides connection information for the Azure Storage account.</span></span>

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

### <a name="sap-bw-input-dataset"></a><span data-ttu-id="9d1d5-216">SAP BW bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9d1d5-216">SAP BW input dataset</span></span>
<span data-ttu-id="9d1d5-217">Ez az adatkészlet meghatározása az SAP Business Warehouse-adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-217">This dataset defines the SAP Business Warehouse dataset.</span></span> <span data-ttu-id="9d1d5-218">A Data Factory adatkészlet típus beállítása **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-218">You set the type of the Data Factory dataset to **RelationalTable**.</span></span> <span data-ttu-id="9d1d5-219">Jelenleg nincs megadva az SAP BW adatkészlethez típusra vonatkozó tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-219">Currently, you do not specify any type-specific properties for an SAP BW dataset.</span></span> <span data-ttu-id="9d1d5-220">A lekérdezés a másolási tevékenység definícióban határozza meg, milyen adatokat olvasni az SAP BW-példány.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-220">The query in the Copy Activity definition specifies what data to read from the SAP BW instance.</span></span> 

<span data-ttu-id="9d1d5-221">A Data Factory szolgáltatásnak külső tulajdonság beállítása TRUE arról értesíti az, hogy a tábla külső data factoryval való, és nem hozzák adat-előállító tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-221">Setting external property to true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="9d1d5-222">Gyakoriság és időköz tulajdonság határozza meg az ütemezés.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-222">Frequency and interval properties defines the schedule.</span></span> <span data-ttu-id="9d1d5-223">Ebben az esetben az adatok olvasható az SAP BW példányból óránként.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-223">In this case, the data is read from the SAP BW instance hourly.</span></span> 

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



### <a name="azure-blob-output-dataset"></a><span data-ttu-id="9d1d5-224">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="9d1d5-224">Azure Blob output dataset</span></span>
<span data-ttu-id="9d1d5-225">Ez az adatkészlet a kimenetet Azure Blob-adathalmazra határozza meg.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-225">This dataset defines the output Azure Blob dataset.</span></span> <span data-ttu-id="9d1d5-226">A type tulajdonság értéke AzureBlob.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-226">The type property is set to AzureBlob.</span></span> <span data-ttu-id="9d1d5-227">A typeProperties a témakör az SAP BW-példány a másolt adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-227">The typeProperties section provides where the data copied from the SAP BW instance is stored.</span></span> <span data-ttu-id="9d1d5-228">Az adatok írása egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-228">The data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="9d1d5-229">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-229">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="9d1d5-230">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-230">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="9d1d5-231">A másolási tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="9d1d5-231">Pipeline with Copy activity</span></span>
<span data-ttu-id="9d1d5-232">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-232">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="9d1d5-233">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** (az SAP BW forrás) és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-233">In the pipeline JSON definition, the **source** type is set to **RelationalSource** (for SAP BW source) and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="9d1d5-234">A megadott lekérdezést az **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-234">The query specified for the **query** property selects the data in the past hour to copy.</span></span>

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



### <a name="type-mapping-for-sap-bw"></a><span data-ttu-id="9d1d5-235">Az SAP BW Programhoz leképezésének</span><span class="sxs-lookup"><span data-stu-id="9d1d5-235">Type mapping for SAP BW</span></span>
<span data-ttu-id="9d1d5-236">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az eseményforrás-típusnak a következő kétlépéses módszert típusok gyűjtése automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="9d1d5-236">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="9d1d5-237">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="9d1d5-237">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="9d1d5-238">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="9d1d5-238">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="9d1d5-239">Ha adatok áthelyezése az SAP BW Programhoz, a következő megfeleltetéseket használ az SAP BW típusok .NET típusú.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-239">When moving data from SAP BW, the following mappings are used from SAP BW types to .NET types.</span></span>

<span data-ttu-id="9d1d5-240">A ABAP szótár típusú adatok</span><span class="sxs-lookup"><span data-stu-id="9d1d5-240">Data type in the ABAP Dictionary</span></span> | <span data-ttu-id="9d1d5-241">.NET-adattípus</span><span class="sxs-lookup"><span data-stu-id="9d1d5-241">.Net Data Type</span></span>
-------------------------------- | --------------
<span data-ttu-id="9d1d5-242">ACCP</span><span class="sxs-lookup"><span data-stu-id="9d1d5-242">ACCP</span></span> |  <span data-ttu-id="9d1d5-243">int</span><span class="sxs-lookup"><span data-stu-id="9d1d5-243">Int</span></span>
<span data-ttu-id="9d1d5-244">KARAKTER</span><span class="sxs-lookup"><span data-stu-id="9d1d5-244">CHAR</span></span> | <span data-ttu-id="9d1d5-245">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-245">String</span></span>
<span data-ttu-id="9d1d5-246">CLNT</span><span class="sxs-lookup"><span data-stu-id="9d1d5-246">CLNT</span></span> | <span data-ttu-id="9d1d5-247">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-247">String</span></span>
<span data-ttu-id="9d1d5-248">PÉNZNEM</span><span class="sxs-lookup"><span data-stu-id="9d1d5-248">CURR</span></span> | <span data-ttu-id="9d1d5-249">Decimális</span><span class="sxs-lookup"><span data-stu-id="9d1d5-249">Decimal</span></span>
<span data-ttu-id="9d1d5-250">CUKY</span><span class="sxs-lookup"><span data-stu-id="9d1d5-250">CUKY</span></span> | <span data-ttu-id="9d1d5-251">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-251">String</span></span>
<span data-ttu-id="9d1d5-252">DEC</span><span class="sxs-lookup"><span data-stu-id="9d1d5-252">DEC</span></span> | <span data-ttu-id="9d1d5-253">Decimális</span><span class="sxs-lookup"><span data-stu-id="9d1d5-253">Decimal</span></span>
<span data-ttu-id="9d1d5-254">FLTP</span><span class="sxs-lookup"><span data-stu-id="9d1d5-254">FLTP</span></span> | <span data-ttu-id="9d1d5-255">Dupla</span><span class="sxs-lookup"><span data-stu-id="9d1d5-255">Double</span></span>
<span data-ttu-id="9d1d5-256">INT1</span><span class="sxs-lookup"><span data-stu-id="9d1d5-256">INT1</span></span> | <span data-ttu-id="9d1d5-257">Bájt</span><span class="sxs-lookup"><span data-stu-id="9d1d5-257">Byte</span></span>
<span data-ttu-id="9d1d5-258">INT2</span><span class="sxs-lookup"><span data-stu-id="9d1d5-258">INT2</span></span> | <span data-ttu-id="9d1d5-259">Int16</span><span class="sxs-lookup"><span data-stu-id="9d1d5-259">Int16</span></span>
<span data-ttu-id="9d1d5-260">INT4</span><span class="sxs-lookup"><span data-stu-id="9d1d5-260">INT4</span></span> | <span data-ttu-id="9d1d5-261">int</span><span class="sxs-lookup"><span data-stu-id="9d1d5-261">Int</span></span>
<span data-ttu-id="9d1d5-262">LANG</span><span class="sxs-lookup"><span data-stu-id="9d1d5-262">LANG</span></span> | <span data-ttu-id="9d1d5-263">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-263">String</span></span>
<span data-ttu-id="9d1d5-264">LCHR</span><span class="sxs-lookup"><span data-stu-id="9d1d5-264">LCHR</span></span> | <span data-ttu-id="9d1d5-265">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-265">String</span></span>
<span data-ttu-id="9d1d5-266">LRAW</span><span class="sxs-lookup"><span data-stu-id="9d1d5-266">LRAW</span></span> | <span data-ttu-id="9d1d5-267">Byte]</span><span class="sxs-lookup"><span data-stu-id="9d1d5-267">Byte[]</span></span>
<span data-ttu-id="9d1d5-268">PREC</span><span class="sxs-lookup"><span data-stu-id="9d1d5-268">PREC</span></span> | <span data-ttu-id="9d1d5-269">Int16</span><span class="sxs-lookup"><span data-stu-id="9d1d5-269">Int16</span></span>
<span data-ttu-id="9d1d5-270">QUAN</span><span class="sxs-lookup"><span data-stu-id="9d1d5-270">QUAN</span></span> | <span data-ttu-id="9d1d5-271">Decimális</span><span class="sxs-lookup"><span data-stu-id="9d1d5-271">Decimal</span></span>
<span data-ttu-id="9d1d5-272">NYERS</span><span class="sxs-lookup"><span data-stu-id="9d1d5-272">RAW</span></span> | <span data-ttu-id="9d1d5-273">Byte]</span><span class="sxs-lookup"><span data-stu-id="9d1d5-273">Byte[]</span></span>
<span data-ttu-id="9d1d5-274">RAWSTRING</span><span class="sxs-lookup"><span data-stu-id="9d1d5-274">RAWSTRING</span></span> | <span data-ttu-id="9d1d5-275">Byte]</span><span class="sxs-lookup"><span data-stu-id="9d1d5-275">Byte[]</span></span>
<span data-ttu-id="9d1d5-276">KARAKTERLÁNC</span><span class="sxs-lookup"><span data-stu-id="9d1d5-276">STRING</span></span> | <span data-ttu-id="9d1d5-277">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-277">String</span></span>
<span data-ttu-id="9d1d5-278">EGYSÉG</span><span class="sxs-lookup"><span data-stu-id="9d1d5-278">UNIT</span></span> | <span data-ttu-id="9d1d5-279">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-279">String</span></span>
<span data-ttu-id="9d1d5-280">DATS</span><span class="sxs-lookup"><span data-stu-id="9d1d5-280">DATS</span></span> | <span data-ttu-id="9d1d5-281">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-281">String</span></span>
<span data-ttu-id="9d1d5-282">NUMC</span><span class="sxs-lookup"><span data-stu-id="9d1d5-282">NUMC</span></span> | <span data-ttu-id="9d1d5-283">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-283">String</span></span>
<span data-ttu-id="9d1d5-284">TIMS</span><span class="sxs-lookup"><span data-stu-id="9d1d5-284">TIMS</span></span> | <span data-ttu-id="9d1d5-285">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="9d1d5-285">String</span></span>

> [!NOTE]
> <span data-ttu-id="9d1d5-286">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-286">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="map-source-to-sink-columns"></a><span data-ttu-id="9d1d5-287">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="9d1d5-287">Map source to sink columns</span></span>
<span data-ttu-id="9d1d5-288">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="9d1d5-288">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="9d1d5-289">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="9d1d5-289">Repeatable read from relational sources</span></span>
<span data-ttu-id="9d1d5-290">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-290">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="9d1d5-291">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-291">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="9d1d5-292">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-292">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="9d1d5-293">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-293">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="9d1d5-294">Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="9d1d5-294">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="9d1d5-295">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="9d1d5-295">Performance and Tuning</span></span>
<span data-ttu-id="9d1d5-296">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="9d1d5-296">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
