---
title: "Adatok áthelyezése az Azure Data Factory használatával SAP HANA |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával SAP HANA áthelyezni az adatokat."
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
ms.openlocfilehash: 2ab488d82d24999a6231e40cb719715463c51d64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="move-data-from-sap-hana-using-azure-data-factory"></a><span data-ttu-id="2a20f-103">Helyezze át az adatokat az SAP HANA Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="2a20f-103">Move data From SAP HANA using Azure Data Factory</span></span>
<span data-ttu-id="2a20f-104">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factoryben az adatok mozgatása egy helyszíni SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2a20f-104">This article explains how to use the Copy Activity in Azure Data Factory to move data from an on-premises SAP HANA.</span></span> <span data-ttu-id="2a20f-105">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="2a20f-105">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

<span data-ttu-id="2a20f-106">Egy helyszíni SAP HANA-adattároló adatok bármely támogatott fogadó adattárolóhoz másolhatja.</span><span class="sxs-lookup"><span data-stu-id="2a20f-106">You can copy data from an on-premises SAP HANA data store to any supported sink data store.</span></span> <span data-ttu-id="2a20f-107">A másolási tevékenység által támogatott mosdók adattárolókhoz listájáért lásd: a [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="2a20f-107">For a list of data stores supported as sinks by the copy activity, see the [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="2a20f-108">Adat-előállító jelenleg csak áthelyezése adatait egy SAP HANA egyéb adattárakhoz, de nem az egyéb adattárakhoz adatok áthelyezése egy SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2a20f-108">Data factory currently supports only moving data from an SAP HANA to other data stores, but not for moving data from other data stores to an SAP HANA.</span></span>

## <a name="supported-versions-and-installation"></a><span data-ttu-id="2a20f-109">Támogatott verziók és telepítés</span><span class="sxs-lookup"><span data-stu-id="2a20f-109">Supported versions and installation</span></span>
<span data-ttu-id="2a20f-110">Ez az összekötő támogatja az SAP HANA-adatbázisból bármely verzióját.</span><span class="sxs-lookup"><span data-stu-id="2a20f-110">This connector supports any version of SAP HANA database.</span></span> <span data-ttu-id="2a20f-111">Támogatja az adatok másolását a HANA információk modellek (például az elemzési és a számítási nézetek) és a sorhoz/oszlophoz táblák SQL-lekérdezések használatával.</span><span class="sxs-lookup"><span data-stu-id="2a20f-111">It supports copying data from HANA information models (such as Analytic and Calculation views) and Row/Column tables using SQL queries.</span></span>

<span data-ttu-id="2a20f-112">Ahhoz, hogy a kapcsolat az SAP HANA-példányra, a következő összetevők telepítése:</span><span class="sxs-lookup"><span data-stu-id="2a20f-112">To enable the connectivity to the SAP HANA instance, install the following components:</span></span>
- <span data-ttu-id="2a20f-113">**Az adatkezelési átjáró**: Data Factory támogatja a helyszíni adatokhoz való kapcsolódásról (beleértve az SAP HANA) az adatkezelési átjáró összetevő használatával nevezik.</span><span class="sxs-lookup"><span data-stu-id="2a20f-113">**Data Management Gateway**: Data Factory service supports connecting to on-premises data stores (including SAP HANA) using a component called Data Management Gateway.</span></span> <span data-ttu-id="2a20f-114">Az adatkezelési átjáró és az átjáró beállításának lépéseit, lásd: [áthelyezése a helyszíni adatok között adattároló adattár felhőbe](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2a20f-114">To learn about Data Management Gateway and step-by-step instructions for setting up the gateway, see [Moving data between on-premises data store to cloud data store](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span> <span data-ttu-id="2a20f-115">Átjáróra szükség, akkor is, ha az SAP HANA Azure IaaS virtuális gépként (VM) van tárolva.</span><span class="sxs-lookup"><span data-stu-id="2a20f-115">Gateway is required even if the SAP HANA is hosted in an Azure IaaS virtual machine (VM).</span></span> <span data-ttu-id="2a20f-116">Az átjárót telepítheti a adattárként azonos virtuális Gépen vagy a másik virtuális gép mindaddig, amíg az átjáró képes kapcsolódni az adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="2a20f-116">You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.</span></span>
- <span data-ttu-id="2a20f-117">**SAP HANA ODBC-illesztőprogram** az átjárót működtető gépen.</span><span class="sxs-lookup"><span data-stu-id="2a20f-117">**SAP HANA ODBC driver** on the gateway machine.</span></span> <span data-ttu-id="2a20f-118">Az SAP HANA ODBC-illesztő a programot letöltheti a [SAP szoftverletöltő központból](https://support.sap.com/swdc).</span><span class="sxs-lookup"><span data-stu-id="2a20f-118">You can download the SAP HANA ODBC driver from the [SAP Software Download Center](https://support.sap.com/swdc).</span></span> <span data-ttu-id="2a20f-119">Keresés a kulcsszóval **SAP HANA ügyfél Windows**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-119">Search with the keyword **SAP HANA CLIENT for Windows**.</span></span> 

## <a name="getting-started"></a><span data-ttu-id="2a20f-120">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="2a20f-120">Getting started</span></span>
<span data-ttu-id="2a20f-121">A másolási tevékenység, mely az adatok egy helyszíni Cassandra adattároló különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="2a20f-121">You can create a pipeline with a copy activity that moves data from an on-premises Cassandra data store by using different tools/APIs.</span></span> 

- <span data-ttu-id="2a20f-122">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-122">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="2a20f-123">Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) létrehozásával egy folyamatot, az adatok másolása varázsló segítségével gyorsan útmutatást.</span><span class="sxs-lookup"><span data-stu-id="2a20f-123">See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard.</span></span> 
- <span data-ttu-id="2a20f-124">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-124">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="2a20f-125">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="2a20f-125">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span> 

<span data-ttu-id="2a20f-126">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="2a20f-126">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="2a20f-127">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="2a20f-127">Create **linked services** to link input and output data stores to your data factory.</span></span>
2. <span data-ttu-id="2a20f-128">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="2a20f-128">Create **datasets** to represent input and output data for the copy operation.</span></span> 
3. <span data-ttu-id="2a20f-129">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="2a20f-129">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> 

<span data-ttu-id="2a20f-130">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="2a20f-130">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="2a20f-131">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="2a20f-131">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="2a20f-132">Adatok másolása egy helyszíni SAP HANA használt adat-előállító entitások JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az SAP HANA az Azure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="2a20f-132">For a sample with JSON definitions for Data Factory entities that are used to copy data from an on-premises SAP HANA, see [JSON example: Copy data from SAP HANA to Azure Blob](#json-example-copy-data-from-sap-hana-to-azure-blob) section of this article.</span></span> 

<span data-ttu-id="2a20f-133">A következő szakaszok részletesen bemutatják egy SAP HANA-adattároló való adat-előállító tartozó entitások meghatározásához használt JSON tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="2a20f-133">The following sections provide details about JSON properties that are used to define Data Factory entities specific to an SAP HANA data store:</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="2a20f-134">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="2a20f-134">Linked service properties</span></span>
<span data-ttu-id="2a20f-135">A következő táblázat a JSON-elemek szerepelnek SAP HANA kapcsolódó szolgáltatásra vonatkozó leírást.</span><span class="sxs-lookup"><span data-stu-id="2a20f-135">The following table provides description for JSON elements specific to SAP HANA linked service.</span></span>

<span data-ttu-id="2a20f-136">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2a20f-136">Property</span></span> | <span data-ttu-id="2a20f-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="2a20f-137">Description</span></span> | <span data-ttu-id="2a20f-138">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="2a20f-138">Allowed values</span></span> | <span data-ttu-id="2a20f-139">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2a20f-139">Required</span></span>
-------- | ----------- | -------------- | --------
<span data-ttu-id="2a20f-140">kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="2a20f-140">server</span></span> | <span data-ttu-id="2a20f-141">A kiszolgálóra az SAP HANA-példány neve.</span><span class="sxs-lookup"><span data-stu-id="2a20f-141">Name of the server on which the SAP HANA instance resides.</span></span> <span data-ttu-id="2a20f-142">Ha a kiszolgáló egy testreszabott portot használ, adja meg a `server:port`.</span><span class="sxs-lookup"><span data-stu-id="2a20f-142">If your server is using a customized port, specify `server:port`.</span></span> | <span data-ttu-id="2a20f-143">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-143">string</span></span> | <span data-ttu-id="2a20f-144">Igen</span><span class="sxs-lookup"><span data-stu-id="2a20f-144">Yes</span></span>
<span data-ttu-id="2a20f-145">AuthenticationType</span><span class="sxs-lookup"><span data-stu-id="2a20f-145">authenticationType</span></span> | <span data-ttu-id="2a20f-146">Hitelesítés típusa.</span><span class="sxs-lookup"><span data-stu-id="2a20f-146">Type of authentication.</span></span> | <span data-ttu-id="2a20f-147">Karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2a20f-147">string.</span></span> <span data-ttu-id="2a20f-148">"Basic" vagy "Windows"</span><span class="sxs-lookup"><span data-stu-id="2a20f-148">"Basic" or "Windows"</span></span> | <span data-ttu-id="2a20f-149">Igen</span><span class="sxs-lookup"><span data-stu-id="2a20f-149">Yes</span></span> 
<span data-ttu-id="2a20f-150">felhasználónév</span><span class="sxs-lookup"><span data-stu-id="2a20f-150">username</span></span> | <span data-ttu-id="2a20f-151">Az SAP-kiszolgálóhoz hozzáféréssel rendelkező felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="2a20f-151">Name of the user who has access to the SAP server</span></span> | <span data-ttu-id="2a20f-152">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-152">string</span></span> | <span data-ttu-id="2a20f-153">Igen</span><span class="sxs-lookup"><span data-stu-id="2a20f-153">Yes</span></span>
<span data-ttu-id="2a20f-154">jelszó</span><span class="sxs-lookup"><span data-stu-id="2a20f-154">password</span></span> | <span data-ttu-id="2a20f-155">A felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="2a20f-155">Password for the user.</span></span> | <span data-ttu-id="2a20f-156">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-156">string</span></span> | <span data-ttu-id="2a20f-157">Igen</span><span class="sxs-lookup"><span data-stu-id="2a20f-157">Yes</span></span>
<span data-ttu-id="2a20f-158">gatewayName</span><span class="sxs-lookup"><span data-stu-id="2a20f-158">gatewayName</span></span> | <span data-ttu-id="2a20f-159">Az átjáró, amely a Data Factory szolgáltatásnak csatlakoznia való kapcsolódáshoz a helyszíni SAP HANA-példány neve.</span><span class="sxs-lookup"><span data-stu-id="2a20f-159">Name of the gateway that the Data Factory service should use to connect to the on-premises SAP HANA instance.</span></span> | <span data-ttu-id="2a20f-160">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-160">string</span></span> | <span data-ttu-id="2a20f-161">Igen</span><span class="sxs-lookup"><span data-stu-id="2a20f-161">Yes</span></span>
<span data-ttu-id="2a20f-162">encryptedCredential</span><span class="sxs-lookup"><span data-stu-id="2a20f-162">encryptedCredential</span></span> | <span data-ttu-id="2a20f-163">A titkosított hitelesítő adatokban karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2a20f-163">The encrypted credential string.</span></span> | <span data-ttu-id="2a20f-164">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-164">string</span></span> | <span data-ttu-id="2a20f-165">Nem</span><span class="sxs-lookup"><span data-stu-id="2a20f-165">No</span></span>

## <a name="dataset-properties"></a><span data-ttu-id="2a20f-166">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2a20f-166">Dataset properties</span></span>
<span data-ttu-id="2a20f-167">Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2a20f-167">For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="2a20f-168">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="2a20f-168">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="2a20f-169">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt azokról az adattárban adatok helyét.</span><span class="sxs-lookup"><span data-stu-id="2a20f-169">The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store.</span></span> <span data-ttu-id="2a20f-170">Nincsenek az SAP HANA-adatkészlet típusú támogatott típusra vonatkozó tulajdonságok **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-170">There are no type-specific properties supported for the SAP HANA dataset of type **RelationalTable**.</span></span> 


## <a name="copy-activity-properties"></a><span data-ttu-id="2a20f-171">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2a20f-171">Copy activity properties</span></span>
<span data-ttu-id="2a20f-172">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2a20f-172">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="2a20f-173">A rendszer például név, leírás, a bemeneti és kimeneti táblák tulajdonságokat olyan szabályzatok állnak rendelkezésre az összes tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2a20f-173">Properties such as name, description, input and output tables, are policies are available for all types of activities.</span></span>

<span data-ttu-id="2a20f-174">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="2a20f-174">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="2a20f-175">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="2a20f-175">For Copy activity, they vary depending on the types of sources and sinks.</span></span>

<span data-ttu-id="2a20f-176">Ha a másolási tevékenység forrása típusa **RelationalSource** (amely tartalmazza a SAP HANA), a következő tulajdonságok érhetők el typeProperties szakaszában:</span><span class="sxs-lookup"><span data-stu-id="2a20f-176">When source in copy activity is of type **RelationalSource** (which includes SAP HANA), the following properties are available in typeProperties section:</span></span>

| <span data-ttu-id="2a20f-177">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="2a20f-177">Property</span></span> | <span data-ttu-id="2a20f-178">Leírás</span><span class="sxs-lookup"><span data-stu-id="2a20f-178">Description</span></span> | <span data-ttu-id="2a20f-179">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="2a20f-179">Allowed values</span></span> | <span data-ttu-id="2a20f-180">Szükséges</span><span class="sxs-lookup"><span data-stu-id="2a20f-180">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="2a20f-181">lekérdezés</span><span class="sxs-lookup"><span data-stu-id="2a20f-181">query</span></span> | <span data-ttu-id="2a20f-182">Adja meg az adatokat olvasni az SAP HANA-példány az SQL-lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="2a20f-182">Specifies the SQL query to read data from the SAP HANA instance.</span></span> | <span data-ttu-id="2a20f-183">SQL-lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="2a20f-183">SQL query.</span></span> | <span data-ttu-id="2a20f-184">Igen</span><span class="sxs-lookup"><span data-stu-id="2a20f-184">Yes</span></span> |

## <a name="json-example-copy-data-from-sap-hana-to-azure-blob"></a><span data-ttu-id="2a20f-185">JSON-példa: adatok másolása az SAP HANA az Azure-Blobba</span><span class="sxs-lookup"><span data-stu-id="2a20f-185">JSON example: Copy data from SAP HANA to Azure Blob</span></span>
<span data-ttu-id="2a20f-186">Az alábbi minta minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot biztosít [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="2a20f-186">The following sample provides sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="2a20f-187">Ez a példa bemutatja, hogyan adatok másolása az Azure Blob Storage egy helyszíni SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="2a20f-187">This sample shows how to copy data from an on-premises SAP HANA to an Azure Blob Storage.</span></span> <span data-ttu-id="2a20f-188">Azonban az adatok átmásolhatók **közvetlenül** a mosdók egyik felsorolt [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="2a20f-188">However, data can be copied **directly** to any of the sinks listed [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="2a20f-189">Ez a minta JSON kódtöredékek biztosít.</span><span class="sxs-lookup"><span data-stu-id="2a20f-189">This sample provides JSON snippets.</span></span> <span data-ttu-id="2a20f-190">Nem tartalmazza az adat-előállítóban létrehozásának részletes leírása.</span><span class="sxs-lookup"><span data-stu-id="2a20f-190">It does not include step-by-step instructions for creating the data factory.</span></span> <span data-ttu-id="2a20f-191">Lásd: [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk lépéseit.</span><span class="sxs-lookup"><span data-stu-id="2a20f-191">See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions.</span></span>

<span data-ttu-id="2a20f-192">A minta a következő data factory entitások rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2a20f-192">The sample has the following data factory entities:</span></span>

1. <span data-ttu-id="2a20f-193">A társított szolgáltatás típusa [SapHana](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2a20f-193">A linked service of type [SapHana](#linked-service-properties).</span></span>
2. <span data-ttu-id="2a20f-194">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="2a20f-194">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
3. <span data-ttu-id="2a20f-195">Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2a20f-195">An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](#dataset-properties).</span></span>
4. <span data-ttu-id="2a20f-196">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="2a20f-196">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="2a20f-197">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="2a20f-197">A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="2a20f-198">A minta másol adatokat egy SAP HANA-példányát az Azure blob óránként.</span><span class="sxs-lookup"><span data-stu-id="2a20f-198">The sample copies data from an SAP HANA instance to an Azure blob hourly.</span></span> <span data-ttu-id="2a20f-199">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="2a20f-199">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="2a20f-200">Első lépésként a telepítő az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="2a20f-200">As a first step, setup the data management gateway.</span></span> <span data-ttu-id="2a20f-201">Az utasítások szerepelnek a [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2a20f-201">The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article.</span></span>

### <a name="sap-hana-linked-service"></a><span data-ttu-id="2a20f-202">SAP HANA társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2a20f-202">SAP HANA linked service</span></span>
<span data-ttu-id="2a20f-203">A kapcsolódó szolgáltatás hivatkozások SAP HANA-példány az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="2a20f-203">This linked service links your SAP HANA instance to the data factory.</span></span> <span data-ttu-id="2a20f-204">A type tulajdonság beállítása **SapHana**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-204">The type property is set to **SapHana**.</span></span> <span data-ttu-id="2a20f-205">A typeProperties a témakör az SAP HANA-példány-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="2a20f-205">The typeProperties section provides connection information for the SAP HANA instance.</span></span>

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

### <a name="azure-storage-linked-service"></a><span data-ttu-id="2a20f-206">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="2a20f-206">Azure Storage linked service</span></span>
<span data-ttu-id="2a20f-207">A kapcsolódó szolgáltatás hivatkozások az Azure Storage-fiókban az adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="2a20f-207">This linked service links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="2a20f-208">A type tulajdonság beállítása **AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-208">The type property is set to **AzureStorage**.</span></span> <span data-ttu-id="2a20f-209">A typeProperties a témakör az Azure Storage-fiók kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="2a20f-209">The typeProperties section provides connection information for the Azure Storage account.</span></span>

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

### <a name="sap-hana-input-dataset"></a><span data-ttu-id="2a20f-210">SAP HANA-bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="2a20f-210">SAP HANA input dataset</span></span>

<span data-ttu-id="2a20f-211">Ez az adatkészlet meghatározása az SAP HANA-adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="2a20f-211">This dataset defines the SAP HANA dataset.</span></span> <span data-ttu-id="2a20f-212">A Data Factory adatkészlet típus beállítása **RelationalTable**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-212">You set the type of the Data Factory dataset to **RelationalTable**.</span></span> <span data-ttu-id="2a20f-213">Jelenleg nincs megadva egy SAP HANA-adatkészlet típusra vonatkozó tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="2a20f-213">Currently, you do not specify any type-specific properties for an SAP HANA dataset.</span></span> <span data-ttu-id="2a20f-214">A lekérdezés a másolási tevékenység definícióban határozza meg, milyen adatokat olvasni az SAP HANA-példány.</span><span class="sxs-lookup"><span data-stu-id="2a20f-214">The query in the Copy Activity definition specifies what data to read from the SAP HANA instance.</span></span> 

<span data-ttu-id="2a20f-215">A Data Factory szolgáltatásnak külső tulajdonság beállítása TRUE arról értesíti az, hogy a tábla külső data factoryval való, és nem hozzák adat-előállító tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2a20f-215">Setting external property to true informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

<span data-ttu-id="2a20f-216">Gyakoriság és időköz tulajdonság határozza meg az ütemezés.</span><span class="sxs-lookup"><span data-stu-id="2a20f-216">Frequency and interval properties defines the schedule.</span></span> <span data-ttu-id="2a20f-217">Ebben az esetben a adatolvasás a SAP HANA-példányból óránként.</span><span class="sxs-lookup"><span data-stu-id="2a20f-217">In this case, the data is read from the SAP HANA instance hourly.</span></span> 

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

### <a name="azure-blob-output-dataset"></a><span data-ttu-id="2a20f-218">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="2a20f-218">Azure Blob output dataset</span></span>
<span data-ttu-id="2a20f-219">Ez az adatkészlet a kimenetet Azure Blob-adathalmazra határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2a20f-219">This dataset defines the output Azure Blob dataset.</span></span> <span data-ttu-id="2a20f-220">A type tulajdonság értéke AzureBlob.</span><span class="sxs-lookup"><span data-stu-id="2a20f-220">The type property is set to AzureBlob.</span></span> <span data-ttu-id="2a20f-221">A typeProperties a témakör az SAP HANA-példány a másolt adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="2a20f-221">The typeProperties section provides where the data copied from the SAP HANA instance is stored.</span></span> <span data-ttu-id="2a20f-222">Az adatok írása egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="2a20f-222">The data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="2a20f-223">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="2a20f-223">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="2a20f-224">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="2a20f-224">The folder path uses year, month, day, and hours parts of the start time.</span></span>

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


### <a name="pipeline-with-copy-activity"></a><span data-ttu-id="2a20f-225">A másolási tevékenység-feldolgozási folyamat</span><span class="sxs-lookup"><span data-stu-id="2a20f-225">Pipeline with Copy activity</span></span>

<span data-ttu-id="2a20f-226">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="2a20f-226">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="2a20f-227">Az adatcsatorna JSON-definícióból a **forrás** típusúra **RelationalSource** (az SAP HANA-forrás) és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="2a20f-227">In the pipeline JSON definition, the **source** type is set to **RelationalSource** (for SAP HANA source) and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="2a20f-228">A megadott SQL-lekérdezést a **lekérdezés** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="2a20f-228">The SQL query specified for the **query** property selects the data in the past hour to copy.</span></span>

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


### <a name="type-mapping-for-sap-hana"></a><span data-ttu-id="2a20f-229">SAP Hana leképezésének</span><span class="sxs-lookup"><span data-stu-id="2a20f-229">Type mapping for SAP HANA</span></span>
<span data-ttu-id="2a20f-230">Ahogyan az a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység az eseményforrás-típusnak a következő kétlépéses módszert típusok gyűjtése automatikus típuskonverziók hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="2a20f-230">As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:</span></span>

1. <span data-ttu-id="2a20f-231">A natív eseményforrás-típusnak átalakítása .NET-típusa</span><span class="sxs-lookup"><span data-stu-id="2a20f-231">Convert from native source types to .NET type</span></span>
2. <span data-ttu-id="2a20f-232">.NET-típus konvertálása natív a fogadó típusa</span><span class="sxs-lookup"><span data-stu-id="2a20f-232">Convert from .NET type to native sink type</span></span>

<span data-ttu-id="2a20f-233">Adatok áthelyezése SAP HANA, ha .NET típusú a következő megfeleltetéseket használtak SAP HANA-típusok.</span><span class="sxs-lookup"><span data-stu-id="2a20f-233">When moving data from SAP HANA, the following mappings are used from SAP HANA types to .NET types.</span></span>

<span data-ttu-id="2a20f-234">SAP HANA-típus</span><span class="sxs-lookup"><span data-stu-id="2a20f-234">SAP HANA Type</span></span> | <span data-ttu-id="2a20f-235">.NET-alapú típusa</span><span class="sxs-lookup"><span data-stu-id="2a20f-235">.NET Based Type</span></span>
------------- | ---------------
<span data-ttu-id="2a20f-236">TINYINT</span><span class="sxs-lookup"><span data-stu-id="2a20f-236">TINYINT</span></span> | <span data-ttu-id="2a20f-237">Bájt</span><span class="sxs-lookup"><span data-stu-id="2a20f-237">Byte</span></span>
<span data-ttu-id="2a20f-238">SMALLINT</span><span class="sxs-lookup"><span data-stu-id="2a20f-238">SMALLINT</span></span> | <span data-ttu-id="2a20f-239">Int16</span><span class="sxs-lookup"><span data-stu-id="2a20f-239">Int16</span></span>
<span data-ttu-id="2a20f-240">INT</span><span class="sxs-lookup"><span data-stu-id="2a20f-240">INT</span></span> | <span data-ttu-id="2a20f-241">Int32</span><span class="sxs-lookup"><span data-stu-id="2a20f-241">Int32</span></span>
<span data-ttu-id="2a20f-242">BIGINT</span><span class="sxs-lookup"><span data-stu-id="2a20f-242">BIGINT</span></span> | <span data-ttu-id="2a20f-243">Int64</span><span class="sxs-lookup"><span data-stu-id="2a20f-243">Int64</span></span>
<span data-ttu-id="2a20f-244">VALÓS</span><span class="sxs-lookup"><span data-stu-id="2a20f-244">REAL</span></span> | <span data-ttu-id="2a20f-245">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="2a20f-245">Single</span></span>
<span data-ttu-id="2a20f-246">DUPLA</span><span class="sxs-lookup"><span data-stu-id="2a20f-246">DOUBLE</span></span> | <span data-ttu-id="2a20f-247">Egyetlen</span><span class="sxs-lookup"><span data-stu-id="2a20f-247">Single</span></span>
<span data-ttu-id="2a20f-248">DECIMÁLIS</span><span class="sxs-lookup"><span data-stu-id="2a20f-248">DECIMAL</span></span> | <span data-ttu-id="2a20f-249">Decimális</span><span class="sxs-lookup"><span data-stu-id="2a20f-249">Decimal</span></span>
<span data-ttu-id="2a20f-250">LOGIKAI ÉRTÉK</span><span class="sxs-lookup"><span data-stu-id="2a20f-250">BOOLEAN</span></span> | <span data-ttu-id="2a20f-251">Bájt</span><span class="sxs-lookup"><span data-stu-id="2a20f-251">Byte</span></span>
<span data-ttu-id="2a20f-252">VARCHAR</span><span class="sxs-lookup"><span data-stu-id="2a20f-252">VARCHAR</span></span> | <span data-ttu-id="2a20f-253">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-253">String</span></span>
<span data-ttu-id="2a20f-254">NVARCHAR</span><span class="sxs-lookup"><span data-stu-id="2a20f-254">NVARCHAR</span></span> | <span data-ttu-id="2a20f-255">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-255">String</span></span>
<span data-ttu-id="2a20f-256">CLOB</span><span class="sxs-lookup"><span data-stu-id="2a20f-256">CLOB</span></span> | <span data-ttu-id="2a20f-257">Byte]</span><span class="sxs-lookup"><span data-stu-id="2a20f-257">Byte[]</span></span>
<span data-ttu-id="2a20f-258">ALPHANUM</span><span class="sxs-lookup"><span data-stu-id="2a20f-258">ALPHANUM</span></span> | <span data-ttu-id="2a20f-259">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="2a20f-259">String</span></span>
<span data-ttu-id="2a20f-260">A BLOB</span><span class="sxs-lookup"><span data-stu-id="2a20f-260">BLOB</span></span> | <span data-ttu-id="2a20f-261">Byte]</span><span class="sxs-lookup"><span data-stu-id="2a20f-261">Byte[]</span></span>
<span data-ttu-id="2a20f-262">DÁTUM</span><span class="sxs-lookup"><span data-stu-id="2a20f-262">DATE</span></span> | <span data-ttu-id="2a20f-263">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="2a20f-263">DateTime</span></span>
<span data-ttu-id="2a20f-264">IDŐ</span><span class="sxs-lookup"><span data-stu-id="2a20f-264">TIME</span></span> | <span data-ttu-id="2a20f-265">A TimeSpan</span><span class="sxs-lookup"><span data-stu-id="2a20f-265">TimeSpan</span></span>
<span data-ttu-id="2a20f-266">IDŐBÉLYEG</span><span class="sxs-lookup"><span data-stu-id="2a20f-266">TIMESTAMP</span></span> | <span data-ttu-id="2a20f-267">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="2a20f-267">DateTime</span></span>
<span data-ttu-id="2a20f-268">SECONDDATE</span><span class="sxs-lookup"><span data-stu-id="2a20f-268">SECONDDATE</span></span> | <span data-ttu-id="2a20f-269">Dátum és idő</span><span class="sxs-lookup"><span data-stu-id="2a20f-269">DateTime</span></span>

## <a name="known-limitations"></a><span data-ttu-id="2a20f-270">Ismert korlátozásai</span><span class="sxs-lookup"><span data-stu-id="2a20f-270">Known limitations</span></span>
<span data-ttu-id="2a20f-271">Ha az adatok másolása SAP HANA, van néhány ismert korlátozásai:</span><span class="sxs-lookup"><span data-stu-id="2a20f-271">There are a few known limitations when copying data from SAP HANA:</span></span>

- <span data-ttu-id="2a20f-272">NVARCHAR karakterláncok csak legfeljebb 4000 Unicode-karaktereket az</span><span class="sxs-lookup"><span data-stu-id="2a20f-272">NVARCHAR strings are truncated to maximum length of 4000 Unicode characters</span></span>
- <span data-ttu-id="2a20f-273">SMALLDECIMAL nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2a20f-273">SMALLDECIMAL is not supported</span></span>
- <span data-ttu-id="2a20f-274">VARBINARY nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2a20f-274">VARBINARY is not supported</span></span>
- <span data-ttu-id="2a20f-275">Érvényes dátumok csak közötti 1899 – 12/30 és 31-9999-12</span><span class="sxs-lookup"><span data-stu-id="2a20f-275">Valid Dates are between 1899/12/30 and 9999/12/31</span></span>

## <a name="map-source-to-sink-columns"></a><span data-ttu-id="2a20f-276">Térkép forrás oszlopok gyűjtése</span><span class="sxs-lookup"><span data-stu-id="2a20f-276">Map source to sink columns</span></span>
<span data-ttu-id="2a20f-277">A forrás oszlop szerepel a fogadó dataset adatkészlet leképezési oszlopok, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="2a20f-277">To learn about mapping columns in source dataset to columns in sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="repeatable-read-from-relational-sources"></a><span data-ttu-id="2a20f-278">A relációs források ismételhető Olvasás</span><span class="sxs-lookup"><span data-stu-id="2a20f-278">Repeatable read from relational sources</span></span>
<span data-ttu-id="2a20f-279">Ha az adatok másolását a relációs adatokat tárol, ismételhetőség tartsa szem előtt, nem kívánt eredmények elkerülése érdekében.</span><span class="sxs-lookup"><span data-stu-id="2a20f-279">When copying data from relational data stores, keep repeatability in mind to avoid unintended outcomes.</span></span> <span data-ttu-id="2a20f-280">Az Azure Data Factoryben futtathatja a szelet manuálisan.</span><span class="sxs-lookup"><span data-stu-id="2a20f-280">In Azure Data Factory, you can rerun a slice manually.</span></span> <span data-ttu-id="2a20f-281">Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik.</span><span class="sxs-lookup"><span data-stu-id="2a20f-281">You can also configure retry policy for a dataset so that a slice is rerun when a failure occurs.</span></span> <span data-ttu-id="2a20f-282">A szelet akkor fut újra, vagy módon, ha győződjön meg arról, hogy ugyanazokat az adatokat olvasható függetlenül attól, hogy a szelet futtatása hány alkalommal kell.</span><span class="sxs-lookup"><span data-stu-id="2a20f-282">When a slice is rerun in either way, you need to make sure that the same data is read no matter how many times a slice is run.</span></span> <span data-ttu-id="2a20f-283">Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span><span class="sxs-lookup"><span data-stu-id="2a20f-283">See [Repeatable read from relational sources](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="2a20f-284">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="2a20f-284">Performance and Tuning</span></span>
<span data-ttu-id="2a20f-285">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="2a20f-285">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
